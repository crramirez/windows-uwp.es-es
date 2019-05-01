---
Description: Cuando termine de crear el envío de la aplicación y haga clic en Enviar para el Store, el envío entra en el paso de certificación.
title: Proceso de certificación de la aplicación
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, publicar, preprocesamiento, certificación, liberar, pendientes, enviar, publicar, estado, tiempo
ms.localizationpriority: medium
ms.openlocfilehash: fe9df9ce95c6b17bcd3d702bf09ac57b9f205e0c
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63790585"
---
# <a name="the-app-certification-process"></a>Proceso de certificación de la aplicación

Cuando termines de crear el envío de la aplicación y hagas clic en **Enviar a Store**, el envío entra en el paso de certificación. Normalmente este proceso se completa en unas horas, aunque en algunos casos puede tardar hasta tres días laborables. Después de su envío pasa la certificación, puede tardar hasta 24 horas para los clientes vean la lista de la aplicación para un nuevo envío o para un envío actualizado con los cambios a los paquetes. Si la actualización cambia solo Store detalles del anuncio, se completará el proceso de publicación en menos de una hora.  Se le notificará cuando se publica su envío y estado de la aplicación en el panel será **en el Store**.

## <a name="preprocessing"></a>Preprocesamiento

Después de cargar los paquetes de la aplicación correctamente y enviar la aplicación para su certificación, los paquetes se ponen en cola para someterse a pruebas. Se mostrará un mensaje si se detectan los errores durante el preprocesamiento. Para obtener más información sobre posibles errores, consulta [Resolver errores de envío](resolve-submission-errors.md).

## <a name="certification"></a>Certification

Durante esta fase, se llevan a cabo varias pruebas:

-   **Pruebas de seguridad:** Esta primera prueba comprueba los paquetes de la aplicación para detectar virus y malware. Si la aplicación no pasa esta prueba, necesitarás comprobar el sistema de desarrollo ejecutando el software antivirus más reciente y luego recompilar el paquete de la aplicación en un sistema limpio.
-   **Pruebas de compatibilidad técnica:** Compatibilidad técnica se prueba mediante el Kit de certificación de aplicaciones de Windows. (Debes asegurarte siempre de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviarla a la Tienda).
-   **Cumplimiento de contenidos:** La cantidad de tiempo que tarde varía según la complejidad de la aplicación es, cuánto contenido visual tiene y cuántas aplicaciones se han enviado recientemente. Asegúrate de proporcionar toda la información que los evaluadores deben conocer en la página [Notas para la certificación](notes-for-certification.md).

Una vez completado el proceso de certificación, obtendrás un informe donde se indicará si la aplicación pasó o no la certificación. Si no la pasó, el informe indicará en qué prueba produjo errores o qué [directiva](https://docs.microsoft.com/legal/windows/agreements/store-policies) no se cumplió. Después de solucionar el problema, puedes crear un nuevo envío de la aplicación para volver a iniciar el proceso de certificación.

## <a name="release"></a>Publicación

Cuando su aplicación pasa de certificación, está listo para pasar a la **publicación** proceso.

- Si ha indicado que se debe publicar su envío tan pronto como sea posible (opción predeterminada), se iniciará inmediatamente el proceso de publicación.
- Si se trata de la primera vez que ha publicado la aplicación, y se especificó un **fecha de lanzamiento** en el [programación](configure-precise-release-scheduling.md#release) sección, la aplicación estará disponible conforme a su **fechadelanzamiento**selecciones.
- Si ha usado [publicación mantenga opciones](manage-submission-options.md#publishing-hold-options) para especificar que no se deben liberar hasta una fecha determinada, esperaremos hasta esa fecha para comenzar el proceso de publicación, a menos que seleccione **fecha de cambio de versión**.
- Si ha usado [publicación mantenga opciones](manage-submission-options.md#publishing-hold-options) para especificar que desea publicar el envío manualmente, no se iniciará el proceso de publicación hasta que seleccione **publicar ahora** (o seleccione **cambio fecha de lanzamiento** y elegir una fecha concreta).


## <a name="publishing"></a>Publishing

Los paquetes de la aplicación se firman digitalmente para protegerlos de alteraciones después de su lanzamiento. Una vez iniciada esta fase, ya no puedes cancelar el envío ni cambiar la fecha de lanzamiento.

Para nuevas aplicaciones y actualizaciones que se incluyen los cambios realizados en los paquetes de la aplicación, se completará el proceso de publicación en 24 horas. Para las actualizaciones que solo cambie opciones como los detalles del anuncio de Store, pero no cambian los paquetes de la aplicación, el proceso de publicación tardará menos de una hora.

Mientras la aplicación está en la fase de publicación, el **mostrar detalles** vincular en la columna Estado de envío de la aplicación le permite saber cuándo están disponibles para los clientes en cada uno de su sistema operativo compatible con los nuevos paquetes y Store detalles del anuncio versiones. Los pasos que no se hayan completado llevarán la indicación **Pendiente**. La aplicación permanecerá en la fase de publicación hasta que se ha completado el proceso, lo que significa que los nuevos paquetes o detalles del anuncio están disponibles para todos los clientes potenciales de la aplicación.

## <a name="in-the-store"></a>En la Tienda 

Tras completar los pasos anteriores, el estado del envío cambiará de **Publicación** a **En la Tienda**. Entonces, tu envío estará disponible en Microsoft Store para que lo descarguen los clientes (a menos que hayas elegido otra opción de [Visibilidad](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> También se realizan comprobaciones puntuales de aplicaciones después de haberse publicado, para poder identificar posibles problemas y garantizar que la aplicación cumple con todas las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Si encontramos algún problema, se te notificará al respecto y sobre cómo solucionarlo, si corresponde, o bien sobre si la aplicación se ha quitado de la Tienda.

 

 

 




