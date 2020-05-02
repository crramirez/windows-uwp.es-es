---
Description: Los iconos adecuados armonizan con la tipografía y con el resto del lenguaje de diseño. No mezclan metáforas y comunican solo lo necesario de la manera más rápida y simple posible.
title: Iconos
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fb0101597a38553f56579913a9c40502fcdbf051
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "79434232"
---
# <a name="icons-for-uwp-apps"></a>Iconos de aplicaciones para UWP

![Imagen de encabezado de iconos](images/icons/header-icons.png)

Los iconos proporcionan una notación abreviada visual para una acción, un concepto o un producto. Al comprimir el significado en una imagen simbólica, los iconos pueden cruzar los obstáculos del idioma y ayudar a conservar un recurso muy valioso: el espacio en la pantalla. 

Los iconos pueden aparecer en aplicaciones y fuera de ellas: 

:::row:::
    :::column:::
        **Iconos de dentro de la aplicación**

        ![Iconos de dentro de la aplicación](images/icons/inside-icons.png) En la aplicación, usa los iconos para representar una acción, como copiar texto o navegar a la página de configuración.
    :::column-end:::
    :::column:::
**Iconos de fuera de la aplicación**

        ![Iconos de fuera de la aplicación](images/icons/outside-icons.jpg) Fuera de la aplicación, Windows utiliza un icono para representar la aplicación en el menú Inicio y en la barra de tareas. Si el usuario elige anclar la aplicación al menú Inicio, la ventana de inicio de la aplicación puede incluir el icono de la aplicación. El icono de la aplicación aparece en la barra de título y se puede crear una pantalla de presentación con el logotipo de la aplicación.
    :::column-end:::
:::row-end:::

En este artículo se describen iconos dentro de la aplicación. Para obtener información sobre los iconos fuera de la aplicación (iconos de la aplicación), consulta el [Artículo sobre iconos de aplicaciones y ventanas](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Cuándo usar iconos

Los iconos pueden ahorrar espacio, pero ¿cuándo deberías usarlos? 

:::row:::
    :::column:::
        ![Imagen estándar de iconos](images/icons/icons-standard.svg) ![adecuados](images/do.svg)<br>

Usa un icono para las acciones como cortar, copiar, pegar y guardar o para los elementos de navegación de un menú de navegación.
    :::column-end:::
    :::column:::
        ![Imagen de concepto de iconos](images/icons/icons-concept.svg) ![inadecuados](images/dont.svg)<br>

Usa un icono en caso de que ya exista uno para el concepto que quieres representar. (Para ver si existe un icono, comprueba la lista de iconos Segoe).
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Icono de carro de la compra](images/icons/icon-shopping-cart.svg) ![adecuado](images/do.svg)<br>

Usa un icono que sea fácil de entender para el usuario y que sea lo suficientemente sencillo para que, en tamaños pequeños, sea claro.
    :::column-end:::
    :::column:::
        ![Imagen de concepto de iconos](images/icons/icon-bad-example.png) ![inadecuados](images/dont.svg)<br>

No uses un icono si su significado no está claro, o si requiere una forma compleja para hacerlo claro.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Uso del tipo correcto de icono

Existen muchas formas de crear un icono. Puedes usar una fuente de símbolos como Segoe MDL2 Assets. Podrías crear tu propia imagen basada en vectores. Incluso puedes usar una imagen de mapa de bits, aunque no lo recomendamos. Este es un resumen de las distintas formas en que puedes agregar un icono a la aplicación. 

### <a name="use-a-predefined-icon"></a>Usa un icono predefinido.
:::row:::
    :::column:::
