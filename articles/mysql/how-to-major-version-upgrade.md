---
title: Mise à niveau de version majeure dans Azure Database pour MySQL – Serveur unique
description: Cet article explique comment mettre à niveau une version majeure pour Azure Database pour MySQL – Serveur unique.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 1/13/2021
ms.openlocfilehash: b62f4ebc61ac27478788d8b2bae5e4145f87ac8b
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98630198"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server"></a>Mise à niveau de version majeure dans Azure Database pour MySQL Serveur unique

> [!IMPORTANT]
> La mise à niveau de version majeure pour un serveur unique Azure Database pour MySQL est en préversion publique.

Cet article décrit comment vous pouvez mettre à niveau votre version majeure de MySQL sur place dans un serveur unique Azure Database pour MySQL.

Cette fonctionnalité permet aux clients d’effectuer des mises à niveau sur place de leurs serveurs MySQL 5.6 vers MySQL 5.7 en un clic de bouton sans devoir déplacer des données ou modifier une chaîne de connexion d’application.

> [!Note]
> * La mise à niveau de version majeure n’est disponible que pour une mise à niveau de version majeure de MySQL 5.6 vers MySQL 5.7<br>
> * La mise à niveau de version majeure n’est pas encore prise en charge sur un serveur réplica.
> * Le serveur ne sera pas disponible pendant l’opération de mise à niveau. Par conséquent, il est recommandé d’effectuer les mises à niveau au cours de votre fenêtre de maintenance planifiée.

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57-using-azure-portal"></a>Mise à niveau de version majeure de MySQL 5.6 à MySQL 5.7 avec le Portail Azure

Pour effectuer une mise à niveau de version majeure de votre serveur Azure Database pour MySQL 5.6 avec le Portail Azure, procédez comme suit.

