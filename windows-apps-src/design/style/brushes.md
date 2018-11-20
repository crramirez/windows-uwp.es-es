---
author: Jwmsft
ms.assetid: 02141F86-355E-4046-86EA-2A89D615B7DB
title: Usar pinceles
description: Los objetos Brush se usan para pintar los interiores o los contornos de formas, texto y partes de los controles, de forma que el objeto pintado esté visible en una interfaz de usuario.
ms.author: jimwalk
ms.date: 07/13/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e96604daa9f8736601f52c917b556369ec620e96
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7296846"
---
# <a name="using-brushes-to-paint-backgrounds-foregrounds-and-outlines"></a>Usar pinceles para pintar fondos, primeros planos y esquemas

Los objetos [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) se usan para pintar el interior o el contorno de formas, texto y partes de controles XAML, de forma que el objeto pintado sea visible en una interfaz de usuario. Veamos los pinceles disponibles y cómo usarlos.

> **API importantes**: [Clase Brush](/uwp/api/Windows.UI.Xaml.Media.Brush)

## <a name="introduction-to-brushes"></a>Introducción a los pinceles

Para pintar un objeto tal como [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) o las partes de un [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) que se muestran en el Canvas de la aplicación, usa un [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Por ejemplo, puedes establecer la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) del **Shape** o el [**Background**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx), y las propiedades [**Foreground**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx) de un **Control** en un valor **Brush**, y ese **Brush** determina cómo se pinta o se representa el elemento en la interfaz de usuario. 

Los distintos tipos de pinceles son: 
-   [**AcrylicBrush**](/uwp/api/windows.ui.xaml.media.acrylicbrush)
-   [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)
-   [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 
-   [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101)
-   [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703)
-   [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase)

## <a name="solid-color-brushes"></a>Pinceles de colores sólidos

Un [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) pinta un área con un único [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723), como rojo o azul. Este es el pincel más básico. En XAML hay tres maneras de definir un **SolidColorBrush** y el color sólido que especifica: nombres de color predefinidos, valores de color hexadecimales o la sintaxis de los elementos de las propiedades.

### <a name="predefined-color-names"></a>Nombres de color predefinidos

Puedes usar un nombre de color predefinido, como [**Yellow**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.yellow.aspx) o [**Magenta**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.magenta.aspx). Hay 256 colores con nombre disponibles. El analizador XAML convierte el nombre del color en una estructura [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) con los canales de color correctos. Los 256 colores con nombre se basan en los nombres de colores *X11* desde las hojas de estilo, Level 3 (CSS3) especificación, por lo que es posible que ya conozcas esta lista de colores con nombre si tienes experiencia con el desarrollo o diseño web.

Este ejemplo establece la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) de un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) en el color predefinido [**Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx).

```xml
<Rectangle Width="100" Height="100" Fill="Red" />
```

Esta imagen muestra el [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) aplicado al [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

![Un SolidColorBrush representado.](images/brushes-solidcolorbrush.jpg)

Si vas a definir un [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) mediante programación en lugar de usar XAML, cada color con nombre está disponible como una propiedad estática de la clase [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors). Por ejemplo, para declarar un valor [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) de un **SolidColorBrush** para representar el color con nombre "Orquídea", establece el valor de **Color** en el valor estático [**Colors.Orchid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.orchid.aspx).

### <a name="hexadecimal-color-values"></a>Valores de color hexadecimales

Puedes usar una cadena de formato hexadecimal para declarar valores precisos de color de 24 bits con un canal alfa de 8 bits para un [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962). Dos caracteres del intervalo 0 a F definen cada valor de componente y el orden de los valores de componente de la cadena hexadecimal es: canal alfa (opacidad), canal rojo, canal verde y canal azul (**ARGB**). Por ejemplo, el valor hexadecimal "\#FFFF0000" define el rojo completamente opaco (alfa="FF", rojo="FF", verde="00" y azul="00").

Este ejemplo de XAML establece la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx) de un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) en el valor hexadecimal "\#FFFF0000" y produce un resultado idéntico a usar el color con nombre [**Colors.Red**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.red.aspx).

```xml
<StackPanel>
  <Rectangle Width="100" Height="100" Fill="#FFFF0000" />
</StackPanel>
```

