---
title: Inicio automático con Reproducción automática
description: Puedes usar Reproducción automática para ofrecer tu aplicación como una opción cuando un usuario conecte un dispositivo a su PC. Esto incluye dispositivos que no son de volumen, como una cámara o un reproductor multimedia, o dispositivos de volumen, como una unidad USB, una tarjeta SD o un DVD.
ms.assetid: AD4439EA-00B0-4543-887F-2C1D47408EA7
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1a3e2eae72da7eb45104a023ce0fb75a4e4451f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173019"
---
# <a name="span-iddev_launch_resumeauto-launching_with_autoplayspanauto-launching-with-autoplay"></a><span id="dev_launch_resume.auto-launching_with_autoplay"></span>Inicio automático con Reproducción automática

Puedes usar **Reproducción automática** para ofrecer tu aplicación como una opción cuando un usuario conecte un dispositivo a su PC. Esto incluye dispositivos que no son de volumen, como una cámara o un reproductor multimedia, o dispositivos de volumen, como una unidad USB, una tarjeta SD o un DVD. También puedes usar **Reproducción automática** para ofrecer tu aplicación como una opción cuando los usuarios compartan archivos entre dos equipos mediante proximidad (pulsación).

> **Nota:**    Si es un fabricante de dispositivos y desea asociar la [aplicación de dispositivo Microsoft Store](/windows-hardware/drivers/devapps/) como controlador de **reproducción automática** para el dispositivo, puede identificar esa aplicación en los metadatos del dispositivo. Para obtener más información, consulte [reproducción automática para aplicaciones de dispositivos Microsoft Store](/windows-hardware/drivers/devapps/autoplay-for-uwp-device-apps).

## <a name="register-for-autoplay-content"></a>Registrar una aplicación para el contenido de Reproducción automática

Puedes registrar aplicaciones como opciones para eventos de contenido de **Reproducción automática**. Los eventos de contenido de **Reproducción automática** se generan cuando se inserta en el equipo un dispositivo de volumen, como una tarjeta de memoria de cámara, una unidad USB o un DVD. Aquí te mostramos cómo identificar tu aplicación como una opción de **Reproducción automática** cuando se inserta un dispositivo de volumen de una cámara.

En este tutorial, creaste una aplicación que muestra archivos de imagen o los copia a Imágenes. Registraste la aplicación para el evento de contenido **ShowPicturesOnArrival** de Reproducción automática.

Reproducción automática también genera eventos de contenido para contenido compartido entre equipos mediante proximidad (pulsación). Puedes usar los pasos y el código de esta sección para administrar archivos compartidos entre equipos que usan la proximidad. En la siguiente tabla se enumeran los eventos de contenido de Reproducción automática disponibles para compartir contenido al usar la proximidad.

| Acción         | Evento de contenido de Reproducción automática  |
|----------------|-------------------------|
| Uso compartido de música  | PlayMusicFilesOnArrival |
| Uso compartido de vídeos | PlayVideoFilesOnArrival |

 
Cuando se comparten archivos mediante proximidad, la propiedad **Files** del objeto **FileActivatedEventArgs** contiene una referencia a una carpeta raíz que incluye todos los archivos compartidos.

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Paso 1: Crear un nuevo proyecto y agregar declaraciones de Reproducción automática

1.  Abre Microsoft Visual Studio y selecciona **Nuevo proyecto** en el menú **Archivo**. En la sección **Visual C#** , en **Windows**, seleccione **aplicación vacía (Windows universal)**. Asigna un nombre a la aplicación **AutoPlayDisplayOrCopyImages** y haz clic en **Aceptar**.
2.  Abra el archivo package. appxmanifest y seleccione la pestaña **capacidades** . Seleccione las capacidades de la **biblioteca de imágenes** y **almacenamiento extraíble** . Esta acción otorga a la aplicación acceso a dispositivos de almacenamiento extraíbles para la memoria de la cámara y acceso a las imágenes locales.
3.  En el archivo de manifiesto, seleccione la pestaña **declaraciones** . En la lista desplegable **declaraciones disponibles** , seleccione contenido de **reproducción automática** y haga clic en **Agregar**. Selecciona el elemento **Contenido de Reproducción automática** que se agregó a la lista **Declaraciones admitidas**.
4.  Una declaración de **Contenido de Reproducción automática** identifica tu aplicación como opción cuando Reproducción automática genera un evento de contenido. El evento se basa en el contenido de un dispositivo de volumen como un DVD o una unidad USB. Reproducción automática examina el contenido del dispositivo de volumen y determina qué evento de contenido generará. Si la raíz del volumen contiene una carpeta DCIM, AVCHD o PRIVAte \\ ACHD, o si un usuario ha habilitado **elegir qué hacer con cada tipo de medio** en el panel de control de reproducción automática, y las imágenes se encuentran en la raíz del volumen, la reproducción automática genera el evento **ShowPicturesOnArrival** . En la sección **Acciones de inicio**, especifica los valores de la Tabla 1 a continuación para la primera acción de inicio.
5.  En la sección **Acciones de inicio** del elemento **Contenido de reproducción automática**, haz clic en **Agregar nuevo** para agregar una segunda acción de inicio. Escribe los valores de la Tabla 2 a continuación para la segunda acción de inicio.
6.  En la lista desplegable **Declaraciones disponibles**, selecciona **Asociaciones de tipo de archivo** y haz clic en **Agregar**. En las propiedades de la nueva declaración de **asociaciones de tipo de archivo** , establezca el campo nombre para **Mostrar** en **copia de reproducción automática o mostrar imágenes** y el campo **nombre** en **Image \_ association1**. En la sección **Tipos de archivo admitidos**, haz clic en **Agregar nuevo**. Establece el campo **Tipo de archivo** en **.jpg**. En la sección **Tipos de archivo admitidos**, establece el campo **Tipo de archivo** de la nueva asociación de archivo en **.png**. Para los eventos de contenido, Reproducción automática filtra todos los tipos de archivo que no están asociados explícitamente a la aplicación.
7.  Guarde y cierre el archivo de manifiesto.

