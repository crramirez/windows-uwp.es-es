---
Description: Aprende a integrar imágenes en la aplicación, así como a usar las API de las dos clases principales de XAML, Image e ImageBrush.
title: Imágenes y pinceles de imagen
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 61fa4f8afa0404591831be4136c16672503274f2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362782"
---
# <a name="images-and-image-brushes"></a>Imágenes y pinceles de imagen

Para mostrar una imagen, puedes usar los objetos **Image** o **ImageBrush**. Un objeto Image representa una imagen y un objeto ImageBrush pinta otro objeto con una imagen. 

> **API importantes**: [Clase de la imagen](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), [del origen de propiedad](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source), [clase ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush), [propiedad ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)

## <a name="are-these-the-right-elements"></a>¿Son estos los elementos adecuados?
Usa un elemento **Image** para mostrar una imagen independiente en la aplicación.

Usa un elemento **ImageBrush** para aplicar una imagen a otro objeto. Entre los usos de ImageBrush, se incluyen efectos decorativos para el texto o fondos para los controles o los contenedores de diseño.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Image">abrir la aplicación y ver la imagen en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>Crear una imagen

### <a name="image"></a>Image
En este ejemplo se muestra cómo crear una imagen con el objeto [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image).


```XAML
<Image Width="200" Source="sunset.jpg" />
```

Este es el objeto Image representado.

![Ejemplo de un elemento de imagen](images/Image_Licorice.jpg)

