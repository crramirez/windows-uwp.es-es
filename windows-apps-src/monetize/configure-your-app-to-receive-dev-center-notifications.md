---
description: Obtenga información sobre cómo registrar la aplicación para UWP para recibir notificaciones de envío enviadas desde el centro de Partners.
title: Configurar la aplicación para notificaciones de inserción dirigidas
ms.date: 02/08/2017
ms.topic: article
keywords: SDK de Windows 10, UWP, Microsoft Store Services, notificaciones de envío de destino, centro de Partners
ms.assetid: 30c832b7-5fbe-4852-957f-7941df8eb85a
ms.localizationpriority: medium
ms.openlocfilehash: 2296ae29ddcfd868e31c294f8859d4f4b925fca8
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033543"
---
# <a name="configure-your-app-for-targeted-push-notifications"></a>Configurar la aplicación para notificaciones de inserción dirigidas

Puede usar la página **notificaciones** de envío del centro de partners para interactuar directamente con los clientes mediante el envío de notificaciones de envío de destino a los dispositivos en los que está instalada la aplicación plataforma universal de Windows (UWP). Por ejemplo, puedes usar las notificaciones push dirigidas para sugerir a los clientes que realicen una determinada acción, como valorar tu aplicación o probar una nueva característica. Puedes enviar diferentes tipos de notificaciones de inserción, incluidas notificaciones del sistema, notificaciones de icono y notificaciones XML sin formato. También puedes supervisar la tasa de inicios de la aplicación que se derivan de las notificaciones de inserción. Para obtener más información acerca de esta característica, consulta [Enviar notificaciones de inserción a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md).

Para poder enviar notificaciones de envío de destino a los clientes desde el centro de Partners, debe usar un método de la clase [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) en el SDK de Microsoft Store Services para registrar la aplicación para recibir notificaciones. Puede usar métodos adicionales de esta clase para notificar al centro de partners que la aplicación se ha iniciado como respuesta a una notificación de envío de destino (si desea realizar un seguimiento de la tasa de lanzamientos de la aplicación que se han producido en las notificaciones) y para dejar de recibir notificaciones.

## <a name="configure-your-project"></a>Configurar el proyecto

Antes de escribir código, sigue estos pasos para agregar una referencia al Microsoft Store Services SDK en tu proyecto:

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo. 
2. Abra el proyecto en Visual Studio.
3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia** .
4. En el cuadro de diálogo **Administrador de referencias** , expande **Windows Universal** y haz clic en **Extensiones** .
5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar** .

## <a name="register-for-push-notifications"></a>Registro de notificaciones push

Para registrar la aplicación para recibir notificaciones de envío de destino desde el centro de Partners:

1. En el proyecto, busque una sección de código que se ejecute durante el inicio en el que puede registrar la aplicación para recibir notificaciones.
2. Agrega esta instrucción en la parte superior del archivo de código.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="EngagementNamespace":::

3. Obtén un objeto [StoreServicesEngagementManager](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager) y llama a una de las sobrecargas [RegisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) del código de inicio que identificaste anteriormente. Se llamará a este método cada vez que se inicie la aplicación.

  * Si desea que el centro de Partners cree su propio URI de canal para las notificaciones, llame a la sobrecarga [RegisterNotificationChannelAsync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) .

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync1":::
      > [!IMPORTANT]
      > Si la aplicación también llama a [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) para crear un canal de notificación para WNS, asegúrese de que el código no llama a [CreatePushNotificationChannelForApplicationAsync](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager.createpushnotificationchannelforapplicationasync) y la sobrecarga [RegisterNotificationChannelAsync ()](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) simultáneamente. Si necesitas llamar a estos dos métodos, llámalos secuencialmente y espera la devolución de un método antes de llamar al otro.

  * Si desea especificar el URI de canal que se va a usar para las notificaciones de envío de destino del centro de Partners, llame a la sobrecarga de [RegisterNotificationChannelAsync (StoreServicesNotificationChannelParameters)](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.registernotificationchannelasync) . Por ejemplo, es posible que quieras hacer esto si tu aplicación ya usa Servicios de notificaciones de inserción de Windows (WNS) y quieres usar el mismo URI de canal. Primero debes crear un objeto [StoreServicesNotificationChannelParameters](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters) y asignar la propiedad [CustomNotificationChannelUri](/uwp/api/microsoft.services.store.engagement.storeservicesnotificationchannelparameters.customnotificationchanneluri) al URI de tu canal.

      :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="RegisterNotificationChannelAsync2":::

> [!NOTE]
> Cuando se llama al método **RegisterNotificationChannelAsync** , se crea un archivo denominado MicrosoftStoreEngagementSDKId.txt en el almacén de datos de la aplicación local para la aplicación (la carpeta devuelta por la propiedad [ApplicationData. LocalFolder](/uwp/api/Windows.Storage.ApplicationData.LocalFolder) ). Este archivo contiene un identificador que usa la infraestructura de notificaciones de envío de destino. Asegúrese de que la aplicación no modifique ni elimine este archivo. De lo contrario, los usuarios pueden recibir varias instancias de notificaciones, o bien es posible que las notificaciones no se comporten correctamente de otras maneras.

<span id="notification-customers" />

### <a name="how-targeted-push-notifications-are-routed-to-customers"></a>Cómo se enrutan las notificaciones de envío de destino a los clientes

Cuando la aplicación llama a **RegisterNotificationChannelAsync** , este método recopila el cuenta de Microsoft del cliente que ha iniciado sesión actualmente en el dispositivo. Más adelante, cuando se envía una notificación de envío de destino a un segmento que incluye a este cliente, el centro de Partners envía la notificación a los dispositivos asociados al cuenta de Microsoft de este cliente.

