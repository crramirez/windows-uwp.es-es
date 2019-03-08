---
title: Servicios sociales de programación
description: Proporciona un ejemplo de código de cómo usar la API de administrador de redes sociales de Xbox Live.
ms.assetid: 101d059a-e03f-472c-8300-800aa5730ee2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, el Administrador de redes sociales, ejemplo
ms.localizationpriority: medium
ms.openlocfilehash: 5039d9ed205cadfee3b2e64527a1f58f1624accc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592060"
---
# <a name="programming-social-services"></a>Servicios sociales de programación

> [!NOTE]
> Este artículo muestra el uso avanzado de API.  Como punto de partida, eche un vistazo a la [Introducción a la API del Administrador de redes sociales](../intro-to-social-manager.md) que simplifican enormemente el desarrollo.  Comunique su madre si encuentra un escenario no admitido en el Administrador de redes sociales.

En el ejemplo de código siguiente se muestra cómo recuperar una relación de redes sociales con Xbox Live. Se genera una lista de todos los usuarios en el sistema y se recupera la primera de ellas. A continuación, recupera todas las relaciones de redes sociales del usuario. Por último, muestra las propiedades públicas de cada una de esas relaciones.

```cpp
XboxLiveContext^ xboxLiveContext = NULL;

// An XboxLiveContext for a user should be created only once, or you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_SocialService_GetSocialRelationshipsAsync()
{
    // Generate a list of users on the system.
    // Create an XboxLiveContext from user 0 (the first one returned).
    // This user's authentication token will be used for service requests.
    XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

    // Asynchronously retrieve all social relationships from that context.
    auto asyncOp = xboxLiveContext->SocialService->GetSocialRelationshipsAsync();

    create_task( asyncOp )
    .then([](XboxSocialRelationshipResult^ result)
    {
        // For each social relationship of the specified user...
        for( XboxSocialRelationship^ xboxSocialRelationship : result->Items )
        {
            // ...display the public properties of the relationship.
            LogComment( xboxSocialRelationship->XboxUserId );
            LogComment( xboxSocialRelationship->IsFavorite.ToString() );
        }

    })
    .wait();
}
```
