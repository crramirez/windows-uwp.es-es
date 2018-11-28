---
Description: The Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. This provides a mechanism to deliver new updates to your users in a power-efficient and dependable way.
title: Introducción a los Servicios de notificaciones de inserción de Windows (WNS)
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f131ad229b4ba22f7fa4652aa302e3596819f206
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7845672"
---
# <a name="windows-push-notification-services-wns-overview"></a>Introducción a los Servicios de notificaciones de inserción de Windows (WNS)
 

Con los Servicios de notificaciones de inserción de Windows (WNS), los desarrolladores de terceros pueden enviar actualizaciones de notificaciones del sistema, de icono, de distintivo y sin procesar desde su propio servicio de nube. Esto proporciona un mecanismo para enviar nuevas actualizaciones a los usuarios de una manera segura y de bajo consumo.

## <a name="how-it-works"></a>Cómo funciona


En el siguiente diagrama se muestra el flujo completo de datos para el envío de una notificación de inserción. Esto conlleva los siguientes pasos:

1.  La aplicación solicita un canal de notificación de inserción de la plataforma universal de Windows.
2.  Windows solicita a WNS que cree un canal de notificación. que será devuelto al dispositivo de llamada en forma de identificador uniforme de recursos (URI).
3.  Windows devuelve el URI del canal de notificación a tu aplicación.
4.  Tu aplicación envía el URI a tu propio servicio de nube. A continuación, debes almacenar el URI en tu propio servicio de nube para poder acceder al URI al enviar notificaciones. El URI es una interfaz entre tu propia aplicación y tu propio servicio; es responsabilidad tuya implementar esta interfaz con estándares web seguros y fiables.
5.  Cuando tu servicio de nube tenga una actualización por enviar, WNS recibe una notificación mediante el URI del canal. Para ello, se emite una solicitud HTTPPOST, incluida la carga de notificación, a través de la Capa de sockets seguros (SSL). Este paso requiere autenticación.
6.  WNS recibe la solicitud y enruta la notificación hacia el dispositivo pertinente.

![Diagrama de flujo de datos WNS para notificaciones de inserción](images/wns-diagram-01.png)

## <a name="registering-your-app-and-receiving-the-credentials-for-your-cloud-service"></a>Registro de una aplicación y recepción de las credenciales para el servicio de nube


