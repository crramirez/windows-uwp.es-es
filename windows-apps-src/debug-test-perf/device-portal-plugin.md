---
author: PatrickFarley
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Escribir un complemento personalizado para Device Portal
description: Obtén información sobre cómo escribir una aplicación para UWP que usa Windows Device Portal para hospedar una página web y proporcionar información de diagnóstico.
ms.author: pafarley
ms.date: 03/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 6bf511a910a266877d54a014f087c1748ff25838
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3929647"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Escribir un complemento personalizado para Device Portal

Obtén información sobre cómo escribir una aplicación para UWP que usa Windows Device Portal para hospedar una página web y proporcionar información de diagnóstico.

A partir de Creators Update, puedes usar Device Portal para hospedar las interfaces de diagnóstico de tu aplicación. En este artículo se describen las tres partes necesarias para crear un objeto DevicePortalProvider para tu aplicación: los cambios de appxmanifest, la configuración de conexión de la aplicación al servicio Device Portal y controlar una solicitud entrante. Para comenzar, también se proporciona una aplicación de ejemplo (disponible próximamente). 

## <a name="create-a-new-uwp-app-project"></a>Creación de un nuevo proyecto de aplicación para UWP
En esta guía, crearemos todo en una solución para hacerlo más sencillo.

En Microsoft Visual Studio 2017, crea un nuevo proyecto de aplicación para UWP. Ve a Archivo > Nuevo proyecto y selecciona Plantillas > Visual C# > Universal de Windows > Aplicación vacía (Universal de Windows). Ponle el nombre "DevicePortalProvider". Esta será la aplicación que contiene el servicio de aplicaciones. Asegúrate de elegir el SDK de Creators Update que se debe admitir.  Es posible que tengas que actualizar Visual Studio o instalar el nuevo SDK. Consulta [aquí](https://blogs.windows.com/buildingapps/2017/04/05/updating-tooling-windows-10-creators-update/) para más información. 

## <a name="add-the-deviceportalprovider-extension-to-your-packageappxmanifest-file"></a>Agregar la extensión devicePortalProvider al archivo package.appxmanifest
Tendrás que agregar algo de código a tu archivo *package.appxmanifest* para que tu aplicación sea funcional como complemento de Device Portal. En primer lugar, agrega las siguientes definiciones de espacio de nombres a la parte superior del archivo. Agrégalas también al atributo `IgnorableNamespaces`.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

Para declarar que tu aplicación es un proveedor de Device Portal, tienes que crear un servicio de aplicaciones y una nueva extensión de proveedor Device Portal que lo usa. Agrega tanto la extensión windows.appService como la extensión windows.devicePortalProvider en el elemento `Extensions` bajo `Application`. Asegúrate de que los atributos `AppServiceName` coinciden en cada extensión. Esto indica al servicio Device Portal que este servicio de aplicaciones se puede iniciar para controlar las solicitudes en el espacio de nombres de controlador. 

```xml
...   
<Application 
    Id="App" 
    Executable="$targetnametoken$.exe"
    EntryPoint="DevicePortalProvider.App">
    ...
    <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MySampleProvider.SampleProvider">
            <uap:AppService Name="com.sampleProvider.wdp" />
        </uap:Extension>
        <uap4:Extension Category="windows.devicePortalProvider">
            <uap4:DevicePortalProvider 
                DisplayName="My Device Portal Provider Sample App" 
                AppServiceName="com.sampleProvider.wdp" 
                HandlerRoute="/MyNamespace/api/" />
        </uap4:Extension>
    </Extensions>
</Application>
...
```

El atributo `HandlerRoute` hace referencia al espacio de nombres REST reclamado por la aplicación. Las solicitudes HTTP en ese espacio de nombres (implícitamente seguidas de un carácter comodín) que reciba el servicio Device Portal se enviarán a la aplicación para su control. En este caso, las solicitudes HTTP a `<ip_address>/MyNamespace/api/*` autenticadas correctamente se enviarán a la aplicación. Los conflictos entre las rutas de controlador se resuelven a través de una comprobación de tipo "el más largo gana": se selecciona la ruta que coincida con el mayor número de solicitudes, lo que significa que una solicitud a "/MyNamespace/api/foo" coincidirá con un proveedor con "/MyNamespace/api" en lugar de uno con "/MyNamespace".  

Para esta funcionalidad se requieren dos nuevas capacidades. También deben agregarse al archivo *package.appxmanifest*.

```xml
...
<Capabilities>
    ...
    <Capability Name="privateNetworkClientServer" />
    <rescap:Capability Name="devicePortalProvider" />
</Capabilities>
...
```

