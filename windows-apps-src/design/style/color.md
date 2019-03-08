---
description: Aprende a usar colores de énfasis y temas en tus aplicaciones para UWP.
title: Color en las aplicaciones para UWP
ms.date: 04/7/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: karenmui
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 49d891888e26b6ce4c9f94e92605eaf7d619b6f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654260"
---
# <a name="color"></a>Color

![imagen principal](images/header-color.svg)

El color proporciona una forma intuitiva de comunicar información a los usuarios en tu aplicación: puede usarse para indicar interactividad, proporcionar comentarios a las acciones del usuario y dar una sensación de continuidad visual a tu interfaz. 

En aplicaciones para UWP, los colores vienen determinados principalmente por color de énfasis y tema. En este artículo, trataremos cómo puedes usar el color en la aplicación y cómo usar los recursos de tema y color de énfasis para que tu aplicación para UWP se pueda usar en cualquier contexto de tema. 

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

**Nota**: En Visual Studio, el valor predeterminado RequestedTheme es claro, por lo que necesitará cambiar el RequestedTheme para probar ambas.

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

Para obtener más información acerca de cómo usar pinceles de temas en tu aplicación, consulta [Recursos de temas](../controls-and-patterns/xaml-theme-resources.md).

## <a name="accent-color"></a>Color de énfasis

Los controles comunes usan un color de énfasis para transmitir información de estado. De manera predeterminada, el color de énfasis es el `SystemAccentColor` que los usuarios seleccionan en su Configuración. Sin embargo, también puedes personalizar el color de énfasis de tu aplicación para reflejar tu marca.

![controles de Windows](images/color/windows-controls.svg)

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

