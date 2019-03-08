---
Description: Repasa esta lista para evitar problemas que, con frecuencia, hacen que las aplicaciones no se puedan certificar o problemas que pueden detectarse durante una comprobación puntual después de que la aplicación se haya publicado.
title: Evitar errores de certificación comunes
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 62c99c159ff68201919fa15baded999e3b6a2477
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625800"
---
# <a name="avoid-common-certification-failures"></a>Evitar errores de certificación comunes


Repasa esta lista para evitar problemas que, con frecuencia, hacen que las aplicaciones no se puedan certificar o problemas que pueden detectarse durante una comprobación puntual después de que la aplicación se haya publicado.

> [!NOTE]
> No olvide revisar la [las directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies) para asegurarse de que la aplicación cumple todos los requisitos en la lista.

-   Envía la aplicación solamente cuando esté terminada. Puedes usar la descripción de la aplicación para mencionar las próximas funciones, pero asegúrate de que no contenga secciones incompletas, vínculos a páginas web en construcción o cualquier elemento que pueda dar al cliente la impresión de que la aplicación está incompleta.

-   [Prueba tu aplicación con el Kit para la certificación de aplicaciones de Windows](../debug-test-perf/windows-app-certification-kit.md) antes de que envíes tu aplicación.

-   Prueba tu aplicación en diversas configuraciones para asegurarte de que sea lo más estable posible.

-   Asegúrate de que tu aplicación no se bloquee, si no existe conectividad de red. Aun si se requiere una conexión para usar realmente la aplicación, es necesario que tenga un rendimiento adecuado cuando no haya conexión.

-   No te olvides de [proporciona toda la información necesaria](notes-for-certification.md) que se requiere para usar tu aplicación, como el nombre de usuario y contraseña para una cuenta de prueba en el caso de que tu aplicación necesite que los usuarios se registren en un servicio, o todos los pasos necesarios para acceder a funciones ocultas o bloqueadas.

-   Incluir un [URL de la política de privacidad](enter-app-properties.md#privacy-policy-url) si su aplicación requiere una; por ejemplo, si la aplicación tiene acceso a cualquier tipo de información personal de cualquier manera o en caso contrario es requerido por la ley. Para ayudar a determinar si la aplicación requiere una directiva de privacidad, revise el [acuerdo del desarrollador de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) y [las directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies).

-   Asegúrate de que la descripción de la aplicación refleja fielmente lo que hace la aplicación. Para obtener ayuda, consulta nuestras directrices sobre cómo [escribir una buena descripción de la aplicación](write-a-great-app-description.md).

-   Proporciona respuestas completas y precisas a todas las preguntas de la sección [Clasificaciones por edades](age-ratings.md).

-   No [declares tu aplicación como accesible](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), a menos que hayas realizado en ella ingeniería específica y la hayas probado en escenarios de accesibilidad.

-   Si tu aplicación usa las API de comercio de la Tienda Windows desde el espacio de nombres [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), asegúrate de probar la aplicación y comprobar que administre excepciones típicas. Asimismo, asegúrate de que la aplicación use la clase [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) y no la clase [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator), que está destinada a fines de prueba solamente. (Ten en cuenta si la aplicación está destinada a Windows 10, versión 1607 (o posteriores), se recomienda usar los miembros del espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) en lugar del espacio de nombres Windows.ApplicationModel.Store).


 

 




