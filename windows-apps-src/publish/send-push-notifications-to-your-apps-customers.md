---
description: Obtenga información sobre cómo enviar notificaciones del centro de partners a la aplicación para animar a los grupos de clientes a realizar una acción, como clasificar una aplicación o comprar un complemento.
title: Enviar notificaciones push dirigidas a los clientes de la aplicación
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, notificaciones de destino, notificaciones de envío, notificación del sistema, icono
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: medium
ms.openlocfilehash: 3233dcee13b103978e2aa85181c8aa8a54e0b55c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034598"
---
# <a name="send-notifications-to-your-apps-customers"></a>Enviar notificaciones a los clientes de la aplicación

Interactuar con los clientes en el momento justo y con el mensaje adecuado es clave para lograr tener éxito como desarrollador de aplicaciones. Las notificaciones pueden animar a los clientes a realizar una acción, como clasificar una aplicación, comprar un complemento, probar una nueva característica o descargar otra aplicación (quizás gratis con un [código promocional](generate-promotional-codes.md) que proporcione).

El [centro de Partners](https://partner.microsoft.com/dashboard) proporciona una plataforma de interacción con los clientes controlada por datos que puede usar para enviar notificaciones a todos los clientes de la aplicación o solo a un subconjunto de los clientes de Windows 10 de su aplicación que cumplen los criterios que ha definido en un segmento de [cliente](create-customer-segments.md). También puede crear una notificación que se enviará a los clientes de más de una de las aplicaciones.

> [!IMPORTANT]
> Estas notificaciones solo se pueden usar con aplicaciones de UWP.

Al considerar el contenido de las notificaciones, tenga en cuenta lo siguiente:
- El contenido de las notificaciones debe cumplir las directivas de [contenido](/legal/windows/agreements/store-policies#content_policies)de la tienda.
- El contenido de la notificación no debe incluir información confidencial o potencialmente confidencial.
- Aunque haremos todo lo posible por entregar la notificación según lo programado, en ocasiones puede haber problemas de latencia que afecten a la entrega.
- Asegúrese de no enviar notificaciones con demasiada frecuencia. Más de una vez cada 30 minutos puede parecer intrusivo (y en muchos escenarios, con menos frecuencia de lo que es preferible).
- Tenga en cuenta que si un cliente que usa la aplicación (y ha iniciado sesión con su cuenta de Microsoft en el momento en que se determina la pertenencia al segmento), más adelante asigna su dispositivo a alguien para que lo use, es posible que la otra persona vea la notificación destinada al cliente original. Para obtener más información, consulte [configuración de la aplicación para notificaciones de envío de destino](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).
- Si envía la misma notificación a los clientes de varias aplicaciones, no puede destinarse a un segmento. la notificación se enviará a todos los clientes de las aplicaciones que seleccione.


## <a name="getting-started-with-notifications"></a>Introducción a las notificaciones

En un nivel alto, debe hacer tres cosas para usar las notificaciones para interactuar con los clientes.

1. **Registra la aplicación para recibir notificaciones push.** Para ello, agregue una referencia al SDK de Microsoft Store Services en la aplicación y, a continuación, agregue algunas líneas de código que registren un canal de notificación entre el centro de Partners y la aplicación. Usaremos ese canal para enviar las notificaciones a los clientes. Para obtener más información, consulte [configuración de la aplicación para notificaciones de envío de destino](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Decida a qué clientes desea dirigirse.** Puede enviar su notificación a todos los clientes de la aplicación o (para las notificaciones creadas para una sola aplicación) a un grupo de clientes denominado *segmento* , que puede definir en función de los criterios demográficos o de ingresos. Para obtener más información, consulta [Crear segmentos de clientes](create-customer-segments.md).
3. **Cree el contenido de la notificación y envíelo.** Por ejemplo, puede crear una notificación que fomente a los nuevos clientes para clasificar la aplicación o enviar una notificación que fomente un trato especial para comprar un complemento.


## <a name="to-create-and-send-a-notification"></a>Para crear y enviar una notificación

Siga estos pasos para crear una notificación en el centro de Partners y enviarla a un segmento de cliente determinado.

> [!NOTE]
> Para que una aplicación pueda recibir notificaciones del centro de Partners, primero debe llamar al método [RegisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) en la aplicación para registrar la aplicación para recibir notificaciones. Este método está disponible en el [SDK de Microsoft Store Services](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). Para obtener más información sobre cómo llamar a este método, incluido un ejemplo de código, consulte [configuración de la aplicación para notificaciones de envío de destino](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1. En el [centro de Partners](https://partner.microsoft.com/dashboard), expanda la sección **participar** y, a continuación, seleccione **notificaciones** .
2. En la página **notificaciones** , seleccione **nueva notificación** .
3. En la sección **seleccionar una plantilla** , elija el [tipo de notificación](#notification-template-types) que desea enviar y, a continuación, haga clic en **Aceptar** .
4. En la página siguiente, use el menú desplegable para elegir **una o** **varias aplicaciones** para las que desea generar una notificación. Solo puede seleccionar las aplicaciones que se han [configurado para recibir notificaciones mediante el SDK de Microsoft Store Services](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
5. En la sección **configuración de notificaciones** , elija un **nombre** para la notificación y, si procede, elija el **grupo de clientes** al que desea enviar la notificación. (Las notificaciones enviadas a varias aplicaciones solo pueden enviarse a todos los clientes de esas aplicaciones). Si desea usar un segmento que no ha creado ya, seleccione **crear nuevo grupo de clientes** . Tenga en cuenta que se tardan 24 horas antes de poder usar un nuevo segmento para las notificaciones. Para obtener más información, consulta [Crear segmentos de clientes](create-customer-segments.md).
6. Si desea especificar Cuándo se debe enviar la notificación, desactive la casilla **Enviar notificación inmediatamente** y elija una fecha y hora específicas (en UTC para todos los clientes, a menos que especifique para usar la zona horaria local de cada cliente).
7. Si desea que la notificación expire en algún momento, desactive la casilla la **notificación nunca expira** y elija una fecha y hora de expiración específicas (en UTC).
8. **Para notificaciones a una sola aplicación:** Si desea filtrar los destinatarios para que la notificación solo se entregue a personas que usan determinados idiomas o que se encuentran en zonas horarias específicas, active la casilla **usar filtros** . Después, puede especificar las opciones de idioma y/o zona horaria que desea usar.
8. **Para notificaciones a varias aplicaciones:** Especifique si desea enviar la notificación solo a la última aplicación activa en cada dispositivo (por cliente) o a todas las aplicaciones de cada dispositivo.
10. En la sección **Notification content (Contenido de la notificación)** y en el menú **Language (Idioma)** , elige los idiomas en los que quieres que se muestre la notificación. Para obtener más información, consulta [Traducir las notificaciones)](#translate-your-notifications).
11. En la sección **Options (Opciones)** , escribe un texto y configura las opciones que quieras. Si has usado alguna plantilla, algunas de estas se proporcionarán de manera predeterminada, pero puedes realizar los cambios que quieras.

    Las opciones disponibles varían según el tipo de notificación que uses. Estas son algunas de las opciones:

    * **Activation type (Tipo de activación)** (tipo de notificación del sistema interactiva). Puedes elegir **Foreground (Primer plano)** , **Background (Segundo plano)** o **Protocol (Protocolo)** .
    * **Launch (Iniciar)** (tipo de notificación del sistema interactiva). Puedes elegir que la notificación abra una aplicación o un sitio web.
    * **Track app launch rate (Seguir tasa de inicio de la aplicación)** (tipo de notificación del sistema interactiva). Si quieres medir el rendimiento de tu interacción con los clientes a través de cada notificación, selecciona esta casilla. Para obtener más información, consulta [Medir el rendimiento de las notificaciones](#measure-notification-performance).
    * **Duration (Duración)** (tipo de notificación del sistema interactiva). Puedes elegir **Short (Breve)** o **Long (Larga)** .
    * **Scenario (Escenario)** (tipo de notificación del sistema interactiva). Puedes elegir **Default (Predeterminado)** , **Alarm (Alarma)** , **Reminder (Aviso)** o **Incoming call (Llamada entrante)** .
    * **Base URI (URI base)** (tipo de notificación del sistema interactiva). Para obtener más información, consulta [BaseUri](/uwp/api/windows.ui.xaml.frameworkelement.baseuri#Windows_UI_Xaml_FrameworkElement_BaseUri).
    * **Add image query (Agregar consulta de imagen)** (tipo de notificación del sistema interactiva). Para obtener más información, consulta [addImageQuery](/uwp/schemas/tiles/toastschema/element-visual#attributes-and-elements).
    * **Objetos visuales** . Una imagen, un vídeo o un sonido. Para obtener más información, consulta [visual](/uwp/schemas/tiles/toastschema/element-visual).
    * **Entrada** / de **Acción** / de **Selección** (tipo de notificación interactiva). De esta forma puedes permitir a los usuarios interactuar con la notificación. Para obtener más información, consulte [notificaciones del sistema interactivas y adaptables](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).
    * **Binding (Enlace)** (tipo de icono interactivo). La plantilla de la notificación del sistema. Para obtener más información, consulta [binding](/uwp/schemas/tiles/toastschema/element-binding).

    > [!TIP]
    > Pruebe a usar la aplicación del [visualizador de notificaciones](https://www.microsoft.com/store/apps/9nblggh5xsl1) para diseñar y probar los iconos adaptables y las notificaciones del sistema interactivas.

12. Selecciona **Save as draft (Guardar como borrador)** para seguir trabajando más adelante en la notificación, o **Send (Enviar)** si ya has terminado.


## <a name="notification-template-types"></a>Tipos de plantilla de notificación

Puedes elegir entre una variedad de plantillas de notificación.

-   **Blank (En blanco) (notificación del sistema).** Empieza con una notificación del sistema vacía que puedes personalizar. Una notificación del sistema es una interfaz de usuario emergente que se muestra en pantalla y permite que la aplicación se comunique con el cliente cuando este se encuentra en otra aplicación, en la pantalla Inicio o en el escritorio.
-   **En blanco (icono).** Empieza con una notificación de icono vacía que puedes personalizar. Los iconos son una representación de la aplicación en la pantalla Inicio. Los iconos pueden ser "dinámicos", lo que significa que el contenido que muestran puede cambiar en las respuestas a las notificaciones.
-   **Ask for ratings (Pedir calificaciones) (notificación del sistema).** Una notificación del sistema que pide a los clientes que califiquen la aplicación. Cuando el cliente selecciona la notificación, se muestra la página de calificaciones de la Tienda de tu aplicación.
-   **Ask for feedback (Pedir comentarios) (notificación del sistema).** Una notificación del sistema que pide a los clientes que escriban un comentario de la aplicación. Cuando el cliente selecciona la notificación, se muestra la página del Centro de opiniones de tu aplicación.
    > [!NOTE]
    > Si elige este tipo de plantilla, en el cuadro **Launch (iniciar** ), recuerde reemplazar el valor de marcador de posición {PACKAGE_FAMILY_NAME} por el nombre de familia de paquete (PFN) real de la aplicación. Encontrarás el PFN de tu aplicación en la página [App identity (Identidad de la aplicación)](view-app-identity-details.md) ( **App management [Administración de la aplicación]** > **App identity [Identidad de la aplicación]** ).

    ![Cuadro "Launch" de la notificación del sistema de solicitud de comentarios](images/push-notifications-feedback-toast-launch-box.png)

-   **Cross-promote (Promocionar de manera cruzada) (notificación del sistema).** Una notificación del sistema para promocionar otra aplicación de tu elección. Cuando el cliente selecciona la notificación, se muestra la descripción de la Tienda de la otra aplicación.
    > [!NOTE]
    > Si elige este tipo de plantilla, en el cuadro **Launch (iniciar** ), recuerde reemplazar el valor de marcador de posición **{ProductID que desea promocionar aquí}** por el ID. de almacén real del elemento que desea promover. Encontrarás el identificador de la Tienda en la página [App identity (Identidad de la aplicación)](view-app-identity-details.md) ( **App management [Administración de la aplicación]** > **App identity [Identidad de la aplicación]** ).

    ![Cuadro "Launch" de la notificación del sistema de promoción cruzada](images/push-notifications-promote-toast-launch-box.png)

-   **Promote a sale (Promocionar una oferta) (notificación del sistema).** Una notificación del sistema que puedes usar para anunciar una oferta de tu aplicación. Cuando el cliente selecciona la notificación, se muestra la descripción de la Tienda de tu aplicación.
-   **Prompt for update (Solicitar actualización) (notificación del sistema).** Una notificación del sistema que recomienda a los clientes con una versión anterior de la aplicación que instalen la última versión. Cuando el cliente seleccione la notificación, se iniciará la aplicación de la tienda y se mostrará la lista de **descargas y actualizaciones** . Tenga en cuenta que esta plantilla solo se puede usar con una sola aplicación y no puede tener como destino un segmento de cliente concreto o definir una hora para enviarla. siempre se programará esta notificación para que se envíe en un plazo de 24 horas, y se realizará el mejor esfuerzo para dirigirse a todos los usuarios que todavía no ejecuten la versión más reciente de la aplicación.


## <a name="measure-notification-performance"></a>Medir el rendimiento de las notificaciones

Puedes medir cuál es el rendimiento de tu interacción con los clientes mediante cada notificación.


### <a name="to-measure-notification-performance"></a>Medir el rendimiento de las notificaciones

1.  Cuando crees una notificación, en la sección **Notification content (Contenido de la notificación)** , selecciona la casilla **Track app launch rate (Seguir tasa de inicio de la aplicación)** .
2.  En la aplicación, llame al método [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) para notificar al centro de partners que la aplicación se inició en respuesta a una notificación de destino. Este método lo proporciona el SDK de Microsoft Store Services. Para obtener más información sobre cómo llamar a este método, consulte [configuración de la aplicación para recibir notificaciones del centro de Partners](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


### <a name="to-view-notification-performance"></a>Ver el rendimiento de una notificación

Cuando haya configurado la notificación y la aplicación para medir el rendimiento de las notificaciones, tal y como se ha descrito anteriormente, puede ver el rendimiento de las notificaciones.

Para revisar los datos detallados de cada notificación:

1.  En el centro de Partners, expanda la sección **participar** y seleccione **notificaciones** .
2.  En la tabla de notificaciones existentes, seleccione **en curso** o **completado** y, a continuación, examine las columnas **tasa de entrega** y **frecuencia de inicio** de la aplicación para ver el rendimiento de alto nivel de cada notificación.
3.  Para ver datos de rendimiento más detallados, selecciona el nombre de una notificación. En la sección **estadísticas de entrega** , puede ver el **recuento** y la información **porcentual** de los siguientes tipos de **Estado** de notificación:
    * **Failed (Erróneo)** : la notificación no se ha entregado por algún motivo. Esto puede suceder, por ejemplo, si se produce algún problema en el servicio de notificaciones de Windows.
    * **Error de expiración del canal** : no se pudo entregar la notificación porque el canal entre la aplicación y el centro de Partners ha expirado. Por ejemplo, esto puede suceder si el cliente no abre la aplicación en mucho tiempo.
    * **Sending (Enviando)** : la notificación está en la cola de envío.
    * **Sent (Enviada)** : la notificación se ha enviado.
    * **Launched (Iniciada)** : se ha enviado la notificación, el cliente ha hecho clic en ella y, como resultado, se ha abierto la aplicación. Ten en cuenta solo se realiza el seguimiento de los inicios de las aplicaciones. Las notificaciones que invitan al cliente a realizar otras acciones, como iniciar la Tienda para dejar una calificación, no se incluyen en este estado.
    * **Unknown (Desconocido)** : no se ha podido determinar el estado de esta notificación.

Para analizar los datos de actividad de los usuarios de todas las notificaciones:

1.  En el centro de Partners, expanda la sección **participar** y seleccione **notificaciones** .
2.  En la página **notificaciones** , haga clic en la pestaña **analizar** . Esta pestaña muestra los datos siguientes:
    * Vistas de gráficos de los distintos Estados de acción del usuario para las notificaciones del centro de actividades y del sistema.
    * Vistas del mapa del mundo de las tarifas de clic a través de las notificaciones del centro de actividades y del sistema.
3. Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es 30D (30 días), pero puede optar por mostrar los datos de 3, 6 o 12 meses, o de un intervalo de datos personalizado que especifique. También puede expandir **filtros** para filtrar todos los datos por aplicación y mercado.

## <a name="translate-your-notifications"></a>Traducir las notificaciones

Para maximizar el efecto de las notificaciones, plantéate la posibilidad de traducirlas a los idiomas que tus clientes prefieren. El centro de Partners facilita la traducción automática de las notificaciones aprovechando el potencial del servicio [Microsoft Translator](https://www.microsoft.com/translator/home.aspx) .

1.  Cuando hayas escrito la notificación en tu idioma predeterminado, selecciona **Add languages (Agregar idiomas)** (debajo del menú **Languages [Idiomas]** de la sección **Notification content [Contenido de la notificación]** ).
2.  En la ventana **Add languages (Agregar idiomas)** , selecciona los idiomas adicionales en los que quieres que se muestren tus notificaciones y luego selecciona **Update (Actualizar)** .
La notificación se traducirá automáticamente a los idiomas que hayas elegido en la ventana **Add languages (Agregar idiomas)** ; esos idiomas se agregarán al menú **Language (Idioma)** .
3.  Para ver la traducción de la notificación, selecciona el idioma que has agregado en el menú **Language (Idioma)** .

Debes tener lo siguiente en cuenta sobre las traducciones:
 - Puedes modificar la traducción automática escribiendo en el cuadro **Content (Contenido)** del idioma que corresponda.
 - Si agregas otro cuadro de texto a la versión en inglés de la notificación después de modificar una traducción automática, el nuevo cuadro de texto no se agregará a la notificación traducida. En ese caso, deberás agregar manualmente los nuevos cuadros de texto a cada una de las notificaciones traducidas.
 - Si cambias el texto en inglés después de que la notificación se haya traducido, actualizaremos automáticamente las notificaciones traducidas para que reflejen el cambio. Sin embargo, esto no sucede si previamente has decidido modificar la traducción original.

## <a name="related-topics"></a>Temas relacionados
- [Iconos de aplicaciones para UWP](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [Aplicación Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [Método StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync)
