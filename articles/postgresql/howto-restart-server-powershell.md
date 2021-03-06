---
title: Redémarrer un serveur – Azure PowerShell - Azure Database pour PostgreSQL
description: Cet article explique comment redémarrer un serveur Azure Database pour PostgreSQL à l’aide de PowerShell.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 06/08/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 2d37de4a1861cb78b4a76c8ca7bc8c3643245b32
ms.sourcegitcommit: 0830e02635d2f240aae2667b947487db01f5fdef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2020
ms.locfileid: "97706964"
---
# <a name="restart-azure-database-for-postgresql-server-using-powershell"></a>Redémarrer un serveur Azure Database pour PostgreSQL à l’aide de PowerShell

Cette rubrique explique comment redémarrer un serveur Azure Database pour PostgreSQL. Vous pouvez avoir besoin de redémarrer votre serveur pour des raisons de maintenance, ce qui entraîne une brève interruption pendant l’opération.

Le redémarrage du serveur est bloqué si le service est occupé. Par exemple, le service peut traiter une opération précédemment demandée, telle que la mise à l’échelle de vCores.

> [!NOTE] 
> Le temps nécessaire à un redémarrage varie selon le processus de récupération de PostgreSQL. Pour réduire le délai de redémarrage, nous vous recommandons de diminuer la quantité d’activités se produisant sur le serveur avant le redémarrage. Vous pouvez également augmenter la fréquence des points de contrôle. Vous pouvez également régler les valeurs des paramètres liés aux points de contrôle, y compris `max_wal_size`. Il est également recommandé d’exécuter la commande `CHECKPOINT` avant de redémarrer le serveur.

## <a name="prerequisites"></a>Prérequis

Pour utiliser ce guide pratique, il vous faut :

- Le [module Az PowerShell](/powershell/azure/install-az-ps) installé localement ou [Azure Cloud Shell](https://shell.azure.com/) dans le navigateur
- Un [serveur Azure Database pour PostgreSQL](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Tant que le module Az.PostgreSql PowerShell est en préversion, vous devez l’installer séparément du module Az PowerShell à l’aide de la commande suivante : `Install-Module -Name Az.PostgreSql -AllowPrerelease`
> Une fois que le module Az.PostgreSql PowerShell est en disponibilité générale, il devient partie intégrante des versions ultérieures du module Az PowerShell et est disponible en mode natif dans Azure Cloud Shell.

Si vous choisissez d’utiliser PowerShell en local, connectez-vous à votre compte Azure à l’aide de la cmdlet [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="restart-the-server"></a>Redémarrez le serveur

Redémarrez le serveur en utilisant la commande suivante :

```azurepowershell-interactive
Restart-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer un serveur Azure Database pour PostgreSQL à l’aide de PowerShell](quickstart-create-postgresql-server-database-using-azure-powershell.md)