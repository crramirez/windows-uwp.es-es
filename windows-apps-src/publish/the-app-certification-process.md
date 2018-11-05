---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: Proceso de certificación de la aplicación
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, publicar, preprocesamiento, certificación, lanzamiento, pendientes, enviar, publicar, estado, tiempo
ms.localizationpriority: medium
ms.openlocfilehash: 161a95141511bc00b1d1a707e893d85dccfa8409
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6024613"
---
# <a name="the-app-certification-process"></a>Proceso de certificación de la aplicación

Cuando termines de crear el envío de la aplicación y hagas clic en **Enviar a Store**, el envío entra en el paso de certificación. Normalmente este proceso se completa en unas horas, aunque en algunos casos puede tardar hasta tres días laborables. Cuando el envío supere la certificación, puede tardar hasta 24 horas para los clientes vean la descripción de la aplicación para un nuevo envío, o para un envío actualizado con los cambios a los paquetes. Si la actualización solo cambia los detalles de descripción de la tienda, se completará el proceso de publicación en menos de una hora.  Se te notificará cuando el envío se publique y estado de la aplicación en el panel será **En la tienda**.

## <a name="preprocessing"></a>Preprocesamiento

Después de cargar los paquetes de la aplicación correctamente y enviar la aplicación para su certificación, los paquetes se ponen en cola para someterse a pruebas. Se mostrará un mensaje si se detectan los errores durante el preprocesamiento. Para obtener más información sobre posibles errores, consulta [Resolver errores de envío](resolve-submission-errors.md).

## <a name="certification"></a>Certificación

Durante esta fase, se llevan a cabo varias pruebas:

-   **Pruebas de seguridad:** la primera prueba comprueba si hay virus y malware en los paquetes de la aplicación. Si la aplicación no pasa esta prueba, necesitarás comprobar el sistema de desarrollo ejecutando el software antivirus más reciente y luego recompilar el paquete de la aplicación en un sistema limpio.
-   **Pruebas de cumplimiento técnico:** el cumplimiento de los requisitos técnicos se prueba con el Kit para la certificación de aplicaciones en Windows. (Debes asegurarte siempre de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviarla a la Tienda).
-   **Cumplimiento de requisitos de contenido:** el tiempo que tarda depende de la complejidad de la aplicación, la cantidad de contenido visual que tenga y la cantidad de aplicaciones que se hayan enviado recientemente. Asegúrate de proporcionar toda la información que los evaluadores deben conocer en la página [Notas para la certificación](notes-for-certification.md).

Una vez completado el proceso de certificación, obtendrás un informe donde se indicará si la aplicación pasó o no la certificación. Si no la pasó, el informe indicará en qué prueba produjo errores o qué [directiva](https://docs.microsoft.com/legal/windows/agreements/store-policies) no se cumplió. Después de solucionar el problema, puedes crear un nuevo envío de la aplicación para volver a iniciar el proceso de certificación.

## <a name="release"></a>Versión

Cuando la aplicación supere la certificación, está listo para trasladar el proceso de **publicación** .

- Si has indicado que se debe publicar el envío tan pronto como sea posible (opción predeterminada), el proceso de publicación comenzará de inmediato.
- Si es la primera vez que has publicado la aplicación y especifica una **fecha de lanzamiento** en la sección [programación](configure-precise-release-scheduling.md#release) , la aplicación estará disponible en función de las selecciones de la **fecha de lanzamiento** .
- Si has usado [las opciones de suspensión de publicación](manage-submission-options.md#publishing-hold-options) para especificar que no se debe lanzar hasta una fecha determinada, esperaremos hasta esa fecha para comenzar el proceso de publicación, a menos que selecciones **Cambiar fecha de lanzamiento**.
- Si has usado [las opciones de suspensión de publicación](manage-submission-options.md#publishing-hold-options) para especificar que quieres publicar el envío manualmente, hemos no se inicia el proceso de publicación hasta que seleccione **Publicar ahora** (o selecciona **Cambiar fecha de lanzamiento** y elige una fecha específica).


## <a name="publishing"></a>Publicación

Los paquetes de la aplicación se firman digitalmente para protegerlos de alteraciones después de su lanzamiento. Una vez iniciada esta fase, ya no puedes cancelar el envío ni cambiar la fecha de lanzamiento.

Para nuevas aplicaciones y actualizaciones que se incluyen los cambios en los paquetes de la aplicación, se completará el proceso de publicación en 24 horas. Para las actualizaciones que solo cambian opciones como la descripción de los detalles de la tienda, pero no cambian los paquetes de la aplicación, el proceso de publicación tardará menos de una hora.

Mientras la aplicación está en la fase de publicación, el vínculo **Mostrar detalles** de la columna de estado de envío de la aplicación te permite saber cuándo están disponibles para los clientes en cada uno de tus versiones de sistema operativo compatibles los nuevos paquetes y los detalles de descripción de la tienda. Los pasos que no se hayan completado llevarán la indicación **Pendiente**. La aplicación permanecerá en la fase de publicación hasta que se complete el proceso, lo que significa que los nuevos paquetes o detalles del listado estén disponibles para todos los clientes potenciales de la aplicación.

## <a name="in-the-store"></a>En Store 

Tras completar satisfactoriamente los pasos anteriores, el estado del envío cambiará de **Publicación** a **En Store**. Entonces, tu envío estará disponible en Microsoft Store para que lo descarguen los clientes (a menos que hayas elegido otra opción de [Visibilidad](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> También se realizan comprobaciones puntuales de aplicaciones después de haberse publicado, para poder identificar posibles problemas y garantizar que la aplicación cumple con todas las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Si encontramos algún problema, se te notificará al respecto y sobre cómo solucionarlo, si corresponde, o bien sobre si la aplicación se ha quitado de la Tienda.

 

 

 




