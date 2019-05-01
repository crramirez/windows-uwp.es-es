---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pinceles de composición
description: Un pincel pinta el área de un objeto Visual con su salida. Distintos pinceles tienen tipos de salida diferentes.
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d51bc945c721ae15889dece8f84959f9a78192bc
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63790531"
---
# <a name="composition-brushes"></a>Pinceles de composición
Todos los elementos visibles en la pantalla de una aplicación para UWP están visibles porque se pintaron con un pincel. Los pinceles te permiten pintar objetos de la interfaz de usuario (IU) con contenido que abarca desde colores sencillos y sólidos, imágenes o dibujos hasta una cadena de efectos complejos. En este tema se presentan los conceptos de pintura con CompositionBrush.

Ten en cuenta que al trabajar con una aplicación para UWP XAML, puedes elegir pintar un UIElement con un [XAML Brush](/windows/uwp/design/style/brushes) o [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush). Por lo general, es más fácil y recomendable elegir un pincel XAML si tu escenario es compatible con XAML Brush. Por ejemplo, animar el color de un botón, cambiar el relleno de un texto o una forma con una imagen. Por otro lado, si está intentando hacer algo que no es compatible con un pincel XAML, tal como la pintura con una máscara con animaciones, un estiramiento de nueve cuadrículas animado o una cadena de efecto, puede usar CompositionBrush para pintar un objeto UIElement mediante el uso de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Al trabajar con la capa Visual, debe usarse un CompositionBrush para pintar el área de un [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Requisitos previos](./composition-brushes.md#prerequisites)
-   [Pintar con CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Pintar con un color sólido](./composition-brushes.md#paint-with-a-solid-color)
    -   [Pintar con un degradado lineal](./composition-brushes.md#paint-with-a-linear-gradient) 
    -   [Pintar con un degradado radial](./composition-brushes.md#paint-with-a-radial-gradient)
    -   [Pintar con una imagen](./composition-brushes.md#paint-with-an-image)
    -   [Pintar con un dibujo personalizado](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Pintar con un vídeo](./composition-brushes.md#paint-with-a-video)
    -   [Pintar con un efecto de filtro](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Pintar con CompositionBrush con una máscara de opacidad](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Pintar con CompositionBrush mediante NineGrid stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Pintar con píxeles en segundo plano](./composition-brushes.md#paint-using-background-pixels)
-   [Combinar CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Uso de un pincel de XAML de vs. CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Temas relacionados](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Requisitos previos
En esta introducción se supone que estás familiarizado con la estructura de una aplicación de composición básica, tal como se describe en [Información general sobre la capa visual](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Pintar con un CompositionBrush

Un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "pinta" un área con su salida. Distintos pinceles tienen tipos de salida diferentes. Algunos pinceles pintan un área de un color sólido, otros con un degradado, imagen, dibujo personalizado o efecto. También hay pinceles especializados que modifican el comportamiento de otros pinceles. Por ejemplo, la máscara de opacidad puede usarse para controlar el área que se pinta con un CompositionBrush. También se puede usar una cuadrícula de nueve cuadros para controlar la extensión aplicada a un CompositionBrush al pintar un área. CompositionBrush puede ser uno de los siguientes tipos:

|Clase                                   |Detalles                                         |Introducido en|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Pinta un área de un color sólido                        |Windows 10, versión 1511 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Pinta un área con el contenido de una [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Windows 10, versión 1511 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Pinta un área con el contenido de un efecto de composición |Windows 10, versión 1511 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Pinta un elemento visual con un CompositionBrush con una máscara de opacidad |Windows 10, versión 1607 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Pinta un área con un CompositionBrush con una cuadrícula NineGrid |Windows 10, versión 1607 (SDK 14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Pinta un área con un degradado lineal                    |Windows 10, versión 1709 (SDK 16299)
|[CompositionRadialGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionradialgradientbrush)|Pinta un área con un degradado radial                    |Windows 10, versión 1903 (SDK de Insider Preview)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Pinta un área mediante el muestreo de píxeles de fondo desde la aplicación o píxeles directamente detrás de la ventana de la aplicación en el escritorio. Se usa como una entrada a otra CompositionBrush como un CompositionEffectBrush | Windows 10, versión 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Pintar con un color sólido

Un [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) pinta un área de un color sólido. Hay varias formas de especificar el color de un SolidColorBrush. Por ejemplo, puedes especificar sus canales alfa, rojo, azul y verde (ARGB) o usar uno de los colores predefinidos proporcionados por la clase [Colores](https://docs.microsoft.com/uwp/api/windows.ui.colors).

En la siguiente ilustración y código se muestra un pequeño árbol visual para crear un rectángulo que se traza en un pincel de color negro y se dibuja con un pincel de color sólido con el valor de color 0x9ACD32.

![CompositionColorBrush](images/composition-compositioncolorbrush.png)

```cs
Compositor _compositor;
ContainerVisual _container;
SpriteVisual _colorVisual1, _colorVisual2;
CompositionColorBrush _blackBrush, _greenBrush;

_compositor = Window.Current.Compositor;
_container = _compositor.CreateContainerVisual();

_blackBrush = _compositor.CreateColorBrush(Colors.Black);
_colorVisual1= _compositor.CreateSpriteVisual();
_colorVisual1.Brush = _blackBrush;
_colorVisual1.Size = new Vector2(156, 156);
_colorVisual1.Offset = new Vector3(0, 0, 0);
_container.Children.InsertAtBottom(_colorVisual1);

_ greenBrush = _compositor.CreateColorBrush(Color.FromArgb(0xff, 0x9A, 0xCD, 0x32));
_colorVisual2 = _compositor.CreateSpriteVisual();
_colorVisual2.Brush = _greenBrush;
_colorVisual2.Size = new Vector2(150, 150);
_colorVisual2.Offset = new Vector3(3, 3, 0);
_container.Children.InsertAtBottom(_colorVisual2);
```

### <a name="paint-with-a-linear-gradient"></a>Pintar con un degradado lineal

Un [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) pinta un área con un degrado lineal. Un degradado lineal mezcla dos o más colores a lo largo de una línea, el eje de degradado. Debes usar objetos GradientStop para especificar los colores del degradado y sus posiciones.

En la siguiente ilustración y código se muestran una clase SpriteVisual pintada con un LinearGradientBrush con 2 delimitadores con un color amarillo y rojo.

![CompositionLinearGradientBrush](images/composition-compositionlineargradientbrush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionLinearGradientBrush _redyellowBrush;

_compositor = Window.Current.Compositor;

_redyellowBrush = _compositor.CreateLinearGradientBrush();
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Red));
_redyellowBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.Yellow));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = _redyellowBrush;
_gradientVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-radial-gradient"></a>Pintar con un degradado radial

Un [CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush) pinta un área con un degradado radial. Un degradado radial que combina dos o más colores con el degradado empezando desde el centro de la elipse y terminando en radius de la elipse. Los objetos GradientStop se usan para definir los colores y su ubicación en el degradado.

En la ilustración y el código siguiente se muestra un SpriteVisual pinta con RadialGradientBrush con 2 GradientStops.

![CompositionRadialGradientBrush](images/radial-gradient-brush.png)

```cs
Compositor _compositor;
SpriteVisual _gradientVisual;
CompositionRadialGradientBrush RGBrush;

_compositor = Window.Current.Compositor;

RGBrush = _compositor.CreateRadialGradientBrush();
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(0, Colors.Aquamarine));
RGBrush.ColorStops.Add(_compositor.CreateColorGradientStop(1, Colors.DeepPink));
_gradientVisual = _compositor.CreateSpriteVisual();
_gradientVisual.Brush = RGBrush;
_gradientVisual.Size = new Vector2(200, 200);
```

### <a name="paint-with-an-image"></a>Pintar con una imagen

Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) pinta un área con píxeles representados en un ICompositionSurface. Por ejemplo, un CompositionSurfaceBrush puede usarse para pintar un área con una imagen representada en una superficie de ICompositionSurface con la API [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface).

En la siguiente ilustración y código se muestran un SpriteVisual pintado con un mapa de bits de color orozuz representado en un objeto ICompositionSurface con LoadedImageSurface. Las propiedades de CompositionSurfaceBrush pueden usarse para ampliar y alinear el mapa de bits dentro de los límites del elemento visual.

![CompositionSurfaceBrush](images/composition-compositionsurfacebrush.png)

```cs
Compositor _compositor;
SpriteVisual _imageVisual;
CompositionSurfaceBrush _imageBrush;

_compositor = Window.Current.Compositor;

_imageBrush = _compositor.CreateSurfaceBrush();

// The loadedSurface has a size of 0x0 till the image has been been downloaded, decoded and loaded to the surface. We can assign the surface to the CompositionSurfaceBrush and it will show up once the image is loaded to the surface.
LoadedImageSurface _loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_imageBrush.Surface = _loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _imageBrush;
_imageVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-custom-drawing"></a>Pintar con un dibujo personalizado
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también puede usarse para pintar un área con píxeles de un ICompositionSurface representado con [Win2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (o D2D).

En el siguiente código se muestra una clase SpriteVisual pintada con una ejecución de texto representada en un ICompositionSurface con Win2D. Ten en cuenta que para usar Win2D debes incluir el paquete [Win2D NuGet](https://www.nuget.org/packages/Win2D.uwp) en el proyecto.

```cs
Compositor _compositor;
CanvasDevice _device;
CompositionGraphicsDevice _compositionGraphicsDevice;
SpriteVisual _drawingVisual;
CompositionSurfaceBrush _drawingBrush;

_device = CanvasDevice.GetSharedDevice();
_compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(_compositor, _device);

_drawingBrush = _compositor.CreateSurfaceBrush();
CompositionDrawingSurface _drawingSurface = _compositionGraphicsDevice.CreateDrawingSurface(new Size(256, 256), DirectXPixelFormat.B8G8R8A8UIntNormalized, DirectXAlphaMode.Premultiplied);

using (var ds = CanvasComposition.CreateDrawingSession(_drawingSurface))
{
     ds.Clear(Colors.Transparent);
     var rect = new Rect(new Point(2, 2), (_drawingSurface.Size.ToVector2() - new Vector2(4, 4)).ToSize());
     ds.FillRoundedRectangle(rect, 15, 15, Colors.LightBlue);
     ds.DrawRoundedRectangle(rect, 15, 15, Colors.Gray, 2);
     ds.DrawText("This is a composition drawing surface", rect, Colors.Black, new CanvasTextFormat()
     {
          FontFamily = "Comic Sans MS",
          FontSize = 32,
          WordWrapping = CanvasWordWrapping.WholeWord,
          VerticalAlignment = CanvasVerticalAlignment.Center,
          HorizontalAlignment = CanvasHorizontalAlignment.Center
     }
);

_drawingBrush.Surface = _drawingSurface;

_drawingVisual = _compositor.CreateSpriteVisual();
_drawingVisual.Brush = _drawingBrush;
_drawingVisual.Size = new Vector2(156, 156);
```

Del mismo modo, CompositionSurfaceBrush también puede usarse para pintar una clase SpriteVisual mediante SwapChain con la interoperación de Win2D. [En esta muestra](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) se ofrece un ejemplo de cómo usar Win2D para pintar una clase SpriteVisual con una cadena de intercambio.

### <a name="paint-with-a-video"></a>Pintar con un vídeo
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también puede usarse para pintar un área con píxeles de un ICompositionSurface representado con un vídeo cargado a través de la clase [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer).

En el siguiente código se muestra una clase SpriteVisual pintada con un vídeo cargado en un ICompositionSurface.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("https://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
var item = new MediaPlaybackItem(source);
_mediaPlayer.Source = item;
_mediaPlayer.IsLoopingEnabled = true;

// Get the surface from MediaPlayer and put it on a brush
_videoSurface = _mediaPlayer.GetSurface(_compositor);
_videoBrush = _compositor.CreateSurfaceBrush(_videoSurface.CompositionSurface);

_videoVisual = _compositor.CreateSpriteVisual();
_videoVisual.Brush = _videoBrush;
_videoVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-filter-effect"></a>Pintar con un efecto de filtro

Un [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) pinta un área con salida de un CompositionEffect. Los efectos de la capa visual pueden considerarse efectos de filtro que se pueden animar aplicados a una colección de contenido de origen, como por ejemplo, colores, degradados, imágenes, vídeos, cadenas de intercambio, regiones de la IU o árboles de elementos visuales. El contenido de origen normalmente se especifica mediante otro CompositionBrush.

En la siguiente ilustración y código se muestran una clase SpriteVisual pintada con una imagen de un gato con el efecto de filtro de desaturación aplicado.

![CompositionEffectBrush](images/composition-cat-desaturated.png)

```cs
Compositor _compositor;
SpriteVisual _effectVisual;
CompositionEffectBrush _effectBrush;

_compositor = Window.Current.Compositor;

var graphicsEffect = new SaturationEffect {
                              Saturation = 0.0f,
                              Source = new CompositionEffectSourceParameter("mySource")
                         };

var effectFactory = _compositor.CreateEffectFactory(graphicsEffect);
_effectBrush = effectFactory.CreateBrush();

CompositionSurfaceBrush surfaceBrush =_compositor.CreateSurfaceBrush();
LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/cat.jpg"));
SurfaceBrush.surface = loadedSurface;

_effectBrush.SetSourceParameter("mySource", surfaceBrush);

_effectVisual = _compositor.CreateSpriteVisual();
_effectVisual.Brush = _effectBrush;
_effectVisual.Size = new Vector2(156, 156);
```

Para obtener más información sobre cómo crear un efecto con CompositionBrushes, consulta [Efectos de capa Visual](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Pintar con un CompositionBrush con una máscara de opacidad aplicada

Un [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) pinta un área con un CompositionBrush con una máscara de opacidad aplicada. El origen de la máscara de opacidad puede ser cualquier CompositionBrush del tipo CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush o CompositionNineGridBrush. La máscara de opacidad debe especificarse como un CompositionSurfaceBrush.

En la siguiente ilustración y código se muestran una clase SpriteVisual pintada con un CompositionMaskBrush. El origen de la máscara es un CompositionLinearGradientBrush que se enmascara como un círculo con una imagen de un círculo como una máscara.

![CompositionMaskBrush](images/composition-compositionmaskbrush.png)

```cs
Compositor _compositor;
SpriteVisual _maskVisual;
CompositionMaskBrush _maskBrush;

_compositor = Window.Current.Compositor;

_maskBrush = _compositor.CreateMaskBrush();

CompositionLinearGradientBrush _sourceGradient = _compositor.CreateLinearGradientBrush();
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(0,Colors.Red));
_sourceGradient.ColorStops.Add(_compositor.CreateColorGradientStop(1,Colors.Yellow));
_maskBrush.Source = _sourceGradient;

LoadedImageSurface loadedSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/circle.png"), new Size(156.0, 156.0));
_maskBrush.Mask = _compositor.CreateSurfaceBrush(loadedSurface);

_maskVisual = _compositor.CreateSpriteVisual();
_maskVisual.Brush = _maskBrush;
_maskVisual.Size = new Vector2(156, 156);
```

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Pintar con un CompositionBrush con una cuadrícula NineGrid

Un [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) pinta un área con un CompositionBrush que se estira con la metáfora de cuadrícula de nueve cuadros. La metáfora de la cuadrícula de nueve cuadros te permite ajustar los bordes y las esquinas de un CompositionBrush de manera diferente que su centro. El origen de la cuadrícula de nueve cuadros puede ser cualquier CompositionBrush del tipo CompositionColorBrush, CompositionSurfaceBrush o CompositionEffectBrush.

En el siguiente código se muestra una clase SpriteVisual pintada con un CompositionNineGridBrush. El origen de la máscara es un CompositionSurfaceBrush que se estira con una cuadrícula de nueve cuadros.

```cs
Compositor _compositor;
SpriteVisual _nineGridVisual;
CompositionNineGridBrush _nineGridBrush;

_compositor = Window.Current.Compositor;

_ninegridBrush = _compositor.CreateNineGridBrush();

// nineGridImage.png is 50x50 pixels; nine-grid insets, as measured relative to the actual size of the image, are: left = 1, top = 5, right = 10, bottom = 20 (in pixels)

LoadedImageSurface _imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/nineGridImage.png"));
_nineGridBrush.Source = _compositor.CreateSurfaceBrush(_imageSurface);

// set Nine-Grid Insets

_ninegridBrush.SetInsets(1, 5, 10, 20);

// set appropriate Stretch on SurfaceBrush for Center of Nine-Grid

sourceBrush.Stretch = CompositionStretch.Fill;

_nineGridVisual = _compositor.CreateSpriteVisual();
_nineGridVisual.Brush = _ninegridBrush;
_nineGridVisual.Size = new Vector2(100, 75);
```

### <a name="paint-using-background-pixels"></a>Pintar con píxeles de fondo

Un [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) pinta un área con el contenido detrás del área. Un CompositionBackdropBrush nunca se utiliza por sí solo, sin embargo, se usa como entrada a otro CompositionBrush como un EffectBrush. Por ejemplo, mediante el uso de un CompositionBackdropBrush como una entrada a un efecto de desenfoque, puedes lograr un efecto de cristal esmerilado.

En el siguiente código se muestra un pequeño árbol visual para crear una imagen con CompositionSurfaceBrush y una superposición de cristal esmerilado encima de la imagen. La superposición de cristal esmerilado se crea mediante la colocación de una clase SpriteVisual rellenada con un EffectBrush encima de la imagen. El EffectBrush usa un CompositionBackdropBrush como entrada al efecto de desenfoque.

```cs
Compositor _compositor;
SpriteVisual _containerVisual;
SpriteVisual _imageVisual;
SpriteVisual _backdropVisual;

_compositor = Window.Current.Compositor;

// Create container visual to host the visual tree
_containerVisual = _compositor.CreateContainerVisual();

// Create _imageVisual and add it to the bottom of the container visual.
// Paint the visual with an image.

CompositionSurfaceBrush _licoriceBrush = _compositor.CreateSurfaceBrush();

LoadedImageSurface loadedSurface = 
    LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/licorice.jpg"));
_licoriceBrush.Surface = loadedSurface;

_imageVisual = _compositor.CreateSpriteVisual();
_imageVisual.Brush = _licoriceBrush;
_imageVisual.Size = new Vector2(156, 156);
_imageVisual.Offset = new Vector3(0, 0, 0);
_containerVisual.Children.InsertAtBottom(_imageVisual)

// Create a SpriteVisual and add it to the top of the containerVisual.
// Paint the visual with an EffectBrush that applies blur to the content
// underneath the Visual to create a frosted glass effect.

GaussianBlurEffect blurEffect = new GaussianBlurEffect(){
                                    Name = "Blur",
                                    BlurAmount = 1.0f,
                                    BorderMode = EffectBorderMode.Hard,
                                    Source = new CompositionEffectSourceParameter("source");
                                    };

CompositionEffectFactory blurEffectFactory = _compositor.CreateEffectFactory(blurEffect);
CompositionEffectBrush _backdropBrush = blurEffectFactory.CreateBrush();

// Create a BackdropBrush and bind it to the EffectSourceParameter source

_backdropBrush.SetSourceParameter("source", _compositor.CreateBackdropBrush());
_backdropVisual = _compositor.CreateSpriteVisual();
_backdropVisual.Brush = _licoriceBrush;
_backdropVisual.Size = new Vector2(78, 78);
_backdropVisual.Offset = new Vector3(39, 39, 0);
_containerVisual.Children.InsertAtTop(_backdropVisual);
```

## <a name="combining-compositionbrushes"></a>Combinar CompositionBrushes
Un número de CompositionBrushes usa otros CompositionBrushes como entradas. Por ejemplo el método SetSourceParameter puede usarse para establecer otro CompositionBrush como una entrada a un CompositionEffectBrush. En la siguiente tabla se describen las combinaciones de CompositionBrushes compatibles. Ten en cuenta que mediante una combinación no compatible se generará una excepción.

<table>
<tbody>
<tr>
<th>Pincel</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush.Mask</th>
<th>MaskBrush.Source</th>
<th>NineGridBrush.Source</th>
</tr>
<tr>
<td>CompositionColorBrush</td>
<td>SÍ</td>
<td>SÍ</td>
<td>SÍ</td>
<td>SÍ</td>
</tr>
<tr>
<td>CompositionLinear<br />GradientBrush</td>
<td>SÍ</td>
<td>SÍ</td>
<td>SÍ</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionSurfaceBrush</td>
<td>SÍ</td>
<td>SÍ</td>
<td>SÍ</td>
<td>SÍ</td>
</tr>
<tr>
<td>CompositionEffectBrush</td>
<td>NO</td>
<td>NO</td>
<td>SÍ</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>NO</td>
<td>NO</td>
<td>NO</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>SÍ</td>
<td>SÍ</td>
<td>SÍ</td>
<td>NO</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>SÍ</td>
<td>NO</td>
<td>NO</td>
<td>NO</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Uso de un pincel de XAML de vs. CompositionBrush

En la siguiente tabla se proporciona una lista de escenarios y si se recomienda el uso del pincel XAML o de composición al pintar un UIElement o una clase SpriteVisual en la aplicación. 

> [!NOTE]
> Si se recomienda un CompositionBrush para un XAML UIElement, se da por hecho que el CompositionBrush está empaquetado con una XamlCompositionBrushBase.

|Escenario                                                                   | XAML UIElement                                                                                                |Composition SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Pintar un área de un color sólido                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área de un color animado                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área con un degradado estático                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con delimitadores de degradado animado                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con una imagen                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Pintar un área con una página web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/D
|Pintar un área con una imagen con una cuadrícula NineGrid                         |[Control de imagen](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con una cuadrícula animada NineGrid                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con una cadena de intercambio                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con la interoperación de cadena de intercambio
|Pintar un área con un vídeo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con la interoperación multimedia
|Pintar un área con un dibujo 2D personalizado                                       |[CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) de Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con interoperación de Win2D
|Pintar un área con una máscara no animada                                       |Usar XAML [formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) para definir una máscara   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con una máscara animada                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con un efecto de filtro animado                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Pintar un área con un efecto que se aplica a los píxeles de fondo        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Temas relacionados

[Composición nativo DirectX y Direct2D interactuar con el método BeginDraw y EndDraw](composition-native-interop.md)

[Interoperabilidad de pincel XAML con XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
