---
description: Cuando termine de crear el envío de la aplicación y haga clic en enviar a la tienda, el envío entra en el paso de certificación.
title: Proceso de certificación de la aplicación
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, publicación, preprocesamiento, certificación, versión, pendiente, envío, publicación, estado, tiempo
ms.localizationpriority: medium
ms.openlocfilehash: 414a182074f03256c3c66492eab21e574e1a960c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034988"
---
# <a name="the-app-certification-process"></a>Proceso de certificación de la aplicación

Cuando termine de crear el envío de la aplicación y haga clic en **Enviar a la tienda** , el envío entra en el paso de certificación. Normalmente este proceso se completa en unas horas, aunque en algunos casos puede tardar hasta tres días laborables. Después de que el envío pase la certificación, los clientes pueden tardar hasta 24 horas en ver la lista de la aplicación en busca de un nuevo envío o en un envío actualizado con cambios en los paquetes. Si la actualización solo cambia los detalles de la lista, el proceso de publicación se completará en menos de una hora.  Se le notificará cuando se publique el envío, y el estado de la aplicación en el panel estará **en el almacén** .

## <a name="preprocessing"></a>Preprocessing (Preprocesamiento)

Después de cargar los paquetes de la aplicación correctamente y enviar la aplicación para su certificación, los paquetes se ponen en cola para someterse a pruebas. Se mostrará un mensaje si se detectan los errores durante el preprocesamiento. Para obtener más información sobre posibles errores, consulta [Resolver errores de envío](resolve-submission-errors.md).

## <a name="certification"></a>Certificación

Durante esta fase, se llevan a cabo varias pruebas:

-   **Pruebas de seguridad:** La primera prueba comprueba si hay virus y malware en los paquetes de la aplicación. Si la aplicación no pasa esta prueba, necesitarás comprobar el sistema de desarrollo ejecutando el software antivirus más reciente y luego recompilar el paquete de la aplicación en un sistema limpio.
-   **Pruebas de cumplimiento técnico:** el cumplimiento de los requisitos técnicos se prueba con el Kit para la certificación de aplicaciones en Windows. (Debes asegurarte siempre de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviarla a la Tienda).
-   **Cumplimiento de requisitos de contenido:** el tiempo que tarda depende de la complejidad de la aplicación, la cantidad de contenido visual que tenga y la cantidad de aplicaciones que se hayan enviado recientemente. Asegúrate de proporcionar toda la información que los evaluadores deben conocer en la página [Notas para la certificación](notes-for-certification.md).

Una vez completado el proceso de certificación, obtendrás un informe donde se indicará si la aplicación pasó o no la certificación. Si no la pasó, el informe indicará en qué prueba produjo errores o qué [directiva](store-policies.md) no se cumplió. Después de solucionar el problema, puedes crear un nuevo envío de la aplicación para volver a iniciar el proceso de certificación.

## <a name="release"></a>Release

Cuando la aplicación pasa la certificación, está lista para pasar al proceso de **publicación** .

- Si ha indicado que el envío debe publicarse lo antes posible (la opción predeterminada), el proceso de publicación comenzará de inmediato.
- Si es la primera vez que ha publicado la aplicación y ha especificado una fecha de **lanzamiento** en la sección [programación](configure-precise-release-scheduling.md#release) , la aplicación estará disponible según las selecciones de **fecha de lanzamiento** .
- Si ha usado [Opciones de suspensión de publicación](manage-submission-options.md#publishing-hold-options) para especificar que no se debe liberar hasta una fecha determinada, esperar hasta esa fecha para comenzar el proceso de publicación, a menos que seleccione **cambiar fecha de lanzamiento** .
- Si ha usado [las opciones de retención de publicación](manage-submission-options.md#publishing-hold-options) para especificar que desea publicar manualmente el envío, no comenzará el proceso de publicación hasta que seleccione **publicar ahora** (o seleccione **cambiar fecha de lanzamiento** y seleccionar una fecha específica).


## <a name="publishing"></a>Publicación

Los paquetes de la aplicación se firman digitalmente para protegerlos de alteraciones después de su lanzamiento. Una vez iniciada esta fase, ya no puedes cancelar el envío ni cambiar la fecha de lanzamiento.

En el caso de las nuevas aplicaciones y actualizaciones que incluyen cambios en los paquetes de la aplicación, el proceso de publicación se completará en un plazo de 24 horas. En el caso de las actualizaciones que solo cambian opciones como el almacenamiento de detalles de la lista, pero que no cambian los paquetes de la aplicación, el proceso de publicación tardará menos de una hora.

Mientras la aplicación se encuentra en la fase de publicación, el vínculo **Mostrar detalles** de la columna Estado del envío de la aplicación le permite saber cuándo los nuevos paquetes y los detalles de la lista de tiendas están disponibles para los clientes en cada una de las versiones de SO admitidas. Los pasos que no se hayan completado se mostrarán como **pendientes** . La aplicación permanecerá en la fase de publicación hasta que se complete el proceso, lo que significa que los nuevos paquetes y/o los detalles de la lista están disponibles para todos los clientes potenciales de la aplicación.

## <a name="in-the-store"></a>En la Tienda 

Tras completar los pasos anteriores, el estado del envío cambiará de **Publicación** a **En la Tienda** . El envío estará disponible en el Microsoft Store para que los clientes lo descarguen (a menos que haya elegido otra opción de [detección](choose-visibility-options.md#discoverability) ). 

> [!NOTE]
> También realizamos comprobaciones puntuales de las aplicaciones después de que se hayan publicado para que podamos identificar posibles problemas y asegurarse de que la aplicación cumple con todas las [directivas de Microsoft Store](store-policies.md). Si encontramos algún problema, se te notificará al respecto y sobre cómo solucionarlo, si corresponde, o bien sobre si la aplicación se ha quitado de la Tienda.

 

 

 




