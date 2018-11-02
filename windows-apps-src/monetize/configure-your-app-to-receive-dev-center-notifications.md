---
author: Xansky
Description: Learn how to register your UWP app to receive push notifications that you send from Windows Dev Center.
title: Configurar la aplicación para notificaciones push dirigidas
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP, Microsoft Store Services SDK, Microsoft Store Services SDK, targeted push notifications, notificaciones push dirigidas, Dev Center, Centro de desarrollo
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: 451643387076a8f944ffe94206ea6fac08524026
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5935458"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Configurar la aplicación para notificaciones push dirigidas

Puedes usar la página **Notificaciones de inserción** del panel del Centro de desarrollo de Windows para interactuar directamente con los clientes mediante el envío de notificaciones push dirigidas a los dispositivos en los que está instalada la aplicación para la Plataforma universal de Windows (UWP). Por ejemplo, puedes usar las notificaciones push dirigidas para sugerir a los clientes que realicen una determinada acción, como valorar tu aplicación o probar una nueva característica. Puedes enviar diferentes tipos de notificaciones de inserción, incluidas notificaciones del sistema, notificaciones de icono y notificaciones XML sin formato. También puedes supervisar la tasa de inicios de la aplicación que se derivan de las notificaciones de inserción. Para obtener más información acerca de esta característica, consulta [Enviar notificaciones de inserción a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md).

Para poder enviar notificaciones push dirigidas a los clientes del Centro de desarrollo, debes usar un método de la clase [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) en el Microsoft Store Services SDK para registrar la aplicación para la recepción de notificaciones. Puedes usar métodos adicionales de esta clase para notificar al Centro de desarrollo que la aplicación se ha iniciado en respuesta a una notificación push dirigida (si quieres hacer un seguimiento de la tasa de inicios de la aplicación que se derivan de las notificaciones) y para dejar de recibir notificaciones.

## <a name="configure-your-project"></a>Configurar tu proyecto

Antes de escribir código, sigue estos pasos para agregar una referencia al Microsoft Store Services SDK en tu proyecto:

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo. 
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.
4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

## <a name="register-for-push-notifications"></a>Registrar para notificaciones de inserción

Para registrar tu aplicación para que reciba notificaciones push dirigidas del Centro de desarrollo:

1. En tu proyecto, busca una sección de código que se ejecute durante el inicio en el que puedes registrar tu aplicación para la recepción de notificaciones.
2. Agrega la siguiente instrucción en la parte superior del archivo de código.

    [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#EngagementNamespace)]

3. Obtén un objeto [StoreServicesEngagementManager](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) y llama a una de las sobrecargas [RegisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) del código de inicio que identificaste anteriormente. Se llamará a este método cada vez que se inicie la aplicación.

  * Si quieres que el Centro de desarrollo cree su propio URI de canal para las notificaciones, llama a la sobrecarga [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync).

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync1)]
      > [!IMPORTANT]
      > Si tu aplicación también llama a [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) para crear un canal de notificación de WNS, asegúrate de que el código no llama a [CreatePushNotificationChannelForApplicationAsync](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) y a la sobrecarga [RegisterNotificationChannelAsync()](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) al mismo tiempo. Si necesitas llamar a estos dos métodos, llámalos secuencialmente y espera la devolución de un método antes de llamar al otro.

  * Si quieres especificar el URI de canal que se usará para las notificaciones push dirigidas del Centro de desarrollo, llama a la sobrecarga [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync). Por ejemplo, es posible que quieras hacer esto si tu aplicación ya usa Servicios de notificaciones de inserción de Windows (WNS) y quieres usar el mismo URI de canal. Primero debes crear un objeto [StoreServicesNotificationChannelParameters](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) y asignar la propiedad [CustomNotificationChannelUri](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) al URI de tu canal.

      [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#RegisterNotificationChannelAsync2)]

> [!NOTE]
> Al llamar al método **RegisterNotificationChannelAsync**, se crea un archivo denominado MicrosoftStoreEngagementSDKId.txt en al almacén de datos de aplicaciones local para la aplicación (la carpeta devuelta por la propiedad [ApplicationData.LocalFolder](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalFolder)). Este archivo contiene el id. usado por la infraestructura de notificaciones push dirigidas. Asegúrate de que la aplicación no modifica ni elimina este archivo. De lo contrario, los usuarios pueden recibir varias instancias de notificaciones o puede que estas no se comporten correctamente de otro modo.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Cómo se enrutan las notificaciones push dirigidas a los clientes

Cuando la aplicación llama a **RegisterNotificationChannelAsync**, este método toma la cuenta de Microsoft del cliente que ha iniciado sesión en el dispositivo. Posteriormente, cuando envías una notificación push dirigida a un segmento que incluye este cliente, el Centro de desarrollo envía la notificación a los dispositivos que están asociados a la cuenta de Microsoft de este cliente.

Ten en cuenta que, si el cliente que ha iniciado la aplicación cede su dispositivo a otra persona para que lo use mientras todavía está conectado en el dispositivo con su cuenta de Microsoft, esa persona podrá ver la notificación que estaba dirigida al cliente original. Esto puede tener consecuencias imprevistas, en particular para las aplicaciones que ofrecen servicios a los que los clientes pueden conectarse para usarlos. Para impedir que otros usuarios vean tus notificaciones dirigidas en este escenario, llama al método [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) cuando los clientes cierren sesión en la aplicación. Para más información, consulta [Anular el registro para notificaciones push](#unregister) más tarde en este artículo.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Cómo responde la aplicación cuando el usuario la inicia

Después de que tu aplicación esté registrada para recibir notificaciones y de [enviar una notificación de inserción a los clientes de la aplicación desde el Centro de desarrollo](../publish/send-push-notifications-to-your-apps-customers.md), se llamará a uno de estos puntos de entrada de tu aplicación cuando el usuario inicie la aplicación en respuesta a la notificación de inserción. Si tienes código que quieres que se ejecute cuando el usuario inicie la aplicación, puedes agregar el código a uno de estos puntos de entrada de tu aplicación.

  * Si la notificación de inserción tiene un tipo de activación en primer plano, anula el método [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) de la clase **App** de tu proyecto y agrega el código a este método.

  * Si la notificación de inserción tiene un tipo de activación en segundo plano, agrega el código al método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) para la [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md).

Por ejemplo, es posible que quieras recompensar a los usuarios de la aplicación que hayan adquirido complementos de pago en la aplicación regalándoles un complemento gratuito. En este caso, puedes enviar una notificación de inserción a un [segmento de clientes](../publish/create-customer-segments.md) que esté dirigida específicamente a esos usuarios. A continuación, puedes agregar código para regalarles una [compra desde la aplicación](in-app-purchases-and-trials.md) gratuita en uno de los puntos de entrada señalados anteriormente.

## <a name="notify-dev-center-of-your-app-launch"></a>Notificar al Centro de desarrollo el inicio de la aplicación

Si seleccionas la opción **Seguir tasa de inicio de la aplicación** para la notificación push dirigida en el Centro de desarrollo, llama al método [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) desde el punto de entrada correspondiente de la aplicación para notificar al Centro de desarrollo que la aplicación se ha iniciado en respuesta a una notificación push.

Este método devuelve también los argumentos de inicio originales de la aplicación. Cuando elijas realizar un seguimiento de la tasa de inicio de la aplicación para tu notificación push, se agregará un id. de seguimiento opaco a los argumentos de inicio para el seguimiento del inicio de la aplicación en el Centro de desarrollo. Debes pasar los argumentos de inicio para tu aplicación al método [ParseArgumentsAndTrackAppLaunch](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch), que enviará el identificador de seguimiento al Centro de desarrollo, eliminará el identificador de seguimiento de los argumentos de inicio y devolverá los argumentos de inicio originales a tu código.

La forma de llamar al método depende del tipo de activación de la notificación push:

* Si la notificación de inserción tiene un tipo de activación en primer plano, llama a este método desde la invalidación del método [OnActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onactivated) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActivatedEventArgs](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement** y **Windows.ApplicationModel.Activation**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/App.xaml.cs#OnActivated)]

* Si la notificación de inserción tiene un tipo de activación en segundo plano, llama a este método desde el método [Run](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) para tu [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActionTriggerDetail](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** y **Windows.UI.Notifications**.

  [!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#Run)]

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Anular el registro para notificaciones push

Si quieres que tu aplicación deje de recibir notificaciones push dirigidas del Centro de desarrollo, llama al método [UnregisterNotificationChannelAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync).

[!code-cs[DevCenterNotifications](./code/StoreSDKSamples/cs/DevCenterNotifications.cs#UnregisterNotificationChannelAsync)]

Ten en cuenta que este método invalida el canal que se esté utilizando para las notificaciones, de manera que la aplicación ya no recibirá notificaciones de inserción de *ningún* servicio. Una vez cerrado, el canal no puede usarse de nuevo para ningún servicio, incluidas las notificaciones push dirigidas del Centro de desarrollo y otras notificaciones con WNS. Para reanudar el envío de notificaciones push a esta aplicación, la aplicación debe solicitar un nuevo canal.

## <a name="related-topics"></a>Temas relacionados

* [Enviar notificaciones de inserción a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md)
* [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview)
* [Cómo solicitar, crear y guardar un canal de notificación](https://docs.microsoft.com/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
