---
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Usar pinceles
description: Los objetos Brush se usan para pintar los interiores o los contornos de formas, texto y partes de los controles, de forma que el objeto pintado sea visible en una interfaz de usuario.
ms.date: 04/28/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9e70c76f3ed659a46dd9834442049849dd3b7761
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175529"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Usar pinceles para pintar fondos, primeros planos y esquemas

Usa los objetos [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) para pintar los interiores y los contornos del texto, los controles y las formas XAML, de modo que estén visibles en la interfaz de usuario de la aplicación.

> **API importantes**:  [Clase Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Introducción a los pinceles

Para pintar un objeto [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape), texto o partes de un objeto [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) que se muestra en el lienzo de la aplicación, establece la propiedad [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) del objeto **Shape** o [**Background**](/uwp/api/windows.ui.xaml.controls.control.background) y las propiedades [**Foreground**](/uwp/api/windows.ui.xaml.controls.control.foreground) de un objeto **Control** en el valor **Brush**.

Los diferentes tipos de pinceles son: 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)
-   [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) 
-   [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) 
-   [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush)
-   [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush)
-   [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Pinceles de color sólido

Un objeto [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) pinta un área con un único [**Color**](/uwp/api/Windows.UI.Color), como rojo o azul. Este es el pincel más básico. En XAML hay tres maneras de definir un objeto **SolidColorBrush** y el color que especifica: nombres de color predefinidos, valores de color hexadecimales o la sintaxis de los elementos de las propiedades.

### <a name="predefined-color-names"></a>Nombres de color predefinidos

Puedes usar un nombre de color predefinido, como [**Yellow**](/uwp/api/windows.ui.colors.yellow) o [**Magenta**](/uwp/api/windows.ui.colors.magenta). Hay 256 colores con nombre disponibles. El analizador XAML convierte el nombre del color en una estructura [**Color**](/uwp/api/Windows.UI.Color) con los canales de color correctos. Los 256 colores con nombre se basan en los nombres de colores *X11* de la especificación de hojas de estilo CSS de nivel 3 (CSS3), por lo que es posible que ya conozcas esta lista de colores con nombre si tienes experiencia en desarrollo o diseño web.

El ejemplo siguiente establece la propiedad [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) de un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) en el color predefinido [**Red**](/uwp/api/windows.ui.colors.red).

```xaml
<Rectangle Width="100" Height="100" Fill="Red" />
```

![Objeto SolidColorBrush representado](images/brushes-solidcolorbrush.jpg)

*SolidColorBrush aplicado a un rectángulo*

Si defines un objeto [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) mediante programación en lugar de usar XAML, cada color con nombre está disponible como un valor de propiedad estático de la clase [**Colors**](/uwp/api/windows.ui.colors). Por ejemplo, para declarar un valor [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) de un objeto **SolidColorBrush** para representar el color con nombre "Orchid", establece el valor de **Color** en el valor estático [**Colors.Orchid**](/uwp/api/windows.ui.colors.orchid).

### <a name="hexadecimal-color-values"></a>Valores de color hexadecimales

Puedes usar una cadena de formato hexadecimal para declarar valores precisos de color de 24 bits con un canal alfa de 8 bits para un objeto [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Dos caracteres del intervalo 0 a F definen cada valor de componente y el orden de los valores de componente de la cadena hexadecimal es: canal alfa (opacidad), canal rojo, canal verde y canal azul (**ARGB**). Por ejemplo, el valor hexadecimal "\#FFFF0000" define un rojo completamente opaco (alfa="FF", rojo="FF", verde="00" y azul="00").

En este ejemplo de XAML se establece la propiedad [**Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill) de un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) en el valor hexadecimal "\#FFFF0000", cuyo resultado es idéntico al uso del color con nombre [**Colors.Red**](/uwp/api/windows.ui.colors.red).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="property-element-syntax"></a>Sintaxis de elemento de propiedad

