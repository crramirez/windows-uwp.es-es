---
description: Aprende a usar colores de énfasis y temas en tus aplicaciones para UWP.
title: Color en las aplicaciones para UWP
ms.date: 04/07/2019
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e5cd8ecafd3557719e70c50890da4c3eade18f52
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714137"
---
# <a name="color"></a>Color

![imagen principal](images/header-color.svg)

El color proporciona una forma intuitiva de comunicar información a los usuarios en tu aplicación. Puede usarse para indicar interactividad, proporcionar comentarios a las acciones del usuario y dar una sensación de continuidad visual a tu interfaz.

En aplicaciones para UWP, los colores vienen determinados principalmente por el color de énfasis y el tema. En este artículo, trataremos cómo puedes usar el color en la aplicación y cómo se emplean los recursos de tema y color de énfasis para que una aplicación para UWP se pueda usar en cualquier contexto de tema.

## <a name="color-principles"></a>Principios de color

:::row:::
    :::column:::
        **Use color meaningfully.**
        When color is used sparingly to highlight important elements, it can help create a user interface that is fluid and intuitive.
    :::column-end:::
    :::column:::
        **Use color to indicate interactivity.**
        It's a good idea to choose one color to indicate elements of your application that are interactive. For example, many web pages use blue text to denote a hyperlink.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Color is personal.**
        In Windows, users can choose an accent color and a light or dark theme, which are reflected throughout their experience. You can choose how to incorporate the user's accent color and theme into your application, personalizing their experience.
    :::column-end:::
    :::column:::
        **Color is cultural.**
        Consider how the colors you use will be interpreted by people from different cultures. For example, in some cultures the color blue is associated with virtue and protection, while in others it represents mourning.
    :::column-end:::
:::row-end:::

## <a name="themes"></a>Temas

Las aplicaciones para UWP pueden usar un tema de aplicación claro u oscuro. El tema afecta a los colores de fondo, al texto, a los iconos y a los [controles comunes](../controls-and-patterns/index.md) de la aplicación.

### <a name="light-theme"></a>Tema claro

![Tema claro](images/color/light-theme.svg)

### <a name="dark-theme"></a>Tema oscuro

![Tema oscuro](images/color/dark-theme.svg)

De manera predeterminada, el tema de tu aplicación para UWP es la preferencia de tema del usuario de la configuración de Windows o el tema predeterminado del dispositivo (es decir, oscuro en XBox). Aun así, también puedes establecer el tema de tu aplicación para UWP.

### <a name="changing-the-theme"></a>Modificación del tema

