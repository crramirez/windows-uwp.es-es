---
Description: Obtenga información sobre cómo registrar su aplicación UWP para recibir notificaciones de inserción que se envían desde el centro de partners.
title: Configurar la aplicación para notificaciones push dirigidas
ms.date: 02/08/2017
ms.topic: article
keywords: destino de Windows 10, uwp, Microsoft Store Services SDK, las notificaciones de inserción, centro de partners
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: f60780186256e7f78a9596c979c79bfc704ae4c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660170"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Configurar la aplicación para notificaciones push dirigidas

Puede usar el **notificaciones de inserción** página en el centro de partners para ponerse en contacto directamente con los clientes mediante el envío de notificaciones de inserción a los dispositivos en el que se instala la aplicación de plataforma Universal de Windows (UWP) de destino. Por ejemplo, puedes usar las notificaciones push dirigidas para sugerir a los clientes que realicen una determinada acción, como valorar tu aplicación o probar una nueva característica. Puedes enviar diferentes tipos de notificaciones de inserción, incluidas notificaciones del sistema, notificaciones de icono y notificaciones XML sin formato. También puedes supervisar la tasa de inicios de la aplicación que se derivan de las notificaciones de inserción. Para obtener más información acerca de esta característica, consulta [Enviar notificaciones de inserción a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md).

Para poder enviar notificaciones push destinadas a sus clientes desde el centro de partners, debe usar un método de la [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) clase en el de Microsoft Store Services SDK para registrar la aplicación para recibir notificaciones. Puede usar los métodos adicionales de esta clase para notificar al centro de partners que se inició la aplicación en respuesta a una notificación de inserción de destino (si desea realizar un seguimiento de la tasa de lanzamientos de la aplicación que se obtienen de las notificaciones) y para dejar de recibir notificaciones.

## <a name="configure-your-project"></a>Configurar tu proyecto

Antes de escribir código, sigue estos pasos para agregar una referencia al Microsoft Store Services SDK en tu proyecto:

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo. 
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.
4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

## <a name="register-for-push-notifications"></a>Registrar para notificaciones de inserción

Para registrar la aplicación para recibir notificaciones de inserción de destino desde el centro de partners:

1. En tu proyecto, busca una sección de código que se ejecute durante el inicio en el que puedes registrar tu aplicación para la recepción de notificaciones.
2. Agrega esta instrucción en la parte superior del archivo de código.

    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. Obtén un objeto [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) y llama a una de las sobrecargas [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) del código de inicio que identificaste anteriormente. Se llamará a este método cada vez que se inicie la aplicación.

  * Si desea que el centro de partners para crear su propio URI de canal para las notificaciones, llame a la [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) sobrecargar.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > Si tu aplicación también llama a [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) para crear un canal de notificación de WNS, asegúrate de que el código no llama a [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) y a la sobrecarga [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) al mismo tiempo. Si necesitas llamar a estos dos métodos, llámalos secuencialmente y espera la devolución de un método antes de llamar al otro.

  * Si desea especificar el canal de URI que se utiliza para recibir notificaciones de inserción de destino del centro de partners, llamada la [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) sobrecargar. Por ejemplo, es posible que quieras hacer esto si tu aplicación ya usa Servicios de notificaciones de inserción de Windows (WNS) y quieres usar el mismo URI de canal. Primero debes crear un objeto [StoreServicesNotificationChannelParameters](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) y asignar la propiedad [CustomNotificationChannelUri](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) al URI de tu canal.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> Al llamar al método **RegisterNotificationChannelAsync**, se crea un archivo denominado MicrosoftStoreEngagementSDKId.txt en al almacén de datos de aplicaciones local para la aplicación (la carpeta devuelta por la propiedad [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalFolder)). Este archivo contiene el id. usado por la infraestructura de notificaciones push dirigidas. Asegúrate de que la aplicación no modifica ni elimina este archivo. De lo contrario, los usuarios pueden recibir varias instancias de notificaciones o puede que estas no se comporten correctamente de otro modo.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Cómo se enrutan las notificaciones push dirigidas a los clientes

Cuando la aplicación llama a **RegisterNotificationChannelAsync**, este método toma la cuenta de Microsoft del cliente que ha iniciado sesión en el dispositivo. Más adelante, cuando se envía una notificación push destinadas a un segmento que incluye a este cliente, Partner Center envía la notificación a los dispositivos que están asociados con la cuenta de Microsoft de este cliente.