Puedes usar la sintaxis de los elementos de las propiedades para definir un objeto [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush). Esta sintaxis es más detallada que los métodos anteriores, pero puedes especificar valores de propiedades adicionales en un elemento, como el valor de [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity). Para obtener más información sobre la sintaxis XAML, incluida la sintaxis de los elementos de las propiedades, consulta [Introducción a XAML](../../xaml-platform/xaml-overview.md) y [Guía de sintaxis XAML](../../xaml-platform/xaml-syntax-guide.md).

En los ejemplos anteriores, el pincel se crea de forma implícita y automática, como parte de un método abreviado deliberado del lenguaje XAML que ayuda a simplificar las definiciones de la interfaz de usuario en los casos más comunes. El ejemplo siguiente crea un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) y crea explícitamente el objeto [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) como un valor de elemento para una propiedad [**Rectangle.Fill**](/uwp/api/windows.ui.xaml.shapes.shape.fill). El objeto [**Color**](/uwp/api/windows.ui.xaml.media.solidcolorbrush.color) de **SolidColorBrush** se establece en [**Blue**](/uwp/api/windows.ui.colors.blue) y el objeto [**Opacity**](/uwp/api/windows.ui.xaml.media.brush.opacity) se establece en 0,5.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="linear-gradient-brushes"></a>Pinceles de degradado lineal

Un objeto [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) pinta un área con un degradado que se define a lo largo de una línea. Esta línea se denomina *eje de degradado*. Los colores del degradado y su ubicación en el eje de degradado se especifican con objetos [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop). De manera predeterminada, el eje de degradado va de la esquina superior izquierda a la esquina inferior derecha de la superficie que pinta el pincel, lo que produce un sombreado en diagonal.

El objeto [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) es el bloque de creación básico de un pincel de degradado. Un delimitador de degradado especifica que el objeto [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) del pincel se encuentra en un objeto [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) a lo largo del eje de degradado, cuando el pincel se aplica a la superficie que se está pintando.

El color del delimitador de degradado se especifica por medio de su propiedad [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color). El color se puede establecer usando un nombre de color predefinido o mediante valores **ARGB** hexadecimales.

La propiedad [**Offset**](/uwp/api/windows.ui.xaml.media.gradientstop.offset) de un objeto [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop) especifica la posición de cada objeto **GradientStop** en el eje de degradado. Un objeto **Offset** es de tipo **double** con valores entre 0 y 1. Un objeto **Offset** con un valor de 0 sitúa el objeto **GradientStop** al principio del eje de degradado; es decir, junto a su [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint). Un objeto **Offset** con un valor de 1 sitúa el objeto **GradientStop** en su [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint). Como mínimo, un objeto [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) debe tener dos valores de **GradientStop**, en los que cada objeto **GradientStop** debe especificar un valor de [**Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) distinto y tener un valor de **Offset** entre 0 y 1.

En este ejemplo se crea un degradado lineal con cuatro colores y se usa para pintar un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<!-- This rectangle is painted with a diagonal linear gradient. -->
<Rectangle Width="200" Height="100">
    <Rectangle.Fill>
        <LinearGradientBrush StartPoint="0,0" EndPoint="1,1">
            <GradientStop Color="Yellow" Offset="0.0" x:Name="GradientStop1"/>
            <GradientStop Color="Red" Offset="0.25" x:Name="GradientStop2"/>
            <GradientStop Color="Blue" Offset="0.75" x:Name="GradientStop3"/>
            <GradientStop Color="LimeGreen" Offset="1.0" x:Name="GradientStop4"/>
        </LinearGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

El color de cada punto entre los delimitadores de degradado se interpola linealmente como una combinación del color especificado por los dos delimitadores de degradado limítrofes. En la imagen siguiente, se resaltan los delimitadores de degradado del ejemplo anterior. Los círculos marcan la posición de los delimitadores de degradado y la línea punteada muestra el eje de degradado.

![Delimitadores de degradado](images/linear-gradients-stops.png)

*Combinación de colores especificada por los dos delimitadores de degradado*

