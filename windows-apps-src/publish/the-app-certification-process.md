---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: Proceso de certificación de la aplicación
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, publicación, preprocesamiento, certificación, liberar, pendientes, enviar, publicar, estado, tiempo
ms.localizationpriority: medium
ms.openlocfilehash: 8372f316786d83d72dff8ef7a0a8fd53e5390743
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "2811014"
---
# <a name="the-app-certification-process"></a>Proceso de certificación de la aplicación

Cuando termines de crear el envío de la aplicación y hagas clic en **Enviar a Store**, el envío entra en el paso de certificación. Normalmente este proceso se completa en unas horas, aunque en algunos casos puede tardar hasta tres días laborables. Después de que su envío pase certificación, puede tardar hasta 24 horas para que los clientes vean el listado de la aplicación para una nueva presentación, o para una presentación actualizada con los cambios a los paquetes. Si la actualización sólo cambia la lista de detalles de la tienda, el proceso de publicación se completará en menos de una hora.  Se le notificará cuando se publica su envío y estado de la aplicación en el panel estará **En el almacén**.

## <a name="preprocessing"></a>Preprocesamiento

Después de cargar los paquetes de la aplicación correctamente y enviar la aplicación para su certificación, los paquetes se ponen en cola para someterse a pruebas. Se mostrará un mensaje si se detectan los errores durante el preprocesamiento. Para obtener más información sobre posibles errores, consulta [Resolver errores de envío](resolve-submission-errors.md).

## <a name="certification"></a>Certificación

Durante esta fase, se llevan a cabo varias pruebas:

-   **Pruebas de seguridad:** la primera prueba comprueba si hay virus y malware en los paquetes de la aplicación. Si la aplicación no pasa esta prueba, necesitarás comprobar el sistema de desarrollo ejecutando el software antivirus más reciente y luego recompilar el paquete de la aplicación en un sistema limpio.
-   **Pruebas de cumplimiento técnico:** el cumplimiento de los requisitos técnicos se prueba con el Kit para la certificación de aplicaciones en Windows. (Debes asegurarte siempre de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviarla a la Tienda).
-   **Cumplimiento de requisitos de contenido:** el tiempo que tarda depende de la complejidad de la aplicación, la cantidad de contenido visual que tenga y la cantidad de aplicaciones que se hayan enviado recientemente. Asegúrate de proporcionar toda la información que los evaluadores deben conocer en la página [Notas para la certificación](notes-for-certification.md).

Una vez completado el proceso de certificación, obtendrás un informe donde se indicará si la aplicación pasó o no la certificación. Si no la pasó, el informe indicará en qué prueba produjo errores o qué [directiva](https://docs.microsoft.com/legal/windows/agreements/store-policies) no se cumplió. Después de solucionar el problema, puedes crear un nuevo envío de la aplicación para volver a iniciar el proceso de certificación.

## <a name="release"></a>Versión

Cuando la aplicación pasa certificación, está preparado para pasar al proceso de **publicación** .

- Si ha indicado que se debe publicar su envío tan pronto como sea posible (opción predeterminada), el proceso de publicación comenzará inmediatamente.
- Si se trata de la primera vez que ha publicado la aplicación y especifica una **fecha de lanzamiento** en la sección [programación](configure-precise-release-scheduling.md#release) , la aplicación se convertirán en disponible en función de las selecciones de **fecha de lanzamiento** .
- Si ha utilizado la [publicación mantenga las opciones](manage-submission-options.md#publishing-hold-options) para especificar que no deben ser liberar hasta una fecha determinada, se debe esperar hasta esa fecha para comenzar el proceso de publicación, a menos que seleccione la **fecha de lanzamiento de cambio**.
- Si ha utilizado la [publicación mantenga las opciones](manage-submission-options.md#publishing-hold-options) para especificar que desea publicar la presentación manualmente, se no se inicia el proceso de publicación hasta que seleccione **Publicar ahora** (o seleccione la **fecha de lanzamiento de cambio** y seleccionar una fecha específica).


## <a name="publishing"></a>Publicación

Los paquetes de la aplicación se firman digitalmente para protegerlos de alteraciones después de su lanzamiento. Una vez iniciada esta fase, ya no puedes cancelar el envío ni cambiar la fecha de lanzamiento.

Para nuevas aplicaciones y actualizaciones que se incluyen los cambios realizados en los paquetes de la aplicación, el proceso de publicación se completará dentro de 24 horas. Para las actualizaciones que sólo cambiar opciones como almacén de detalles del anuncio, pero no cambian los paquetes de la aplicación, el proceso de publicación tardará menos de una hora.

Mientras la aplicación se encuentra en la fase de publicación, el vínculo **Mostrar detalles** en la columna Estado para el envío de la aplicación le permite saber cuándo están disponibles para los clientes en cada uno de sus versiones compatibles del sistema operativo los nuevos paquetes y almacén de detalles del anuncio. Los pasos que no se hayan completado llevarán la indicación **Pendiente**. La aplicación permanecerá en la fase de publicación hasta que haya finalizado el proceso, lo que significa que los nuevos paquetes o detalles del anuncio están disponibles para todos los clientes potenciales de su aplicación.

## <a name="in-the-store"></a>En Store 

Tras completar satisfactoriamente los pasos anteriores, el estado del envío cambiará de **Publicación** a **En Store**. Entonces, tu envío estará disponible en Microsoft Store para que lo descarguen los clientes (a menos que hayas elegido otra opción de [Visibilidad](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> También se realizan comprobaciones puntuales de aplicaciones después de haberse publicado, para poder identificar posibles problemas y garantizar que la aplicación cumple con todas las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Si encontramos algún problema, se te notificará al respecto y sobre cómo solucionarlo, si corresponde, o bien sobre si la aplicación se ha quitado de la Tienda.

 

 

 




