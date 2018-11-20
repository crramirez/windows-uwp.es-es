---
author: Karl-Bridge-Microsoft
Description: UWP apps that support Windows Ink can serialize and deserialize ink strokes to an Ink Serialized Format (ISF) file. The ISF file is a GIF image with additional metadata for all ink stroke properties and behaviors. Apps that are not ink-enabled, can view the static GIF image, including alpha-channel background transparency.
title: Almacenar y recuperar datos de trazos de lápiz de Windows Ink
ms.assetid: C96C9D2F-DB69-4883-9809-4A0DF7CEC506
label: Store and retrieve Windows Ink stroke data
template: detail.hbs
keywords: Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, ISF, Ink Serialized Format, interacción del usuario, entrada
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 946e8421cf21c37c929e7a9f9687117c3d7aca92
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7422382"
---
# <a name="store-and-retrieve-windows-ink-stroke-data"></a>Almacenar y recuperar datos de trazos de lápiz de Windows Ink


Las aplicaciones para UWP que admiten Windows Ink pueden serializar y deserializar los trazos de lápiz en un archivo de formato Ink Serialized Format (ISF). El archivo ISF es una imagen GIF con metadatos adicionales para todos los comportamientos y propiedades de trazo de lápiz. Las aplicaciones que no están habilitadas para la entrada de lápiz puede ver la imagen GIF estática, incluida la transparencia del fondo del canal alfa.

> [!NOTE]
> El formato ISF es la representación más compacta y persistente de la entrada de lápiz. Puede incrustarse en un formato de documento binario, como un archivo GIF, o colocarse directamente en el Portapapeles.

> **API importantes**: [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)

## <a name="save-ink-strokes-to-a-file"></a>Guardar los trazos de lápiz en un archivo

Aquí se muestra cómo guardar trazos de lápiz dibujados en un control [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

**Descargar este ejemplo de [Guardar y cargar trazos de lápiz desde un archivo de formato Ink Serialized Format (ISF)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)**

1.  En primer lugar, debemos configurar la interfaz de usuario.

    La interfaz de usuario incluye los botones "Save", "Load" y "Clear", así como el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

    El [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada del lápiz y el mouse como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)), y se declaran los agentes de escucha para los eventos de clic de los botones.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Por último, guardamos la entrada de lápiz en el controlador de eventos de clic del botón **Save**.

    Un [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) permite al usuario seleccionar el archivo y la ubicación donde deben guardarse los datos de entrada de lápiz.

    Una vez que se selecciona un archivo, se abre una secuencia [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) establecida en [**ReadWrite**](https://msdn.microsoft.com/library/windows/apps/br241635).

    Después, llamamos al método [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/br242114) para serializar los trazos de lápiz administrados por el objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) en la secuencia.

```csharp
// Save ink data to a file.
    private async void btnSave_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Strokes present on ink canvas.
        if (currentStrokes.Count > 0)
        {
            // Let users choose their ink file using a file picker.
            // Initialize the picker.
            Windows.Storage.Pickers.FileSavePicker savePicker = 
                new Windows.Storage.Pickers.FileSavePicker();
            savePicker.SuggestedStartLocation = 
                Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
            savePicker.FileTypeChoices.Add(
                "GIF with embedded ISF", 
                new List<string>() { ".gif" });
            savePicker.DefaultFileExtension = ".gif";
            savePicker.SuggestedFileName = "InkSample";

            // Show the file picker.
            Windows.Storage.StorageFile file = 
                await savePicker.PickSaveFileAsync();
            // When chosen, picker returns a reference to the selected file.
            if (file != null)
            {
                // Prevent updates to the file until updates are 
                // finalized with call to CompleteUpdatesAsync.
                Windows.Storage.CachedFileManager.DeferUpdates(file);
                // Open a file stream for writing.
                IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
                // Write the ink strokes to the output stream.
                using (IOutputStream outputStream = stream.GetOutputStreamAt(0))
                {
                    await inkCanvas.InkPresenter.StrokeContainer.SaveAsync(outputStream);
                    await outputStream.FlushAsync();
                }
                stream.Dispose();

                // Finalize write so other apps can update file.
                Windows.Storage.Provider.FileUpdateStatus status =
                    await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);

                if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
                {
                    // File saved.
                }
                else
                {
                    // File couldn't be saved.
                }
            }
            // User selects Cancel and picker returns null.
            else
            {
                // Operation cancelled.
            }
        }
    }
```

> [!NOTE]
> El formato GIF es el único formato de archivo admitido para guardar datos de entrada de lápiz. Sin embargo, el método [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) (que se muestra en la sección siguiente) admite otros formatos por motivos de compatibilidad con versiones anteriores.

## <a name="load-ink-strokes-from-a-file"></a>Cargar trazos de lápiz de un archivo

