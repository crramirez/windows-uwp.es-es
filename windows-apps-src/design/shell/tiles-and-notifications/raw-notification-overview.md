---
description: Obtenga información acerca de las notificaciones sin procesar, que son notificaciones de envío cortas y de uso general que son estrictamente instrucciones y no incluyen un componente de interfaz de usuario.
title: Introducción a las notificaciones sin procesar
ms.assetid: A867C75D-D16E-4AB5-8B44-614EEB9179C7
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6b01f961a28e3a6db52725c4f2f26dc3b3aaee9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172369"
---
# <a name="raw-notification-overview"></a>Introducción a las notificaciones sin procesar


Las notificaciones sin procesar son breves notificaciones de inserción de carácter general. Guardan un propósito estrictamente instructivo y no incluyen ningún componente de interfaz de usuario. Al igual que sucede con el resto de los tipos de notificaciones de inserción, la característica Servicios de notificaciones de inserción de Windows (WNS) entrega notificaciones sin procesar desde el servicio de nube a tu aplicación.

Puedes usar las notificaciones sin procesar con diversos fines, como activar la aplicación para que ejecute una tarea en segundo plano (si el usuario permite que la aplicación lleve esto a cabo). Si usas WNS para comunicarte con tu aplicación, evitarás la carga de procesamiento que se produce cuando se generan conexiones de socket persistentes, se envían mensajes HTTP GET y se establecen otras conexiones de servicio a aplicación.

> [!IMPORTANT]
> Para adquirir un mayor conocimiento sobre las notificaciones sin procesar, recomendamos que te familiarices con los conceptos que se abordan en el tema [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md).

 

Al igual que ocurre con las notificaciones de inserción de tipo notificación, notificación del sistema y notificación de icono, las notificaciones sin procesar se insertan desde el servicio de nube de tu aplicación en WNS a través de un identificador uniforme de recursos (URI) de canal asignado. A su vez, WNS entrega la notificación al dispositivo y la cuenta de usuario asociados a dicho canal. A diferencia del resto de las notificaciones de inserción, las notificaciones sin procesar carecen de un formato especificado. El contenido de carga se define totalmente según la aplicación.

Fijémonos en una aplicación de colaboración de documentos teóricos para ver un ejemplo de aplicación que puede sacar partido de las notificaciones sin procesar. Supongamos que hay dos usuarios que se encuentran modificando un documento al mismo tiempo. El servicio de nube donde se hospeda el documento compartido puede usar las notificaciones sin procesar para avisar a cada usuario de los cambios del otro. Estas notificaciones sin procesar no tienen por qué contener los cambios en el documento, sino indicar a la copia de la aplicación de cada usuario que se ponga en contacto con la ubicación central y sincronice los cambios disponibles. Gracias a las notificaciones sin procesar, la aplicación y el servicio de nube se ahorrarán la carga derivada de tener que mantener conexiones persistentes durante todo el tiempo en el que el documento esté abierto.

## <a name="how-raw-notifications-work"></a>Funcionamiento de las notificaciones sin procesar


Todas las notificaciones sin procesar son notificaciones de envío, de modo que la configuración necesaria para enviar y recibir notificaciones de envío también lo será para las notificaciones sin procesar:

-   Debes tener un canal de WNS válido para enviar notificaciones sin procesar. Para obtener más información sobre cómo adquirir un canal de notificaciones de envío, consulta [Cómo solicitar, crear y guardar un canal de notificaciones](/previous-versions/windows/apps/hh465412(v=win.10)).
-   Debes incluir la funcionalidad de **Internet** en el manifiesto de la aplicación. En el editor de manifiestos de Microsoft Visual Studio, encontrarás esta opción en la pestaña **Capacidades** como **Internet (cliente)**. Para obtener más información, vea [**capacidades**](/uwp/schemas/appxpackage/appxmanifestschema/element-capabilities).

El cuerpo de la notificación está en un formato definido por la aplicación. El cliente recibe los datos como una cadena terminada en null (**HSTRING**) que solo la aplicación conoce.

Si el cliente no está conectado, WNS almacenará las notificaciones sin procesar en la memoria caché, pero solo si la notificación incluye el encabezado [X-WNS-Cache-Policy](/previous-versions/windows/apps/hh465435(v=win.10)). Sin embargo, solo se almacenará y enviará una única notificación sin procesar cuando el dispositivo vuelva a estar conectado.

Pueden suceder tres cosas con una notificación sin procesar: que se envíe a la aplicación que se está ejecutando a través de un evento de entrega de notificaciones, que se envíe a una tarea en segundo plano o que se descarte. Si el cliente no está conectado y WNS trata de enviar una notificación sin procesar, esta se descartará.

## <a name="creating-a-raw-notification"></a>Crear una notificación sin procesar


Enviar una notificación sin procesar es lo mismo que enviar una notificación del sistema o de icono, pero con estas diferencias:

-   El encabezado HTTP Content-Type debe estar establecido en "application/octet-stream".
-   El encabezado HTTP [X-WNS-Type](/previous-versions/windows/apps/hh465435(v=win.10)) debe estar establecido en "wns/raw".
-   El cuerpo de la notificación puede contener cualquier carga de cadena que no supere los 5 KB de tamaño.

