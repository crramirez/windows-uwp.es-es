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
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919792"
---
# <a name="composition-brushes"></a>Pinceles de composición
Todos los elementos visibles en la pantalla de una aplicación UWP está visible porque lo ha pintado un pincel. Pinceles permiten pintar los objetos de la interfaz (IU) de usuario con contenido desde simples colores sólidos hasta imágenes o dibujos a la cadena de efectos complejos. Este tema presenta los conceptos de la pintura con CompositionBrush.

Tenga en cuenta cuando se trabaja con la aplicación XAML UWP, usted puede elegir para pintar un UIElement con un [Pincel en XAML](/windows/uwp/design/style/brushes) o un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush). Normalmente, resulta más fácil y conveniente para elegir un pincel XAML si el escenario es compatible con un pincel en XAML. Por ejemplo, animar el color de un botón, cambiar el relleno de un texto o una forma con una imagen. Por otra parte, si intenta hacer algo que no es compatible con un pincel XAML tal como la pintura con una máscara con animaciones o un estiramiento de nueve cuadrículas animado o una cadena de efecto, puede utilizar un CompositionBrush para pintar un objeto UIElement mediante el uso de [ XamlCompositionBrushBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

Cuando se trabaja con la capa Visual, un CompositionBrush debe utilizarse para pintar el área de un [SpriteVisual](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual).

-   [Requisitos previos](./composition-brushes.md#prerequisites)
-   [Pintar con CompositionBrush](./composition-brushes.md#paint-with-a-compositionbrush)
    -   [Pintar con un color sólido](./composition-brushes.md#paint-with-a-solid-color)
    -   [Pintar con un degradado lineal](./composition-brushes.md#paint-with-a-linear-gradient)
    -   [Pintar con una imagen](./composition-brushes.md#paint-with-an-image)
    -   [Pintar con un dibujo personalizado](./composition-brushes.md#paint-with-a-custom-drawing)
    -   [Pintar con un vídeo](./composition-brushes.md#paint-with-a-video)
    -   [Pintar con un efecto de filtro](./composition-brushes.md#paint-with-a-filter-effect)
    -   [Pintar con un CompositionBrush con una máscara de opacidad](./composition-brushes.md#paint-with-a-compositionbrush-with-opacity-mask-applied)
    -   [Pintar con un CompositionBrush con NineGrid stretch](./composition-brushes.md#paint-with-a-compositionbrush-using-ninegrid-stretch)
    -   [Pintura con píxeles de fondo](./composition-brushes.md#paint-using-background-pixels)
-   [Combinar CompositionBrushes](./composition-brushes.md#combining-compositionbrushes)
-   [Utilizando un pincel XAML frente a CompositionBrush](./composition-brushes.md#using-a-xaml-brush-vs-compositionbrush)
-   [Temas relacionados](./composition-brushes.md#related-topics)

## <a name="prerequisites"></a>Requisitos previos
Esta información general se supone que está familiarizado con la estructura de una aplicación de composición básica, como se describe en la [información general de la capa Visual](visual-layer.md).

## <a name="paint-with-a-compositionbrush"></a>Pintar con un CompositionBrush

Un [CompositionBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBrush) "pinta" un área con su salida. Distintos pinceles tienen tipos de salida diferentes. Algunos pinceles pintan un área con un color sólido, otros con un degradado, imagen, dibujo personalizado o efecto. También hay pinceles especializados que modifican el comportamiento de otros pinceles. Por ejemplo, máscara de opacidad puede utilizarse para controlar el área que se pinta con un CompositionBrush o una cuadrícula de nueve puede utilizarse para controlar el estiramiento aplicado a un CompositionBrush que al pintar un área. CompositionBrush puede ser de uno de los siguientes tipos:

|Clase                                   |Detalles                                         |Introducido en|
|-------------------------------------|---------------------------------------------------------|--------------------------------------|
|[CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush)         |Pinta un área con un color sólido                        |Actualización de noviembre de Windows10 (SDK 10586)|
|[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush)       |Pinta un área con el contenido de un [ICompositionSurface](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.ICompositionSurface)|Actualización de noviembre de Windows10 (SDK 10586)|
|[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)        |Pinta un área con el contenido de un efecto de composición |Actualización de noviembre de Windows10 (SDK 10586)|
|[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)          |Pinta un objeto visual con un CompositionBrush con una máscara de opacidad |Actualización de aniversario Windows10 (SDK 14393)
|[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)      |Pinta un área con un CompositionBrush con un tramo de NineGrid |Actualización de aniversario Windows10 SDK (14393)
|[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)|Pinta un área con un degradado lineal                    |Windows 10 caen creadores actualización (Insider Preview SDK)
|[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)     |Pinta un área muestreando píxeles de fondo desde ya sea la aplicación, o directamente detrás de la ventana de aplicación en el escritorio. Utilizado como entrada a otra CompositionBrush como un CompositionEffectBrush | Actualización de Windows 10 aniversario (SDK 14393)

### <a name="paint-with-a-solid-color"></a>Pintar con un color sólido

Una [CompositionColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionColorBrush) pinta un área con un color sólido. Hay una gran variedad de maneras de especificar el color de un objeto SolidColorBrush. Por ejemplo, puede especificar sus alfa, rojo, verde y azules (ARGB) los canales o utilice uno de los colores predefinidos proporcionados por la clase de [colores](https://docs.microsoft.com/uwp/api/windows.ui.colors) .

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

Una [CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush) pinta un área con un degradado lineal. Un degradado lineal mezcla dos o más colores a lo largo de una línea, el eje de degradado. Utilice objetos GradientStop para especificar los colores del degradado y sus posiciones.

La siguiente ilustración y código muestra una SpriteVisual pinta con un LinearGradientBrush con 2 paradas con un color rojo y amarillo.

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

Una [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) pinta un área con píxeles representadas en un ICompositionSurface. Por ejemplo, un CompositionSurfaceBrush puede utilizarse para pintar una área con una imagen representada en una superficie de ICompositionSurface con [LoadedImageSurface](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.loadedimagesurface) API.

La siguiente ilustración y código muestra una SpriteVisual pintada con un mapa de bits de un regaliz representada en un ICompositionSurface con LoadedImageSurface. Las propiedades de CompositionSurfaceBrush pueden utilizarse para ajustar y alinear el mapa de bits dentro de los límites del objeto visual.

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
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también puede utilizarse para pintar una área con píxeles de un ICompositionSurface representado utilizando [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) (o D2D).

El código siguiente muestra que un SpriteVisual pintado con una ejecución representado en un ICompositionSurface de texto mediante Win2D. Nota para poder utilizar Win2D, debe incluir el paquete [NuGet Win2D](http://www.nuget.org/packages/Win2D.uwp) en el proyecto.

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

De forma similar, el CompositionSurfaceBrush puede utilizarse para pintar una SpriteVisual con un SwapChain mediante la interoperabilidad de Win2D. [Este ejemplo](https://github.com/Microsoft/Win2D-Samples/tree/master/CompositionExample) proporciona un ejemplo de cómo utilizar Win2D para pintar un SpriteVisual con un swapchain.

### <a name="paint-with-a-video"></a>Pintar con un vídeo
Un [CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) también puede utilizarse para pintar una área con píxeles de un ICompositionSurface que se representa mediante un vídeo cargado a través de la clase [MediaPlayer](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) .

El código siguiente muestra que un SpriteVisual pintada con un vídeo cargado en un ICompositionSurface.

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

Una [CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush) pinta un área con la salida de un CompositionEffect. Efectos en la capa Visual pueden considerarse como efectos de filtro pueden animar aplicados a una colección de contenido de origen, como colores, degradados, imágenes, vídeos, swapchains, regiones de la interfaz de usuario o árboles de objetos visuales. El contenido de origen normalmente se especifica mediante otro CompositionBrush.

La siguiente ilustración y código muestra un SpriteVisual pintado con una imagen de un gato que se ha aplicado el efecto de filtro de Desaturación.

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

Para obtener más información acerca de cómo crear un efecto de CompositionBrushes ver [los efectos en la capa Visual](https://docs.microsoft.com/en-us/windows/uwp/composition/composition-effects)

### <a name="paint-with-a-compositionbrush-with-opacity-mask-applied"></a>Pintar con un CompositionBrush con la máscara de opacidad aplicada

Una [CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush) pinta un área con un CompositionBrush con una máscara de opacidad aplicada. El origen de la máscara de opacidad puede ser cualquier CompositionBrush de tipo CompositionColorBrush, CompositionLinearGradientBrush, CompositionSurfaceBrush, CompositionEffectBrush o CompositionNineGridBrush. La máscara de opacidad debe especificarse como un CompositionSurfaceBrush.

La siguiente ilustración y código muestra una pintada con un CompositionMaskBrush de SpriteVisual. El origen de la máscara es un CompositionLinearGradientBrush que se enmascara como un círculo con una imagen del círculo como una máscara.

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

### <a name="paint-with-a-compositionbrush-using-ninegrid-stretch"></a>Pintar con un CompositionBrush con NineGrid stretch

Una [CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush) pinta un área con un CompositionBrush que se expande mediante la metáfora de nueve cuadrículas. La metáfora de nueve cuadrículas permite estirar los bordes y las esquinas de un CompositionBrush de forma diferente que su centro. El origen del tramo de nueve cuadrículas puede por cualquier CompositionBrush de tipo CompositionColorBrush, CompositionSurfaceBrush o CompositionEffectBrush.

El código siguiente muestra que un SpriteVisual pintada con un CompositionNineGridBrush. El origen de la máscara es un CompositionSurfaceBrush que se expande mediante una cuadrícula de nueve.

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

### <a name="paint-using-background-pixels"></a>Pintura con píxeles de fondo

Una [CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush) pinta un área con el contenido de la zona. Un CompositionBackdropBrush nunca se utiliza por sí mismo, pero en su lugar se utiliza como entrada a otra CompositionBrush como un EffectBrush. Por ejemplo, utilizando un CompositionBackdropBrush como una entrada para un efecto de desenfoque, puede lograr un efecto de vidrio esmerilado.

El código siguiente muestra un pequeño árbol visual para crear una imagen con una superposición de cristal encima de la imagen y de CompositionSurfaceBrush. La superposición de vidrio esmerilado se crea mediante la colocación de un SpriteVisual que se rellena con un EffectBrush encima de la imagen. El EffectBrush utiliza un CompositionBackdropBrush como una entrada para el efecto de desenfoque.

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
Un número de CompositionBrushes usar otros CompositionBrushes como entradas. Por ejemplo, mediante el método SetSourceParameter permite establecer otro CompositionBrush como entrada a un CompositionEffectBrush. La tabla siguiente describe las combinaciones compatibles de CompositionBrushes. Tenga en cuenta que el uso de una combinación no permitida se iniciará una excepción.

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


## <a name="using-a-xaml-brush-vs-compositionbrush"></a>Utilizando un pincel XAML frente a CompositionBrush

En la siguiente tabla proporciona una lista de escenarios y si se prescribe el uso de pincel XAML o composición al pintar un objeto UIElement o un SpriteVisual en la aplicación. 

> [!NOTE]
> Si se sugiere un CompositionBrush para un elemento de IU XAML, se supone que el CompositionBrush se ha empaquetado mediante un XamlCompositionBrushBase.

|Escenario                                                                   | UIElement XAML                                                                                                |Composición SpriteVisual
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------------------------------
|Pintar un área con un color sólido                                             |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área con color animado                                          |[SolidColorBrush](https://msdn.microsoft.com/library/windows/apps/BR242962)                                |[CompositionColorBrush](https://msdn.microsoft.com/library/windows/apps/Mt589399)
|Pintar un área con un degradado estático                                       |[LinearGradientBrush](https://msdn.microsoft.com/library/windows/apps/BR210108)                            |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con degradado animada                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)                                                                                 |[CompositionLinearGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionlineargradientbrush)
|Pintar un área con una imagen                                                |[ImageBrush](https://msdn.microsoft.com/library/windows/apps/BR210101)                                     |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415)
|Pintar un área con una página Web                                               |[WebViewBrush](https://msdn.microsoft.com/library/windows/apps/BR227703)                                   |N/A
|Pintar un área con una imagen mediante la ampliación de NineGrid                         |[Control de imagen](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)                   |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con estiramiento de NineGrid animado                               |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)                                                                                       |[CompositionNineGridBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionNineGridBrush)
|Pintar un área con un swapchain                                             |[SwapChainPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel)                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con interoperabilidad swapchain
|Pintar un área con un vídeo                                                 |[MediaElement](https://msdn.microsoft.com/library/windows/apps/mt187272.aspx)                                                                                                  |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con la interoperabilidad de los medios de comunicación
|Pintar un área con un dibujo personalizado 2D                                       |[CanvasControl](http://microsoft.github.io/Win2D/html/T_Microsoft_Graphics_Canvas_UI_Xaml_CanvasControl.htm) de Win2D                                                                                                 |[CompositionSurfaceBrush](https://msdn.microsoft.com/library/windows/apps/Mt589415) con Win2D interoperabilidad
|Pintar un área con máscara sin animaciones                                       |Utilizar [formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes) XAML para definir una máscara   |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con una máscara con animaciones                                        |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)                                                                                           |[CompositionMaskBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionMaskBrush)
|Pintar un área con un efecto de filtro animado                               |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)                                                                                         |[CompositionEffectBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionEffectBrush)
|Pintar un área con un efecto que se aplica a los píxeles de fondo        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)                                                                                        |[CompositionBackdropBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.CompositionBackdropBrush)

## <a name="related-topics"></a>Temas relacionados

[Interoperabilidad nativa de DirectX y Direct2D de composición con BeginDraw y EndDraw](composition-native-interop.md)

[Interoperabilidad de pincel XAML con XamlCompositionBrushBase](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)
