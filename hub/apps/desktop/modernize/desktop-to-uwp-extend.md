---
title: Ampliación de la aplicación con componentes e interfaces de usuario de Windows
description: Amplíe su aplicación de escritorio con los proyectos de UWP y componentes de Windows Runtime para agregar experiencias modernas de Windows 10.
ms.date: 06/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: d0a1d1b7685ae76c26c94fa104b6c0ff6334364d
ms.sourcegitcommit: 609441402c17d92e7bfac83a6056909bb235223c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2020
ms.locfileid: "90837830"
---
# <a name="extend-your-desktop-app-with-modern-uwp-components"></a>Ampliación de la aplicación de escritorio con componentes de UWP modernos

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. Si quieres agregar estas experiencias, amplía tu aplicación de escritorio con los componentes de proyectos de UWP y Windows Runtime.

En muchos casos, puedes llamar a las API de Windows Runtime directamente desde la aplicación de escritorio, por lo que, antes de revisar esta guía, consulta [Mejora para Windows 10](desktop-to-uwp-enhance.md).

> [!NOTE]
> En las características que se describen en este artículo se requiere que la aplicación de escritorio tenga [identidad de paquete](modernize-packaged-apps.md), bien mediante el [empaquetado de la aplicación de escritorio en un paquete MSIX](/windows/msix/desktop/desktop-to-uwp-root) o mediante la [concesión de la identidad de la aplicación con un paquete disperso](grant-identity-to-nonpackaged-apps.md).

Si estás listo, comencemos.

<a id="setup"></a>

## <a name="first-setup-your-solution"></a>En primer lugar, configurar la solución

Agrega uno o varios proyectos de UWP y componentes de Runtime a la solución.

Comienza con una solución que contenga un **Proyecto de paquete de aplicación de Windows** con una referencia a la aplicación de escritorio.

Esta imagen muestra un ejemplo de solución.

![Ampliar el proyecto de inicio](images/desktop-to-uwp/extend-start-project.png)

Si la solución no contiene un proyecto de empaquetado, consulta [Empaquetado de la aplicación de escritorio mediante Visual Studio](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net).

### <a name="configure-the-desktop-application"></a>Configuración de la aplicación de escritorio

Asegúrate de que la aplicación de escritorio tiene referencias a los archivos que necesitas para llamar a las API de Windows Runtime.

Para ello, consulte [Llamada a las API de Windows Runtime en aplicaciones de escritorio](desktop-to-uwp-enhance.md).

### <a name="add-a-uwp-project"></a>Agregar un proyecto de UWP

Agrega una **Aplicación vacía (Windows universal)** a la solución.

Aquí es donde debes crear una interfaz de usuario XAML moderna o usar las API que se ejecutan solo dentro de un proceso de UWP.

![Incorporación de proyecto nuevo](images/desktop-to-uwp/add-uwp-project-to-solution.png)

En tu proyecto de empaquetado, haz clic con el botón derecho en el nodo **Aplicaciones** y, a continuación, elige **Agregar referencia**.

![Agregar referencia](images/desktop-to-uwp/add-uwp-project-reference.png)

A continuación, agrega una referencia al proyecto de UWP.

![Seleccionar proyecto de UWP](images/desktop-to-uwp/choose-uwp-project.png)

Tu solución debe tener un aspecto similar a este:

![Solución con proyecto de UWP](images/desktop-to-uwp/uwp-project-reference.png)

### <a name="optional-add-a-windows-runtime-component"></a>(Opcional) Agregar un componente de Windows Runtime

Para realizar algunos escenarios, tendrás que agregar código a un componente de Windows Runtime.

![servicio de aplicación de componente de Runtime](images/desktop-to-uwp/add-runtime-component.png)

A continuación, desde el proyecto de UWP, agrega una referencia al componente de Runtime. Tu solución debe tener un aspecto similar a este:

![Referencia de componente de Runtime](images/desktop-to-uwp/runtime-component-reference.png)

### <a name="build-your-solution"></a>Compilar la solución

