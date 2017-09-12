---
author: normesta
Description: "Ampliar tu aplicación de escritorio con componentes e interfaces de usuario de Windows"
Search.Product: eADQiWindows 10XVcnh
title: "Ampliar tu aplicación de escritorio con componentes e interfaces de usuario de Windows"
ms.author: normesta
ms.date: 07/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.openlocfilehash: fce6076f07957b50e83cf80e8d12350630b99456
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2017
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Ampliar tu aplicación de escritorio con componentes de UWP modernos

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. Si quieres agregar estas experiencias, amplía tu aplicación de escritorio con el componente de UWP.

En muchos casos, puedes llamar a las API de UWP directamente desde la aplicación de escritorio, por lo que, antes de revisar esta guía, consulta [Mejorar para Windows 10](desktop-to-uwp-enhance.md).

>[!NOTE]
>Esta guía da por hecho que has creado un paquete de la aplicación de Windows para la aplicación de escritorio mediante el Puente de dispositivo de escritorio. Si aún no has hecho esto, consulta [Puente de dispositivo de escritorio](desktop-to-uwp-root.md).

Si estás listo, comencemos.

## <a name="show-a-modern-xaml-ui"></a>Mostrar una interfaz de usuario de XAML moderna

Como parte del flujo de tu aplicación, puedes incorporar interfaces de usuario basadas en XAML modernas en la aplicación de escritorio. Estas interfaces de usuario pueden ser adaptables naturalmente para diferentes tamaños de pantalla y resoluciones y admitir modelos interactivos modernos, como la entrada táctil y manuscrita.

Por ejemplo, con una pequeña cantidad de marcado XAML, puedes ofrecer a los usuarios potentes características de visualización relacionadas con mapas.

Esta imagen muestra una aplicación de VB6 que abre una interfaz de usuario moderna basada en XAML que contiene un control de mapa.

![diseño adaptativo](images\desktop-to-uwp\extend-xaml-ui.png)

### <a name="have-a-closer-look-at-this-app"></a>Echa un vistazo más a fondo a esta aplicación

:heavy_check_mark: [Ver un vídeo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Add-a-XAML-UI-and-Toast-Notification-to-a-VB6-Application-OsJHC7WhD_8006218965)