**Tabla 1**

| Configuración             | Value                 |
|---------------------|-----------------------|
| Verbo                | show                  |
| Nombre para mostrar de la acción | Mostrar imágenes         |
| Evento de contenido       | ShowPicturesOnArrival |

La configuración **Nombre para mostrar de la acción** identifica la cadena que Reproducción automática muestra para tu aplicación. La configuración **Verbo** identifica un valor que se pasa a la aplicación para la opción seleccionada. Puede especificar varias acciones de inicio para un evento de reproducción automática y usar el valor de **verbo** para determinar qué opción ha seleccionado un usuario para la aplicación. Puede indicar qué opción seleccionó el usuario mediante la comprobación de la propiedad **Verb** de los argumentos de evento de inicio pasados a la aplicación. Puede usar cualquier valor para el valor **Verb** , excepto, **Open**, que está reservado.

**Tabla 2**  

| Configuración             | Value                      |
|--------------------:|----------------------------|
| Verbo                | copy                       |
| Nombre para mostrar de la acción | Copiar imágenes en la biblioteca |
| Evento de contenido       | ShowPicturesOnArrival      |

### <a name="step-2-add-xaml-ui"></a>Paso 2: Agregar la interfaz de usuario XAML

Abre el archivo MainPage.xaml y agrega el siguiente XAML a la sección &lt;Grid&gt; predeterminada.

```xml
<TextBlock FontSize="18">File List</TextBlock>
<TextBlock x:Name="FilesBlock" HorizontalAlignment="Left" TextWrapping="Wrap"
           VerticalAlignment="Top" Margin="0,20,0,0" Height="280" Width="240" />
<Canvas x:Name="FilesCanvas" HorizontalAlignment="Left" VerticalAlignment="Top"
        Margin="260,20,0,0" Height="280" Width="100"/>
```

### <a name="step-3-add-initialization-code"></a>Paso 3: Agregar código de inicialización

El código de este paso comprueba el valor de verbo en la propiedad **Verb**, que es uno de los argumentos de inicio que se transmiten a la aplicación durante el evento **OnFileActivated**. Después, el código llama a un método relacionado con la opción seleccionada por el usuario. Para el evento de memoria de la cámara, Reproducción automática transmite la carpeta raíz del almacenamiento de la cámara a la aplicación. Puedes recuperar esta carpeta del primer elemento de la propiedad **Files**.

Abre el archivo App.xaml.cs y agrega el siguiente código a la clase **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    if (args.Verb == "show")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call DisplayImages with root folder from camera storage.
        page.DisplayImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    if (args.Verb == "copy")
    {
        Frame rootFrame = (Frame)Window.Current.Content;
        MainPage page = (MainPage)rootFrame.Content;

        // Call CopyImages with root folder from camera storage.
        page.CopyImages((Windows.Storage.StorageFolder)args.Files[0]);
    }

    base.OnFileActivated(args);
}
```

> **Nota:**    Los `DisplayImages` `CopyImages` métodos y se agregan en los pasos siguientes.

### <a name="step-4-add-code-to-display-images"></a>Paso 4: Agregar código para mostrar imágenes

En el archivo MainPage.xaml.cs, agregue el código siguiente a la clase **mainpage** .

```cs
async internal void DisplayImages(Windows.Storage.StorageFolder rootFolder)
{
    // Display images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();
    for (int i = 0; i < fileList.Count; i++)
    {
        var file = (Windows.Storage.StorageFile)fileList[i];
        WriteMessageText(file.Name + "\n");
        DisplayImage(file, i);
    }
}

async private void DisplayImage(Windows.Storage.IStorageItem file, int index)
{
    try
    {
        var sFile = (Windows.Storage.StorageFile)file;
        Windows.Storage.Streams.IRandomAccessStream imageStream =
            await sFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
        Windows.UI.Xaml.Media.Imaging.BitmapImage imageBitmap =
            new Windows.UI.Xaml.Media.Imaging.BitmapImage();
        imageBitmap.SetSource(imageStream);
        var element = new Image();
        element.Source = imageBitmap;
        element.Height = 100;
        Thickness margin = new Thickness();
        margin.Top = index * 100;
        element.Margin = margin;
        FilesCanvas.Children.Add(element);
    }
    catch (Exception e)
    {
       WriteMessageText(e.Message + "\n");
    }
}

