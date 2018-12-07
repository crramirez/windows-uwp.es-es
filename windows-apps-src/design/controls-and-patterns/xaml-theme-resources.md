---
Description: Theme resources in XAML are a set of resources that apply different values depending on which system theme is active.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_theme\_resources
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Recursos de temas XAML
ms.assetid: 41B87DBF-E7A2-44E9-BEBA-AF6EEBABB81B
label: XAML theme resources
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 770896f467ff3a2c24fff65fdf16f1e13c83b688
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8785446"
---
# <a name="xaml-theme-resources"></a>Recursos de temas XAML

Los recursos de tema en XAML son un conjunto de recursos que aplican distintos valores según el tema del sistema que esté activo. Existen tres temas que admiten el marco XAML: "Light", "Dark" y "HighContrast".

**Prerequisites**: para este tema se presupone que has leído [Referencias a ResourceDictionary y a recursos XAML](resourcedictionary-and-xaml-resource-references.md).

## <a name="theme-resources-v-static-resources"></a>Comparación entre recursos de temas y recursos estáticos

Hay dos extensiones de marcado XAML que pueden hacer referencia a un recurso XAML de un diccionario de recursos XAML existente: la [extensión de marcado {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) y la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md).

La evaluación de una [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) se produce cuando la aplicación se carga y, posteriormente, cada vez que el tema cambia en tiempo de ejecución. Esto suele ocurrir cuando el usuario cambia la configuración de su dispositivo o cuando se produce un cambio de programación en la aplicación que modifique su tema actual.

En cambio, una [extensión de marcado {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) solo se evalúa cuando la aplicación carga por primera vez el código XAML. No se actualiza. Es similar a buscar y reemplazar en el código XAML con el valor real en tiempo de ejecución en el inicio de la aplicación.

## <a name="theme-resources-in-the-resource-dictionary-structure"></a>Recursos de temas en la estructura del diccionario de recursos

Cada recurso de tema es parte del archivo XAML themeresources.xaml. Por cuestiones de diseño, themeresources.xaml está disponible en la carpeta \\(Archivos de programa)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;Versión del SDK&gt;\\Generic de una instalación del Kit de desarrollo de software (SDK) de Windows. Los diccionarios de recursos de themeresources.xaml también se reproducen en generic.xaml en el mismo directorio.

Windows Runtime no usa estos archivos físicos para la búsqueda en tiempo de ejecución. Por este motivo, están específicamente en una carpeta DesignTime y no se copian a aplicaciones de forma predeterminada. En cambio, estos diccionarios de recursos existen en la memoria como parte del propio Windows Runtime y las referencias de los recursos XAML de la aplicación a recursos de tema (o recursos del sistema) se resuelven allí en tiempo de ejecución.

## <a name="guidelines-for-custom-theme-resources"></a>Directrices para recursos de temas personalizados

Sigue estas instrucciones cuando definas y consumas tus propios recursos de temas personalizados:

- Especifica los diccionarios de temas para "Light" y "Dark", además de tu diccionario "HighContrast". Aunque puedes crear un objeto [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794) con "Predeterminado" como clave, se prefiere que seas explícito y en su lugar uses "Light", "Dark" y "HighContrast".

- Usa la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) en: Estilos, Establecedores, Plantillas de controles, Establecedores de propiedades y Animaciones.

- No uses la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) en las definiciones de recursos dentro de [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807). Usa la [extensión de marcado {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) en su lugar.

    EXCEPCIÓN: se puede usar la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) como referencia para los recursos independientes del tema de la aplicación en tus [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807). Algunos ejemplos de estos recursos son recursos de color de énfasis como `SystemAccentColor`, o los recursos de color del sistema, que normalmente llevan el prefijo SystemColor como `SystemColorButtonFaceColor`.