<!-- check this is true --> También puede tener acceso a la paleta de colores de énfasis mediante programación con el [ **UISettings.GetColorValue** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UISettings#Windows_UI_ViewManagement_UISettings_GetColorValue_Windows_UI_ViewManagement_UIColorType_) método y [ **UIColorType** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIColorType) valor enum.

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

![color sobre color](images/color/color-on-color.png)

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

Las letras "Argb" significa Alfa (opacidad), Rojo, Verde y Azul, que son los cuatro componentes de un color. Cada argumente pueden oscilar entre 0 y 255 minutos. Puedes elegir omitir el primer valor, que te dará una opacidad predeterminada de 255 o 100 % opaco.

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

## <a name="scoping-system-colors"></a>Ámbito de los colores del sistema

Además de definir sus propios colores en la aplicación, también puede limitarse nuestros systematized colores en las regiones deseadas en toda la aplicación mediante el uso de la **ColorSchemeResources** etiqueta. Esta API permite no solo para colorear y normalmente no obtenía grandes grupos de controles a la vez estableciendo algunas propiedades, pero también proporciona muchos otros sistemas que beneficia de tema con definir sus propios colores personalizados manualmente:

- Cualquier color que se establece utilizando **ColorSchemeResources** no tendrá efecto contraste alto
  * Lo que significa que la aplicación será accesible a más personas sin diseño adicionales ni costos de desarrollo
- Puede establecer fácilmente colores claro, oscuro o generalizada en ambos temas estableciendo una propiedad en la API
- Conjunto de colores en **ColorSchemeResources** se realizará en cascada a todos los controles similares que también use ese color del sistema
  * Esto garantiza que habrá un caso de un color uniforme a través de la aplicación al tiempo que mantiene la apariencia de su marca
- Afecta a todos los estados visuales, animaciones y las variaciones de opacidad sin necesidad de re-template

### <a name="how-to-use-colorschemeresources"></a>Cómo usar ColorSchemeResources

ColorSchemeResources es una API que indica que el sistema que se va qué recursos con ámbito where. ColorSchemeResources debe tener un [x: Key](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute), que puede ser uno de tres opciones:
- Predeterminado
  * Mostrará los cambios de color en ambos [luz](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) y [oscuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme) tema
- Claro
  * Se mostrará su color cambia únicamente en [el tema claro](https://docs.microsoft.com/windows/uwp/design/style/color#light-theme) 
- Oscuro
  * Se mostrará su color cambia únicamente en [tema oscuro](https://docs.microsoft.com/windows/uwp/design/style/color#dark-theme)

Establecer x: Key se asegurará de que sus colores cambian de manera apropiada para el tema del sistema o aplicación, si quiere una apariencia personalizada diferente de cualquier tema que.

### <a name="how-to-apply-scoped-colors"></a>Cómo aplicar los colores con ámbito

Definir el ámbito de los recursos a través de la **ColorSchemeResources** API en XAML le permite aprovechar cualquier color del sistema o el pincel que se encuentra en nuestro [los recursos de tema](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-theme-resources) biblioteca y volver a definirlos dentro del ámbito de una página o contenedor.

Por ejemplo, si se ha definido dos colores del sistema - **SystemBaseLowColor** y **SystemBaseMediumLowColor** dentro de una cuadrícula y, a continuación, colocar dos botones en la página: uno dentro de la cuadrícula y fuera de uno:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default" 
        SystemBaseLowColor="LightGreen" 
        SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Buton Content="Button_A"/>
</Grid>
<Buton Content="Button_B"/>
```

Obtendría **Button_A** con los nuevos colores aplicados y **Button_B** permanecería atractivo, como nuestro botón predeterminado de sistema:

![colores del sistema con ámbito en el botón](images/color/scopedcolors_cyan_button.png)

Sin embargo, puesto que nuestro todos los colores del sistema en cascada en otros controles demasiado, establecer **SystemBaseLowColor** y **SystemBaseMediumLowColor** afectará a algo más que los botones. En este caso, los controles, como **ToggleButton**, **RadioButton** y **control deslizante** también se verán afectados por estos cambios de color del sistema, estos controles se deben colocar por encima de ejemplo ámbito de la cuadrícula.
Si desea definir el ámbito de un cambio de color del sistema *a una sola controla solo* puede hacerlo mediante la definición de **ColorSchemeResources** dentro de los recursos de ese control:

```xaml
<Grid x:Name="Grid_A">
    <Button Content="Button_A">
        <Button.Resources>
            <ColorSchemeResources x:Key="Default" 
                SystemBaseLowColor="LightGreen" 
                SystemBaseMediumLowColor="DarkCyan"/>
        </Button.Resources>
    </Button>
</Grid>
<Button Content="Button_B"/>
```
Básicamente, tiene exactamente lo mismo que antes, pero ahora ningún otro control que se agrega a la cuadrícula no recogerá los cambios de color. Esto es porque los colores del sistema se limitan a **Button_A** solo.

### <a name="nesting-scoped-resources"></a>Recursos de anidamiento de ámbito

Anidamiento de colores del sistema también es posible y se realiza colocando **ColorSchemeResources** en recursos de los elementos anidados dentro del marcado de su diseño de la aplicación:

```xaml
<Grid x:Name="Grid_A">
    <Grid.Resources>
        <ColorSchemeResources x:Key="Default"
            SystemBaseLowColor="LightGreen"
            SystemBaseMediumLowColor="DarkCyan"/>
    </Grid.Resources>

    <Button Content="Button_A"/>
    <Grid x:Name="Grid_B">
        <Grid.Resources>
            <ColorSchemeResources x:Key="Default"
                SystemBaseLowColor="Goldenrod"
                SystemBaseMediumLowColor="DarkGoldenrod"/>
        </Grid.Resources>

        <Button Content="Nested Button"/>
    </Grid>
</Grid>
```

En este ejemplo, **Button_A** es definir colores heredados en **Grid_A**de recursos, y **botón anidados** hereda los colores de **Grid_B**de recursos. Por extensión, esto significa que ningún otro control ubicado dentro de **Grid_B** se compruebe o aplicar **Grid_B**de recursos por primera vez, antes de comprobar o aplicar **Grid_A**del recursos y, por último, aplicando nuestra colores predeterminados si no se define en el nivel de página o aplicación.

Esto funciona para cualquier número de elementos anidados cuyos recursos tienen las definiciones de color.

### <a name="scoping-with-a-resourcedictionary"></a>Ámbito con un objeto ResourceDictionary

No se limitan a un contenedor o los recursos de la página y también se puede definir estos colores del sistema en un ResourceDictionary que, a continuación, se puede combinar en cualquier ámbito de la manera en que normalmente podría combinar un diccionario.

#### <a name="mycustomthemexaml"></a>MyCustomTheme.xaml

En primer lugar, debe crear un objeto ResourceDictionary. A continuación, coloque el **ColorPaletteResources** dentro de la ThemeDictionaries e invalidar los colores del sistema deseado:

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

En la página que contiene el diseño, basta con combinar ese diccionario en el ámbito que desee:

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

Ahora, todos los recursos, temas y colores personalizados pueden colocarse en una sola **MyCustomTheme** diccionario de recursos y un ámbito donde sea necesario sin tener que preocuparse de desorden adicional en el marcado de diseño.

### <a name="other-ways-to-define-color-resources"></a>Otras formas de definir recursos de color

También permite ColorSchemeResources colores del sistema para colocarse y se definen directamente dentro de él como un contenedor, en lugar de en línea:

``` xaml
<ColorSchemeResources x:Key="Dark">
    <Color x:Key="SystemBaseLowColor">Goldenrod</Color>
</ColorSchemeResources>
```

## <a name="usability"></a>Facilidad de uso

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-contrast.svg)
    :::column-end:::
    :::column span="2":::
        **Contrast**

        Make sure that elements and images have sufficient contrast to differentiate between them, regardless of the accent color or theme.

        When considering what colors to use in your application, accessiblity should be a primary concern. Use the guidance below to make sure your application is accessible to as many users as possible.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-lighting.svg)
    :::column-end:::
    :::column span="2":::
        **Lighting**

        Be aware that variation in ambient lighting can affect the useability of your app. For example, a page with a black background might unreadable outside due to screen glare, while a page with a white background might be painful to look at in a dark room.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![contrast illustration](images/color/illo-colorblindness.svg)
    :::column-end:::
    :::column span="2":::
        **Colorblindness**

        Be aware of how colorblindness could affect the useability of your application. For example, a user with red-green colorblindness will have difficulty distinguishing red and green elements from each other. About **8 percent of men** and **0.5 percent of women** are red-green colorblind, so avoid using these color combinations as the sole differentiator between application elements.
    :::column-end:::
:::row-end:::

## <a name="related-articles"></a>Artículos relacionados

- [Estilos XAML](../controls-and-patterns/xaml-styles.md)
- [Recursos de tema XAML](../controls-and-patterns/xaml-theme-resources.md)