// Write a message to MessageBlock on the UI thread.
private Windows.UI.Core.CoreDispatcher messageDispatcher = Window.Current.CoreWindow.Dispatcher;

private async void WriteMessageText(string message, bool overwrite = false)
{
    await messageDispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            if (overwrite)
                FilesBlock.Text = message;
            else
                FilesBlock.Text += message;
        });
}
```

### <a name="step-5-add-code-to-copy-images"></a>Paso 5: Agregar código para copiar imágenes

En el archivo MainPage.xaml.cs, agregue el código siguiente a la clase **mainpage** .

```cs
async internal void CopyImages(Windows.Storage.StorageFolder rootFolder)
{
    // Copy images from first folder in root\DCIM.
    var dcimFolder = await rootFolder.GetFolderAsync("DCIM");
    var folderList = await dcimFolder.GetFoldersAsync();
    var cameraFolder = folderList[0];
    var fileList = await cameraFolder.GetFilesAsync();

    try
    {
        var folderName = "Images " + DateTime.Now.ToString("yyyy-MM-dd HHmmss");
        Windows.Storage.StorageFolder imageFolder = await
            Windows.Storage.KnownFolders.PicturesLibrary.CreateFolderAsync(folderName);

        foreach (Windows.Storage.IStorageItem file in fileList)
        {
            CopyImage(file, imageFolder);
        }
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy images.\n" + e.Message + "\n");
    }
}

