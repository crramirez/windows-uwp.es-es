---
author: Karl-Bridge-Microsoft
ms.assetid: ''
title: Compatibilidad con la entrada de lápiz en tu aplicación para UWP
description: Un tutorial paso a paso para agregar compatibilidad con la entrada de lápiz a tu aplicación para UWP.
keywords: entrada de lápiz, entrada manuscrita, tutorial
ms.author: kbridge
ms.date: 01/25/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: cac6caf7f8feab86103e27d7947209bb3fd5c0a8
ms.sourcegitcommit: 346b5c9298a6e9e78acf05944bfe13624ea7062e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2018
ms.locfileid: "1707100"
---
# <a name="tutorial-support-ink-in-your-uwp-app"></a>Tutorial: compatibilidad con la entrada de lápiz en tu aplicación para UWP

![Lápiz para Surface](images/ink/ink-hero-small.png)  
*Lápiz para Surface* (disponible para su compra en [Microsoft Store](https://aka.ms/purchasesurfacepen)).

En este tutorial se explica cómo crear una aplicación básica para la Plataforma universal de Windows (UWP) compatible con la escritura y dibujo con Windows Ink. Usamos fragmentos de código de una aplicación de muestra, que puedes descargar de GitHub (consulta [Código de muestra](#sample-code)), para mostrar las diferentes características y las API asociadas de Windows Ink (consulta [Componentes de la plataforma Windows Ink](#components-of-the-windows-ink-platform)) que se tratan en cada paso.

Nos centraremos en lo siguiente:
* Adición de compatibilidad con la entrada de lápiz básica
* Adición de una barra de herramientas de la entrada de lápiz
* Compatibilidad con el reconocimiento de escritura a mano
* Compatibilidad con el reconocimiento de formas básicas
* Guardar y cargar la entrada de lápiz

Para más información sobre cómo implementar estas características, consulta [Interacciones de lápiz y Windows Ink en aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/input/pen-and-stylus-interactions).

## <a name="introduction"></a>Introducción

Con Windows Ink, puedes proporcionar a tus clientes el equivalente digital de casi cualquier experiencia de lápiz y papel imaginable, desde notas y anotaciones rápidas, manuscritas, a demostraciones en pizarra interactiva y desde dibujos de arquitectura e ingeniería a obras maestras personales.

## <a name="prerequisites"></a>Requisitos previos

* Un equipo (o máquina virtual) que ejecute la versión actual de Windows 10.
* [Visual Studio 2017 y RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk)
* Si no estás familiarizado con el desarrollo de aplicaciones para la Plataforma universal de Windows (UWP) con Visual Studio, echa un vistazo a estos temas antes de empezar este tutorial:  
    * [Preparación](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)
    * [Crear una aplicación "Hola mundo" (XAML)](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)
* **[OPCIONAL]** Un lápiz digital y un equipo con una pantalla compatible con la entrada de lápiz digital.

> [!NOTE] 
> Aunque Windows Ink sea compatible con el dibujo con mouse y la función táctil (te mostramos cómo hacerlo en el paso 3 de este tutorial), para una experiencia óptima de Windows Ink, te recomendamos un lápiz digital y un equipo con pantalla compatible con la entrada de lápiz digital.

## <a name="sample-code"></a>Código de muestra
En este tutorial, usamos una aplicación de muestra de entrada de lápiz para mostrar los conceptos y la funcionalidad analizada.

Descarga este código de muestra y origen de Visual Studio de [GitHub](https://github.com/) en [windows-appsample-get-started-ink sample](https://aka.ms/appsample-ink):

1. Selecciona el botón verde **Clone or download**.  
![Clonar el repositorio](images/ink/ink-clone.png)
2. Si tienes una cuenta de GitHub, puedes clonar el repositorio en el equipo local seleccionando **Abrir en Visual Studio**. 
3. Si no tienes una cuenta de GitHub, o solo quieres una copia local del proyecto, elige **Download ZIP** (tendrás que irlo comprobando con regularidad para descargar las actualizaciones más recientes).

> [!IMPORTANT]
> La mayoría del código de la muestra está comentado. Cuando sigamos cada paso de este tema, se te pedirá que quites los comentarios de diversas secciones del código. En Visual Studio, simplemente resalta las líneas de código y presiona CTRL-K y, a continuación, CTRL-U.

## <a name="components-of-the-windows-ink-platform"></a>Componentes de la plataforma Windows Ink

Estos objetos proporcionan la mayor parte de la experiencia de la entrada de lápiz para las aplicaciones para UWP.

| Componente | Descripción |
| --- | --- |
| [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) | Un control de plataforma de interfaz de usuario XAML que, de manera predeterminada, recibe y muestra todas las entradas de lápiz como un trazo de lápiz o un trazo de borrado. |
| [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) | Un objeto de código subyacente, cuya instancia se creó con un control [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) (expuesto a través de la propiedad [**InkCanvas.InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter)). Este objeto proporciona todas las funcionalidades de entrada de lápiz predeterminadas expuestas por [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), junto con un completo conjunto de API para la personalización adicional. |
| [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) | Un control de la plataforma de interfaz de usuario XAML que contiene una colección personalizable y ampliable de botones que activan características relacionadas con la entrada de lápiz en un control [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) asociado. |
| [**IInkD2DRenderer**](https://msdn.microsoft.com/library/mt147263)<br/>Esta funcionalidad no se explica aquí, para más información, consulta [Muestra de entrada de lápiz compleja](http://go.microsoft.com/fwlink/p/?LinkID=620314). | Habilita la representación de trazos de lápiz en el contexto de dispositivo de Direct2D designado de una aplicación universal de Windows, en lugar del control [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) predeterminado. |

## <a name="step-1-run-the-sample"></a>Paso 1: ejecutar la muestra

Después de descargar la aplicación de muestra RadialController, comprueba que se ejecuta:
1. Abre el proyecto de muestra en Visual Studio.
2. Establece la lista desplegable **Plataformas de solución** en una selección que no sea ARM.
3. Presiona F5 para compilar, implementar y ejecutar.  

   > [!NOTE]
   > Como alternativa, puedes seleccionar el elemento de menú **Depurar** > **Iniciar depuración** o seleccionar el botón de ejecución del **Equipo Local** que se muestra aquí.
   > ![Botón Proyecto de compilación de Visual Studio](images/ink/ink-vsrun-small.png)

Se abre la ventana de la aplicación y, después de que aparezca una pantalla de presentación durante unos segundos, verás esta pantalla inicial.

![Aplicación vacía](images/ink/ink-app-step1-empty-small.png)

Bien, ahora tenemos la aplicación para UWP básica que usaremos en el resto de este tutorial. En los pasos siguientes, agregaremos la funcionalidad de entrada de lápiz.

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>Paso 2: usar InkCanvas para la compatibilidad con la entrada de lápiz básica

Probablemente ya hayas observado que la aplicación, en su forma inicial, no te permite dibujar nada con el lápiz (aunque puedes usarlo como dispositivo señalador estándar para interactuar con la aplicación). 

Vamos a corregir esta pequeña limitación en este paso.

Para agregar la funcionalidad de entrada de lápiz básica, solo debes colocar un control [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) de plataforma UWP en la página adecuada de la aplicación.

> [!NOTE]
> Un control InkCanvas tiene el valor predeterminado cero para las propiedades [**Height**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Height) y [**Width**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.Width), a menos que se trate de un elemento secundario de un elemento que, de manera automática, cambie el tamaño de sus elementos secundarios. 

### <a name="in-the-sample"></a>En la muestra:
1. Abre el archivo MainPage.xaml.cs.
2. Busca el código marcado con el título "// Step 2: Use InkCanvas to support basic inking".
3. Quita las marcas de comentario de las siguientes líneas. (Estas referencias son necesarias para la funcionalidad que se usa en los pasos posteriores).  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. Abre el archivo MainPage.xaml.
5. Busca el código marcado con el título "\<!-- Step 2: Basic inking with InkCanvas -->".
6. Quita la marca de comentario de la siguiente línea.  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

Eso es todo. 

Ahora vuelve a ejecutar la aplicación. Adelante, garabatea, escribe tu nombre o, si sostienes un espejo o tienes buena memoria, dibuja tu retrato.

![Entrada de lápiz básica](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>Paso 3: compatibilidad con la entrada de lápiz de la función táctil y el mouse

Verás que, de manera predeterminada, la entrada de lápiz solo es compatible con el lápiz. Si intentas escribir o dibujar con el dedo, el mouse o el panel táctil, te decepcionarás.

Para darle la vuelta a esto, tienes que agregar una segunda línea de código. Esta vez será en el código subyacente para el archivo XAML en el que declaraste el control [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas). 

En este paso, se introduce el objeto [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter), que proporciona la administración precisa de la entrada, procesamiento y representación de la entrada de lápiz (estándar o modificada) en tu [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).

> [!NOTE]
> La entrada de lápiz estándar (punta del lápiz o punta/botón del borrador) no se modifica con un elemento de hardware secundario, como un botón del lápiz, el botón derecho del mouse o un mecanismo similar. 

Para habilitar la entrada de lápiz con el ratón y la función táctil, establece la propiedad [**InputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) de [**InkPresenter**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.input.inking.inkpresenter) en la combinación de valores de [**CoreInputDeviceTypes**](https://docs.microsoft.com/uwp/api/windows.ui.core.coreinputdevicetypes) que quieras.

### <a name="in-the-sample"></a>En la muestra:
1. Abre el archivo MainPage.xaml.cs.
2. Busca el código marcado con el título "// Step 3: Support inking with touch and mouse".
3. Quita las marcas de comentario de las siguientes líneas.  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

Vuelve a ejecutar la aplicación y verás que todos tus sueños sobre dibujar con el dedo en una pantalla de ordenador se han hecho realidad.

> [!NOTE]
> Al especificar los tipos de dispositivo de entrada, debes indicar la compatibilidad para cada tipo de entrada (incluido el lápiz), ya que al establecer esta propiedad se reemplaza la configuración predeterminada del control [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas).

## <a name="step-4-add-an-ink-toolbar"></a>Paso 4: agregar una barra de herramientas de entrada de lápiz

El control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) es un control de la plataforma UWP que proporciona una colección personalizable y extensible de botones para activar las características relacionadas con la entrada de lápiz. 

De manera predeterminada, el control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) incluye un conjunto básico de botones que permiten a los usuarios seleccionar rápidamente entre un lápiz, un marcador de resaltado o un borrador, cualquiera de los cuales puede usarse junto con una galería de símbolos (regla o transportador). Los botones de marcador de resaltado y lápiz también proporcionan un control flotante para seleccionar el color y el tamaño del trazo de lápiz.

Para agregar un control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) predeterminado a una aplicación de entrada de lápiz, solo tienes que colocarlo en la misma página que el control [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) y asociar los dos controles.

### <a name="in-the-sample"></a>En la muestra
1. Abre el archivo MainPage.xaml.
2. Busca el código marcado con el título "\<!-- Step 4: Add an ink toolbar -->".
3. Quita las marcas de comentario de las siguientes líneas.  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> Para mantener la interfaz de usuario y el código lo más despejados y sencillos posible, se usa un diseño de cuadrícula básico y se declara el control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) después del control [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) en una fila de cuadrícula. Si lo declaras antes del control [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), el control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) se representa en primer lugar, debajo del lienzo y es inaccesible para el usuario.  

Ahora ejecuta la aplicación otra vez para ver el control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) y probar algunas de las herramientas.

![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>Desafío: agregar un botón personalizado
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/challenge-icon.png)

</td>
<td>

Este es un ejemplo de un control <strong><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar">InkToolbar</a></strong> personalizado (del Bloc de bocetos del Área de trabajo de Windows Ink).

![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/ink/ink-inktoolbar-sketchpad-small.png)

Para los detalles sobre cómo personalizar el control [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), consulta [Agregar un control InkToolbar a una aplicación de entrada manuscrita para la Plataforma universal de Windows (UWP)](https://docs.microsoft.com/en-us/windows/uwp/input/ink-toolbar).

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>Paso 5: compatibilidad con el reconocimiento de escritura a mano

Ahora que puedes escribir y dibujar en la aplicación, vamos a probar a hacer algo útil con esos garabatos.

En este paso, usamos las características del reconocimiento de escritura a mano de Windows Ink para intentar descifrar lo que has escrito.

> [!NOTE]
> Se puede mejorar el reconocimiento de escritura a mano a través de la configuración de **Lápiz y Windows Ink**:
> 1. Abre el menú Inicio y selecciona **Configuración**.
> 2. En la pantalla de configuración selecciona **Dispositivos** > **Lápiz y Windows Ink**.
> ![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/ink/ink-settings-small.png)
> 3. Selecciona **Reconocer mi escritura a mano** para abrir el cuadro de diálogo **Personalización de escritura a mano**.
> ![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>En la muestra:
1. Abre el archivo MainPage.xaml.
2. Busca el código marcado con el título "\<!-- Step 5: Support handwriting recognition -->".
3. Quita las marcas de comentario de las siguientes líneas.  

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
5. Busca el código marcado con el título " Step 5: Support handwriting recognition".
6. Quita las marcas de comentario de las siguientes líneas.  

- Estas son las variables globales necesarias para este paso.

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- Este es el controlador para el botón **Recognize text**, donde haremos el procesamiento del reconocimiento.

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

7. Vuelve a ejecutar la aplicación, escribe algo y, a continuación, haz clic en el botón **Recognize text**.
8. Los resultados del reconocimiento se muestran al lado del botón.

### <a name="challenge-1-international-recognition"></a>Desafío 1: reconocimiento internacional
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/challenge-icon.png)

</td>
<td>

<p>Windows Ink es compatible con el reconocimiento de texto en muchos de los idiomas compatibles con Windows. Cada paquete de idioma incluye un motor de reconocimiento de escritura a mano que se puede instalar con el paquete de idioma.</p>

<p>Dirígete a un idioma específico consultando los motores de reconocimiento de escritura a mano instalados.</p>

<p>Para más información sobre el reconocimiento de escritura a mano internacional, consulta <a href="https://docs.microsoft.com/windows/uwp/input/convert-ink-to-text">Reconocimiento de trazos de Windows Ink como texto</a>.</p>

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>Desafío 2: reconocimiento dinámico
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/challenge-icon.png)

</td>
<td>

<p>Para este tutorial, es necesario presionar un botón para iniciar el reconocimiento. También puedes realizar el reconocimiento dinámico mediante el uso de una función de temporización básica.</p>

<p>Para más información sobre el reconocimiento dinámico, consulta <a href="https://docs.microsoft.com/windows/uwp/input/convert-ink-to-text">Reconocimiento de trazos de Windows Ink como texto</a>.</p>

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>Paso 6: reconocer formas

De acuerdo, ahora puedes convertir las notas manuscritas en algo un poco más legible. Pero ¿qué pasa con esos garabatos temblorosos por la cafeína de la reunión matinal sobre el diagrama de flujo anónimo?

Con el análisis de la entrada de lápiz, la aplicación también puede reconocer un conjunto de formas, entre las que se incluyen las siguientes:
- Círculo
- Rombo
- Dibujo
- Elipse
- Triángulo equilátero
- Hexágono
- Triángulo isósceles
- Paralelogramo
- Pentágono
- Cuadrilátero
- Rectángulo
- Triángulo rectángulo
- Cuadrado
- Trapezoide
- Triángulo

En este paso, usamos las características de reconocimiento de formas de Windows Ink para intentar organizar tus garabatos.

En este ejemplo, no intentamos volver a dibujar trazos de lápiz (aunque es posible). En su lugar, agregamos un lienzo estándar debajo de InkCanvas donde dibujamos objetos elipse o polígono equivalentes derivados de la entrada de lápiz original. A continuación, borramos los trazos de lápiz correspondientes.

### <a name="in-the-sample"></a>En la muestra:
1. Abre el archivo MainPage.xaml.
2. Busca el código marcado con el título "\<!-- Step 6: Recognize shapes -->".
3. Quita la marca de comentario de esta línea.  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. Abre el archivo MainPage.xaml.cs.
5. Busca el código marcado con el título "// Step 6: Recognize shapes".
6. Quita las marcas de comentario de estas líneas:  

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

7. Ejecuta la aplicación, dibuja algunas formas y haz clic en el botón **Recognize shape**.

Este es un ejemplo de un diagrama de flujo rudimentario de una servilleta digital.

![Diagrama de flujo de entrada de lápiz original](images/ink/ink-app-step6-shapereco1-small.png)

Este es el mismo diagrama de flujo después del reconocimiento de formas.

![Diagrama de flujo de entrada de lápiz original](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>Paso 7: guardar y cargar la entrada de lápiz

¿Has terminado garabatear y te gusta cómo queda, pero piensas que más adelante puede que retoques un par de cosas? Puedes guardar los trazos de entrada de lápiz en un archivo ISF (Ink Serialized Format) y cargarlos para edición cuando te llegue la inspiración. 

El archivo ISF es una imagen GIF básica que incluye metadatos adicionales que describen los comportamientos y propiedades del trazo de lápiz. Las aplicaciones que no están habilitadas para la entrada de lápiz pueden omitir los metadatos adicionales y cargar la imagen GIF básica (incluida la transparencia del fondo del canal alfa).

En este paso, enlazamos los botones **Guardar** y **Cargar** situados junto a la barra de herramientas de entrada de lápiz.

### <a name="in-the-sample"></a>En la muestra:
1. Abre el archivo MainPage.xaml.
2. Busca el código marcado con el título "\<!-- Step 7: Saving and loading ink -->".
3. Quita las marcas de comentario de las siguientes líneas. 

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
5. Busca el código marcado con el título "// Step 7: Save and load ink".
6. Quita las marcas de comentario de las siguientes líneas.  

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

7. Ejecuta la aplicación y dibuja algo.
8. Selecciona el botón **Guardar** y guarda el dibujo.
9. Borra la entrada de lápiz o reinicia la aplicación.
10. Selecciona el botón **Cargar** y abre el archivo de entrada de lápiz que acabas de guardar.

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>Desafío: usar el Portapapeles para copiar y pegar trazos de lápiz 
<table class="wdg-noborder">
<tr>
<td>

![InkToolbar del Bloc de bocetos del área de trabajo de entrada de lápiz](images/challenge-icon.png)

</td>

<td>

<p>Windows Ink también es compatible con copiar y pegar trazos de lápiz en y desde el Portapapeles.</p>

<p>Para más información sobre cómo usar el Portapapeles con la entrada de lápiz, consulta <a href="https://docs.microsoft.com/en-us/windows/uwp/input/save-and-load-ink">Almacenar y recuperar datos de trazos de lápiz de Windows Ink</a>.</p>

</td>
</tr>
</table>

## <a name="summary"></a>Resumen

Enhorabuena, has completado el tutorial **Entrada: compatibilidad con la entrada de lápiz en tu aplicación para UWP**. Te hemos mostrado el código básico necesario para la compatibilidad con la entrada de lápiz en tus aplicaciones para UWP y cómo ofrecer algunas de las mejores experiencias de usuario compatibles con la plataforma Windows Ink.

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de lápiz y Windows Ink en aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/input/pen-and-stylus-interactions)

**Muestras**
* [Muestra de entrada de lápiz simple (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Muestra de entrada de lápiz compleja (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Muestra de entrada de lápiz (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Tutorial de introducción: Admitir la entrada de lápiz en tu aplicación para UWP](https://aka.ms/appsample-ink)
* [Muestra de libro para colorear](https://aka.ms/cpubsample-coloringbook)
* [Muestra de notas familiares](https://aka.ms/cpubsample-familynotessample)