> [!CAUTION]
> Si no sigues estas directrices es posible que veas un comportamiento inesperado con relación a los temas de tu aplicación. Para más información, consulta la sección [Solución de problemas de recursos de tema](#troubleshooting-theme-resources).

## <a name="the-xaml-color-ramp-and-theme-dependent-brushes"></a>Los pinceles dependientes del tema y la rampa de colores de XAML

El conjunto de colores combinado para los temas "Light", "Dark" y "HighContrast" constituyen la *Rampa de colores de Windows* en XAML. Si quieres modificar los temas del sistema o bien aplicar un tema del sistema a tus propios elementos XAML, es importante comprender cómo se estructuran los recursos de color.

Para obtener más información acerca de cómo aplicar color en tu aplicación para UWP, consulta [Color en aplicaciones para UWP](../style/color.md).

### <a name="light-and-dark-theme-colors"></a>Temas de colores Claro y Oscuro

El marco XAML proporciona un conjunto de recursos [Color](/uwp/api/Windows.UI.Color) designados con valores diseñados exclusivamente para los temas "Light" y "Dark". Las claves que usas para hacer referencia a estos tienen un formato de nombre de tipo: `System[Simple Light/Dark Name]Color`.

Esta tabla enumera la clave, el nombre simple y la representación de cadena del color (con el formato \#aarrggbb) para los recursos "Light" y "Dark" proporcionados por el marco XAML. La clave se usa para hacer referencia al recurso en una aplicación. El "Nombre simple claro/oscuro" se usa como parte de la convención de nomenclatura de pincel que explicaremos más adelante.

| Tecla                             | Nombre simple claro/oscuro | Light      | Dark       |
|---------------------------------|------------------------|------------|------------|
| SystemAltHighColor              | AltHigh                | \#FFFFFFFF | \#FF000000 |
| SystemAltLowColor               | AltLow                 | \#33FFFFFF | \#33000000 |
| SystemAltMediumColor            | AltMedium              | \#99FFFFFF | \#99000000 |
| SystemAltMediumHighColor        | AltMediumHigh          | \#CCFFFFFF | \#CC000000 |
| SystemAltMediumLowColor         | AltMediumLow           | \#66FFFFFF | \#66000000 |
| SystemBaseHighColor             | BaseHigh               | \#FF000000 | \#FFFFFFFF |
| SystemBaseLowColor              | BaseLow                | \#33000000 | \#33FFFFFF |
| SystemBaseMediumColor           | BaseMedium             | \#99000000 | \#99FFFFFF |
| SystemBaseMediumHighColor       | BaseMediumHigh         | \#CC000000 | \#CCFFFFFF |
| SystemBaseMediumLowColor        | BaseMediumLow          | \#66000000 | \#66FFFFFF |
| SystemChromeAltLowColor         | ChromeAltLow           | \#FF171717 | \#FFF2F2F2 |
| SystemChromeBlackHighColor      | ChromeBlackHigh        | \#FF000000 | \#FF000000 |
| SystemChromeBlackLowColor       | ChromeBlackLow         | \#33000000 | \#33000000 |
| SystemChromeBlackMediumLowColor | ChromeBlackMediumLow   | \#66000000 | \#66000000 |
| SystemChromeBlackMediumColor    | ChromeBlackMedium      | \#CC000000 | \#CC000000 |
| SystemChromeDisabledHighColor   | ChromeDisabledHigh     | \#FFCCCCCC | \#FF333333 |
| SystemChromeDisabledLowColor    | ChromeDisabledLow      | \#FF7A7A7A | \#FF858585 |
| SystemChromeHighColor           | ChromeHigh             | \#FFCCCCCC | \#FF767676 |
| SystemChromeLowColor            | ChromeLow              | \#FFF2F2F2 | \#FF171717 |
| SystemChromeMediumColor         | ChromeMedium           | \#FFE6E6E6 | \#FF1F1F1F |
| SystemChromeMediumLowColor      | ChromeMediumLow        | \#FFF2F2F2 | \#FF2B2B2B |
| SystemChromeWhiteColor          | ChromeWhite            | \#FFFFFFFF | \#FFFFFFFF |
| SystemListLowColor              | ListLow                | \#19000000 | \#19FFFFFF |
| SystemListMediumColor           | ListMedium             | \#33000000 | \#33FFFFFF |

:::row:::
    :::column:::
        #### Light theme
    :::column-end:::
    :::column:::
        #### Dark theme
    :::column-end:::
:::row-end:::

#### <a name="base"></a>Base

:::row:::
    :::column:::
        ![The base light theme](images/themes/light-base.png)
    :::column-end:::
    :::column:::
        ![The base dark theme](images/themes/dark-base.png)
    :::column-end:::
:::row-end:::

#### <a name="alt"></a>Alt

:::row:::
    :::column:::
        ![The alt light theme](images/themes/light-alt.png)
    :::column-end:::
    :::column:::
        ![The alt dark theme](images/themes/dark-alt.png)
    :::column-end:::
:::row-end:::

#### <a name="list"></a>Lista

:::row:::
    :::column:::
        ![The list light theme](images/themes/light-list.png)
    :::column-end:::
    :::column:::
        ![The list dark theme](images/themes/dark-list.png)
    :::column-end:::
:::row-end:::

#### <a name="chrome"></a>Chrome

:::row:::
    :::column:::
        ![The chrome light theme](images/themes/light-chrome.png)
    :::column-end:::
    :::column:::
        ![The chrome dark theme](images/themes/dark-chrome.png)
    :::column-end:::
:::row-end:::

### <a name="windows-system-high-contrast-colors"></a>Colores en contraste alto del sistema de Windows

Además del conjunto de recursos proporcionados por el marco XAML, hay un conjunto de valores de color derivado de la paleta del sistema de Windows. Estos colores no son específicos de las aplicaciones de Windows Runtime o de la Plataforma universal de Windows (UWP). Sin embargo, muchos de los recursos XAML [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) consumen estos colores cuando el sistema está funcionando (y la aplicación está ejecutándose) usando el tema "HighContrast". El marco XAML proporciona estos colores de todo el sistema como recursos con clave. Las claves siguen el formato de nombre: `SystemColor[name]Color`.

Esta tabla enumera los colores de todo el sistema que XAML proporciona como objetos de recursos derivados de la paleta del sistema de Windows. La columna "Nombre de accesibilidad" muestra cómo se etiqueta el color en la interfaz de usuario de la configuración de Windows. La columna "Nombre simple de contraste alto" es una descripción en una palabra de cómo se aplica el color en los controles comunes de XAML. Se usa como parte de la convención de nomenclatura de pincel que explicaremos más adelante. La columna "Predeterminado inicial" muestra los valores que se obtendrían si el sistema no se ejecutara en contraste alto.

| Tecla                           | Nombre de accesibilidad            | Nombre simple de contraste alto | Predeterminado inicial |
|-------------------------------|--------------------------------|--------------------------|-----------------|
| SystemColorButtonFaceColor    | **Texto del botón** (segundo plano)   | Background               | \#FFF0F0F0      |
| SystemColorButtonTextColor    | **Texto del botón** (primer plano)   | Primer plano               | \#FF000000      |
| SystemColorGrayTextColor      | **Texto deshabilitado**              | Deshabilitada                 | \#FF6D6D6D      |
| SystemColorHighlightColor     | **Texto seleccionado** (segundo plano) | Resaltar                | \#FF3399FF      |
| SystemColorHighlightTextColor | **Texto seleccionado** (primer plano) | HighlightAlt             | \#FFFFFFFF      |
| SystemColorHotlightColor      | **Hyperlinks**                 | Hyperlink                | \#FF0066CC      |
| SystemColorWindowColor        | **Background**                 | PageBackground           | \#FFFFFFFF      |
| SystemColorWindowTextColor    | **Text**                       | PageText                 | \#FF000000      |

Windows proporciona distintos temas de contraste alto y permite al usuario establecer los colores específicos para su configuración de contraste alto a través del Centro de accesibilidad, como se muestra aquí. Por lo tanto, no es posible proporcionar una lista definitiva de valores de color de contraste alto.

![La interfaz de usuario de la configuración de contraste alto de Windows](images/high-contrast-settings.png)

Para más información acerca de la compatibilidad los temas de contraste alto, consulta [Temas de contraste alto](https://msdn.microsoft.com/library/windows/apps/mt244346).

### <a name="system-accent-color"></a>Color de énfasis del sistema

Además de los colores de tema de contraste alto del sistema, el color de énfasis del sistema se proporciona como un recurso de color especial con la clave `SystemAccentColor`. En tiempo de ejecución, este recurso toma el color que el usuario haya especificado como color de énfasis en la configuración de personalización de Windows.

> [!NOTE]
> Si bien es posible reemplazar los recursos de color del sistema, se recomienda respetar los colores elegidos por el usuario, especialmente para la configuración de contraste alto.

### <a name="theme-dependent-brushes"></a>Pinceles dependientes del tema

Los recursos de color que se muestra en las secciones anteriores se usan para establecer la propiedad [Color](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) de los recursos [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) en los diccionarios de recursos de temas del sistema. Usa los recursos de pincel para aplicar el color a elementos XAML. Las claves para los recursos de pincel tienen el siguiente formato de nombre: `SystemControl[Simple HighContrast name][Simple light/dark name]Brush`. Por ejemplo: `SystemControlBackroundAltHighBrush`.

Veamos cómo se determina el valor de color de este pincel en tiempo de ejecución. En los diccionarios de recursos "Light" y "Dark", el pincel se define así:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{StaticResource SystemAltHighColor}"/>`

En el diccionario de recursos "HighContrast", el pincel se define así:

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>`

Cuando este pincel se aplica a un elemento XAML, el tema actual determina su color en tiempo de ejecución, como se muestra en esta tabla.

| Tema        | Nombre simple del color | Recurso de color             | Valor en tiempo de ejecución                                              |
|--------------|-------------------|----------------------------|------------------------------------------------------------|
| Light        | AltHigh           | SystemAltHighColor         | \#FFFFFFFF                                                 |
| Dark         | AltHigh           | SystemAltHighColor         | \#FF000000                                                 |
| HighContrast | Background        | SystemColorButtonFaceColor | El color especificado en la configuración para el fondo del botón. |

Puedes usar el esquema de nombre `SystemControl[Simple HighContrast name][Simple light/dark name]Brush` para determinar qué pincel aplicar a tus propios elementos XAML.

<!--
For many examples of how the brushes are used in the XAML control templates, see the [Default control styles and templates](default-control-styles-and-templates.md).
-->

> [!NOTE]
> No todas las combinaciones de \[*Nombre simple de contraste alto*\]\[*Nombre simple claro/oscuro*\] se proporcionan como recurso de pincel.

## <a name="the-xaml-type-ramp"></a>La rampa de tipos XAML

El archivo themeresources.xaml define varios recursos que definen un [Style](https://msdn.microsoft.com/library/windows/apps/br208849) que puedes aplicar a los contenedores de texto en tu interfaz de usuario, específicamente para [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) o [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565). No son los estilos implícitos predeterminados. Se proporcionan para facilitar la creación de definiciones de interfaces de usuario de XAML que coincidan con la *Rampa de tipos de Windows* documentada en [Directrices para fuentes](../style/typography.md).

Estos estilos son para atributos de texto que quieres aplicar a todo el contenedor de texto. Si quieres que los estilos se apliquen solo a secciones del texto, debes establecer los atributos en los elementos de texto dentro del contenedor, como en [Run](https://msdn.microsoft.com/library/windows/apps/br209959) para [TextBlock.Inlines](https://msdn.microsoft.com/library/windows/apps/br209668) o en [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244503) para [RichTextBlock.Blocks](https://msdn.microsoft.com/library/windows/apps/br244347).

Los estilos tienen este aspecto cuando se aplican a un [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652):

![estilos de bloque de texto](../style/images/type/text-block-type-ramp.svg)

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

Para obtener instrucciones sobre cómo usar la tabla de tipos UWP en la aplicación, consulta [Tipografía en aplicaciones para UWP](../style/typography.md).

### <a name="basetextblockstyle"></a>BaseTextBlockStyle

**TargetType**: [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

Proporciona las propiedades comunes para todos los demás estilos contenedores de [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652).

```XAML
<!-- Usage -->
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
</Style>
```

### <a name="headertextblockstyle"></a>HeaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="HeaderTextBlockStyle" TargetType="TextBlock"
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="46"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subheadertextblockstyle"></a>SubheaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubheaderTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="34"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="titletextblockstyle"></a>TitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="TitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="SemiLight"/>
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subtitletextblockstyle"></a>SubtitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubtitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodytextblockstyle"></a>BodyTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BodyTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="15"/>
</Style>
```

### <a name="captiontextblockstyle"></a>CaptionTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="CaptionTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="12"/>
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

### <a name="baserichtextblockstyle"></a>BaseRichTextBlockStyle

**TargetType**: [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565)

Proporciona las propiedades comunes para todos los demás estilos contenedores de [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565).

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BaseRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BaseRichTextBlockStyle" TargetType="RichTextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodyrichtextblockstyle"></a>BodyRichTextBlockStyle

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BodyRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BodyRichTextBlockStyle" TargetType="RichTextBlock" BasedOn="{StaticResource BaseRichTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

**Nota**: los estilos [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565) no tienen todos los estilos de rampa de texto que realiza el [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) , principalmente porque el modelo de objetos de documento basado en bloques de **RichTextBlock** es más fácil establecer los atributos en el texto individual elementos. Además, al establecer [TextBlock.Text](https://msdn.microsoft.com/library/windows/apps/br209676) mediante la propiedad de contenido XAML se crea una situación en la que no hay ningún elemento de texto para el estilo y, por tanto, tendrías que aplicar estilo al contenedor. Esto no supone ningún problema para **RichTextBlock**, porque su contenido de texto siempre tiene que estar en elementos de texto específicos, como [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244503), que es donde probablemente aplicarás los estilos XAML para el encabezado de página, el subtítulo de página y las definiciones de rampa de texto similares.

## <a name="miscellaneous-named-styles"></a>Varios estilos con nombre

Existe un conjunto adicional de definiciones [Style](https://msdn.microsoft.com/library/windows/apps/br208849) con clave que se puede usar para aplicar estilo a un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) de forma diferente a su estilo implícito predeterminado.

### <a name="textblockbuttonstyle"></a>TextBlockButtonStyle

**TargetType**: [ButtonBase](https://msdn.microsoft.com/library/windows/apps/br227736)

Aplicar este estilo para un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) cuando necesites mostrar texto en el que un usuario pueda hacer clic para realizar una acción. Al texto se le aplica estilo con el color de énfasis actual para distinguirlo como interactivo y tiene rectángulos de foco que funcionan bien con texto. A diferencia del estilo implícito de un [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739), el **TextBlockButtonStyle** no subraya el texto.

La plantilla también aplica estilo al texto presentado para usar **SystemControlHyperlinkBaseMediumBrush** (para el estado "PointerOver"), **SystemControlHighlightBaseMediumLowBrush** (para el estado "Pressed") y **SystemControlDisabledBaseLowBrush** (para el estado "Disabled").

Este es un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) al que se le ha aplicado el recurso **TextBlockButtonStyle**.

```XAML
<Button Content="Clickable text" Style="{StaticResource TextBlockButtonStyle}"
        Click="Button_Click"/>
```

Tiene esta apariencia:

![Un botón al que se le aplica estilo para que tenga el aspecto de texto](images/styles-textblock-button-style.png)

### <a name="navigationbackbuttonnormalstyle"></a>NavigationBackButtonNormalStyle

**TargetType**: [Button](https://msdn.microsoft.com/library/windows/apps/br209265)

Este [Style](https://msdn.microsoft.com/library/windows/apps/br208849) proporciona una plantilla completa para un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) que puede ser el botón de retroceso en la navegación para una aplicación de navegación. Las dimensiones predeterminadas son 40 x 40 píxeles. Para personalizar el estilo puedes establecer explícitamente la [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [FontSize](https://msdn.microsoft.com/library/windows/apps/br209406) y otras propiedades en tu **Button** o crear un estilo derivado mediante [BasedOn](https://msdn.microsoft.com/library/windows/apps/br208852).

Este es un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) al que se ha aplicado el recurso **NavigationBackButtonNormalStyle**.

```XAML
<Button Style="{StaticResource NavigationBackButtonNormalStyle}" />
```

Tiene esta apariencia:

![Un botón al que se le ha aplicado el estilo de botón de retroceso](images/styles-back-button-normal.png)

### <a name="navigationbackbuttonsmallstyle"></a>NavigationBackButtonSmallStyle

**TargetType**: [Button](https://msdn.microsoft.com/library/windows/apps/br209265)

Este [Style](https://msdn.microsoft.com/library/windows/apps/br208849) proporciona una plantilla completa para un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) que puede ser el botón de retroceso en la navegación para una aplicación de navegación. Es similar a **NavigationBackButtonNormalStyle**, pero con unas dimensiones de 30 x 30 píxeles.

Este es un [Button](https://msdn.microsoft.com/library/windows/apps/br209265) al que se ha aplicado el recurso **NavigationBackButtonSmallStyle**.

```XAML
<Button Style="{StaticResource NavigationBackButtonSmallStyle}" />
```

## <a name="troubleshooting-theme-resources"></a>Solución de recursos de temas

Si no sigues las directrices [para usar recursos de temas](#guidelines-for-using-theme-resources), es posible que veas un comportamiento inesperado en relación con los temas de tu aplicación.

Por ejemplo, cuando abres un control flotante con temas claros, partes de la aplicación con temas oscuros también cambian como si estuvieran en el tema claro. O si navegas a una página con temas claros y, a continuación, retrocedes en la navegación, la página original con temas oscuros (o partes de él) ahora tienen una apariencia como si estuvieran en un tema claro.

Normalmente, estos tipos de problemas se producen al proporcionar un tema "Predeterminado" y un tema "HighContrast" para admitir escenarios de contraste alto y, a continuación, usar ambos temas, "Light" y "Dark", en distintas partes de la aplicación.

Por ejemplo, ten en cuenta esta definición del diccionario de temas:

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Intuitivamente, esto parece correcto. Quieres cambiar el color señalado por `myBrush` cuando está en contraste alto, pero cuando no, te basas en la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) para asegurarte de que `myBrush` apunte al color correcto para tu tema. Si la aplicación nunca tiene [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/dn298515) establecido en elementos dentro de su árbol visual, esto normalmente funcionará según lo esperado. Sin embargo, te encuentras problemas en la aplicación en cuanto empiezas a cambiar el tema de distintas partes de tu árbol visual.

El problema se produce porque los pinceles son recursos compartidos, a diferencia de la mayoría del resto de tipos XAML. Si tienes 2 elementos en subárboles XAML con distintos temas que hacen referencia al mismo recurso de pincel, entonces a medida que el marco recorre cada subárbol para actualizar sus expresiones de la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md), los cambios que se producen en el recurso compartido de pincel se reflejan en el otro subárbol, que no es el resultado previsto.

Para corregir esto, reemplaza el diccionario "Predeterminado" por diccionarios de temas independientes para los temas "Light" y "Dark" además de "HighContrast":

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Sin embargo, seguirán produciéndose problemas si se hace referencia a cualquiera de estos recursos en propiedades heredadas como [Foreground](https://msdn.microsoft.com/library/windows/apps/br209414). La plantilla de control personalizado puede especificar el color de primer plano de un elemento usando la [{extensión de marcado ThemeResource}](../../xaml-platform/themeresource-markup-extension.md), pero cuando el marco propaga el valor heredado a los elementos secundarios, proporciona una referencia directa al recurso que resuelve la expresión de la extensión de marcado {ThemeResource}. Esto provoca problemas cuando el marco procesa los cambios de tema a medida que recorre el árbol visual del control. Vuelve a evaluar la expresión de la extensión de marcado {ThemeResource} para obtener un nuevo recurso de pincel, pero aún no propaga esta referencia a los elementos secundarios del control; esto sucede más adelante, por ejemplo durante el pase de medición siguiente.

Como resultado, después de recorrer el árbol visual del control en respuesta a un cambio de tema, el marco recorre los elementos secundarios y actualiza cualquier expresión de la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) que se establezca en estos o en los objetos establecidos en sus propiedades. En este punto se produce el problema: el marco recorre el recurso de pincel y como especifica su color usando una extensión de marcado {ThemeResource}, se vuelve a evaluar.

En este punto, el marco parece haber contaminado el diccionario de temas porque ahora tiene un recurso de un diccionario que tiene el color establecido de otro diccionario.

Para solucionar este problema, usa la [extensión de marcado {StaticResource}](../../xaml-platform/staticresource-markup-extension.md) en lugar de la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md). Una vez aplicadas las directrices, los diccionarios de temas tienen este aspecto:

```XAML
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

Observa que la [extensión de marcado {ThemeResource}](../../xaml-platform/themeresource-markup-extension.md) aún se usa en el diccionario "HighContrast" en lugar de la extensión de marcado [{StaticResource}](../../xaml-platform/staticresource-markup-extension.md). Esta situación está incluida en la excepción descrita anteriormente en las directrices. La mayoría de los valores que se usan para el tema "HighContrast" emplean opciones de colores que están controladas por el sistema de manera global, pero que se exponen a XAML como un recurso con nombre especial (aquellos con el prefijo 'SystemColor' en el nombre). El sistema permite al usuario establecer los colores específicos que deben usarse para su configuración de contraste alto a través del Centro de accesibilidad. Estas opciones de color se aplican a los recursos con nombre especial. El marco XAML también usa el mismo evento de tema cambiado para actualizar estos pinceles cuando detecta que han cambiado en el nivel del sistema. Este es el motivo por el que se usa aquí la extensión de marcado {ThemeResource}.