async internal void CopyImage(Windows.Storage.IStorageItem file,
                              Windows.Storage.StorageFolder imageFolder)
{
    try
    {
        Windows.Storage.StorageFile sFile = (Windows.Storage.StorageFile)file;
        await sFile.CopyAsync(imageFolder, sFile.Name);
        WriteMessageText(sFile.Name + " copied.\n");
    }
    catch (Exception e)
    {
        WriteMessageText("Failed to copy file.\n" + e.Message + "\n");
    }
}
```

### <a name="step-6-build-and-run-the-app"></a>Paso 6: Compila y ejecuta la aplicación

1.  Presiona F5 para compilar e implementar la aplicación (en modo de depuración).
2.  Para ejecutar la aplicación, inserta una tarjeta de memoria de una cámara u otro dispositivo de almacenamiento de una cámara en el equipo. A continuación, selecciona una de las opciones de evento de contenido que especificaste en el archivo package.appxmanifest de la lista de opciones de Reproducción automática. Este código de muestra solo muestra o copia imágenes de la carpeta DCIM de una tarjeta de memoria de una cámara. Si la tarjeta de memoria de la cámara almacena imágenes en una carpeta de ACHD de carga AVCHD o privada \\ , deberá actualizar el código según corresponda.
    **Nota:**    Si no tiene una tarjeta de memoria de cámara, puede usar una unidad flash si tiene una carpeta llamada **DCIM** en la raíz y si la carpeta DCIM tiene una subcarpeta que contiene imágenes.

## <a name="register-for-an-autoplay-device"></a>Registro para el dispositivo de reproducción automática


Puedes registrar aplicaciones como opciones para eventos de dispositivo de **Reproducción automática**. Los eventos de dispositivo de **Reproducción automática** se generan cuando se conecta un dispositivo a un equipo.

Aquí te enseñamos a identificar tu aplicación como una opción de **Reproducción automática** cuando una cámara se conecta a un equipo. La aplicación se registra como un controlador para el **evento \\ ImageSourceAutoPlay de WPD** . Esto es un evento común que genera el sistema de dispositivos portátiles de Windows (WPD) cuando las cámaras y otros dispositivos de imagen notifican que son un ImageSource con MTP. Para obtener más información, consulta [Dispositivos portátiles de Windows](/previous-versions/ff597729(v=vs.85)).

**Importante**    Las API de [**Windows. Devices. portable. StorageDevice**](/uwp/api/Windows.Devices.Portable.StorageDevice) forman parte de la [familia de dispositivos de escritorio](../get-started/universal-application-platform-guide.md). Las aplicaciones pueden usar estas API solamente en los dispositivos de Windows 10 de la familia de dispositivos de escritorio como, por ejemplo, los equipos.

 

### <a name="step-1-create-a-new-project-and-add-autoplay-declarations"></a>Paso 1: Crear un nuevo proyecto y agregar declaraciones de Reproducción automática

1.  Abre Visual Studio y selecciona **Nuevo proyecto** en el menú **Archivo**. En la sección **Visual C#** , en **Windows**, seleccione **aplicación vacía (Windows universal)**. Asigne a la aplicación el nombre ** \_ AutoPlayDevice** y haga clic en **Aceptar.**
2.  Abra el archivo package. appxmanifest y seleccione la pestaña **capacidades** . Seleccione la capacidad de **almacenamiento extraíble** . Esto proporcionar a la aplicación acceso a los datos de la cámara dispositivo de volumen de almacenamiento extraíble.
3.  En el archivo de manifiesto, seleccione la pestaña **declaraciones** . En la lista desplegable **declaraciones disponibles** , seleccione dispositivo de **reproducción automática** y haga clic en **Agregar**. Selecciona el nuevo elemento **Dispositivo de Reproducción automática** que se agregó a la lista **Declaraciones admitidas**.
4.  Una declaración **Dispositivo de Reproducción automática** identifica a la aplicación como opción cuando Reproducción automática genera un evento de dispositivo para eventos conocidos. En la sección **Acciones de inicio**, especifica los valores de la tabla a continuación para la primera acción de inicio.
5.  En la lista desplegable **Declaraciones disponibles**, selecciona **Asociaciones de tipo de archivo** y haz clic en **Agregar**. En las propiedades de la nueva declaración de **asociaciones de tipo de archivo** , establezca el campo nombre para **Mostrar** en **Mostrar imágenes de la cámara** y el campo **nombre** en **cámara \_ association1**. En la sección **Tipos de archivo admitidos**, haz clic en **Agregar nuevo** (si es necesario). Establece el campo **Tipo de archivo** en **.jpg**. En la sección **Tipos de archivo admitidos**, vuelve a hacer clic en **Agregar nuevo**. Establece el campo **Tipo de archivo** de la nueva asociación de archivos en **.png**. Para los eventos de contenido, Reproducción automática filtra todos los tipos de archivo que no están asociados explícitamente a la aplicación.
6.  Guarde y cierre el archivo de manifiesto.

| Configuración             | Value            |
|---------------------|------------------|
| Verbo                | show             |
| Nombre para mostrar de la acción | Mostrar imágenes    |
| Evento de contenido       | \\ImageSource ImageSource |

La configuración **Nombre para mostrar de la acción** identifica la cadena que Reproducción automática muestra para tu aplicación. La configuración **Verbo** identifica un valor que se pasa a la aplicación para la opción seleccionada. Puede especificar varias acciones de inicio para un evento de reproducción automática y usar el valor de **verbo** para determinar qué opción ha seleccionado un usuario para la aplicación. Puede indicar qué opción seleccionó el usuario mediante la comprobación de la propiedad **Verb** de los argumentos de evento de inicio pasados a la aplicación. Puede usar cualquier valor para el valor **Verb** , excepto, **Open**, que está reservado. Para ver un ejemplo del uso de varios verbos en una sola aplicación, consulta [Registrar una aplicación para el contenido de Reproducción automática](#register-for-autoplay-content).

### <a name="step-2-add-assembly-reference-for-the-desktop-extensions"></a>Paso 2: Agregar una referencia de ensamblado para las extensiones de escritorio

Las API necesarias para obtener acceso al almacenamiento en un dispositivo portátil de Windows, [**Windows.Devices.Portable.StorageDevice**](/uwp/api/Windows.Devices.Portable.StorageDevice), forman parte de la [familia de dispositivos de escritorio](../get-started/universal-application-platform-guide.md). Esto significa que se necesita un ensamblado especial para usar las API y esas llamadas solo funcionarán en un dispositivo de la familia de dispositivos de escritorio (por ejemplo, un equipo).

1.  En el **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, entonces, selecciona **Agregar referencia...**
2.  Expande **Windows universal** y haz clic en **Extensiones**.
3.  Después, selecciona **extensiones de escritorio de Windows para la UWP** y haz clic en **Aceptar**.

### <a name="step-3-add-xaml-ui"></a>Paso 3: Agregar la interfaz de usuario XAML

Abre el archivo MainPage.xaml y agrega el siguiente XAML a la sección &lt;Grid&gt; predeterminada.

```xml
<StackPanel Orientation="Vertical" Margin="10,0,-10,0">
    <TextBlock FontSize="24">Device Information</TextBlock>
    <StackPanel Orientation="Horizontal">
        <TextBlock x:Name="DeviceInfoTextBlock" FontSize="18" Height="400" Width="400" VerticalAlignment="Top" />
        <ListView x:Name="ImagesList" HorizontalAlignment="Left" Height="400" VerticalAlignment="Top" Width="400">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Vertical">
                        <Image Source="{Binding Path=Source}" />
                        <TextBlock Text="{Binding Path=Name}" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
            <ListView.ItemsPanel>
                <ItemsPanelTemplate>
                    <WrapGrid Orientation="Horizontal" ItemHeight="100" ItemWidth="120"></WrapGrid>
                </ItemsPanelTemplate>
            </ListView.ItemsPanel>
        </ListView>
    </StackPanel>
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Paso 4: Agregar código de activación

El código de este paso hace referencia a la cámara como [**StorageDevice**](/uwp/api/Windows.Devices.Portable.StorageDevice) al pasar el identificador de información del dispositivo de la cámara al método [**FromId**](/uwp/api/windows.devices.portable.storagedevice.fromid). El identificador de información del dispositivo de la cámara se obtiene difundiendo primero los argumentos del evento como [**DeviceActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.DeviceActivatedEventArgs) y después obteniendo el valor de la propiedad [**DeviceInformationId**](/uwp/api/windows.applicationmodel.activation.deviceactivatedeventargs.deviceinformationid).

Abre el archivo App.xaml.cs y agrega el siguiente código a la clase **App**.

