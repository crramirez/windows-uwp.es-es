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
ms.openlocfilehash: 5e464251200812e79474d05d9d0a680b49167871
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64564538"
---
# <a name="icons-for-uwp-apps"></a>Iconos de aplicaciones para UWP

![imagen de encabezado de iconos](images/icons/header-icons.png)

Los iconos proporcionan una notación abreviada visual para una acción, un concepto o un producto. Al comprimir el significado en una imagen simbólico, los iconos pueden cruzar los obstáculos del lenguaje y ayudar a conservar un recurso muy valioso: el espacio en la pantalla. 

Los iconos pueden aparecer en aplicaciones y fuera de ellas: 

:::row:::
    :::column:::
        **Icons inside the app**

        ![icons inside the app](images/icons/inside-icons.png)
Dentro de la aplicación, utilice los iconos para representar una acción, como copiar texto o navegar a la página de configuración.
    :::column-end:::
    :::column:::
**Iconos de fuera de la aplicación**

        ![icons outside the app](images/icons/outside-icons.jpg)
Fuera de la aplicación, Windows utiliza un icono para representar la aplicación en el menú Inicio y en la barra de tareas. Si el usuario elige anclar la aplicación al menú Inicio, el icono de inicio de la aplicación puede abarcar el icono de la aplicación. Icono de la aplicación aparece en la barra de título y se puede crear una pantalla de presentación con el logotipo de la aplicación.
    :::column-end:::
:::row-end:::

En este artículo se describen iconos dentro de la aplicación. Para obtener información sobre los iconos fuera de la aplicación (iconos de la aplicación), consulta el [artículo de iconos de aplicaciones y ventanas](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Cuándo usar iconos

Los iconos pueden ahorrar espacio, pero ¿cuándo deberías usarlos? 

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

Usar un icono para las acciones, como cortar, copiar, pegar y guardar o para los elementos de navegación de un menú de navegación.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

Usar un icono si ya existe uno para el concepto de que desea representar. (Para ver si existe un icono, compruebe la lista de iconos Segoe).
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

Usar un icono si es fácil para el usuario a comprender qué significa el icono y es lo suficientemente sencilla para ser claro en tamaños pequeños.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

No use un icono si su significado no está claro, o si lo que desactive requiere una forma compleja.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Usar el tipo correcto de icono

Existen muchas formas de crear un icono. Puedes usar una fuente de símbolos como Segoe MDL2 Assets. Podría crear su propia imagen basada en vectores. Incluso puedes usar una imagen de mapa de bits, aunque no lo recomendamos. Este es un resumen de las distintas formas en que puedes agregar un icono a la aplicación. 

### <a name="use-a-predefined-icon"></a>Usa un icono predefinido.
:::row:::
    :::column:::
Microsoft ofrece más de 1.000 iconos en el formulario de la fuente Segoe MDL2 activos. Es posible que no sea intuitivo obtener un icono de una fuente, pero nuestra tecnología de visualización de fuentes implica que estos iconos tendrán un aspecto limpio y nítido en cualquier pantalla, en cualquier resolución y en cualquier tamaño. Para obtener instrucciones, consulte [iconos Segoe MDL2](segoe-ui-symbol-font.md).
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Usa una fuente.
:::row:::
    :::column:::
No tiene que usar la fuente Segoe MDL2 activos, puede usar cualquier fuente que el usuario ha instalado en su sistema, como Wingdings o Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Usa un archivo Scalable Vector Graphics (SVG).
:::row:::
    :::column:::
Recursos SVG son ideales para los iconos, ya que siempre RECURRO sharp de cualquier tamaño o resolución. La mayoría de las aplicaciones de dibujo pueden exportar a SVG. Para obtener instrucciones, consulte [SVGImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource).
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Usa objetos de geometría.
:::row:::
    :::column:::
Como los archivos SVG, geometrías son un recurso basado en vectores, por lo que siempre RECURRO sharp. Sin embargo, la creación de una geometría resulta complicada porque tienes que especificar individualmente cada punto y curva. Realmente solo es una buena opción si tienes que modificar el icono mientras se está ejecutan tu aplicación (para animarlo, por ejemplo). Para obtener instrucciones, consulta [Mover y dibujar comandos para geometrías](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>También puedes usar una imagen de mapa de bits, como PNG, GIF o JPEG, aunque no lo recomendamos.
:::row:::
    :::column:::
Imágenes de mapa de bits se crean con un tamaño específico, para que tengan que se puede escalar o reducir verticalmente según lo grande desea que el icono sea y la resolución de la pantalla. Cuando la imagen se reduce, puede aparecer borrosa; cuando se aumenta, puede aparecer en bloque y pixelada. Si tienes que usar una imagen de mapa de bits, te recomendamos usar PNG o GIF a través de un formato JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Hacer que el icono haga algo

Una vez que tenga un icono, el siguiente paso es lo que haga algo al asociarlo con el comando o una acción de navegación. La mejor manera de hacerlo es agregar el icono a un botón o una barra de comandos. 

![Imagen de la barra de comandos](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Crear un botón de icono

Puedes colocar un icono en un botón estándar. Dado que puedes usar botones en una variedad más amplia de lugares, esto proporciona un poco más de flexibilidad para dónde aparece el icono de la acción. 

Hay algunas maneras de agregar un icono a un botón:

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
Establece la familia de fuentes del botón en `Segoe MDL2 Assets` y su propiedad de contenido en el valor unicode del glifo que desea usar:
    :::column-end:::
    :::column:::
        ![Create an icon button step 1](images/icons/create-icon-step-1.svg)
    :::column-end:::
:::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row:::
    :::column span="2":::
        <b>Step 2</b><br>
Puede usar uno de los objetos de elemento de icono: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), o [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Esto le ofrece más tipos de iconos que puede elegir y le permite combinar iconos y otros tipos de contenido, como texto, si desea:
    :::column-end:::
    :::column:::
        ![Create an icon button step 2](images/icons/icon-text-step-2.svg)
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

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Crear una serie de iconos en una barra de comandos

:::row:::
    :::column span:::
Cuando haya una serie de comandos que van juntas, como cortar/copiar/pegar o un conjunto de comandos para un programa de edición de fotografías de dibujo póngalos todos juntos en un [barra de comandos](../controls-and-patterns/app-bars.md). Una barra de comandos toma uno o varios botones de la barra de aplicaciones o botones de alternancia de la barra de aplicaciones, cada uno de los cuales representa una acción. Cada botón tiene una propiedad [Icon](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) que usas para controlar qué icono muestra. Hay varias formas de especificar el icono. 
    :::column-end:::
    :::column:::
        ![Example of a command bar with icons](images/icons/create-icon-command-bar.svg)
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
+ [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon): el icono se basa en un archivo de imagen de mapa de bits con el **Uri** especificado.
+ [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon): el icono se basa en los datos de [Path](/uwp/api/windows.ui.xaml.shapes.path).

Para obtener información sobre las comandos, consulta el [artículo de la barra comandos](../controls-and-patterns/app-bars.md). 



## <a name="related-articles"></a>Artículos relacionados

* [Directrices para los recursos de icono y el icono](../shell/tiles-and-notifications/app-assets.md)
