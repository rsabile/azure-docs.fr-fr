---
title: Générer une chaîne de connexion avec PowerShell – Azure Database for MariaDB
description: Cet article fournit un exemple Azure PowerShell pour générer une chaîne de connexion à Azure Database for MariaDB.
author: savjani
ms.author: pariks
ms.service: jroth
ms.custom: mvc, devx-track-azurepowershell
ms.topic: how-to
ms.date: 8/5/2020
ms.openlocfilehash: 79109a7f39debfa14c8a11d9014685382151ab3b
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98663721"
---
# <a name="how-to-generate-an-azure-database-for-mariadb-connection-string-with-powershell"></a>Comment générer une chaîne de connexion à Azure Database for MariaDB avec PowerShell

Cet article montre comment générer une chaîne de connexion pour un serveur Azure Database for MariaDB. Vous pouvez utiliser une chaîne de connexion pour vous connecter à Azure Database for MariaDB à partir de nombreuses applications différentes.

## <a name="requirements"></a>Spécifications

Cet article utilise comme point de départ les ressources créées dans le guide suivant :

* [Démarrage rapide : Créer un serveur Azure Database for MariaDB à l’aide de PowerShell](quickstart-create-mariadb-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Obtenir la chaîne de connexion

La cmdlet `Get-AzMariaDbConnectionString` est utilisée pour générer une chaîne de connexion afin de connecter des applications à Azure Database for MariaDB. L’exemple suivant retourne la chaîne de connexion pour un client PHP depuis **mydemoserver**.

```azurepowershell-interactive
Get-AzMariaDbConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
$con=mysqli_init();mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "mydemoserver.mariadb.database.azure.com", "myadmin@mydemoserver", {your_password}, {your_database}, 3306);
```

Les valeurs valides pour le paramètre `Client` sont :

* ADO&#46;NET
* JDBC
* Node.js
* PHP
* Python
* Ruby
* WebApp

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Personnaliser les paramètres du serveur Azure Database for MariaDB à l’aide de PowerShell](howto-configure-server-parameters-using-powershell.md)