```cs
protected override void OnActivated(IActivatedEventArgs args)
{
   if (args.Kind == ActivationKind.Device)
   {
      Frame rootFrame = null;
      // Ensure that the current page exists and is activated
      if (Window.Current.Content == null)
      {
         rootFrame = new Frame();
         rootFrame.Navigate(typeof(MainPage));
         Window.Current.Content = rootFrame;
      }
      else
      {
         rootFrame = Window.Current.Content as Frame;
      }
      Window.Current.Activate();

      // Make sure the necessary APIs are present on the device
      bool storageDeviceAPIPresent =
      Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.Portable.StorageDevice");

      if (storageDeviceAPIPresent)
      {
         // Reference the current page as type MainPage
         var mPage = rootFrame.Content as MainPage;

         // Cast the activated event args as DeviceActivatedEventArgs and show images
         var deviceArgs = args as DeviceActivatedEventArgs;
         if (deviceArgs != null)
         {
            mPage.ShowImages(Windows.Devices.Portable.StorageDevice.FromId(deviceArgs.DeviceInformationId));
         }
      }
      else
      {
         // Handle case where APIs are not present (when the device is not part of the desktop device family)
      }

   }

   base.OnActivated(args);
}
```

> **Nota:**    El `ShowImages` método se agrega en el paso siguiente.

### <a name="step-5-add-code-to-display-device-information"></a>Paso 5: Agregar código para mostrar información de dispositivo

Puedes obtener información de la cámara en las propiedades de la clase [**StorageDevice**](/uwp/api/Windows.Devices.Portable.StorageDevice). El código de este paso muestra al usuario el nombre del dispositivo y otra información cuando la aplicación se ejecuta. El código llama después a los métodos GetImageList y GetThumbnail, que agregarás en el paso siguiente, para mostrar miniaturas de las imágenes almacenadas en la cámara.

En el archivo MainPage.xaml.cs, agrega el siguiente código a la clase **MainPage**.

```cs
private Windows.Storage.StorageFolder rootFolder;

internal async void ShowImages(Windows.Storage.StorageFolder folder)
{
    DeviceInfoTextBlock.Text = "Display Name = " + folder.DisplayName + "\n";
    DeviceInfoTextBlock.Text += "Display Type =  " + folder.DisplayType + "\n";
    DeviceInfoTextBlock.Text += "FolderRelativeId = " + folder.FolderRelativeId + "\n";

    // Reference first folder of the device as the root
    rootFolder = (await folder.GetFoldersAsync())[0];
    var imageList = await GetImageList(rootFolder);

    foreach (Windows.Storage.StorageFile img in imageList)
    {
        ImagesList.Items.Add(await GetThumbnail(img));
    }
}
```

> **Nota:**    Los `GetImageList` `GetThumbnail` métodos y se agregan en el paso siguiente.

### <a name="step-6-add-code-to-display-images"></a>Paso 6: Agregar código para mostrar imágenes

El código de este paso muestra las miniaturas de las imágenes almacenadas en la cámara. El código realiza llamadas asincrónicas a la cámara para obtener la imagen de miniatura. Sin embargo, la siguiente llamada asincrónica no ocurre hasta que se complete la anterior. Esto garantiza que solo se haga una solicitud a la cámara cada vez.

En el archivo MainPage.xaml.cs, agrega el siguiente código a la clase **MainPage**.

```cs
async private System.Threading.Tasks.Task<List<Windows.Storage.StorageFile>> GetImageList(Windows.Storage.StorageFolder folder)
{
    var result = await folder.GetFilesAsync();
    var subFolders = await folder.GetFoldersAsync();
    foreach (Windows.Storage.StorageFolder f in subFolders)
        result = result.Union(await GetImageList(f)).ToList();

    return (from f in result orderby f.Name select f).ToList();
}

async private System.Threading.Tasks.Task<Image> GetThumbnail(Windows.Storage.StorageFile img)
{
    // Get the thumbnail to display
    var thumbnail = await img.GetThumbnailAsync(Windows.Storage.FileProperties.ThumbnailMode.SingleItem,
                                                100,
                                                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale);

    // Create a XAML Image object bind to on the display page
    var result = new Image();
    result.Height = thumbnail.OriginalHeight;
    result.Width = thumbnail.OriginalWidth;
    result.Name = img.Name;
    var imageBitmap = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
    imageBitmap.SetSource(thumbnail);
    result.Source = imageBitmap;

    return result;
}
```

### <a name="step-7-build-and-run-the-app"></a>Paso 7: Compila y ejecuta la aplicación

1.  Presiona F5 para compilar e implementar la aplicación (en modo de depuración).
2.  Para ejecutar la aplicación, conecta una cámara al equipo. Después, selecciona la aplicación en la lista de opciones de Reproducción automática.
    **Nota:**    No todas las cámaras anuncian el evento de dispositivo de reproducción automática del ** \\ ImageSource de WPD** .

## <a name="configure-removable-storage"></a>Configurar el almacenamiento extraíble

Puedes identificar un dispositivo de volumen, como una tarjeta de memoria o una unidad USB, como dispositivo de **Reproducción automática** cuando el dispositivo de volumen se conecte al equipo. Esto es particularmente útil cuando deseas asociar una aplicación específica para la **Reproducción automática** para el dispositivo de volumen.

