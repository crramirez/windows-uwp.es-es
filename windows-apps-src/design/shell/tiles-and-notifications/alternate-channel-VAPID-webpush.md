---
title: Canales de inserción alternativo mediante Webpush y VAPID en UWP
description: Instrucciones para utilizar los canales de inserción alternativo con el protocolo VAPID desde una aplicación para UWP
ms.date: 01/10/2017
ms.topic: article
keywords: API de Windows 10, uwp, WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: ba8630a2e877345adeac7eb443dd3e418d3ed277
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639530"
---
# <a name="alternate-push-channels-using-webpush-and-vapid-in-uwp"></a>Canales de inserción alternativo mediante Webpush y VAPID en UWP 
A partir de la actualización Fall Creators Update, las aplicaciones para UWP pueden usar inserción web con autenticación VAPID para enviar notificaciones push.  

## <a name="introduction"></a>Introducción
La introducción del estándar web inserción permite que los sitios Web pueden actuar más igual que las aplicaciones, enviar notificaciones, incluso cuando los usuarios no están en el sitio Web.

El protocolo de autenticación VAPID se creó para permitir que los sitios Web para autenticarse con los servidores de inserción en un proveedor de manera independiente. Con todos los proveedores mediante el protocolo VAPID, sitios Web pueden enviar notificaciones de inserción sin conocer el explorador en el que se está ejecutando. Esto es una mejora considerable sobre la implementación de un protocolo de inserción diferentes para cada plataforma. 

Las aplicaciones para UWP pueden usar webpush y VAPID para enviar notificaciones push con estas ventajas, también. Estos protocolos pueden ahorrar tiempo de desarrollo de nuevas aplicaciones y simplificar el soporte técnico de multiplataforma para aplicaciones existentes. Además, las aplicaciones empresariales o las aplicaciones ahora pueden enviar notificaciones sin necesidad de registrarse en la Microsoft Store. Con suerte, esto abre nuevas formas de ponerse en contacto con los usuarios en todas las plataformas.  

## <a name="alternate-channels"></a>Canales alternativos 
En UWP, estos canales VAPID se denominan canales alternativos y proporcionan una funcionalidad similar a un canal de inserción de la web. Puede desencadenar una tarea en segundo plano de aplicación para ejecutar, habilite el cifrado de mensajes y permiten varios canales desde una única aplicación. Para obtener más información sobre la diferencia entre los tipos de canal diferentes, consulte [elegir el canal derecho](channel-types.md).

Uso de canales alternativos es una excelente manera de obtener acceso a las notificaciones de inserción si la aplicación no puede usar un canal principal o si desea compartir código entre el sitio Web y la aplicación. Configuración de un canal es sencillo y familiar a cualquiera que se usa el estándar de inserción de web o trabajado con Windows antes de las notificaciones de inserción.

## <a name="code-example"></a>Ejemplo de código

El proceso básico de cómo configurar un canal alternativo para una aplicación UWP es similar a la configuración de un canal principal o secundario. En primer lugar, registrar un canal con el [server WNS](windows-push-notification-services--wns--overview.md). A continuación, registrar para ejecutarse como una tarea en segundo plano. Después de la notificación se envía y se desencadena la tarea en segundo plano, controle el evento.  

### <a name="get-a-channel"></a>Obtener un canal 
Para crear un canal alternativo, la aplicación debe proporcionar dos fragmentos de información: la clave pública para su servidor y el nombre del canal se está creando. Los detalles acerca de las claves de servidor están disponibles en las especificaciones de inserción de web, pero se recomienda usar una biblioteca de inserción web estándar en el servidor para generar las claves.  

Id. del canal es especialmente importante porque una aplicación puede crear varios canales alternativos. Cada canal debe identificarse mediante un identificador único que se incluirá en las cargas de notificación enviadas por ese canal.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
La aplicación envía el canal de copia de seguridad en su servidor y lo guarda localmente. Guardar localmente el Id. del canal permite que la aplicación diferenciar entre los canales y renovar los canales con el fin de evitar que expiren.

Como cada otro tipo de canal de notificación de inserción, pueden caducar canales web. Para evitar que los canales expiren sin su conocimiento de aplicación, crear un nuevo canal cada vez que se inicia la aplicación.    

### <a name="register-for-a-background-task"></a>Regístrese para una tarea en segundo plano 

Una vez que la aplicación ha creado un canal alternativo, se debe registrar para recibir las notificaciones en primer plano o en segundo plano. El ejemplo siguiente se registra para usar el modelo de proceso de uno para recibir las notificaciones en segundo plano.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Recibir las notificaciones 

Una vez que la aplicación se ha registrado para recibir las notificaciones, debe ser capaz de procesar las notificaciones entrantes. Puesto que una sola aplicación puede registrar varios canales, asegúrese de comprobar el Id. del canal antes de procesar la notificación.  

```csharp
protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args) 
{ 
    base.OnBackgroundActivated(args); 
    var raw = args.TaskInstance.TriggerDetails as RawNotification; 
    switch (raw.ChannelId) 
    { 
        case "Football": 
            AppPostFootballScore(raw.Content); 
            break; 
        case "News": 
            AppProcessNewsItem(raw.Content); 
            break; 
        case "Baseball": 
            AppProcessBaseball(raw.Content); 
            break; 
 
        default: 
            //We don't have the channelID registered, should only happen in the case of a 
            //caching issue on the application server 
            break; 
    }                           
} 
```

Tenga en cuenta que si la notificación proceden de un canal principal, a continuación, el Id. del canal no se establecerá.  

## <a name="note-on-encryption"></a>Tenga en cuenta acerca del cifrado 

Puede utilizar cualquier esquema de cifrado que considere más útiles para la aplicación. En algunos casos, es suficiente para que se basan en el estándar TLS entre el servidor y cualquier dispositivo de Windows. En otros casos, es posible que más sentido usar el esquema de cifrado de la inserción de web o de otro esquema de diseño.  

Si desea usar otra forma de cifrado, la clave es el uso sin formato. Propiedad Headers. Contiene todos los encabezados de cifrado que se incluyeron en la solicitud POST para el servidor de inserción. Desde allí, la aplicación puede usar las claves para descifrar el mensaje.  

## <a name="related-topics"></a>Temas relacionados
- [Tipos de canal de notificación](channel-types.md)
- [Servicios de notificación de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Clase PushNotificationChannel](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [Clase PushNotificationChannelManager](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)


