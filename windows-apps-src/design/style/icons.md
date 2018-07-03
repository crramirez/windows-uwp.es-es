---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Iconos
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 077967c37f76c8f1d0942f365344de65db13b041
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983578"
---
# <a name="icons-for-uwp-apps"></a>Iconos de aplicaciones para UWP

![imagen de encabezado de iconos](images/icons/header-icons.png)

Los iconos proporcionan una notación abreviada visual para una acción, un concepto o un producto. Al comprimir el significado en una imagen simbólico, los iconos pueden cruzar los obstáculos del lenguaje y ayudar a conservar un recurso muy valioso: el espacio en la pantalla. 

Los iconos pueden aparecer en aplicaciones y fuera de ellas: 

:::row::: :::column::: **Iconos dentro de la aplicación**

        ![icons inside the app](images/icons/inside-icons.png)
        Inside your app, you use icons to represent an action, such as copying text or navigating to the settings page.
    :::column-end:::
    :::column:::
        **Icons outside the app**

        ![icons outside the app](images/icons/outside-icons.jpg)
         Outside your app, Windows uses an icon to represent your app in the start menu and in the taskbar. If the user chooses to pin your app to the start menu, your app's start tile can feature your app's icon. Your app's icon appears in the title bar and you can choose to create a splash screen with your app's logo.
    :::column-end:::
:::row-end:::

En este artículo se describen iconos dentro de la aplicación. Para obtener información sobre los iconos fuera de la aplicación (iconos de la aplicación), consulta el [artículo de iconos de aplicaciones y ventanas](/windows/uwp/design/shell/tiles-and-notifications/app-assets).

## <a name="when-to-use-icons"></a>Cuándo usar iconos

Los iconos pueden ahorrar espacio, pero ¿cuándo deberías usarlos? 