En este ejemplo, la propiedad [Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) especifica la ubicación de la imagen que quieres mostrar. Puede establecer el origen mediante la especificación de una dirección URL absoluta (por ejemplo, http://contoso.com/myPicture.jpg) o especificando una dirección URL que sea relativa a la estructura de empaquetado de la aplicación. En nuestro ejemplo, el archivo de imagen "licorice.jpg" se ubica en la carpeta raíz del proyecto y se declara la configuración del proyecto que incluye el archivo de imagen como contenido.

### <a name="imagebrush"></a>ImageBrush

Con el objeto [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) puedes utilizar una imagen para pintar un área que admita un objeto [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush). Por ejemplo, puedes usar un objeto ImageBrush para el valor de la propiedad [Fill](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) de un elemento [Ellipse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) o la propiedad [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background) de un elemento [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas).

En el siguiente ejemplo se muestra cómo usar ImageBrush para pintar un elemento Ellipse.

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Este es el elemento Ellipse que ha pintado ImageBrush.

![Un elemento Ellipse que ha pintado ImageBrush.](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>Ajustar una imagen

Si no se establecen los valores [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) o [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) de un elemento **Image**, se representa con las dimensiones que especifique la propiedad **Source**. Cuando se establecen los valores **Width** y **Height**, se crea un área rectangular contenedora donde se muestra la imagen. Es posible especificar la forma en que la imagen rellena esta área contenedora mediante el uso de la propiedad [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.stretch). La propiedad Stretch acepta estos valores, que define la enumeración [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch):

-   **Ninguna**: La imagen no se ajusta para rellenar las dimensiones de salida. Ten cuidado con esta configuración de Stretch; si la imagen de origen es más grande que el área contenedora, la imagen se recortará, lo cual no es deseable, ya que no tendrás control sobre la ventanilla, que sí tendrías con un elemento [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) intencionado.
-   **Uniforme**: La imagen se escala para ajustarse a las dimensiones de salida. aunque la relación de aspecto del contenido se mantiene. Este es el valor predeterminado.
-   **UniformToFill**: La imagen se escala para que rellene el área de salida completamente pero conserva su relación de aspecto original.
-   **Rellenar**: La imagen se escala para ajustarse a las dimensiones de salida. Como el alto y el ancho del contenido se escalan de forma independiente, la relación de aspecto original de la imagen quizás no se mantenga. En otras palabras, la imagen podría distorsionarse para rellenar completamente el área de salida.

![Ejemplo de las configuraciones de Stretch.](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>Recortar una imagen

Puedes usar la propiedad [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) para recortar un área de la imagen de salida. Para ello, establece la propiedad Clip en un valor [Geometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Geometry). Actualmente, no se admiten los recortes no rectangulares.

En el siguiente ejemplo se muestra cómo usar un objeto [RectangleGeometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry) como la región de recorte de una imagen. En este ejemplo, se define un objeto **Image** con un valor de alto de 200. Un elemento **RectangleGeometry** define un rectángulo para el área de la imagen que se mostrará. La propiedad [Rect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rectanglegeometry.rect) está establecida en "25,25,100,150", que define un rectángulo que comienza en la posición "25,25" con un ancho de 100 y un alto de 150. Solo se muestra la parte de la imagen que está dentro del área del rectángulo.

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

Esta es la imagen recortada sobre un fondo negro.

![Un objeto Image recortado por RectangleGeometry.](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>Aplicar una opacidad

Puedes aplicar una propiedad [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity) a una imagen para que se represente semitranslúcida. Los valores de opacidad van de 0,0 a 1,0; donde 1,0 es totalmente opaco y 0,0 es totalmente transparente. En este ejemplo se muestra cómo aplicar una opacidad de 0,5 a un elemento Image.

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
```

Esta es la imagen representada con una opacidad de 0,5 y un fondo negro que se ve a través de la opacidad parcial.

![Un objeto Image con una opacidad de 0,5.](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>Formatos de archivos de imagen

**Image** e **ImageBrush** pueden mostrar los siguientes formatos de archivo de imagen:

-   Formato JPEG (Joint Photographic Experts Group)
-   Formato PNG (Portable Network Graphics)
-   Mapa de bits (BMP)
-   Formato de intercambio de gráficos (GIF)
-   Tagged Image File Format (TIFF)
-   JPEG XR
-   iconos (ICO)

Las API para [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) y [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) no incluyen ningún método dedicado para codificar y descodificar formatos multimedia. Todas las operaciones de codificación y descodificación están integradas y, como mucho, mostrarán aspectos de la codificación o descodificación como parte de los datos de evento para los eventos de carga. Si quieres realizar trabajo especializado con la codificación o descodificación de imágenes, que podrías usar si la aplicación realiza manipulaciones o conversiones de imágenes, deberías usar las API que están disponibles en el espacio de nombres [Windows.Graphics.Imaging](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging). Estas API también son compatibles con Windows Imaging Component (WIC) en Windows.

A partir de Windows 10, versión 1607, el elemento **Image** admite imágenes GIF animadas. Si usas un elemento **BitmapImage** como propiedad **Source** de la imagen, puedes acceder a las API de BitmapImage para controlar la reproducción de la imagen GIF animada. Para obtener más información, consulta los comentarios en la página sobre la clase [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage).

> **Nota**&nbsp;&nbsp;La compatibilidad con GIF animados está disponible si tu aplicación está compilada para Windows 10, versión 1607 y se ejecuta en la versión 1607 (o posterior). Si tu aplicación está compilada para versiones anteriores o se ejecuta en versiones anteriores, el primer fotograma del GIF se muestra, pero no animado.

Para obtener más información sobre los recursos de la aplicación y sobre cómo empaquetar orígenes de imagen en una aplicación, consulta [Definición de recursos de una aplicación](https://docs.microsoft.com/previous-versions/windows/apps/hh965321(v=win.10)).

### <a name="writeablebitmap"></a>WriteableBitmap

Un elemento [WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) proporciona una clase [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) que se puede modificar y que no usa la descodificación basada en archivos básica de WIC. Puedes modificar imágenes de forma dinámica y volver a representar la imagen actualizada. Para definir el contenido del búfer de un elemento **WriteableBitmap**, usa la propiedad [PixelBuffer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer) para acceder al búfer y utiliza un tipo de búfer de secuencia o específico del lenguaje para rellenarlo. Para obtener código de ejemplo, consulta [WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap).

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

La clase [RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) captura el árbol de la interfaz de usuario de XAML de una aplicación en funcionamiento y después muestra un origen de imagen de mapa de bits. Tras la captura, dicho origen de imagen se puede aplicar a otras partes de la aplicación, guardar como un recurso o dato de la aplicación o emplear en otros escenarios. Un escenario especialmente útil es crear una miniatura en tiempo de ejecución de una página XAML para un esquema de navegación, como al proporcionar un vínculo de imagen desde un control [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub). **RenderTargetBitmap** presenta algunas limitaciones en cuanto al contenido que va a aparecer en la imagen capturada. Para más información, consulta el tema de referencia de API para [RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap).

### <a name="image-sources-and-scaling"></a>Orígenes y escala de las imágenes

Deberías crear los orígenes de imágenes con varios tamaños recomendados para garantizar que la aplicación tenga una apariencia excelente cuando Windows cambie su escala. Cuando se especifica una propiedad **Source** para un elemento **Image**, puedes usar una convención de nomenclatura que haga referencia automáticamente al recurso correcto para la escala actual. Para obtener información específica de la convención de nomenclatura y obtener más información, consulte [inicio rápido: Uso de recursos de imagen o archivo](https://docs.microsoft.com/previous-versions/windows/apps/hh965325(v=win.10)).

Para más información sobre cómo diseñar teniendo en cuenta la escala, consulta [Directrices sobre la experiencia del usuario para diseño y escalado](https://developer.microsoft.com/windows/design).

### <a name="image-and-imagebrush-in-code"></a>Image e ImageBrush en código

Es habitual especificar elementos Image e ImageBrush con XAML en lugar de código. Esto es así porque estos elementos suelen ser el resultado de herramientas de diseño como parte de una definición de interfaz de usuario de XAML.

Si defines un elemento Image o ImageBrush mediante código, usa los constructores predeterminados, establece la propiedad Source relevante ([Image.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) o [ImageBrush.ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)). Las propiedades Source requieren un elemento [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (no un URI) cuando se establecen con código. Si el origen es una secuencia, usa el método [SetSourceAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) para inicializar el valor. Si el origen es un URI, que incluye contenido en la aplicación que usa los esquemas **ms-appx** o **ms-resource**, utiliza el constructor [BitmapImage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.) que toma un URI. También podrías considerar la posibilidad de controlar el evento [ImageOpened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened) si hay problemas de temporización con la recuperación o la descodificación del origen de la imagen, ya que puede que necesites mostrar contenido alternativo hasta que el origen de la imagen esté disponible. Para obtener código de ejemplo, consulta [Muestra de imágenes XAML](https://go.microsoft.com/fwlink/p/?linkid=238575).

> [!NOTE]
> Si estableces imágenes mediante código, puedes usar el control automático para obtener acceso a recursos sin calificar con calificadores de referencia cultural y de escala actuales, o bien puedes usar [ResourceManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) y [ResourceMap](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap) con calificadores de escala y referencia cultural para obtener los recursos directamente. Para obtener más información, consulta [Sistema de administración de recursos](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)).

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

-   [Audio, vídeo y cámara](https://docs.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Clase de imagen](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)
-   [Clase ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)