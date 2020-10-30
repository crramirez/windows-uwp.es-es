---
description: Repasa esta lista para evitar problemas que, con frecuencia, hacen que las aplicaciones no se puedan certificar o problemas que pueden detectarse durante una comprobación puntual después de que la aplicación se haya publicado.
title: Evitar errores de certificación comunes
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 672da214582fb6b206d7e16e1e776be40caeec90
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031228"
---
# <a name="avoid-common-certification-failures"></a>Evitar errores de certificación comunes


Repasa esta lista para evitar problemas que, con frecuencia, hacen que las aplicaciones no se puedan certificar o problemas que pueden detectarse durante una comprobación puntual después de que la aplicación se haya publicado.

> [!NOTE]
> Asegúrese de revisar las [directivas de Microsoft Store](store-policies.md) para asegurarse de que la aplicación cumple todos los requisitos que se enumeran allí.

-   Envía la aplicación solamente cuando esté terminada. Puedes usar la descripción de la aplicación para mencionar las próximas funciones, pero asegúrate de que no contenga secciones incompletas, vínculos a páginas web en construcción o cualquier elemento que pueda dar al cliente la impresión de que la aplicación está incompleta.

-   [Prueba tu aplicación con el Kit para la certificación de aplicaciones de Windows](../debug-test-perf/windows-app-certification-kit.md) antes de que envíes tu aplicación.

-   Prueba tu aplicación en diversas configuraciones para asegurarte de que sea lo más estable posible.

-   Asegúrese de que la aplicación no se bloquea sin conectividad de red. Aun si se requiere una conexión para usar realmente la aplicación, es necesario que tenga un rendimiento adecuado cuando no haya conexión.

-   [Proporcione la información necesaria](notes-for-certification.md) para usar la aplicación, como el nombre de usuario y la contraseña de una cuenta de prueba si la aplicación requiere que los usuarios inicien sesión en un servicio o en los pasos necesarios para tener acceso a las características ocultas o bloqueadas.

-   Incluya una [dirección URL de directiva de privacidad](enter-app-properties.md#privacy-policy-url) si la aplicación requiere una. por ejemplo, si la aplicación tiene acceso a cualquier tipo de información personal de algún modo o lo requiere la ley. Para ayudar a determinar si la aplicación requiere una directiva de privacidad, revise el [contrato para desarrolladores de aplicaciones](/legal/windows/agreements/app-developer-agreement) y las directivas de [Microsoft Store](store-policies.md).

-   Asegúrate de que la descripción de la aplicación refleja fielmente lo que hace la aplicación. Para obtener ayuda, consulta nuestras directrices sobre cómo [escribir una buena descripción de la aplicación](write-a-great-app-description.md).

-   Proporcione respuestas completas y precisas a todas las preguntas de la sección [clasificaciones de edad](age-ratings.md) .

-   No [declares tu aplicación como accesible](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), a menos que hayas realizado en ella ingeniería específica y la hayas probado en escenarios de accesibilidad.

-   Si tu aplicación usa las API de comercio de la Tienda Windows desde el espacio de nombres [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store), asegúrate de probar la aplicación y comprobar que administre excepciones típicas. Además, asegúrese de que la aplicación usa la clase [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) y no la clase [**CurrentAppSimulator**](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) , que solo es para fines de prueba. (Tenga en cuenta que si la aplicación tiene como destino Windows 10, versión 1607 o posterior, se recomienda que use los miembros del espacio de nombres [Windows. Services. Store](/uwp/api/windows.services.store) en lugar del espacio de nombres Windows. ApplicationModel. Store).


 

 