Ten en cuenta que, si el cliente que ha iniciado la aplicación cede su dispositivo a otra persona para que lo use mientras todavía está conectado en el dispositivo con su cuenta de Microsoft, esa persona podrá ver la notificación que estaba dirigida al cliente original. Esto puede tener consecuencias imprevistas, en particular para las aplicaciones que ofrecen servicios a los que los clientes pueden conectarse para usarlos. Para impedir que otros usuarios vean tus notificaciones dirigidas en este escenario, llama al método [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) cuando los clientes cierren sesión en la aplicación. Para más información, consulta [Anular el registro para notificaciones push](#unregister) más tarde en este artículo.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Cómo responde la aplicación cuando el usuario la inicia

Después de la aplicación se registra para recibir notificaciones y se [enviar una notificación push a los clientes de la aplicación desde el centro de partners](../publish/send-push-notifications-to-your-apps-customers.md), uno de los siguientes puntos de entrada en la aplicación se llamará cuando el usuario inicia la aplicación en la respuesta con la notificación push. Si tienes código que quieres que se ejecute cuando el usuario inicie la aplicación, puedes agregar el código a uno de estos puntos de entrada de tu aplicación.

  * Si la notificación de inserción tiene un tipo de activación en primer plano, anula el método [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) de la clase **App** de tu proyecto y agrega el código a este método.

  * Si la notificación de inserción tiene un tipo de activación en segundo plano, agrega el código al método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) para la [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md).

Por ejemplo, es posible que quieras recompensar a los usuarios de la aplicación que hayan adquirido complementos de pago en la aplicación regalándoles un complemento gratuito. En este caso, puedes enviar una notificación de inserción a un [segmento de clientes](../publish/create-customer-segments.md) que esté dirigida específicamente a esos usuarios. A continuación, puedes agregar código para regalarles una [compra desde la aplicación](in-app-purchases-and-trials.md) gratuita en uno de los puntos de entrada señalados anteriormente.

## <a name="notify-partner-center-of-your-app-launch"></a>Notificar al centro de partners de su inicio de la aplicación

Si selecciona el **velocidad de inicio de aplicación de seguimiento** opción para la notificación de inserción de destino en el centro de partners, llamada la [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) método desde el punto de entrada correspondiente en la aplicación notifique al centro de partners que se inició la aplicación en respuesta a una notificación de inserción.

Este método devuelve también los argumentos de inicio originales de la aplicación. Cuando opta por controlar la velocidad de inicio de la aplicación para la notificación de inserción, iniciar un seguimiento opaco A que identificador se agrega los argumentos de inicio para ayudar a realizar un seguimiento de la aplicación en el centro de partners. Debe pasar los argumentos de inicio para la aplicación a la [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) , este método y envía el identificador de seguimiento al centro de partners, quita el identificador de seguimiento de los argumentos de inicio y devuelve el original Iniciar argumentos en el código.

La forma de llamar al método depende del tipo de activación de la notificación push:

* Si la notificación de inserción tiene un tipo de activación en primer plano, llama a este método desde la invalidación del método [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement** y **Windows.ApplicationModel.Activation**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* Si la notificación de inserción tiene un tipo de activación en segundo plano, llama a este método desde el método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) para tu [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActionTriggerDetail](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** y **Windows.UI.Notifications**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Anular el registro para notificaciones de inserción

Si desea que la aplicación para dejar de recibir notificaciones de inserción desde el centro de partners, llamada de destino la [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) método.

[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

Ten en cuenta que este método invalida el canal que se esté utilizando para las notificaciones, de manera que la aplicación ya no recibirá notificaciones de inserción de *ningún* servicio. Después de que se ha cerrado, el canal no puede utilizarse nuevamente para todos los servicios, incluidas las notificaciones de inserción de destino desde el centro de partners y otras notificaciones con WNS. Para reanudar el envío de notificaciones de inserción a esta aplicación, la aplicación debe solicitar un nuevo canal.

## <a name="related-topics"></a>Temas relacionados

* [Enviar notificaciones push a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md)
* [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)
* [Cómo solicitar, crear y guardar un canal de notificación](https://docs.microsoft.com/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
