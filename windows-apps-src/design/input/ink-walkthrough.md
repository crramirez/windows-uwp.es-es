---
ms.assetid: ''
title: Compatibilidad con entrada de lápiz en la aplicación de Windows
description: Obtenga información sobre cómo admitir la escritura y el dibujo con Windows Ink en una aplicación básica de Plataforma universal de Windows (UWP) siguiendo este tutorial paso a paso.
keywords: tinta, entrada manuscrita, tutorial
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a0660312746a88a61ccb7b2ca9c01d720ebb2be3
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219698"
---
# <a name="tutorial-support-ink-in-your-windows-app"></a>Tutorial: compatibilidad con la entrada manuscrita en la aplicación de Windows

![Lápiz para Surface](images/ink/ink-hero-small.png)  
*Lápiz para Surface* (disponible para su compra en [Microsoft Store](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)).

En este tutorial se explica cómo crear una aplicación básica de Windows que admita la escritura y el dibujo con Windows Ink. Usamos fragmentos de código de una aplicación de ejemplo, que puede descargar de GitHub (consulte el [código de ejemplo](#sample-code)), para mostrar las distintas características y las API de Windows Ink asociadas (consulte [los componentes de la plataforma de tinta de Windows](#components-of-the-windows-ink-platform)) que se describen en cada paso.

Nos centramos en lo siguiente:
* Agregar compatibilidad básica con entradas manuscritas
* Agregar una barra de herramientas de entrada manuscrita
* Compatibilidad con el reconocimiento de escritura a mano
* Compatibilidad con el reconocimiento de forma básico
* Guardar y cargar entradas manuscritas

Para obtener más información sobre la implementación de estas características, consulte [interacciones de lápiz y Windows Ink en aplicaciones de Windows](./pen-and-stylus-interactions.md).

## <a name="introduction"></a>Introducción

Con Windows Ink, puede proporcionar a los clientes el equivalente digital de casi cualquier experiencia de lápiz y papel imaginable, desde anotaciones rápidas y manuscritas hasta demostraciones de pizarra y desde dibujos arquitectónicos y de ingeniería hasta obras maestras personales.

## <a name="prerequisites"></a>Requisitos previos

* Un equipo (o una máquina virtual) que ejecute la versión actual de Windows 10
* [Visual Studio 2019 y el SDK de RS2](https://developer.microsoft.com/windows/downloads)
* [SDK de Windows 10 (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* En función de la configuración, es posible que tenga que instalar el paquete de NuGet [Microsoft. NETCore. UniversalWindowsPlatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform) y habilitar el **modo de desarrollador** en la configuración del sistema (configuración-> actualización & seguridad-> para desarrolladores-> usar características de desarrollador).
* Si no está familiarizado con el desarrollo de aplicaciones de Windows con Visual Studio, consulte estos temas antes de empezar este tutorial:  
    * [Prepárate](../../get-started/get-set-up.md)
    * [Creación de una aplicación "Hello, world" (XAML)](../../get-started/create-a-hello-world-app-xaml-universal.md)
* **[Opcional]** Un lápiz digital y un equipo con una pantalla que admita la entrada de esa plumilla digital.

> [!NOTE] 
> Aunque Windows Ink puede ser compatible con el dibujo con un mouse y un toque (se muestra cómo hacer esto en el paso 3 de este tutorial) para una experiencia de Windows Ink óptima, se recomienda tener un lápiz digital y un equipo con una pantalla que admita la entrada de esa plumilla digital.

## <a name="sample-code"></a>Código de ejemplo
En este tutorial, usamos una aplicación de entrada de lápiz de ejemplo para mostrar los conceptos y la funcionalidad que se describen.

Descargue este ejemplo de Visual Studio y el código fuente de [GitHub](https://github.com/) en el [ejemplo Windows-appsample-Get-Started-Ink](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink):

1. Seleccione el botón de **clonación o descarga** de verde  
![Clonar el repositorio](images/ink/ink-clone.png)
2. Si tiene una cuenta de GitHub, puede clonar el repositorio en el equipo local eligiendo **abrir en Visual Studio** . 
3. Si no tiene una cuenta de GitHub, o simplemente quiere una copia local del proyecto, elija **download zip** (tendrá que comprobar de nuevo con regularidad para descargar las actualizaciones más recientes).

> [!IMPORTANT]
> La mayor parte del código del ejemplo se marca como comentario. A medida que avanzamos en cada paso, se le pedirá que quite las marcas de comentario de las distintas secciones del código. En Visual Studio, solo tiene que resaltar las líneas de código y presionar CTRL-K y, a continuación, CTRL + U.

## <a name="components-of-the-windows-ink-platform"></a>Componentes de la plataforma Windows Ink

Estos objetos proporcionan la mayor parte de la experiencia de entrada manuscrita para aplicaciones de Windows.

| Componente | Descripción |
| --- | --- |
| [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) | Un control de plataforma de interfaz de usuario XAML que, de manera predeterminada, recibe y muestra todas las entradas de lápiz como un trazo de lápiz o un trazo de borrado. |
| [**Objeto**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | Un objeto de código subyacente, cuya instancia se creó con un control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (expuesto a través de la propiedad [**InkCanvas.InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter)). Este objeto proporciona toda la funcionalidad de entrada manuscrita predeterminada expuesta por el [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas), junto con un conjunto completo de API para la personalización y personalización adicionales. |
| [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) | Control de plataforma de interfaz de usuario XAML que contiene una colección personalizable y extensible de botones que activan las características relacionadas con la tinta en un [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)asociado. |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)<br/>Aquí no tratamos esta funcionalidad. para obtener más información, consulte el [ejemplo de tinta compleja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk). | Habilita la representación de trazos de lápiz en el contexto de dispositivo de Direct2D designado de una aplicación universal de Windows, en lugar del control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) predeterminado. |

## <a name="step-1-run-the-sample"></a>Paso 1: ejecutar el ejemplo

Después de descargar la aplicación de ejemplo RadialController, compruebe que se ejecuta:
1. Abra el proyecto de ejemplo en Visual Studio.
2. Establezca la lista desplegable **plataformas de solución** en una selección que no sea de ARM.
3. Presione F5 para compilar, implementar y ejecutar.  

   > [!NOTE]
   > Como alternativa, puede seleccionar el elemento de menú **depurar**  >  **iniciar depuración** o seleccionar el botón ejecutar **equipo local** que se muestra aquí.
   > ![Botón compilar proyecto de Visual Studio](images/ink/ink-vsrun-small.png)

La ventana de la aplicación se abre y, después de que aparezca una pantalla de presentación durante unos segundos, verá esta pantalla inicial.

![Aplicación vacía](images/ink/ink-app-step1-empty-small.png)

Bien, ahora tenemos la aplicación básica de Windows que usaremos en el resto de este tutorial. En los pasos siguientes, agregamos la funcionalidad de tinta.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>Paso 2: usar InkCanvas para admitir la entrada de lápiz básica

Es posible que ya haya observado que la aplicación, en su forma inicial, no le permite dibujar nada con el lápiz (aunque puede usar el lápiz como un dispositivo de puntero estándar para interactuar con la aplicación). 

Vamos a corregir esa pequeña limitación en este paso.

Para agregar la funcionalidad básica de entrada manuscrita, solo tiene que colocar un control [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) en la página correspondiente de la aplicación.

> [!NOTE]
> Un InkCanvas tiene propiedades de [**alto**](/uwp/api/windows.ui.xaml.frameworkelement.Height) y [**ancho**](/uwp/api/windows.ui.xaml.frameworkelement.Width) predeterminadas de cero, a menos que sea el elemento secundario de un elemento que ajusta automáticamente el tamaño de sus elementos secundarios. 

### <a name="in-the-sample"></a>En el ejemplo:
1. Abre el archivo MainPage.xaml.cs.
2. Busque el código marcado con el título de este paso ("//Step 2: usar InkCanvas para admitir la entrada de lápiz básica").
3. Quite las marcas de comentario de las líneas siguientes. (Estas referencias son necesarias para la funcionalidad usada en los pasos siguientes).  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. Abra el archivo MainPage. Xaml.
5. Busque el código marcado con el título de este paso (" \<!-- Step 2: Basic inking with InkCanvas --> ").
6. Quite la marca de comentario de la línea siguiente.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

Eso es todo. 

Ahora, vuelva a ejecutar la aplicación. Avance y garabatear, escriba su nombre o (si está manteniendo un reflejo o tiene una memoria muy buena), dibuje su propio retrato.

![Entrada de lápiz básica](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>Paso 3: compatibilidad con la entrada manuscrita con Touch y mouse

Observará que, de forma predeterminada, las entradas manuscritas solo se admiten para la entrada manuscrita. Si intenta escribir o dibujar con el dedo, el mouse o el touchpad, estará decepcionado.

Para desactivar la desaprobación, debe agregar una segunda línea de código. Esta vez se encuentra en el código subyacente para el archivo XAML en el que declaró el [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas). 

En este paso, se presenta el objeto [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) , que proporciona una administración más específica de la entrada, el procesamiento y la representación de la entrada manuscrita (estándar y modificado) en el control [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas).

> [!NOTE]
> La entrada de lápiz estándar (sugerencia o botón de la punta del lápiz o del borrador) no se modifica con una prestación de hardware secundaria, como un botón de barril del lápiz, un botón secundario del mouse o un mecanismo similar. 

Para habilitar el mouse y la entrada táctil, establezca la propiedad [**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) del [**objeto InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) en la combinación de los valores de [**CoreInputDeviceTypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) que desee.

### <a name="in-the-sample"></a>En el ejemplo:
1. Abre el archivo MainPage.xaml.cs.
2. Busque el código marcado con el título de este paso ("//Step 3: support inking with touch and mouse").
3. Quite las marcas de comentario de las líneas siguientes.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

Vuelva a ejecutar la aplicación y verá que todos los sueños de dibujo con dedo se han puesto en funcionamiento.

> [!NOTE]
> Al especificar los tipos de dispositivo de entrada, debe indicar la compatibilidad para cada tipo de entrada específico (incluido el lápiz), ya que al establecer esta propiedad se invalida la configuración predeterminada de [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) .

## <a name="step-4-add-an-ink-toolbar"></a>Paso 4: agregar una barra de herramientas de entrada manuscrita

[**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) es un control de plataforma UWP que proporciona una colección personalizable y extensible de botones para activar características relacionadas con la tinta. 

De forma predeterminada, el control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) incluye un conjunto básico de botones que permiten a los usuarios seleccionar rápidamente entre un lápiz, un lápiz, un marcador de resaltado o un borrador, cualquiera de los cuales puede usarse junto con una galería de símbolos (regla o Protractor). Los botones de lápiz, lápiz y marcador de resaltado proporcionan también un control flotante para seleccionar el color de la tinta y el tamaño del trazo.

Para agregar un control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) predeterminado a una aplicación de entrada manuscrita, simplemente colóquelo en la misma página que el [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) y asocie los dos controles.

### <a name="in-the-sample"></a>En el ejemplo
1. Abra el archivo MainPage. Xaml.
2. Busque el código marcado con el título de este paso (" \<!-- Step 4: Add an ink toolbar --> ").
3. Quite las marcas de comentario de las líneas siguientes.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> Para mantener la interfaz de usuario y el código en la forma más sencilla y simple posible, usamos un diseño de cuadrícula básico y declaramos el control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) después del control [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) en una fila de la cuadrícula. Si se declara antes que el control [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas), el control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) se representa primero, debajo del lienzo e inaccesible para el usuario.  

Ahora vuelva a ejecutar la aplicación para ver el control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) y probar algunas de las herramientas.

![InkToolbar desde dibujo con el área de trabajo de tinta](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>Desafío: agregar un botón personalizado
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar desde dibujo en el área de trabajo de tinta](images/challenge-icon.png)

</td>
<td>

A continuación se muestra un ejemplo de un control **[InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar)** personalizado (de dibujo en el área de trabajo de Windows Ink).

![InkToolbar desde dibujo en el área de trabajo de tinta](images/ink/ink-inktoolbar-sketchpad-small.png)

Para obtener más información sobre cómo personalizar un control [inktoolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar), vea [Agregar un control inktoolbar a una aplicación de entrada de lápiz de la aplicación Windows](ink-toolbar.md).

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>Paso 5: compatibilidad con el reconocimiento de escritura a mano

Ahora que puede escribir y dibujar en la aplicación, vamos a intentar hacer algo útil con esos garabatos.

En este paso, se usan las características de reconocimiento de escritura a mano de Windows Ink para intentar descifrar lo que ha escrito.

> [!NOTE]
> Se puede mejorar el reconocimiento de escritura a mano a través del lápiz & la configuración de **Windows Ink** :
> 1. Abra el menú Inicio y seleccione **configuración**.
> 2. En la pantalla de configuración, seleccione **dispositivos**  >  **lápiz & Windows Ink**.
> ![InkToolbar desde dibujo en el área de trabajo de tinta](images/ink/ink-settings-small.png)
> 3. Seleccione **obtener para conocer la escritura a mano** y abrir el cuadro de diálogo de **Personalización de escritura a mano** .
> ![InkToolbar desde dibujo en el área de trabajo de tinta](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>En el ejemplo:
1. Abra el archivo MainPage. Xaml.
2. Busque el código marcado con el título de este paso (" \<!-- Step 5: Support handwriting recognition --> ").
3. Quite las marcas de comentario de las líneas siguientes.  

``` xaml
    <Button x:Name="recognizeText" 
            Content="Recognize text"  
            Grid.Row="0" Grid.Column="0"
            Margin="10,10,10,10"
            Click="recognizeText_ClickAsync"/>
    <TextBlock x:Name="recognitionResult" 
                Text="Recognition results: "
                VerticalAlignment="Center" 
                Grid.Row="0" Grid.Column="1"
                Margin="50,0,0,0" />
```

4. Abre el archivo MainPage.xaml.cs.
5. Busque el código marcado con el título de este paso ("paso 5: compatibilidad con el reconocimiento de escritura a mano").
6. Quite las marcas de comentario de las líneas siguientes.  

- Estas son las variables globales que se requieren para este paso.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- Este es el controlador del botón **reconocer texto** , donde hacemos el procesamiento de reconocimiento.

``` csharp
    private async void recognizeText_ClickAsync(object sender, RoutedEventArgs e)
    {
        strokesText = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (strokesText.Count > 0)
        {
            analyzerText.AddDataForStrokes(strokesText);

            resultText = await analyzerText.AnalyzeAsync();

            if (resultText.Status == InkAnalysisStatus.Updated)
            {
                words = analyzerText.AnalysisRoot.FindNodes(InkAnalysisNodeKind.InkWord);
                foreach (var word in words)
                {
                    InkAnalysisInkWord concreteWord = (InkAnalysisInkWord)word;
                    foreach (string s in concreteWord.TextAlternates)
                    {
                        recognitionResult.Text += s;
                    }
                }
            }
            analyzerText.ClearDataForAllStrokes();
        }
    }
```

7. Vuelva a ejecutar la aplicación, escriba algo y, a continuación, haga clic en el botón **reconocer texto**
8. Los resultados del reconocimiento se muestran al lado del botón

### <a name="challenge-1-international-recognition"></a>Desafío 1: reconocimiento internacional
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar desde dibujo en el área de trabajo de tinta](images/challenge-icon.png)

</td>
<td>

Windows Ink admite el reconocimiento de texto para muchos de los idiomas admitidos por Windows. Cada paquete de idioma incluye un motor de reconocimiento de escritura a mano que se puede instalar con el paquete de idioma.

Se dirige a un idioma específico mediante la consulta de los motores de reconocimiento de escritura a mano instalados.

Para obtener más información sobre el reconocimiento de escritura a mano internacional, consulte [reconocer trazos de Windows Ink como texto](convert-ink-to-text.md).

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>Desafío 2: reconocimiento dinámico
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar desde dibujo en el área de trabajo de tinta](images/challenge-icon.png)

