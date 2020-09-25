---
Description: Use el reconocimiento de escritura a mano y el análisis de tinta para reconocer trazos de Windows Ink como texto y formas.
title: Reconocer trazos de Windows Ink como texto y formas
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keywords: Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, reconocimiento de escritura a mano, interacción del usuario, entrada
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 66b5303d65e1fefbf3eb8a156ce4ca4c10afda96
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220568"
---
# <a name="recognize-windows-ink-strokes-as-text-and-shapes"></a>Reconocer trazos de Windows Ink como texto y formas

Convierta trazos de lápiz en texto y formas mediante las capacidades de reconocimiento integradas en Windows Ink.

> **API importantes**: [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas), [**Windows. UI. Input. inking**](/uwp/api/Windows.UI.Input.Inking)

## <a name="free-form-recognition-with-ink-analysis"></a>Reconocimiento de forma libre con análisis de tinta

Aquí se muestra cómo usar el motor de análisis de tinta de Windows ([Windows. UI. Input. inking. Analysis](/uwp/api/windows.ui.input.inking.analysis)) para clasificar, analizar y reconocer un conjunto de trazos de forma libre en un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) como texto o formas. (Además del reconocimiento de texto y forma, el análisis de tinta también se puede usar para reconocer la estructura del documento, las listas de viñetas y los dibujos genéricos).

> [!NOTE]
> En el caso de escenarios de texto sin formato básicos, de una sola línea, como la entrada de formulario, vea [reconocimiento de escritura a mano restringido](#constrained-handwriting-recognition) más adelante en este tema.

En este ejemplo, el reconocimiento se inicia cuando el usuario hace clic en un botón para indicar que ha terminado de dibujar.

**Descargar este ejemplo del [ejemplo de análisis de tinta (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)**

1. En primer lugar, se configura la interfaz de usuario (MainPage. xaml). 

   La interfaz de usuario incluye un botón "Recognize", un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)y un [**lienzo**](/uwp/api/windows.ui.xaml.controls.canvas)estándar. Cuando se presiona el botón "reconocer", se analizan todos los trazos de entrada de lápiz del lienzo de tinta y se dibujan (si se reconocen) las formas y el texto correspondientes en el lienzo estándar. Los trazos de tinta originales se eliminan del lienzo de tinta.

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

2. En el archivo de código subyacente de la interfaz de usuario (MainPage.xaml.cs), agregue las referencias de tipo de espacio de nombres necesarias para la funcionalidad de análisis de tinta y de tinta:
    - [Windows.UI.Input.Inking](/uwp/api/windows.ui.input.inking)
    - [Windows. UI. Input. inking. Analysis](/uwp/api/windows.ui.input.inking.analysis)
    - [Windows.UI.Xaml.Shapes](/uwp/api/windows.ui.xaml.shapes)

3. A continuación, se especifican las variables globales:

   ```csharp
    InkAnalyzer inkAnalyzer = new InkAnalyzer();
    IReadOnlyList<InkStroke> inkStrokes = null;
    InkAnalysisResult inkAnalysisResults = null;
   ```

4. A continuación, se establecen algunos comportamientos básicos de entrada de lápiz:
    - El [**objeto InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) se configura para interpretar los datos de entrada del lápiz, el mouse y el toque como trazos de entrada de lápiz ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). 
    - Los trazos de lápiz se representan en el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con los [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class) especificados. 
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

