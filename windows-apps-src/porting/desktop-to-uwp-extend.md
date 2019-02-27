---
Description: Extend your desktop application with Windows UIs and components
Search.Product: eADQiWindows 10XVcnh
title: Ampliar tu aplicación de escritorio con componentes e interfaces de usuario de Windows
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2d1fac6d735d4f6915dea1af531dffa666607fe3
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117815"
---
# <a name="extend-your-desktop-application-with-modern-uwp-components"></a>Ampliar tu aplicación de escritorio con componentes de UWP modernos

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. Si quieres agregar estas experiencias, amplía tu aplicación de escritorio con el componente de proyectos de UWP y componentes de Windows Runtime.

En muchos casos puede llamar a Windows Runtime APIs directamente desde la aplicación de escritorio, por lo tanto, antes de revisar esta guía, consulta [mejorar para Windows 10](desktop-to-uwp-enhance.md).

>[!NOTE]
>Esta guía se da por hecho que has creado un paquete de aplicación de Windows para la aplicación de escritorio. Si aún no has hecho esto, vea [empaquetar aplicaciones de escritorio](desktop-to-uwp-root.md).

Si estás listo, comencemos.

<a id="setup" />

## <a name="first-setup-your-solution"></a>En primer lugar, configura tu solución

Agrega uno o varios proyectos de UWP y componentes de Runtime a la solución.

Comienza con una solución que contenga un **proyecto de empaquetado de aplicación Windows** con una referencia a la aplicación de escritorio.

Esta imagen muestra un ejemplo de solución.

![Ampliar proyecto de inicio](images/desktop-to-uwp/extend-start-project.png)

Si la solución no contiene un proyecto de empaquetado, consulta el [paquete de la aplicación de escritorio con Visual Studio](desktop-to-uwp-packaging-dot-net.md).

### <a name="configure-the-desktop-application"></a>Configurar la aplicación de escritorio

Asegúrate de que la aplicación de escritorio tiene referencias a los archivos que se debe llamar a Windows Runtime APIs.

