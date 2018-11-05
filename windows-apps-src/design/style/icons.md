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
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 31bc1d33bccbf2d1948a119ea42bfef2599f3336
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032202"
---
# <a name="icons-for-uwp-apps"></a>Iconos de aplicaciones para UWP

![imagen de encabezado de iconos](images/icons/header-icons.png)

Los iconos proporcionan una notación abreviada visual para una acción, un concepto o un producto. Al comprimir el significado en una imagen simbólico, los iconos pueden cruzar los obstáculos del lenguaje y ayudar a conservar un recurso muy valioso: el espacio en la pantalla. 

Los iconos pueden aparecer en aplicaciones y fuera de ellas: 

:::row:::
    :::column:::
        **Icons inside the app**

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

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icons standard image](images/icons/icons-standard.svg)<br>

        Use an icon for actions, like cut, copy, paste, and save, or for navigation items in a navigation menu.
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![icons concept image](images/icons/icons-concept.svg)<br>

        Use an icon if one already exists for the concept you want to represent. (To see whether an icon exists, check the Segoe icon list.)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![do](images/do.svg)
        ![icon shopping cart](images/icons/icon-shopping-cart.svg)<br>

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
:::row:::
    :::column:::
        Microsoft provides over 1000 icons in the form of the Segoe MDL2 Assets font. It might not be intuitive to get an icon from a font, but our font display technology means these icons will look crisp and sharp on any display, at any resolution, and at any size. 
    :::column-end:::
    :::column:::
        ![pre-defined icon image](images/icons/predefined-icon.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-font"></a>Usa una fuente.
:::row:::
    :::column:::
        You don't have to use the Segoe MDL2 Assets font--you can use any font the user has installed on their system, such as Wingdings or Webdings.
    :::column-end:::
    :::column:::
        ![wingdings image](images/icons/wingdings.png)
    :::column-end:::
:::row-end:::

### <a name="use-a-scalable-vector-graphics-svg-file"></a>Usa un archivo Scalable Vector Graphics (SVG).
:::row:::
    :::column:::
        SVG resources are ideal for icons, because they always look sharp at any size or resolution. Most drawing applications can export to SVG. 
    :::column-end:::
    :::column:::
        ![SVG image](images/icons/icon-scale.gif)
    :::column-end:::
:::row-end:::

### <a name="use-geometry-objects"></a>Usa objetos de geometría.
:::row:::
    :::column:::
        Like SVG files, geometries are a vector-based resource, so they always look sharp. However, creating a geometry is complicated because you have to individually specify each point and curve. It's really only a good choice if you need to modify the icon while your app is running (to animate it, for example). For instructions, see [Move and draw commands for geometries](../../xaml-platform/move-draw-commands-syntax.md). 
    :::column-end:::
    :::column:::
        ![Geometry objects image](images/icons/geometry-objects.png)
    :::column-end:::
:::row-end:::

### <a name="you-can-also-use-a-bitmap-image-such-as-png-gif-or-jpeg-although-we-dont-recommend-it"></a>También puedes usar una imagen de mapa de bits, como PNG, GIF o JPEG, aunque no lo recomendamos.
:::row:::
    :::column:::
        Bitmap images are created at a specific size, so they have to be scaled up or down depending on how large you want the icon to be and the resolution of the screen. When the image is scaled down (shrunk), it can appear blurry; when it's scaled up, it can appear blocky and pixelated. If you have to use a bitmap image we recommend using a PNG or GIF over a JPEG. 
    :::column-end:::
    :::column:::
        ![don't](images/dont.svg)
        ![Bitmap image](images/icons/bitmap-image.png)
    :::column-end:::
:::row-end:::

## <a name="make-the-icon-do-something"></a>Hacer que el icono haga algo

Una vez que tengas un icono, el siguiente paso es conseguir que hacer algo asociándolo con el comando o una acción de navegación. La mejor manera de hacerlo es agregar el icono a un botón o una barra de comandos. 

![Imagen de la barra de comandos](images/icons/app-bar-desktop.svg)

## <a name="create-an-icon-button"></a>Crear un botón de icono

Puedes colocar un icono en un botón estándar. Dado que puedes usar botones en una variedad más amplia de lugares, esto proporciona un poco más de flexibilidad para dónde aparece el icono de la acción. 

Hay algunas maneras de agregar un icono a un botón:

:::row:::
    :::column span="2":::
        <b>Step 1</b><br>
        Set the button's font family to `Segoe MDL2 Assets` and its content property to the unicode value of the glyph you want to use:
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
        You can use one of the icon element objects: [BitmapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.bitmapicon),
        [FontIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.fonticon), 
        [PathIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon), or
        [SymbolIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.symbolicon). This gives you more types of icons to choose from, and enables you to combine icons and other types of content, such as text, if you want:
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
        When you have a series of commands that go together, such as cut/copy/paste or a set of drawing commands for a photo-editing program, put them together in a [command bar](../controls-and-patterns/app-bars.md). A command bar takes one or more app bar buttons or app bar toggle buttons, each of which represents an action. Each button has an [Icon](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.appbarbutton#Windows_UI_Xaml_Controls_AppBarButton_Icon) property you use to control which icon it displays. There are a variety of ways to specify the icon. 
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

* [Directrices sobre los activos de icono y de ventanas](../shell/tiles-and-notifications/app-assets.md)
