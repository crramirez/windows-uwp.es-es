---
author: Karl-Bridge-Microsoft
Description: Convertir trazos de lápiz en texto mediante el reconocimiento de escritura a mano, o en formas mediante el reconocimiento personalizado.
title: Reconocer trazos de Windows Ink como texto
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Recognize Windows Ink strokes as text
template: detail.hbs
keyword: Windows Ink, Windows Inking, DirectInk, InkPresenter, InkCanvas, handwriting recognition
---

# Reconocer trazos de Windows Ink como texto

Convertir trazos de lápiz en texto mediante el reconocimiento de escritura a mano, o en formas mediante el reconocimiento personalizado.

**API importantes**

-   [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)
-   [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)


El reconocimiento de escritura a mano está integrado en la plataforma de entrada de lápiz de Windows, y admite un amplio conjunto de configuraciones regionales e idiomas.

Para todos los ejemplos que aquí se ofrecen, agrega las referencias de espacio de nombres necesarias para la funcionalidad de entrada de lápiz. Se incluye "Windows.UI.Input.Inking".

## <span id="Basic_handwriting_recognition"></span><span id="basic_handwriting_recognition"></span><span id="BASIC_HANDWRITING_RECOGNITION"></span>Reconocimiento de escritura a mano básico


Aquí se muestra cómo usar el motor de reconocimiento de escritura a mano, asociado con el paquete de idioma instalado de manera predeterminada, para interpretar un conjunto de trazos en un [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535).

Para iniciar el reconocimiento, el usuario debe hacer clic en un botón cuando termina de escribir a mano.

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