Para ello, consulta la sección [en primer lugar, configura el proyecto](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project) del tema [mejorar tu aplicación de escritorio para Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance#first-set-up-your-project).

### <a name="add-a-uwp-project"></a>Agregar un proyecto de UWP

Agrega una **Aplicación vacía (Windows universal)** a la solución.

Aquí es donde debes crear una UI de XAML moderna o usar las API que se ejecutan solo dentro de un proceso de UWP.

![Proyecto de UWP](images/desktop-to-uwp/add-uwp-project-to-solution.png)

En tu proyecto de empaquetado, haz clic con el botón derecho en el nodo **Applications** y, a continuación, elige **Agregar referencia**.

![Añadir referencia de proyecto de UWP](images/desktop-to-uwp/add-uwp-project-reference.png)

A continuación, añade una referencia al proyecto de UWP.

![Añadir referencia de proyecto de UWP](images/desktop-to-uwp/choose-uwp-project.png)

Tu solución debe tener un aspecto similar a este:

![Solución con proyecto de UWP](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Opcional) Agregar un componente de Windows Runtime

Para realizar algunos escenarios, tendrás que agregar código a un componente de Windows Runtime.

![servicio de aplicación de componente de Runtime](images/desktop-to-uwp/add-runtime-component.png)

A continuación, desde tu proyecto de UWP, agrega una referencia al componente de Runtime. Tu solución debe tener un aspecto similar a este:

![Referencia de componente de Runtime](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>Compilar la solución

Compilar la solución para garantizar que no aparece ningún error. Si recibes errores, abre el **Administrador de configuración** y asegúrate de que tus proyectos destinados a la misma plataforma.

![Administrador de configuración](images/desktop-to-uwp/config-manager.png)

Echemos un vistazo a algunas cosas que puedes hacer con los proyectos de UWP y los componentes de Runtime.

## <a name="show-a-modern-xaml-ui"></a>Mostrar una interfaz de usuario de XAML moderna

Como parte del flujo de tu aplicación, puedes incorporar interfaces de usuario basadas en XAML modernas en la aplicación de escritorio. Estas interfaces de usuario pueden ser adaptables naturalmente para diferentes tamaños de pantalla y resoluciones y admitir modelos interactivos modernos, como la entrada táctil y manuscrita.

Por ejemplo, con una pequeña cantidad de marcado XAML, puedes ofrecer a los usuarios potentes características de visualización relacionadas con mapas.

Esta imagen muestra una aplicación de Windows Forms que abre una interfaz de usuario moderna basada en XAML que contiene un control de mapa.

![diseño adaptativo](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>En este ejemplo se muestra una UI de XAML mediante la adición de un proyecto de UWP a la solución. Este es el enfoque admitido estable mostrar interfaces de usuario de XAML en una aplicación de escritorio. La alternativa de este enfoque es para agregar controles de XAML de UWP directamente a la aplicación de escritorio mediante el uso de una isla de XAML. Islas de XAML están actualmente disponibles como una vista previa de desarrollador. Aunque te animamos a probarlas en su propio código prototipo ahora, no recomendamos que uses ellos en el código de producción en este momento. Estas API y los controles seguirán madurando y estabilizar en futuras versiones de Windows. Para obtener más información sobre cómo islas de XAML, consulta [los controles de UWP en aplicaciones de escritorio](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)

### <a name="the-design-pattern"></a>El modelo de diseño

Para mostrar una interfaz de usuario basada en XAML, deberás hacer lo siguiente:

:uno: [Configurar la solución](#solution-setup)

:dos: [Crear una interfaz de usuario de XAML](#xaml-UI)

:tres: [Agregar una extensión de protocolo al proyecto de UWP](#add-a-protocol-extension)

:cuatro: [Iniciar la aplicación para UWP desde tu aplicación de escritorio](#start)

:cinco: [En el proyecto de UWP, muestra la página que quieres](#parse)

<a id="solution-setup" />

### <a name="setup-your-solution"></a>Configurar la solución

Para obtener instrucciones generales sobre cómo configurar la solución, consulta la sección [En primer lugar, configura tu solución](#setup) al principio de esta guía.

Tu solución tendría un aspecto similar a este:

![Solución de interfaz de usuario para XAML](images/desktop-to-uwp/xaml-ui-solution.png)

En este ejemplo, el proyecto de Windows Forms se denomina **Landmarks** y el proyecto de UWP que contiene la interfaz de usuario de XAML **MapUI**.

<a id="xaml-UI" />

### <a name="create-a-xaml-ui"></a>Crear una interfaz de usuario de XAML

Agregar una interfaz de usuario de XAML a tu proyecto de UWP Este es el XAML para un mapa básico.

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Margin="12,20,12,14">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <maps:MapControl x:Name="myMap" Grid.Column="0" Width="500" Height="500"
                     ZoomLevel="{Binding ElementName=zoomSlider,Path=Value, Mode=TwoWay}"
                     Heading="{Binding ElementName=headingSlider,Path=Value, Mode=TwoWay}"
                     DesiredPitch="{Binding ElementName=desiredPitchSlider,Path=Value, Mode=TwoWay}"
                     HorizontalAlignment="Left"
                     MapServiceToken="<Your Key Goes Here" />
    <Grid Grid.Column="1" Margin="12">
        <StackPanel>
            <Slider Minimum="1" Maximum="20" Header="ZoomLevel" Name="zoomSlider" Value="17.5"/>
            <Slider Minimum="0" Maximum="360" Header="Heading" Name="headingSlider" Value="0"/>
            <Slider Minimum="0" Maximum="64" Header=" DesiredPitch" Name="desiredPitchSlider" Value="32"/>
        </StackPanel>
    </Grid>
</Grid>
```

### <a name="add-a-protocol-extension"></a>Agregar una extensión de protocolo

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto de empaquetado en tu solución y agrega esta extensión.

```xml
<Extensions>
  <uap:Extension Category="windows.protocol" Executable="MapUI.exe" EntryPoint="MapUI.App">
    <uap:Protocol Name="xamluidemo" />
  </uap:Extension>
</Extensions>
```

Asigna un nombre al protocolo, proporciona el nombre del archivo ejecutable generado por el proyecto de UWP y el nombre de la clase del punto de entrada.

También puedes abrir el **package.appxmanifest** en el diseñador, elegir la pestaña **Declaraciones** y, a continuación, agregar la extensión allí.

![Pestaña Declaraciones](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Los controles de mapa descargan datos de Internet, por lo que si usas uno, tendrás que agregar la funcionalidad de "cliente de Internet" al manifiesto también.

<a id="start" />

### <a name="start-the-uwp-app"></a>Iniciar la aplicación para UWP

En primer lugar, desde tu aplicación de escritorio, crea un [Uri](https://msdn.microsoft.com/library/system.uri.aspx) que incluya el nombre del protocolo y los parámetros que quieras pasar a la aplicación para UWP. Luego llama al método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync).

```csharp

private void Statue_Of_Liberty_Click(object sender, EventArgs e)
{
    ShowMap(40.689247, -74.044502);
}

private async void ShowMap(double lat, double lon)
{
    string str = "xamluidemo://";

    Uri uri = new Uri(str + "location?lat=" +
        lat.ToString() + "&?lon=" + lon.ToString());

    var success = await Windows.System.Launcher.LaunchUriAsync(uri);

}
```

<a id="parse" />

### <a name="parse-parameters-and-show-a-page"></a>Analizar parámetros y mostrar una página

En la clase **Aplicación** de tu proyecto de UWP, invalida el controlador de eventos **OnActivated**. Si la aplicación se activa por el protocolo, analiza los parámetros y luego abre la página que quieras.

```csharp
protected override void OnActivated(Windows.ApplicationModel.Activation.IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        ProtocolActivatedEventArgs protocolArgs = (ProtocolActivatedEventArgs)e;
        Uri uri = protocolArgs.Uri;
        if (uri.Scheme == "xamluidemo")
        {
            Frame rootFrame = new Frame();
            Window.Current.Content = rootFrame;
            rootFrame.Navigate(typeof(MainPage), uri.Query);
            Window.Current.Activate();
        }
    }
}
```

En el código subyacente de la página XAML, invalida el ``OnNavigatedTo`` método para usar los parámetros pasados en la página. En este caso, usaremos la latitud y longitud que se pasaron a esta página para mostrar una ubicación en un mapa.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
 {
     if (e.Parameter != null)
     {
         WwwFormUrlDecoder decoder = new WwwFormUrlDecoder(e.Parameter.ToString());

         double lat = Convert.ToDouble(decoder[0].Value);
         double lon = Convert.ToDouble(decoder[1].Value);

         BasicGeoposition pos = new BasicGeoposition();

         pos.Latitude = lat;
         pos.Longitude = lon;

         myMap.Center = new Geopoint(pos);

         myMap.Style = MapStyle.Aerial3D;

     }

     base.OnNavigatedTo(e);
 }
```

## <a name="making-your-desktop-application-a-share-target"></a>Hacer que la aplicación de escritorio sea un destino de recursos compartidos

Puedes hacer la aplicación de escritorio sea un destino de recursos compartidos para que los usuarios puedan compartir con facilidad datos, como imágenes de otras aplicaciones que admiten el uso compartido.

Por ejemplo, los usuarios podrían elegir tu aplicación para compartir imágenes desde Microsoft Edge, la aplicación fotos. Esta es una aplicación de muestra WPF que tiene esa funcionalidad.

![destino de uso compartido](images/desktop-to-uwp/share-target.png).

Consulta el ejemplo completo [aquí](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget)

### <a name="the-design-pattern"></a>El modelo de diseño

Para que la aplicación sea un destino de recursos compartidos, deberás hacer lo siguiente:

:uno: [Agrega una extensión de destino de recursos compartidos](#share-extension)

: dos: [invalidar el controlador de eventos OnShareTargetActivated](#override)

: tres: [Agregar extensiones de escritorio al proyecto de UWP](#desktop-extensions)

: four: [Agregar la extensión del proceso de plena confianza](#full-trust)

: five: [modificar la aplicación de escritorio para obtener el archivo compartido](#modify-desktop)

<a id="share-extension" />

Los pasos siguientes  

### <a name="add-a-share-target-extension"></a>Agregar una extensión de destino de recursos compartidos

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto de empaquetado en la solución y agrega la extensión de destino de recursos compartidos.

```xml
<Extensions>
      <uap:Extension
          Category="windows.shareTarget"
          Executable="ShareTarget.exe"
          EntryPoint="App">
        <uap:ShareTarget>
          <uap:SupportedFileTypes>
            <uap:SupportsAnyFileType />
          </uap:SupportedFileTypes>
          <uap:DataFormat>Bitmap</uap:DataFormat>
        </uap:ShareTarget>
      </uap:Extension>
</Extensions>  
```

Proporciona el nombre del archivo ejecutable generado por el proyecto de UWP y el nombre de la clase del punto de entrada. Este marcado, se da por hecho que el nombre del archivo ejecutable de la aplicación para UWP es `ShareTarget.exe`.

También tendrás que especificar qué tipos de archivos se pueden compartir con la aplicación. En este ejemplo, vamos a poner la aplicación de escritorio de [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) destino de contenido compartido para las imágenes de mapa de bits por lo que especificamos `Bitmap` para el tipo de archivo admitido.

<a id="override" />

### <a name="override-the-onsharetargetactivated-event-handler"></a>Invalidar el controlador de eventos OnShareTargetActivated

Invalidar el controlador de eventos **OnShareTargetActivated** en la clase de la **aplicación** del proyecto UWP.

A este controlador de eventos se llama cuando los usuarios eligen tu aplicación para compartir sus archivos.

```csharp

protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    shareWithDesktopApplication(args.ShareOperation);
}

private async void shareWithDesktopApplication(ShareOperation shareOperation)
{
    if (shareOperation.Data.Contains(StandardDataFormats.StorageItems))
    {
        var items = await shareOperation.Data.GetStorageItemsAsync();
        StorageFile file = items[0] as StorageFile;
        IRandomAccessStreamWithContentType stream = await file.OpenReadAsync();

        await file.CopyAsync(ApplicationData.Current.LocalFolder);
            shareOperation.ReportCompleted();

        await FullTrustProcessLauncher.LaunchFullTrustProcessForCurrentAppAsync();
    }
}
```

En este código, se guarda la imagen que se comparte por el usuario en una carpeta de almacenamiento local de aplicaciones. Más adelante, modificaremos la aplicación de escritorio a las imágenes de extracción desde esa misma carpeta. La aplicación de escritorio puede hacerlo porque se incluye en el mismo paquete de la aplicación para UWP.

<a id="desktop-extensions" />

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Agregar extensiones de escritorio al proyecto UWP

Agregar la extensión de **Extensiones de escritorio de Windows para UWP** al proyecto de aplicación para UWP.

![extensión de escritorio](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust" />

### <a name="add-the-full-trust-process-extension"></a>Agrega la extensión del proceso de plena confianza

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto de empaquetado en la solución y, a continuación, agrega la extensión de proceso de plena confianza junto a la extensión de destino de recurso compartido de agregar este archivo anteriormente.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Esta extensión habilitará la aplicación para UWP iniciar la aplicación de escritorio a la que quieres que el recurso compartido de un archivo. En el ejemplo, hacemos referencia al archivo ejecutable de la aplicación de escritorio de [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) .

<a id="modify-desktop" />

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Modificar la aplicación de escritorio para obtener el archivo compartido

Modificar la aplicación de escritorio para buscar y procesar el archivo compartido. En este ejemplo, la aplicación para UWP almacena el archivo compartido en la carpeta de datos locales de la aplicación. Por lo tanto, se podría modificar la aplicación de escritorio de [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) a fotos de extracción de esa carpeta.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

Para las instancias de la aplicación de escritorio que ya está abierta por el usuario, te podríamos controlar el evento [FileSystemWatcher](https://docs.microsoft.com/dotnet/api/system.io.filesystemwatcher?view=netframework-4.7.2) y pasa la ruta de acceso a la ubicación del archivo. De este modo, todas las instancias de la aplicación de escritorio abiertas mostrará la foto compartida.

```csharp
...

   FileSystemWatcher watcher = new FileSystemWatcher(Photos.Path);

...

private void Watcher_Created(object sender, FileSystemEventArgs e)
{
    // new file got created, adding it to the list
    Dispatcher.BeginInvoke(System.Windows.Threading.DispatcherPriority.Normal, new Action(() =>
    {
        if (File.Exists(e.FullPath))
        {
            ImageFile item = new ImageFile(e.FullPath);
            Photos.Insert(0, item);
            PhotoListBox.SelectedIndex = 0;
            CurrentPhoto.Source = (BitmapSource)item.Image;
        }
    }));
}

```

## <a name="create-a-background-task"></a>Crear una tarea en segundo plano

Se añade una tarea en segundo plano para ejecutar código de aplicación incluso cuando la aplicación está suspendida. Las tareas en segundo plano son geniales para tareas pequeñas que no requieren la interacción del usuario. Por ejemplo, la tarea puede descargar correo, mostrar una notificación del sistema sobre un mensaje de chat entrante o reaccionar a un cambio en una condición del sistema.

Esta es una aplicación de muestra WPF que registra una tarea en segundo plano.

![tarea en segundo plano](images/desktop-to-uwp/sample-background-task.png)

La tarea realiza una solicitud http y mide el tiempo que tarda la solicitud en devolver una respuesta. Tus tareas probablemente serán mucho más interesantes, pero este ejemplo es ideal para conocer la mecánica básica de una tarea en segundo plano.

Consulta el ejemplo completo [aquí](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

### <a name="the-design-pattern"></a>El modelo de diseño

Para crear un servicio en segundo plano, deberás hacer lo siguiente:

:uno: [Implementar la tarea en segundo plano](#implement-task)

:dos: [Configurar la tarea en segundo plano](#configure-background-task)

:tres: [Registrar la tarea en segundo plano](#register-background-task)

<a id="implement-task" />

### <a name="implement-the-background-task"></a>Implementar la tarea en segundo plano

Implementa la tarea en segundo plano agregando el código a un proyecto de componente de Windows Runtime.

```csharp
public sealed class SiteVerifier : IBackgroundTask
{
    public async void Run(IBackgroundTaskInstance taskInstance)
    {

        taskInstance.Canceled += TaskInstance_Canceled;
        BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
        var msg = await MeasureRequestTime();
        ShowToast(msg);
        deferral.Complete();
    }

    private async Task<string> MeasureRequestTime()
    {
        string msg;
        try
        {
            var url = ApplicationData.Current.LocalSettings.Values["UrlToVerify"] as string;
            var http = new HttpClient();
            Stopwatch clock = Stopwatch.StartNew();
            var response = await http.GetAsync(new Uri(url));
            response.EnsureSuccessStatusCode();
            var elapsed = clock.ElapsedMilliseconds;
            clock.Stop();
            msg = $"{url} took {elapsed.ToString()} ms";
        }
        catch (Exception ex)
        {
            msg = ex.Message;
        }
        return msg;
    }
```

<a id="configure-background-task" />

### <a name="configure-the-background-task"></a>Configurar la tarea en segundo plano

En el Diseñador de manifiestos, abre el archivo **package.appxmanifest** del proyecto de empaquetado en la solución.

En la pestaña **Declaraciones**, agrega una declaración de **Tareas en segundo plano**.

![Opción de tarea en segundo plano](images/desktop-to-uwp/background-task-option.png)

A continuación, elige las propiedades deseadas. Nuestro ejemplo usa la propiedad **Timer**.

![Propiedad Timer](images/desktop-to-uwp/timer-property.png)

Proporciona el nombre completo de la clase en el componente de Windows Runtime que implementa la tarea en segundo plano.

![Propiedad Timer](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task" />

### <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

Agrega código al proyecto de aplicación de escritorio que registre la tarea en segundo plano.

```csharp
public void RegisterBackgroundTask(String triggerName)
{
    var current = BackgroundTaskRegistration.AllTasks
        .Where(b => b.Value.Name == triggerName).FirstOrDefault().Value;

    if (current is null)
    {
        BackgroundTaskBuilder builder = new BackgroundTaskBuilder();
        builder.Name = triggerName;
        builder.SetTrigger(new MaintenanceTrigger(15, false));
        builder.TaskEntryPoint = "HttpPing.SiteVerifier";
        builder.Register();
        System.Diagnostics.Debug.WriteLine("BGTask registered:" + triggerName);
    }
    else
    {
        System.Diagnostics.Debug.WriteLine("Task already:" + triggerName);
    }
}
```

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
