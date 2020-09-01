---
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pinceles de composición
description: Un pincel pinta el área de un objeto Visual con su salida. Distintos pinceles tienen tipos de salida diferentes.
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e3c0454b5596490631f32c3481675f3c70d4eb76
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175649"
---
# <a name="composition-brushes"></a>Pinceles de composición
Todo lo que está visible en la pantalla desde una aplicación de UWP es visible porque lo ha pintado un pincel. Los pinceles permiten pintar objetos de la interfaz de usuario (UI) con contenido que abarca desde colores simples y sólidos hasta imágenes o dibujos hasta una cadena de efectos complejos. En este tema se presentan los conceptos del dibujo con CompositionBrush.

Tenga en cuenta que, al trabajar con la aplicación de UWP XAML, puede optar por pintar un UIElement con un [pincel XAML](../design/style/brushes.md) o un [CompositionBrush](/uwp/api/Windows.UI.Composition.CompositionBrush). Normalmente, es más fácil y aconsejable elegir un pincel XAML si el escenario es compatible con un pincel XAML. Por ejemplo, animar el color de un botón, cambiando el relleno de un texto o una forma con una imagen. Por otro lado, si está intentando realizar algo que no es compatible con un pincel de XAML, como pintar con una máscara animada o un estiramiento de nueve o una cadena de efectos animados, puede usar una CompositionBrush para pintar un UIElement mediante el uso de [XamlCompositionBrushBase](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Al trabajar con la capa visual, se debe usar una CompositionBrush para pintar el área de una [SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Requisitos previos](./composition-brushes.md#prerequisites)
-   [Pintar con CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Pintar con un color sólido](./composition-brushes.md#paint-with-a-solid-color)
    -   [Pintar con un degradado lineal](./composition-brushes.md#paint-with-a-linear-gradient) 
    -   [Pintar con un degradado radial](./composition-brushes.md#paint-with-a-radial-gradient)
    -   [Pintar con una imagen](./composition-brushes.md#paint-with-an-image)
    -   [Pintar con un dibujo personalizado](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Pintar con un vídeo](./composition-brushes.md#paint-with-a-video)
    -   [Pintar con un efecto de filtro](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Pintar con un CompositionBrush con una máscara de opacidad](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Pintar con un CompositionBrush con NineGrid Stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Pintar con píxeles de fondo](./composition-brushes.md#paint-using-background-pixels)
-   [Combinación de CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Usar un pincel de XAML en lugar de CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Temas relacionados](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Requisitos previos
En esta información general se supone que está familiarizado con la estructura de una aplicación de composición básica, como se describe en la [información general sobre la capa visual](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Pintar con un CompositionBrush

Un [CompositionBrush](/uwp/api/Windows.UI.Composition.CompositionBrush) "pinta" un área con su salida. Distintos pinceles tienen tipos de salida diferentes. Algunos pinceles pintan un área con un color sólido, otros con un degradado, una imagen, un dibujo personalizado o un efecto. También hay pinceles especializados que modifican el comportamiento de otros pinceles. Por ejemplo, la máscara de opacidad se puede usar para controlar qué área está dibujada por un CompositionBrush, o bien se puede usar una de nueve cuadrícula para controlar el ajuste aplicado a un CompositionBrush al pintar un área. CompositionBrush puede ser de uno de los siguientes tipos:

|Clase                                   |Detalles                                         |Introducido en|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Pinta un área con un color sólido                        |Windows 10, versión 1511 (SDK 10586)|
|[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Pinta un área con el contenido de un [ICompositionSurface](/uwp/api/Windows.UI.Composition.ICompositionSurface)|Windows 10, versión 1511 (SDK 10586)|
|[CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Pinta un área con el contenido de un efecto de composición. |Windows 10, versión 1511 (SDK 10586)|
|[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Dibuja un control visual con una CompositionBrush con una máscara de opacidad. |Windows 10, versión 1607 (SDK 14393)
|[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Pinta un área con un CompositionBrush con un NineGrid Stretch |Windows 10, versión 1607 (SDK 14393)
|[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Pinta un área con un degradado lineal                    |Windows 10, versión 1709 (SDK 16299)
|[CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush)|Pinta un área con un degradado radial                    |Windows 10, versión 1903 (SDK de Insider Preview)
|[CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Pinta un área mediante el muestreo de los píxeles de fondo de la aplicación o de los píxeles directamente detrás de la ventana de la aplicación en el escritorio. Se usa como entrada para otro CompositionBrush como un CompositionEffectBrush | Windows 10, versión 1607 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Pintar con un color sólido

Un [CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush) pinta un área con un color sólido. Hay varias maneras de especificar el color de un SolidColorBrush. Por ejemplo, puede especificar sus canales alfa, rojo, azul y verde (ARGB) o usar uno de los colores predefinidos proporcionados por la clase [Colors](/uwp/api/windows.ui.colors) .

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

Un [CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush) pinta un área con un degradado lineal. Un degradado lineal combina dos o más colores en una línea, el eje de degradado. Los objetos GradientStop se usan para especificar los colores del degradado y sus posiciones.

En la ilustración y el código siguientes se muestra un SpriteVisual pintado con un LinearGradientBrush con 2 paradas con un color rojo y amarillo.

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

Un [CompositionRadialGradientBrush](/uwp/api/windows.ui.composition.compositionradialgradientbrush) pinta un área con un degradado radial. Un degradado radial combina dos o más colores con el degradado empezando desde el centro de la elipse y terminando en el radio de la elipse. Los objetos GradientStop se usan para definir los colores y su ubicación en el degradado.

En la ilustración y el código siguientes se muestra un SpriteVisual pintado con un RadialGradientBrush con 2 GradientStops.

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

Un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) pinta un área con píxeles representados en un ICompositionSurface. Por ejemplo, un CompositionSurfaceBrush se puede usar para pintar un área con una imagen representada en una superficie de ICompositionSurface mediante la API de [LoadedImageSurface](/uwp/api/windows.ui.xaml.media.loadedimagesurface) .

En la ilustración y el código siguientes se muestra un SpriteVisual pintado con un mapa de bits de un licorice representado en un ICompositionSurface con LoadedImageSurface. Las propiedades de CompositionSurfaceBrush se pueden usar para expandir y alinear el mapa de bits dentro de los límites del visual.

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
Un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también se puede usar para pintar un área con píxeles de un ICompositionSurface representado mediante [WIN2D](https://microsoft.github.io/Win2D/html/Introduction.htm) (o D2D).

En el código siguiente se muestra un SpriteVisual pintado con una ejecución de texto representada en un ICompositionSurface con Win2D. Tenga en cuenta que, para poder usar Win2D, debe incluir el paquete [NuGet de win2d](https://www.nuget.org/packages/Win2D.uwp) en el proyecto.

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

Del mismo modo, el CompositionSurfaceBrush también se puede usar para pintar un SpriteVisual con un intercambio mediante la interoperabilidad de Win2D. En [este ejemplo](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) se proporciona un ejemplo de cómo usar Win2D para pintar un SpriteVisual con un intercambio.

### <a name="paint-with-a-video"></a>Pintar con un vídeo
Un [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también se puede usar para pintar un área con píxeles de un ICompositionSurface representado mediante un vídeo cargado a través de la clase [MediaPlayer](/uwp/api/Windows.Media.Playback.MediaPlayer) .

En el código siguiente se muestra un SpriteVisual pintado con un vídeo cargado en un ICompositionSurface.

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

Un [CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush) pinta un área con la salida de un CompositionEffect. Los efectos en el nivel visual se pueden considerar como efectos de filtro que se pueden animar aplicados a una colección de contenido de origen como colores, degradados, imágenes, vídeos, intercambio, regiones de la interfaz de usuario o árboles de objetos visuales. Normalmente, el contenido de origen se especifica mediante otro CompositionBrush.

En la ilustración y el código siguientes se muestra un SpriteVisual pintado con una imagen de un gato que tiene aplicado el efecto de filtro de dessaturación.

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

Para obtener más información sobre cómo crear un efecto mediante CompositionBrushes, vea [efectos en la capa visual](./composition-effects.md) .

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Pintar con un CompositionBrush con una máscara de opacidad aplicada

Un [CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush) pinta un área con un CompositionBrush con una máscara de opacidad aplicada a ella. El origen de la máscara de opacidad puede ser cualquier CompositionBrush del tipo CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush o CompositionNineGridBrush. La máscara de opacidad debe especificarse como CompositionSurfaceBrush.

En la ilustración y el código siguientes se muestra un SpriteVisual pintado con un CompositionMaskBrush. El origen de la máscara es un CompositionLinearGradientBrush que se enmascara para que se parezca a un círculo mediante una imagen de círculo como máscara.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Pintar con un CompositionBrush con NineGrid Stretch

Un [CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) pinta un área con un CompositionBrush que se ajusta mediante la metáfora de nueve cuadrículas. La metáfora de nueve cuadrículas le permite expandir los bordes y las esquinas de un CompositionBrush de forma distinta a su centro. El origen del estiramiento de nueve cuadrículas puede ser por cualquier CompositionBrush del tipo CompositionColorBrush, CompositionSurfaceBrush o CompositionEffectBrush.

En el código siguiente se muestra un SpriteVisual pintado con un CompositionNineGridBrush. El origen de la máscara es un CompositionSurfaceBrush que se ajusta mediante una extensión de nueve cuadrículas.

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

Un [CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)  pinta un área con el contenido detrás del área. Un CompositionBackdropBrush nunca se usa por sí solo, sino que se usa como entrada para otro CompositionBrush como un EffectBrush. Por ejemplo, si usa un CompositionBackdropBrush como entrada para un efecto de desenfoque, puede conseguir un efecto de cristal helada.

En el código siguiente se muestra un pequeño árbol visual para crear una imagen mediante CompositionSurfaceBrush y una superposición de cristal helada encima de la imagen. La superposición de cristal helada se crea colocando una SpriteVisual rellenada con un EffectBrush sobre la imagen. EffectBrush usa CompositionBackdropBrush como entrada para el efecto de desenfoque.

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

## <a name="combining-compositionbrushes"></a>Combinación de CompositionBrushes
Un número de CompositionBrushes usa otros CompositionBrushes como entradas. Por ejemplo, se puede usar el método SetSourceParameter para establecer otro CompositionBrush como entrada para un CompositionEffectBrush. En la tabla siguiente se describen las combinaciones admitidas de CompositionBrushes. Tenga en cuenta que si se usa una combinación no admitida, se producirá una excepción.

<table>
<tbody>
<tr>
<th>Pincel</th>
<th>EffectBrush.SetSourceParameter()</th>
<th>MaskBrush. Mask</th>
<th>Origen de MaskBrush.</th>
<th>Origen de NineGridBrush.</th>
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
<td>No</td>
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
<td>No</td>
<td>No</td>
<td>SÍ</td>
<td>No</td>
</tr>
<tr>
<td>CompositionMaskBrush</td>
<td>No</td>
<td>No</td>
<td>No</td>
<td>No</td>
</tr>
<tr>
<td>CompositionNineGridBrush</td>
<td>SÍ</td>
<td>SÍ</td>
<td>SÍ</td>
<td>No</td>
</tr>
<tr>
<td>CompositionBackdropBrush</td>
<td>SÍ</td>
<td>No</td>
<td>No</td>
<td>No</td>
</tr>
</tbody>
</table>


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Usar un pincel de XAML en lugar de CompositionBrush

En la tabla siguiente se proporciona una lista de escenarios y si se recomienda el uso de pinceles XAML o de composición al pintar un UIElement o un SpriteVisual en la aplicación. 

> [!NOTE]
> Si se sugiere un CompositionBrush para un UIElement de XAML, se supone que el CompositionBrush se empaqueta con un XamlCompositionBrushBase.

|Escenario                                                                   | UIElement de XAML                                                                                                |SpriteVisual de composición
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Pintar un área con color sólido                                             |[SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|Pintar un área con color animado                                          |[SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)                                |[CompositionColorBrush](/uwp/api/Windows.UI.Composition.CompositionColorBrush)
|Pintar un área con un degradado estático                                       |[LinearGradientBrush](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)                            |[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con delimitadores de degradado animados                                 |[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con una imagen                                                |[ImageBrush](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)                                     |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)
|Pintar un área con una página web                                               |[WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)                                   |N/D
|Pintar un área con una imagen mediante NineGrid Stretch                         |[Control de imagen](/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con el ajuste de NineGrid animado                               |[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con un intercambio                                             |[SwapChainPanel](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |Interoperabilidad de [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) con intercambio
|Pintar un área con un vídeo                                                 |[MediaElement](../design/controls-and-patterns/media-playback.md)                                                                                                  |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) con interoperabilidad multimedia
|Pintar un área con un dibujo 2D personalizado                                       |[CanvasControl](https://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) de Win2D                                                                                                 |[CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) con interoperabilidad de Win2D
|Pintar un área con una máscara no animada                                       |Usar [formas](../design/controls-and-patterns/shapes.md) XAML para definir una máscara   |[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con una máscara animada                                        |[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con un efecto de filtro animado                               |[CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Pintar un área con un efecto aplicado a los píxeles de fondo        |[CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Temas relacionados

[Interoperabilidad de DirectX y Direct2D nativa de composición con BeginDraw y EndDraw](composition-native-interop.md)

[Interoperabilidad de pincel de XAML con XamlCompositionBrushBase](../design/style/brushes.md#xamlcompositionbrushbase)