5. En este ejemplo, se realiza el análisis de tinta en el controlador de eventos Click del botón "Recognize".
    - En primer lugar, llame a [**GetStrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.GetStrokes) en el [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.StrokeContainer) de [**InkCanvas. InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) para obtener la colección de todos los trazos de entrada de lápiz actuales.
    - Si los trazos de lápiz están presentes, páselo en una llamada a [**AddDataForStrokes**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer#Windows_UI_Input_Inking_Analysis_InkAnalyzer_AddDataForStrokes_Windows_Foundation_Collections_IIterable_Windows_UI_Input_Inking_InkStroke__) de InkAnalyzer.
    - Estamos intentando reconocer los dibujos y el texto, pero puede usar el método [**SetStrokeDataKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) para especificar si solo le interesa el texto (incluidas la estructura del documento y las listas de viñetas) o solo en los dibujos (incluido el reconocimiento de formas).
    - Llame a [**AnalyzeAsync**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) para iniciar el análisis de tinta y obtener [**InkAnalysisResult**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si [**Estado**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) devuelve un estado de **actualizado**, llame a [**FindNodes**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) para [**InkAnalysisNodeKind. InkWord**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind) y [**InkAnalysisNodeKind. InkDrawing**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Recorra en iteración ambos conjuntos de tipos de nodo y dibuje el texto o la forma correspondiente en el lienzo de reconocimiento (debajo del lienzo de tinta).
    - Por último, elimine los nodos reconocidos del InkAnalyzer y los trazos de tinta correspondientes del lienzo de tinta.

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

6. Esta es la función para dibujar un TextBlock en nuestro lienzo de reconocimiento. Usamos el rectángulo delimitador del trazo de tinta asociado en el lienzo de entrada de lápiz para establecer la posición y el tamaño de fuente del TextBlock.

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

7. Estas son las funciones para dibujar elipses y polígonos en nuestro lienzo de reconocimiento. Usamos el rectángulo delimitador del trazo de tinta asociado en el lienzo de entrada de lápiz para establecer la posición y el tamaño de fuente de las formas.

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

Este es un ejemplo en acción:

| Antes del análisis | Después del análisis |
| --- | --- |
| ![Antes del análisis](images/ink/ink-analysis-raw2-small.png) | ![Después del análisis](images/ink/ink-analysis-analyzed2-small.png) |

## <a name="constrained-handwriting-recognition"></a>Reconocimiento de escritura a mano restringido

En la sección anterior ([reconocimiento de forma libre con análisis de tinta](#free-form-recognition-with-ink-analysis)), mostramos cómo usar las [API de análisis de tinta](/uwp/api/windows.ui.input.inking.analysis) para analizar y reconocer trazos de tinta arbitrarios dentro de un área de InkCanvas.

En esta sección, se muestra cómo usar el motor de reconocimiento de escritura a mano de lápiz de Windows (no el análisis de tinta) para convertir un conjunto de trazos de un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) a texto (según el paquete de idioma predeterminado instalado).

> [!NOTE]
> El reconocimiento de escritura a mano básico que se muestra en esta sección es más adecuado para escenarios de entrada de texto de una sola línea, como la entrada de formulario. Para escenarios de reconocimiento más completos que incluyen el análisis y la interpretación de la estructura del documento, los elementos de lista, las formas y los dibujos (además del reconocimiento de texto), vea la sección anterior: [reconocimiento de forma libre con análisis de tinta](#free-form-recognition-with-ink-analysis).

En este ejemplo, el reconocimiento se inicia cuando el usuario hace clic en un botón para indicar que ha terminado de escribir.

**Descargar este ejemplo de [ejemplo de reconocimiento de escritura a mano manuscrita](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)**

1. En primer lugar, debemos configurar la interfaz de usuario.

   La interfaz de usuario incluye un botón "Reconocer", el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) y un área de visualización de los resultados del reconocimiento.    

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

2. En este ejemplo, debe agregar primero las referencias de tipo de espacio de nombres necesarias para la funcionalidad de entrada de lápiz:
    - [Windows.UI.Input](/uwp/api/windows.ui.input)
    - [Windows.UI.Input.Inking](/uwp/api/windows.ui.input.inking)

3. A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

    El [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar los datos de entrada de lápiz y mouse como trazos de lápiz ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Los trazos de lápiz se representan en el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con los [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class) especificados. También se declara un agente de escucha para el evento clic en el botón "Reconocer".

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

4. Por último, realizamos el reconocimiento de escritura a mano básico. Para este ejemplo, usamos el controlador de eventos de clic del botón "Reconocer" para llevar a cabo el reconocimiento de escritura a mano.

   - Un [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) almacena todos los trazos de lápiz en un objeto [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Los trazos se exponen a través de la propiedad [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) del **InkPresenter** y se recuperan mediante el método [**GetStrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes). 

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - Se crea un [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) para administrar el proceso de reconocimiento de escritura a mano.

    ```csharp
    // Create a manager for the InkRecognizer object
        // used in handwriting recognition.
        InkRecognizerContainer inkRecognizerContainer =
            new InkRecognizerContainer();
    ```

    - Se llama a [**RecognizeAsync**](/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) para recuperar un conjunto de objetos [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) . Los resultados del reconocimiento se producen para cada palabra detectada por un [**InkRecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
        IReadOnlyList<InkRecognitionResult> recognitionResults =
            await inkRecognizerContainer.RecognizeAsync(
                inkCanvas.InkPresenter.StrokeContainer,
                InkRecognitionTarget.All);
    ```

    - Cada objeto [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) contiene un conjunto de candidatos de texto. El motor de reconocimiento tiene en cuenta el elemento de nivel superior de esta lista para que sea la mejor coincidencia, seguida de los candidatos restantes en orden de confianza decreciente.

       Recorremos en iteración cada [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) y compilamos la lista de candidatos. A continuación, se muestran los candidatos y se borra el [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) (lo que también borra el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

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

    - Este es el ejemplo del controlador de clics, en su totalidad.

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

El reconocimiento de escritura a mano integrado en la plataforma de Windows Ink incluye un amplio subconjunto de configuraciones regionales y idiomas compatibles con Windows.

Vea el tema de la propiedad [**InkRecognizer.Name**](/uwp/api/windows.ui.input.inking.inkrecognizer.name) para obtener una lista de los idiomas admitidos por [**InkRecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer) .

La aplicación puede consultar el conjunto de motores de reconocimiento de escritura a mano instalados y usar uno de ellos, o dejar que un usuario seleccione su idioma preferido.

**Nota:**    Los usuarios pueden ver una lista de los idiomas instalados en **configuración- &gt; tiempo & idioma**. Los idiomas instalados se enumeran en **Idiomas**.

Para instalar nuevos paquetes de idioma y habilitar el reconocimiento de escritura a mano para ese idioma:

1. Ve a **Configuración &gt; Hora e idioma &gt; Región e idioma**.
2. Selecciona **Agregar un idioma**.
3. Selecciona un idioma de la lista y, después, elige la versión de la región. El idioma aparece ahora en la página **Región e idioma**.
4. Haz clic en el idioma y selecciona **Opciones**.
5. En la página **Opciones de idioma**, descarga el **motor de reconocimiento de escritura a mano** (aquí también se pueden descargar el paquete de idioma completo, el motor de reconocimiento de voz y la distribución del teclado).

Aquí se muestra cómo usar el motor de reconocimiento de escritura a mano para interpretar un conjunto de trazos en un [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) según el reconocedor seleccionado.

Para iniciar el reconocimiento, el usuario debe hacer clic en un botón cuando termina de escribir a mano.

1. En primer lugar, debemos configurar la interfaz de usuario.

   La interfaz de usuario incluye un botón "Reconocer", un cuadro combinado que enumera todos los reconocedores de escritura a mano instalados, el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) y un área para visualizar los resultados del reconocimiento.

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

2. A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

   El [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) está configurado para interpretar los datos de entrada de lápiz y mouse como trazos de lápiz ([**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.inputdevicetypes)). Los trazos de lápiz se representan en el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) con los [**InkDrawingAttributes**](/windows/desktop/tablet/inkdrawingattributes-class) especificados.

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

3. Rellenamos el cuadro combinado del reconocedor con una lista de los reconocedores de escritura a mano instalados.

   Se crea un [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) para administrar el proceso de reconocimiento de escritura a mano. Usa este objeto para llamar a [**GetRecognizers**](/uwp/api/windows.ui.input.inking.inkrecognizercontainer.getrecognizers) y recuperar la lista de reconocedores instalados a fin de rellenar el cuadro combinado del reconocedor.

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
    
4. Actualiza el reconocedor de escritura a mano si la selección del cuadro combinado del reconocedor cambia.

   Usa el [**InkRecognizerContainer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizerContainer) para llamar a [**SetDefaultRecognizer**](/uwp/api/windows.ui.input.inking.inkrecognizercontainer.setdefaultrecognizer) según el reconocedor seleccionado en el cuadro combinado del reconocedor.

    ```csharp
    // Handle recognizer change.
        private void comboInstalledRecognizers_SelectionChanged(
            object sender, SelectionChangedEventArgs e)
        {
            inkRecognizerContainer.SetDefaultRecognizer(
                (InkRecognizer)comboInstalledRecognizers.SelectedItem);
        }
    ```

5. Por último, realizamos el reconocimiento de escritura a mano según el reconocedor de escritura a mano seleccionado. Para este ejemplo, usamos el controlador de eventos de clic del botón "Reconocer" para llevar a cabo el reconocimiento de escritura a mano.

   - Un [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) almacena todos los trazos de lápiz en un objeto [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer). Los trazos se exponen a través de la propiedad [**StrokeContainer**](/uwp/api/windows.ui.input.inking.inkpresenter.strokecontainer) del **InkPresenter** y se recuperan mediante el método [**GetStrokes**](/uwp/api/windows.ui.input.inking.inkstrokecontainer.getstrokes).

    ```csharp
    // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes =
            inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
    ```

    - Se llama a [**RecognizeAsync**](/uwp/api/windows.ui.input.inking.inkmanager.recognizeasync) para recuperar un conjunto de objetos [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) .

      Los resultados del reconocimiento se producen para cada palabra detectada por un [**InkRecognizer**](/uwp/api/Windows.UI.Input.Inking.InkRecognizer).

    ```csharp
    // Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
    ```

    - Cada objeto [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) contiene un conjunto de candidatos de texto. El motor de reconocimiento tiene en cuenta el elemento de nivel superior de esta lista para que sea la mejor coincidencia, seguida de los candidatos restantes en orden de confianza decreciente.

       Recorremos en iteración cada [**InkRecognitionResult**](/uwp/api/Windows.UI.Input.Inking.InkRecognitionResult) y compilamos la lista de candidatos. A continuación, se muestran los candidatos y se borra el [**InkStrokeContainer**](/uwp/api/Windows.UI.Input.Inking.InkStrokeContainer) (lo que también borra el [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)).

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

    - Este es el ejemplo del controlador de clics, en su totalidad.

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

Mientras que los dos ejemplos anteriores requieren que el usuario presione un botón para iniciar el reconocimiento, también puede realizar el reconocimiento dinámico mediante la entrada de trazo emparejada con una función de control de tiempo básica.

Para este ejemplo, usaremos la misma interfaz de usuario y la misma configuración de trazo que en el ejemplo de reconocimiento internacional anterior.

1. Estos objetos globales ([InkAnalyzer](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer), [InkStroke](/uwp/api/windows.ui.input.inking.inkstroke), [InkAnalysisResult](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult), [DispatcherTimer](/uwp/api/windows.ui.xaml.dispatchertimer)) se usan en nuestra aplicación.

    ```csharp
    // Stroke recognition globals.
    InkAnalyzer inkAnalyzer;
    DispatcherTimer recoTimer;
    ```

2. En lugar de un botón para iniciar el reconocimiento, agregamos agentes de escucha para dos eventos de trazo de lápiz del [**InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.inkpresenter) ([**StrokesCollected**](/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected) y [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)) y configuramos un temporizador básico ([**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer)) con un segundo intervalo de [**Tick**](/uwp/api/windows.ui.xaml.dispatchertimer.tick).

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

3. A continuación, se definen los controladores para los eventos de InkPresenter que se han declarado en el primer paso (también se invalida el evento de página [**OnNavigatingFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom) para administrar nuestro temporizador).

    - [**StrokesCollected**](/uwp/api/windows.ui.input.inking.inkpresenter.strokescollected)  
    Agregue trazos de entrada de lápiz ([**AddDataForStrokes**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.adddataforstrokes)) a InkAnalyzer e inicie el temporizador de reconocimiento cuando el usuario detenga la entrada manuscrita (levantando el lápiz o el dedo o soltando el botón del mouse). Después de un segundo sin ninguna entrada de lápiz, se inicia el reconocimiento.  

        Use el método [**SetStrokeDataKind**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.setstrokedatakind) para especificar si solo está interesado en el texto (incluidas las listas del procesador de la estructura de los documentos AMD) o solo en los dibujos (reconocimiento de la forma incluidos).

    - [**StrokeStarted**](/uwp/api/windows.ui.input.inking.inkstrokeinput.strokestarted)  
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

4. Por último, se realiza el reconocimiento de escritura a mano. Para este ejemplo, usamos el controlador de eventos de [**Graduación**](/uwp/api/windows.ui.xaml.dispatchertimer.tick) de un [**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer) para iniciar el reconocimiento de escritura a mano.
    - Llame a [**AnalyzeAsync**](/uwp/api/windows.ui.input.inking.analysis.inkanalyzer.AnalyzeAsync) para iniciar el análisis de tinta y obtener [**InkAnalysisResult**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult).
    - Si [**Estado**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisresult.Status) devuelve un estado de **actualizado**, llame a [**FindNodes**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisroot.findnodes) para los tipos de nodo de  [**InkAnalysisNodeKind. InkWord**](/uwp/api/windows.ui.input.inking.analysis.inkanalysisnodekind).
    - Recorrer en iteración los nodos y mostrar el texto reconocido.
    - Por último, elimine los nodos reconocidos del InkAnalyzer y los trazos de tinta correspondientes del lienzo de tinta.

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

- [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Ejemplos del tema

- [Ejemplo de análisis de tinta (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
- [Ejemplo de reconocimiento de escritura a mano manuscrita (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)

### <a name="other-samples"></a>Otras muestras

- [Ejemplo de entrada de lápiz simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Ejemplo de tinta compleja (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ejemplo de entrada de lápiz (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Tutorial de introducción: compatibilidad con la entrada de lápiz en la aplicación de Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Muestra de libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Muestra de notas familiares](https://github.com/Microsoft/Windows-appsample-familynotes)
