---
title: Inscrire une API web appelant des API web | Azure
titleSuffix: Microsoft identity platform
description: Découvrez comment créer une API web qui appelle des API web en aval (inscription d’application).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8349b7a53eb7b03b27c695bb24c8cb9bc665a1ac
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98756349"
---
# <a name="a-web-api-that-calls-web-apis-app-registration"></a>Une API web qui appelle des API web : Inscription d'application

Une API web qui appelle des API web en aval a la même inscription qu’une API web protégée. Par conséquent, vous devez suivre les instructions fournies à [API web protégée : Inscription d’application](scenario-protected-web-api-app-registration.md).

Comme l’application web appelle maintenant des API web, elle devient une application cliente confidentielle. C’est pourquoi des informations d’inscription supplémentaires sont nécessaires : l’application a besoin de partager des secrets (informations d’identification du client) avec la plateforme d’identités Microsoft.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Autorisations des API

Les applications web appellent des API pour le compte d’utilisateurs pour lesquels le jeton du porteur a été reçu. Elles doivent demander des autorisations déléguées. Pour plus d’informations, consultez [Ajouter des autorisations pour accéder à votre API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-your-web-api).

## <a name="next-steps"></a>Étapes suivantes

Passez à l’article suivant de ce scénario, [Configuration du code de l’application](scenario-web-api-call-api-app-configuration.md).
