---
title: azcopy | Microsoft Docs
description: Cet article fournit des informations de référence sur la commande azcopy.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 19d5a5214ae4d2d6c0a18b68863211aa1f6573c6
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98878984"
---
# <a name="azcopy"></a>azcopy

AzCopy est un outil en ligne de commande qui permet de déplacer des données vers et à partir du stockage Azure.

## <a name="synopsis"></a>Synopsis

Le format général de la commande est le suivant : `azcopy [command] [arguments] --[flag-name]=[flag-value]`.

Pour signaler des problèmes ou pour en savoir plus sur l’outil, consultez [https://github.com/Azure/azure-storage-azcopy](https://github.com/Azure/azure-storage-azcopy).

## <a name="related-conceptual-articles"></a>Articles conceptuels associés

- [Bien démarrer avec AzCopy](storage-use-azcopy-v10.md)
- [Transférer des données avec AzCopy et le Stockage Blob](./storage-use-azcopy-v10.md#transfer-data)
- [Transférer des données avec AzCopy et le stockage de fichiers](storage-use-azcopy-files.md)
- [Configurer, optimiser et dépanner AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Options

**--cap-mbps** (float)  Limite la vitesse de transfert, en mégabits par seconde. Par moment, le débit peut dépasser légèrement cette limite. Si cette option est définie sur zéro ou si elle est omise, le débit n’est pas limité.

**--help** Aide pour azcopy
      
**--output-type**  (string) Met en forme la sortie de la commande. Les formats possibles sont « text » et « JSON ». La valeur par défaut est `text`. (valeur par défaut `text`)

**--trusted-microsoft-suffixes** (string) spécifie des suffixes de domaine supplémentaires où des jetons de connexion Azure Active Directory peuvent être envoyés.  La valeur par défaut est «  *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net ». Tous les éléments répertoriés ici sont ajoutés à la valeur par défaut. Pour la sécurité, vous devez placer uniquement des domaines Microsoft Azure ici. Séparez plusieurs entrées par des points-virgules.

## <a name="see-also"></a>Voir aussi

- [Bien démarrer avec AzCopy](storage-use-azcopy-v10.md)
- [azcopy bench](storage-ref-azcopy-bench.md)
- [azcopy copy](storage-ref-azcopy-copy.md)
- [azcopy doc](storage-ref-azcopy-doc.md)
- [azcopy env](storage-ref-azcopy-env.md)
- [azcopy jobs](storage-ref-azcopy-jobs.md)
- [azcopy jobs clean](storage-ref-azcopy-jobs-clean.md)
- [azcopy jobs list](storage-ref-azcopy-jobs-list.md)
- [azcopy jobs remove](storage-ref-azcopy-jobs-remove.md)
- [azcopy jobs resume](storage-ref-azcopy-jobs-resume.md)
- [azcopy jobs show](storage-ref-azcopy-jobs-show.md)
- [azcopy list](storage-ref-azcopy-list.md)
- [azcopy login](storage-ref-azcopy-login.md)
- [azcopy logout](storage-ref-azcopy-logout.md)
- [azcopy make](storage-ref-azcopy-make.md)
- [azcopy remove](storage-ref-azcopy-remove.md)
- [azcopy sync](storage-ref-azcopy-sync.md)