:heavy_check_mark: [Obtener la aplicación](https://www.microsoft.com/en-us/store/p/vb6-app-with-xaml-sample/9n191ncxf2f6)

:heavy_check_mark: [Examinar el código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/VB6withXaml)

### <a name="the-design-pattern"></a>El modelo de diseño

Para mostrar una interfaz de usuario basada en XAML, deberás hacer lo siguiente:

:uno: [Agregar un proyecto de UWP a la solución](#project)

:dos: [Agregar una extensión de protocolo a ese proyecto](#protocol)

:tres: [Iniciar la aplicación para UWP desde tu aplicación de escritorio](#start)

:cuatro: [En el proyecto de UWP, muestra la página que quieres](#parse)

<span id="project" />
### <a name="add-a-uwp-project"></a>Agregar un proyecto de UWP

Agrega un proyecto **Aplicación vacía (Windows universal)** a la solución.

<span id="protocol" />
### <a name="add-a-protocol-extension"></a>Agregar una extensión de protocolo

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto y agrega la extensión.

```xml
<Extensions>
      <uap:Extension
          Category="windows.protocol"
          Executable="MapUI.exe"
          EntryPoint=" MapUI.App">
        <uap:Protocol Name="desktopbridgemapsample" />
      </uap:Extension>
    </Extensions>     
```

Asigna un nombre al protocolo, proporciona el nombre del archivo ejecutable generado por el proyecto de UWP y el nombre de la clase del punto de entrada.

También puedes abrir el **package.appxmanifest** en el diseñador, elegir la pestaña **Declaraciones** y, a continuación, agregar la extensión allí.

![Pestaña Declaraciones](images\desktop-to-uwp\protocol-properties.png)



> [!NOTE]
> Los controles de mapa descargan datos de Internet, por lo que si usas uno, tendrás que agregar la funcionalidad de "cliente de Internet" al manifiesto también.

<span id="start" />
### <a name="start-the-uwp-app"></a>Iniciar la aplicación para UWP

En primer lugar, crea un [Uri](https://msdn.microsoft.com/library/system.uri.aspx) que incluya el nombre del protocolo y los parámetros que quieras pasar a la aplicación para UWP. Luego llama al método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_).

Este es un ejemplo básico en C#.

```csharp

private async void showMap(double lat, double lon)
{
    string str = "desktopbridgemapsample://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

    if (success)
    {
        // URI launched
    }
    else
    {
        // URI launch failed
    }
}
```
En nuestro ejemplo, estamos haciendo algo un poco más indirecto. Hemos ajustado la llamada en una función de interoperabilidad invocable por VB6 denominada ``LaunchMap``. Dicha función se escribe usando C++.

Este es el bloque de VB:

```VB
Private Declare Function LaunchMap Lib "UWPWrappers.dll" _
  (ByVal lat As Double, ByVal lon As Double) As Boolean
 
Private Sub EiffelTower_Click()
    LaunchMap 48.858222, 2.2945
End Sub
```

Esta es la función de C++:

```C++

DllExport bool __stdcall LaunchMap(double lat, double lon)
{
  try
  {
    String ^str = ref new String(L"desktopbridgemapsample://");
    Uri ^uri = ref new Uri(
      str + L"location?lat=" + lat.ToString() + L"&?lon=" + lon.ToString());
 
    // now launch the UWP component
    Launcher::LaunchUriAsync(uri);
  }
  catch (Exception^ ex) { return false; }
  return true;
}

```

<span id="parse" />
### <a name="parse-parameters-and-show-a-page"></a>Analizar parámetros y mostrar una página

En la clase **Aplicación** de tu proyecto de UWP, invalida el controlador de eventos **OnActivated**. Si la aplicación se activa por el protocolo, analiza los parámetros y luego abre la página que quieras.

```C++
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ e)
{
  if (e->Kind == ActivationKind::Protocol)
  {
    ProtocolActivatedEventArgs^ protocolArgs = (ProtocolActivatedEventArgs^)e;
    Uri ^uri = protocolArgs->Uri;
    if (uri->SchemeName == "desktopbridgemapsample")
    {
      Frame ^rootFrame = ref new Frame();
      Window::Current->Content = rootFrame;
      rootFrame->Navigate(TypeName(MainPage::typeid), uri->Query);
      Window::Current->Activate();
    }
  }
}
```

### <a name="similar-samples"></a>Ejemplos similares

[Ejemplo de Northwind: ejemplo de un extremo a otro para código heredado de interfaz de usuario de UWA y Win32](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/NorthwindSample)

[Ejemplo de Northwind: aplicación para UWP que se conectar a SQL Server](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SQLServer)

## <a name="provide-services-to-other-apps"></a>Proporcionar servicios a otras aplicaciones

Agrega un servicio que otras aplicaciones pueden consumir. Por ejemplo, puedes agregar un servicio que ofrece a otras aplicaciones acceso controlado a la base de datos subyacente de la aplicación. Al implementar una tarea en segundo plano, las aplicaciones pueden ponerse en contacto con el servicio incluso si no se está ejecutando la aplicación de escritorio.

Este es un ejemplo que hace esto.

![diseño adaptativo](images\desktop-to-uwp\winforms-app-service.png)

### <a name="have-a-closer-look-at-this-app"></a>Echa un vistazo más a fondo a esta aplicación

:heavy_check_mark: [Ver un vídeo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Expose-an-AppService-from-a-Windows-Forms-Data-Application-GiqNS7WhD_706218965)

:heavy_check_mark: [Obtener la aplicación](https://www.microsoft.com/en-us/store/p/winforms-appservice/9p7d9b6nk5tn)

:heavy_check_mark: [Examinar el código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinformsAppService)

### <a name="the-design-pattern"></a>El modelo de diseño

Para proporcionar un servicio, deberás hacer lo siguiente:

:uno: [Agregar un componente de Windows Runtime](#component)

:dos: [Agregar una extensión de servicio de aplicaciones](#extension)

:tres: [Implementar el servicio de aplicaciones](#appservice)

:cuatro: [Probar el servicio de aplicaciones](#test)

<span id="component" />

### <a name="add-a-windows-runtime-component"></a>Agregar un componente de Windows Runtime

Agrega un proyecto **Componente de Windows Runtime (Windows Universal)** a la solución.

A continuación, haz referencia al proyecto del componente en tiempo de ejecución de tu proyecto de empaquetado de UWP.

<span id="extension" />
### <a name="add-an-app-service-extension"></a>Agregar una extensión de servicio de aplicaciones

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto de empaquetado y agrega una extensión de servicio de aplicaciones.

```xml
<Extensions>
      <uap:Extension
          Category="windows.appService"
          EntryPoint="MyAppService.AppServiceTask">
        <uap:AppService Name="com.microsoft.samples.winforms" />
      </uap:Extension>
    </Extensions>    
```

Asigna un nombre al servicio de aplicaciones y proporciona el nombre de la clase del punto de entrada. Esta es la clase que usarás para implementar tu servicio de aplicaciones.

<span id="appservice" />
### <a name="implement-the-app-service"></a>Implementar el servicio de aplicaciones

Aquí es donde podrás validar y controlar solicitudes de otras aplicaciones.

```csharp
public sealed class AppServiceTask : IBackgroundTask
{
    private BackgroundTaskDeferral backgroundTaskDeferral;
 
    public void Run(IBackgroundTaskInstance taskInstance)
    {
        this.backgroundTaskDeferral = taskInstance.GetDeferral();
        taskInstance.Canceled += OnTaskCanceled;
        var details = taskInstance.TriggerDetails as AppServiceTriggerDetails;
        details.AppServiceConnection.RequestReceived += OnRequestReceived;
    }
 
    private async void OnRequestReceived(AppServiceConnection sender,
                                         AppServiceRequestReceivedEventArgs args)
    {
        var messageDeferral = args.GetDeferral();
        ValueSet message = args.Request.Message;
        string id = message["ID"] as string;
        ValueSet returnData = DataBase.GetData(id);
        await args.Request.SendResponseAsync(returnData);
        messageDeferral.Complete();
    }
 
 
    private void OnTaskCanceled(IBackgroundTaskInstance sender,
                                BackgroundTaskCancellationReason reason)
    {
        if (this.backgroundTaskDeferral != null)
        {
            this.backgroundTaskDeferral.Complete();
        }
    }
}
```

<span id="test" />
### <a name="test-the-app-service"></a>Probar el servicio de aplicaciones

Prueba el servicio llamándole desde otra aplicación.

```csharp
private async void button_Click(object sender, RoutedEventArgs e)
{
    AppServiceConnection dataService = new AppServiceConnection();
    dataService.AppServiceName = "com.microsoft.samples.winforms";
    dataService.PackageFamilyName = "Microsoft.SDKSamples.WinformWithAppService";
 
    var status = await dataService.OpenAsync();
    if (status == AppServiceConnectionStatus.Success)
    {
        string id = int.Parse(textBox.Text);
        var message = new ValueSet();
        message.Add("ID", id);
        AppServiceResponse response = await dataService.SendMessageAsync(message);
        string result = "";
 
        if (response.Status == AppServiceResponseStatus.Success)
        {
            if (response.Message["Status"] as string == "OK")
            {
                DisplayResult(response.Message["Result"]);
            }
        }
    }
}
```

Obtén más información sobre los servicios de aplicaciones aquí: [Crear y usar un servicio de aplicaciones](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

### <a name="similar-samples"></a>Ejemplos similares

[Ejemplo de puente de servicio de aplicaciones](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample)

[Ejemplo de puente de servicio de aplicaciones con la aplicación win32 de C++](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/AppServiceBridgeSample_C%2B%2B)

[Aplicación MFC que recibe notificaciones push](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/MFCwithPush)


## <a name="making-your-desktop-application-a-share-target"></a>Hacer que la aplicación de escritorio sea un destino de recursos compartidos

Puedes hacer la aplicación de escritorio sea un destino de recursos compartidos para que los usuarios puedan compartir con facilidad datos, como imágenes de otras aplicaciones que admiten el uso compartido.

Por ejemplo, los usuarios podrían elegir tu aplicación para compartir imágenes desde Microsoft Edge, la aplicación Fotos. Esta es una aplicación de muestra WPF que tiene esa funcionalidad.

![destino de uso compartido](images\desktop-to-uwp\share-target.png)

### <a name="have-a-closer-look-at-this-app"></a>Echa un vistazo más a fondo a esta aplicación

:heavy_check_mark: [Ver un vídeo](https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373/Demo-Make-a-WPF-Application-a-Share-Target-xd6Fu6WhD_8406218965)

:heavy_check_mark: [Obtener la aplicación](https://www.microsoft.com/en-us/store/p/wpf-app-as-sharetarget/9pjcjljlck37)

:heavy_check_mark: [Examinar el código](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WPFasShareTarget)

### <a name="the-design-pattern"></a>El modelo de diseño

Para que la aplicación sea un destino de recursos compartidos, deberás hacer lo siguiente:

:uno: [Agregar un proyecto de UWP a la solución](#project2)

:dos: [Agrega una extensión de destino de recursos compartidos](#share-extension)

:tres: [Invalidar el controlador de eventos OnNavigatedTo](#override)

<span id="project2" />
### <a name="add-a-uwp-project-to-your-solution"></a>Agregar un proyecto de UWP a la solución

Agrega un proyecto **Aplicación vacía (Windows universal)** a la solución.

<span id="share-extension" />
### <a name="add-a-share-target-extension"></a>Agregar una extensión de destino de recursos compartidos

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto y agrega la extensión.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="ShareTarget.App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Proporciona el nombre del archivo ejecutable generado por el proyecto de UWP y el nombre de la clase del punto de entrada. También tendrás que especificar qué tipos de archivos se pueden compartir con la aplicación.

<span id="override" />
### <a name="override-the-onnavigatedto-event-handler"></a>Invalidar el controlador de eventos OnNavigatedTo

Invalida el controlador de eventos **OnActivated** en la clase **Aplicación** de tu proyecto de UWP.

A este controlador de eventos se llama cuando los usuarios eligen tu aplicación para compartir sus archivos.

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
  this.shareOperation = (ShareOperation)e.Parameter;
  if (this.shareOperation.Data.Contains(StandardDataFormats.StorageItems))
  {
      this.sharedStorageItems =
        await this.shareOperation.Data.GetStorageItemsAsync();
       
      foreach (StorageFile item in this.sharedStorageItems)
      {
          ProcessSharedFile(item);
      }
  }
}
```

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a preguntas específicas**

Nuestro equipo supervisa estas [etiquetas de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Envíanos tus comentarios acerca de este artículo**

Usa la sección comentarios que tienes a continuación.
