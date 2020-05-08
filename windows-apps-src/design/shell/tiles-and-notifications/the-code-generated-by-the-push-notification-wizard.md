---
Description: Usar un asistente en Visual Studio te permite generar notificaciones de inserción desde un servicio móvil creado con los Servicios móviles de Azure.
title: Código generado por el Asistente para notificaciones de inserción
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9651af0c9caaae58bad82b2e33c1b0621b205054
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970460"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>Código generado por el Asistente para notificaciones de inserción
 

Usar un asistente en Visual Studio te permite generar notificaciones de inserción desde un servicio móvil creado con los Servicios móviles de Azure. El Asistente de Visual Studio genera código para ayudarte a empezar. En este tema se explica cómo el asistente modifica el proyecto, qué hace el código generado, cómo se usa este código y qué puedes hacer después para sacarle todo el partido a las notificaciones de inserción. Consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md).

## <a name="how-the-wizard-modifies-your-project"></a>Cómo el asistente modifica tu proyecto


El Asistente para notificaciones de inserción modifica tu proyecto de estos modos:

-   Agrega una referencia al cliente administrado de los Servicios móviles (MobileServicesManagedClient.dll). No se aplica a proyectos de JavaScript.
-   Agrega un archivo a una subcarpeta de los servicios, y asigna un nombre al archivo push.register.cs, push.register.vb, push.register.cpp o push.register.js.
-   Crea una tabla del canal en el servidor de bases de datos para el servicio móvil. La tabla contiene información necesaria para enviar notificaciones de inserción a instancias de la aplicación.
-   Crea scripts para cuatro funciones: eliminar, insertar, leer y actualizar.
-   Crea un script con una API personalizada, notifyallusers.js, que envía una notificación de inserción a todos los clientes.
-   Agrega una declaración a tu archivo App.xaml.cs, App.xaml.vb o App.xaml.cpp, o agrega una declaración a un nuevo archivo, service.js, para proyectos JavaScript. La declaración declara un objeto MobileServiceClient que contiene la información necesaria para conectarse al servicio móvil. Puedes acceder a este objeto MobileServiceClient, denominado *MyServiceName*Client, desde cualquier página de la aplicación mediante el nombre App.*MyServiceName*Client.

El archivo services.js contiene el siguiente código:

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>Registro para notificaciones de inserción


En el registro de inserciones. \*, el método UploadChannel registra el dispositivo para recibir notificaciones de envío. La Tienda realiza un seguimiento de las copias de tu aplicación que hay instaladas y ofrece el canal de notificaciones de inserción. Consulta [**PushNotificationChannelManager**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager).

El código de cliente es similar para tanto para el backend de JavaScript como para el backend de .NET. De manera predeterminada, al agregar notificaciones de inserción para un servicio backend de JavaScript, se inserta una llamada de ejemplo a la API personalizada notifyAllUsers en el método UploadChannel.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MobileServices;
using Newtonsoft.Json.Linq;

namespace App2
{
    internal class mymobileservice1234Push
    {
        public async static void UploadChannel()
        {
            var channel = await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            try
            {
                await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri);
                await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers");
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

        private static void HandleRegisterException(Exception exception)
        {
            
        }
    }
}
```

```vb
Imports Microsoft.WindowsAzure.MobileServices
Imports Newtonsoft.Json.Linq

Friend Class mymobileservice1234Push
    Public Shared Async Sub UploadChannel()
        Dim channel = Await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync()

        Try
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri)
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri, New String() {"tag1", "tag2"})
            Await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers")
        Catch exception As Exception
            HandleRegisterException(exception)
        End Try
    End Sub

    Private Shared Sub HandleRegisterException(exception As Exception)

    End Sub
End Class
```

```c++
#include "pch.h"
#include "services\mobile services\mymobileservice1234\mymobileservice1234Push.h"

using namespace AzureMobileHelper;

using namespace web;
using namespace concurrency;

using namespace Windows::Networking::PushNotifications;

void mymobileservice1234Push::UploadChannel()
{
    create_task(PushNotificationChannelManager::CreatePushNotificationChannelForApplicationAsync()).
    then([] (PushNotificationChannel^ newChannel) 
    {
        return mymobileservice1234MobileService::GetClient().get_push().register_native(newChannel->Uri->Data());
    }).then([]()
    {
        return mymobileservice1234MobileService::GetClient().invoke_api(L"notifyAllUsers");
    }).then([](task<json::value> result)
    {
        try
        {
            result.wait();
        }
        catch(...)
        {
            HandleExceptionsComingFromTheServer();
        }
    });
}

