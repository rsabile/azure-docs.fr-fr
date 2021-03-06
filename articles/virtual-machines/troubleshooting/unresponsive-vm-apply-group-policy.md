---
title: La machine virtuelle Azure ne répond pas lors de l’application de la stratégie
description: Cet article décrit les étapes à suivre pour résoudre les problèmes de blocage de l’écran de chargement lors de l’application d’une stratégie pendant le démarrage sur une machine virtuelle Azure.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b5628
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.author: v-mibufo
ms.openlocfilehash: 7160ec9564ede21eab0a205b2d66a7d566639506
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632654"
---
# <a name="vm-is-unresponsive-when-applying-group-policy-local-users-and-groups-policy"></a>La machine virtuelle ne répond pas quand une stratégie Utilisateurs locaux et groupes de stratégie de groupe est appliquée

Cet article décrit les étapes à suivre pour résoudre les problèmes de blocage de l’écran de chargement lorsqu’une machine virtuelle (MV) Azure applique une stratégie pendant le démarrage.

## <a name="symptoms"></a>Symptômes

Quand vous utilisez [Diagnostics de démarrage](./boot-diagnostics.md) pour visualiser une capture d’écran de la machine virtuelle, l’écran est bloqué et affiche le message : « Application de la stratégie Utilisateurs locaux et groupes de stratégie de groupe ».

:::image type="content" source="media//unresponsive-vm-apply-group-policy/applying-group-policy-1.png" alt-text="Capture d’écran du chargement de l’application d’une stratégie Utilisateurs locaux et groupes de stratégie de groupe (Windows Server 2012 R2).":::

:::image type="content" source="media/unresponsive-vm-apply-group-policy/applying-group-policy-2.png" alt-text="Capture d’écran du chargement de l’application d’une stratégie Utilisateurs locaux et groupes de stratégie de groupe (Windows Server 2012).":::

## <a name="cause"></a>Cause

Des verrous sont en conflit lorsque la stratégie tente de nettoyer les anciens profils utilisateur.

> [!NOTE]
> Cela concerne uniquement Windows Server 2012 et Windows Server 2012 R2.

Voici la stratégie problématique :