Aquí se muestra cómo identificar el dispositivo de volumen como dispositivo de **Reproducción automática**.

Para identificar el dispositivo de volumen como dispositivo de **Reproducción automática**, agrega un archivo autorun.inf a la unidad raíz del dispositivo. En el archivo autorun.inf, agrega una clave **CustomEvent** a la sección **Ejecución automática**. Cuando el dispositivo de volumen se conecte a un equipo, **Reproducción automática** encontrará el archivo autorun.inf y tratará el volumen como un dispositivo. **Reproducción automática** creará un evento de **Reproducción automática** con el nombre que proporcionaste para la clave **CustomEvent**. Después podrás crear una aplicación y registrarla como controlador del evento de **Reproducción automática**. Cuando se conecte el dispositivo al equipo, **Reproducción automática** mostrará la aplicación como controlador del dispositivo de volumen. Para obtener más información acerca de los archivos autorun.inf, consulta [Entradas de autorun.inf](/windows/desktop/shell/autorun-cmds).

### <a name="step-1-create-an-autoruninf-file"></a>Paso 1: Crear un archivo autorun.inf

En la unidad raíz del dispositivo de volumen, agrega un archivo denominado autorun.inf. Abre el archivo autorun.inf y agrega el siguiente texto.

``` syntax
[AutoRun]
CustomEvent=AutoPlayCustomEventQuickstart
```

### <a name="step-2-create-a-new-project-and-add-autoplay-declarations"></a>Paso 2: Crear un nuevo proyecto y agregar declaraciones de Reproducción automática

1.  Abre Visual Studio y selecciona **Nuevo proyecto** en el menú **Archivo**. En la sección **Visual C#** , en **Windows**, seleccione **aplicación vacía (Windows universal)**. Asigna el nombre **AutoPlayCustomEvent** a la aplicación y haz clic en**Aceptar**.
2.  Abra el archivo package. appxmanifest y seleccione la pestaña **capacidades** . Seleccione la capacidad de **almacenamiento extraíble** . Esto permite que la aplicación tenga acceso a los archivos y carpetas de los dispositivos de almacenamiento extraíble.
3.  En el archivo de manifiesto, seleccione la pestaña **declaraciones** . En la lista desplegable **declaraciones disponibles** , seleccione contenido de **reproducción automática** y haga clic en **Agregar**. Selecciona el elemento **Contenido de Reproducción automática** que se agregó a la lista **Declaraciones admitidas**.

    **Nota:**    También puede optar por agregar una declaración de **dispositivo de reproducción automática** para el evento personalizado de reproducción automática.  
4.  En la sección **Acciones de inicio** de tu declaración de evento de **contenido de Reproducción automática**, especifica los valores en la siguiente tabla para la primera acción de inicio.
5.  En la lista desplegable **Declaraciones disponibles**, selecciona **Asociaciones de tipo de archivo** y haz clic en **Agregar**. En las propiedades de la nueva declaración de **asociaciones de tipo de archivo** , establezca el campo nombre para **Mostrar** en **Mostrar archivos. ms** y el campo **nombre** en ** \_ Asociación de MS**. En la sección **Tipos de archivo admitidos**, haz clic en **Agregar nuevo**. Establece el campo **Tipo de archivo** en **.ms**. Para los eventos de contenido, Reproducción automática filtra todos los tipos de archivo que no están asociados explícitamente a la aplicación.
6.  Guarde y cierre el archivo de manifiesto.

| Configuración             | Value                         |
|---------------------|-------------------------------|
| Verbo                | show                          |
| Nombre para mostrar de la acción | Mostrar archivos                    |
| Evento de contenido       | AutoPlayCustomEventQuickstart |

El valor **Evento de contenido** es el texto que suministraste para la clave **CustomEvent** en el archivo autorun.inf. La configuración **Nombre para mostrar de la acción** identifica la cadena que Reproducción automática muestra para tu aplicación. La configuración **Verbo** identifica un valor que se pasa a la aplicación para la opción seleccionada. Puede especificar varias acciones de inicio para un evento de reproducción automática y usar el valor de **verbo** para determinar qué opción ha seleccionado un usuario para la aplicación. Puede indicar qué opción seleccionó el usuario mediante la comprobación de la propiedad **Verb** de los argumentos de evento de inicio pasados a la aplicación. Puede usar cualquier valor para el valor **Verb** , excepto, **Open**, que está reservado.

### <a name="step-3-add-xaml-ui"></a>Paso 3: Agregar la interfaz de usuario XAML

Abre el archivo MainPage.xaml y agrega el siguiente XAML a la sección &lt;Grid&gt; predeterminada.

```xml
<StackPanel Orientation="Vertical">
    <TextBlock FontSize="28" Margin="10,0,800,0">Files</TextBlock>
    <TextBlock x:Name="FilesBlock" FontSize="22" Height="600" Margin="10,0,800,0" />
</StackPanel>
```

### <a name="step-4-add-activation-code"></a>Paso 4: Agregar código de activación

El código de este paso llama a un método para que muestre las carpetas en la unidad raíz del dispositivo de volumen. Para los eventos de contenido de Reproducción automática, Reproducción automática transmite la carpeta raíz del dispositivo de almacenamiento en los argumentos de inicio que se transmiten a la aplicación durante el evento **OnFileActivated**. Puedes recuperar esta carpeta del primer elemento de la propiedad **Files**.