Para modificar temas, cambia la propiedad **RequestedTheme** en el archivo `App.xaml`.

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">
</Application>
```

Si quitas la propiedad **RequestedTheme**, la aplicación usará la configuración del sistema del usuario.

Los usuarios también pueden seleccionar el tema de contraste alto, que usa una pequeña paleta de colores que contrastan entre sí, lo que facilita la visualización de la interfaz. En ese caso, el sistema invalidará tu RequestedTheme.

### <a name="testing-themes"></a>Temas de pruebas

Si no solicitas un tema para la aplicación, asegúrate de probarla tanto en temas claros como oscuros para garantizar que sea legible en todas las circunstancias.

**Nota**: En Visual Studio, el valor predeterminado de RequestedTheme es claro, por lo que tendrás que cambiar el RequestedTheme para probar ambos.

## <a name="theme-brushes"></a>Pinceles de temas

Los controles comunes usan automáticamente [pinceles de temas](../controls-and-patterns/xaml-theme-resources.md#the-xaml-color-ramp-and-theme-dependent-brushes) para ajustar el contraste para temas claros y oscuros.

Por ejemplo, esta es una ilustración de cómo [AutoSuggestBox](../controls-and-patterns/auto-suggest-box.md) usa pinceles de temas:

![Ejemplo de control de pinceles de temas](images/color/theme-brushes.svg)

Los pinceles de temas se usan para los fines siguientes:

- **Base** es para el texto.
- **Alt** es la inversa de Base.
- **Cromo** es para los elementos de nivel superior, como paneles de navegación o barras de comandos.
- **Lista** es para controles de lista.

**Baja**/**Media**/**Alta** hacen referencia a la intensidad del color.

### <a name="using-theme-brushes"></a>Usar pinceles de temas

:::row:::
    :::column:::
        When creating templates for custom controls, use theme brushes rather than hard code color values. This way, your app can easily adapt to any theme.

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

Para obtener más información sobre cómo usar pinceles de temas en tu aplicación, consulta [Recursos de temas](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Color de énfasis

Los controles comunes usan un color de énfasis para transmitir información de estado. De manera predeterminada, el color de énfasis es el `SystemAccentColor` que los usuarios seleccionan en la configuración. A pesar de esto, también puedes personalizar el color de énfasis de tu aplicación para reflejar tu marca.

![Controles de Windows](images/color/windows-controls.svg)

:::row:::
    :::column:::
        ![user-selected accent header](images/color/user-accent.svg)
        ![user-selected accent color](images/color/user-selected-accent.svg)
    :::column-end:::
    :::column:::
        ![custom accent header](images/color/custom-accent.svg)
        ![custom brand accent color](images/color/brand-color.svg)
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

Si seleccionas un color de énfasis personalizado para tu aplicación, asegúrate de que el texto y los fondos que usan el color de énfasis tengan suficiente contraste para mejorar la legibilidad. Para probar el contraste, puedes usar la herramienta Selector de colores en la configuración de Windows, o bien emplear estas [herramientas de contraste en línea](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources).

![Selector de colores de énfasis personalizado de la configuración de Windows](images/color/color-picker.svg)

## <a name="accent-color-palette"></a>Paleta de color de énfasis

Un algoritmo de color de énfasis en el shell de Windows genera tonos claros y oscuros del color de énfasis.

![Paleta de color de énfasis](images/color/accent-color-palette.svg)

Puedes acceder a estos tonos como [recursos de temas](../controls-and-patterns/xaml-theme-resources.md):

- `SystemAccentColorLight3`
- `SystemAccentColorLight2`
- `SystemAccentColorLight1`
- `SystemAccentColorDark1`
- `SystemAccentColorDark2`
- `SystemAccentColorDark3`

<!-- check this is true -->
También puedes acceder a la paleta de colores de énfasis mediante programación con el método [**UISettings.GetColorValue**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) y la enumeración [**UIColorType**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType).

Es posible usar la paleta de colores de énfasis para los temas de color de la aplicación. A continuación se incluye un ejemplo de cómo se puede usar la paleta de colores de énfasis en un botón.

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

Cuando uses el texto con color en un fondo con color, asegúrate de que hay suficiente contraste entre el texto y el fondo. De manera predeterminada, el hipervínculo o el hipertexto usarán el color de énfasis. Si aplicas variaciones del color de énfasis al fondo, debes usar una variación del color de énfasis original para optimizar el contraste del texto con color en un fondo con color.

En el gráfico siguiente se muestra un ejemplo de los distintos tonos claros y oscuros del color de énfasis y de cómo se puede aplicar un tipo con color a una superficie con color.

![Color sobre color](images/color/color-on-color.png)

Para obtener más información sobre los controles de estilos, consulta [Estilos XAML](../controls-and-patterns/xaml-styles.md).

## <a name="color-api"></a>API de color

Hay varias API que se pueden usar para agregar color a la aplicación. En primer lugar, la clase [**Colors**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colors), que implementa una gran lista de colores predefinidos. Se puede acceder automáticamente a estos con las propiedades XAML. En el siguiente ejemplo, creamos un botón y establecemos las propiedades de color de fondo y de primer plano en miembros de la clase **Colors**.

```xaml
<Button Background="MediumSlateBlue" Foreground="White">Button text</Button>
```

Puedes crear tus propios colores a partir de valores hexadecimales o RGB con la estructura [**Color**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.color) en XAML.

```xaml
<Color x:Key="LightBlue">#FF36C0FF</Color>
```

También puedes crear el mismo color en código mediante el método **FromArgb**.

```csharp
Color LightBlue = Color.FromArgb(255,54,192,255);
```

Las letras "Argb" significan "alfa (opacidad), rojo, verde y azul", que son los cuatro componentes de un color. Cada argumente puede oscilar entre 0 y 255. Puedes optar por omitir el primer valor, lo que dará una opacidad predeterminada de 255, o 100 % opaco.

> [!Note]
> Si usas C++, debes crear colores mediante la clase [**ColorHelper**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.colorhelper).

El uso más habitual de **Color** es como argumento de un [**SolidColorBrush**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.solidcolorbrush), que se puede usar para pintar elementos de la interfaz de usuario de un solo color sólido. Por lo general, estos pinceles se definen en una clase [**ResourceDictionary**](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.ResourceDictionary), de modo que puedan volver a usarse para varios elementos.

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="ButtonBackgroundBrush" Color="#FFFF4F67"/>
    <SolidColorBrush x:Key="ButtonForegroundBrush" Color="White"/>
</ResourceDictionary>
```