2.  A continuación, definimos algunos comportamientos de entrada de lápiz básicos.

    El [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada de lápiz y mouse como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)). Los trazos de lápiz se representan en el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) con los [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados. También se declara un agente de escucha para el evento clic en el botón "Reconocer".
```    CSharp
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

3.  Por último, realizamos el reconocimiento de escritura a mano básico. Para este ejemplo, usamos el controlador de eventos de clic del botón "Reconocer" para llevar a cabo el reconocimiento de escritura a mano.

    Un [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) almacena todos los trazos de lápiz en un objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Los trazos se exponen a través de la propiedad [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) del **InkPresenter** y se recuperan mediante el método [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    An [**InkRecognizerContainer**](https://msdn.microsoft.com/library/windows/apps/br208479) is created to manage the handwriting recognition process.
```    CSharp
// Create a manager for the InkRecognizer object 
    // used in handwriting recognition.
    InkRecognizerContainer inkRecognizerContainer = 
        new InkRecognizerContainer();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults = 
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer, 
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
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
```    CSharp
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

## <span id="International_recognition"></span><span id="international_recognition"></span><span id="INTERNATIONAL_RECOGNITION"></span>Reconocimiento internacional


Se puede usar un completo subconjunto de idiomas admitidos por Windows para el reconocimiento de escritura a mano.

En la tabla siguiente se enumeran los idiomas que admite el [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478). La primera columna contiene los valores posibles para [**Nombre**](https://msdn.microsoft.com/library/windows/apps/br208484).

| Nombre                                                            | Etiqueta de idioma de IETF | Cobertura                                                                      |
|-----------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------|
| Microsoft English (US) Handwriting Recognizer                   | en-US             | Inglés de Estados Unidos y Filipinas                                       |
| Microsoft English (UK) Handwriting Recognizer                   | en-GB             | Inglés de Gran Bretaña y todas las demás ubicaciones que no se especifican en esta tabla |
| Microsoft English (Canada) Handwriting Recognizer               | en-CA             | Inglés de Canadá                                                             |
| Microsoft English (Australia) Handwriting Recognizer            | en-AU             | Inglés de Australia                                                          |
| Microsoft-Handschrifterkennung - Deutsch                        | de-DE             | Alemán de Alemania, Austria, Luxemburgo y Namibia                           |
| Microsoft-Handschrifterkennung - Deutsch (Schweiz)              | de-CH             | Alemán de Suiza y Liechtenstein                                       |
| Reconocimiento de escritura a mano en español de Microsoft      | es-ES             | Español de España y todas las demás ubicaciones que no se especifican en esta tabla         |
| Reconocedor de escritura en Español (México) de Microsoft       | es-MX             | Español de México y Estados Unidos                                       |
| Reconocedor de escritura en Español (Argentina) de Microsoft    | es-AR             | Español de Argentina, Paraguay y Uruguay                                   |
| Reconnaissance d'écriture Microsoft - Français                  | fr                | Francés de Francia, Canadá, Bélgica, Suiza y todas las demás ubicaciones       |
| Microsoft 日本語手書き認識エンジン                              | ja                | Japonés de todas las ubicaciones                                                     |
| Riconoscimento grafia italiana Microsoft                        | it                | Italiano de Italia, Suiza y todas las demás ubicaciones                        |
| Microsoft Nederlandstalige handschriftherkenning                | nl-NL             | Neerlandés de los Países Bajos, Surinam y Antillas                           |
| Microsoft Nederlandstalige (België) handschriftherkenning       | nl-BE             | Neerlandés de Bélgica (flamenco)                                                    |
| Microsoft 中文(简体)手写识别器                                  | zh                | Chino simplificado                                                  |
| Microsoft 中文(繁體)手寫辨識器                                  | zh-Hant           | Chino tradicional                                                 |
| Microsoft система распознавания русского рукописного ввода      | ru                | Ruso de todas las ubicaciones                                                      |
| Reconhecedor de Manuscrito da Microsoft para Português (Brasil) | pt-BR             | Portugués de Brasil                                                          |
| Reconhecedor de escrita manual da Microsoft para português      | pt-PT             | Portugués de Portugal y todas las demás ubicaciones                               |
| Microsoft 한글 필기 인식기                                      | ko                | Coreano de todas las ubicaciones                                                       |
| System rozpoznawania polskiego pisma odręcznego firmy Microsoft | pl                | Polaco de todas las ubicaciones                                                       |
| Microsoft Handskriftstolk för svenska                           | sv                | Sueco de todas las ubicaciones                                                      |
| Microsoft rozpoznávač rukopisu pro český jazyk                  | cs                | Checo de todas las ubicaciones                                                        |
| Microsoft Genkendelse af dansk håndskrift                       | da                | Danés de todas las ubicaciones                                                       |
| Microsoft Håndskriftsgjenkjenner for norsk                      | nb                | Noruego (bokmål) de todas las ubicaciones                                           |
| Microsoft Håndskriftsgjenkjenner for nynorsk                    | nn                | Noruego (nynorsk) de todas las ubicaciones                                          |
| Microsoftin suomenkielinen käsinkirjoituksen tunnistus          | fi                | Finés de todas las ubicaciones                                                      |
| Microsoft recunoaştere grafie - Română                          | ro                | Rumano de todas las ubicaciones                                                     |
| Microsoftov hrvatski rukopisni prepoznavač                      | hr                | Croata de todas las ubicaciones                                                     |
| Microsoft prepoznavač rukopisa za srpski (latinica)             | sr-Latn           | Serbio en latino de todas las ubicaciones                                           |
| Microsoft препознавач рукописа за српски (ћирилица)             | sr                | Serbio en cirílico de todas las ubicaciones                                        |
| Reconeixedor d'escriptura manual en català de Microsoft         | ca                | Catalán de todas las ubicaciones                                                      |

 

La aplicación puede consultar el conjunto de los motores de reconocimiento de escritura a mano instalados y usar uno de estos o permitir que un usuario seleccione su idioma preferido.

**Nota**  
Los usuarios pueden ver una lista de los idiomas instalados desde Configuración -&gt; Hora e idioma. Los idiomas instalados se enumeran en Idiomas.

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
```    CSharp
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
```    CSharp
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
```    CSharp
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
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = 
        inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
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
```    CSharp
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

## <span id="Dynamic_handwriting_recognition"></span><span id="dynamic_handwriting_recognition"></span><span id="DYNAMIC_HANDWRITING_RECOGNITION"></span>Reconocimiento de escritura a mano dinámico


Los dos ejemplos anteriores requieren que el usuario presione un botón para iniciar el reconocimiento. La aplicación también puede realizar el reconocimiento dinámico mediante la entrada de trazos combinada con una función de temporización básica.

Para este ejemplo, usaremos la misma interfaz de usuario y la misma configuración de trazo que en el ejemplo de reconocimiento internacional anterior.

1.  Como en los ejemplos anteriores, el [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) está configurado para interpretar los datos de entrada de lápiz y mouse como trazos de lápiz ([**InputDeviceTypes**](https://msdn.microsoft.com/library/windows/apps/dn922019)), y los trazos de lápiz se representan en el [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) con los [**InkDrawingAttributes**](https://msdn.microsoft.com/library/windows/desktop/ms695050) especificados.

    En lugar de un botón para iniciar el reconocimiento, agregamos agentes de escucha para dos eventos de trazo de lápiz del [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) ([**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024) y [**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)) y configuramos un temporizador básico ([**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250)) con un segundo intervalo de [**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256).    
```    CSharp
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

        // Listen for stroke events on the InkPresenter to 
        // enable dynamic recognition.
        // StrokesCollected is fired when the user stops inking by 
        // lifting their pen or finger, or releasing the mouse button.
        inkCanvas.InkPresenter.StrokesCollected += 
            inkCanvas_StrokesCollected;
        // StrokeStarted is fired when ink input is first detected.
        inkCanvas.InkPresenter.StrokeInput.StrokeStarted += 
            inkCanvas_StrokeStarted;

        // Timer to manage dynamic recognition.
        recoTimer = new DispatcherTimer();
        recoTimer.Interval = new TimeSpan(0, 0, 1);
        recoTimer.Tick += recoTimer_Tick;
    }

    // Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }    