> [!IMPORTANT]
> Nous vous recommandons de d’abord effectuer la mise à niveau sur la copie restaurée du serveur au lieu de procéder directement à la mise à niveau de la production. Consultez [Comment effectuer une restauration à un point dans le temps](howto-restore-server-portal.md#point-in-time-restore).

1. Dans le [portail Azure](https://portal.azure.com/), sélectionnez votre serveur Azure Database pour MySQL 5.6 existant.

2. Dans la page **Vue d’ensemble**, cliquez sur le bouton **Mise à niveau** dans la barre d’outils.

3. Dans la section **Mise à niveau**, sélectionnez **OK** pour mettre à niveau le serveur Azure Database pour MySQL de la version 5.6 vers la version 5.7.

   :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="Azure Database pour MySQL – Vue d’ensemble – Mise à niveau":::

4. Une notification confirme la réussite de la mise à niveau.


## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57-using-azure-cli"></a>Mise à niveau de version majeure de MySQL 5.6 à MySQL 5.7 avec Azure CLI

Pour effectuer une mise à niveau de version majeure de votre serveur Azure Database pour MySQL 5.6 avec Azure CLI, procédez comme suit.

> [!IMPORTANT]
> Nous vous recommandons de d’abord effectuer la mise à niveau sur la copie restaurée du serveur au lieu de procéder directement à la mise à niveau de la production. Consultez [Comment effectuer une restauration à un point dans le temps](howto-restore-server-cli.md#server-point-in-time-restore).

1. Installez [Azure CLI pour Windows](/cli/azure/install-azure-cli) ou utilisez Azure CLI dans [Azure Cloud Shell](../cloud-shell/overview.md) pour exécuter les commandes de mise à niveau. 
 
   La version 2.16.0 ou une version ultérieure d’Azure CLI est nécessaire pour cette mise à niveau. Si vous utilisez Azure Cloud Shell, la version la plus récente est déjà installée. Exécutez az version pour rechercher la version et les bibliothèques dépendantes installées. Pour effectuer une mise à niveau vers la dernière version, exécutez az upgrade.

2. Une fois la connexion établie, exécutez la commande [az mysql server upgrade](https://docs.microsoft.com/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_upgrade&preserve-view=true) :
    
   ```azurecli
   az mysql server upgrade --name testsvr --resource-group testgroup --subscription MySubscription --target-server-version 5.7"
   ```
   
   L’invite de commandes affiche le message « -Running ». Il disparaît une fois la mise à niveau de la version effectuée.

## <a name="frequently-asked-questions"></a>Forum aux questions

### <a name="when-will-this-upgrade-feature-be-ga-as-we-have-mysql-v56-in-our-production-environment-that-we-need-to-upgrade"></a>Quand cette fonctionnalité de mise à niveau sera-t-elle en disponibilité générale ? Nous avons dans notre environnement de production MySQL v5.6, que nous devons mettre à niveau.

La disponibilité générale de cette fonctionnalité est prévue avant l’abandon de MySQL v5.6. Toutefois, la fonctionnalité est prête pour la production et entièrement prise en charge par Azure. Vous pouvez donc l’exécuter en toute confiance dans votre environnement. Dans le cadre des meilleures pratiques recommandées, nous vous suggérons vivement de l’exécuter et de la tester d’abord sur une copie restaurée du serveur afin de pouvoir estimer le temps d’arrêt pendant la mise à niveau et d’effectuer un test de compatibilité des applications avant de l’exécuter en production. Pour plus d’informations, consultez [Procédure de restauration à un point dans le temps](howto-restore-server-portal.md#point-in-time-restore) afin de créer une copie de votre serveur à un point dans le temps. 

### <a name="will-this-cause-downtime-of-the-server-and-if-so-how-long"></a>Cela entraînera-t-il un temps d’arrêt du serveur et, le cas échéant, de combien de temps ?

Oui, le serveur n’est pas disponible pendant le processus de mise à niveau. Nous vous recommandons donc d’effectuer cette opération au cours de la fenêtre de maintenance planifiée. Le temps d’arrêt estimé dépend de la taille de la base de données, de la taille de stockage approvisionnée (IOPs approvisionnée) et du nombre de tables sur la base de données. La durée de la mise à niveau est directement proportionnelle au nombre de tables sur le serveur. Les mises à niveau des serveurs de référence SKU de base sont censées prendre plus de temps que sur une plateforme de stockage standard. Pour estimer le temps d’arrêt de votre environnement de serveur, nous vous recommandons d’effectuer la mise à niveau sur la copie restaurée du serveur.  

### <a name="it-is-noted-that-it-is-not-supported-on-replica-server-yet-what-does-that-mean-concrete"></a>Notez que la prise en charge n’est pas encore assurée sur un serveur réplica. Qu’est-ce que cela signifie concrètement ?

Actuellement, la mise à niveau de la version majeure n’est pas prise en charge pour le serveur réplica, ce qui signifie que vous ne devez pas l’exécuter pour les serveurs impliqués dans la réplication (serveur source ou réplica). Si vous souhaitez tester la mise à niveau des serveurs impliqués dans la réplication avant d’ajouter la prise en charge du réplica pour la fonctionnalité de mise à niveau, nous vous recommandons de procéder comme suit :

1. Au cours de la maintenance planifiée, [arrêtez la réplication et supprimez le serveur de réplication](howto-read-replicas-portal.md) après avoir capturé son nom et toutes les informations de configuration (paramètres du pare-feu, configuration des paramètres du serveur s’il est différent du serveur source).
2. Effectuez la mise à niveau du serveur source.
3. Approvisionnez un nouveau serveur réplica en lecture avec le même nom et les mêmes paramètres de configuration que ceux capturés à l’étape 1. Le nouveau serveur réplica passe automatiquement à la v5.7 une fois que le serveur source a été mis à niveau vers v5.7.

### <a name="what-will-happen-if-we-do-not-choose-to-upgrade-our-mysql-v56-server-before-february-5-2021"></a>Que se passe-t-il si nous ne choisissons pas de mettre à niveau notre serveur MySQL v5.6 avant le 5 février 2021 ?

Vous pouvez continuer à exécuter votre serveur MySQL v5.6 comme auparavant. Azure **n’effectuera jamais** de mise à niveau forcée sur votre serveur. Toutefois, les restrictions documentées dans la [stratégie de version d’Azure Database pour MySQL](concepts-version-policy.md) s’appliquent.

## <a name="next-steps"></a>Étapes suivantes

Découvrez la [Stratégie de contrôle de version Azure Database pour MySQL](concepts-version-policy.md).