</td>
<td>

En este tutorial, es necesario presionar un botón para iniciar el reconocimiento. También puede realizar el reconocimiento dinámico mediante una función de control de tiempo básica.

Para obtener más información sobre el reconocimiento dinámico, consulte [reconocer trazos de Windows Ink como texto](convert-ink-to-text.md).

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>Paso 6: reconocer formas

Bien, ahora puede convertir las notas manuscritas en algo un poco más legible. Pero, ¿qué ocurre con esos garabatos caffeinateds de la reunión anónima en los diagramas de flujo de la mañana?

Con el análisis de tinta, la aplicación también puede reconocer un conjunto de formas básicas, entre las que se incluyen:

- Circle
- Diamond
- Dibujo
- Ellipse
- EquilateralTriangle
- Tuerca
- IsoscelesTriangle
- Inclina
- Pentágono
- Cuadrangular
- Rectángulo
- RightTriangle
- Square
- Trapecio
- Triangle

En este paso, se usan las características de reconocimiento de formas de Windows Ink para intentar limpiar los garabatos.

En este ejemplo, no se intenta volver a dibujar trazos de lápiz (aunque es posible). En su lugar, se agrega un lienzo estándar en el InkCanvas en el que se dibujan objetos de elipse o polígono equivalentes derivados de la tinta original. A continuación, se eliminarán los trazos de tinta correspondientes.

