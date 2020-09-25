---
title: Canales de inserciones alternativos mediante VAPID en UWP
description: Instrucciones para usar canales de envío alternativos con el protocolo VAPID desde una aplicación de Windows
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, API de WinRT, WNS
localizationpriority: medium
ms.openlocfilehash: 79ea88cb457e9a0d7ba33ef51a184e6f52ab19c4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219278"
---
# <a name="alternate-push-channels-using-vapid-in-windows"></a>Canales de inserciones alternativos mediante VAPID en Windows 
A partir de la actualización de Fall Creators, las aplicaciones de Windows pueden usar la autenticación VAPID para enviar notificaciones de envío.  

> [!NOTE]
> Estas API están pensadas para los exploradores Web que hospedan otros sitios web y crean canales en su nombre.  Si desea agregar notificaciones de webtrigger a la aplicación Web, le recomendamos que siga los estándares W3C y WhatWG para crear un trabajo de servicio y enviar una notificación.

## <a name="introduction"></a>Introducción
La introducción del estándar de inserción web permite que los sitios web puedan actuar más como las aplicaciones y enviar notificaciones incluso cuando los usuarios no están en el sitio Web.

El protocolo de autenticación VAPID se creó para permitir que los sitios web se autentiquen con servidores de inserciones de manera independiente del proveedor. Con todos los proveedores que usan el protocolo VAPID, los sitios web pueden enviar notificaciones de envío sin conocer el explorador en el que se está ejecutando. Se trata de una mejora significativa en lo que se refiere a la implementación de un protocolo de extracción diferente para cada plataforma. 

Las aplicaciones de Windows pueden usar VAPID para enviar notificaciones de envío con estas ventajas también. Estos protocolos pueden ahorrar tiempo de desarrollo para aplicaciones nuevas y simplificar la compatibilidad entre plataformas de las aplicaciones existentes. Además, las aplicaciones empresariales o las aplicaciones con instalación de prueba ahora pueden enviar notificaciones sin registrarse en el Microsoft Store. Afortunadamente, se abrirán nuevas formas de interactuar con los usuarios en todas las plataformas.  

## <a name="alternate-channels"></a>Canales alternativos 
En UWP, estos canales VAPID se denominan canales alternativos y proporcionan una funcionalidad similar a un canal de extracción Web. Pueden desencadenar la ejecución de una tarea en segundo plano de la aplicación, habilitar el cifrado de mensajes y permitir varios canales desde una sola aplicación. Para obtener más información acerca de la diferencia entre los distintos tipos de canales, consulte [elección del canal adecuado](channel-types.md).

El uso de canales alternativos es una excelente manera de acceder a las notificaciones de envío si la aplicación no puede usar un canal principal o si desea compartir código entre su sitio web y la aplicación. La configuración de un canal es fácil y familiar para cualquier persona que haya usado el estándar de inserciones web o haya trabajado antes con las notificaciones de Windows.

## <a name="code-example"></a>Ejemplo de código

El proceso básico de configuración de un canal alternativo para una aplicación de Windows es similar a la configuración de un canal principal o secundario. En primer lugar, Regístrese para obtener un canal con el [servidor WNS](windows-push-notification-services--wns--overview.md). A continuación, Regístrese para ejecutarse como una tarea en segundo plano. Después de enviar la notificación y de desencadenar la tarea en segundo plano, controle el evento.  

### <a name="get-a-channel"></a>Obtener un canal 
Para crear un canal alternativo, la aplicación debe proporcionar dos fragmentos de información: la clave pública para su servidor y el nombre del canal que está creando. Los detalles sobre las claves de servidor están disponibles en la especificación de la extracción Web, pero se recomienda usar una biblioteca de inserciones web estándar en el servidor para generar las claves.  

El identificador de canal es especialmente importante porque una aplicación puede crear varios canales alternativos. Cada canal debe identificarse mediante un identificador único que se incluirá con las cargas de notificación enviadas a lo largo de ese canal.  

```csharp
private async void AppCreateVAPIDChannelAsync(string appChannelId, IBuffer applicationServerKey) 
{ 
    // From the spec: applicationServer Key (p256dh):  
    //               An Elliptic curve Diffie–Hellman public key on the P-256 curve 
    //               (that is, the NIST secp256r1 elliptic curve).   
    //               The resulting key is an uncompressed point in ANSI X9.62 format             
    // ChannelId is an app provided value for it to identify the channel later.  
    // case of this app it is from the set { "Football", "News", "Baseball" } 
    PushNotificationChannel webChannel = await PushNotificationChannelManager.GetDefault().CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId); 
 
    //Save the channel  
    AppUpdateChannelMapping(appChannelId, webChannel); 
             
    //Tell our web service that we have a new channel to push notifications to 
    AppPassChannelToSite(webChannel.Uri); 
} 
```
La aplicación envía la copia de seguridad del canal a su servidor y la guarda localmente. Guardar el identificador de canal localmente permite a la aplicación diferenciar los canales y renovar los canales para evitar que expiren.

Al igual que todos los demás tipos de canal de notificaciones de envío, los canales web pueden expirar. Para evitar que los canales expiren sin que la aplicación sepa, cree un nuevo canal cada vez que se inicie la aplicación.    

### <a name="register-for-a-background-task"></a>Registrarse para una tarea en segundo plano 

Una vez que la aplicación haya creado un canal alternativo, debe registrarse para recibir las notificaciones en primer plano o en segundo plano. En el ejemplo siguiente se registra para usar el modelo de un proceso para recibir las notificaciones en segundo plano.  

```csharp
var builder = new BackgroundTaskBuilder(); 
builder.Name = "Push Trigger"; 
builder.SetTrigger(new PushNotificationTrigger()); 
BackgroundTaskRegistration task = builder.Register(); 
```
### <a name="receive-the-notifications"></a>Recibir notificaciones 

Una vez que la aplicación se ha registrado para recibir notificaciones, debe ser capaz de procesar las notificaciones entrantes. Puesto que una sola aplicación puede registrar varios canales, asegúrese de comprobar el identificador del canal antes de procesar la notificación.  

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

Tenga en cuenta que si la notificación procede de un canal principal, no se establecerá el identificador de canal.  

## <a name="note-on-encryption"></a>Nota sobre el cifrado 

Puede usar cualquier esquema de cifrado que le resulte más útil para su aplicación. En algunos casos, es suficiente confiar en el estándar de TLS entre el servidor y cualquier dispositivo de Windows. En otros casos, es posible que tenga más sentido usar el esquema de cifrado de la extracción Web u otro esquema del diseño.  

Si desea utilizar otra forma de cifrado, la clave es usar el formato sin procesar. Propiedad headers. Contiene todos los encabezados de cifrado que se incluyeron en la solicitud POST en el servidor de inserciones. Desde allí, la aplicación puede usar las claves para descifrar el mensaje.  

## <a name="related-topics"></a>Temas relacionados
- [Tipos de canales de notificaciones](channel-types.md)
- [Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md)
- [Clase PushNotificationChannel](/uwp/api/windows.networking.pushnotifications.pushnotificationchannel)
- [Clase PushNotificationChannelManager](/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)