Abre el archivo App.xaml.cs y agrega el siguiente código a la clase **App**.

```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
    var rootFrame = Window.Current.Content as Frame;
    var page = rootFrame.Content as MainPage;

    // Call ShowFolders with root folder from device storage.
    page.DisplayFiles(args.Files[0] as Windows.Storage.StorageFolder);

    base.OnFileActivated(args);
}
```

> **Nota:**    El `DisplayFiles` método se agrega en el paso siguiente.

 

### <a name="step-5-add-code-to-display-folders"></a>Paso 5: Agregar código para mostrar carpetas

En el archivo MainPage.xaml.cs, agregue el código siguiente a la clase **mainpage** .

```cs
internal async void DisplayFiles(Windows.Storage.StorageFolder folder)
{
    foreach (Windows.Storage.StorageFile f in await ReadFiles(folder, ".ms"))
    {
        FilesBlock.Text += "  " + f.Name + "\n";
    }
}

internal async System.Threading.Tasks.Task<IReadOnlyList<Windows.Storage.StorageFile>>
    ReadFiles(Windows.Storage.StorageFolder folder, string fileExtension)
{
    var options = new Windows.Storage.Search.QueryOptions();
    options.FileTypeFilter.Add(fileExtension);
    var query = folder.CreateFileQueryWithOptions(options);
    var files = await query.GetFilesAsync();

    return files;
}
```

### <a name="step-6-build-and-run-the-qpp"></a>Paso 6: Compilar y ejecutar la aplicación

1.  Presiona F5 para compilar e implementar la aplicación (en modo de depuración).
2.  Para ejecutar la aplicación, inserta una tarjeta de memoria u otro dispositivo de almacenamiento en el equipo. Luego, selecciona la aplicación en la lista de opciones de controlador de Reproducción automática.

## <a name="autoplay-event-reference"></a>Referencia del evento de reproducción automática


El sistema **Reproducción automática** permite registrar las aplicaciones para diversos eventos de inserción de volumen (disco) y dispositivo Para registrar la aplicación en eventos de contenido de **Reproducción automática**, debes habilitar la funcionalidad **Almacenamiento extraíble** en el manifiesto del paquete. La tabla muestra los eventos en los que puedes registrar tu aplicación y cuándo se generan.