`Computer Configuration\Policies\Administrative Templates\System/User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="resolution"></a>Résolution

### <a name="process-overview"></a>Vue d’ensemble du processus

> [!TIP]
> Si vous disposez d’une sauvegarde récente de la machine virtuelle, vous pouvez essayer de [restaurer la machine virtuelle à partir de la sauvegarde](../../backup/backup-azure-arm-restore-vms.md) pour résoudre le problème de démarrage.

1. [Créer une machine virtuelle de réparation et y accéder](#step-1-create-and-access-a-repair-vm)
1. [Désactiver la stratégie](#step-2-disable-the-policy)
1. [Activer la console série et la collecte de l’image mémoire](#step-3-enable-serial-console-and-memory-dump-collection)
1. [Régénérer la machine virtuelle](#step-4-rebuild-the-vm)

> [!NOTE]
> Si vous rencontrez cette erreur de démarrage, le système d’exploitation invité n’est pas opérationnel. Vous devez résoudre ce problème en mode hors connexion.

### <a name="step-1-create-and-access-a-repair-vm"></a>Étape 1 : Créer une machine virtuelle de réparation et y accéder

1. Suivez les [étapes 1 à 3 des commandes de réparation de machine virtuelle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) pour préparer une machine virtuelle de réparation.
2. Connectez-vous à la machine virtuelle de réparation à l’aide de la connexion Bureau à distance.

### <a name="step-2-disable-the-policy"></a>Étape 2 : Désactiver la stratégie

1. Sur la machine virtuelle de réparation, ouvrir l’éditeur du Registre.
1. Localiser la clé **HKEY_LOCAL_MACHINE**, puis sélectionner **Fichier** > **Charger Hive** dans le menu.

    :::image type="content" source="media/unresponsive-vm-apply-group-policy/registry.png" alt-text="La capture d’écran montre en surbrillance HKEY_LOCAL_MACHINE et le menu qui contient l’option « Charger Hive ».":::

    - Vous pouvez utiliser « Charger Hive » pour charger des clés de registre à partir d’un système hors connexion. Dans ce cas, le système est le disque défectueux attaché à la machine virtuelle de réparation.
    - Les paramètres à l’ensemble du système sont stockés sur `HKEY_LOCAL_MACHINE` et peuvent être abrégés sous la forme « HKLM ».
1. Dans le disque attaché, accédez au fichier `\windows\system32\config\SOFTWARE` et ouvrez-le.

    1. Lorsque vous êtes invité à saisir un nom, saisissez BROKENSOFTWARE.
    1. Pour vérifier que BROKENSOFTWARE a été chargé, développez **HKEY_LOCAL_MACHINE** et recherchez la clé BROKENSOFTWARE ajoutée.
1. Accédez à BROKENSOFTWARE et vérifiez si la clé CleanupProfile existe dans la ruche chargée.

    1. Si la clé existe, la stratégie CleanupProfile est définie. Sa valeur représente la stratégie de rétention mesurée en jours. Poursuivez la suppression de la clé.
    1. Si la clé n’existe pas, la stratégie CleanupProfile n’est pas définie. [Soumettez un ticket de support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), incluant le fichier memory.dmp situé dans le répertoire Windows du disque du système d’exploitation attaché.

1. Supprimez la clé CleanupProfiles à l’aide de cette commande :

    ```
    reg delete "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows\System" /v CleanupProfiles /f
    ```
1.  Déchargez la ruche BROKENSOFTWARE à l’aide de cette commande :

    ```
    reg unload HKLM\BROKENSOFTWARE
    ```

### <a name="step-3-enable-serial-console-and-memory-dump-collection"></a>Étape 3 : Activer la console série et la collecte de l’image mémoire

Pour activer la collecte d’image mémoire et la console série, exécutez le script suivant :

1. Ouvrez une session d’invite de commandes avec élévation de privilèges. (Exécutez en tant qu’administrateur.)
1. Exécutez ces commandes pour activer la console série :
    
    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    ```

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```
1. Assurez-vous que l’espace libre sur le disque du système d’exploitation est au moins égal à la taille de la mémoire (RAM) de la machine virtuelle.

    Si l’espace est insuffisant sur le disque du système d’exploitation, modifiez l’emplacement de l’image mémoire et rattachez-le à un disque de données attaché avec suffisamment d’espace libre. Pour changer l’emplacement, remplacez « %SystemRoot% » par la lettre de lecteur (par exemple, « F: ») du disque de données dans les commandes suivantes.

    **Configuration suggérée pour activer l’image mémoire du système d’exploitation**

    Chargez le disque de système d’exploitation corrompu :

    ```
    REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
    ```

    Activez sur ControlSet001 :
    
    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```

    Activez sur ControlSet002 :
    
    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```
    
    Déchargez le disque de système d’exploitation corrompu :

    ```
    REG UNLOAD HKLM\BROKENSYSTEM
    ```

### <a name="step-4-rebuild-the-vm"></a>Étape 4 : Régénérez la machine virtuelle.

Suivez l’[étape 5 des commandes de réparation de machine virtuelle](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) pour réassembler la machine virtuelle.

Si le problème est résolu, cela signifie que la stratégie est désormais désactivée localement. Pour une solution permanente, n’utilisez pas la stratégie CleanupProfiles sur les machines virtuelles. Utilisez une autre méthode pour effectuer les nettoyages des profils.

N’utilisez pas cette stratégie :

`Machine\Admin Templates\System\User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="next-steps"></a>Étapes suivantes

Si vous rencontrez des problèmes lorsque vous appliquez une mise à jour Windows, consultez [La machine virtuelle ne répond pas (erreur « C01A001D ») lors de l’application de la mise à jour Windows](./unresponsive-vm-apply-windows-update.md).
