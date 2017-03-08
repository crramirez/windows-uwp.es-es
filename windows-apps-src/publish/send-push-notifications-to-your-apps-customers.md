---
author: shawjohn
Description: "Aprende a enviar notificaciones push dirigidas desde el Centro de desarrollo de Windows a tu aplicación para animar a los clientes a realizar una acción, como calificar una aplicación o comprar un complemento."
title: "Enviar notificaciones push dirigidas a los clientes de la aplicación"
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 16386c81-702d-47cd-9f91-67659f5dca73
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: a2bd3308863b6343a7616bf86e0b0036f1631bcd
ms.lasthandoff: 02/08/2017

---

# <a name="send-targeted-push-notifications-to-your-apps-customers"></a>Enviar notificaciones push dirigidas a los clientes de la aplicación

Interactuar con los clientes en el momento justo y con el mensaje adecuado es clave para lograr tener éxito como desarrollador de aplicaciones. El Centro de desarrollo de Windows ofrece una plataforma de participación basada en datos para los clientes que puedes usar para enviar notificaciones push a todos los clientes o solo a un subconjunto de los clientes de Windows 10 que cumplan los criterios que hayas definido en un [segmento de clientes](create-customer-segments.md).

Puedes usar las notificaciones push dirigidas para animar a los clientes a realizar una acción, como calificar una aplicación, comprar un complemento, probar una nueva característica o descargar otra aplicación.

> **Importante** Las notificaciones push dirigidas solo pueden usarse con aplicaciones para UWP.

## <a name="getting-started-with-push-notifications"></a>Introducción a las notificaciones push