| Escenario                                                           | Evento                              | Descripción   |
|--------------------------------------------------------------------|------------------------------------|---------------|
| Uso de fotos en una cámara                                           | **WPD\ImageSource**                | Se genera para cámaras identificadas como dispositivos portátiles de Windows y ofrece la funcionalidad ImageSource. |
| Uso de música en un reproductor de audio                                     | **WPD\AudioSource**                | Se genera para reproductores de medios identificados como dispositivos portátiles de Windows y ofrece la funcionalidad AudioSource. |
| Uso de vídeos en una cámara                                     | **WPD\VideoSource**                | Se genera para videocámaras identificadas como dispositivos portátiles de Windows y ofrece la funcionalidad VideoSource. |
| Acceder a una unidad flash o disco duro externo conectado              | **StorageOnArrival**               | Se genera cuando se conecta una unidad o un volumen al equipo.   Si la unidad o el volumen contienen una carpeta DCIM, AVCHD o PRIVATE\ACHD en la raíz del disco, en su lugar, se genera el evento **ShowPicturesOnArrival**. |
| Uso de fotos de dispositivos de almacenamiento masivo (heredados)                            | **ShowPicturesOnArrival**          | Se genera cuando la unidad o el volumen contienen una carpeta DCIM, AVCHD o PRIVATE\ACHD en la raíz del disco. Si un usuario habilitó **Elegir qué hacer con cada tipo de medio** en el Panel de control de Reproducción Automática, esta funcionalidad examinará un volumen conectado al equipo para determinar el tipo de contenido del disco. Cuando se encuentran imágenes, se genera **ShowPicturesOnArrival**. |
| Recepción de fotos con uso compartido de proximidad (tocar y enviar)             | **ShowPicturesOnArrival**          | Cuando los usuarios envíen contenido mediante proximidad (tocar y enviar), Reproducción automática examinará los archivos compartidos para determinar el tipo de contenido. Si se encuentran imágenes, se genera **ShowPicturesOnArrival**. |
| Uso de música de dispositivos de almacenamiento masivo (heredados)                             | **PlayMusicFilesOnArrival**        | Si un usuario habilitó **Elegir qué hacer con cada tipo de medio** en el Panel de control de Reproducción Automática, esta funcionalidad examinará un volumen conectado al equipo para determinar el tipo de contenido del disco.  Cuando se encuentran los archivos de música, se genera **PlayMusicFilesOnArrival**. |
| Recepción de música con uso compartido de proximidad (tocar y enviar)              | **PlayMusicFilesOnArrival**        | Cuando los usuarios envíen contenido mediante proximidad (tocar y enviar), Reproducción automática examinará los archivos compartidos para determinar el tipo de contenido. Si se encuentran los archivos de música, se genera **PlayMusicFilesOnArrival**. |
| Uso de vídeos de dispositivos de almacenamiento (heredados)                            | **PlayVideoFilesOnArrival**        | Si un usuario habilitó **Elegir qué hacer con cada tipo de medio** en el Panel de control de Reproducción Automática, esta funcionalidad examinará un volumen conectado al equipo para determinar el tipo de contenido del disco. Cuando se encuentran los archivos de vídeo, se genera **PlayVideoFilesOnArrival**. |
| Recepción de vídeos con uso compartido de proximidad (tocar y enviar)             | **PlayVideoFilesOnArrival**        | Cuando los usuarios envíen contenido mediante proximidad (tocar y enviar), Reproducción automática examinará los archivos compartidos para determinar el tipo de contenido. Si se encuentran los archivos de vídeo, se genera **PlayVideoFilesOnArrival**. |
| Administración de conjuntos mixtos de archivos de un dispositivo conectado               | **MixedContentOnArrival**          | Si un usuario habilitó **Elegir qué hacer con cada tipo de medio** en el Panel de control de Reproducción Automática, esta funcionalidad examinará un volumen conectado al equipo para determinar el tipo de contenido del disco. Si no se encuentra ningún tipo de contenido específico (por ejemplo, imágenes), se genera **MixedContentOnArrival**. |
| Administración de conjuntos mixtos de archivos con uso compartido de proximidad (tocar y enviar) | **MixedContentOnArrival**          | Cuando los usuarios envíen contenido mediante proximidad (tocar y enviar), Reproducción automática examinará los archivos compartidos para determinar el tipo de contenido. Si no se encuentra ningún tipo de contenido específico (por ejemplo, imágenes), se genera **MixedContentOnArrival**. |
| Administrar vídeos de medios ópticos                                    | **PlayDVDMovieOnArrival**<br />**PlayBluRayOnArrival**<br />**PlayVideoCDMovieOnArrival**<br />**PlaySuperVideoCDMovieOnArrival** | Cuando se inserta un disco en la unidad óptica, la reproducción automática examinará los archivos para determinar el tipo de contenido. Cuando se encuentran archivos de vídeo, se genera el evento correspondiente al tipo de disco óptico. |
| Administrar música de medios ópticos                                    | **PlayCDAudioOnArrival**<br />**PlayDVDAudioOnArrival** | Cuando se inserta un disco en la unidad óptica, la reproducción automática examinará los archivos para determinar el tipo de contenido. Cuando se encuentran archivos de música, se genera el evento correspondiente al tipo de disco óptico. |
| Reproducir discos mejorados                                                | **PlayEnhancedCDOnArrival**<br />**PlayEnhancedDVDOnArrival** | Cuando se inserta un disco en la unidad óptica, la reproducción automática examinará los archivos para determinar el tipo de contenido. Cuando se encuentra un disco mejorado, se genera el evento correspondiente al tipo de disco óptico. |
| Administrar discos ópticos grabables                                     | **HandleCDBurningOnArrival** <br />**HandleDVDBurningOnArrival** <br />**HandleBDBurningOnArrival** | Cuando se inserta un disco en la unidad óptica, la reproducción automática examinará los archivos para determinar el tipo de contenido. Cuando se encuentra un disco grabable, se genera el evento correspondiente al tipo de disco óptico. |
| Administrar cualquier otra conexión de volúmenes o dispositivos                       | **UnknownContentOnArrival**        | Se genera para todos los eventos si se encuentra contenido que no coincida con ninguno de los eventos de contenido de Reproducción automática. No se recomienda el uso de este evento. Solo debes registrar tu aplicación para los eventos de Reproducción automática específicos que esta pueda administrar. |

Puedes especificar que Reproducción automática genere un evento de contenido personalizado con la entrada **CustomEvent** en el archivo autorun.inf para un volumen. Para obtener más información, consulta [Entradas en autorun.inf](/windows/desktop/shell/autorun-cmds).

Puedes agregar una extensión al archivo package.appxmanifest de tu aplicación para registrarla como controlador de eventos de Contenido de Reproducción automática o Dispositivo de Reproducción automática. Si usa Visual Studio, puede Agregar un **contenido de reproducción automática** o una declaración de **dispositivo de reproducción automática** en la pestaña **declaraciones** . Si va a editar el archivo package. appxmanifest para su aplicación directamente, agregue un elemento de [**extensión**](/uwp/schemas/appxpackage/appxmanifestschema/element-1-extension) al manifiesto del paquete que especifique **Windows. autoPlayContent** o **Windows. autoPlayDevice** como **categoría**. Por ejemplo, la siguiente entrada del manifiesto del paquete agrega una extensión de **Contenido de Reproducción automática** para registrar la aplicación como controlador del evento **ShowPicturesOnArrival**.

```xml
  <Applications>
    <Application Id="AutoPlayHandlerSample.App">
      <Extensions>
        <Extension Category="windows.autoPlayContent">
          <AutoPlayContent>
            <LaunchAction Verb="show" ActionDisplayName="Show Pictures"
                          ContentEvent="ShowPicturesOnArrival" />
          </AutoPlayContent>
        </Extension>
      </Extensions>
    </Application>
  </Applications>
```

 

 