---
author: QuinnRadich
description: Aprende a usar colores de énfasis y temas en tus aplicaciones para UWP.
title: Color en las aplicaciones para UWP
ms.author: quradic
ms.date: 4/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
design-contact: karenmui
ms.localizationpriority: medium
ms.openlocfilehash: 19f4d9cde6ee2bc9615f044f18bc5e8828ca1985
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "3421303"
---
# <a name="color"></a>Color

![imagen principal](images/header-color.svg)

El color proporciona una forma intuitiva de comunicar información a los usuarios en tu aplicación: puede usarse para indicar interactividad, proporcionar comentarios a las acciones del usuario y dar una sensación de continuidad visual a tu interfaz. 

En aplicaciones para UWP, los colores vienen determinados principalmente por color de énfasis y tema. En este artículo, trataremos cómo puedes usar el color en la aplicación y cómo usar los recursos de tema y color de énfasis para que tu aplicación para UWP se pueda usar en cualquier contexto de tema. 

## <a name="color-principles"></a>Principios de color

:::row:::
    :::column:::
        **Usar color significativamente.**
Cuando se usa color con moderación para resaltar elementos importantes, puede ayudar a crear una interfaz de usuario que sea fluida e intuitiva.
    :::column-end:::
    :::column:::
        **Usar color para indicar interactividad.**
Es aconsejable elegir un color para indicar elementos de tu aplicación que son interactivos. Por ejemplo, muchas páginas web usa texto azul para indicar un hipervínculo.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **El color es personal.**
En Windows, los usuarios pueden elegir un color de énfasis y un tema claro u oscuro, que se reflejan a lo largo de su experiencia. Puedes elegir cómo incorporar el tema y el color de énfasis del usuario en la aplicación, personalizando su experiencia.
    :::column-end:::
    :::column:::
        **El color es cultural.**
Ten en cuenta cómo se interpretarán los colores que usas por parte de personas de diferentes culturas. Por ejemplo, en algunas culturas el color azul se asocia a virtud y protección, mientras que en otras representa duelo.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>Temas

Las aplicaciones para UWP pueden usar un tema de aplicación claro u oscuro. El tema afecta a los colores de fondo, al texto, a los iconos y a los [controles comunes ](../controls-and-patterns/index.md) de la aplicación.

### <a name="light-theme"></a>Tema claro

![tema claro](images/color/light-theme.svg)

### <a name="dark-theme"></a>Tema oscuro

![tema oscuro](images/color/dark-theme.svg)

De manera predeterminada, el tema de tu aplicación para UWP es la preferencia de tema del usuario de la configuración de Windows o el tema predeterminado del dispositivo (es decir, oscuro en XBox). Sin embargo, puedes establecer el tema de la aplicación para UWP. 

### <a name="changing-the-theme"></a>Modificación del tema

Para modificar temas, cambia la propiedad **RequestedTheme** en tu archivo `App.xaml`.

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

Quitar la propiedad **RequestedTheme** significa que la aplicación usará la configuración del sistema del usuario.

Los usuarios también pueden seleccionar el tema de contraste alto, que usa una pequeña paleta de colores que contrastan entre sí, lo que facilita la visualización de la interfaz. En ese caso, el sistema invalidará tu RequestedTheme.

### <a name="testing-themes"></a>Temas de pruebas

Si no solicitas un tema para la aplicación, asegúrate de probar tu aplicación tanto en temas claros como oscuros para garantizar que tu aplicación será legible en todas las condiciones.

**Nota**: En Visual Studio, el valor predeterminado de RequestedTheme es claro, por lo que tendrás que cambiar el RequestedTheme para probar ambos.

## <a name="theme-brushes"></a>Pinceles de temas

Los controles comunes usan automáticamente [pinceles de temas](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes) para ajustar el contraste para temas claros y oscuros.

Por ejemplo, esta es una ilustración de cómo [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) usa pinceles de temas:

![ejemplo de control de pinceles de temas](images/color/theme-brushes.svg)

Los pinceles de temas se usan para los siguientes fines:

- **Base** es para el texto.
- **Alt** es la inversa de Base.
- **Cromo** es para los elementos de nivel superior, como paneles de navegación o barras de comandos.
- **Lista** es para controles de lista.

**Baja**/**Media**/**Alta** hacen referencia a la intensidad del color.

### <a name="using-theme-brushes"></a>Usar pinceles de temas

:::row:::
    :::column:::
        Al crear plantillas para controles personalizados, usa pinceles de temas en lugar de valores de color codificados. De esta forma, tu aplicación se puede adaptar con facilidad a cualquier tema.

        For example, these [item templates for ListView](../controls-and-patterns/item-templates-listview.md) demonstrate how to use theme brushes in a custom template.
    :::column-end:::
    :::column:::
         ![double line list item with icon example](images/color/list-view.svg)
    :::column-end:::
:::row-end:::