Antes de enviar notificaciones con WNS, la aplicación debe registrarse en el panel de la Tienda. Esto te proporcionará las credenciales de la aplicación que tu servicio en la nube usará en la autenticación con WNS. Estas credenciales son un identificador de seguridad de paquete (SID) y una clave secreta. Para realizar este registro, inicia sesión en [El centro de partners](https://partner.microsoft.com/dashboard). Después de crear la aplicación, puedes recuperar las credenciales siguiendo las instrucciones de la página **Administración de aplicaciones: WNS/MPNS**. Si quieres usar la solución Servicios de Live, sigue el vínculo del **sitio de Servicios Live** de esta página.

Cada aplicación tiene su propio conjunto de credenciales para su servicio de nube. Estas credenciales no se pueden usar para enviar notificaciones a cualquier otra aplicación.

Para obtener más información acerca del registro de una aplicación, consulta el tema sobre [cómo autenticar con el Servicio de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407).

## <a name="requesting-a-notification-channel"></a>Solicitud de un canal de notificación


Cuando se ejecuta una aplicación que puede recibir notificaciones de inserción, primero debe solicitar un canal de notificación mediante [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager#Windows_Networking_PushNotifications_PushNotificationChannelManager_CreatePushNotificationChannelForApplicationAsync_System_String_). Para obtener una explicación detallada y un código de ejemplo, consulta el tema sobre [cómo solicitar, crear y guardar un canal de notificación](https://msdn.microsoft.com/library/windows/apps/hh465412). Esta API devuelve un URI de canal que está vinculado exclusivamente a la aplicación que llama y su icono, y a través del cual pueden enviarse todos los tipos de notificaciones.

Después de crear correctamente un URI de canal, la aplicación lo envía a su servicio de nube junto con metadatos específicos que deben asociarse con este URI.

### <a name="important-notes"></a>Notas importantes

-   No garantizamos que el URI de canal de notificación de una aplicación siempre sea el mismo. Recomendamos que la aplicación solicite un canal nuevo cada vez que se ejecute y que actualice su servicio cuando el URI cambie. El desarrollador nunca debe cambiar el URI de canal; debe considerarlo como una cadena de caja negra. En este momento, los URI de canal expiran después de 30 días. Si la aplicación de Windows 10 renueva periódicamente su canal en segundo plano, a continuación, puedes descargar la [muestra de inserción y las notificaciones periódicas](http://go.microsoft.com/fwlink/p/?linkid=231476) para Windows8.1 y volver a usar su código fuente y el patrón que muestra.
-   El desarrollador es quien implementa la interfaz entre el servicio de nube y la aplicación cliente. Recomendamos que la aplicación pase por un proceso de autenticación con su propio servicio y transmita datos en un protocolo seguro, como HTTPS.
-   Es importante que el servicio de nube siempre asegure que el URI de canal use el dominio "notify.windows.com". El servicio nunca debe insertar notificaciones en un canal en otro dominio. Si alguna vez la devolución de llamada de la aplicación se ve comprometida, un atacante malintencionado podría enviar un URI de canal para suplantar WNS. Si no se inspecciona el dominio, el servicio de nube podría revelar información a este atacante sin saberlo.
-   Si tu servicio de nube intenta enviar una notificación a un canal expirado, WNS devolverá el [código de respuesta 410](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#WNSResponseCodes). En respuesta a ese código, tu servicio no debe seguir intentando enviar notificaciones a ese URI.

## <a name="authenticating-your-cloud-service"></a>Autenticación del servicio de nube


Para enviar una notificación, se debe autenticar el servicio de nube con WNS. El primer paso en este proceso se realiza cuando registras tu aplicación con el Panel de la Microsoft Store. Durante el proceso de registro, la aplicación recibe un identificador de seguridad de paquete (SID) y una clave secreta. El servicio de nube usa esta información para autenticar con WNS.

El esquema de autenticación de WNS se implementa mediante el perfil de credenciales de cliente del protocolo [OAuth 2.0](http://go.microsoft.com/fwlink/p/?linkid=226787). El servicio de nube realiza la autenticación con WNS al proporcionar sus credenciales (SID de paquete y clave secreta). A cambio recibe un token de acceso. Este token de acceso permite al servicio de nube enviar una notificación. El token se requiere con cada solicitud de notificación enviada a WNS.

En un nivel alto, la cadena de información es la siguiente:

1.  El servicio de nube envía sus credenciales a WNS mediante HTTPS siguiendo el protocolo OAuth 2.0. Esto autentica el servicio con WNS.
2.  WNS devuelve un token de acceso, si la autenticación fue correcta. Este token de acceso se usa en solicitudes de notificación subsiguientes hasta que expire.

![Diagrama WNS para la autenticación del servicio de nube](images/wns-diagram-02.png)

En la autenticación con WNS, el servicio de nube envía una solicitud HTTP en una capa de sockets seguros (SSL). Los parámetros se proporcionan en el formato "aplicación/x-www-formato-urlcodificada". Indica tu SID de paquete en el campo "client\_id" y tu clave secreta en el campo "client\_secret". Para obtener detalles de sintaxis, consulta la referencia sobre la [solicitud de token de acceso](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#access_token_request).

**Nota**esto es solo un ejemplo, no copiar y pegar el código que puedes usar correctamente en su propio código.

 

``` http
 POST /accesstoken.srf HTTP/1.1
 Content-Type: application/x-www-form-urlencoded
 Host: https://login.live.com
 Content-Length: 211
 
 grant_type=client_credentials&client_id=ms-app%3a%2f%2fS-1-15-2-2972962901-2322836549-3722629029-1345238579-3987825745-2155616079-650196962&client_secret=Vex8L9WOFZuj95euaLrvSH7XyoDhLJc7&scope=notify.windows.com
```

WNS autentica el servicio de nube y, si es correcto, envía una respuesta de "200 Correcto". El token de acceso se devuelve en los parámetros incluidos en el cuerpo de la respuesta HTTP, mediante el tipo de medios "application/json". Una vez que el servicio recibe el token de acceso, estás listo para enviar notificaciones.

El siguiente ejemplo muestra una respuesta de autenticación correcta, incluido el token de acceso. Para obtener detalles de sintaxis, consulta el tema sobre la [solicitud de servicio de notificaciones de inserción y encabezados de respuesta](https://msdn.microsoft.com/library/windows/apps/hh465435).

``` http
 HTTP/1.1 200 OK   
 Cache-Control: no-store
 Content-Length: 422
 Content-Type: application/json
 
 {
     "access_token":"EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=", 
     "token_type":"bearer"
 }
```

### <a name="important-notes"></a>Notas importantes

-   El protocolo OAuth 2.0 admitido en este procedimiento sigue la versión de borrador V16.
-   Las Solicitudes de comentarios (RFC) de OAuth usan el término "cliente" para referirse al servicio de nube.
-   Es posible que haya cambios en este procedimiento cuando se finalice el borrador de OAuth.
-   El token de acceso se puede volver a usar para varias solicitudes de notificación. Esto permite al servicio de nube autenticar solo una vez para enviar varias notificaciones. Sin embargo, cuando el token de acceso expira, el servicio de nube debe volver a autenticar para recibir un token de acceso nuevo.

## <a name="sending-a-notification"></a>Envío de una notificación


Mediante el URI de canal, el servicio de nube puede enviar una notificación siempre que tenga una actualización para el usuario.

El token de acceso descrito anteriormente puede volver a usarse para varias solicitudes de notificación; no es necesario que el servicio de nube solicite un token de acceso nuevo para cada notificación. Si el token de acceso expiró, la solicitud de notificación devolverá un error. Recomendamos que no intentes reenviar tu notificación más de una vez, si se rechaza el token de acceso. Si encuentras este error, deberás solicitar un token de acceso nuevo y reenviar la notificación. Para ver el código de error exacto, consulta el tema sobre [códigos de respuesta de una notificación de inserción](https://msdn.microsoft.com/library/windows/apps/hh465435).

1.  El servicio de nube envía una solicitud HTTP POST al URI de canal. Esta solicitud debe hacerse en SSL y contener los encabezados necesarios, así como la carga de la notificación. El encabezado de autorización debe incluir el token de acceso adquirido para la autorización.

    Aquí te mostramos un ejemplo de solicitud. Para obtener detalles de sintaxis, consulta el tema sobre [códigos de respuesta de una notificación de inserción](https://msdn.microsoft.com/library/windows/apps/hh465435).

    Para obtener detalles sobre cómo redactar la carga de notificación, consulta [Inicio rápido: envío de una notificación de inserción](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252). La carga de una notificación, una notificación del sistema o una notificación de icono se suministra como contenido XML conforme a su [Esquema de iconos adaptativos](adaptive-tiles-schema.md) o el [Esquema de iconos heredados](https://msdn.microsoft.com/library/windows/apps/br212853). Por su parte, la carga de una notificación sin procesar carece de una estructura específica, ya que se define estrictamente para cada aplicación.

    ``` http
     POST https://cloud.notify.windows.com/?token=AQE%bU%2fSjZOCvRjjpILow%3d%3d HTTP/1.1
     Content-Type: text/xml
     X-WNS-Type: wns/tile
     Authorization: Bearer EgAcAQMAAAAALYAAY/c+Huwi3Fv4Ck10UrKNmtxRO6Njk2MgA=
     Host: cloud.notify.windows.com
     Content-Length: 24

     <body>
     ....
    ```

2.  WNS responde para indicar que la notificación se recibió y que será enviada en la próxima oportunidad disponible. Sin embargo, WNS no proporciona una confirmación de extremo a extremo sobre la recepción de la notificación en el dispositivo o aplicación.

En este diagrama se muestra el flujo de datos:

![Diagrama WNS para enviar una notificación](images/wns-diagram-03.png)

### <a name="important-notes"></a>Notas importantes

-   WNS no garantiza la confiabilidad ni la latencia de una notificación.
-   Las notificaciones nunca deben incluir información confidencial.
-   Para enviar una notificación, el servicio de nube primero debe autenticar con WNS y recibir un token de acceso.
-   Un token de acceso solo permite a un servicio de nube enviar notificaciones a una sola aplicación para la cual se creó ese token. No se puede usar un mismo token de acceso para enviar notificaciones entre varias aplicaciones. Por lo tanto, si tu servicio de nube admite varias aplicaciones, debe proporcionarse el token de acceso correcto de la aplicación al insertarse una notificación en cada URI de canal.
-   Cuando el dispositivo no tenga conexión, WNS almacenará de manera predeterminada hasta cinco notificaciones de icono (si la cola está habilitada; de lo contrario, almacenará una sola) y una notificación de distintivo para cada URI de canal y notificaciones sin procesar. Este comportamiento de almacenamiento en caché predeterminado puede modificarse a través del encabezado [Directiva-de-memoria-caché-de-X-WNS](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_cache). Ten en cuenta que las notificaciones del sistema nunca se almacenan cuando el dispositivo está sin conexión.
-   En escenarios donde el contenido de una notificación está personalizado para el usuario, WNS recomienda que el servicio de nube envíe esas actualizaciones inmediatamente apenas se reciben. Entre los ejemplos de este escenario, se incluyen actualizaciones de fuentes de medios sociales, invitaciones de comunicación instantánea, notificaciones de mensaje nuevo o alertas. De forma alternativa, puedes tener escenarios en los que la misma actualización genérica se envía con frecuencia a un gran subconjunto de usuarios; por ejemplo, pronóstico del tiempo, cotizaciones y nuevas actualizaciones. Las instrucciones de WNS especifican que la frecuencia de estas actualizaciones debe ser de una cada 30 minutos, como máximo. El usuario final o WNS podrían considerarlas abusivas si se envían con mayor frecuencia.

## <a name="expiration-of-tile-and-badge-notifications"></a>Expiración de las notificaciones de icono y de distintivo


De forma predeterminada, las notificaciones de icono y de distintivo expiran después de su descarga. Cuando una notificación expira, el contenido se quita del icono o de la cola y no se vuelve a mostrar al usuario. Se recomienda establecer una caducidad (con un tiempo apropiado para tu aplicación) en todas las notificaciones de icono y distintivo de modo que el contenido del icono no persista más allá de su relevancia. El tiempo de caducidad explícito resulta esencial para contenido con una vida útil definida. Esto también garantiza la eliminación de contenido obsoleto si el servicio de nube deja de enviar notificaciones o si el usuario se desconecta de la red durante un período de tiempo prolongado.

Tu servicio de nube puede establecer una caducidad para cada notificación al ajustar el encabezado X-WNS-TTL para que especifique el tiempo (en segundos) que tu notificación permanecerá válida después de su envío. Si quieres obtener más información, consulta el tema sobre [encabezados de respuesta y solicitud del servicio de notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_ttl).

Por ejemplo, durante un día de gran actividad en el mercado de valores, puedes establecer la caducidad para la actualización del precio de unas acciones en el doble del intervalo de envío (por ejemplo, una hora después de la recepción si estás enviando notificaciones cada media hora). Otro ejemplo sería una aplicación de noticias, que podría determinar que un día es un tiempo de caducidad apropiado para una actualización diaria del icono de noticias.

## <a name="push-notifications-and-battery-saver"></a>Notificaciones de inserción y ahorro de batería


El ahorro de batería amplía la duración de la batería limitando la actividad en segundo plano en el dispositivo. Windows 10 permite al usuario establecer el ahorro de batería que se active automáticamente cuando la batería cae por debajo de un umbral especificado. Cuando el ahorro de batería está activado, se deshabilita la recepción de notificaciones de inserción para ahorrar energía. Sin embargo, hay algunas excepciones. La siguiente configuración de ahorro de batería de Windows 10 (que se encuentra en la aplicación **configuración** ) permite que la aplicación recibir notificaciones de inserción, incluso cuando el ahorro de batería está activado.

-   **Permitir las notificaciones de inserción desde cualquier aplicación en el modo de ahorro de batería**: esta configuración permite que todas las aplicaciones reciban notificaciones de inserción aunque esté activado el ahorro de batería. Ten en cuenta que esta opción se aplica solo a Windows 10 para ediciones de escritorio (Home, Pro, Enterprise y Education).
-   **Siempre permitido**: esta opción permite que ciertas aplicaciones se ejecuten en segundo plano mientras el ahorro de batería está activado (recepción de notificaciones de inserción incluida). El usuario es quien debe mantener manualmente esta lista.

No hay ninguna forma de comprobar el estado de estos dos valores, pero puedes comprobar el estado del ahorro de batería. En Windows 10, usa la propiedad [**EnergySaverStatus**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatus) para comprobar el estado del ahorro de batería. La aplicación también puede usar el evento [**EnergySaverStatusChanged**](https://docs.microsoft.com/uwp/api/Windows.System.Power.PowerManager.EnergySaverStatusChanged) para detectar cambios en el ahorro de batería.

Si la aplicación depende en gran medida de las notificaciones de inserción, te recomendamos que notifiques a los usuarios que no pueden recibir notificaciones mientras esté activado el ahorro de batería y para que les resulte más fácil ajustar la **configuración de ahorro de batería**. Con el esquema URI de configuración de ahorro de batería en Windows 10, `ms-settings:batterysaver-settings`, puedes proporcionar un vínculo práctico a la aplicación configuración.

**Sugerencia**  al notificar al usuario acerca de la configuración de ahorro de batería, te recomendamos que proporciones una forma de suprimir el mensaje en el futuro. Por ejemplo, la casilla `dontAskMeAgainBox` del siguiente ejemplo guarda la preferencia del usuario en [**LocalSettings**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData.LocalSettings).

 

Este es un ejemplo de cómo comprobar si el ahorro de batería está activado en Windows 10. En este ejemplo se notifica al usuario y se inicia la aplicación Configuración para la **configuración de ahorro de batería**. La casilla `dontAskAgainSetting` permite al usuario suprimir el mensaje si no desea volver a recibir notificaciones.

```cs
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;
using Windows.System;
using Windows.System.Power;
...
...
async public void CheckForEnergySaving()
{
   //Get reminder preference from LocalSettings
   bool dontAskAgain;
   var localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
   object dontAskSetting = localSettings.Values["dontAskAgainSetting"];
   if (dontAskSetting == null)
   {  // Setting does not exist
      dontAskAgain = false;
   }
   else
   {  // Retrieve setting value
      dontAskAgain = Convert.ToBoolean(dontAskSetting);
   }
   
   // Check if battery saver is on and that it's okay to raise dialog
   if ((PowerManager.EnergySaverStatus == EnergySaverStatus.On)
         && (dontAskAgain == false))
   {
      // Check dialog results
      ContentDialogResult dialogResult = await saveEnergyDialog.ShowAsync();
      if (dialogResult == ContentDialogResult.Primary)
      {
         // Launch battery saver settings (settings are available only when a battery is present)
         await Launcher.LaunchUriAsync(new Uri("ms-settings:batterysaver-settings"));
      }

      // Save reminder preference
      if (dontAskAgainBox.IsChecked == true)
      {  // Don't raise dialog again
         localSettings.Values["dontAskAgainSetting"] = "true";
      }
   }
}
```

Este es el código XAML para la clase [**ContentDialog**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) presentada en este ejemplo.

```xaml
<ContentDialog x:Name="saveEnergyDialog"
               PrimaryButtonText="Open battery saver settings"
               SecondaryButtonText="Ignore"
               Title="Battery saver is on."> 
   <StackPanel>
      <TextBlock TextWrapping="WrapWholeWords">
         <LineBreak/><Run>Battery saver is on and you may 
          not receive push notifications.</Run><LineBreak/>
         <LineBreak/><Run>You can choose to allow this app to work normally
         while in battery saver, including receiving push notifications.</Run>
         <LineBreak/>
      </TextBlock>
      <CheckBox x:Name="dontAskAgainBox" Content="OK, got it."/>
   </StackPanel>
</ContentDialog>
```

## <a name="related-topics"></a>Temas relacionados


* [Enviar una notificación de icono local](sending-a-local-tile-notification.md)
* [Inicio rápido: Envío de una notificación de inserción](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [Cómo actualizar un distintivo mediante notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [Cómo solicitar, crear y guardar un canal de notificación](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [Cómo interceptar notificaciones para aplicaciones en ejecución](https://msdn.microsoft.com/library/windows/apps/xaml/jj709907.aspx)
* [Cómo autenticar con los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [Solicitud de servicio de notificaciones de inserción y encabezados de respuesta](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [Directrices y lista de comprobación de notificaciones de inserción](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [Notificaciones sin procesar](https://msdn.microsoft.com/library/windows/apps/hh761488)
 

 