En un nivel elevado, deberás seguir tres pasos para usar las notificaciones push con el propósito de interactuar con los clientes:
1. **Registra la aplicación para recibir notificaciones push.** Para ello, agrega una referencia a Microsoft Store Services SDK en tu aplicación y, a continuación, agrega unas líneas de código que registre un canal de notificación entre el Centro de desarrollo y la aplicación. Usaremos ese canal para entregar las notificaciones push a tus clientes. Para obtener más información, consulta [Configurar la aplicación para recibir notificaciones de inserción del Centro de desarrollo](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2. **Crear uno o varios segmentos de clientes a los que quieras dirigirte.** Puedes agrupar a los clientes en segmentos según los datos demográficos o el nivel de ingresos. Para obtener más información, consulta [Crear segmentos de clientes](create-customer-segments.md).
3. **Crea una notificación push y envíala a un segmento de clientes en concreto.** Por ejemplo, envía una notificación para animar a los nuevos clientes a calificar la aplicación o con una oferta especial para comprar un complemento.

## <a name="to-create-and-send-a-targeted-push-notification"></a>Crear y enviar una notificación push dirigida

1. Si aún no lo has hecho, instala [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) y llama al método [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) en el código de inicio de tu aplicación para registrarla de modo que reciba notificaciones. Para obtener más información acerca de cómo llamar a este método, consulta [Configure your app to receive Dev Center notifications (Configurar la aplicación para recibir notificaciones push del Centro de desarrollo)](../monetize/configure-your-app-to-receive-dev-center-notifications.md).
2.    Selecciona tu aplicación en el [panel del Centro de desarrollo de Windows](https://developer.microsoft.com/dashboard/overview).
3.    En el menú de navegación de la izquierda, expande **Services (Servicios)**y selecciona **Push notifications (Notificaciones push)**.
4.    En la página **Targeted push notifications (Notificaciones push dirigidas)**, selecciona **New notification (Nueva notificación)**.
5.    En la sección **Select a template (Seleccionar una plantilla)**, elige el tipo de notificación que quieras enviar. Para obtener más información, consulta [Tipos de plantilla de notificación](#notification-template-types).
  ![Plantillas de notificación](images/push-notifications-template.png)
6.    En la sección **Notification settings (Configuración de notificaciones)**, elige un **nombre** para la notificación y el **grupo de clientes** al que quieres enviar la notificación.
Si aún no has creado ningún segmento, selecciona **Create new customer group (Crear nuevo grupo de clientes)**. Ten en cuenta que para que un nuevo segmento pueda usar notificaciones deben transcurrir 24 horas. Para obtener más información, consulta [Crear segmentos de clientes](create-customer-segments.md).
7.    Si quieres concretar cuándo enviar la notificación, desactiva la casilla **Send notification immediately (Enviar notificación inmediatamente)**, y elige una fecha y una hora.
8.    Si quieres que la notificación expire en algún momento, desactiva la casilla **Notification never expires (La notificación no expira nunca)**, y elige una fecha y una hora de expiración.
9.    En la sección **Notification content (Contenido de la notificación)** y en el menú **Language (Idioma)**, elige los idiomas en los que quieres que se muestre la notificación. Para obtener más información, consulta [Traducir las notificaciones)](#translate-your-notifications).
10.    En la sección **Options (Opciones)**, escribe un texto y configura las opciones que quieras. Si has usado alguna plantilla, algunas de estas se proporcionarán de manera predeterminada, pero puedes realizar los cambios que quieras.
   Las opciones disponibles varían según el tipo de notificación que uses. Estas son algunas de las opciones:
   - **Activation type (Tipo de activación)** (tipo de notificación del sistema interactiva). Puedes elegir **Foreground (Primer plano)**, **Background (Segundo plano)** o **Protocol (Protocolo)**.
   - **Launch (Iniciar)** (tipo de notificación del sistema interactiva). Puedes elegir que la notificación abra una aplicación o un sitio web.
   - **Track app launch rate (Seguir tasa de inicio de la aplicación)** (tipo de notificación del sistema interactiva). Si quieres medir el rendimiento de tu interacción con los clientes a través de cada notificación, selecciona esta casilla. Para obtener más información, consulta [Medir el rendimiento de las notificaciones](#measure-notification-performance).
   - **Duration (Duración)** (tipo de notificación del sistema interactiva). Puedes elegir **Short (Breve)** o **Long (Larga)**.
   - **Scenario (Escenario)** (tipo de notificación del sistema interactiva). Puedes elegir **Default (Predeterminado)**, **Alarm (Alarma)**, **Reminder (Aviso)** o **Incoming call (Llamada entrante)**.
   - **Base URI (URI base)** (tipo de notificación del sistema interactiva). Para obtener más información, consulta [BaseUri](https://msdn.microsoft.com/library/windows/apps/br208712).
   - **Add image query (Agregar consulta de imagen)** (tipo de notificación del sistema interactiva). Para obtener más información, consulta [addImageQuery](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Visual (Objeto visual)**. Una imagen, un vídeo o un sonido. Para obtener más información, consulta [visual](https://msdn.microsoft.com/library/windows/apps/br230847).
   - **Input (Entrada)**/**Action (Acción)**/**Selection (Selección)** (tipo de notificación del sistema interactivas). De esta forma puedes permitir a los usuarios interactuar con la notificación. Para obtener más información, consulta [Notificaciones del sistema interactivas y adaptables](../controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts.md#actions).
   - **Binding (Enlace)** (tipo de icono interactivo). La plantilla de la notificación del sistema. Para obtener más información, consulta [binding](https://msdn.microsoft.com/library/windows/apps/br230843).

   > **Sugerencia** Prueba la aplicación [Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1) para diseñar y probar tus iconos adaptables y notificaciones del sistema interactivas.

11.    Selecciona **Save as draft (Guardar como borrador)** para seguir trabajando más adelante en la notificación, o **Send (Enviar)** si ya has terminado.

> **Nota** El contenido de tus notificaciones debe cumplir con las [Directivas de contenido](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies) de la Tienda.

## <a name="notification-template-types"></a>Tipos de plantilla de notificación

Puedes elegir entre una variedad de plantillas de notificación.

-    **Blank (En blanco) (notificación del sistema).** Empieza con una notificación del sistema vacía que puedes personalizar. Una notificación del sistema es una interfaz de usuario emergente que se muestra en pantalla y permite que la aplicación se comunique con el cliente cuando este se encuentra en otra aplicación, en la pantalla Inicio o en el escritorio.
-    **En blanco (icono).** Empieza con una notificación de icono vacía que puedes personalizar. Los iconos son una representación de la aplicación en la pantalla Inicio. Los iconos pueden ser "dinámicos", lo que significa que el contenido que muestran puede cambiar en las respuestas a las notificaciones.
-    **Ask for ratings (Pedir calificaciones) (notificación del sistema).** Una notificación del sistema que pide a los clientes que califiquen la aplicación. Cuando el cliente selecciona la notificación, se muestra la página de calificaciones de la Tienda de tu aplicación.
-    **Ask for feedback (Pedir comentarios) (notificación del sistema).** Una notificación del sistema que pide a los clientes que escriban un comentario de la aplicación. Cuando el cliente selecciona la notificación, se muestra la página del Centro de opiniones de tu aplicación.
   > **Nota** Si eliges este tipo de plantilla, recuerda que en el cuadro **Launch (Iniciar)** debes reemplazar el valor de marcador de posición {PACKAGE_FAMILY_NAME} por el Nombre de familia de paquete (PFN) real de la aplicación. Encontrarás el PFN de tu aplicación en la página [App identity (Identidad de la aplicación)](view-app-identity-details.md) (**App management [Administración de la aplicación]** > **App identity [Identidad de la aplicación]**).

   ![Cuadro "Launch" de la notificación del sistema de solicitud de comentarios](images/push-notifications-feedback-toast-launch-box.png)
-    **Cross-promote (Promocionar de manera cruzada) (notificación del sistema).** Una notificación del sistema para promocionar otra aplicación de tu elección. Cuando el cliente selecciona la notificación, se muestra la descripción de la Tienda de la otra aplicación.
  > **Nota** Si eliges este tipo de plantilla, recuerda que en el cuadro **Launch (Iniciar)** debes reemplazar el valor de marcador de posición **{ProductId you want to promote here}** por el identificador de la Tienda real del elemento que quieras promocionar. Encontrarás el identificador de la Tienda en la página [App identity (Identidad de la aplicación)](view-app-identity-details.md) (**App management [Administración de la aplicación]** > **App identity [Identidad de la aplicación]**).

  ![Cuadro "Launch" de la notificación del sistema de promoción cruzada](images/push-notifications-promote-toast-launch-box.png)
-    **Promote a sale (Promocionar una oferta) (notificación del sistema).** Una notificación del sistema que puedes usar para anunciar una oferta de tu aplicación. Cuando el cliente selecciona la notificación, se muestra la descripción de la Tienda de tu aplicación.
- **Prompt for update (Solicitar actualización) (notificación del sistema).** Una notificación del sistema que recomienda a los clientes con una versión anterior de la aplicación que instalen la última versión. Cuando el cliente selecciona la notificación, se muestra la lista **Descargas y actualizaciones** de la aplicación de la Tienda. No hace falta que crees un segmento de clientes para usar esta plantilla. Programaremos esta notificación en menos de 24 horas y haremos todo lo posible para dirigirnos a todos los usuarios que aún no utilicen la última versión de tu aplicación.

## <a name="measure-notification-performance"></a>Medir el rendimiento de las notificaciones

Puedes medir cuál es el rendimiento de tu interacción con los clientes mediante cada notificación.

###<a name="to-measure-notification-performance"></a>Medir el rendimiento de las notificaciones

1.    Cuando crees una notificación, en la sección **Notification content (Contenido de la notificación)**, selecciona la casilla **Track app launch rate (Seguir tasa de inicio de la aplicación)**.
2.    En la aplicación, llama al método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) para notificar al Centro de desarrollo de que tu aplicación se ha iniciado como respuesta a una notificación dirigida. Este método lo proporciona el SDK de Microsoft Store. Para obtener más información acerca de cómo llamar a este método, consulta [Configure your app to receive Dev Center notifications (Configurar la aplicación para recibir notificaciones push del Centro de desarrollo)](../monetize/configure-your-app-to-receive-dev-center-notifications.md).

###<a name="to-view-notification-performance"></a>Ver el rendimiento de una notificación

Cuando hayas configurado la notificación y la aplicación para [medir el rendimiento de las notificaciones](#to-measure-notification-performance) como se ha indicado anteriormente, puedes usar el panel para ver cuál es el rendimiento de tus notificaciones.

1.  En el panel, selecciona una de tus aplicaciones.
2.  Expande la sección **Services (Servicios)** del menú de la izquierda y luego selecciona **Push notifications (Notificaciones push)** para ver las notificaciones asociadas a dicha aplicación.
3.    En la página **Targeted push notifications (Notificaciones push dirigidas)**, selecciona **In progress (En curso)** o **Completed (Completada)**y luego mira las columnas **Delivery rate (Tasa de entrega)** y **App launch rate (Tasa de inicio de aplicación)** para comprobar el rendimiento de alto nivel de cada notificación.
4.  Para ver datos de rendimiento más detallados, selecciona el nombre de una notificación. Se mostrará la sección **Delivery statistics (Estadísticas de entrega)** con información de **recuento** y **porcentaje** de los siguientes tipos de **estados** de las notificaciones:
 - **Failed (Erróneo)**: la notificación no se ha entregado por algún motivo. Esto puede suceder, por ejemplo, si se produce algún problema en el servicio de notificaciones de Windows.
 - **Channel expiration failure (Error de expiración del canal)**: la notificación no se ha entregado porque ha expirado el canal entre la aplicación y el Centro de desarrollo. Por ejemplo, esto puede suceder si el cliente no abre la aplicación en mucho tiempo.
 - **Sending (Enviando)**: la notificación está en la cola de envío.
 - **Sent (Enviada)**: la notificación se ha enviado.
 - **Launched (Iniciada)**: se ha enviado la notificación, el cliente ha hecho clic en ella y, como resultado, se ha abierto la aplicación. Ten en cuenta solo se realiza el seguimiento de los inicios de las aplicaciones. Las notificaciones que invitan al cliente a realizar otras acciones, como iniciar la Tienda para dejar una calificación, no se incluyen en este estado.
 - **Unknown (Desconocido)**: no se ha podido determinar el estado de esta notificación.

## <a name="translate-your-notifications"></a>Traducir las notificaciones

Para maximizar el efecto de las notificaciones, plantéate la posibilidad de traducirlas a los idiomas que tus clientes prefieren. El Centro de desarrollo facilita el proceso de traducción automática de las notificaciones aprovechando la potencia del servicio [Microsoft Translator](https://msdn.microsoft.com/library/dd576287.aspx).

1.    Cuando hayas escrito la notificación en tu idioma predeterminado, selecciona **Add languages (Agregar idiomas)** (debajo del menú **Languages [Idiomas]** de la sección **Notification content [Contenido de la notificación]**).
2.    En la ventana **Add languages (Agregar idiomas)**, selecciona los idiomas adicionales en los que quieres que se muestren tus notificaciones y luego selecciona **Update (Actualizar)**.
La notificación se traducirá automáticamente a los idiomas que hayas elegido en la ventana **Add languages (Agregar idiomas)**; esos idiomas se agregarán al menú **Language (Idioma)**.
3.    Para ver la traducción de la notificación, selecciona el idioma que has agregado en el menú**Language (Idioma)**.

Debes tener lo siguiente en cuenta sobre las traducciones:
 - Puedes modificar la traducción automática escribiendo en el cuadro **Content (Contenido)** del idioma que corresponda.
 - Si agregas otro cuadro de texto a la versión en inglés de la notificación después de modificar una traducción automática, el nuevo cuadro de texto no se agregará a la notificación traducida. En ese caso, deberás agregar manualmente los nuevos cuadros de texto a cada una de las notificaciones traducidas.
 - Si cambias el texto en inglés después de que la notificación se haya traducido, actualizaremos automáticamente las notificaciones traducidas para que reflejen el cambio. Sin embargo, esto no sucede si previamente has decidido modificar la traducción original.

## <a name="related-topics"></a>Temas relacionados
- [Iconos, distintivos y notificaciones para las aplicaciones para UWP](../controls-and-patterns/tiles-badges-notifications.md)
- [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview.md)
- [Introducción a los Servicios de notificaciones de inserción de Windows (WNS) (aplicaciones de Windows en tiempo de ejecución)](https://msdn.microsoft.com/library/windows/apps/hh913756.aspx)
- [Aplicación Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)
- [Método StoreServicesEngagementManager.RegisterNotificationChannelAsync() | registerNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx)
- [Customer segmentation and push notifications: a new Windows Dev Center Insider Program feature (Segmentación de clientes y notificaciones push: una nueva característica del programa Insider del Centro de desarrollo de Windows) (entrada de blog)](https://blogs.windows.com/buildingapps/2016/08/17/customer-segmentation-and-push-notifications-a-new-windows-dev-center-insider-program-feature/#XTuCqrG8G5IMgWew.97)

