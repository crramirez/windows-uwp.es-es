---
author: jnHs
Description: Review this list to help avoid issues that frequently prevent apps from getting certified, or that might be identified during a spot check after the app is published.
title: Evitar errores de certificación comunes
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7f37412ac88b001f412a1495f2b4efa029cf4d3a
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5869245"
---
# <a name="avoid-common-certification-failures"></a>Evitar errores de certificación comunes


Repasa esta lista para evitar problemas que, con frecuencia, hacen que las aplicaciones no se puedan certificar o problemas que pueden detectarse durante una comprobación puntual después de que la aplicación se haya publicado.

> [!NOTE]
> Asegúrate de revisar las [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies) para garantizar que la aplicación cumple los requisitos establecidos en ellas.

-   Envía la aplicación solamente cuando esté terminada. Puedes usar la descripción de la aplicación para mencionar las próximas funciones, pero asegúrate de que no contenga secciones incompletas, vínculos a páginas web en construcción o cualquier elemento que pueda dar al cliente la impresión de que la aplicación está incompleta.

-   [Prueba tu aplicación con el Kit para la certificación de aplicaciones de Windows](../debug-test-perf/windows-app-certification-kit.md) antes de que envíes tu aplicación.

-   Prueba tu aplicación en diversas configuraciones para asegurarte de que sea lo más estable posible.

-   Asegúrate de que tu aplicación no se bloquee, si no existe conectividad de red. Aun si se requiere una conexión para usar realmente la aplicación, es necesario que tenga un rendimiento adecuado cuando no haya conexión.

-   No te olvides de [proporciona toda la información necesaria](notes-for-certification.md) que se requiere para usar tu aplicación, como el nombre de usuario y contraseña para una cuenta de prueba en el caso de que tu aplicación necesite que los usuarios se registren en un servicio, o todos los pasos necesarios para acceder a funciones ocultas o bloqueadas.

-   Incluir una [dirección URL de la directiva de privacidad](enter-app-properties.md#privacy-policy-url) , si la aplicación requiere una; Por ejemplo, si la aplicación tiene acceso a cualquier tipo de información personal de cualquier forma o se exige por ley. Para ayudar a determinar si la aplicación requiere una directiva de privacidad, revisa el [Acuerdo para desarrolladores](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y las [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies).

-   Asegúrate de que la descripción de la aplicación refleja fielmente lo que hace la aplicación. Para obtener ayuda, consulta nuestras directrices sobre cómo [escribir una buena descripción de la aplicación](write-a-great-app-description.md).

-   Proporciona respuestas completas y precisas a todas las preguntas de la sección [Clasificaciones por edades](age-ratings.md).

-   No [declares tu aplicación como accesible](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), a menos que hayas realizado en ella ingeniería específica y la hayas probado en escenarios de accesibilidad.

-   Si tu aplicación usa las API de comercio de la Tienda Windows desde el espacio de nombres [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), asegúrate de probar la aplicación y comprobar que administre excepciones típicas. Asimismo, asegúrate de que la aplicación use la clase [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) y no la clase [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator), que está destinada a fines de prueba solamente. (Ten en cuenta si la aplicación está destinada a Windows10, versión 1607 (o posteriores), se recomienda usar los miembros del espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) en lugar del espacio de nombres Windows.ApplicationModel.Store).


 

 




