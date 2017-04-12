---
author: drewbat
ms.assetid: 
title: "Desarrollar aplicaciones para UWP con API de versión preliminar"
description: "Comprender las ventajas y desventajas de usar las API de versión preliminar que se incluyen en las versiones preliminares del SDK de UWP."
ms.openlocfilehash: ede7e8d5e9cce39850edbdb70a715d76c78f0c01
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developing-uwp-apps-with-pre-release-apis"></a>Desarrollar aplicaciones para UWP con API de versión preliminar

Microsoft publica versiones preliminares del SDK de Plataforma universal de Windows (UWP) para que los desarrolladores puedan interactuar con las nuevas características de plataforma antes de que se finalicen. De este modo, consigues avanzar en la incorporación de características en tu aplicación y te ayuda a publicar la aplicación poco después de que se publique la versión RTM oficial del SDK. El uso de API de versión preliminar también te permite proporcionar comentarios a Microsoft para ayudar a influenciar la dirección de la plataforma en versiones futuras.

## <a name="important-limitations-on-the-use-of-pre-release-apis"></a>Limitaciones importantes sobre el uso de API de versión preliminar
Antes de usar una API de versión preliminar en tu aplicación, ten en cuenta las siguientes implicaciones importantes: 
* Las aplicaciones que usan API de versión preliminar no se pueden enviar a la Tienda Windows hasta las API se publiquen oficialmente en una versión RTM. Recomendamos encarecidamente que mantengas el código de desarrollo de versión preliminar independiente del código para las aplicaciones publicadas actualmente. 
* Las aplicaciones que desarrolles mediante un SDK de versión preliminar no se pueden enviar a la Tienda, incluso si no usan API de versión preliminar. Deberías instalar las herramientas de versión preliminar en un equipo o máquina virtual que sea independiente del equipo de producción que usas para el desarrollo principal. 
* Es probable que las API de versión preliminar cambien antes de la publicación de la versión RTM. Cuando se incluyen API en un SDK de versión preliminar, es probable que la característica o escenario que habilitan se incluya en el SDK final. Sin embargo, es posible que los nombres, las firmas y el comportamiento de algunas API específicas cambien antes de la versión final. También es posible que la API se quite por completo. 

## <a name="how-to-identify-a-prerelease-api"></a>Cómo identificar una API de versión preliminar 
En la documentación de referencia de API para UWP, las API que sean versiones preliminares llevan una etiqueta con el texto siguiente: 

En este artículo se describe una API o característica de versión preliminar que podría sufrir importantes modificaciones antes de que su publicación comercial. Puedes usar esta característica para desarrollo y pruebas ahora mismo, pero las aplicaciones que la usen no se pueden enviar a la Tienda Windows hasta después de su publicación comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí. Para más información sobre el desarrollo con API de versión preliminar, consulta Desarrollar aplicaciones para UWP con API de versión preliminar. 

## <a name="get-the-latest-sdk-insider-preview"></a>Obtener el SDK Insider Preview más reciente 
1. [Regístrate en el Programa Windows Insider](https://insider.windows.com/) para acceder a versiones preliminares del SDK. 
3. Antes de instalar las herramientas de versión preliminar para desarrolladores, consulta las [notas de la versión del emulador actual de SDK y Mobile](http://go.microsoft.com/fwlink/?LinkId=829180).
4. Instala [SDK Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).
5. Consulta el [Foro de la comunidad de Windows Insider Preview](http://go.microsoft.com/fwlink/p/?LinkId=507620).
