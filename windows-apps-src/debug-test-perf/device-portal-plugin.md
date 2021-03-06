---
ms.assetid: 82ab5fc9-3a7f-4d9e-9882-077ccfdd0ec9
title: Escribir un complemento personalizado para el Portal de dispositivos
description: Obtén información sobre cómo escribir una aplicación para UWP que usa el Portal de dispositivos Windows para hospedar una página web y proporcionar información de diagnóstico.
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: f66650291e2966d6a3a6ac2b5d794006382d2fbf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170029"
---
# <a name="write-a-custom-plugin-for-device-portal"></a>Escribir un complemento personalizado para el Portal de dispositivos

Obtén información sobre cómo escribir una aplicación para UWP que usa el Portal de dispositivos Windows para hospedar una página web y proporcionar información de diagnóstico.

A partir de Windows 10 Creators Update, (versión 1703, compilación 15063), puede usar el Portal de dispositivos para hospedar las interfaces de diagnóstico de la aplicación. En este artículo se describen las tres partes necesarias para crear un objeto DevicePortalProvider para la aplicación: los cambios del [manifiesto de paquete de la aplicación](/uwp/schemas/appxpackage/appx-package-manifest), la configuración de la conexión de la aplicación al [servicio de Portal de dispositivos](./device-portal.md) y el control de una solicitud entrante.

## <a name="create-a-new-uwp-app-project"></a>Creación de un proyecto de aplicación para UWP

En Microsoft Visual Studio, cree un proyecto de aplicación para UWP. Vaya a **Archivo > Nuevo > Proyecto** y seleccione **Aplicación vacía (Windows universal) para C#** y, a continuación, haga clic en **Siguiente**. En el cuadro de diálogo **Configurar el nuevo proyecto**. Asigne al proyecto el nombre "DevicePortalProvider" y, a continuación, haga clic en **Crear**. Esta será la aplicación que contiene el servicio de aplicaciones. Es posible que deba actualizar Visual Studio o instalar la versión más reciente de [Windows SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/).

## <a name="add-the-deviceportalprovider-extension-to-your-application-package-manifest"></a>Adición de la extensión devicePortalProvider al manifiesto de paquete de aplicación

Tendrás que agregar algo de código a tu archivo *package.appxmanifest* para que tu aplicación sea funcional como complemento del Portal de dispositivos. En primer lugar, agrega las siguientes definiciones de espacio de nombres a la parte superior del archivo. Agrégalas también al atributo `IgnorableNamespaces`.

```xml
<Package
    ... 
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
    IgnorableNamespaces="uap mp rescap uap4">
    ...
```

Para declarar que tu aplicación es un proveedor del Portal de dispositivos, tienes que crear un servicio de aplicaciones y una nueva extensión de proveedor del Portal de dispositivos que lo usa. Agrega tanto la extensión windows.appService como la extensión windows.devicePortalProvider en el elemento `Extensions` bajo `Application`. Asegúrate de que los atributos `AppServiceName` coinciden en cada extensión. Esto indica al servicio Portal de dispositivos que este servicio de aplicaciones se puede iniciar para controlar las solicitudes en el espacio de nombres de controlador. 

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

El atributo `HandlerRoute` hace referencia al espacio de nombres REST reclamado por la aplicación. Las solicitudes HTTP en ese espacio de nombres (implícitamente seguidas de un carácter comodín) que reciba el servicio Portal de dispositivos se enviarán a la aplicación para su control. En este caso, las solicitudes HTTP a `<ip_address>/MyNamespace/api/*` autenticadas correctamente se enviarán a la aplicación. Los conflictos entre las rutas de controlador se resuelven a través de una comprobación de tipo "el más largo gana": se selecciona la ruta que coincida con el mayor número de solicitudes, lo que significa que una solicitud a "/MyNamespace/api/foo" coincidirá con un proveedor con "/MyNamespace/api" en lugar de uno con "/MyNamespace".  

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
> La funcionalidad "devicePortalProvider" está restringida ("rescap"), lo que significa que tienes que obtener autorización previa de Microsoft Store antes de que tu aplicación se pueda publicar allí. Sin embargo, esto no impide probar la aplicación de manera local mediante la instalación de prueba. Para más información sobre las funcionalidades restringidas, consulta [Declaraciones de funcionalidades de las aplicaciones](../packaging/app-capability-declarations.md).