Para obtener más información sobre cómo usar pinceles, consulta [Pinceles XAML](brushes.md).

## <a name="scoping-system-colors"></a>Definición del ámbito de los colores del sistema

Además de definir tus propios colores en la aplicación, puedes definir el ámbito de los colores del sistema en las regiones deseadas de la aplicación mediante la etiqueta **ColorPaletteResources**. Esta API no solo te permite aplicar a la vez un color y un tema a grandes grupos de controles mediante el establecimiento de unas pocas propiedades, sino que también te aporta muchas otras ventajas del sistema de las que normalmente no disfrutarías al definir colores personalizados manualmente:

- Los colores establecidos mediante **ColorPaletteResources** no tendrán efecto sobre el contraste alto.
  * Esto significa que la aplicación será accesible a más personas sin costes adicionales de diseño o desarrollo.
- Los colores se pueden establecer fácilmente en claro, oscuro o generalizado en ambos temas mediante el establecimiento de una propiedad en la API.
- Los colores establecidos en **ColorPaletteResources** se propagarán a todos los controles similares que también usen ese color del sistema.
  * Esto garantiza que habrá una pauta de colores uniforme en la aplicación, al mismo tiempo que se mantiene la apariencia de la marca.
- Afecta a todos los estados visuales, animaciones y variaciones de opacidad sin necesidad de crear una nueva plantilla.

### <a name="how-to-use-colorpaletteresources"></a>Cómo usar ColorPaletteResources

ColorPaletteResources es una API que le indica al sistema qué recursos están restringidos a un ámbito y dónde. ColorPaletteResources debe tener un atributo [x:Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), que puede ser uno de los tres siguientes:
- Predeterminado
  * Mostrará los cambios de color tanto en el tema [claro](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) como en el [oscuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme).
- Claro
  * Mostrará los cambios de color solo en el [tema claro](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme).
- Oscuro
  * Mostrará los cambios de color solo en el [tema oscuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme).

Al establecer el atributo x:Key, te asegurarás de que los colores cambien de una forma adecuada para el tema del sistema o de la aplicación, en el caso de que te interese que haya una apariencia personalizada diferente al usar cada uno de los temas.

### <a name="how-to-apply-scoped-colors"></a>Cómo aplicar colores con ámbito

El hecho de restringir el ámbito de los recursos mediante la API **ColorPaletteResources** en XAML te permite aprovechar cualquier color o pincel del sistema que se encuentre en nuestra biblioteca de [recursos de temas](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) y volver a definirlos dentro del ámbito de una página o contenedor.

Por ejemplo, si has definido dos colores del sistema (**BaseLow** y **BaseMediumLow**) en una cuadrícula y, luego, has colocado dos botones en la página (uno dentro de la cuadrícula y otro fuera):

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
        BaseLow="LightGreen"
        BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Obtendrás el objeto **Button_A** con los nuevos colores aplicados, mientras que el objeto **Button_B** mantendrá la apariencia de nuestro botón predeterminado del sistema:

![Colores del sistema con ámbito en el botón](images/color/scopedcolors_cyan_button.png)

A pesar de esto, dado que todos nuestros colores del sistema se propagan también a los demás controles, el hecho de establecer **BaseLow** y **BaseMediumLow** afectará a algo más que los botones. En este caso, los controles como **ToggleButton**, **RadioButton** y **Slider** también se verán afectados por estos cambios en el color del sistema, siempre y cuando estos controles se coloquen en el ámbito de la cuadrícula del ejemplo anterior.
Si quieres restringir el ámbito del cambio del color del sistema *a un solo control*, define **ColorPaletteResources** en los recursos de ese control:

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="LightGreen"
                BaseMediumLow="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Básicamente, tienes justo lo mismo que antes, pero ahora los demás controles que se agreguen a la cuadrícula no adoptarán los cambios de color. Esto se debe a que los colores del sistema tienen un ámbito restringido exclusivamente a **Button_A**.

### <a name="nesting-scoped-resources"></a>Anidar recursos con ámbito

Existe la posibilidad de anidar los colores del sistema. Para ello, es necesario colocar **ColorPaletteResources** en los recursos de los elementos anidados dentro del marcado del diseño de la aplicación:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorPaletteResources x:Key="Default"
            BaseLow="LightGreen"
            BaseMediumLow="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorPaletteResources x:Key="Default"
                BaseLow="Goldenrod"
                BaseMediumLow="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

En este ejemplo, **Button_A** hereda los colores definidos en los recursos de **Grid_A**, mientras que **Nested Button** hereda los colores de los recursos de **Grid_B**. Por extensión, esto significa que los demás controles que se coloquen en **Grid_B** comprobarán o aplicarán primero los recursos de **Grid_B**, antes de comprobar o aplicar los recursos de **Grid_A**. Por último, aplicarán nuestros colores predeterminados si no hay nada definido en el nivel de página o aplicación.

Esto funciona para cualquier número de elementos anidados cuyos recursos tengan definiciones de color.

### <a name="scoping-with-a-resourcedictionary"></a>Definición del ámbito con una clase ResourceDictionary

No hace falta que te limites a los recursos de una página o de un contenedor; también puedes definir estos colores del sistema en una clase ResourceDictionary que, después, se puede combinar en cualquier ámbito de la misma manera en que combinarías un diccionario.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

En primer lugar, crea una clase ResourceDictionary. Después, coloca **ColorPaletteResources** dentro de ThemeDictionaries y reemplaza los colores del sistema deseados:

```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TestApp">

    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key="Default">
            <ResourceDictionary.MergedDictionaries>

                <ColorPaletteResources x:Key="Default"
                    Accent="#FF0073CF"
                    AltHigh="#FF000000"
                    AltLow="#FF000000"/>

            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>        
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

#### <a name="mainpagexaml"></a>MainPage.xaml

En la página que contiene el diseño, basta con que combines ese diccionario en el ámbito que quieras:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
            <ResourceDictionary>
                <ResourceDictionary.MergedDictionaries>
                    <ResourceDictionary Source="MyCustomTheme.xaml"/>
                </ResourceDictionary.MergedDictionaries>
            </ResourceDictionary>
    </Grid.Resources>

    <Button Content="Button_A"/>
</Grid>
```

Ahora, todos los recursos, temas y colores personalizados pueden colocarse en un solo diccionario de recursos **MyCustomTheme** y definirse en un ámbito cuando sea necesario sin preocuparse por que se desordene el marcado de diseño.

### <a name="other-ways-to-define-color-resources"></a>Otras formas de definir recursos de color

ColorPaletteResources también permite colocar y definir colores del sistema directamente en su interior, como si fuera un contenedor, en lugar de en línea:

``` xaml
<ColorPaletteResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorPaletteResources>
```

## <a name="usability"></a>Facilidad de uso

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessibility should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the usability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the usability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Artículos relacionados

- [Estilos XAML](../controls-and-patterns/xaml-styles.md)
- [Recursos de temas XAML](../controls-and-patterns/xaml-theme-resources.md)
