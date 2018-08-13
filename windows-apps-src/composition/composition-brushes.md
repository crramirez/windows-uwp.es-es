---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pinceles de composición
description: Un pincel pinta el área de un objeto Visual con su salida. Distintos pinceles tienen tipos de salida diferentes.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 103ecd24c35d75d3ea1d305d7294048dc628d2e2
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1038575"
---
# <a name="composition-brushes"></a>Pinceles de composición
Todo lo visible en la pantalla de una aplicación UWP está visible porque lo ha pintado un pincel. Pinceles permiten pintar los objetos de la interfaz (IU) de usuario con contenido desde simples colores sólidos hasta imágenes o dibujos a la cadena de efectos complejos. En este tema se presentan los conceptos de pintura con CompositionBrush.

Tenga en cuenta, cuando se trabaja con la aplicación UWP XAML, puede elegir un UIElement con un [Pincel en XAML](/windows/uwp/design/style/brushes) o un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush)de pintura. Normalmente, es más fácil y conveniente para elegir un pincel XAML si su escenario es compatible con un pincel en XAML. Por ejemplo, si se anima el color de un botón, cambiar el relleno de un texto o una forma con una imagen. Por otro lado, si se intenta hacer algo que no es compatible con un pincel XAML como tal como la pintura con una máscara con animaciones o un estiramiento nueve cuadrícula animado o una cadena de efecto, puede usar un CompositionBrush a pintar un UIElement mediante el uso de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Cuando se trabaja con la capa Visual, se debe usar un CompositionBrush a pintar el área de un [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Requisitos previos](./composition-brushes.md#prerequisites)
-   [Pintar con CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Pintar con un color sólido](./composition-brushes.md#paint-with-a-solid-color)
    -   [Pintar con un degradado lineal](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [Pintar con una imagen](./composition-brushes.md#paint-with-an-image)
    -   [Pintar con un dibujo personalizado](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Pintar con un vídeo](./composition-brushes.md#paint-with-a-video)
    -   [Pintar con un efecto de filtro](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Pintar con un CompositionBrush con una máscara de opacidad](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Pintar con un CompositionBrush con NineGrid estiramiento](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Pintar con píxeles de fondo](./composition-brushes.md#paint-using-background-pixels)
-   [Combinación de CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Uso de un pincel XAML frente a CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Temas relacionados](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Requisitos previos
Esta información general se supone que está familiarizado con la estructura de una aplicación básica de composición, tal como se describe en la [información general de la capa Visual](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Pintura con un CompositionBrush

Un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "dibuja" un área con su salida. Distintos pinceles tienen tipos de salida diferentes. Algunos pinceles pintan un área con un color sólido, otros con un degradado, imagen, dibujo personalizado o efecto. También hay pinceles especializados que modifican el comportamiento de otros pinceles. Por ejemplo, máscara de opacidad puede utilizarse para controlar el área que se dibuja un CompositionBrush o una cuadrícula de nueve se puede usar para controlar el estiramiento aplicado a un CompositionBrush al pintar un área. CompositionBrush puede ser de uno de los siguientes tipos:

|Clase                                   |Detalles                                         |Introducido en|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Dibuja un área con un color sólido                        |Actualización de noviembre 10 de Windows (SDK de 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Dibuja un área con el contenido de un [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Actualización de noviembre 10 de Windows (SDK de 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Dibuja un área con el contenido de un efecto de composición |Actualización de noviembre 10 de Windows (SDK de 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Dibuja un objeto visual con un CompositionBrush con una máscara de opacidad |Actualización de Windows 10 aniversario (SDK de 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Dibuja un área con un CompositionBrush con un estiramiento NineGrid |Actualización de Windows 10 aniversario SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Dibuja un área con un degradado lineal                    |Windows 10 se dividen los creadores actualización (vista previa de información confidencial SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Dibuja un área de muestreo de píxeles de fondo de ya sea la aplicación, o directamente detrás de la ventana de aplicación en el escritorio. Usar como una entrada en otro CompositionBrush como un CompositionEffectBrush | Actualización de Windows 10 aniversario (SDK de 14393)

### <a name="paint-with-a-solid-color"></a>Pintar con un color sólido

Un [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) dibuja un área con un color sólido. Hay una gran variedad de formas para especificar el color de un objeto SolidColorBrush. Por ejemplo, puede especificar sus los canales alfa, rojo, verdes y azules (ARGB) o utilice uno de los colores predefinidos proporcionados por la clase de [colores](https://docs.microsoft.com/uwp/api/windows.ui.colors) .

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

Un [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) dibuja un área con un degradado lineal. Un degradado lineal mezcla dos o más colores a lo largo de una línea, el eje de degradado. Use objetos GradientStop para especificar los colores del degradado y sus posiciones.

En la ilustración y el código siguiente se muestra un SpriteVisual a pintar con un LinearGradientBrush con 2 se detiene con un color amarillo y rojo.

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

### <a name="paint-with-an-image"></a>Pintar con una imagen

Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) dibuja un área con píxeles representadas en un ICompositionSurface. Por ejemplo, un CompositionSurfaceBrush puede utilizarse para pintar un área con una imagen representada a una superficie de ICompositionSurface con [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API.

En la ilustración y el código siguiente muestra un SpriteVisual a pintar con un mapa de bits de un licorice representada en un ICompositionSurface con LoadedImageSurface. Las propiedades de CompositionSurfaceBrush pueden usarse para estirar y alinear el mapa de bits dentro de los límites del objeto visual.

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
También se puede usar un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) para pintar un área con píxeles desde un ICompositionSurface representada utilizando [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) (o D2D).

El código siguiente muestra que un SpriteVisual a pintar con una ejecución representado en un ICompositionSurface de texto con Win2D. Tenga en cuenta para poder usar Win2D debe incluir el paquete [NuGet Win2D](http://www.nuget.org/packages/Win2D.uwp) en el proyecto.

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

De forma similar, el CompositionSurfaceBrush puede utilizarse para pintar una SpriteVisual con un SwapChain mediante la interoperabilidad de Win2D. [En este ejemplo](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) se proporciona un ejemplo de cómo usar Win2D para un SpriteVisual con un swapchain de pintura.

### <a name="paint-with-a-video"></a>Pintar con un vídeo
También se puede usar un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) para pintar un área con píxeles desde un ICompositionSurface representada utilizando un vídeo cargado a través de la clase [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) .

El código siguiente muestra que un SpriteVisual a pintar con un vídeo cargado en un ICompositionSurface.

```cs
Compositor _compositor;
SpriteVisual _videoVisual;
CompositionSurfaceBrush _videoBrush;

// MediaPlayer set up with a create from URI

_mediaPlayer = new MediaPlayer();

// Get a source from a URI. This could also be from a file via a picker or a stream
var source = MediaSource.CreateFromUri(new Uri("http://go.microsoft.com/fwlink/?LinkID=809007&clcid=0x409"));
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

Un [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) dibuja un área con salida de un CompositionEffect. Efectos en la capa Visual pueden considerarse como efectos de filtro pueden animar aplicados a una colección de contenido de origen, como colores, degradados, imágenes, vídeos, swapchains, áreas de la interfaz de usuario o árboles de objetos visuales. El contenido de origen se especifica normalmente utilizando otra CompositionBrush.

En la ilustración y el código siguiente se muestra un SpriteVisual a pintar con una imagen de un gato que se ha aplicado el efecto de filtro de Desaturación.

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

Para obtener más información sobre la creación de un efecto mediante CompositionBrushes, vea [efectos de capa Visual](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Pintar con un CompositionBrush con la máscara de opacidad aplicada

Un [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) dibuja un área con un CompositionBrush con una máscara de opacidad que se ha aplicado. El origen de la máscara de opacidad puede ser cualquier CompositionBrush de tipo CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush o CompositionNineGridBrush. La máscara de opacidad debe especificarse como un CompositionSurfaceBrush.

En la ilustración y el código siguiente se muestra un SpriteVisual a pintar con un CompositionMaskBrush. El origen de la máscara es un CompositionLinearGradientBrush que se enmascara para el aspecto de un círculo con una imagen del círculo como una máscara.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Pintar con un CompositionBrush con NineGrid estiramiento

Un [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) dibuja un área con un CompositionBrush que se extiende con la metáfora de nueve cuadrículas. La metáfora nueve cuadrícula permite estirar bordes y esquinas de un CompositionBrush de manera diferente que su centro. El origen de la ajustar a la cuadrícula de nueve puede por cualquier CompositionBrush de tipo CompositionColorBrush, CompositionSurfaceBrush o CompositionEffectBrush.

El código siguiente muestra que un SpriteVisual a pintar con un CompositionNineGridBrush. El origen de la máscara es un CompositionSurfaceBrush que se extiende el uso de una cuadrícula de nueve.

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

Un [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) dibuja un área con el contenido detrás del área. Un CompositionBackdropBrush nunca se usa en su propio, pero en su lugar, se usa como una entrada en otro CompositionBrush como un EffectBrush. Por ejemplo, mediante el uso de un CompositionBackdropBrush como una entrada para un efecto de desenfoque, puede lograr un efecto de cristal.

El código siguiente muestra un pequeño árbol visual para crear una imagen mediante CompositionSurfaceBrush y una superposición de cristal por encima de la imagen. Se crea la superposición de cristal colocando una SpriteVisual rellenado con un EffectBrush por encima de la imagen. El EffectBrush utiliza un CompositionBackdropBrush como una entrada para el efecto de desenfoque.

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
Un número de CompositionBrushes usar otros CompositionBrushes como entradas. Por ejemplo, se usa el método SetSourceParameter puede utilizarse para establecer otra CompositionBrush como una entrada en un CompositionEffectBrush. En la tabla siguiente se describe las combinaciones compatibles de CompositionBrushes. Tenga en cuenta que el uso de una combinación no permitida se producirá una excepción.

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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Uso de un pincel XAML frente a CompositionBrush

En la siguiente tabla proporciona una lista de escenarios y si es que se indican XAML o la composición del uso de pincel al pintar un UIElement o un SpriteVisual en la aplicación. 

> [!NOTE]
> Si se sugiere un CompositionBrush para un elemento de IU XAML, se supone que el CompositionBrush se ha empaquetado mediante un XamlCompositionBrushBase.

|Escenario                                                                   | UIElement XAML                                                                                                |Composición SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Pintar un área con un color sólido                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área con color animado                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área con un degradado estático                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con puntos de degradado animadas                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con una imagen                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Pintar un área con una página Web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/D
|Pintar un área con una imagen con estiramiento NineGrid                         |[Control de imagen](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con animada estiramiento NineGrid                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con un swapchain                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con swapchain interoperabilidad
|Pintar un área con un vídeo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con la interoperabilidad de medios
|Pintar un área con un dibujo personalizado 2D                                       |[CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) de Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con Win2D interoperabilidad
|Pintar un área con máscara no animado                                       |Use [las formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) XAML para definir una máscara de   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con una máscara con animaciones                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con un efecto de filtro animada                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Pintar un área con un efecto que se aplica a los píxeles de fondo        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Temas relacionados

[Interoperabilidad nativa de DirectX y Direct2D de composición con BeginDraw y EndDraw](composition-native-interop.md)

[Interoperabilidad de pincel XAML con XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