### <a name="span-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanspan-idpropertyelementsyntaxspanproperty-element-syntax"></a><span id="Property_element_syntax__"></span><span id="property_element_syntax__"></span><span id="PROPERTY_ELEMENT_SYNTAX__"></span>Sintaxis de elemento de propiedad

Puedes usar la sintaxis de los elementos de las propiedades para definir un [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962). Esta sintaxis es más detallada que los métodos anteriores, pero puedes especificar valores de propiedades adicionales en un elemento, como el valor de [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx). Para obtener más información sobre la sintaxis XAML, incluida la sintaxis de los elementos de propiedad, consulta el tema [introducción a XAML](https://msdn.microsoft.com/library/windows/apps/Mt185595) y [Guía de sintaxis XAML](https://msdn.microsoft.com/library/windows/apps/Mt185596).

En los ejemplos anteriores, la cadena "SolidColorBrush" ni siquiera se vio en la sintaxis. El pincel se crea de forma implícita y automática, como parte de un método abreviado deliberado del lenguaje XAML que ayuda a simplificar la definición de la interfaz de usuario en los casos más comunes. El ejemplo siguiente crea un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) y crea explícitamente el [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) como un valor de elemento para una propiedad [**Rectangle.Fill**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.shapes.shape.fill.aspx). El [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.solidcolorbrush.color.aspx) de **SolidColorBrush** se establece en [**Blue**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.colors.blue.aspx) y [**Opacity**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.opacity.aspx) se establece en 0,5.

```xml
<Rectangle Width="200" Height="150">
    <Rectangle.Fill>
        <SolidColorBrush Color="Blue" Opacity="0.5" />
    </Rectangle.Fill>
</Rectangle>
```

## <a name="span-idlineargradientbrushesspanspan-idlineargradientbrushesspanspan-idlineargradientbrushesspanlinear-gradient-brushes"></a><span id="Linear_gradient_brushes_"></span><span id="linear_gradient_brushes_"></span><span id="LINEAR_GRADIENT_BRUSHES_"></span>Pinceles de degradado lineal

Un [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) pinta un área con un degradado que se define a lo largo de una línea. Esta línea se denomina *eje de degradado*. Los colores del degradado y su ubicación en el eje de degradado se especifican con objetos [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078). De manera predeterminada, el eje de degradado va de la esquina superior izquierda a la esquina inferior derecha de la superficie pintada por el pincel, lo que produce un sombreado en diagonal.

[**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) es el componente básico de un pincel de degradado. Un delimitador de degradado especifica el [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) del pincel en [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) a lo largo del eje de degradado, cuando se aplica el pincel a la superficie que se está pintando.

La propiedad [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) del delimitador de degradado especifica el color de este. El color se puede establecer usando un nombre de color predefinido o mediante valores **ARGB** hexadecimales.

La propiedad [**Offset**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.offset.aspx) de un [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) especifica la posición de cada **GradientStop** a lo largo del eje de degradado. Un **Offset** es un **double** con valores entre 0 y 1. Un **Offset** con valor 0 sitúa el **GradientStop** al principio del eje de degradado; es decir, junto a su [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx). Un **Offset** con valor 1 sitúa el **GradientStop** en el [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx). Como mínimo, un [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) debe tener dos valores **GradientStop**, en los que cada **GradientStop** debe especificar un [**Color**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.gradientstop.color.aspx) distinto y tener un valor de **Offset** entre 0 y 1.

Este ejemplo crea un degradado lineal con cuatro colores y lo usa para pintar un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

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

El color de cada punto entre los delimitadores de degradado se interpolan linealmente como una combinación del color especificado por los dos delimitadores de degradado limítrofes. La ilustración resalta los delimitadores de degradado del ejemplo anterior. Los círculos marcan la posición de los puntos de degradado y la línea punteada muestra el eje de degradado.

![Delimitadores de degradado](images/linear-gradients-stops.png) Para cambiar la línea en la que se sitúan los delimitadores de degradado, establece las propiedades [**StartPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.startpoint.aspx) y [**EndPoint**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.lineargradientbrush.endpoint.aspx) en valores distintos a los valores de inicio predeterminados, `(0,0)` y `(1,1)`. Si cambias los valores de coordenadas de **StartPoint** y **EndPoint**, puedes crear degradados horizontales o verticales, invertir el sentido de degradado o condensar la extensión del degradado para aplicarlo en un área más reducida que la superficie pintada completa. Para condensar el degradado, debes establecer los valores de **StartPoint** o **EndPoint** en un valor entre 0 y 1. Por ejemplo, si deseas un degradado horizontal en el que toda la atenuación se produzca en la mitad izquierda del pincel y el lado derecho sea sólido con el color del último [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078), debes especificar un **StartPoint** de `(0,0)` y un **EndPoint** de `(0.5,0)`.