### <a name="in-the-sample"></a>En el ejemplo:
1. Abra el archivo MainPage. Xaml.
2. Busque el código marcado con el título de este paso (" \<!-- Step 6: Recognize shapes --> ").
3. Quite la marca de comentario de esta línea.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. Abra el archivo MainPage.xaml.cs
5. Busque el código marcado con el título de este paso ("//Step 6: rerecognize Shapes").
6. Quite los comentarios de estas líneas:  

``` csharp
    private async void recognizeShape_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        ...
    }

    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        ...
    }
```

7. Ejecute la aplicación, dibuje algunas formas y haga clic en el botón **reconocer forma**

A continuación se muestra un ejemplo de un diagrama de flujo rudimentario de una servilleta digital.

![Diagrama de flujo de tinta original](images/ink/ink-app-step6-shapereco1-small.png)

Este es el mismo diagrama de flujo después del reconocimiento de la forma.

![Diagrama de flujo de tinta original](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>Paso 7: guardar y cargar la entrada de lápiz

Por lo tanto, ha acabado doodling y usted le gusta lo que ve, pero cree que le gustaría ajustar un par de cosas más adelante. Puede guardar los trazos de tinta en un archivo de formato serializado de tinta (ISF) y cargarlos para su edición siempre que se produce la inspiración. 

El archivo ISF es una imagen GIF básica que incluye metadatos adicionales que describen las propiedades y los comportamientos del trazo de tinta. Las aplicaciones que no están habilitadas para entrada manuscrita pueden omitir los metadatos adicionales y cargar la imagen GIF básica (incluida la transparencia de fondo de canal alfa).

En este paso, se enlazan los botones **Guardar** y **cargar** situados junto a la barra de herramientas de entrada manuscrita.

### <a name="in-the-sample"></a>En el ejemplo:
1. Abra el archivo MainPage. Xaml.
2. Busque el código marcado con el título de este paso (" \<!-- Step 7: Saving and loading ink --> ").
3. Quite las marcas de comentario de las líneas siguientes. 

``` xaml
    <Button x:Name="buttonSave" 
            Content="Save" 
            Click="buttonSave_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
    <Button x:Name="buttonLoad" 
            Content="Load"  
            Click="buttonLoad_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
```

4. Abre el archivo MainPage.xaml.cs.
5. Busque el código marcado con el título de este paso ("//Step 7: Save and upload Ink").
6. Quite las marcas de comentario de las líneas siguientes.  

``` csharp
    private async void buttonSave_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private async void buttonLoad_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }
```

7. Ejecute la aplicación y dibuje algo.
8. Seleccione el botón **Guardar** y guarde el dibujo.
9. Borre la tinta o reinicie la aplicación.
10. Seleccione el botón **cargar** y abra el archivo de entrada manuscrita que acaba de guardar.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>Desafío: usar el portapapeles para copiar y pegar trazos de lápiz 
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar desde dibujo en el área de trabajo de tinta](images/challenge-icon.png)