void mymobileservice1234Push::HandleExceptionsComingFromTheServer()
{
}
```

```js
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.addEventListener("activated", function (args) {
        if (args.detail.kind == activation.ActivationKind.launch) {
            Windows.Networking.PushNotifications.PushNotificationChannelManager.createPushNotificationChannelForApplicationAsync()
                .then(function (channel) {
                    mymobileserviceclient1234Client.push.registerNative(channel.Uri, new Array("tag1", "tag2"))
                    return mymobileservice1234Client.push.registerNative(channel.uri);
                })
                .done(function (registration) {
                    return mymobileservice1234Client.invokeApi("notifyAllUsers");
                }, function (error) {
                    // Error

                });
        }
    });
})();
```

Las etiquetas de notificación de inserción proporcionan una forma de restringir las notificaciones para un subconjunto de clientes. Puedes usar el método registerNative (o RegisterNativeAsync) para registrarte para recibir todas las notificaciones de inserción sin especificar etiquetas, o bien puedes registrarte con etiquetas proporcionando el segundo argumento, una matriz de etiquetas. Si te registras con una o más etiquetas, solo recibirás notificaciones que coinciden con esas etiquetas.

## <a name="server-side-scripts-javascript-backend-only"></a>Scripts del lado servidor (solo backend de JavaScript)


Para los servicios móviles que usan el backend de JavaScript, los scripts del lado servidor se ejecutan cuando se producen operaciones de eliminación, inserción, lectura o actualización. Los scripts no implementan estas operaciones, sino que se ejecutan cuando el cliente realiza una llamada a la API REST de los Servicios móviles de Windows que desencadena estos eventos. Después, los scripts pasan el control a las propias operaciones mediante una llamada a request.execute o request.respond para emitir una respuesta al contexto de llamada. Consulta [Referencia de la API de REST de los Servicios móviles de Azure](https://msdn.microsoft.com/library/azure/jj710108.aspx).

En el script del lado servidor hay disponible toda una variedad de funciones. Consulta [Registrar operaciones de tabla en Servicios móviles de Azure](https://msdn.microsoft.com/library/azure/dn167708.aspx). Puedes consultar una referencia de todas las funciones disponibles en [Referencia de script de servidor de Servicios móviles](https://msdn.microsoft.com/library/windowsazure/jj554226).

También se crea el siguiente código de API personalizado en Notifyallusers.js:

```js
exports.post = function(request, response) {
    response.send(statusCodes.OK,{ message : 'Hello World!' })
    
    // The following call is for illustration purpose only
    // The call and function body should be moved to a script in your app
    // where you want to send a notification
    sendNotifications(request);
};