### <a name="span-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanspan-idusetoolstomakegradientsspanuse-tools-to-make-gradients"></a><span id="Use_tools_to_make_gradients"></span><span id="use_tools_to_make_gradients"></span><span id="USE_TOOLS_TO_MAKE_GRADIENTS"></span>Usar herramientas para hacer degradados

Ahora que ya sabes cómo funcionan los degradados lineales, puedes usar Visual Studio o Blend para crear estos degradados con facilidad. Para crear un degradado, selecciona el objeto al que deseas aplicar un degradado en la superficie de diseño o en la vista XAML. Expande **Pincel** y selecciona la pestaña **Degradado lineal** ficha (consulta la siguiente captura de pantalla).

![Crea el degradado lineal con Visual Studio.](images/tool-gradient-brush-1.png)

Ahora puedes cambiar los colores de los delimitadores de degradado y deslizar sus posiciones con la barra situada en la parte inferior. También puede agregar nuevos delimitadores de degradado haciendo clic en la barra y quitándolos. Para ello arrastra los delimitadores fuera de la barra (consulta la siguiente captura de pantalla).

![Barra situada en la parte inferior de la ventana de propiedades que controla los delimitadores de degradado.](images/tool-gradient-brush-2.png)

## <a name="span-idimagebrushesspanspan-idimagebrushesspanspan-idimagebrushesspanimage-brushes"></a><span id="Image_brushes"></span><span id="image_brushes"></span><span id="IMAGE_BRUSHES"></span>Pinceles de imagen

Un [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) pinta una superficie con una imagen que proviene de un origen de archivo de imagen. La propiedad [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) se establece con la ruta de acceso de la imagen para cargar. Normalmente, el origen de imagen proviene de un elemento **Content** que forma parte de los recursos de la aplicación.

De manera predeterminada, un [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) amplía su imagen para rellenar completamente el área pintada, lo que puede distorsionar la imagen si las relaciones de aspecto del área pintada y de la imagen son diferentes. Puedes cambiar este comportamiento cambiando la propiedad [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242975) de su valor predeterminado **Fill** a **None**, **Uniform** o **UniformToFill**.

El ejemplo siguiente crea un [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) y establece el [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/BR210107) en una imagen llamada licorice.jpg que debes incluir como recurso en la aplicación. A continuación, el **ImageBrush** pinta la superficie definida por una forma [**Ellipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse).

