---
description: Un control de vista web inserta una vista en la aplicación, que representa el contenido web mediante el motor de representación de Microsoft Edge. En un control de vista web también pueden aparecer y funcionar hipervínculos.
title: Vista web
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f2f3bf022210e5b5f329cb5824cb36708154b977
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034388"
---
# <a name="web-view"></a>Vista web

Un control de vista web inserta una vista en la aplicación, que representa el contenido web mediante el motor de representación de Microsoft Edge. En un control de vista web también pueden aparecer y funcionar hipervínculos.

> **API importantes** : [clase WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control de vista web para mostrar contenido HTML de formato enriquecido desde un servidor web remoto, código generado de forma dinámica o archivos de contenido en tu paquete de la aplicación. El contenido enriquecido también puede contener código de script y establecer la comunicación entre el script y el código de tu aplicación.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/WebView">abrirla y ver WebView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-web-view"></a>Creación de una vista web

**Modificación de la apariencia de una vista web**

[WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) no es una subclase de [Control](/uwp/api/Windows.UI.Xaml.Controls.Control), por lo que no tiene una plantilla de control. Sin embargo, puedes establecer varias propiedades para controlar algunos aspectos visuales de la vista web.
- Para restringir el área de visualización, establece las propiedades [Width](/uwp/api/windows.ui.xaml.frameworkelement.width) y [Height](/uwp/api/windows.ui.xaml.frameworkelement.height). 
- Para traducir, escalar, sesgar y girar una vista web, usa la propiedad [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform).
- Para controlar la opacidad de la vista web, establece la propiedad [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity).
- Para especificar un color que se usa como fondo de la página web cuando el contenido HTML no especifica uno, establece la propiedad [DefaultBackgroundColor](/uwp/api/windows.ui.xaml.controls.webview.defaultbackgroundcolor). 

**Obtención del título de la página web**

Puedes obtener el título del documento HTML que se muestra actualmente en la vista web mediante la propiedad [DocumentTitle](/uwp/api/windows.ui.xaml.controls.webview.documenttitle). 

**Eventos de entrada y orden de tabulación**

Aunque WebView no es una subclase de Control, recibe el foco de entrada del teclado y participa en la secuencia de tabulación. Proporciona un método [Focus](/uwp/api/windows.ui.xaml.controls.webview.focus) y eventos [GotFocus](/uwp/api/windows.ui.xaml.uielement.gotfocus) y [LostFocus](/uwp/api/windows.ui.xaml.uielement.lostfocus), pero carece de propiedades relacionadas con la tabulación. Su posición en la secuencia de tabulación es igual a su posición en el orden de documentos XAML. La secuencia de tabulación incluye todos los elementos de la vista web que pueden recibir el foco de entrada. 

Tal y como se indica en la tabla Events de la página de la clase [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView), la vista web no admite la mayoría de los eventos de entrada de usuario heredados de [UIElement](/uwp/api/Windows.UI.Xaml.UIElement), como [KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown), [KeyUp](/uwp/api/windows.ui.xaml.uielement.keyup) y [PointerPressed](/uwp/api/windows.ui.xaml.uielement.pointerpressed). En vez de eso, puedes usar [InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) con la función JavaScript **eval** para usar los controladores de eventos HTML y usar **window.external.notify** desde el controlador de eventos HTML para notificar a la aplicación mediante [WebView.ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify).

### <a name="navigating-to-content"></a>Navegación al contenido

Vista Web proporciona varias API para una navegación básica: [GoBack](/uwp/api/windows.ui.xaml.controls.webview.goback), [GoForward](/uwp/api/windows.ui.xaml.controls.webview.goforward), [Stop](/uwp/api/windows.ui.xaml.controls.webview.stop), [Refresh](/uwp/api/windows.ui.xaml.controls.webview.refresh), [CanGoBack](/uwp/api/windows.ui.xaml.controls.webview.cangoback) y [CanGoForward](/uwp/api/windows.ui.xaml.controls.webview.cangoforward). Puedes usarlas para agregar a tu aplicación funcionalidades de exploración web típicas. 

Para establecer el contenido inicial de la vista web, debes establecer la propiedad [Source](/uwp/api/windows.ui.xaml.controls.webview.source) en XAML. El analizador XAML convierte automáticamente la cadena en un [URI](/uwp/api/Windows.Foundation.Uri). 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

La propiedad Source se puede establecer en el código, pero es más habitual usar uno de los métodos **Navigate** para cargar contenido en el código. 

Para cargar contenido web, usa el método [Navigate](/uwp/api/windows.ui.xaml.controls.webview.navigate) con un **URI** que utilice el esquema http o https. 

```csharp
webView1.Navigate("http://www.contoso.com");
```

Para navegar a un URI con una solicitud POST y encabezados HTTP, usa el método [NavigateWithHttpRequestMessage](/uwp/api/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage). Este método solo admite [HttpMethod.Post](/uwp/api/windows.web.http.httpmethod.post) y [HttpMethod.Get](/uwp/api/windows.web.http.httpmethod.get) como valores de la propiedad [HttpRequestMessage.Method](/uwp/api/windows.web.http.httprequestmessage.method). 

Para cargar contenido sin comprimir y sin cifrar desde los almacenes de datos [LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) o [TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder) de la aplicación, usa el método **Navigate** con un **URI** que utilice [ms-appdata scheme](../../app-resources/uri-schemes.md). La compatibilidad de la vista web con este esquema requiere que coloques el contenido en una subcarpeta de la carpeta local o temporal. Esto permite la navegación a identificadores URI como ms-appdata:///local/ *carpeta*/*archivo*.html y ms-appdata:///temp/ *carpeta*/*archivo*.html. (Para cargar archivos comprimidos o cifrados, consulta [NavigateToLocalStreamUri](/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri)). 

Cada una de estas subcarpetas de primer nivel está aislada del contenido de otras subcarpetas de primer nivel. Por ejemplo, puedes ir a ms-appdata:///temp/carpeta1/archivo.html, pero en este archivo no puedes tener un vínculo a ms-appdata:///temp/carpeta2/archivo.html. Sin embargo, sí puedes establecer un vínculo al contenido HTML del paquete de la aplicación mediante el **esquema ms-appx-web** , y al contenido web mediante los esquemas URI **http** y **https**.

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

Para cargar contenido desde el paquete de la aplicación, usa el método **Navigate** con un **URI** que utilice el [esquema ms-appx-web](/previous-versions/windows/apps/jj655406(v=win.10)). 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

Puedes cargar contenido local a través de una resolución personalizada mediante el método [NavigateToLocalStreamUri](/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri). Esto posibilita escenarios avanzados como la descarga y el almacenamiento en caché de contenido web para usarlo sin conexión, o la extracción de contenido de un archivo comprimido.

### <a name="responding-to-navigation-events"></a>Respuesta a eventos de navegación

El control de vista web proporciona varios eventos que puedes usar para responder a estados de carga de contenido y navegación. Para el contenido de vista web raíz los eventos se producen en el siguiente orden: [NavigationStarting](/uwp/api/windows.ui.xaml.controls.webview.navigationstarting), [ContentLoading](/uwp/api/windows.ui.xaml.controls.webview.contentloading), [DOMContentLoaded](/uwp/api/windows.ui.xaml.controls.webview.domcontentloaded), [NavigationCompleted](/uwp/api/windows.ui.xaml.controls.webview.navigationcompleted)


**NavigationStarting** : se produce antes de que la vista web se desplace al nuevo contenido. Para cancelar la navegación en un controlador para este evento, establece la propiedad WebViewNavigationStartingEventArgs.Cancel en true. 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** : se produce cuando la vista web empieza a cargar contenido nuevo. 

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

**DOMContentLoaded** : se produce cuando la vista web ha terminado de analizar el contenido HTML actual. 

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

**NavigationCompleted** : se produce cuando la vista web ha terminado de cargar el contenido actual, o si se produce un error de navegación. Para determinar si se produjo un error en la navegación, comprueba las propiedades [IsSuccess](/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess) y [WebErrorStatus](/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus) de la clase [WebViewNavigationCompletedEventArgs](/uwp/api/Windows.UI.Xaml.Controls.WebViewNavigationCompletedEventArgs). 

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
- [FrameNavigationStarting](/uwp/api/windows.ui.xaml.controls.webview.framenavigationstarting): se produce antes de que un fotograma en la vista web navegue a nuevo contenido. 
- [FrameContentLoading](/uwp/api/windows.ui.xaml.controls.webview.framecontentloading): se produce cuando un fotograma en la vista web empieza a cargar contenido nuevo. 
- [FrameDOMContentLoaded](/uwp/api/windows.ui.xaml.controls.webview.framedomcontentloaded): se produce cuando un fotograma en la vista web ha terminado de analizar el contenido HTML actual. 
- [FrameNavigationCompleted](/uwp/api/windows.ui.xaml.controls.webview.framenavigationcompleted): se produce cuando un fotograma en la vista web ha terminado de cargar el contenido. 

### <a name="responding-to-potential-problems"></a>Respuesta a posibles problemas

Puedes responder a posibles problemas con el contenido, como scripts de larga duración, elementos que la vista web no puede cargar y advertencias de contenido no seguro. 

Puede parecer que la aplicación no responde mientras se ejecutan scripts. El evento [LongRunningScriptDetected](/uwp/api/windows.ui.xaml.controls.webview.longrunningscriptdetected) se produce periódicamente mientras la vista web ejecuta JavaScript, lo que ofrece una oportunidad para interrumpir el script. Para determinar cuánto tiempo se ha estado ejecutando el script, comprueba la propiedad [ExecutionTime](/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime) de [WebViewLongRunningScriptDetectedEventArgs](/uwp/api/Windows.UI.Xaml.Controls.WebViewLongRunningScriptDetectedEventArgs). Para detener el script, establece la propiedad [StopPageScriptExecution](/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution) de los argumentos del evento en **true**. No se volverá a ejecutar el script detenido a menos que se vuelva a cargar durante una navegación posterior por la vista web. 

El control de vista web no puede hospedar tipos de archivo arbitrarios. Cuando se intenta cargar contenido que la vista web no puede hospedar, se produce el evento [UnviewableContentIdentified](/uwp/api/windows.ui.xaml.controls.webview.unviewablecontentidentified). Puedes controlar este evento y notificar al usuario, o usar la clase [Launcher](/uwp/api/Windows.System.Launcher) para redirigir el archivo a un explorador externo o a otra aplicación.

Del mismo modo, el evento [UnsupportedUriSchemeIdentified](/uwp/api/windows.ui.xaml.controls.webview.unsupportedurischemeidentified) se produce cuando se invoca un esquema de URI no compatible en el contenido web, como fbconnect:// o mailto://. Puedes controlar este evento para proporcionar un comportamiento personalizado, en vez de permitir que el iniciador del sistema predeterminado inicie el URI.

El evento [UnsafeContentWarningDisplayingevent](/uwp/api/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying) se produce cuando el filtro SmartScreen indica que un contenido no es seguro y se muestra una página de aviso en la vista web. Si el usuario elige continuar con la navegación, la navegación posterior en esa página no mostrará la advertencia ni activará el evento.

### <a name="handling-special-cases-for-web-view-content"></a>Control de casos especiales del contenido de la vista web

Puedes usar la propiedad [ContainsFullScreenElement](/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelement) y el evento [ContainsFullScreenElementChanged](/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelementchanged) para detectar el estado de pantalla completa, responder al mismo y habilitar esta experiencia para el contenido web, por ejemplo, para reproducir vídeo de pantalla completa. Por ejemplo, puedes usar el evento ContainsFullScreenElementChanged para cambiar el tamaño de la vista web de modo que ocupe la totalidad de la vista de la aplicación, o bien, como se muestra en el siguiente ejemplo, colocar una aplicación en ventana en modo de pantalla completa cuando quieras esta experiencia web.

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

Puedes usar el evento [NewWindowRequested](/uwp/api/windows.ui.xaml.controls.webview.newwindowrequested) para controlar los casos en los que el contenido web hospedado solicite que se muestre una nueva ventana, como una ventana emergente. Puedes usar otro control WebView para mostrar el contenido de la ventana solicitada.

Usa el evento [PermissionRequested](/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) para habilitar características web que requieran funciones especiales. Actualmente, incluyen la geolocalización, el almacenamiento IndexedDB, y el audio y vídeo del usuario (por ejemplo, de un micrófono o una cámara web). Aunque la aplicación tenga acceso a la ubicación o el contenido multimedia del usuario, es necesario declarar esta funcionalidad en el manifiesto de la aplicación. Por ejemplo, una aplicación que usa la geolocalización precisa las siguientes declaraciones de funcionalidades como mínimo en Package.appxmanifest:

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

Además de administrar mediante la aplicación el evento [PermissionRequested](/uwp/api/windows.ui.xaml.controls.webview.permissionrequested), el usuario debe aprobar los cuadros de diálogo estándar del sistema para que la aplicación solicite las funciones multimedia o de ubicación que se quieren habilitar.

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

Si la aplicación requiere la entrada del usuario u otras operaciones asincrónicas para responder a una solicitud de permiso, usa el método [Defer](/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer) de [WebViewPermissionRequest](/uwp/api/Windows.UI.Xaml.Controls.WebViewPermissionRequest) para crear un elemento [WebViewDeferredPermissionRequest](/uwp/api/Windows.UI.Xaml.Controls.WebViewDeferredPermissionRequest) sobre el que se pueda actuar más adelante. Consulta [WebViewPermissionRequest.Defer](/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer). 

Si los usuarios deben cerrar sesión de forma segura en un sitio web hospedado en una vista web, o en otros casos en los que la seguridad sea importante, llama al método estático [ClearTemporaryWebDataAsync](/uwp/api/windows.ui.xaml.controls.webview.cleartemporarywebdataasync) para borrar todo el contenido de una sesión de vista web almacenado localmente en caché. Esto impide que usuarios malintencionados obtengan acceso a información confidencial. 

### <a name="interacting-with-web-view-content"></a>Interacción con el contenido de la vista web

Puedes interactuar con el contenido de la vista web usando el método [InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) para invocar o insertar script en dicho contenido, y el evento [ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) para obtener información de este.

Para invocar JavaScript dentro del contenido de la vista web, usa el método [InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync). El script invocado solo puede devolver valores de cadena. 

Por ejemplo, si el contenido una vista web llamada `webView1` contiene una función llamada `setDate` que utiliza 3 parámetros, puedes invocarla de la manera siguiente. 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


Puedes usar **InvokeScriptAsync** con la función **eval** de JavaScript para insertar contenido en la página web.

Aquí, el texto de un cuadro de texto XAML (`nameTextBox.Text`) se escribe en un elemento div en una página HTML hospedada en `webView1`. 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Los scripts del contenido de la vista web pueden usar **window.external.notify** con un parámetro de cadena para enviar información a la aplicación. Para recibir estos mensajes, controla el evento [ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify). 

Para permitir que una página web externa active el evento **ScriptNotify** al llamar a window.external.notify, debes incluir el URI de la página en la sección **ApplicationContentUriRules** del manifiesto de la aplicación. (Puedes hacerlo en Microsoft Visual Studio, en la pestaña de URI de Contenido del diseñador Package.appxmanifest). Los URI de esta lista deben usar HTTPS y pueden contener caracteres comodín de subdominio (por ejemplo, `https://*.microsoft.com`), pero no pueden contener caracteres comodín de dominio (por ejemplo, `https://*.com` y `https://*.*`). El requisito del manifiesto no se aplica al contenido que se origina en el paquete de la aplicación, que usa un URI ms-local-stream:// o se carga mediante [NavigateToString](/uwp/api/windows.ui.xaml.controls.webview.navigatetostring). 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>Acceso a Windows Runtime en una vista web

Puedes usar el método [AddWebAllowedObject](/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject) para insertar una instancia de una clase nativa de un componente de Windows Runtime en el contexto JavaScript de la vista web. Así dispondrás de acceso completo a los métodos, propiedades y eventos nativos del objeto en el contenido JavaScript de la vista web. La clase se debe representar con el atributo [AllowForWeb](/uwp/api/Windows.Foundation.Metadata.AllowForWebAttribute). 

Por ejemplo, este código inserta en una vista web una instancia de `MyClass` importada de un componente de Windows Runtime.

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

Para obtener más información, consulta [WebView.AddWebAllowedObject](/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject). 

Además, puedes permitir que el contenido JavaScript confiable de una vista web acceda directamente a las API de Windows Runtime. Esto proporciona funcionalidades nativas de alta eficacia para aplicaciones web hospedadas en una vista web. Para habilitar esta característica, el URI del contenido confiable debe estar en la lista blanca del elemento ApplicationContentUriRules de la aplicación en Package.appxmanifest, y WindowsRuntimeAccess debe estar establecido específicamente en "all". 

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

### <a name="options-for-web-content-hosting"></a>Opciones de hospedaje de contenido web

Puedes usar la propiedad [WebView.Settings](/uwp/api/windows.ui.xaml.controls.webview.settings) (del tipo [WebViewSettings](/uwp/api/Windows.UI.Xaml.Controls.WebViewSettings)) para controlar si JavaScript e IndexedDB están habilitados. Por ejemplo, si usas una vista web para mostrar contenido estrictamente estático, puedes deshabilitar JavaScript para obtener el mejor rendimiento.

### <a name="capturing-web-view-content"></a>Captura de contenido de una vista web

Para habilitar el uso compartido del contenido de una vista web con otras aplicaciones, usa el método [CaptureSelectedContentToDataPackageAsync](/uwp/api/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync), que devuelve el contenido seleccionado como [DataPackage](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Este método es asincrónico, por lo que debes usar un aplazamiento para evitar que el controlador de evento [DataRequested](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) actúe antes de que finalice la llamada asincrónica. 

Para obtener una imagen previa del contenido actual de la vista web, usa el método [CapturePreviewToStreamAsync](/uwp/api/windows.ui.xaml.controls.webview.capturepreviewtostreamasync). Este método crea una imagen del contenido actual y la escribe en la secuencia especificada. 

### <a name="threading-behavior"></a>Comportamiento de subprocesos

De manera predeterminada, el contenido de una vista web se hospeda en el subproceso de la interfaz de usuario en la familia de dispositivos de escritorio, y fuera de dicho subproceso en todos los demás dispositivos. Puedes usar la propiedad estática [WebView.DefaultExecutionMode](/uwp/api/windows.ui.xaml.controls.webview.defaultexecutionmode) para consultar el comportamiento del subproceso predeterminado para el cliente actual. Si es necesario, puedes usar el constructor [WebView(WebViewExecutionMode)](/uwp/api/windows.ui.xaml.controls.webview.-ctor#Windows_UI_Xaml_Controls_WebView__ctor_Windows_UI_Xaml_Controls_WebViewExecutionMode_) para invalidar este comportamiento. 

> **Nota**&nbsp;&nbsp;Pueden producirse problemas de rendimiento al hospedar contenido en el subproceso de la interfaz de usuario en dispositivos móviles, así que asegúrate de probar todos los dispositivos de destino cuando cambies el elemento DefaultExecutionMode.

Una vista web que hospeda contenido en el subproceso de la interfaz de usuario no es compatible con los controles primarios que requieren gestos para propagar desde el control de vista web al elemento primario, como [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView), [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) y otros controles relacionados. Estos controles no podrán recibir los gestos que se inicien en la vista web fuera del subproceso. Además, no se admite directamente la impresión de contenido web desde subprocesos. Deberás imprimir los elementos con un relleno [WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="recommendations"></a>Recomendaciones


-   Asegúrate de que el sitio web cargado tiene un formato correcto para el dispositivo y usa colores, tipografía y navegación coherentes con el resto de tu aplicación.
-   Los campos de entrada deben tener el tamaño correcto. Los usuarios podrían no darse cuenta de que pueden acercar la imagen para escribir texto.
-   Si una vista web no se parece al resto de tu aplicación, considera la posibilidad de usar controles alternativos u otras maneras de realizar las tareas pertinentes. Si tu vista web coincide con el resto de la aplicación, los usuarios considerarán el conjunto una experiencia óptima.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Temas relacionados

- [Clase WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView)