// The following code should be moved to appropriate script in your app where notification is sent
function sendNotifications(request) {
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual><binding template="ToastText01">' +
        '<text id="1">Sample Toast</text></binding></visual></toast>';
    var push = request.service.push; 
    push.wns.send(null,
        payload,
        'wns/toast', {
            success: function (pushResponse) {
                console.log("Sent push:", pushResponse);
            }
        });
}
```

La función sendNotifications envía una sola notificación en forma de notificación del sistema. También puedes usar otros tipos de notificaciones de inserción.

**Sugerencia**  para obtener información sobre cómo obtener ayuda mientras se editan scripts, vea [Habilitar IntelliSense para JavaScript del lado servidor](https://blogs.msdn.com/b/visualstudio/archive/2013/07/26/enabling-intellisense-for-mobile-services-javascript-in-visual-studio.aspx).

 

## <a name="push-notification-types"></a>Tipos de notificaciones de inserción


Windows admite notificaciones de otro tipo además de notificaciones de inserción. Para obtener información general sobre las notificaciones, consulte [elección de un método de entrega de notificaciones](choosing-a-notification-delivery-method.md).

Las notificaciones del sistema son fáciles de usar; puedes ver un ejemplo en el código Insert.js, en la tabla del canal que se ha generado automáticamente. Si tienes previsto usar notificaciones de icono o de rótulo informativo, deberás crear una plantilla XML para el icono y el rótulo informativo, y deberás especificar la codificación de la información empaquetada en la plantilla. Vea [trabajar con iconos, distintivos y notificaciones del sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

Dado que Windows responde a las notificaciones del sistema, puede administrar la mayoría de estas notificaciones cuando la aplicación no se está ejecutando. Por ejemplo, una notificación de inserción permitiría al usuario saber cuándo ha recibido un nuevo mensaje de correo aunque la aplicación de correo local no se esté ejecutando. Windows administra las notificaciones del sistema mostrando un mensaje, como la primera línea de un mensaje de texto. Windows administra una notificación de icono o de rótulo informativo actualizando el icono dinámico de una aplicación para reflejar el número de mensajes de correo nuevos. De este modo, puedes solicitar a los usuarios de tu aplicación que la comprueben en busca de nueva información. Tu aplicación puede recibir notificaciones sin procesar mientras se está ejecutando y puedes usarlas para enviar datos a la aplicación. Si tu aplicación no se está ejecutando, puedes configurar una tarea en segundo plano para supervisar las notificaciones de inserción.

Debe usar las notificaciones de envío de acuerdo con las instrucciones de las aplicaciones de aplicaciones de Windows, ya que estas notificaciones usan los recursos de un usuario y pueden distraerse si se sobreutilizan. Consulta [Directrices y lista de comprobación de notificaciones de inserción](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview).

Si vas a actualizar iconos dinámicos con notificaciones de inserción, también deberías seguir las directrices de [Directrices y lista de comprobación para iconos y notificaciones](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles).

## <a name="next-steps"></a>Pasos siguientes


### <a name="using-the-windows-push-notification-services-wns"></a>Uso de los Servicios de notificaciones de inserción de Windows (WNS)

Puedes llamar a Servicios de notificaciones de inserción de Windows (WNS) directamente si los Servicios móviles no ofrecen la suficiente flexibilidad, si quieres escribir tu código de servidor en C# o Visual Basic, o si ya tienes un servicio en la nube y quieres usarlo para enviar notificaciones de inserción desde él. Al llamar directamente a WNS, puedes enviar notificaciones de inserción desde tu propio servicio en la nube, como un rol de trabajador que supervisa los datos desde una base de datos u otro servicio web. El servicio en la nube debe autenticarse en WNS para enviar notificaciones de inserción a tus aplicaciones. Consulta [Cómo autenticar con los Servicios de notificaciones de inserción de Windows (JavaScript)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10)) o [(C#/C++/VB)](https://docs.microsoft.com/previous-versions/windows/apps/hh868206(v=win.10)).

También puedes enviar notificaciones de inserción si ejecutas una tarea programada en tu servicio móvil. Consulta [Programación de trabajos periódicos en Servicios móviles](https://azure.microsoft.com/documentation/articles/mobile-services-schedule-recurring-tasks/).

**ADVERTENCIA**  una vez que haya ejecutado el Asistente para notificaciones de inserciones una vez, no ejecute el Asistente una segunda vez para agregar el código de registro de otro servicio móvil. Si ejecutas el asistente más de una vez por cada proyecto, se generará código que provocará llamadas superpuestas al método [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync), que da lugar a una excepción en tiempo de ejecución. Si quieres registrar notificaciones de inserción para más de un servicio móvil, ejecuta el asistente una sola vez y después vuelve a escribir el código de registro para asegurarte de que las llamadas a **CreatePushNotificationChannelForApplicationAsync** no se ejecutan a la vez. Por ejemplo, puede lograr esto moviendo el código generado por el asistente en el registro de extracción. \* (incluida la llamada a **CreatePushNotificationChannelForApplicationAsync**) fuera del evento onlaunched, pero las características específicas de esto dependerán de la arquitectura de la aplicación.

 

## <a name="related-topics"></a>Temas relacionados


* [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](windows-push-notification-services--wns--overview.md)
* [Introducción a las notificaciones sin procesar](raw-notification-overview.md)
* [Conexión con los Servicios móviles de Microsoft Azure (JavaScript)](https://docs.microsoft.com/previous-versions/windows/apps/dn263160(v=win.10))
* [Conexión con los Servicios móviles de Microsoft Azure (C#/C++/VB)](https://docs.microsoft.com/previous-versions/windows/apps/dn263175(v=win.10))
* [Inicio rápido: agregar notificaciones de inserción para un servicio móvil (JavaScript)](https://docs.microsoft.com/previous-versions/windows/apps/dn263163(v=win.10))
 

 




