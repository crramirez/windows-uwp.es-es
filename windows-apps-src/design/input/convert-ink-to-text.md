---
author: Karl-Bridge-Microsoft
Description: Use handwriting recognition and ink analysis to recognize Windows Ink strokes as text and shapes.
title: Reconocer trazos de Windows Ink como texto y formas
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, reconocimiento de escritura a mano, interacción del usuario, entrada
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 83142b0a3b24e25f8e7a922d800262f505cd8cc2
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7569135"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Reconocer trazos de Windows Ink como texto y formas

Convierte trazos de lápiz en texto y formas mediante las capacidades de reconocimiento integradas en Windows Ink.

> **API importantes**: [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535), [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


## <a name="free-form-recognition-with-ink-analysis"></a>Reconocimiento de formas libres mediante análisis de la entrada de lápiz

Aquí se muestra cómo usar el motor de análisis de Windows Ink ([Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)) para clasificar, analizar y reconocer un conjunto de trazos de forma libre en un [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) como texto o formas. Además del reconocimiento de texto y formas, el análisis de la entrada de lápiz puede usarse para reconocer la estructura de documentos, listas con viñetas y dibujos genéricos.

> [!NOTE]
> Para los escenarios de texto básico, en una línea y sin formato, como la entrada de formas, consulta la sección anterior: [Reconocimiento restringido de escritura a mano](#constrained-handwriting-recognition).

En este ejemplo, el reconocimiento se inicia cuando el usuario hace clic en un botón para indicar que ha terminado de dibujar.

**Descargar este ejemplo de [Ejemplo de análisis (básico) de la entrada de lápiz](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1.  En primer lugar, debemos configurar la interfaz de usuario (MainPage.xaml). 

    La interfaz de usuario incluye un botón "Reconocer", un [**InkCanvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)y un [**Canvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas) estándar. Cuando se presiona el botón "Reconocer", se analizan todos los trazos de lápiz en lienzo y (si se reconocen) el texto y las formas correspondientes se dibujan en el lienzo estándar. A continuación, se eliminan los trazos de lápiz originales del lienzo de entrada de lápiz.
```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel" 
                    Orientation="Horizontal" 
                    Grid.Row="0">
            <TextBlock x:Name="Header" 
                        Text="Basic ink analysis sample" 
                        Style="{ThemeResource HeaderTextBlockStyle}" 
                        Margin="10,0,0,0" />
            <Button x:Name="recognize" 
                    Content="Recognize" 
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid x:Name="drawingCanvas" Grid.Row="1">

            <!-- The canvas where we render the replacement text and shapes. -->
            <Canvas x:Name="recognitionCanvas" />
            <!-- The canvas for ink input. -->
            <InkCanvas x:Name="inkCanvas" />

        </Grid>
    </Grid>
```
2. En el archivo de código subyacente de la interfaz de usuario (MainPage.xaml.cs), agrega las referencias de tipo del espacio de nombres necesarias para nuestra funcionalidad de entrada de lápiz y de análisis de la entrada de lápiz:
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)
    - [Windows.UI.Input.Inking.Analysis](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes)

3. A continuación, se especifican las variables globales:
```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
```
4.  Luego, definimos algunos comportamientos de entrada de lápiz básicos:
    - El [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada de lápiz, mouse y entrada táctil como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). 
    - Los trazos de lápiz se representan en el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) con los [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados. 
    - También se declara un agente de escucha para el evento clic en el botón "Reconocer".
```csharp
/// <summary>
/// Initialize the UI page.
/// </summary>
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen | 
            Windows.UI.Core.CoreInputDeviceTypes.Touch;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += RecognizeStrokes_Click;
    }
```
5.  Para este ejemplo, el análisis de la entrada de lápiz se realiza en el controlador de eventos de clic del botón "Reconocer".
    - En primer lugar, llama a [**GetStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) en la propiedad [**StrokeContainer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) de la clase [**InkCanvas.InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) para obtener la colección de todos los trazos de lápiz actuales.
    - Si hay trazos de lápiz, pásalos en una llamada a [**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) de la clase InkAnalyzer.
    - Estamos intentando reconocer dibujos y texto, pero puedes usar el método [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) para especificar si estás interesado únicamente en texto (incluyendo la estructura del documento y listas con viñetas) o solo en dibujos (incluyendo reconocimiento de formas).
    - Llama a [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) para iniciar el análisis de entrada de lápiz y obtener la clase [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si [**Status**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) devuelve un estado de **Updated**, llama a [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) tanto para [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) como para [**InkAnalysisNodeKind.InkDrawing**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Itera por ambos conjuntos de tipos de nodos y dibuja el texto o la forma correspondientes en el lienzo de reconocimiento (debajo del lienzo de entrada de lápiz).
    - Por último, elimina los nodos reconocidos de la clase InkAnalyzer y los trazos de lápiz correspondientes del lienzo de entrada de lápiz.
```csharp
/// <summary>
/// The "Analyze" button click handler.
/// Ink recognition is performed here.
/// </summary>
/// <param name="sender">Source of the click event</param>
/// <param name="e">Event args for the button click routed event</param>
private async void RecognizeStrokes_Click(object sender, RoutedEventArgs e)
    {
        inkStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (inkStrokes.Count > 0)
        {
            inkAnalyzer.AddDataForStrokes(inkStrokes);

            // In this example, we try to recognizing both 
            // writing and drawing, so the platform default 
            // of "InkAnalysisStrokeKind.Auto" is used.
            // If you're only interested in a specific type of recognition,
            // such as writing or drawing, you can constrain recognition 
            // using the SetStrokDataKind method as follows:
            // foreach (var stroke in strokesText)
            // {
            //     analyzerText.SetStrokeDataKind(
            //      stroke.Id, InkAnalysisStrokeKind.Writing);
            // }
            // This can improve both efficiency and recognition results.
            inkAnalysisResults = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (inkAnalysisResults.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes = 
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Draw primary recognized text on recognitionCanvas 
                // (for this example, we ignore alternatives), and delete 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    // Draw a TextBlock object on the recognitionCanvas.
                    DrawText(node.RecognizedText, node.BoundingRect);

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke = 
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();

                // Find all strokes that are recognized as a drawing and 
                // create a corresponding ink analysis InkDrawing node.
                var inkdrawingNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkDrawing);
                // Iterate through each InkDrawing node.
                // Draw recognized shapes on recognitionCanvas and
                // delete ink analysis data and recognized strokes.
                foreach (InkAnalysisInkDrawing node in inkdrawingNodes)
                {
                    if (node.DrawingKind == InkAnalysisDrawingKind.Drawing)
                    {
                        // Catch and process unsupported shapes (lines and so on) here.
                    }
                    // Process generalized shapes here (ellipses and polygons).
                    else
                    {
                        // Draw an Ellipse object on the recognitionCanvas (circle is a specialized ellipse).
                        if (node.DrawingKind == InkAnalysisDrawingKind.Circle || node.DrawingKind == InkAnalysisDrawingKind.Ellipse)
                        {
                            DrawEllipse(node);
                        }
                        // Draw a Polygon object on the recognitionCanvas.
                        else
                        {
                            DrawPolygon(node);
                        }
                        foreach (var strokeId in node.GetStrokeIds())
                        {
                            var stroke = inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                            stroke.Selected = true;
                        }
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
    }
```
6. Esta es la función para dibujar una clase TextBlock en nuestro lienzo de reconocimiento. Usamos el rectángulo delimitador del trazo de lápiz asociado en el lienzo de entrada de lápiz para establecer la posición y el tamaño de fuente de la clase TextBlock.
```csharp
/// <summary>
/// Draw ink recognition text string on the recognitionCanvas.
/// </summary>
/// <param name="recognizedText">The string returned by text recognition.</param>
/// <param name="boundingRect">The bounding rect of the original ink writing.</param>
private void DrawText(string recognizedText, Rect boundingRect)
{
    TextBlock text = new TextBlock();
    Canvas.SetTop(text, boundingRect.Top);
    Canvas.SetLeft(text, boundingRect.Left);

    text.Text = recognizedText;
    text.FontSize = boundingRect.Height;

    recognitionCanvas.Children.Add(text);
}
```
7. Estas son las funciones para dibujar elipses y polígonos en el lienzo de reconocimiento. Usamos el rectángulo delimitador del trazo de lápiz asociado en el lienzo de entrada de lápiz para establecer la posición y el tamaño de fuente de las formas.
```csharp
    // Draw an ellipse on the recognitionCanvas.
    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        var points = shape.Points;
        Ellipse ellipse = new Ellipse();

        ellipse.Width = shape.BoundingRect.Width;
        ellipse.Height = shape.BoundingRect.Height;

        Canvas.SetTop(ellipse, shape.BoundingRect.Top);
        Canvas.SetLeft(ellipse, shape.BoundingRect.Left);

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        ellipse.Stroke = brush;
        ellipse.StrokeThickness = 2;
        recognitionCanvas.Children.Add(ellipse);
    }

    // Draw a polygon on the recognitionCanvas.
    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        List<Point> points = new List<Point>(shape.Points);
        Polygon polygon = new Polygon();

        foreach (Point point in points)
        {
            polygon.Points.Add(point);
        }

        var brush = new SolidColorBrush(Windows.UI.ColorHelper.FromArgb(255, 0, 0, 255));
        polygon.Stroke = brush;
        polygon.StrokeThickness = 2;
        recognitionCanvas.Children.Add(polygon);
    }
```

Así se ve el ejemplo en acción:

| Antes del análisis | Después del análisis |
| --- | --- |
| ![Antes del análisis](images/ink/ink-analysis-raw2-small.png) | ![Después del análisis](images/ink/ink-analysis-analyzed2-small.png) |

---

## <a name="constrained-handwriting-recognition"></a>Reconocimiento restringido de escritura a mano

En la sección anterior ([Reconocimiento de formas libres mediante análisis de entrada de lápiz](#free-form-recognition-with-ink-analysis)), se mostró cómo usar las [API de análisis de entrada de lápiz](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis) para analizar y reconocer trazos de lápiz arbitrarios dentro de un área de InkCanvas.

En esta sección, se muestra cómo usar el motor de reconocimiento de escritura a mano de Windows Ink (no análisis de escritura a mano) para convertir un conjunto de trazos de un [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) en texto (conforme al paquete de idioma instalado predeterminado).

> [!NOTE]
> El reconocimiento básico de escritura a mano de esta sección es ideal para escenarios de entrada de texto, en una línea, como la entrada de formas. Para escenarios de reconocimiento enriquecidos que incluyen el análisis y la interpretación de la estructura de documentos, elementos de lista, formas y dibujos (además del reconocimiento de texto), consulta la sección anterior: [Reconocimiento de formas libres mediante análisis de entrada de lápiz](#free-form-recognition-with-ink-analysis).

En este ejemplo, el reconocimiento se inicia cuando el usuario hace clic en un botón para indicar que ha terminado de escribir.

**Descargar este ejemplo de [Ejemplo de reconocimiento de escritura a mano de la entrada de lápiz](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)**

1.  En primer lugar, debemos configurar la interfaz de usuario.

    La interfaz de usuario incluye un botón "Reconocer", el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) y un área de visualización de los resultados del reconocimiento.    
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Basic ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <Button x:Name="recognize"
                    Content="Recognize"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2. Para este ejemplo, primero debes agregar las referencias de tipo de espacio de nombres necesarias para nuestra funcionalidad de entrada de lápiz:
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/windows.ui.input.inking)


3.  A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

    El [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada de lápiz y mouse como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Los trazos de lápiz se representan en el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) con los [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados. También se declara un agente de escucha para el evento clic en el botón "Reconocer".
```csharp
public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Set initial ink stroke attributes.
        InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
        drawingAttributes.Color = Windows.UI.Colors.Black;
        drawingAttributes.IgnorePressure = false;
        drawingAttributes.FitToCurve = true;
        inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

        // Listen for button click to initiate recognition.
        recognize.Click += Recognize_Click;
    }
```

4.  Por último, realizamos el reconocimiento de escritura a mano básico. Para este ejemplo, usamos el controlador de eventos de clic del botón "Reconocer" para llevar a cabo el reconocimiento de escritura a mano.

    Un [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) almacena todos los trazos de lápiz en un objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Los trazos se exponen a través de la propiedad [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) del **InkPresenter** y se recuperan mediante el método [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.

```csharp
// Create a manager for the InkRecognizer object
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer =
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (var result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates = result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.

```csharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // Create a manager for the InkRecognizer object
            // used in handwriting recognition.
            InkRecognizerContainer inkRecognizerContainer =
                new InkRecognizerContainer();

            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (var result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates = result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog = new Windows.UI.Popups.MessageDialog("You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <a name="international-recognition"></a>Reconocimiento internacional

El reconocimiento de escritura a mano integrado en la plataforma de Windows Ink incluye un amplio subconjunto de configuraciones regionales e idiomas compatibles con Windows.

Para obtener una lista de idiomas compatibles con [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkrecognizer.name.aspx), consulta el tema de la propiedad [**InkRecognizer.Name**](https://msdn.microsoft.com/library/windows/apps/br208478).

Tu aplicación puede consultar el conjunto de los motores de reconocimiento de escritura a mano instalados y usar uno de ellos, o permitir que los usuarios seleccionen su idioma preferido.

**Nota**  los usuarios pueden ver una lista de idiomas instalados yendo a **configuración -&gt; hora e idioma**. Los idiomas instalados se enumeran en **Idiomas**.

Para instalar nuevos paquetes de idioma y habilitar el reconocimiento de escritura a mano para ese idioma:

1.  Ve a **Configuración &gt; Hora e idioma &gt; Región e idioma**.
2.  Selecciona **Agregar un idioma**.
3.  Selecciona un idioma de la lista y, después, elige la versión de la región. El idioma aparece ahora en la página **Región e idioma**.
4.  Haz clic en el idioma y selecciona **Opciones**.
5.  En la página **Opciones de idioma**, descarga el **motor de reconocimiento de escritura a mano** (aquí también se pueden descargar el paquete de idioma completo, el motor de reconocimiento de voz y la distribución del teclado).

 

Aquí se muestra cómo usar el motor de reconocimiento de escritura a mano para interpretar un conjunto de trazos en un [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) según el reconocedor seleccionado.

Para iniciar el reconocimiento, el usuario debe hacer clic en un botón cuando termina de escribir a mano.

1.  En primer lugar, debemos configurar la interfaz de usuario.

    La interfaz de usuario incluye un botón "Reconocer", un cuadro combinado que enumera todos los reconocedores de escritura a mano instalados, el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) y un área para visualizar los resultados del reconocimiento.
```    XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="HeaderPanel"
                    Orientation="Horizontal"
                    Grid.Row="0">
            <TextBlock x:Name="Header"
                       Text="Advanced international ink recognition sample"
                       Style="{ThemeResource HeaderTextBlockStyle}"
                       Margin="10,0,0,0" />
            <ComboBox x:Name="comboInstalledRecognizers"
                     Margin="50,0,10,0">
                <ComboBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock Text="{Binding Name}" />
                        </StackPanel>
                    </DataTemplate>
                </ComboBox.ItemTemplate>
            </ComboBox>
            <Button x:Name="buttonRecognize"
                    Content="Recognize"
                    IsEnabled="False"
                    Margin="50,0,10,0"/>
        </StackPanel>
        <Grid Grid.Row="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="*"/>
                <RowDefinition Height="Auto"/>
            </Grid.RowDefinitions>
            <InkCanvas x:Name="inkCanvas"
                       Grid.Row="0"/>
            <TextBlock x:Name="recognitionResult"
                       Grid.Row="1"
                       Margin="50,0,10,0"/>
        </Grid>
    </Grid>
```

2.  A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

    El [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada de lápiz y mouse como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Los trazos de lápiz se representan en el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) con los [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados.

    Llamamos a una función `InitializeRecognizerList` para rellenar el cuadro combinado del reconocedor con una lista de reconocedores de escritura a mano instalados.

    También declaramos agentes de escucha para el evento de clic en el botón "Reconocer" y el evento cambiado por la selección en el cuadro combinado del reconocedor.
```csharp
 public MainPage()
     {
         this.InitializeComponent();

         // Set supported inking device types.
         inkCanvas.InkPresenter.InputDeviceTypes =
             Windows.UI.Core.CoreInputDeviceTypes.Mouse |
             Windows.UI.Core.CoreInputDeviceTypes.Pen;

         // Set initial ink stroke attributes.
         InkDrawingAttributes drawingAttributes = new InkDrawingAttributes();
         drawingAttributes.Color = Windows.UI.Colors.Black;
         drawingAttributes.IgnorePressure = false;
         drawingAttributes.FitToCurve = true;
         inkCanvas.InkPresenter.UpdateDefaultDrawingAttributes(drawingAttributes);

         // Populate the recognizer combo box with installed recognizers.
         InitializeRecognizerList();

         // Listen for combo box selection.
         comboInstalledRecognizers.SelectionChanged +=
             comboInstalledRecognizers_SelectionChanged;

         // Listen for button click to initiate recognition.
         buttonRecognize.Click += Recognize_Click;
     }
```

3.  Rellenamos el cuadro combinado del reconocedor con una lista de los reconocedores de escritura a mano instalados.

    Se crea un [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) para administrar el proceso de reconocimiento de escritura a mano. Usa este objeto para llamar a [**GetRecognizers**](https://msdn.microsoft.com/library/windows/apps/br208480) y recuperar la lista de reconocedores instalados a fin de rellenar el cuadro combinado del reconocedor.
```csharp
// Populate the recognizer combo box with installed recognizers.
    private void InitializeRecognizerList()
    {
        // Create a manager for the handwriting recognition process.
        inkRecognizerContainer = new InkRecognizerContainer();
        // Retrieve the collection of installed handwriting recognizers.
        IReadOnlyList<InkRecognizer> installedRecognizers =
            inkRecognizerContainer.GetRecognizers();
        // inkRecognizerContainer is null if a recognition engine is not available.
        if (!(inkRecognizerContainer == null))
        {
            comboInstalledRecognizers.ItemsSource = installedRecognizers;
            buttonRecognize.IsEnabled = true;
        }
    }
```

4.  Actualiza el reconocedor de escritura a mano si la selección del cuadro combinado del reconocedor cambia.

    Usa el [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) para llamar a [**SetDefaultRecognizer**](https://msdn.microsoft.com/library/windows/apps/hh920328) según el reconocedor seleccionado en el cuadro combinado del reconocedor.
```csharp
// Handle recognizer change.
    private void comboInstalledRecognizers_SelectionChanged(
        object sender, SelectionChangedEventArgs e)
    {
        inkRecognizerContainer.SetDefaultRecognizer(
            (InkRecognizer)comboInstalledRecognizers.SelectedItem);
    }
```

5.  Por último, realizamos el reconocimiento de escritura a mano según el reconocedor de escritura a mano seleccionado. Para este ejemplo, usamos el controlador de eventos de clic del botón "Reconocer" para llevar a cabo el reconocimiento de escritura a mano.

    Un [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) almacena todos los trazos de lápiz en un objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Los trazos se exponen a través de la propiedad [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) del **InkPresenter** y se recuperan mediante el método [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```csharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes =
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).

```csharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).

```csharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
    {
        // Get all recognition candidates from each recognition result.
        IReadOnlyList<string> candidates =
            result.GetTextCandidates();
        str += "Candidates: " + candidates.Count.ToString() + "\n";
        foreach (string candidate in candidates)
        {
            str += candidate + " ";
        }
    }
    // Display the recognition candidates.
    recognitionResult.Text = str;
    // Clear the ink canvas once recognition is complete.
    inkCanvas.InkPresenter.StrokeContainer.Clear();
```

    Here's the click handler example, in full.
    
```csharp
// Handle button click to initiate recognition.
    private async void Recognize_Click(object sender, RoutedEventArgs e)
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

        // Ensure an ink stroke is present.
        if (currentStrokes.Count > 0)
        {
            // inkRecognizerContainer is null if a recognition engine is not available.
            if (!(inkRecognizerContainer == null))
            {
                // Recognize all ink strokes on the ink canvas.
                IReadOnlyList<InkRecognitionResult> recognitionResults =
                    await inkRecognizerContainer.RecognizeAsync(
                        inkCanvas.InkPresenter.StrokeContainer,
                        InkRecognitionTarget.All);
                // Process and display the recognition results.
                if (recognitionResults.Count > 0)
                {
                    string str = "Recognition result\n";
                    // Iterate through the recognition results.
                    foreach (InkRecognitionResult result in recognitionResults)
                    {
                        // Get all recognition candidates from each recognition result.
                        IReadOnlyList<string> candidates =
                            result.GetTextCandidates();
                        str += "Candidates: " + candidates.Count.ToString() + "\n";
                        foreach (string candidate in candidates)
                        {
                            str += candidate + " ";
                        }
                    }
                    // Display the recognition candidates.
                    recognitionResult.Text = str;
                    // Clear the ink canvas once recognition is complete.
                    inkCanvas.InkPresenter.StrokeContainer.Clear();
                }
                else
                {
                    recognitionResult.Text = "No recognition results.";
                }
            }
            else
            {
                Windows.UI.Popups.MessageDialog messageDialog =
                    new Windows.UI.Popups.MessageDialog(
                        "You must install handwriting recognition engine.");
                await messageDialog.ShowAsync();
            }
        }
        else
        {
            recognitionResult.Text = "No ink strokes to recognize.";
        }
    }
```

## <a name="dynamic-recognition"></a>Reconocimiento dinámico

Si bien los dos ejemplos anteriores requieren que el usuario presione un botón para iniciar el reconocimiento, también puedes realizar el reconocimiento dinámico mediante la entrada de trazos en combinación con una función de temporización básica.

Para este ejemplo, usaremos la misma interfaz de usuario y la misma configuración de trazos que en el ejemplo de reconocimiento internacional anterior.

1. Estos objetos globales ([InkAnalyzer](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult) y [DispatcherTimer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dispatchertimer)) se usan en toda nuestra aplicación.    
```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
```

2.  En lugar de un botón para iniciar el reconocimiento, agregamos agentes de escucha para dos eventos de trazo de lápiz del [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) y [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)) y configuramos un temporizador básico ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)) con un segundo intervalo de [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256).    
```csharp
    public MainPage()
    {
        this.InitializeComponent();

        // Set supported inking device types.
        inkCanvas.InkPresenter.InputDeviceTypes =
            Windows.UI.Core.CoreInputDeviceTypes.Mouse |
            Windows.UI.Core.CoreInputDeviceTypes.Pen;

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
            inkCanvas_StrokeStarted;

        inkAnalyzer = new InkAnalyzer();

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = TimeSpan.FromSeconds(1);
        recoTimer.Tick += recoTimer_TickAsync;
    }
```

3.  A continuación, definimos los controladores para los eventos de InkPresenter que declaramos en el primer paso (pasamos por alto también el evento de la página [**OnNavigatingFrom**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) para administrar el temporizador).

    - [**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    Agrega entrada de lápiz ([**AddDataForStrokes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) al InkAnalyzer e inicia el temporizador de reconocimiento cuando el usuario detenga la entrada manuscrita (levantando el lápiz o el dedo, o bien soltando el botón del ratón). Después de un segundo sin entrada de lápiz, se inicia el reconocimiento.  

        Usa el método [**SetStrokeDataKind**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) para especificar si estás interesado únicamente en texto (incluyendo la estructura del documento y listas con viñetas) o solo en dibujos (incluyendo reconocimiento de formas).

    - [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    Si se inicia un nuevo trazo antes del siguiente evento de graduación del temporizador, detén el temporizador, ya que el nuevo trazo es probablemente la continuación de una sola entrada de escritura a mano.
```csharp
    // Handler for the InkPresenter StrokeStarted event.
    // Don't perform analysis while a stroke is in progress.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }    
    // Handler for the InkPresenter StrokesCollected event.
    // Stop the timer and add the collected strokes to the InkAnalyzer.
    // Start the recognition timer when the user stops inking (by 
    // lifting their pen or finger, or releasing the mouse button).
    // If ink input is not detected after one second, initiate recognition.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Stop();
        // If you're only interested in a specific type of recognition,
        // such as writing or drawing, you can constrain recognition 
        // using the SetStrokDataKind method, which can improve both 
        // efficiency and recognition results.
        // In this example, "InkAnalysisStrokeKind.Writing" is used.
        foreach (var stroke in args.Strokes)
        {
            inkAnalyzer.AddDataForStroke(stroke);
            inkAnalyzer.SetStrokeDataKind(stroke.Id, InkAnalysisStrokeKind.Writing);
        }
        recoTimer.Start();
    }    
    // Override the Page OnNavigatingFrom event handler to 
    // stop our timer if user leaves page.
    protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
    {
        recoTimer.Stop();
    } 
```

4.  Por último, realizamos el reconocimiento de escritura a mano. Para este ejemplo, usamos el identificador de eventos [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256) de un [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) para iniciar el reconocimiento de escritura a mano.
    - Llama a [**AnalyzeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) para iniciar el análisis de entrada de lápiz y obtener la clase [**InkAnalysisResult**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si [**Status**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) devuelve un estado **Actualizado**, llama a [**FindNodes**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) para los tipos de nodo de  [**InkAnalysisNodeKind.InkWord**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Recorre los nodos y muestra el texto reconocido.
    - Por último, elimina los nodos reconocidos de la clase InkAnalyzer y los trazos de lápiz correspondientes del lienzo de entrada de lápiz.
```csharp
    private async void recoTimer_TickAsync(object sender, object e)
    {
        recoTimer.Stop();
        if (!inkAnalyzer.IsAnalyzing)
        {
            InkAnalysisResult result = await inkAnalyzer.AnalyzeAsync();

            // Have ink strokes on the canvas changed?
            if (result.Status == InkAnalysisStatus.Updated)
            {
                // Find all strokes that are recognized as handwriting and 
                // create a corresponding ink analysis InkWord node.
                var inkwordNodes =
                    inkAnalyzer.AnalysisRoot.FindNodes(
                        InkAnalysisNodeKind.InkWord);

                // Iterate through each InkWord node.
                // Display the primary recognized text (for this example, 
                // we ignore alternatives), and then delete the 
                // ink analysis data and recognized strokes.
                foreach (InkAnalysisInkWord node in inkwordNodes)
                {
                    string recognizedText = node.RecognizedText;
                    // Display the recognition candidates.
                    recognitionResult.Text = recognizedText;

                    foreach (var strokeId in node.GetStrokeIds())
                    {
                        var stroke =
                            inkCanvas.InkPresenter.StrokeContainer.GetStrokeById(strokeId);
                        stroke.Selected = true;
                    }
                    inkAnalyzer.RemoveDataForStrokes(node.GetStrokeIds());
                }
                inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            }
        }
        else
        {
            // Ink analyzer is busy. Wait a while and try again.
            recoTimer.Start();
        }
    }
```

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md)

**Ejemplos del tema**
* [Ejemplo de análisis (básico) de la entrada de lápiz (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Ejemplo de reconocimiento de escritura a mano de la entrada de lápiz (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

**Otras muestras**
* [Muestra de entrada de lápiz simple (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Muestra de entrada de lápiz compleja (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Muestra de entrada de lápiz (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Tutorial de introducción: Admitir la entrada de lápiz en tu aplicación para UWP](https://aka.ms/appsample-ink)
* [Muestra de libro para colorear](https://aka.ms/cpubsample-coloringbook)
* [Muestra de notas familiares](https://aka.ms/cpubsample-familynotessample)


 
