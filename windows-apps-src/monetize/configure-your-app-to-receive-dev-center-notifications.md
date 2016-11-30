---
author: mcleanbyron
Description: "Aprende a registrar tu aplicación para UWP para que reciba notificaciones de inserción que envíes desde el Centro de desarrollo de Windows."
title: "Configurar la aplicación para recibir notificaciones de inserción del Centro de desarrollo"
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: 0e6ac52f1e76c0f59cc428b2ff26dc524e93cbde

---

# Configurar la aplicación para recibir notificaciones de inserción del Centro de desarrollo

Puedes usar la página **Notificaciones de inserción** del panel del Centro de desarrollo de Windows para interactuar directamente con los clientes mediante el envío de notificaciones push dirigidas a los dispositivos en los que está instalada la aplicación para la Plataforma universal de Windows (UWP). Por ejemplo, puedes usar las notificaciones push dirigidas para sugerir a los clientes que realicen una determinada acción, como valorar tu aplicación o probar una nueva característica. Puedes enviar diferentes tipos de notificaciones de inserción, incluidas notificaciones del sistema, notificaciones de icono y notificaciones XML sin formato. También puedes supervisar la tasa de inicios de la aplicación que se derivan de las notificaciones de inserción. Para obtener más información acerca de esta característica, consulta [Enviar notificaciones de inserción a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md).

Para poder enviar notificaciones push dirigidas a los clientes del Centro de desarrollo, debes usar un método de la clase [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) en el Microsoft Store Services SDK para registrar la aplicación para la recepción de notificaciones. Puedes usar métodos adicionales de esta clase para notificar al Centro de desarrollo que la aplicación se ha iniciado en respuesta a una notificación push dirigida (si quieres hacer un seguimiento de la tasa de inicios de la aplicación que se derivan de las notificaciones) y para dejar de recibir notificaciones.

## Configurar tu proyecto

Antes de escribir código, sigue estos pasos para agregar una referencia al Microsoft Store Services SDK en tu proyecto:

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo. Además de la API para registrar una aplicación para la recepción de notificaciones, este SDK también proporciona API para otras características, como ejecutar experimentos en las aplicaciones con pruebas A/B y mostrar anuncios. 
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.
4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

## Registrar para notificaciones de inserción

Para registrar tu aplicación para que reciba notificaciones push dirigidas del Centro de desarrollo:

1. En tu proyecto, busca una sección de código que se ejecute durante el inicio y en la que puedes registrar tu aplicación para la recepción de notificaciones del Centro de desarrollo.
2. Agrega esta instrucción en la parte superior del archivo de código.

  ```csharp
  using Microsoft.Services.Store.Engagement;
  ```

3. Obtén un objeto [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) y llama a una de las sobrecargas [RegisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync.aspx) del código de inicio que identificaste anteriormente. Se llamará a este método cada vez que se inicie la aplicación.

  * Si quieres que el Centro de desarrollo cree su propio URI de canal para las notificaciones, llama a la sobrecarga [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx).

    ```csharp
    StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
    await engagementManager.RegisterNotificationChannelAsync();
    ```

    >**Importante:**&nbsp;&nbsp;si tu aplicación también llama a [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) para crear un canal de notificación de WNS, asegúrate de que el código no llama a [CreatePushNotificationChannelForApplicationAsync](https://msdn.microsoft.com/library/windows/apps/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync.aspx) y a la sobrecarga [RegisterNotificationChannelAsync()](https://msdn.microsoft.com/library/windows/apps/mt771190.aspx) al mismo tiempo. Si necesitas llamar a estos dos métodos, llámalos secuencialmente y espera la devolución de un método antes de llamar al otro.

  * Si quieres especificar el URI de canal que se usará para las notificaciones push dirigidas del Centro de desarrollo, llama a la sobrecarga [RegisterNotificationChannelAsync(StoreServicesNotificationChannelParameters)](https://msdn.microsoft.com/library/windows/apps/mt771191.aspx). Por ejemplo, es posible que quieras hacer esto si tu aplicación ya usa Servicios de notificaciones de inserción de Windows (WNS) y quieres usar el mismo URI de canal. Primero debes crear un objeto [StoreServicesNotificationChannelParameters](https://msdns.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.aspx) y asignar la propiedad [CustomNotificationChannelUri](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri.aspx) al URI de tu canal.

    ```csharp
    StoreServicesNotificationChannelParameters parameters =
        new StoreServicesNotificationChannelParameters();
    parameters.CustomNotificationChannelUri = "Assign your channel URI here";

    StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
    await engagementManager.RegisterNotificationChannelAsync(parameters);
    ```

  >#### Comprender cómo responde la aplicación cuando el usuario inicia la aplicación

  >Después de que tu aplicación esté registrada para recibir notificaciones y de [enviar una notificación de inserción a los clientes de la aplicación desde el Centro de desarrollo](../publish/send-push-notifications-to-your-apps-customers.md), se llamará a uno de estos puntos de entrada de tu aplicación cuando el usuario inicie la aplicación en respuesta a la notificación de inserción. Si tienes código que quieres que se ejecute cuando el usuario inicie la aplicación, puedes agregar el código a uno de estos puntos de entrada de tu aplicación.

  >* Si la notificación de inserción tiene un tipo de activación en primer plano, anula el método [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) de la clase **App** de tu proyecto y agrega el código a este método.

  >* Si la notificación de inserción tiene un tipo de activación en segundo plano, agrega el código al método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) para la [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md).

  >Por ejemplo, es posible que quieras recompensar a los usuarios de la aplicación que hayan adquirido complementos de pago en la aplicación regalándoles un complemento gratuito. En este caso, puedes enviar una notificación de inserción a un [segmento de clientes](../publish/create-customer-segments.md) que esté dirigida específicamente a esos usuarios. A continuación, puedes agregar código para regalarles una [compra desde la aplicación](in-app-purchases-and-trials.md) gratuita en uno de los puntos de entrada señalados anteriormente.

## Notificar el Centro de desarrollo el inicio de la aplicación

Si seleccionas la opción **Velocidad de inicio de aplicación de pista** para una notificación de inserción del Centro de desarrollo, llama al método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx) desde el punto de entrada correspondiente de la aplicación para notificar al Centro de desarrollo de que la aplicación se ha iniciado en respuesta a una notificación de inserción.

