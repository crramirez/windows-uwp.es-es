---
author: jwmsft
ms.assetid: 03dd256f-78c0-e1b1-3d9f-7b3afab29b2f
title: Pinceles de composición
description: Un pincel pinta el área de un objeto Visual con su salida. Distintos pinceles tienen tipos de salida diferentes.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 730d5ae9062fe39533cd615facaf5beaa7d02ffd
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7101624"
---
# <a name="composition-brushes"></a>Pinceles de composición
Todos los elementos visibles en la pantalla de una aplicación para UWP está visible porque se ha pintado por un pincel. Pinceles te permiten pintar los objetos de la interfaz de usuario con el contenido que van desde simples colores sólidos, imágenes o dibujos en la cadena de efectos complejos. En este tema se presenta los conceptos de pintura con CompositionBrush.

Ten en cuenta al trabajar con la aplicación XAML UWP, puede elegir para pintar un UIElement con un [Pincel en XAML](/windows/uwp/design/style/brushes) o un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush). Por lo general, es aconsejable elegir un pincel XAML si tu escenario es compatible con un pincel de XAML y más fácil. Por ejemplo, animar el color de un botón, cambiar el relleno de un texto o una forma con una imagen. Por otro lado, si estás intentando hacer algo que no es compatible con un pincel XAML como el dibujo con una máscara con animaciones o un ajustar a la cuadrícula de nueve animado o una cadena de efectos, puedes usar un CompositionBrush para pintar un UIElement mediante el uso de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Al trabajar con la capa Visual, debe usarse un CompositionBrush para pintar el área de un [objeto SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Requisitos previos](./composition-brushes.md#prerequisites)
-   [Paint con CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Dibujar con un color sólido](./composition-brushes.md#paint-with-a-solid-color)
    -   [Pintar con un degradado lineal](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [Pintar con una imagen](./composition-brushes.md#paint-with-an-image)
    -   [Pintar con un dibujo personalizado](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Pintar con un vídeo](./composition-brushes.md#paint-with-a-video)
    -   [Dibujar con un efecto de filtro](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Dibujar con un CompositionBrush con una máscara de opacidad.](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Dibujar con un CompositionBrush con stretch NineGrid](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Pintar el uso de píxeles en segundo plano](./composition-brushes.md#paint-using-background-pixels)
-   [Combinar CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Usar un pincel frente a CompositionBrush XAML](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Temas relacionados](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Requisitos previos
Esta introducción se supone que estás familiarizado con la estructura de una aplicación de composición básica, como se describe en la [información general de la capa Visual](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Paint con un CompositionBrush

Un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "pinta" un área con su salida. Distintos pinceles tienen tipos de salida diferentes. Algunos pinceles pintan un área con un color sólido, otros usuarios que tengan un degradado, la imagen, el dibujo personalizado o el efecto. También hay pinceles especializados que modifican el comportamiento de otros pinceles. Por ejemplo, máscara de opacidad puede usarse para controlar el área que se pinta por un CompositionBrush o una cuadrícula de nueve puede usarse para controlar el estiramiento aplicado a un CompositionBrush cuando pintar un área. CompositionBrush puede ser de uno de los siguientes tipos:

|Clase                                   |Detalles                                         |Introducida en|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Pinta un área con un color sólido:                        |Actualización de noviembre de Windows 10 (SDK 10586)|
|[Compositionsurfacebrush.](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Pinta un área con el contenido de un [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Actualización de noviembre de Windows 10 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Pinta un área con el contenido de un efecto de composición |Actualización de noviembre de Windows 10 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Pinta un elemento visual con un CompositionBrush con una máscara de opacidad. |Actualización de aniversario de Windows 10 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Pinta un área con un CompositionBrush con un stretch NineGrid |Actualización de aniversario de Windows 10 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Pinta un área con un degradado lineal                    |Windows 10 Fall Creators Update (SDK de Insider Preview)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Pinta un área mediante el muestreo de píxeles en segundo plano de la aplicación o bien, o directamente detrás de la ventana de la aplicación en el escritorio. Se usa como entrada para otro CompositionBrush como un CompositionEffectBrush | Actualización de aniversario de Windows 10 (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Dibujar con un color sólido

Un [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) pinta un área con un color sólido. Hay varias formas de especificar el color de un SolidColorBrush. Por ejemplo, puede especificar sus alfa, rojo, verdes y azules (ARGB) los canales o usar uno de los colores predefinidos proporcionados por la clase de [colores](https://docs.microsoft.com/uwp/api/windows.ui.colors) .

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

Un [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) pinta un área con un degradado lineal. Un degradado lineal mezcla dos o más colores a lo largo de una línea, el eje de degradado. Usar objetos GradientStop para especificar los colores del degradado y sus posiciones.

La siguiente ilustración y código se muestra un objeto SpriteVisual con un LinearGradientBrush de ellas pintado con 2 se detiene con un color rojo y amarillo.

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

Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) pinta un área con los píxeles que se representa en un ICompositionSurface. Por ejemplo, un CompositionSurfaceBrush puede usarse para pintar un área con una imagen representada en una superficie de ICompositionSurface con [Loadedimagesource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API.

La siguiente ilustración y código de muestra pinta un objeto SpriteVisual con un mapa de bits de un licorice representada en un ICompositionSurface con Loadedimagesource. Las propiedades de CompositionSurfaceBrush pueden usarse para ampliar y alinear el mapa de bits dentro de los límites del elemento visual.

![Compositionsurfacebrush.](images/composition-compositionsurfacebrush.png)

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
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también puede usarse para pintar un área con los píxeles de un ICompositionSurface representada con [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) (o D2D).

El siguiente código muestra que una clase SpriteVisual pintada con una ejecución representado en un ICompositionSurface de texto con Win2D. Ten en cuenta que para poder utilizar Win2D, debes incluir el paquete de [NuGet Win2D](http://www.nuget.org/packages/Win2D.uwp) en el proyecto.

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

Del mismo modo, el CompositionSurfaceBrush también puede usarse para pintar un objeto SpriteVisual con un bien mediante la interoperabilidad de Win2D. [Este ejemplo](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) proporciona un ejemplo de cómo utilizar Win2D para pintar un objeto SpriteVisual con un bien.

### <a name="paint-with-a-video"></a>Pintar con un vídeo
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también puede usarse para pintar un área con los píxeles de un ICompositionSurface representada con un vídeo cargado a través de la clase [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) .

El siguiente código muestra que una clase SpriteVisual pintada con un vídeo que se cargan en un ICompositionSurface.

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

### <a name="paint-with-a-filter-effect"></a>Dibujar con un efecto de filtro

Un [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) pinta un área con un CompositionEffect de salida. Efectos de la capa Visual se pueden considerar como aplicado a una colección de contenido de origen, como los colores, degradados, imágenes, vídeos, cadenas de intercambio, regiones de la interfaz de usuario o los árboles de elementos visuales de efectos de filtro que se pueden animar. El contenido de origen normalmente se especifica mediante otro CompositionBrush.

La siguiente ilustración y código se muestra una clase SpriteVisual pintado con una imagen de un gato que se ha aplicado el efecto de filtro de Desaturación.

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

Para obtener más información sobre cómo crear un efecto con CompositionBrushes ver [los efectos de capa Visual](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Dibujar con un CompositionBrush con la máscara de opacidad aplicada

Un [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) pinta un área con un CompositionBrush con una máscara de opacidad aplicada. El origen de la máscara de opacidad puede ser cualquier CompositionBrush de tipo CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush o CompositionNineGridBrush. La máscara de opacidad debe especificarse como un CompositionSurfaceBrush.

La siguiente ilustración y código se muestra una clase SpriteVisual pintado con una CompositionMaskBrush. El origen de la máscara es un CompositionLinearGradientBrush que se enmascara aspecto de un círculo con una imagen de círculo como una máscara.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Dibujar con un CompositionBrush con stretch NineGrid

Un [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) pinta un área con un CompositionBrush que se amplía con la metáfora de cuadrícula de nueve. La metáfora de cuadrícula de nueve te permite estirar bordes y las esquinas de un CompositionBrush de forma diferente de su centro. El origen de la cuadrícula de nueve de stretch puede por cualquier CompositionBrush de tipo CompositionColorBrush, CompositionSurfaceBrush o CompositionEffectBrush.

El siguiente código muestra que una clase SpriteVisual pintada con una CompositionNineGridBrush. El origen de la máscara es un CompositionSurfaceBrush que se amplía usando una cuadrícula de nueve.

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

### <a name="paint-using-background-pixels"></a>Pintar el uso de píxeles en segundo plano

Un [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) pinta un área con el contenido detrás del área. Un CompositionBackdropBrush nunca se usa en su propio, pero en su lugar, se usa como entrada para otro CompositionBrush como un EffectBrush. Por ejemplo, mediante el uso de un CompositionBackdropBrush como entrada para un efecto de desenfoque, puede lograr un efecto de cristal esmerilado.

El siguiente código muestra un pequeño árbol visual para crear una imagen con CompositionSurfaceBrush y una superposición de cristal esmerilado encima de la imagen. La superposición de cristal esmerilado se crea mediante la colocación de un objeto SpriteVisual rellenado con un EffectBrush encima de la imagen. El EffectBrush usa un CompositionBackdropBrush como entrada para el efecto de desenfoque.

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
Un número de CompositionBrushes usa otros CompositionBrushes como entradas. Por ejemplo, mediante el método SetSourceParameter puede usarse para establecer otra CompositionBrush como entrada para una CompositionEffectBrush. La siguiente tabla describe las combinaciones de CompositionBrushes compatibles. Tenga en cuenta que el uso de una combinación no admitida iniciará una excepción.

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
<td>Compositionsurfacebrush.</td>
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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Usar un pincel frente a CompositionBrush XAML

La siguiente tabla proporciona una lista de escenarios y si uso de pincel XAML o composición indicado al dibujar un UIElement o una clase SpriteVisual en la aplicación. 

> [!NOTE]
> Si un CompositionBrush se sugiere para que un UIElement de XAML, se supone que el CompositionBrush se empaqueta con un objeto XamlCompositionBrushBase.

|Escenario                                                                   | UIElement XAML                                                                                                |SpriteVisual de composición
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Pintar un área con un color sólido                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área con un color animado                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área con un degradado estático                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con los delimitadores de degradado animados                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con una imagen                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[Compositionsurfacebrush.](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Pintar un área con una página Web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/A
|Pintar un área con una imagen con stretch NineGrid                         |[Control de imagen](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con animada stretch NineGrid                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con un bien                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con interoperabilidad bien
|Pintar un área con un vídeo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con interoperabilidad de multimedia
|Pintar un área con un dibujo personalizado 2D                                       |[CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) de Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con interoperabilidad de Win2D
|Pintar un área con la máscara no animado                                       |Usar [formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) XAML para definir una máscara   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con una máscara animado.                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con un efecto de filtro animados                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Pintar un área con un efecto que se aplica a los píxeles en segundo plano        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Temas relacionados

[Composición nativa DirectX y Direct2D la interoperabilidad con BeginDraw y EndDraw](composition-native-interop.md)

[Interoperabilidad de XAML pincel con XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
