---
author: JnHs
Description: Learn how to send notifications from Windows Dev Center to your app to encourage groups of customers to take an action, such as rating an app or buying an add-on.
title: Enviar notificaciones push dirigidas a los clientes de la aplicación
ms.author: wdg-dev-content
ms.date: 02/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, notificaciones dirigidas, push dirigidas, notificaciones push, notificación del sistema, ventana
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
ms.localizationpriority: high
ms.openlocfilehash: f7b4558b6c5ea2cc9dbc30cb605f1cb06432504f
ms.sourcegitcommit: d780e3a087ab5240ea643346480a1427bea9e29b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2018
---
# <a name="send-notifications-to-your-apps-customers"></a>Enviar notificaciones a los clientes de la aplicación

Interactuar con los clientes en el momento justo y con el mensaje adecuado resulta clave para lograr tener éxito como desarrollador de aplicaciones. Las notificaciones pueden animar a los clientes a realizar una acción, como calificar una aplicación, comprar un complemento, probar una nueva característica o descargar otra aplicación (quizá gratis con un [código de promoción](generate-promotional-codes.md) que proporciones).

El Centro de desarrollo de Windows ofrece una plataforma de participación basada en datos para los clientes que puedes usar para enviar notificaciones a todos los clientes de la aplicación o solo a un subconjunto de los clientes de Windows 10 de la aplicación que cumplan los criterios que hayas definido en un [segmento de clientes](create-customer-segments.md). <!-- You can also send a single notification to all of the customers for multiple apps. -->

> [!IMPORTANT]
> Estas notificaciones solo pueden usarse con aplicaciones para UWP.