Compila la solución para asegurarte de que no aparezcan errores. Si aparecen errores, abre **Configuration Manager** y asegúrate de que los proyectos tengan como destino la misma plataforma.

![Configuration Manager](images/desktop-to-uwp/config-manager.png)

Echemos un vistazo a algunas cosas que puedes hacer con los proyectos de UWP y los componentes de Runtime.

## <a name="show-a-modern-xaml-ui"></a>Mostrar una interfaz de usuario XAML moderna

Como parte del flujo de la aplicación, puedes incorporar interfaces de usuario basadas en XAML modernas en la aplicación de escritorio. Estas interfaces de usuario pueden ser adaptables naturalmente para diferentes tamaños de pantalla y resoluciones, y admitir modelos interactivos modernos, como la entrada táctil y manuscrita.

Por ejemplo, con una pequeña cantidad de marcado XAML, puedes ofrecer a los usuarios potentes características de visualización relacionadas con mapas.

Esta imagen muestra una aplicación de Windows Forms que abre una interfaz de usuario moderna basada en XAML que contiene un control de mapa.

![diseño adaptativo](images/desktop-to-uwp/extend-xaml-ui.png)

>[!NOTE]
>En este ejemplo se muestra una interfaz de usuario XAML mediante la adición de un proyecto de UWP a la solución. Este es el enfoque compatible estable para mostrar las interfaces de usuario XAML en una aplicación de escritorio. La alternativa a este enfoque consiste en agregar controles XAML de UWP directamente a la aplicación de escritorio mediante una isla XAML. Las islas XAML están disponibles actualmente como versión preliminar para desarrolladores. Aunque te animamos a probarlas ahora en tu propio código de prototipo, no recomendamos usarlos en el código de producción en este momento. Estas API y controles seguirán evolucionando y se estabilizarán en futuras versiones de Windows. Para obtener más información sobre las islas XAML, consulta [Controles de UWP en aplicaciones de escritorio](xaml-islands.md).

### <a name="the-design-pattern"></a>El modelo de diseño

Para mostrar una interfaz de usuario basada en XAML, deberás hacer lo siguiente:

:one: [Configurar la solución](#solution-setup)

:two: [Crear una interfaz de usuario XAML](#xaml-UI)

:three: [Agregar una extensión de protocolo al proyecto de UWP](#add-a-protocol-extension)

:four: [Iniciar la aplicación para UWP desde la aplicación de escritorio](#start)

:five: [En el proyecto de UWP, mostrar la página deseada](#parse)

<a id="solution-setup"></a>

### <a name="setup-your-solution"></a>Configurar la solución

Para obtener instrucciones generales sobre cómo configurar la solución, consulta la sección [En primer lugar, configurar la solución](#setup) al principio de esta guía.

La solución tendrá un aspecto similar a este:

![Solución de interfaz de usuario XAML](images/desktop-to-uwp/xaml-ui-solution.png)

En este ejemplo, el proyecto de Windows Forms se denomina **Landmarks** y el proyecto de UWP que contiene la interfaz de usuario XAML, **MapUI**.

<a id="xaml-UI"></a>

### <a name="create-a-xaml-ui"></a>Crear una interfaz de usuario XAML

Agrega una interfaz de usuario XAML al proyecto de UWP. Este es el XAML para un mapa básico.

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

También puedes abrir el archivo **package.appxmanifest** en el diseñador, elegir la pestaña **Declaraciones** y, a continuación, agregar la extensión allí.

![Pestaña Declaraciones](images/desktop-to-uwp/protocol-properties.png)

> [!NOTE]
> Los controles de mapa descargan datos de Internet, por lo que si usas uno, tendrás que agregar la funcionalidad de "cliente de Internet" al manifiesto también.

<a id="start"></a>

### <a name="start-the-uwp-app"></a>Iniciar la aplicación para UWP

En primer lugar, desde la aplicación de escritorio, crea un [Uri](/dotnet/api/system.uri) que incluya el nombre del protocolo y los parámetros que quieras pasar a la aplicación para UWP. Luego llama al método [LaunchUriAsync](/uwp/api/windows.system.launcher.launchuriasync).

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

<a id="parse"></a>

### <a name="parse-parameters-and-show-a-page"></a>Analizar parámetros y mostrar una página

En la clase **Aplicación** del proyecto de UWP, invalida el controlador de eventos **OnActivated**. Si el protocolo activa la aplicación, analiza los parámetros y luego abre la página que quieras.

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

En el código subyacente de la página XAML, invalida el método ``OnNavigatedTo`` para usar los parámetros pasados a la página. En este caso, usaremos la latitud y la longitud que se pasaron a esta página para mostrar una ubicación en un mapa.

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

## <a name="making-your-desktop-application-a-share-target"></a>Convertir la aplicación de escritorio en un destino de recursos compartidos

Puedes convertir la aplicación de escritorio en un destino de recursos compartidos para que los usuarios puedan compartir con facilidad datos, como imágenes de otras aplicaciones que admiten el uso compartido.

Por ejemplo, los usuarios podrían elegir tu aplicación para compartir imágenes desde Microsoft Edge, la aplicación Fotos. Esta es una aplicación de ejemplo WPF que tiene esa funcionalidad.

![Compartir destino](images/desktop-to-uwp/share-target.png).

Consulta el ejemplo completo [aquí](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/ShareTarget).

### <a name="the-design-pattern"></a>El modelo de diseño

Para que la aplicación sea un destino de recursos compartidos, deberás hacer lo siguiente:

:one: [Agregar una extensión de destino de recursos compartidos](#share-extension)

:two: [Invalidar el controlador de eventos OnShareTargetActivated](#override)

:three: [Agregar extensiones de escritorio al proyecto de UWP](#desktop-extensions)

:four: [Agregar la extensión de proceso de plena confianza](#full-trust)

:five: [Modificar la aplicación de escritorio para obtener el archivo compartido](#modify-desktop)

<a id="share-extension"></a>

Los pasos siguientes  

### <a name="add-a-share-target-extension"></a>Agregar una extensión de destino de recursos compartidos

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto de empaquetado en tu solución y agrega la extensión de destino de recursos compartidos.

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

Proporciona el nombre del archivo ejecutable generado por el proyecto de UWP y el nombre de la clase del punto de entrada. Este marcado supone que el nombre del archivo ejecutable de la aplicación para UWP es `ShareTarget.exe`.

También tendrás que especificar qué tipos de archivos se pueden compartir con la aplicación. En este ejemplo, para convertir la aplicación de escritorio [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) en un destino de recursos compartidos para las imágenes de mapa de bits, se especifica `Bitmap` para el tipo de archivo compatible.

<a id="override"></a>

### <a name="override-the-onsharetargetactivated-event-handler"></a>Invalidar el controlador de eventos OnShareTargetActivated

Invalida el controlador de eventos **OnShareTargetActivated** en la clase **App** del proyecto de UWP.

Se llama a este controlador de eventos cuando los usuarios eligen tu aplicación para compartir sus archivos.

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

En este código, se guarda la imagen que el usuario comparte en una carpeta de almacenamiento local de aplicaciones. Más adelante, modificaremos la aplicación de escritorio para extraer imágenes de esa misma carpeta. La aplicación de escritorio puede hacerlo porque está incluida en el mismo paquete que la aplicación para UWP.

<a id="desktop-extensions"></a>

### <a name="add-desktop-extensions-to-the-uwp-project"></a>Agregar extensiones de escritorio al proyecto de UWP

Agrega la extensión **Windows Desktop Extensions for the UWP** al proyecto de la aplicación para UWP.

![Extensión de escritorio](images/desktop-to-uwp/desktop-extensions.png)

<a id="full-trust"></a>

### <a name="add-the-full-trust-process-extension"></a>Agregar la extensión de proceso de plena confianza

En el **Explorador de soluciones**, abre el archivo **package.appxmanifest** del proyecto de empaquetado en la solución y, a continuación, agrega la extensión de proceso de plena confianza junto a la extensión de destino de recursos compartidos que agregaste anteriormente a este archivo.

```xml
<Extensions>
  ...
      <desktop:Extension Category="windows.fullTrustProcess" Executable="PhotoStoreDemo\PhotoStoreDemo.exe" />
  ...
</Extensions>  
```

Esta extensión permitirá a la aplicación para UWP iniciar la aplicación de escritorio en la que quieres compartir un archivo. En el ejemplo, hacemos referencia al archivo ejecutable de la aplicación de escritorio [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo).

<a id="modify-desktop"></a>

### <a name="modify-the-desktop-application-to-get-the-shared-file"></a>Modificar la aplicación de escritorio para obtener el archivo compartido

Modifica la aplicación de escritorio para buscar y procesar el archivo compartido. En este ejemplo, la aplicación para UWP almacenó el archivo compartido en la carpeta de datos de la aplicación local. Por lo tanto, se modificará la aplicación de escritorio [WPF PhotoStoreDemo](https://github.com/Microsoft/WPF-Samples/tree/master/Sample%20Applications/PhotoStoreDemo) para extraer fotos de esa carpeta.

```csharp
Photos.Path = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
```

En el caso de las instancias de la aplicación de escritorio que ya ha abierto el usuario, también se puede controlar el evento [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) y pasar la ruta de acceso a la ubicación del archivo. De este modo, las instancias abiertas de la aplicación de escritorio mostrarán la foto compartida.

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

Se debe añadir una tarea en segundo plano para ejecutar código incluso cuando la aplicación está suspendida. Las tareas en segundo plano son geniales para tareas pequeñas que no requieren la interacción del usuario. Por ejemplo, la tarea puede descargar correo, mostrar una notificación del sistema sobre un mensaje de chat entrante o reaccionar a un cambio en una condición del sistema.

A continuación se incluye un ejemplo de aplicación WPF que registra una tarea en segundo plano.

![Tarea en segundo plano](images/desktop-to-uwp/sample-background-task.png)

La tarea realiza una solicitud HTTP y mide el tiempo que tarda la solicitud en devolver una respuesta. Tus tareas probablemente serán mucho más interesantes, pero este ejemplo es ideal para conocer la mecánica básica de una tarea en segundo plano.

Consulta el ejemplo completo [aquí](https://github.com/Microsoft/Windows-Packaging-Samples/tree/master/BGTask).

### <a name="the-design-pattern"></a>El modelo de diseño

Para crear un servicio en segundo plano, deberás hacer lo siguiente:

:one: [Implementar la tarea en segundo plano](#implement-task)

:two: [Configurar la tarea en segundo plano](#configure-background-task)

:three: [Registrar la tarea en segundo plano](#register-background-task)

<a id="implement-task"></a>

### <a name="implement-the-background-task"></a>Implementar la tarea en segundo plano

Para implementar la tarea en segundo plano, agrega el código a un proyecto de componente de Windows Runtime.

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

<a id="configure-background-task"></a>

### <a name="configure-the-background-task"></a>Configurar la tarea en segundo plano

En el diseñador de manifiesto, abre el archivo **package.appxmanifest** del proyecto de empaquetado en tu solución.

En la pestaña **Declaraciones**, agrega una declaración de **Tareas en segundo plano**.

![Opción de tarea en segundo plano](images/desktop-to-uwp/background-task-option.png)

A continuación, elige las propiedades deseadas. En nuestro ejemplo se usa la propiedad **Timer**.

![Propiedad Timer](images/desktop-to-uwp/timer-property.png)

Proporciona el nombre completo de la clase en el componente de Windows Runtime que implementa la tarea en segundo plano.

![Especificar el punto de entrada](images/desktop-to-uwp/background-task-entry-point.png)

<a id="register-background-task"></a>

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

## <a name="find-answers-to-your-questions"></a>Encontrar respuestas a tus preguntas

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).