```

2.  Estos son los controladores de los tres eventos que hemos agregado en el primer paso.

    <span id="StrokesCollected"></span><span id="strokescollected"></span><span id="STROKESCOLLECTED"></span>[**StrokesCollected**](https://msdn.microsoft.com/library/windows/apps/dn922024)  
    Inicia el temporizador de reconocimiento cuando el usuario detenga la entrada manuscrita levantando el lápiz o el dedo, o bien soltando el botón del mouse. Después de un segundo sin ninguna entrada de lápiz, se inicia el reconocimiento.

    <span id="StrokeStarted"></span><span id="strokestarted"></span><span id="STROKESTARTED"></span>[**StrokeStarted**](https://msdn.microsoft.com/library/windows/apps/dn914702)  
    Si se inicia un nuevo trazo antes del siguiente evento de graduación del temporizador, detén el temporizador, ya que el nuevo trazo es probablemente la continuación de una sola entrada de escritura a mano.

    <span id="Tick"></span><span id="tick"></span><span id="TICK"></span>[**Tick**](https://msdn.microsoft.com/library/windows/apps/br244256)  
    Llama a la función de reconocimiento después de un segundo sin entrada de lápiz.
```    CSharp
// Handler for the timer tick event calls the recognition function.
    private void recoTimer_Tick(object sender, object e)
    {
        Recognize_Tick();
    }

    // Handler for the InkPresenter StrokeStarted event.
    // If a new stroke starts before the next timer tick event,
    // stop the timer as the new stroke is likely the continuation
    // of a single handwriting entry.
    private void inkCanvas_StrokeStarted(InkStrokeInput sender, PointerEventArgs args)
    {
        recoTimer.Stop();
    }

    // Handler for the InkPresenter StrokesCollected event.
    // Start the recognition timer when the user stops inking by 
    // lifting their pen or finger, or releasing the mouse button.
    // After one second of no ink input, recognition is initiated.
    private void inkCanvas_StrokesCollected(InkPresenter sender, InkStrokesCollectedEventArgs args)
    {
        recoTimer.Start();
    }
```

3.  Por último, realizamos el reconocimiento de escritura a mano según el reconocedor de escritura a mano seleccionado. Para este ejemplo, usamos el controlador de eventos de [**Graduación**](https://msdn.microsoft.com/library/windows/apps/br244256) de un [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) para iniciar el reconocimiento de escritura a mano.

    Un [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn899081) almacena todos los trazos de lápiz en un objeto [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492). Los trazos se exponen a través de la propiedad [**StrokeContainer**](https://msdn.microsoft.com/library/windows/apps/dn948766) del **InkPresenter** y se recuperan mediante el método [**GetStrokes**](https://msdn.microsoft.com/library/windows/apps/br208499).
```    CSharp
// Get all strokes on the InkCanvas.
    IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
```

    [**RecognizeAsync**](https://msdn.microsoft.com/library/windows/apps/br208446) is called to retrieve a set of [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) objects.

    Recognition results are produced for each word that is detected by an [**InkRecognizer**](https://msdn.microsoft.com/library/windows/apps/br208478).
```    CSharp
// Recognize all ink strokes on the ink canvas.
    IReadOnlyList<InkRecognitionResult> recognitionResults =
        await inkRecognizerContainer.RecognizeAsync(
            inkCanvas.InkPresenter.StrokeContainer,
            InkRecognitionTarget.All);
```

    Each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) object contains a set of text candidates. The topmost item in this list is considered by the recognition engine to be the best match, followed by the remaining candidates in order of decreasing confidence.

    We iterate through each [**InkRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/br208464) and compile the list of candidates. The candidates are then displayed and the [**InkStrokeContainer**](https://msdn.microsoft.com/library/windows/apps/br208492) is cleared (which also clears the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535)).
```    CSharp
string str = "Recognition result\n";
    // Iterate through the recognition results.
    foreach (InkRecognitionResult result in recognitionResults)
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

    Here's the recognition function, in full.
```    CSharp
// Respond to timer Tick and initiate recognition.
    private async void Recognize_Tick()
    {
        // Get all strokes on the InkCanvas.
        IReadOnlyList<InkStroke> currentStrokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();

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

        // Stop the dynamic recognition timer.
        recoTimer.Stop();
    }
```

## <span id="related_topics"></span>Artículos relacionados

* [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md)

**Ejemplos**
* [Muestra de entrada de lápiz](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Muestra de entrada de lápiz simple](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Muestra de entrada de lápiz compleja](http://go.microsoft.com/fwlink/p/?LinkID=620314)
 

 






<!--HONumber=May16_HO2-->


