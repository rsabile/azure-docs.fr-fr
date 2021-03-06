---
title: Exemple de graphique de métrique Azure Monitor
description: En savoir plus sur la visualisation de vos données Azure Monitor.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 05dadfe88ed64aea8066b02298ba158a44a03c6f
ms.sourcegitcommit: 6cca6698e98e61c1eea2afea681442bd306487a4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/24/2020
ms.locfileid: "97760176"
---
# <a name="metric-chart-examples"></a>Exemples de graphiques de métrique 

La plateforme Azure propose [plus d’un millier de métriques](./metrics-supported.md), qui ont des dimensions. À l’aide de [filtres de dimension](./metrics-charts.md), de l’application de [division](./metrics-charts.md), du contrôle du type de graphique et de l’ajustement des paramètres de graphique, vous pouvez créer des tableaux de bord et des affichages de diagnostic parlants qui fournissent des insights sur l’intégrité de votre infrastructure et de vos applications. Cet article présente quelques exemples de graphiques que vous pouvez générer à l’aide de [Metrics Explorer](./metrics-charts.md), et explique les étapes nécessaires pour configurer chacun de ces graphiques.

Vous souhaitez partager vos exemples de magnifiques graphiques avec le monde ? Apportez votre contribution à cette page sur GitHub et partagez vos exemples de graphiques ici !

## <a name="website-cpu-utilization-by-server-instances"></a>Utilisation du processeur du site web par instances de serveur

Ce graphique montre si l’utilisation du processeur d’un service App Service se trouvait dans une plage raisonnable et la répartit par instance pour déterminer si la charge a été correctement distribuée. D’après le graphique, vous pouvez constater que l’application s’exécutait sur une instance de serveur unique avant 6 h 00, puis est montée en puissance avec l’ajout d’une autre instance.

![Graphique en courbes du pourcentage de processeur moyen par instance de serveur](./media/metrics-charts/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Comment configurer ce graphique ?

Sélectionnez votre ressource App Service et recherchez la métrique **Pourcentage UC**. Cliquez ensuite sur **Appliquer la division** et sélectionnez la dimension **Instance**.

## <a name="application-availability-by-region"></a>Disponibilité de l’application par région

Affichez la disponibilité de votre application par région pour identifier les emplacements géographiques qui rencontrent des problèmes. Ce graphique montre la métrique de disponibilité Application Insights. Vous pouvez voir que l’application analysée ne rencontre aucun problème de disponibilité au niveau du centre de données USA Est, mais il existe un problème de disponibilité partielle au niveau des régions USA Ouest et Asie Est.

![Graphique de disponibilité moyenne par emplacements](./media/metrics-charts/availability-by-location.png)

### <a name="how-to-configure-this-chart"></a>Comment configurer ce graphique ?

Vous devez tout d’abord activer la surveillance [Disponibilité Application Insights](../app/monitor-web-app-availability.md) pour votre site web. Après cela, sélectionnez votre ressource Application Insights, puis la métrique Disponibilité. Appliquez la division sur la dimension **Emplacement d’exécution**.

## <a name="volume-of-failed-storage-account-transactions-by-api-name"></a>Volume de transactions de compte de stockage non réussies par nom d’API

Votre ressource de compte de stockage est confrontée à un volume trop élevé de transactions non réussies. Vous pouvez utiliser la métrique des transactions pour identifier les API responsables de cet excès d’échecs. Notez que le graphique suivant est configuré avec la même dimension (nom de l’API) au niveau de sa division. Il est filtré en fonction du type de réponse correspondant à un échec :

![Graphique en barres des transactions d’API](./media/metrics-charts/split-and-filter-example.png)

### <a name="how-to-configure-this-chart"></a>Comment configurer ce graphique ?

Dans le sélecteur de métriques, sélectionnez votre compte de stockage et la métrique **Transactions**. Changez le type de graphique par **Graphique à barres**. Cliquez sur **Appliquer la division** et sélectionner une dimension **Nom de l’API**. Cliquez ensuite sur **Ajouter un filtre** et choisissez une fois de plus la dimension **Nom de l’API**. Dans la boîte de dialogue de filtres, sélectionnez les API que vous souhaitez tracer sur le graphique.

## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur Azure Monitor [Workbooks](./workbooks-overview.md)
* En savoir plus sur [Metrics Explorer](metrics-charts.md)