## <a name="set-up-your-background-task-and-winrt-component"></a>Configurar la tarea en segundo plano y el componente WinRT
Para configurar la conexión al Portal de dispositivos, la aplicación debe enlazar una conexión de servicio de la aplicación desde el servicio Portal de dispositivos con la instancia de Portal de dispositivos en ejecución dentro de la aplicación. Para ello, agrega un nuevo componente WinRT a tu aplicación con una clase que implemente [**IBackgroundTask**](/uwp/api/windows.applicationmodel.background.ibackgroundtask).

```csharp
namespace MySampleProvider {
    // Implementing a DevicePortalConnection in a background task
    public sealed class SampleProvider : IBackgroundTask {
        //...
    }
```

Asegúrate de que su nombre coincide con el espacio de nombres y el nombre de clase establecidos por AppService EntryPoint ("MySampleProvider.SampleProvider"). Cuando realizas la primera solicitud al proveedor del Portal de dispositivos, el Portal de dispositivos almacenará la solicitud, iniciará la tarea en segundo plano de la aplicación, llamará a su método **Run** y pasará un elemento [**IBackgroundTaskInstance**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance). Luego, la aplicación lo usa para configurar una instancia de [**DevicePortalConnection**](/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnection).

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

La aplicación debe administrar dos eventos para completar el bucle de control de solicitudes: **Cerrado**, para cada vez que el servicio Portal de dispositivos se apaga, y [**RequestReceived**](/uwp/api/windows.system.diagnostics.deviceportal.deviceportalconnectionrequestreceivedeventargs), que expone las solicitudes HTTP de entrada y proporciona la funcionalidad principal del proveedor del Portal de dispositivos. 

## <a name="handle-the-requestreceived-event"></a>Control del evento RequestReceived
El evento **RequestReceived** se lanzará una vez para cada solicitud HTTP que se realice en la ruta de controlador especificado del complemento. El bucle de control de la solicitud para los proveedores del Portal de dispositivos es similar al de NodeJS Express: los objetos de solicitud y respuesta se proporcionan juntos con el evento, y el controlador responde rellenando el objeto de respuesta. En los proveedores del Portal de dispositivos, el evento **RequestReceived** y sus controladores usan objetos [**Windows.Web.Http.HttpRequestMessage**](/uwp/api/windows.web.http.httprequestmessage) y [**HttpResponseMessage**](/uwp/api/windows.web.http.httpresponsemessage).   

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

En este controlador de solicitud de ejemplo, primero extraemos los objetos de solicitud y respuesta del parámetro *args* y luego creamos una cadena con la dirección URL de la solicitud y algún código adicional en formato HTML. Esto se agrega en el objeto de respuesta como una instancia [**HttpStringContent**](/uwp/api/windows.web.http.httpstringcontent). También se permiten otras clases [**IHttpContent**](/uwp/api/windows.web.http.ihttpcontent), como las de "String" y "Buffer".

La respuesta luego se establece como una respuesta HTTP y se le proporciona un código de estado 200 - Correcto. Debería representarse según lo previsto en el explorador que hizo la llamada original. Ten en cuenta que cuando el controlador de eventos **RequestReceived** realiza la devolución, el mensaje de respuesta se devuelve automáticamente al agente de usuario: no se necesita ningún método "send" adicional.

![Mensaje de respuesta del Portal de dispositivos](images/device-portal/plugin-response-message.png)

## <a name="providing-static-content"></a>Proporcionar contenido estático
El contenido estático se puede entregar directamente desde una carpeta dentro del paquete, por lo que se facilita mucho la adición de una interfaz de usuario al proveedor.  La forma más sencilla de entregar contenido estático es crear una carpeta de contenido en el proyecto que pueda asignarse a una dirección URL.