Si el cliente que inició la aplicación proporciona su dispositivo a otra persona para usar mientras todavía está conectado al dispositivo con sus cuenta de Microsoft, tenga en cuenta que la otra persona puede ver la notificación dirigida al cliente original. Esto puede tener consecuencias no deseadas, especialmente en el caso de las aplicaciones que ofrecen servicios que los clientes pueden iniciar sesión para usar. Para evitar que otros usuarios vean las notificaciones de destino en este escenario, llame al método [UnregisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) cuando los clientes cierren sesión en la aplicación. Para obtener más información, consulte [anulación del registro para las notificaciones de envío](#unregister) más adelante en este artículo.

### <a name="how-your-app-responds-when-the-user-launches-your-app"></a>Cómo responde la aplicación cuando el usuario inicia la aplicación

Una vez que la aplicación se ha registrado para recibir notificaciones y [envía una notificación de inserción a los clientes de la aplicación desde el centro de Partners](../publish/send-push-notifications-to-your-apps-customers.md), se llamará a uno de los siguientes puntos de entrada de la aplicación cuando el usuario inicie la aplicación en respuesta a la notificación de inserción. Si tienes código que quieres que se ejecute cuando el usuario inicie la aplicación, puedes agregar el código a uno de estos puntos de entrada de tu aplicación.

  * Si la notificación de inserción tiene un tipo de activación en primer plano, anula el método [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) de la clase **App** de tu proyecto y agrega el código a este método.

  * Si la notificación de inserción tiene un tipo de activación en segundo plano, agrega el código al método [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) para la [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md).

Por ejemplo, es posible que quieras recompensar a los usuarios de la aplicación que hayan adquirido complementos de pago en la aplicación regalándoles un complemento gratuito. En este caso, puedes enviar una notificación de inserción a un [segmento de clientes](../publish/create-customer-segments.md) que esté dirigida específicamente a esos usuarios. A continuación, puedes agregar código para regalarles una [compra desde la aplicación](in-app-purchases-and-trials.md) gratuita en uno de los puntos de entrada señalados anteriormente.

## <a name="notify-partner-center-of-your-app-launch"></a>Notificar al centro de partners del inicio de la aplicación

Si selecciona la opción de seguimiento de la **frecuencia de inicio** de la aplicación para la notificación de inserción de destino en el centro de Partners, llame al método [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) desde el punto de entrada adecuado en la aplicación para notificar al centro de partners que la aplicación se inició en respuesta a una notificación de inserción.

Este método devuelve también los argumentos de inicio originales de la aplicación. Si decide realizar el seguimiento de la tasa de inicio de la aplicación de la notificación de envío, se agrega un identificador de seguimiento opaco a los argumentos de inicio para ayudar a realizar un seguimiento del inicio de la aplicación en el centro de Partners. Debe pasar los argumentos de inicio de la aplicación al método [ParseArgumentsAndTrackAppLaunch](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.parseargumentsandtrackapplaunch) y este método envía el identificador de seguimiento al centro de Partners, quita el identificador de seguimiento de los argumentos de inicio y devuelve los argumentos de inicio originales al código.

La forma en que se llama a este método depende del tipo de activación de la notificación de extracción:

* Si la notificación de inserción tiene un tipo de activación en primer plano, llama a este método desde la invalidación del método [OnActivated](/uwp/api/windows.ui.xaml.application.onactivated) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActivatedEventArgs](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement** y **Windows.ApplicationModel.Activation** .

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/App.xaml.cs" id="OnActivated":::

* Si la notificación de inserción tiene un tipo de activación en segundo plano, llama a este método desde el método [Run](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) para tu [tarea en segundo plano](../launch-resume/support-your-app-with-background-tasks.md) y pasa los argumentos que están disponibles en el objeto [ToastNotificationActionTriggerDetail](/uwp/api/Windows.UI.Notifications.ToastNotificationActionTriggerDetail) que se pasa a este método. El siguiente ejemplo de código da por hecho que el archivo de código tiene instrucciones **using** para los espacios de nombres **Microsoft.Services.Store.Engagement** , **Windows.ApplicationModel.Background** y **Windows.UI.Notifications** .

  :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="Run":::

<span id="unregister" />

## <a name="unregister-for-push-notifications"></a>Anular el registro para notificaciones de inserción

Si desea que la aplicación deje de recibir notificaciones de envío de destino del centro de Partners, llame al método [UnregisterNotificationChannelAsync](/uwp/api/microsoft.services.store.engagement.storeservicesengagementmanager.unregisternotificationchannelasync) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/DevCenterNotifications.cs" id="UnregisterNotificationChannelAsync":::

Ten en cuenta que este método invalida el canal que se esté utilizando para las notificaciones, de manera que la aplicación ya no recibirá notificaciones de inserción de *ningún* servicio. Una vez que se ha cerrado, no se puede volver a usar el canal para ningún servicio, incluidas las notificaciones de envío de destino del centro de Partners y otras notificaciones que usan WNS. Para reanudar el envío de notificaciones de inserción a esta aplicación, la aplicación debe solicitar un nuevo canal.

## <a name="related-topics"></a>Temas relacionados

* [Enviar notificaciones de inserción a los clientes de la aplicación](../publish/send-push-notifications-to-your-apps-customers.md)
* [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)
* [Cómo solicitar, crear y guardar un canal de notificación](/previous-versions/windows/apps/hh868221(v=win.10))
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