```xaml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}">
    <ListView.ItemTemplate>
        <DataTemplate x:Name="DoubleLineDataTemplate" x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Height="64" AutomationProperties.Name="{x:Bind CompositionName}">
                <Ellipse Height="48" Width="48" VerticalAlignment="Center">
                    <Ellipse.Fill>
                        <ImageBrush ImageSource="Placeholder.png"/>
                    </Ellipse.Fill>
                </Ellipse>
                <StackPanel Orientation="Vertical" VerticalAlignment="Center" Margin="12,0,0,0">
                    <TextBlock Text="{x:Bind CompositionName}"  Style="{ThemeResource BaseTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseHighBrush}" />
                    <TextBlock Text="{x:Bind ArtistName}" Style="{ThemeResource BodyTextBlockStyle}" Foreground="{ThemeResource SystemControlPageTextBaseMediumBrush}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Para obtener más información acerca de cómo usar pinceles de temas en tu aplicación, consulta [Recursos de temas](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Color de énfasis

Los controles comunes usan un color de énfasis para transmitir información de estado. De manera predeterminada, el color de énfasis es el `SystemAccentColor` que los usuarios seleccionan en su Configuración. Sin embargo, también puedes personalizar el color de énfasis de tu aplicación para reflejar tu marca.

![controles de Windows](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![encabezado de énfasis seleccionado por el usuario](images/color/user-accent.svg) ![color de énfasis seleccionado por el usuario](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![encabezado de énfasis personalizado](images/color/custom-accent.svg) ![color de énfasis de marca personalizada](images/color/brand-color.svg)
    :::column-end:::
:::row-end:::

### <a name="overriding-the-accent-color"></a>Reemplazar el color de énfasis

Para cambiar el color de énfasis de tu aplicación, coloca el siguiente código en `app.xaml`.

```xaml
<Application.Resources>
    <ResourceDictionary>
        <Color x:Key="SystemAccentColor">#107C10</Color>
    </ResourceDictionary>
</Application.Resources>
```

### <a name="choosing-an-accent-color"></a>Elegir un color de énfasis

Si seleccionas un color de énfasis personalizado para tu aplicación, asegúrate de que el texto y los fondos que usan el color de énfasis tengan suficiente contraste para mejorar la legibilidad. Para probar el contraste, puedes usar la herramienta de selector de colores en la Configuración de Windows o puedes usar estas [herramientas de contraste en línea](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

![Selector de colores de énfasis personalizado de Configuración de Windows](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>Paleta de color de énfasis

Un algoritmo de color de énfasis en el shell de Windows genera tonos claros y oscuros del color de énfasis.

![paleta de color de énfasis](images/color/accent-color-palette.svg)

Pueden tener acceso a estos tonos como [recursos de temas](../controls-and-patterns/xaml-theme-resources.md):

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
También puedes obtener acceso a la paleta de colores de énfasis mediante programación con el método [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) y la enumeración [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType).

Puedes usar la paleta de colores de énfasis para temas de color en la aplicación. A continuación se incluye un ejemplo de cómo puedes usar la paleta de colores de énfasis en un botón.

![Paleta de colores de énfasis aplicada a un botón](images/color/color-theme-button.svg)

```xaml
<Page.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ButtonBackground" Color="{ThemeResource SystemAccentColor}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPointerOver" Color="{ThemeResource SystemAccentColorLight1}"/>
                <SolidColorBrush x:Key="ButtonBackgroundPressed" Color="{ThemeResource SystemAccentColorDark1}"/>
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Page.Resources>

<Button Content="Button"></Button>
```

Al usar el texto con color en un fondo con color, asegúrate de que hay suficiente contraste entre el texto y el fondo. De manera predeterminada, el hipervínculo o el hipertexto usará el color de énfasis. Si aplicas variaciones del color de énfasis al fondo, debes usar una variación del color de énfasis original para optimizar el contraste del texto con color en un fondo con color.

El gráfico siguiente muestra un ejemplo de los distintos tonos claros y oscuros del color de énfasis, y de cómo se puede aplicar el tipo con color a una superficie con color.

![color sobre color](images/color/color-on-color.svg)

Para obtener más información sobre los controles de estilos, consulta [Estilos XAML](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>API de color

Hay varias API que se pueden usar para agregar color a tu aplicación. En primer lugar, la clase [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors), que implementa una lista grande de colores predefinidos. A estos se puede acceder automáticamente con las propiedades XAML. En el siguiente ejemplo, creamos un botón y establecemos las propiedades de color de fondo y de primer plano para los miembros de la clase **Colors**.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

Puedes crear tus propios colores a partir de valores hexadecimales o RGB con la estructura [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) en XAML.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

También puedes crear el mismo color en el código usando el método **FromArgb**.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

Las letras "Argb" significa Alfa (opacidad), Rojo, Verde y Azul, que son los cuatro componentes de un color. Cada argumente pueden oscilar entre 0 y 255 minutos. Puedes elegir omitir el primer valor, que te dará una opacidad predeterminada de 255 o 100% opaco.

> [!Note]
> Si estás usando C++, debes crear colores usando la clase [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper).

El uso más habitual para un **Color** es como un argumento para un [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush), que se puede usar para pintar elementos de interfaz de usuario de un solo color sólido. Por lo general, estos pinceles se definen en un [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary), de modo que puedan volver a utilizarse para varios elementos.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Para obtener más información acerca de cómo usar pinceles, consulta [Pinceles XAML](brushes.md).

## <a name="usability"></a>Facilidad de uso

:::row:::
    :::column:::
        ![ilustración de contraste](images/color/illo-contrast.svg)
    :::column-end:::
    ::: column span = "2"::: **contraste**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![ilustración de contraste](images/color/illo-lighting.svg)
    :::column-end:::
    ::: column span = "2"::: **iluminación**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![ilustración de contraste](images/color/illo-colorblindness.svg)
    :::column-end:::
    ::: column span = "2"::: **daltonismo**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Artículos relacionados

- [Estilos XAML](../controls-and-patterns/xaml-styles.md)
- [Recursos de temas XAML](../controls-and-patterns/xaml-theme-resources.md)