</td>

<td>

Windows Ink también admite copiar y pegar trazos de lápiz hacia y desde el portapapeles.

Para obtener más información acerca del uso del Portapapeles con entrada de lápiz, vea [almacenar y recuperar datos de trazo de Windows Ink](save-and-load-ink.md).

</td>
</tr>
</table>

## <a name="summary"></a>Resumen

Enhorabuena, ha completado la **entrada: compatibilidad con la entrada de lápiz en el tutorial de la aplicación de Windows** . Le mostramos el código básico necesario para admitir la entrada de lápiz en las aplicaciones de Windows y cómo proporcionar algunas de las experiencias de usuario más enriquecidas admitidas por la plataforma de Windows Ink.

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de lápiz y Windows Ink en aplicaciones de Windows](pen-and-stylus-interactions.md)

### <a name="samples"></a>Ejemplos

* [Ejemplo de análisis de tinta (Basic) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [Ejemplo de reconocimiento de escritura a mano manuscrita (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [Guardar y cargar trazos de entrada de lápiz desde un archivo de formato serializado de tinta (ISF)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [Guardar y cargar trazos de entrada de lápiz desde el portapapeles](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [Ejemplo de posición y orientación de la barra de herramientas de lápiz (básico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [Ejemplo de posición y orientación de la barra de herramientas de lápiz (dinámico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [Ejemplo de entrada de lápiz simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [Ejemplo de tinta compleja (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [Ejemplo de entrada de lápiz (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [Tutorial de introducción: compatibilidad con la entrada de lápiz en la aplicación de Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [Muestra de libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [Muestra de notas familiares](https://github.com/Microsoft/Windows-appsample-familynotes)