Aquí se muestra cómo cargar trazos de lápiz de un archivo y representarlos en un control [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

**Descargar este ejemplo de [Guardar y cargar trazos de lápiz desde un archivo de formato Ink Serialized Format (ISF)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)**

1.  En primer lugar, debemos configurar la interfaz de usuario.

    La interfaz de usuario incluye los botones "Save", "Load" y "Clear", así como el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="Header" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnSave" 
                    Content="Save" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnLoad" 
                    Content="Load" 
                    Margin="50,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <InkCanvas x:Name="inkCanvas" />
        </Grid>
    </Grid>
```

2.  A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

    El [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada del lápiz y el mouse como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)), y se declaran los agentes de escucha para los eventos de clic de los botones.
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to initiate save.
        btnSave.Click += btnSave_Click;
        // Listen for button click to initiate load.
        btnLoad.Click += btnLoad_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;
    }
```

3.  Por último, cargamos la entrada de lápiz en el controlador de eventos de clic del botón **Load**.

    Un [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) permite al usuario seleccionar el archivo y la ubicación de donde deben recuperarse los datos de entrada de lápiz guardados.

    Una vez que se selecciona un archivo, se abre una secuencia [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731) establecida en [**Read**](https://msdn.microsoft.com/library/windows/apps/br241635).

    A continuación, llamamos a [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) para leer, deserializar y cargar los trazos de lápiz guardados en el [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Cuando los trazos se cargan en el **InkStrokeContainer**, el [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) los representa inmediatamente en el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

    > [!NOTE]
    > Se borran todos los trazos existentes en el objeto InkStrokeContainer antes de cargarse los trazos nuevos.

``` csharp
// Load ink data from a file.
private async void btnLoad_Click(object sender, RoutedEventArgs e)
{
    // Let users choose their ink file using a file picker.
    // Initialize the picker.
    Windows.Storage.Pickers.FileOpenPicker openPicker =
        new Windows.Storage.Pickers.FileOpenPicker();
    openPicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    openPicker.FileTypeFilter.Add(".gif");
    // Show the file picker.
    Windows.Storage.StorageFile file = await openPicker.PickSingleFileAsync();
    // User selects a file and picker returns a reference to the selected file.
    if (file != null)
    {
        // Open a file stream for reading.
        IRandomAccessStream stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        // Read from file.
        using (var inputStream = stream.GetInputStreamAt(0))
        {
            await inkCanvas.InkPresenter.StrokeContainer.LoadAsync(inputStream);
        }
        stream.Dispose();
    }
    // User selects Cancel and picker returns null.
    else
    {
        // Operation cancelled.
    }
}
```

> [!NOTE]
> El formato GIF es el único formato de archivo admitido para guardar datos de entrada de lápiz. Sin embargo, el método [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/hh701607) es compatible con los siguientes formatos por motivos de compatibilidad con versiones anteriores.

| Formato                    | Descripción |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| InkSerializedFormat       | Especifica la entrada de lápiz que se vuelve permanente usando ISF. Este formato es la representación más compacta y persistente de la entrada de lápiz. Puede incrustarse en un formato de documento binario o colocarse directamente en el Portapapeles.                                                                                                                                                                                                         |
| Base64InkSerializedFormat | Especifica la entrada de lápiz que se vuelve permanente codificando el ISF como una secuencia en base64. Este formato se ofrece para que la entrada de lápiz pueda codificarse directamente en un archivo XML o HTML.                                                                                                                                                                                                                                                |
| Gif                       | Especifica la entrada de lápiz que se vuelve permanente usando un archivo GIF que contiene el ISF como metadatos incrustados en el archivo. Esto permite ver la entrada de lápiz en aplicaciones que no están habilitadas para entrada de lápiz y mantener la fidelidad total de la entrada de lápiz al volver a una aplicación habilitada para esa entrada. Este formato es ideal cuando se transporta contenido de entrada de lápiz en un archivo HTML y para facilitar su uso por parte de aplicaciones que estén o no habilitadas para entrada de lápiz. |
| Base64Gif                 | Especifica la entrada de lápiz que se vuelve permanente usando un GIF fortificado codificado en base64. Este formato se proporciona cuando se debe codificar la entrada de lápiz directamente en un archivo XML o HTML para la conversión posterior en imagen. Un uso posible es en un formato XML generado para contener toda la información de entrada de lápiz que se use como una manera de generar HTML mediante el Lenguaje de transformación basado en hojas de estilo (XSLT). 

## <a name="copy-and-paste-ink-strokes-with-the-clipboard"></a>Copiar y pegar trazos de lápiz con el Portapapeles

Aquí se muestra cómo usar el Portapapeles para transferir los trazos de lápiz entre aplicaciones.

Para admitir la funcionalidad de Portapapeles, los comandos Cortar y Copiar integrados del [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) requieren la selección de uno o más trazos de lápiz.

Para este ejemplo, habilitamos la selección de trazo cuando se modifica la entrada con el botón de menú contextual del lápiz (o el botón derecho del mouse). Para obtener un ejemplo completo de cómo implementar la selección de trazo, consulta Entrada de paso a través para el procesamiento avanzado en [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md).

**Descargar este ejemplo de [Guardar y cargar trazos de lápiz desde el Portapapeles](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)**

1.  En primer lugar, debemos configurar la interfaz de usuario.

    La interfaz de usuario incluye los botones "Cut", "Copy", "Paste" y "Clear", junto con el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) y un lienzo de selección.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
            <TextBlock x:Name="tbHeader" 
                       Text="Basic ink store sample" 
                       Style="{ThemeResource HeaderTextBlockStyle}" 
                       Margin="10,0,0,0" />
            <Button x:Name="btnCut" 
                    Content="Cut" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnCopy" 
                    Content="Copy" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnPaste" 
                    Content="Paste" 
                    Margin="20,0,10,0"/>
            <Button x:Name="btnClear" 
                    Content="Clear" 
                    Margin="20,0,10,0"/>
        </StackPanel>
        <Grid x:Name="gridCanvas" Grid.Row="1">
            <!-- Canvas for displaying selection UI. -->
            <Canvas x:Name="selectionCanvas"/>
            <!-- Inking area -->
            <InkCanvas x:Name="inkCanvas"/>
        </Grid>
    </Grid>
```

2.  A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

    El [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada de lápiz y mouse como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Aquí también se declaran los agentes de escucha para los eventos de clic de los botones, y los eventos de puntero y trazo para la funcionalidad de selección.

    Para obtener un ejemplo completo de cómo implementar la selección de trazo, consulta Entrada de paso a través para el procesamiento avanzado en [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md).
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for button click to cut ink strokes.
        btnCut.Click += btnCut_Click;
        // Listen for button click to copy ink strokes.
        btnCopy.Click += btnCopy_Click;
        // Listen for button click to paste ink strokes.
        btnPaste.Click += btnPaste_Click;
        // Listen for button click to clear ink canvas.
        btnClear.Click += btnClear_Click;

        // By default, the InkPresenter processes input modified by 
        // a secondary affordance (pen barrel button, right mouse 
        // button, or similar) as ink.
        // To pass through modified input to the app for custom processing 
        // on the app UI thread instead of the background ink thread, set 
        // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
        inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
            InkInputRightDragAction.LeaveUnprocessed;

        // Listen for unprocessed pointer events from modified input.
        // The input is used to provide selection functionality.
        inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
            UnprocessedInput_PointerPressed;
        inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
            UnprocessedInput_PointerMoved;
        inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
            UnprocessedInput_PointerReleased;

        // Listen for new ink or erase strokes to clean up selection UI.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            StrokeInput_StrokeStarted;
        inkCanvas.InkPresenter.StrokesErased +=
            InkPresenter_StrokesErased;
    }
```

3.  Por último, después de agregar compatibilidad de selección de trazo, implementamos la funcionalidad del Portapapeles en los controladores de eventos de clic de los botones **Cut**, **Copy** y **Paste**.

    Para cortar, primero llamamos a [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232) en el [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) del [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).

    A continuación, llamamos a [**DeleteSelected**](https://msdn.microsoft.com/library/windows/apps/br244233) para quitar los trazos del lienzo de entrada de lápiz.

    Por último, eliminamos todos los trazos de selección del lienzo de selección.
    
```csharp
private void btnCut_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
        ClearSelection();
    }
```
```csharp
// Clean up selection UI.
    private void ClearSelection()
    {
        var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        foreach (var stroke in strokes)
        {
            stroke.Selected = false;
        }
        ClearDrawnBoundingRect();
    }

    private void ClearDrawnBoundingRect()
    {
        if (selectionCanvas.Children.Any())
        {
            selectionCanvas.Children.Clear();
            boundingRect = Rect.Empty;
        }
    }
```

Para copiar, simplemente llamamos a [**CopySelectedToClipboard**](https://msdn.microsoft.com/library/windows/apps/br244232) en el [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) del [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011).


```csharp
private void btnCopy_Click(object sender, RoutedEventArgs e)
    {
        inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
    }
```

Para pegar, llamamos a [**CanPasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208495) para garantizar que el contenido del Portapapeles se pueda pegar en el lienzo de entrada de lápiz.

Si es así, llamamos a [**PasteFromClipboard**](https://msdn.microsoft.com/library/windows/apps/br208503) para insertar los trazos de lápiz del Portapapeles en el objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) del objeto [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011), lo que representará los trazos en el lienzo de entrada de lápiz.

```csharp
private void btnPaste_Click(object sender, RoutedEventArgs e)
    {
        if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
        {
            inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                new Point(0, 0));
        }
        else
        {
            // Cannot paste from clipboard.
        }
    }
```

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md)

**Ejemplos del tema**
* [Guardar y cargar trazos de lápiz desde un archivo de formato Ink Serialized Format (ISF)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Guardar y cargar trazos de lápiz desde el Portapapeles](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)

**Otras muestras**
* [Muestra de entrada de lápiz simple (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Muestra de entrada de lápiz compleja (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Muestra de entrada de lápiz (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Tutorial de introducción: Admitir la entrada de lápiz en tu aplicación para UWP](https://aka.ms/appsample-ink)
* [Muestra de libro para colorear](https://aka.ms/cpubsample-coloringbook)
* [Muestra de notas familiares](https://aka.ms/cpubsample-familynotessample)