Al considerar el contenido de las notificaciones, ten en cuenta lo siguiente:
- El contenido de tus notificaciones debe cumplir con las [Directivas de contenido](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies) de la Tienda.
- El contenido de notificación no debe incluir información potencialmente confidencial.
- Si bien haremos todos los esfuerzos para enviar la notificación como esté programada, en ocasiones, es posible que problemas de latencia afecten a la entrega.
- Asegúrate de no enviar notificaciones con demasiada frecuencia. Más de una vez cada 30 minutos puede parecer intrusivo (y en muchos casos, se prefiere una frecuencia menor).
- Ten en cuenta que, si un cliente que usa tu aplicación (e inició sesión con su cuenta de Microsoft en el momento de determinar la pertenencia a un segmento) y más adelante da su dispositivo a una persona para que lo use, esta última puede ver la notificación dirigida al cliente original. Para obtener más información, consulta [Configurar la aplicación para notificaciones push dirigidas](../monetize/configure-your-app-to-receive-dev-center-notifications.md#notification-customers).
<!-- - If you send the same notification to customers of multiple apps, you can't target a segment; the notification will be sent to all customers for the apps you select. -->


## <a name="getting-started-with-notifications"></a>Introducción a las notificaciones

En un nivel elevado, deberás seguir tres pasos para usar las notificaciones con el propósito de interactuar con los clientes.

1. **Registra la aplicación para recibir notificaciones push.** Para ello, agrega una referencia a Microsoft Store Services SDK en tu aplicación y, a continuación, agrega unas líneas de código que registre un canal de notificación entre el Centro de desarrollo y la aplicación. Usaremos ese canal para entregar las notificaciones a tus clientes. Para obtener más detalles, consulta [Configurar la aplicación para notificaciones push dirigidas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Decide a qué clientes quieres dirigirlas.** Puedes enviar la notificación a todos los clientes de la aplicación, o (para las notificaciones creadas para una sola aplicación) a un grupo de clientes denominado un *segmento*, que se puede definir en función de los datos demográficos o de ingresos. Para obtener más información, consulta [Crear segmentos de clientes](create-customer-segments.md). 
3. **Crea tu contenido de notificación y enviarlo.** Por ejemplo, puedes crear una notificación que anime a los clientes nuevos a clasificar la aplicación o enviar una notificación que promueva una oferta especial para adquirir un complemento.


## <a name="to-create-and-send-a-notification"></a>Para crear y enviar una notificación

Sigue estos pasos para crear una notificación push en el panel de información y enviarla a un segmento de cliente en particular.

> [!NOTE]
> Antes de que una aplicación pueda recibir notificaciones del Centro de desarrollo, primero debes llamar al método [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) en tu aplicación para registrar esta de modo que reciba notificaciones. Este método está disponible en el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). Para obtener más información sobre cómo llamar a este método, con un ejemplo de código, consulta [Configurar la aplicación para recibir notificaciones push dirigidas](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

1.  En el [panel de información del Centro de desarrollo de Windows](https://developer.microsoft.com/dashboard/overview), expande la sección **Interactuar** y luego selecciona **Notificaciones**.
2.  En la página **Notificaciones**, selecciona **Nueva notificación**.
3.  En la sección **Seleccionar una plantilla**, elige el tipo de notificación que quieras enviar. Para obtener más información, consulta [Tipos de plantilla de notificación](#notification-template-types). <!-- ![Notification templates](images/push-notifications-template.png) -->
4.      En la siguiente página, <!-- use the drop-down menu to choose either a **Single app** or **Multiple apps** for which you want to generate a notification. --> selecciona una aplicación <!-- or apps --> (que debe estar configurada para recibir las notificaciones mediante el Microsoft Store Services SDK).
5.  En la sección **Configuración de notificaciones**, elige un **Nombre** para la notificación y, si es aplicable, elige el **grupo de clientes** al que quieres enviar la notificación. <!-- (Notifications sent to multiple apps can only be sent to all customers of those apps.) --> Si deseas usar un segmento que no se ha creado todavía, selecciona **Create new customer group**. Ten en cuenta que deben transcurrir 24 horas para poder usar un nuevo segmento para notificaciones. Para obtener más información, consulta [Crear segmentos de clientes](create-customer-segments.md).
6.  Si quieres especificar cuándo enviar la notificación, desactiva la casilla **Send notification immediately** y elige una fecha y hora específicas (en UTC para todos los clientes, a menos que especifiques usar la zona horaria local de cada cliente).
7.  Si quieres que la notificación expire en algún momento, desactiva la casilla **Notification never expires**, y elige una fecha y una hora de expiración específicas (en UTC).
8.  <!-- For notifications to a single app: --> If you want to filter the recipients so that your notification is only delivered to people who use certain languages or are in specific time zones, check the **Use filters** checkbox. You can then specify the language and/or time zone options you want to use.
<!-- and don't forget to update numbers when this comes back... 9.      For notifications to multiple apps: Specify whether to send the notification only to the last active app on each device (per customer), or to all apps on each device. -->
9.  En la sección **Notification content** y en el menú **Language**, elige los idiomas en los que quieres que se muestre la notificación. Para obtener más información, consulta [Traducir las notificaciones)](#translate-your-notifications).
10. En la sección **Options (Opciones)**, escribe un texto y configura las opciones que quieras. Si has usado alguna plantilla, algunas de estas se proporcionarán de manera predeterminada, pero puedes realizar los cambios que quieras.
   Las opciones disponibles varían según el tipo de notificación que uses. Estas son algunas de las opciones:
   - **Activation type (Tipo de activación)** (tipo de notificación del sistema interactiva). Puedes elegir **Foreground (Primer plano)**, **Background (Segundo plano)** o **Protocol (Protocolo)**.
   - **Launch (Iniciar)** (tipo de notificación del sistema interactiva). Puedes elegir que la notificación abra una aplicación o un sitio web.
   - **Track app launch rate (Seguir tasa de inicio de la aplicación)** (tipo de notificación del sistema interactiva). Si quieres medir el rendimiento de tu interacción con los clientes a través de cada notificación, selecciona esta casilla. Para obtener más información, consulta [Medir el rendimiento de las notificaciones](#measure-notification-performance).
   - **Duration (Duración)** (tipo de notificación del sistema interactiva). Puedes elegir **Short (Breve)** o **Long (Larga)**.
   - **Scenario (Escenario)** (tipo de notificación del sistema interactiva). Puedes elegir **Default (Predeterminado)**, **Alarm (Alarma)**, **Reminder (Aviso)** o **Incoming call (Llamada entrante)**.
   - **Base URI (URI base)** (tipo de notificación del sistema interactiva). Para obtener más información, consulta [BaseUri](https://msdn.microsoft.com/library/windows/apps/br208712).
   - **Add image query (Agregar consulta de imagen)** (tipo de notificación del sistema interactiva). Para obtener más información, consulta [addImageQuery](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Visual (Objeto visual)**. Una imagen, un vídeo o un sonido. Para obtener más información, consulta [visual](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Input (Entrada)**/**Action (Acción)**/**Selection (Selección)** (tipo de notificación del sistema interactivas). De esta forma puedes permitir a los usuarios interactuar con la notificación. Para obtener más información, consulta [Notificaciones del sistema interactivas y adaptables](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).
   - **Binding (Enlace)** (tipo de icono interactivo). La plantilla de la notificación del sistema. Para obtener más información, consulta [binding](https://msdn.microsoft.com/library/windows/apps/br230843) (en inglés).

   > [!TIP]
   > Prueba la aplicación [Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1) para diseñar y probar tus iconos adaptables y notificaciones del sistema interactivas.

11. Selecciona **Save as draft (Guardar como borrador)** para seguir trabajando más adelante en la notificación, o **Send (Enviar)** si ya has terminado.


## <a name="notification-template-types"></a>Tipos de plantilla de notificación

Puedes elegir entre una variedad de plantillas de notificación.

-   **Blank (En blanco) (notificación del sistema).** Empieza con una notificación del sistema vacía que puedes personalizar. Una notificación del sistema es una interfaz de usuario emergente que se muestra en pantalla y permite que la aplicación se comunique con el cliente cuando este se encuentra en otra aplicación, en la pantalla Inicio o en el escritorio.
-   **En blanco (icono).** Empieza con una notificación de icono vacía que puedes personalizar. Los iconos son una representación de la aplicación en la pantalla Inicio. Los iconos pueden ser "dinámicos", lo que significa que el contenido que muestran puede cambiar en las respuestas a las notificaciones.
-   **Ask for ratings (Pedir calificaciones) (notificación del sistema).** Una notificación del sistema que pide a los clientes que califiquen la aplicación. Cuando el cliente selecciona la notificación, se muestra la página de calificaciones de la Tienda de tu aplicación.
-   **Ask for feedback (Pedir comentarios) (notificación del sistema).** Una notificación del sistema que pide a los clientes que escriban un comentario de la aplicación. Cuando el cliente selecciona la notificación, se muestra la página del Centro de opiniones de tu aplicación.

   > [!NOTE]
   > Si eliges este tipo de plantilla, recuerda que en la casilla **Launch** (Iniciar) debes reemplazar el valor de marcador de posición {PACKAGE_FAMILY_NAME} por el Nombre de familia de paquete (PFN) real de la aplicación. Encontrarás el PFN de tu aplicación en la página [App identity (Identidad de la aplicación)](view-app-identity-details.md) (**App management [Administración de la aplicación]** > **App identity [Identidad de la aplicación]**).

   ![Cuadro "Launch" de la notificación del sistema de solicitud de comentarios](images/push-notifications-feedback-toast-launch-box.png)
-   **Cross-promote (Promocionar de manera cruzada) (notificación del sistema).** Una notificación del sistema para promocionar otra aplicación de tu elección. Cuando el cliente selecciona la notificación, se muestra la descripción de la aplicación en la Tienda.
   > [!NOTE]
   > Si eliges este tipo de plantilla, recuerda que en la casilla **Launch** (Iniciar) debes reemplazar el valor de marcador de posición **{ProductId you want to promote here}** por el identificador de la Tienda real del elemento que quieras promocionar. Encontrarás el identificador de la Tienda en la página [App identity (Identidad de la aplicación)](view-app-identity-details.md) (**App management [Administración de la aplicación]** > **App identity [Identidad de la aplicación]**).

  ![Cuadro "Launch" de la notificación del sistema de promoción cruzada](images/push-notifications-promote-toast-launch-box.png)
-   **Promote a sale (Promocionar una oferta) (notificación del sistema).** Una notificación del sistema que puedes usar para anunciar una oferta de tu aplicación. Cuando el cliente selecciona la notificación, se muestra la descripción de la Tienda de tu aplicación.
-   **Prompt for update (Solicitar actualización) (notificación del sistema).** Una notificación del sistema que recomienda a los clientes con una versión anterior de la aplicación que instalen la última versión. Cuando el cliente seleccione la notificación, se iniciará la aplicación Store, con la lista **Descargas y actualizaciones**. Ten en cuenta que solo se puede usar esta plantilla con una sola aplicación, pero no puedes tener como objetivo un segmento de clientes concreto o definir un momento para enviarlo; siempre programaremos esta notificación para que se envíe en un plazo de 24 horas y nos esforzaremos por tener como objetivo todos los usuarios que no estén ejecutando aún la versión más reciente de la aplicación.


## <a name="measure-notification-performance"></a>Medir el rendimiento de las notificaciones

Puedes medir cuál es el rendimiento de tu interacción con los clientes mediante cada notificación.


###<a name="to-measure-notification-performance"></a>Medir el rendimiento de las notificaciones

1.  Cuando crees una notificación, en la sección **Notification content (Contenido de la notificación)**, selecciona la casilla **Track app launch rate (Seguir tasa de inicio de la aplicación)**.
2.  En la aplicación, llama al método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) para notificar al Centro de desarrollo de que tu aplicación se ha iniciado como respuesta a una notificación dirigida. Este método lo proporciona el Microsoft Store Services SDK. Para obtener más información acerca de cómo llamar a este método, consulta [Configure your app to receive Dev Center notifications (Configurar la aplicación para recibir notificaciones push del Centro de desarrollo)](../monetize/configure-your-app-to-receive-dev-center-notifications.md).


###<a name="to-view-notification-performance"></a>Ver el rendimiento de una notificación

Cuando hayas configurado la notificación y la aplicación para medir el rendimiento de las notificaciones como se ha indicado anteriormente, puedes usar el panel para ver cuál es el rendimiento de tus notificaciones.

1.  En el [panel de información del Centro de desarrollo de Windows](https://developer.microsoft.com/dashboard/overview), expande la sección **Interactuar** y luego selecciona **Notificaciones**.
2.  En la página **Targeted push notifications (Notificaciones push dirigidas)**, selecciona **In progress (En curso)** o **Completed (Completada)**y luego mira las columnas **Delivery rate (Tasa de entrega)** y **App launch rate (Tasa de inicio de aplicación)** para comprobar el rendimiento de alto nivel de cada notificación.
3.  Para ver datos de rendimiento más detallados, selecciona el nombre de una notificación. En la sección **Delivery statistics**, puedes ver **recuento** y **porcentaje** de los siguientes tipos de **estados** de las notificaciones:
 - **Failed (Erróneo)**: la notificación no se ha entregado por algún motivo. Esto puede suceder, por ejemplo, si se produce algún problema en el servicio de notificaciones de Windows.
 - **Channel expiration failure (Error de expiración del canal)**: la notificación no se ha entregado porque ha expirado el canal entre la aplicación y el Centro de desarrollo. Por ejemplo, esto puede suceder si el cliente no abre la aplicación en mucho tiempo.
 - **Sending (Enviando)**: la notificación está en la cola de envío.
 - **Sent (Enviada)**: la notificación se ha enviado.
 - **Launched (Iniciada)**: se ha enviado la notificación, el cliente ha hecho clic en ella y, como resultado, se ha abierto la aplicación. Ten en cuenta solo se realiza el seguimiento de los inicios de las aplicaciones. Las notificaciones que invitan al cliente a realizar otras acciones, como iniciar la Tienda para dejar una calificación, no se incluyen en este estado.
 - **Unknown (Desconocido)**: no se ha podido determinar el estado de esta notificación.


## <a name="translate-your-notifications"></a>Traducir las notificaciones

Para maximizar el efecto de las notificaciones, plantéate la posibilidad de traducirlas a los idiomas que tus clientes prefieren. El Centro de desarrollo facilita el proceso de traducción automática de las notificaciones aprovechando la potencia del servicio [Microsoft Translator](https://msdn.microsoft.com/library/dd576287.aspx).

1.  Cuando hayas escrito la notificación en tu idioma predeterminado, selecciona **Add languages (Agregar idiomas)** (debajo del menú **Languages [Idiomas]** de la sección **Notification content [Contenido de la notificación]**).
2.  En la ventana **Add languages (Agregar idiomas)**, selecciona los idiomas adicionales en los que quieres que se muestren tus notificaciones y luego selecciona **Update (Actualizar)**.
La notificación se traducirá automáticamente a los idiomas que hayas elegido en la ventana **Add languages (Agregar idiomas)**; esos idiomas se agregarán al menú **Language (Idioma)**.
3.  Para ver la traducción de la notificación, selecciona el idioma que has agregado en el menú**Language (Idioma)**.

Debes tener lo siguiente en cuenta sobre las traducciones:
 - Puedes modificar la traducción automática escribiendo en el cuadro **Content (Contenido)** del idioma que corresponda.
 - Si agregas otro cuadro de texto a la versión en inglés de la notificación después de modificar una traducción automática, el nuevo cuadro de texto no se agregará a la notificación traducida. En ese caso, deberás agregar manualmente los nuevos cuadros de texto a cada una de las notificaciones traducidas.
 - Si cambias el texto en inglés después de que la notificación se haya traducido, actualizaremos automáticamente las notificaciones traducidas para que reflejen el cambio. Sin embargo, esto no sucede si previamente has decidido modificar la traducción original.

## <a name="related-topics"></a>Temas relacionados
- [Iconos de aplicaciones para UWP](../design/shell/tiles-and-notifications/creating-tiles.md)
- [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
- [Aplicación Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [Método StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [Customer segmentation and push notifications: a new Windows Dev Center Insider Program feature (Segmentación de clientes y notificaciones push: una nueva característica del programa Insider del Centro de desarrollo de Windows) (entrada de blog)](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)
