---
title: 'Démarrage rapide : Bien démarrer avec la création d’un espace de travail Synapse'
description: Dans ce tutoriel, vous allez voir comment créer un espace de travail Synapse, un pool SQL dédié et un pool Apache Spark serverless.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 3a2636ec73d20f3011d8413c794e68ef41b1829c
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209183"
---
# <a name="creating-a-synapse-workspace"></a>Création d’un espace de travail Synapse

Dans ce tutoriel, vous allez voir comment créer un espace de travail Synapse, un pool SQL dédié et un pool Apache Spark serverless. 

## <a name="prerequisites"></a>Prérequis

Pour effectuer les étapes de ce tutoriel, vous devez avoir accès à un groupe de ressources pour lequel vous disposez du rôle **Propriétaire**. Créez l’espace de travail Synapse dans ce groupe de ressources.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Créer un espace de travail Synapse dans le portail Azure

1. Ouvrez le [Portail Azure](https://portal.azure.com) et, en haut, recherchez **Synapse**.
1. Dans les résultats de la recherche, sous **Services**, sélectionnez **Azure Synapse Analytics**.
1. Sélectionnez **Ajouter** pour créer un espace de travail.
1. Dans **Informations de base**, sous Détails du projet, entrez votre **Abonnement**, votre **Groupe de ressources** et votre **Région** préférés, puis choisissez un nom d’espace de travail. Dans ce tutoriel, nous utiliserons **myworkspace**.
1. À côté de **Sélectionner Data Lake Storage Gen2**, cliquez sur le bouton **À partir de l’abonnement**.
1. À côté de **Nom du compte**, cliquez sur **Créer**, puis nommez le nouveau compte de stockage **contosolake** ou avec un nom similaire, car ce nom doit être unique.
1. À côté de **Nom du système de fichiers**, cliquez sur **Créer**, puis nommez-le **users**. Cette opération crée un conteneur de stockage appelé **users**
1. L’espace de travail utilise ce compte de stockage comme compte de stockage « principal » pour les tables Spark et les journaux des applications Spark.
1. Cochez la case « Attribuez-moi le rôle Contributeur aux données blob du stockage sur le compte Data Lake Storage Gen2 ». 
1. Sélectionnez **Vérifier + créer** > **Créer**. Votre espace de travail est prêt en quelques minutes.

> [!NOTE]
> Pour activer les fonctionnalités de l’espace de travail à partir d’un pool SQL dédié (anciennement SQL DW) existant, reportez-vous au [Guide pratique pour activer un espace de travail pour votre pool SQL dédié (anciennement SQL DW)](./sql-data-warehouse/workspace-connected-create.md).


## <a name="open-synapse-studio"></a>Ouvrir Synapse Studio

Après avoir créé votre espace de travail Azure Synapse, vous pouvez ouvrir Synapse Studio de deux manières :

* Ouvrez votre espace de travail Synapse dans le [portail Azure](https://portal.azure.com). Dans la section **Vue d’ensemble** de l’espace de travail Synapse, sélectionnez **Ouvrir** dans la zone Ouvrir Synapse Studio.
* Accédez à `https://web.azuresynapse.net` et connectez-vous à votre espace de travail.


## <a name="the-built-in-serverless-sql-pool"></a>Pool SQL serverless intégré

Chaque espace de travail est fourni avec un pool SQL serverless prédéfini dit **intégré**. Ce pool ne peut pas être supprimé. Les pools SQL serverless vous permettent d’utiliser SQL sans avoir à réserver de capacité avec des pools SQL dédiés. Contrairement aux pools SQL dédiés, la facturation d’un pool SQL serverless est basée sur la quantité de données analysées pour exécuter la requête, et non sur le nombre d’unités de capacité allouées au pool.


## <a name="create-a-dedicated-sql-pool"></a>Créer un pool SQL dédié

1. Dans Synapse Studio, dans le volet de gauche, sélectionnez **Gérer** > **Pools SQL**.
1. Sélectionnez **Nouveau**
1. Pour **Nom du pool SQL**, sélectionnez **SQLPOOL1**
1. Pour **Niveau de performances**, choisissez **DW100C**
1. Sélectionnez **Vérifier + créer** > **Créer**. Votre pool SQL dédié sera prêt en quelques minutes. Votre pool SQL dédié est associé à une base de données du pool SQL dédié qui est également appelée **SQLPOOL1**.

Tant qu’il est actif, un pool SQL dédié consomme des ressources facturables. Vous pouvez suspendre le pool plus tard pour réduire les coûts.

> [!NOTE] 
> Quand vous créez un pool SQL dédié (anciennement SQL DW) dans votre espace de travail, la page de provisionnement des pools SQL s’ouvre. Le provisionnement s’effectue sur le serveur SQL logique.


## <a name="create-a-serverless-apache-spark-pool"></a>Créer un pool Apache Spark serverless

1. Dans Synapse Studio, dans le volet de gauche, sélectionnez **Gérer** > **Pools Apache Spark**.
1. Sélectionnez **Nouveau** 
1. Pour **Nom du pool Apache Spark**, entrez **Spark1**.
1. Pour **Taille du nœud**, entrez **Petite**.
1. Pour **Nombre de nœuds**, définissez la valeur minimale 3 et la valeur maximale 3
1. Sélectionnez **Vérifier + créer** > **Créer**. Votre pool Apache Spark sera prêt en quelques secondes.

Le pool Spark indique à Azure Synapse la quantité de ressources Spark à utiliser. Vous payez uniquement les ressources consommées. Quand vous cessez d’utiliser activement le pool, les ressources expirent automatiquement et sont recyclées.


## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Analyser en utilisant un pool SQL dédié](get-started-analyze-sql-pool.md)