:::row::: :::column::: ![do](images/do.svg) ![imagen estándar de iconos](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row::: :::column::: ![hacer](images/do.svg) ![icono del carro de la compra](images/icons/icon-shopping-cart.svg)<br>

        Use an icon if it's easy for the user to understand what the icon means and it's simple enough to be clear at small sizes.
    :::column-end:::
    :::column:::
        ![dont](images/dont.svg)
        ![icons concept image](images/icons/icon-bad-example.png)<br>

        Don't use an icon if its meaning isn't clear, or if making it clear requires a complex shape.
    :::column-end:::
:::row-end:::



## <a name="using-the-right-type-of-icon"></a>Usar el tipo correcto de icono

Existen muchas formas de crear un icono. Puedes usar una fuente de símbolos como Segoe MDL2 Assets. Podrías crear tu propia imagen basada en vectores. Incluso puedes usar una imagen de mapa de bits, aunque no lo recomendamos. Este es un resumen de las distintas formas en que puedes agregar un icono a la aplicación. 

### <a name="use-a-predefined-icon"></a>Usa un icono predefinido.
:::row::: :::column::: Microsoft proporciona más de 1.000 iconos en el formulario de la fuente Segoe MDL2 Assets. Es posible que no sea intuitivo obtener un icono de una fuente, pero nuestra tecnología de visualización de fuentes implica que estos iconos tendrán un aspecto limpio y nítido en cualquier pantalla, en cualquier resolución y en cualquier tamaño. :::column-end::: :::column::: ![imagen de icono predefinida](images/icons/predefined-icon.png) :::column-end::: :::row-end:::

### <a name="use-a-font"></a>Usa una fuente.
:::row::: :::column::: No tienes que usar la fuente Segoe MDL2 Assets;; puedes usar cualquier fuente que el usuario haya instalado en su sistema, como Wingdings o Webdings.
:::column-end::: :::column::: ![imagen de wingdings](images/icons/wingdings.png) :::column-end::: :::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Usa un archivo Scalable Vector Graphics (SVG).
:::row::: :::column::: Los recursos SVG son idóneos para iconos, ya que siempre se ven nítidos en cualquier tamaño o resolución. La mayoría de las aplicaciones de dibujo pueden exportar a SVG. :::column-end::: :::column::: ![imagen SVG](images/icons/icon-scale.gif) :::column-end::: :::row-end:::

### <a name="use-geometry-objects"></a>Usa objetos de geometría.
:::row::: :::column::: Como los archivos SVG, las geometrías son un recurso basado en vectores, por lo que siempre se ven nítidos. Sin embargo, la creación de una geometría resulta complicada porque tienes que especificar individualmente cada punto y curva. Realmente solo es una buena opción si tienes que modificar el icono mientras se está ejecutan tu aplicación (para animarlo, por ejemplo). Para obtener instrucciones, consulta [Mover y dibujar comandos para geometrías](../../xaml-platform/move-draw-commands-syntax.md). :::column-end::: :::column::: ![imagen de objetos de geometría](images/icons/geometry-objects.png) :::column-end::: :::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>También puedes usar una imagen de mapa de bits, como PNG, GIF o JPEG, aunque no lo recomendamos.
:::row::: :::column::: Se crean imágenes de mapa de bits con un tamaño específico, por lo que tienen que ajustarse en función del tamaño que quiere que tenga el icono y de la resolución de la pantalla. Cuando la imagen se reduce, puede aparecer borrosa; cuando se aumenta, puede aparecer en bloque y pixelada. Si tienes que usar una imagen de mapa de bits, te recomendamos usar PNG o GIF a través de un formato JPEG. :::column-end::: :::column::: ![no](images/dont.svg) ![Imagen de mapa de bits](images/icons/bitmap-image.png) :::column-end::: :::row-end:::

## <a name="make-the-icon-do-something"></a>Hacer que el icono haga algo

Una vez que tengas un icono, el siguiente paso es conseguir que hacer algo asociándolo con el comando o una acción de navegación. La mejor manera de lograrlo es agregar el icono a un botón o una barra de comandos. 

![Imagen de la barra de comandos](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Crear un botón de icono

Puedes colocar un icono en un botón estándar. Dado que puedes usar botones en una variedad más amplia de lugares, esto proporciona un poco más de flexibilidad para dónde aparece el icono de la acción. 

Hay algunas maneras de agregar un icono a un botón:

:::row::: :::column span="2"::: <b>Paso 1</b><br>
        Establece la familia de fuentes del botón en `Segoe MDL2 Assets` y su propiedad de contenido en el valor unicode del glifo que quieres usar: :::column-end::: :::column::: ![Crear un botón de icono paso 1](images/icons/create-icon-step-1.svg) :::column-end::: :::row-end:::

```xaml 
<Button FontFamily="Segoe MDL2 Assets" Content="&#xE102;" />
```

:::row::: :::column span="2"::: <b>Paso 2</b><br>
        Puedes usar uno de los objetos de elemento de icono: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon), [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon) o [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). Esto te ofrece más tipos de iconos entre los que elegir y te permite combinar iconos y otros tipos de contenido, como texto, si quieres: :::column-end::: :::column::: ![Crear un botón de icono paso 2](images/icons/icon-text-step-2.svg) :::column-end::: :::row-end:::

```xaml 
<Button>
    <StackPanel>
        <SymbolIcon Symbol="Play" />
        <TextBlock>Play the movie</TextBlock>
    </StackPanel>
</Button>
```

## <a name="create-a-series-of-icons-in-a-command-bar"></a>Crear una serie de iconos en una barra de comandos

:::row::: :::column span::: Cuando tienes una serie de comandos que encajan, como cortar, copiar y pegar, o un conjunto de comandos de dibujo para un programa de edición de fotos, colócalos juntos en una [barra de comandos](../controls-and-patterns/app-bars.md). Una barra de comandos toma uno o varios botones de la barra de aplicaciones o botones de alternancia de la barra de aplicaciones, cada uno de los cuales representa una acción. Cada botón tiene una propiedad [Icon](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) que usas para controlar qué icono muestra. Hay varias formas de especificar el icono. :::column-end::: :::column::: ![ejemplo de una barra de comandos con iconos](images/icons/create-icon-command-bar.svg) :::column-end::: :::row-end:::

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

* [Directrices sobre los activos de icono y de ventanas](../shell/tiles-and-notifications/app-assets.md)
