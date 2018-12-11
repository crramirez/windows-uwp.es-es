---
Description: A web view control embeds a view into your app that renders web content using the Microsoft Edge rendering engine. Hyperlinks can also appear and function in a web view control.
title: Vista web
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2a29f58ff8dc842fd985a44f94ff44baea51dc2e
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885710"
---
# <a name="web-view"></a>Vista web
 

Un control de vista web inserta una vista en la aplicación que representa al contenido web, mediante el motor de representación de Microsoft Edge. En un control de vista web también pueden aparecer y funcionar hipervínculos.

> **API importantes**: [Clase WebView](https://msdn.microsoft.com/library/windows/apps/br227702)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control de vista web para mostrar contenido HTML de formato enriquecido desde un servidor web remoto, código generado dinámicamente o archivos de contenido de tu paquete de la aplicación. El contenido enriquecido también puede contener código de script y comunicarse entre el script y el código de tu aplicación.

## <a name="create-a-web-view"></a>Crear una vista web

**Modificar la apariencia de una vista web**

[WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) no es una subclase [Control](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx), así que no tiene una plantilla de control. Sin embargo, puedes establecer varias propiedades para controlar algunos aspectos visuales de la vista web.
- Para restringir el área de visualización, establece las propiedades [Width](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) y [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx). 
- Para traducir, escalar, sesgar y girar una vista web, usa la propiedad [RenderTransform](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransform.aspx).
- Para controlar la opacidad de la vista web, establece la propiedad [Opacity](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx).
- Para especificar un color que se usa como fondo de la página web cuando el contenido HTML no especifica uno, establece la propiedad [DefaultBackgroundColor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultbackgroundcolor.aspx). 

**Obtener el título de la página web**

Puedes obtener el título del documento HTML que se muestra actualmente en la vista web mediante la propiedad [DocumentTitle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.documenttitle.aspx). 

**Eventos de entrada y orden de tabulación**

Aunque WebView no es una subclase de Control, recibe el foco de entrada de teclado y participa en la secuencia de tabulación. Proporciona un método [Focus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.focus.aspx) y eventos [GotFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.gotfocus.aspx) y [LostFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.lostfocus.aspx), pero carece de propiedades relacionadas con la tabulación. Su posición en la secuencia de tabulación es igual a su posición en el orden de documentos XAML. La secuencia de tabulación incluye todos los elementos de la vista web que pueden recibir el foco de entrada. 

Como se indica en la tabla Events de la página de la clase [WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx), la vista web no admite la mayoría de los eventos de entrada de usuario heredados de [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), como [KeyDown](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keydown.aspx), [KeyUp](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keyup.aspx) y [PointerPressed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx). En vez de eso, puedes usar [InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) con la función JavaScript **eval** para usar los controladores de eventos HTML y usar **window.external.notify** desde el controlador de eventos HTML para notificar a la aplicación mediante [WebView.ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx).

### <a name="navigating-to-content"></a>Navegar hasta el contenido

La vista web proporciona varias API para navegación básica: [GoBack](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goback.aspx), [GoForward](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goforward.aspx), [Stop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.stop.aspx), [Refresh](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.refresh.aspx), [CanGoBack](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoback.aspx) y [CanGoForward](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoforward.aspx). Puedes usarlos para agregar a tu aplicación la funcionalidad típica de exploración web. 

Para establecer el contenido inicial de la vista web, debes establecer la propiedad [Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.source.aspx) en XAML. El analizador XAML convierte automáticamente la cadena en un [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.uri.aspx). 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

Se puede establecer la propiedad Source en código, pero es más habitual usar uno de los métodos **Navigate** para cargar contenido en el código. 

Para cargar contenido web, usa el método [Navigate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigate.aspx) con un **Uri** que usa el esquema http o https. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

Para navegar a un URI con una solicitud POST y encabezados HTTP, usa el método [NavigateWithHttpRequestMessage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage.aspx). Este método solo admite [HttpMethod.Post](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.post.aspx) y [HttpMethod.Get](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.get.aspx) como valores de la propiedad [HttpRequestMessage.Method](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httprequestmessage.method.aspx). 

Para cargar contenido sin comprimir y sin cifrar desde los almacenes de datos [LocalFolder]() o [TemporaryFolder]() de la aplicación, usa el método **Navigate** con un **URI** que utilice el [esquema ms-appdata](). La compatibilidad de la vista web con este esquema requiere que coloques el contenido en una subcarpeta de la carpeta local o temporal. Esto permite la navegación a los elementos URI como ms-appdata:///local/*folder*/*file*.html and ms-appdata:///temp/*folder*/*file*.html. (Para cargar archivos comprimidos o cifrados, consulta [NavigateToLocalStreamUri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx).) 

Cada una de estas subcarpetas de primer nivel está aislada del contenido de otras subcarpetas de primer nivel. Por ejemplo, puedes navegar a ms-appdata:///temp/folder1/file.html, pero no puedes tener en este archivo un vínculo a ms-appdata:///temp/folder2/file.html. Sin embargo, sí puedes vincular al contenido HTML en el paquete de la aplicación mediante el **esquema ms-appx-web**, y al contenido web mediante los esquemas de URI **http** y **https**.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

Para cargar contenido desde el paquete de la aplicación, usa el método **Navigate** con un **Uri** que utilice el esquema [ms-appx-web scheme](https://msdn.microsoft.com/library/windows/apps/xaml/jj655406.aspx#ms_appx_web). 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

Puedes cargar contenido local a través de un solucionador personalizado mediante el método [NavigateToLocalStreamUri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx). Esto posibilita escenarios avanzados como la descarga y almacenamiento en caché de contenido web para usarlo sin conexión, o la extracción de contenido de un archivo comprimido.

### <a name="responding-to-navigation-events"></a>Responder a eventos de navegación

El control de vista web proporciona varios eventos que puedes usar para responder a estados de carga de contenido y navegación. Los eventos se producen en el siguiente orden para el contenido de vista web raíz: [NavigationStarting](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationstarting.aspx), [ContentLoading](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.contentloading.aspx), [DOMContentLoaded](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.domcontentloaded.aspx), [NavigationCompleted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationcompleted.aspx)


**NavigationStarting**: se produce antes de que la vista web se desplace al nuevo contenido. Puedes cancelar la navegación en un controlador para este evento estableciendo la propiedad WebViewNavigationStartingEventArgs.Cancel en true. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading**: se produce cuando la vista web empieza a cargar contenido nuevo. 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded**: se produce cuando la vista web ha terminado de analizar el contenido HTML actual. 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted**: se produce cuando la vista web ha terminado de cargar el contenido actual, o si se produce un error de navegación. Para determinar si se produjo un error en la navegación, comprueba las propiedades [IsSuccess](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess.aspx) y [WebErrorStatus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus.aspx) de la clase [WebViewNavigationCompletedEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.aspx). 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

Los eventos similares se producen en el mismo orden para cada **iframe** en el contenido de la vista web: 
- [FrameNavigationStarting](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationstarting.aspx): se produce antes de que un fotograma de la vista web navegue a nuevo contenido. 
- [FrameContentLoading](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framecontentloading.aspx): se produce cuando un fotograma de la vista web empieza a cargar contenido nuevo. 
- [FrameDOMContentLoaded](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framedomcontentloaded.aspx): se produce cuando un fotograma de la vista web ha terminado de analizar el contenido HTML actual. 
- [FrameNavigationCompleted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationcompleted.aspx): se produce cuando un fotograma de la vista web ha terminado de cargar el contenido. 

### <a name="responding-to-potential-problems"></a>Responder a posibles problemas

Puedes responder a posibles problemas con el contenido, como scripts de larga duración, elementos que la vista web no puede cargar y advertencias de contenido no seguro. 

La aplicación puede parecer bloqueada mientras se ejecutan scripts. El evento [LongRunningScriptDetected](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.longrunningscriptdetected.aspx) se produce periódicamente mientras la vista web ejecuta JavaScript, lo que ofrece una oportunidad para interrumpir el script. Para determinar cuánto tiempo se ha estado ejecutando el script, comprueba la propiedad [ExecutionTime](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime.aspx) de [WebViewLongRunningScriptDetectedEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.aspx). Para detener el script, establece la propiedad [StopPageScriptExecution](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution.aspx) de los argumentos del evento en **true**. No se volverá a ejecutar el script detenido a menos que se vuelve a cargar durante una navegación de vista web posterior. 

El control de vista web no puede hospedar tipos de archivo arbitrarios. Cuando se intenta cargar contenido que la vista web no puede hospedar, se produce el evento [UnviewableContentIdentified](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unviewablecontentidentified.aspx). Puedes controlar este evento y notificar al usuario, o usar la clase [Launcher](https://msdn.microsoft.com/library/windows/apps/xaml/windows.system.launcher.aspx) para redirigir el archivo a un explorador externo o a otra aplicación.

Del mismo modo, el evento [UnsupportedUriSchemeIdentified](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsupportedurischemeidentified.aspx) se produce cuando se invoca un esquema de URI no compatible en el contenido web, como fbconnect:// o mailto://. Puedes controlar este evento para proporcionar un comportamiento personalizado, en vez de permitir que el iniciador del sistema predeterminado inicie el URI.

El evento [UnsafeContentWarningDisplayingevent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying.aspx) se produce cuando el filtro SmartScreen indica que un contenido no es seguro y se muestra una página de aviso en la vista web. Si el usuario elige continuar con la navegación, cualquier navegación posterior en esa página no desencadena ni el evento ni el aviso.

### <a name="handling-special-cases-for-web-view-content"></a>Administrar casos especiales del contenido de la vista web

Puedes usar la propiedad [ContainsFullScreenElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelement.aspx) y el evento [ContainsFullScreenElementChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelementchanged.aspx) para detectar el estado de pantalla completa, responder al mismo y habilitar esta experiencia para el contenido web, por ejemplo, para reproducir vídeo en pantalla completa. Por ejemplo, puedes usar el evento ContainsFullScreenElementChanged para cambiar el tamaño de la vista web de modo que ocupe la totalidad de la vista de la aplicación o, como se muestra en el siguiente ejemplo, para poner una aplicación en ventana en modo de pantalla completa cuando se desea esta experiencia web.

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

Puedes usar el evento [NewWindowRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.newwindowrequested.aspx) para controlar los casos en los que el contenido web hospedado solicita que se muestre una nueva ventana, como una ventana emergente. Puedes usar otro control de WebView para mostrar el contenido de la ventana solicitada.

Usa el evento [PermissionRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx) para habilitar características web que requieran funcionalidades especiales. Actualmente, esto incluye la geolocalización, el almacenamiento IndexedDB y el audio y vídeo del usuario (por ejemplo, desde un micrófono o una cámara web). Aunque se otorgue el acceso a la ubicación o el contenido multimedia del usuario, es necesario declarar esta funcionalidad en el manifiesto de la aplicación. Por ejemplo, una aplicación que usa la geolocalización precisa las siguientes declaraciones como mínimo en Package.appxmanifest:

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

Además de administrar mediante la aplicación el evento [PermissionRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx), el usuario debe aprobar los cuadros de diálogo estándar del sistema para que la aplicación solicite las capacidades multimedia o de geolocalización que se quieren habilitar.

En el siguiente ejemplo, una aplicación permite la geolocalización en un mapa de Bing:

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

Si la aplicación requiere la entrada del usuario u otras operaciones asincrónicas para responder a una solicitud de permiso, usa el método [Defer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx) de [WebViewPermissionRequest](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.aspx) para crear un [WebViewDeferredPermissionRequest](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewdeferredpermissionrequest.aspx) sobre el que se pueda actuar más adelante. Consulta [WebViewPermissionRequest.Defer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx). 

Si los usuarios deben cerrar sesión de forma segura en un sitio web hospedado en una vista web, o en otros casos en los que la seguridad sea importante, llama al método estático [ClearTemporaryWebDataAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cleartemporarywebdataasync.aspx) para borrar todo el contenido de una sesión de vista web almacenado localmente en caché. Esto impide que usuarios malintencionados obtengan acceso a información confidencial. 

### <a name="interacting-with-web-view-content"></a>Interactuar con el contenido de la vista web

Puede interactuar con el contenido de la vista web usando el método [InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) para invocar o insertar script en dicho contenido, y el evento [ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) para obtener información de este.

Para invocar JavaScript dentro del contenido de la vista web, usa el método [InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx). El script invocado solo puede devolver valores de cadena. 

Por ejemplo, si una vista web llamada `webView1` contiene una función llamada `setDate` con 3 parámetros, puedes invocarla así. 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


Puedes usar **InvokeScriptAsync** con la función JavaScript **eval** para insertar contenido en la página web.

Aquí, el texto de un cuadro de texto XAML (`nameTextBox.Text`) se escribe en un elemento div en una página HTML hospedada en `webView1`. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Los scripts en el contenido de la vista web pueden usar **window.external.notify** con un parámetro de cadena para enviar información a la aplicación. Para recibir estos mensajes, controla el evento [ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx). 

Para permitir que una página web externa active el evento **ScriptNotify** al llamar a window.external.notify, debes incluir el URI de la página en la sección **ApplicationContentUriRules** del manifiesto de la aplicación. (Puedes hacerlo en Microsoft Visual Studio en la pestaña URI de contenido del diseñador Package.appxmanifest). Los URI en esta lista deben usar HTTPS y pueden contener caracteres comodín en el subdominio (por ejemplo, `https://*.microsoft.com`), pero no en el dominio (por ejemplo, `https://*.com` y `https://*.*`). El requisito del manifiesto no se aplica al contenido que se origina en el paquete de la aplicación, que usa un URI ms-local-stream:// o se carga mediante [NavigateToString](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetostring.aspx). 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>Obtener acceso a Windows Runtime en una vista web

Puedes usar el método [AddWebAllowedObject](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx) para insertar una instancia de una clase nativa de un componente de Windows Runtime en el contexto JavaScript de la vista web. Esto ofrece acceso completo a los métodos, propiedades y eventos nativos del objeto en el contenido de JavaScript de la vista web. La clase se debe representar con el atributo [AllowForWeb](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.metadata.allowforwebattribute.aspx). 

Por ejemplo, este código inserta en una vista web una instancia de `MyClass` importada desde un componente de Windows Runtime.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

Para obtener más información, consulta [WebView.AddWebAllowedObject](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx). 

Además, puedes permitir que el contenido de JavaScript confiable en una vista web obtenga acceso directo a las API de Windows Runtime. Esto proporciona eficaces funcionalidades nativas para aplicaciones web hospedadas en una vista web. Para habilitar esta característica, el URI del contenido confiable debe estar en la lista blanca de las ApplicationContentUriRules de la aplicación en Package.appxmanifest, y WindowsRuntimeAccess debe estar establecido específicamente en "all". 

Este ejemplo muestra una sección del manifiesto de la aplicación. Aquí, un URI local obtiene acceso a Windows Runtime. 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>Opciones para alojamiento de contenido web

Puedes usar la propiedad [WebView.Settings](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.settings.aspx) (del tipo [WebViewSettings](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewsettings.aspx)) para controlar si JavaScript e IndexedDB están habilitados. Por ejemplo, si usas una vista web para mostrar contenido estrictamente estático, tal vez quieras deshabilitar JavaScript para obtener el mejor rendimiento.

### <a name="capturing-web-view-content"></a>Capturar contenido de una vista web

Para habilitar el uso compartido del contenido de una vista web con otras aplicaciones, usa el método [CaptureSelectedContentToDataPackageAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync.aspx), que devuelve el contenido seleccionado en forma de [DataPackage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datapackage.aspx). Este método es asincrónico, por lo que debes usar un aplazamiento para evitar que tu controlador de evento [DataRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx) actúe antes de que finalice la llamada asincrónica. 

Para obtener una imagen previa del contenido actual de la vista web, usa el método [CapturePreviewToStreamAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.capturepreviewtostreamasync.aspx). Este método crea una imagen del contenido actual y la escribe en la secuencia especificada. 

### <a name="threading-behavior"></a>Comportamiento de subprocesos

De manera predeterminada, el contenido de una vista web está hospedado en el subproceso de interfaz de usuario en la familia de dispositivos de escritorio, y fuera de dicho subproceso en todos los demás dispositivos. Puedes usar la propiedad estática [WebView.DefaultExecutionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultexecutionmode.aspx) para consultar el comportamiento del subproceso predeterminado para el cliente actual. Si es necesario, puedes usar el constructor [WebView(WebViewExecutionMode)](https://msdn.microsoft.com/library/windows/apps/xaml/dn932036.aspx) para invalidar este comportamiento. 

> **Nota:**&nbsp;&nbsp;Pueden producirse problemas de rendimiento al hospedar contenido en el subproceso de la interfaz de usuario en dispositivos móviles, de modo que asegúrate de probar todos los dispositivos de destino cuando cambies el elemento DefaultExecutionMode.

Una vista web que hospeda contenido en el subproceso de la interfaz de usuario no es compatible con los controles primarios que requieren gestos para propagar desde el control de vista web al elemento primario, como [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx), [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) y otros controles relacionados. Estos controles no podrán recibir los gestos que se inicien en la vista web hospedada en el subproceso. Además, no se admite directamente la impresión de contenido web desde subprocesos. Deberás imprimir los elementos con un relleno [WebViewBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.aspx).

## <a name="recommendations"></a>Recomendaciones


-   Asegúrate de que el sitio web cargado tiene un formato correcto para el dispositivo y usa colores, tipografía y navegación coherentes con el resto de tu aplicación.
-   Los campos de entrada deberían cambiar el tamaño de forma apropiada. Los usuarios podrían no darse cuenta de que pueden acercar la imagen para escribir texto.
-   Si una vista web no se parece al resto de tu aplicación, considera la posibilidad de usar controles alternativos u otras maneras de realizar las tareas relevantes. Si tu vista web coincide con el resto de la aplicación, los usuarios considerarán el conjunto una experiencia óptima.



## <a name="related-topics"></a>Temas relacionados

* [Clase WebView](https://msdn.microsoft.com/library/windows/apps/br227702)
 

 