Para cambiar la línea en la que se sitúan los delimitadores de degradado, establece las propiedades [**StartPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.startpoint) y [**EndPoint**](/uwp/api/windows.ui.xaml.media.lineargradientbrush.endpoint) en valores distintos a los valores de inicio predeterminados, `(0,0)` y `(1,1)`. Si cambias los valores de coordenadas de **StartPoint** y **EndPoint**, puedes crear degradados horizontales o verticales, invertir el sentido de degradado o condensar la extensión del degradado para aplicarlo en un área más reducida que la superficie pintada completa. Para condensar el degradado, debes establecer los valores de **StartPoint** o **EndPoint** en un valor entre 0 y 1. Por ejemplo, si deseas un degradado horizontal en el que toda la atenuación se produzca en la mitad izquierda del pincel y el lado derecho sea sólido con el último color de [**GradientStop**](/uwp/api/Windows.UI.Xaml.Media.GradientStop), debes especificar un valor de **StartPoint** de `(0,0)` y un valor de **EndPoint** de `(0.5,0)`.

### <a name="use-tools-to-make-gradients"></a>Usar herramientas para hacer degradados

Ahora que ya sabes cómo funcionan los degradados lineales, puedes usar Visual Studio o Blend para crear estos degradados con facilidad. Para crear un degradado, selecciona el objeto al que deseas aplicar un degradado en la superficie de diseño o en la vista XAML. Expande la opción **Pincel** y selecciona la pestaña **Degradado lineal**.

![Creación de un degradado lineal con Visual Studio](images/tool-gradient-brush-1.png)

*Creación de un degradado lineal con Visual Studio*

Ahora puedes cambiar los colores de los delimitadores de degradado y deslizar sus posiciones con la barra situada en la parte inferior. Para agregar nuevos delimitadores de degradado, haz clic en la barra. Para quitar delimitadores, arrástralos fuera de la barra (consulta la siguiente captura de pantalla).

![Barra situada en la parte inferior de la ventana de propiedades que controla los delimitadores de degradado](images/tool-gradient-brush-2.png)

*Control deslizante de configuración del degradado*

## <a name="radial-gradient-brushes"></a>Pinceles del degradado radial

Se dibuja un objeto [**RadialGradientBrush**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush) en una elipse definida por las propiedades [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) y [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy). Los colores del degradado se inician en el centro de la elipse y terminan en el radio.

Los colores del degradado radial se definen según los delimitadores de color agregados a la propiedad de colección [**GradientStops**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientstops). Cada delimitador de degradado especifica un color y un desplazamiento a lo largo del degradado.

El origen del degradado se establece de manera predeterminada en el centro y se puede desplazar con la propiedad [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin).

[MappingMode](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) define si las propiedades [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx), [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy) y [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin) representan coordenadas relativas o absolutas.

Cuando [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) se establece en `RelativeToBoundingBox`, los valores X e Y de las tres propiedades se tratan como relativos en los límites del elemento, en que `(0,0)` representa la parte superior izquierda, `(1,1)` representa la parte inferior derecha de los límites del elemento para las propiedades [**Center**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.center), [**RadiusX**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusx) y [**RadiusY**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.radiusy), y `(0,0)` representa el centro de la propiedad [**GradientOrigin**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.gradientorigin).

Cuando [**MappingMode**](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush.mappingmode) se establece en `Absolute`, los valores X e Y de las tres propiedades se tratan como coordenadas absolutas dentro de los límites del elemento.

En este ejemplo se crea un degradado lineal con cuatro colores y se usa para pintar un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<!-- This rectangle is painted with a radial gradient. -->
<Rectangle Width="200" Height="200">
    <Rectangle.Fill>
        <media:RadialGradientBrush>
            <GradientStop Color="Blue" Offset="0.0" />
            <GradientStop Color="Yellow" Offset="0.2" />
            <GradientStop Color="LimeGreen" Offset="0.4" />
            <GradientStop Color="LightBlue" Offset="0.6" />
            <GradientStop Color="Blue" Offset="0.8" />
            <GradientStop Color="LightGray" Offset="1" />
        </media:RadialGradientBrush>
    </Rectangle.Fill>