Microsoft proporciona más de 1000 iconos en el formulario de la fuente Segoe MDL2 Assets. Es posible que no sea intuitivo obtener un icono de una fuente, pero nuestra tecnología de visualización de fuentes significa que estos iconos tendrán un aspecto limpio y nítido en cualquier pantalla, en cualquier resolución y en cualquier tamaño. Para obtener instrucciones, consulta [Iconos Segoe MDL2](segoe-ui-symbol-font.md).
    :::column-end:::
    :::column:::
        ![Imagen de icono predefinido](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Usa una fuente.
:::row:::
    :::column:::
No tienes que usar la fuente Segoe MDL2 Assets; puedes usar cualquier fuente que el usuario haya instalado en su sistema, como Wingdings o Webdings.
    :::column-end:::
    :::column:::
        ![Imagen de Wingdings](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Usa un archivo Scalable Vector Graphics (SVG).
:::row:::
    :::column:::
Los recursos SVG son idóneos para iconos, ya que siempre se ven nítidos en cualquier tamaño o resolución. La mayoría de las aplicaciones de dibujo pueden exportar a SVG. Para obtener instrucciones, consulta [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource).
    :::column-end:::
    :::column:::
        ![Imagen de SVG](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Usa objetos de geometría.
:::row:::
    :::column:::
Como los archivos SVG, las geometrías son un recurso basado en vectores, por lo que siempre se ven nítidos. Sin embargo, la creación de una geometría resulta complicada porque tienes que especificar individualmente cada punto y curva. Realmente solo es una buena opción si tienes que modificar el icono mientras se ejecuta tu aplicación (para animarlo, por ejemplo). Para obtener instrucciones, consulta [Mover y dibujar comandos para geometrías](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Imagen de objetos geométricos](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>También puedes usar una imagen de mapa de bits, como PNG, GIF o JPEG, aunque no lo recomendamos.
:::row:::
    :::column:::
Las imágenes de mapa de bits se crean con un tamaño específico, por lo que tienen que ajustarse en función del tamaño que quieres que tenga el icono y de la resolución de la pantalla. Cuando la imagen se reduce puede aparecer borrosa y, cuando se amplía, puede aparecer en bloque y pixelada. Si tienes que usar una imagen de mapa de bits, te recomendamos usar PNG o GIF en lugar de JPEG. 
    :::column-end:::
    :::column:::
        ![Imagen de mapa de bits](images/icons/bitmap-image.png) ![inadecuado](images/dont.svg)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Cómo hacer que el icono haga algo

Una vez que tengas un icono, el siguiente paso es conseguir que haga algo asociándolo con el comando o una acción de navegación. La mejor manera de lograrlo es agregar el icono a un botón o una barra de comandos. 

![Imagen de la barra de comandos](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Creación de un botón de icono

Puedes colocar un icono en un botón estándar. Dado que puedes usar botones en una variedad más amplia de lugares, esto proporciona un poco más de flexibilidad a la hora de determinar el lugar donde aparece el icono de la acción. 

Hay varias maneras de agregar un icono a un botón:

:::row:::
    :::column span="2":::
        <b>Paso 1</b><br>
Establece la familia de fuentes del botón en `Segoe MDL2 Assets` y la propiedad de contenido en el valor unicode del glifo que quieres usar:
    :::column-end:::
    :::column:::
        ![Paso 1 de creación de un botón de icono](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Paso 2</b><br>
Puedes usar uno de los objetos de elemento de icono: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) o [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Esto te ofrece más tipos de iconos entre los que elegir y te permite agrupar iconos y otros tipos de contenido, como texto, si así lo deseas:
    :::column-end:::
    :::column:::
        ![Paso 2 de creación de un botón de icono](images/icons/icon-text-step-2.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Creación de una serie de iconos en una barra de comandos

:::row:::
    :::column span:::
Cuando tienes una serie de comandos que encajan, como cortar, copiar y pegar, o un conjunto de comandos de dibujo para un programa de edición de fotos, colócalos juntos en una [barra de comandos](../controls-and-patterns/app-bars.md). Una barra de comandos toma uno o varios botones de la barra de la aplicación o botones de alternancia de esta, cada uno de los cuales representa una acción. Cada botón tiene una propiedad [Icon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) que se usa para controlar qué icono muestra. Hay varias formas de especificar el icono. 
    :::column-end:::
    :::column:::
        ![Ejemplo de una barra de comandos con iconos](images/icons/create-icon-command-bar.svg)
    :::column-end:::
:::row-end:::

La forma más sencilla es usar la lista de iconos predefinidos que proporcionamos; solo tienes que especificar el nombre del icono, como "Atrás" o "Detener", y el sistema lo dibujará: 

``` xaml
<CommandBar>
    <AppBarToggleButton Icon="Shuffle" Label="Shuffle" Click="AppBarButton_Click" />
    <AppBarToggleButton Icon="RepeatAll" Label="Repeat" Click="AppBarButton_Click"/>
    <AppBarSeparator/>
    <AppBarButton Icon="Back" Label="Back" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Stop" Label="Stop" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Play" Label="Play" Click="AppBarButton_Click"/>
    <AppBarButton Icon="Forward" Label="Forward" Click="AppBarButton_Click"/>
</CommandBar>

```
Para obtener la lista completa de nombres de icono, consulta la [enumeración Symbol](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbol). 

Hay otras maneras de proporcionar iconos para un botón en una barra de comandos:

+ [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon): el icono se basa en un glifo de la familia de fuentes especificada.
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon): el icono se basa en un archivo de imagen de mapa de bits con el **URI** especificado.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon): el icono se basa en los datos de [Path](/uwp/api/windows.ui.xaml.shapes.path).

Para obtener más información sobre las barras de comandos, consulta el [artículo sobre la barra de comandos](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Artículos relacionados

* [Logotipos e iconos de aplicaciones](app-icons-and-logos.md)