> [!NOTE]
> La funcionalidad "devicePortalProvider" está restringida ("rescap"), lo que significa que tienes que obtener autorización previa de la Tienda antes de que tu aplicación se pueda publicar allí. Sin embargo, esto no impide probar la aplicación de manera a través de la instalación de prueba. Para más información sobre las funcionalidades restringidas, consulta [Declaraciones de funcionalidades de las aplicaciones](https://msdn.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities).

## <a name="set-up-your-background-task-and-winrt-component"></a>Configurar la tarea en segundo plano y el componente WinRT
Para configurar la conexión a Device Portal, la aplicación debe enlazar una conexión de servicio de la aplicación desde el servicio Device Portal con la instancia de Device Portal en ejecución dentro de la aplicación. Para ello, agrega un nuevo componente WinRT a tu aplicación con una clase que implemente [**IBackgroundTask**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtask).

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

Asegúrate de que su nombre coincide con el espacio de nombres y clases establecido por el AppService EntryPoint ("MySampleProvider.SampleProvider"). Cuando realizas la primera solicitud al proveedor de Device Portal, Device Portal almacenará de la solicitud, iniciará la tarea en segundo plano de la aplicación, llamará a su método **Run** y pasará un elemento [**IBackgroundTaskInstance**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance). Luego, la aplicación lo usa para configurar una instancia de [**DevicePortalConnection**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection).

```csharp
// Implement background task handler with a DevicePortalConnection
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take a deferral to allow the background task to continue executing 
    this.taskDeferral = taskInstance.GetDeferral();
    taskInstance.Canceled += TaskInstance_Canceled;

    // Create a DevicePortal client from an AppServiceConnection 
    var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    var appServiceConnection = details.AppServiceConnection;
    this.devicePortalConnection = DevicePortalConnection.GetForAppServiceConnection(appServiceConnection);

    // Add Closed, RequestReceived handlers 
    devicePortalConnection.Closed += DevicePortalConnection_Closed;
    devicePortalConnection.RequestReceived += DevicePortalConnection_RequestReceived;
}
```

Hay dos eventos que la aplicación debe controlar para completar el bucle de control de la solicitud: **Closed**, para cuando se cierre el servicio Device Portal, y [**RequestRecieved**](https://docs.microsoft.com/en-us/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs), que pone en la superficie las solicitudes HTTP entrantes y proporciona la funcionalidad principal del proveedor de Device Portal. 

## <a name="handle-the-requestreceived-event"></a>Controlar el evento RequestReceived
El evento **RequestReceived** se lanzará una vez para cada HTTP solicitud que se realice en la ruta de controlador especificado del complemento. El bucle de control de la solicitud para los proveedores de Device Portal es similar al de NodeJS Express: los objetos de solicitud y respuesta se proporcionan juntos con el evento y el controlador responde rellenando el objeto de respuesta. En los proveedores de Device Portal, el evento **RequestReceived** y sus controladores usan objetos [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httprequestmessage) y [**HttpResponseMessage**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpresponsemessage).   

```csharp
// Sample RequestReceived echo handler: respond with an HTML page including the query and some additional process information. 
private void DevicePortalConnection_RequestReceived(DevicePortalConnection sender, DevicePortalConnectionRequestReceivedEventArgs args)
{
    var req = args.RequestMessage;
    var res = args.ResponseMessage;

    if (req.RequestUri.AbsolutePath.EndsWith("/echo"))
    {
        // construct an html response message
        string con = "<h1>" + req.RequestUri.AbsoluteUri + "</h1><br/>";
        var proc = Windows.System.Diagnostics.ProcessDiagnosticInfo.GetForCurrentProcess();
        con += String.Format("This process is consuming {0} bytes (Working Set)<br/>", proc.MemoryUsage.GetReport().WorkingSetSizeInBytes);
        con += String.Format("The process PID is {0}<br/>", proc.ProcessId);
        con += String.Format("The executable filename is {0}", proc.ExecutableFileName);
        res.Content = new HttpStringContent(con);
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
        res.StatusCode = HttpStatusCode.Ok;            
    }
    //...
}
```

En este controlador de solicitud de ejemplo, primero extraemos los objetos de solicitud y respuesta del parámetro *args* y luego creamos una cadena con la dirección URL de la solicitud y código adicional en formato HTML. Esto se agrega en el objeto de respuesta como una instancia [**HttpStringContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.httpstringcontent). También se permiten otras clases [**IHttpContent**](https://docs.microsoft.com/en-us/uwp/api/windows.web.http.ihttpcontent), como las de "String" y "Búfer".

La respuesta luego se establece como una respuesta HTTP y se le proporciona un código de estado de 200 (Aceptar). Debería representarse según lo previsto en el explorador que hizo la llamada original. Ten en cuenta que cuando vuelve el controlador de evento **RequestReceived**, el mensaje de respuesta se devuelve automáticamente al agente de usuario: no se necesita ningún método "send" adicional.

![Mensaje de respuesta Device Portal](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>Proporcionar contenido estático
El contenido estático se puede entregar directamente desde una carpeta dentro del paquete, por lo que se facilita mucho la adición de una interfaz de usuario al proveedor.  La forma más sencilla de entregar contenido estático es crear una carpeta de contenido en el proyecto que pueda asignarse a una dirección URL.

![Carpeta de contenido estático de Device Portal](images/device-portal/plugin-static-content.png)
 
Luego, agrega un controlador de ruta en el controlador de eventos **RequestReceived** que detecta rutas de contenido estático y asigna una solicitud según corresponda.  

```csharp
if (req.RequestUri.LocalPath.ToLower().Contains("/www/")) {
    var filePath = req.RequestUri.AbsolutePath.Replace('/', '\\').ToLower();
    filePath = filePath.Replace("\\backgroundprovider", "")
    try {
        var fileStream = Windows.ApplicationModel.Package.Current.InstalledLocation.OpenStreamForReadAsync(filePath).GetAwaiter().GetResult();
        res.StatusCode = HttpStatusCode.Ok;
        res.Content = new HttpStreamContent(fileStream.AsInputStream());
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    } catch(FileNotFoundException e) {
        string con = String.Format("<h1>{0} - not found</h1>\r\n", filePath);
        con += "Exception: " + e.ToString();
        res.Content = new HttpStringContent(con);
        res.StatusCode = HttpStatusCode.NotFound;
        res.Content.Headers.ContentType = new HttpMediaTypeHeaderValue("text/html");
    }
}
```
Asegúrate de que todos los archivos de la carpeta de contenido se marquen como "Contenido" y se establezcan en "Copiar si es posterior" o "Copiar siempre" en el menú Propiedades de Visual Studio.  De este modo, se garantiza que los archivos estarán dentro del paquete AppX cuando se implemente.  

![Configurar copias de archivos de contenido estático](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>Usar los recursos y las API existentes de Device Portal
El contenido estático que entregue un proveedor de Device Portal se entrega en el mismo puerto que el servicio Device Portal principal.  Esto significa que puedes hacer referencia al código JS y CSS existente que se incluye en Device Portal mediante sencillas etiquetas `<link>` y `<script>` en tu código HTML. En general, recomendamos el uso de *rest.js*, que encapsula todas las API de REST principales de Device Portal en un cómodo objeto webbRest y el archivo *common.css*, lo que te permitirá aplicar estilo a tu contenido para que se adapte al resto de la interfaz de usuario de Device Portal. Puedes ver un ejemplo de esto en la página *index.html* que se incluye en el ejemplo. Usa *rest.js* para recuperar el nombre del dispositivo y ejecutar procesos desde Device Portal. 

![Salida de complemento de Device Portal](images/device-portal/plugin-output.png)
 
Lo que es más importante, el uso de los métodos HttpPost/DeleteExpect200 en webbRest se encargará automáticamente del [control CSRF](https://docs.microsoft.com/en-us/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks), lo que permite a tu página web llamar a las API de REST de cambio de estado.  

> [!NOTE] 
> El contenido estático que se incluye en Device Portal no proporciona una garantía contra cambios importantes. Si bien no se espera que las API cambiarán con frecuencia, podrían hacerlo, especialmente en los archivos *common.js* y *controls.js*, que el proveedor no debe usar. 

## <a name="debugging-the-device-portal-connection"></a>Depurar la conexión a Device Portal
Para depurar la tarea en segundo plano, tienes que cambiar la forma en que Visual Studio ejecuta el código. Sigue los pasos siguientes para depurar una conexión de servicio de la aplicación y comprobar cómo el proveedor controla de las solicitudes HTTP:

1.  En el menú Depurar, selecciona Propiedades de DevicePortalProvider. 
2.  En la pestaña Depurar, en la sección Acción de inicio, selecciona No iniciar, pero depurar mi código al empezar.  
![Colocar el complemento en modo de depuración](images/device-portal/plugin-debug-mode.png)
3.  Establece un punto de interrupción en la función de controlador RequestReceived.
![Punto de interrupción en el controlador RequestReceived](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> Asegúrate de que la arquitectura de la compilación coincida exactamente con la arquitectura de destino. Si estás usando un PC de 64 bits, debes realizar la implementación usando una compilación de AMD64. 
4.  Presionar F5 para implementar la aplicación
5.  Desactiva Device Portal y vuelve a activarlo para que encuentre la aplicación (solo es necesario hacer esto cuando cambies el manifiesto de la aplicación; el resto de las veces, puede volver a implementarla y omitir este paso). 
6.  En el explorador, accede al espacio de nombres del proveedor y debería activarse el punto de interrupción.

## <a name="related-topics"></a>Temas relacionados
* [Introducción a Windows Device Portal](device-portal.md)
* [Crear y usar un servicio de aplicación](https://docs.microsoft.com/en-us/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)