</Rectangle>
```

El color de cada punto entre los delimitadores de degradado se interpola de forma radial como una combinación del color especificado por los dos delimitadores de degradado. En la imagen siguiente, se resaltan los delimitadores de degradado del ejemplo anterior. 

![Delimitadores de degradado](images/radial-gradient.png)

*Delimitadores de degradado*

## <a name="image-brushes"></a>Pinceles de imagen

Un objeto [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) pinta una superficie con una imagen que proviene de un origen de archivo de imagen. La propiedad [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) se establece con la ruta de acceso de la imagen que se va a cargar. Normalmente, el origen de imagen proviene de un elemento **Content** que forma parte de los recursos de la aplicación.

De manera predeterminada, un objeto [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) amplía su imagen para rellenar completamente la superficie pintada, lo que puede distorsionar la imagen si las relaciones de aspecto de dicha superficie y de la imagen son diferentes. Para cambiar este comportamiento puedes cambiar la propiedad [**Stretch**](/uwp/api/windows.ui.xaml.media.tilebrush.stretch) de su valor predeterminado **Fill** a **None**, **Uniform** o **UniformToFill**.

El ejemplo siguiente crea un objeto [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) y establece el objeto [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource) en una imagen llamada licorice.jpg que debes incluir como un recurso en la aplicación. A continuación, el objeto **ImageBrush** pinta la superficie definida por una forma [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

![Objeto ImageBrush representado.](images/brushes-imagebrush.jpg)

*Objeto ImageBrush representado*

Tanto [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) como [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) hacen referencia a un archivo de origen de imagen mediante un identificador de recursos uniforme (URI); el archivo de origen de imagen utiliza varios formatos de archivo posibles. Estos archivos de origen de imagen se especifican como URI. Para más información sobre la especificación de orígenes de imagen, así como sobre los formatos de imagen que puedes usar y su empaquetado en una aplicación, consulta [Image e ImageBrush](../controls-and-patterns/images-imagebrushes.md).

## <a name="brushes-and-text"></a>Pinceles y texto

También puedes usar pinceles para aplicar características de representación a elementos de texto. Por ejemplo, la propiedad [**Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) de [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) utiliza un objeto [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Puedes aplicar al texto cualquiera de los pinceles aquí descritos. Sin embargo, ten cuidado con los que apliques al texto, ya que este podría ser ilegible encima de cualquier fondo si usas pinceles con sangrado de fondo. Usa el objeto [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) para mejorar la legibilidad de los elementos de texto en la mayoría de los casos, a menos que la finalidad del elemento de texto sea principalmente decorativa.

Aunque utilices un color sólido, asegúrate de que el color de texto que elijas tenga suficiente contraste con respecto al color de fondo del contenedor de diseño del texto. El nivel de contraste entre el primer plano del texto y el fondo del contenedor de texto debe tenerse en cuenta para la accesibilidad.

## <a name="webviewbrush"></a>WebViewBrush

Un objeto [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) es un tipo de pincel especial que puede acceder al contenido que se ve normalmente en un control [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView). En lugar de representar el contenido en el área rectangular del control **WebView**, el objeto **WebViewBrush** pinta ese contenido en otro elemento que tiene una propiedad de tipo [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) para una superficie de representación. El objeto **WebViewBrush** no es apropiado para todos los escenarios de uso de pincel, pero es útil para las transiciones de un objeto **WebView**. Para obtener más información, consulta [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) es una clase base usada para crear pinceles personalizados que usan [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) para pintar los elementos de la interfaz de usuario XAML.

Esto permite la interoperación de "lista desplegable" entre las capas Windows.UI.Xaml y Windows.UI.Composition, tal y como se describe en la [**información general de la capa Visual**](../../composition/visual-layer.md). 

Para crear un pincel personalizado, crea una clase que se herede de XamlCompositionBrushBase e implemente los métodos necesarios.

Por ejemplo, esto se puede usar para aplicar [**efectos**](/uwp/composition/composition-effects) a propiedades UIElement de XAML con un [**CompositionEffectBrush**](/uwp/api/Windows.UI.Composition.CompositionEffectBrush), como un **GaussianBlurEffect** o un [**SceneLightingEffect**](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect), que controla las propiedades reflectantes de una propiedad UIElement de XAML cuando está iluminada por un [**XamlLight**](/uwp/api/windows.ui.xaml.media.xamllight).

Para obtener ejemplos de código, consulta [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pinceles como recursos XAML

Puedes declarar cualquier pincel como un recurso XAML con clave en un diccionario de recursos XAML. Esto facilita la replicación de los mismos valores de pincel cuando se aplican a varios elementos de una interfaz de usuario. A continuación, los valores de pincel se comparten y aplican a cualquier caso en el que se haga referencia al recurso de pincel como un uso [{StaticResource}](/uwp/xaml-platform/staticresource-markup-extension) en el código XAML. Esto incluye los casos en los que tienes una plantilla de control XAML que hace referencia al pincel compartido y dicha plantilla de control es un recurso XAML con clave.

## <a name="brushes-in-code"></a>Pinceles en código

Es mucho más frecuente especificar pinceles con XAML que usar código para definirlos. Esto es así porque los pinceles se suelen definir como recursos XAML y porque los valores de pincel suelen ser el resultado de herramientas de diseño, o bien, porque forman parte de una definición de interfaz de usuario XAML. Aun así, si alguna vez quieres definir un pincel con código, todos los tipos [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) están disponibles para la creación de una instancia de código.

Para crear un objeto [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) en código, usa el constructor que utiliza un parámetro [**Color**](/uwp/api/Windows.UI.Color). Pasa un valor que sea una propiedad estática de la clase [**Colors**](/uwp/api/windows.ui.colors), como este:

```cs
SolidColorBrush blueBrush = new SolidColorBrush(Windows.UI.Colors.Blue);
```

```vb
Dim blueBrush as SolidColorBrush = New SolidColorBrush(Windows.UI.Colors.Blue)
```

```cppwinrt
Windows::UI::Xaml::Media::SolidColorBrush blueBrush{ Windows::UI::Colors::Blue() };
```

```cpp
blueBrush = ref new SolidColorBrush(Windows::UI::Colors::Blue);
```

Para los objetos [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) y [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush), usa el constructor predeterminado y después llama a otras API antes de tratar de usar ese pincel para una propiedad de interfaz de usuario.

-   [**ImageSource**](/uwp/api/windows.ui.xaml.media.imagebrush.imagesourceproperty) requiere un [**BitmapImage**](/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) (no un URI) cuando se define un [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) con código. Si el origen es una secuencia, usa el método [**SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) para inicializar el valor. Si el origen es un identificador URI, que incluye contenido en la aplicación que usa los esquemas **ms-appx** o **ms-resource**, utiliza el constructor [**BitmapImage**](/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) que toma un URI. También puedes considerar la posibilidad de controlar el evento [**ImageOpened**](/uwp/api/windows.ui.xaml.media.imagebrush.imageopened) si hay problemas de temporización al recuperar o descodificar el origen de la imagen, ya que es posible que necesites que aparezca otro contenido hasta que el origen de la imagen esté disponible.
-   Para el objeto [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), quizá tengas que llamar a [**Redraw**](/uwp/api/windows.ui.xaml.controls.webviewbrush.redraw) si restableciste la propiedad [**SourceName**](/uwp/api/windows.ui.xaml.controls.webviewbrush.sourcename) recientemente o si el contenido del objeto [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) también cambia con el código.

Para ver ejemplos de código, consulta [**WebViewBrush**](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush), [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) y [**XamlCompositionBrushBase**](/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 