Este método devuelve también los argumentos de inicio originales de la aplicación. Cuando elijas realizar un seguimiento de la velocidad de inicio de la aplicación para una notificación de inserción del Centro de desarrollo, se agregará un identificador de seguimiento opaco a los argumentos de inicio para supervisar el inicio de la aplicación en el Centro de desarrollo. Debes pasar los argumentos de inicio para tu aplicación al método [ParseArgumentsAndTrackAppLaunch](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch.aspx), que enviará el identificador de seguimiento al Centro de desarrollo, eliminará el identificador de seguimiento de los argumentos de inicio y devolverá los argumentos de inicio originales a tu código.

La forma de llamar al método depende del tipo de activación de la notificación push dirigida:

* Si la notificación de inserción tiene un tipo de activación en primer plano, llama a este método desde la invalidación del método [OnActivated](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onactivated.aspx) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.toastnotificationactivatedeventargs.aspx) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement** y **Windows.ApplicationModel.Activation**.

  ```csharp
  protected override void OnActivated(IActivatedEventArgs args)
  {
       base.OnActivated(args);   

       if (args is ToastNotificationActivatedEventArgs)
       {
             var toastActivationArgs = args as ToastNotificationActivatedEventArgs;

             StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
             string originalArgs = engagementManager.ParseArgumentsAndTrackAppLaunch(
                 toastActivationArgs.Argument);

             // Use the originalArgs variable to access the original arguments
             // that were passed to the app.
       }
  }
  ```

* Si la notificación de inserción tiene un tipo de activación en segundo plano, llama a este método desde el método [Run](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.ibackgroundtask.run.aspx) para tu [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActionTriggerDetail](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.toastnotificationactiontriggerdetail.aspx) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement**, **Windows.ApplicationModel.Background** y **Windows.UI.Notifications**.

  ```csharp
  public void Run(IBackgroundTaskInstance taskInstance)
  {
       var details = taskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;

       if (details != null)
       {
            StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
            string originalArgs = engagementManager.ParseArgumentsAndTrackAppLaunch(details.Argument);

            // Use the originalArgs variable to access the original arguments
            // that were passed to the app.
       }
  }
  ```

## Anular el registro para notificaciones de inserción

Si quieres que tu aplicación deje de recibir notificaciones push dirigidas del Centro de desarrollo de Windows, llama al método [UnregisterNotificationChannelAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync).

```csharp
StoreServicesEngagementManager engagementManager = StoreServicesEngagementManager.GetDefault();
await engagementManager.UnregisterNotificationChannelAsync();
```

Ten en cuenta que este método invalida el canal que se esté utilizando para las notificaciones, de manera que la aplicación ya no recibirá notificaciones de inserción de *ningún* servicio. Una vez cerrado, el canal no puede usarse de nuevo para ningún servicio, incluidas notificaciones push dirigidas del Centro de desarrollo de Windows y otras notificaciones con WNS. Para reanudar el envío de notificaciones de inserción a esta aplicación, la aplicación debe solicitar un nuevo canal.

## Temas relacionados

* [Enviar notificaciones de inserción a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md)
* [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
* [Cómo solicitar, crear y guardar un canal de notificación](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh868221)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)



<!--HONumber=Nov16_HO1-->