```xml
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="licorice.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

Este es el aspecto del [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) representado.

![Un ImageBrush representado.](images/brushes-imagebrush.jpg)

[**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) e [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) hacen referencia a un archivo de origen de imagen (en varios formatos de imagen posibles) mediante el Identificador uniforme de recursos (URI). Estos archivos de origen de imagen se especifican como URI. Para más información sobre la especificación de orígenes de imagen, sobre los formatos de imagen que puedes usar y su empaquetado en una aplicación, consulta [Image e ImageBrush](https://msdn.microsoft.com/library/windows/apps/Mt280382).

## <a name="brushes-and-text"></a>Pinceles y texto

También puedes usar pinceles para aplicar características de representación a elementos de texto. Por ejemplo, la propiedad [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) de [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) admite un [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Puedes aplicar cualquiera de los pinceles aquí descritos a texto. Aunque debes tener cuidado porque el texto puede quedar fácilmente ilegible si usas pinceles que puedan aplicar sangría en el fondo usado para representar el texto o que puedan difuminar los contornos de los caracteres de texto. Usa [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) para mejorar la legibilidad de los elementos de texto en la mayoría de los casos, a menos que la finalidad del elemento de texto sea principalmente decorativa.

Incluso cuando utilices un color sólido, asegúrate de que el color de texto que elijas tenga suficiente contraste con respecto al color de fondo del contenedor de diseño del texto. El nivel de contraste entre el primer plano del texto y el fondo del contenedor de texto debe tenerse en cuenta para la accesibilidad.

## <a name="webviewbrush"></a>WebViewBrush

Un [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) es un tipo especial de pincel que puede obtener acceso al contenido que se ve normalmente en un control [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702). En lugar de representar el contenido en el área rectangular del control **WebView**, **WebViewBrush** pinta ese contenido en otro elemento que tiene una propiedad de tipo [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) para una superficie de representación. **WebViewBrush** no es apropiado para todos los escenarios de uso del pincel, pero es útil para las transiciones de un control **WebView**. Para obtener más información, consulta [**WebViewBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush).

## <a name="xamlcompositionbrushbase"></a>XamlCompositionBrushBase

[**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) es una clase base usada para crear pinceles personalizados que usan [**CompositionBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionBrush) para pintar los elementos de la interfaz de usuario XAML.

Esto permite la interoperación de "desplegar" entre las capas Windows.UI.Xaml y Windows.UI.Composition según se describe en [**Información general sobre la capa visual**](/windows/uwp/composition/visual-layer). 

Para crear un pincel personalizado, crea una nueva clase que herede de XamlCompositionBrushBase e implemente los métodos necesarios.

Por ejemplo, puede usarse para aplicar [**efectos**](/windows/uwp/composition/composition-effects) a XAML UIElements con un elemento [**CompositionEffectBrush**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.CompositionEffectBrush), como pueden ser **GaussianBlurEffect** o [**SceneLightingEffect**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect), que controle las propiedades reflectantes de un XAML UIElement cuando se iluminan con un elemento [**XamlLight**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamllight).

Para ver ejemplos de código, consulta la página de referencia para [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).

## <a name="brushes-as-xaml-resources"></a>Pinceles como recursos de XAML

Puedes declarar cualquier pincel como un recurso con clave de XAML en un diccionario de recursos de XAML. Esto facilita la replicación de los mismos valores de pincel cuando se aplican a varios elementos de una interfaz de usuario. Así, los valores de pincel se comparten y aplican a cualquier caso en el que se haga referencia al recurso de pincel como un uso [{StaticResource}](https://msdn.microsoft.com/library/windows/apps/Mt185588) en el código XAML. Esto incluye los casos en los que se tiene una plantilla de control XAML que hace referencia al pincel compartido cuando la misma plantilla de control es un recurso de XAML con clave.

## <a name="brushes-in-code"></a>Pinceles en código

Es mucho más frecuente especificar pinceles con XAML que usar código para definirlos. Esto es así porque los pinceles, por lo general, se definen como recursos XAML, y porque los valores de pincel suelen ser el resultado de herramientas de diseño, o porque se incluyen como parte de una definición de interfaz de usuario de XAML. Aun así, si alguna vez quieres definir un pincel con código, todos los tipos [**Pincel**](/uwp/api/Windows.UI.Xaml.Media.Brush) están disponibles para la creación de una instancia de código.

Para crear un [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) con código, usa el constructor que toma un parámetro [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723). Pasa un valor que sea una propiedad estática de la clase [**Colors**](https://msdn.microsoft.com/library/windows/apps/windows.ui.colors), como este:

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

Para [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703) y [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101), usa el constructor predeterminado y después llama a otras API antes de tratar de usar ese pincel para una propiedad de interfaz de usuario.

-   [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagebrush.imagesourceproperty.aspx) requiere una clase [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) (no un URI) cuando se define una clase [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) mediante código. Si el origen es un flujo, usa el método [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) para inicializar el valor. Si el origen es un URI, que incluye contenido en la aplicación que usa los esquemas **ms-appx** o **ms-resource**, utiliza el constructor [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/br243238.aspx) que toma un URI. También podrías considerar la posibilidad de controlar el evento [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.imagebrush.imageopened.aspx) si hay problemas de temporización con la recuperación o la descodificación del origen de la imagen, ya que puede que necesites contenido alternativo que se mostrará hasta que el origen de la imagen esté disponible.
-   Para [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703), quizá tengas que llamar a [**Redraw**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.redraw.aspx) si restableciste la propiedad [**SourceName**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.sourcename.aspx) recientemente o si el contenido de [**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) cambiará también con el código.

Para ver ejemplos de código, consulta las páginas de referencia de [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/BR227703), [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) y [**XamlCompositionBrushBase**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase).
 

 