Las notificaciones sin procesar están pensadas para usarse como mensajes breves que activan la aplicación para que realice una acción, como ponerse en contacto directamente con el servicio para sincronizar una gran cantidad de datos o efectuar una modificación de estado local en función del contenido de la notificación. Ten en cuenta que no existe garantía de que las notificaciones de envío de WNS se entreguen, de modo que tu aplicación y tu servicio de nube deben contemplar la posibilidad de que la notificación sin procesar no llegue al cliente (si, por ejemplo, no está conectado).

Para obtener más información sobre cómo enviar notificaciones de envío, consulta [Inicio rápido: envío de una notificación de inserción](/previous-versions/windows/apps/hh868252(v=win.10)).

## <a name="receiving-a-raw-notification"></a>Recibir una notificación sin procesar


Tu aplicación puede recibir notificaciones sin procesar de dos modos:

-   A través de [eventos de entrega de notificaciones](#notification-delivery-events) mientras la aplicación se ejecuta.
-   A través de [tareas en segundo plano que la notificación sin procesar desencadena](#background-tasks-triggered-by-raw-notifications), en caso de que la aplicación pueda ejecutar tareas de este tipo.

Una aplicación puede recurrir a cualquiera de estos dos mecanismos para recibir notificaciones sin procesar. Si una aplicación implementa tanto el controlador de eventos de entrega de notificaciones como las tareas en segundo plano desencadenadas por las notificaciones sin procesar, y dicha aplicación se está ejecutando, tendrá prioridad el evento de entrega de notificaciones.

-   Si la aplicación se está ejecutando, el evento de entrega de notificaciones tendrá prioridad sobre la tarea en segundo plano y la aplicación tendrá la primera oportunidad de procesar la notificación.
-   El controlador de eventos de entrega de notificaciones puede establecer la propiedad [**PushNotificationReceivedEventArgs.Cancel**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationReceivedEventArgs.Cancel) del evento en **true**, para indicar que la notificación sin procesar no debe pasar a su tarea en segundo plano correspondiente cuando exista un controlador. Si la propiedad **Cancel** se establece en **false** o no se establece (el valor predeterminado es **false**), la notificación sin procesar desencadenará la tarea en segundo plano después de que el controlador de eventos de entrega de notificaciones haya completado su trabajo.

### <a name="notification-delivery-events"></a>Eventos de entrega de notificaciones

Tu aplicación puede usar un evento de entrega de notificaciones ([**PushNotificationReceived**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel.PushNotificationReceived)) para recibir notificaciones sin procesar mientras se esté usando la aplicación. Cuando el servicio de nube envía una notificación sin procesar, la aplicación en ejecución puede recibirla controlando el evento de entrega de notificaciones en el URI de canal.

Si la aplicación no se está ejecutando y no usa [tareas en segundo plano](#background-tasks-triggered-by-raw-notifications)), WNS quita cualquier notificación sin procesar enviada a esa aplicación. Si no quieres desperdiciar los recursos de tu servicio de nube, considera la posibilidad de implementar lógica en el servicio para saber si la aplicación está activa. Hay dos maneras de obtener esta información: una aplicación puede informar al servicio expresamente de que está lista para recibir notificaciones, o bien WNS puede indicar al servicio cuándo detenerse.

-   **La aplicación notifica al servicio de nube**: la aplicación puede ponerse en contacto con su servicio para avisarle de que se está ejecutando en primer plano. El inconveniente de este método reside en que la aplicación puede terminar poniéndose en contacto con el servicio con demasiada frecuencia, si bien reporta la ventaja de que el servicio siempre sabrá cuándo estará lista para recibir notificaciones sin procesar entrantes. Otra ventaja consiste en que, cuando la aplicación se pone en contacto con su servicio, este sabrá que puede enviar notificaciones sin procesar a la instancia específica de dicha aplicación, en lugar de difundirlas.
-   **El servicio de nube responde a los mensajes de respuesta de WNS**: el servicio de tu aplicación puede usar la información de [X-WNS-NotificationStatus](/previous-versions/windows/apps/hh465435(v=win.10)) y [X-WNS-DeviceConnectionStatus](/previous-versions/windows/apps/hh465435(v=win.10)) que WNS devuelve para saber cuándo dejar de enviar notificaciones sin procesar a la aplicación. Cuando el servicio envía una notificación a un canal a modo de HTTP POST, recibirá uno de los siguientes mensajes en la respuesta:

    -   **X-WNS-NotificationStatus: dropped**: indica que el cliente no recibió la notificación. Se puede decir con certeza que la respuesta **dropped** viene provocada por el hecho de que la aplicación ya no se ejecuta en primer plano en el dispositivo del usuario.
    -   **X-WNS-DeviceConnectionStatus: disconnected** o **X-WNS-DeviceConnectionStatus: tempconnected**: indica que el cliente de Windows ya no está conectado a WNS. Observa que, para recibir este mensaje de WNS, tienes que solicitarlo expresamente definiendo el encabezado [X-WNS-RequestForStatus](/previous-versions/windows/apps/hh465435(v=win.10)) en el HTTP POST de la notificación.

    El servicio de nube de tu aplicación puede usar la información de estos mensajes de estado para cesar sus intentos de comunicarse mediante notificaciones sin procesar y reanudará su envío cuando la aplicación vuelva a ejecutarse en primer plano y se ponga en contacto con él.

    Ten en cuenta que no debes basarte en [X-WNS-NotificationStatus](/previous-versions/windows/apps/hh465435(v=win.10)) para averiguar si la notificación se ha entregado al cliente correctamente.

    Si quieres obtener más información, consulta [Encabezados de respuesta y solicitud del servicio de notificaciones de inserción](/previous-versions/windows/apps/hh465435(v=win.10)).

### <a name="background-tasks-triggered-by-raw-notifications"></a>Tareas en segundo plano desencadenadas por las notificaciones sin procesar

> [!IMPORTANT]
> Antes de usar tareas en segundo plano de notificaciones sin procesar, la aplicación debe tener acceso en segundo plano a través de [**BackgroundExecutionManager.RequestAccessAsync**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_).

 

La tarea en segundo plano se debe registrar con un [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger); de lo contrario, dicha tarea no se ejecutará cuando se reciba una notificación sin procesar.

Una tarea en segundo plano desencadenada por una notificación sin procesar permite que el servicio de nube de tu aplicación se ponga en contacto con la aplicación, incluso cuando la aplicación no se esté ejecutando (aunque puede hacer que se ejecute). Esto sucede sin que la aplicación tenga que mantener una conexión continua. Las notificaciones sin procesar son las únicas que pueden desencadenar tareas en segundo plano. Además, aunque las notificaciones de inserción de tipo notificación, notificación del sistema o notificación de icono no puedan desencadenar tareas en segundo plano, las tareas en segundo plano desencadenadas por las notificaciones sin procesar sí que pueden actualizar iconos e invocar notificaciones del sistema a través de llamadas a la API local.

Pensemos en una aplicación que se usa para leer libros electrónicos con el fin de entender el funcionamiento de las tareas en segundo plano desencadenadas por las notificaciones sin procesar. En primer lugar, un usuario compra un libro en línea, probablemente en otro dispositivo. En respuesta, el servicio de nube de la aplicación puede enviar una notificación sin procesar a cada uno de los dispositivos del usuario, con una carga que indica que se adquirió el libro y la aplicación debe descargarlo. Tras ello, la aplicación se pone en contacto directamente con el servicio de nube de la aplicación para iniciar la descarga en segundo plano del nuevo libro a fin de que, más adelante, cuando el usuario abra la aplicación, encuentre el libro ya listo para leerlo.

Para usar una notificación sin procesar para desencadenar una tarea en segundo plano, tu aplicación debe hacer lo siguiente:

1.  Solicitar permiso para ejecutar tareas en segundo plano (que el usuario puede revocar en cualquier momento) con [**BackgroundExecutionManager.RequestAccessAsync**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_).
2.  Implementar la tarea en segundo plano. Para obtener más información, consulte [compatibilidad con la aplicación con tareas en segundo plano](../../../launch-resume/support-your-app-with-background-tasks.md) .

A continuación se invoca la tarea en segundo plano en respuesta a [**PushNotificationTrigger**](/uwp/api/Windows.ApplicationModel.Background.PushNotificationTrigger) cada vez que se reciba una notificación sin procesar relativa a la aplicación. Esta tarea en segundo plano interpreta la carga específica para la aplicación de la notificación sin procesar y actúa en consecuencia.

Solo se puede ejecutar una tarea en segundo plano en una aplicación cada vez. En caso de que se desencadene una tarea en segundo plano relativa a una aplicación para la que ya hay una tarea ejecutándose, la primera deberá finalizar para que la siguiente comience.

## <a name="other-resources"></a>Otros recursos


Para obtener más información, descarga el [Ejemplo de notificaciones sin procesar](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Raw%20notifications%20sample%20(Windows%208)) para Windows 8.1 y el [Ejemplo de notificaciones de inserción y periódicas](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) para Windows 8.1 y vuelve a usar su código fuente en la aplicación de Windows 10.

## <a name="related-topics"></a>Temas relacionados

* [Instrucciones para notificaciones sin procesar]()
* [Inicio rápido: Crear y registrar una tarea en segundo plano de notificación sin procesar](/previous-versions/windows/apps/jj676800(v=win.10))
* [Inicio rápido: interceptar notificaciones de inserción para aplicaciones en ejecución](/previous-versions/windows/apps/jj709908(v=win.10))
* [**RawNotification**](/uwp/api/Windows.Networking.PushNotifications.RawNotification)
* [**BackgroundExecutionManager.RequestAccessAsync**](/uwp/api/Windows.ApplicationModel.Background.BackgroundExecutionManager#Windows_ApplicationModel_Background_BackgroundExecutionManager_RequestAccessAsync_System_String_)
 

 