![Carpeta de contenido estático del Portal de dispositivos](images/device-portal/plugin-static-content.png)
 
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
Asegúrate de que todos los archivos de la carpeta de contenido se marquen como "Contenido" y se establezcan en "Copiar si es posterior" o "Copiar siempre" en el menú Propiedades de Visual Studio.  De este modo, se garantiza que los archivos estarán dentro del paquete AppX cuando se implemente.  

![Configurar copias de archivos de contenido estático](images/device-portal/plugin-file-copying.png)

## <a name="using-existing-device-portal-resources-and-apis"></a>Usar los recursos y las API existentes del Portal de dispositivos
El contenido estático que entregue un proveedor del Portal de dispositivos se entrega en el mismo puerto que el servicio Portal de dispositivos principal.  Esto significa que puedes hacer referencia al código JS y CSS existente que se incluye en el Portal de dispositivos mediante sencillas etiquetas `<link>` y `<script>` en tu código HTML. En general, recomendamos el uso de *rest.js*, que encapsula todas las API de REST principales del Portal de dispositivos en un cómodo objeto webbRest, y el archivo *common.css*, que te permitirá aplicar estilo a tu contenido para que se adapte al resto de la interfaz de usuario del Portal de dispositivos. Puedes ver un ejemplo de esto en la página *index.html* que se incluye en el ejemplo. Usa *rest.js* para recuperar el nombre del dispositivo y ejecutar procesos desde el Portal de dispositivos. 

![Salida de complemento del Portal de dispositivos](images/device-portal/plugin-output.png)
 
Lo que es más importante, el uso de los métodos HttpPost/DeleteExpect200 en webbRest se encargará automáticamente del [control CSRF](/aspnet/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks), lo que permite a tu página web llamar a las API de REST de cambio de estado.  

> [!NOTE] 
> El contenido estático que se incluye en el Portal de dispositivos no proporciona una garantía contra cambios importantes. Si bien no se espera que las API cambiarán con frecuencia, podrían hacerlo, especialmente en los archivos *common.js* y *controls.js*, que el proveedor no debe usar. 

## <a name="debugging-the-device-portal-connection"></a>Depurar la conexión al Portal de dispositivos
Para depurar la tarea en segundo plano, tienes que cambiar la forma en que Visual Studio ejecuta el código. Aplica los pasos siguientes para depurar una conexión de servicio de la aplicación y comprobar cómo el proveedor controla las solicitudes HTTP:

1.  En el menú Depurar, selecciona Propiedades de DevicePortalProvider. 
2.  En la pestaña Depurar, en la sección Iniciar acción, selecciona "No iniciar, pero depurar mi código al empezar".  
![Colocar el complemento en modo de depuración](images/device-portal/plugin-debug-mode.png)
3.  Establece un punto de interrupción en la función de controlador RequestReceived.
![Punto de interrupción en el controlador RequestReceived](images/device-portal/plugin-requestreceived-breakpoint.png)
> [!NOTE] 
> Asegúrate de que la arquitectura de la compilación coincida exactamente con la arquitectura de destino. Si estás usando un PC de 64 bits, debes realizar la implementación usando una compilación de AMD64. 
4.  Presiona F5 para implementar la aplicación
5.  Desactiva el Portal de dispositivos y vuelve a activarlo para que encuentre la aplicación (solo es necesario hacer esto cuando cambies el manifiesto de la aplicación; el resto de las veces, puedes volver a implementarlo y omitir este paso). 
6.  En el explorador, accede al espacio de nombres del proveedor, y debería activarse el punto de interrupción.

## <a name="related-topics"></a>Temas relacionados
* [Introducción al Portal de dispositivos Windows](device-portal.md)
* [Crear y usar un servicio de aplicación](../launch-resume/how-to-create-and-consume-an-app-service.md)