---
description: Reveal Focus es un efecto de iluminación que anima el borde de los elementos activables cuando el usuario mueve a ellos el foco del teclado o del controlador para juegos.
title: Reveal Focus
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 824476cb098d0ff561fca67497a896586c70b8fb
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681966"
---
# <a name="reveal-focus"></a>Reveal Focus

![imagen principal](images/header-reveal-focus.svg)

Reveal Focus es un efecto de iluminación para [experiencias de 3 metros](/windows/uwp/design/devices/designing-for-tv), como las pantallas de televisión y la Xbox One. Anima el borde de los elementos activables, como los botones, cuando el usuario mueve a ellos el foco del teclado o del controlador para juegos. Está desactivado de manera predeterminada, pero es muy sencillo habilitarlo. 

(Para el efecto Mostrar resaltado, un efecto de iluminación que resalta elementos interactivos, consulta el artículo [Mostrar resaltado](/windows/uwp/design/style/reveal)).


> **API importantes**: [propiedad Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [enumeración FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [propiedad Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Cómo funciona
Reveal Focus llama la atención sobre los elementos enfocados al agregar un efecto de iluminado animado alrededor del borde del elemento:

![Elemento visual Reveal](images/traveling-focus-fullscreen-light-rf.gif)

Esto es especialmente útil en escenarios de 3 metros, donde el usuario podría no estar prestando toda su atención a la pantalla de televisión completa. 

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RevealFocus">abrir la aplicación y ver Reveal Focus en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Cómo se usa

La opción Reveal Focus está desactivada de manera predeterminada. Para habilitarla:
1. En el constructor de la aplicación, llama a la propiedad [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) y comprueba si la familia de dispositivos actual es `Windows.Xbox`.
2. Si la familia de dispositivos es `Windows.Xbox`, establece la propiedad [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) en `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Después de establecer la propiedad **FocusVisualKind**, el sistema aplica automáticamente el efecto Reveal Focus en todos los controles cuya propiedad [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) esté establecida en **True** (valor predeterminado de la mayoría de los controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>¿Por qué Reveal Focus no está activado de manera predeterminada? 
Como puedes ver, es bastante sencillo activar Reveal Focus cuando la aplicación detecta que se está ejecutando en una Xbox. Por lo tanto, ¿por qué el sistema no lo activa automáticamente? Porque Reveal Focus aumenta el tamaño del elemento visual de foco, lo que podría causar problemas con tu diseño de la interfaz de usuario. En algunos casos, te interesará personalizar el efecto Reveal Focus para optimizarlo para tu aplicación.

## <a name="customizing-reveal-focus"></a>Personalización de Reveal Focus

Para personalizar el efecto Reveal Focus, modifica las propiedades visuales del elemento visual de foco para cada control: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) y [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Estas propiedades te permiten personalizar el color y el grosor del rectángulo del foco. (Son las mismas propiedades que usas para crear [Elementos visuales de foco de alta visibilidad](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals)). 

Pero antes de empezar la personalización, es útil saber un poco más sobre los componentes que componen Reveal Focus.

Hay tres partes en los elementos visuales de Reveal Focus predeterminados: el borde principal, el borde secundario y el efecto de iluminado de Reveal. El borde principal es de **2px** de espesor y se ejecuta alrededor de la parte de *fuera* del borde secundario. El borde secundario es de **1px** de espesor y se ejecuta alrededor de la parte de *dentro* del borde primario. El efecto de iluminado de Reveal Focus tiene un grosor proporcional al grosor del borde principal y rodea el *exterior* del borde principal.

Además de los elementos estáticos, los elementos visuales de Reveal Focus presentan una luz animada que late cuando está en reposo y que se mueve en la dirección del foco al moverlo.

![Capas de Reveal Focus](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personalizar el grosor del borde

Para cambiar el grosor de los tipos de borde de un control, usa estas propiedades:

| Tipo de borde | Propiedad |
| --- | --- |
| Principal, efecto de iluminado   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (Al cambiar el borde principal, se cambia el grosor del efecto de iluminado de manera proporcional).   |
| Secundario   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


En este ejemplo se cambia el grosor del borde del elemento visual de foco de un botón:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personalizar el margen

El margen es el espacio entre los límites de los elementos visuales del control y el inicio del borde secundario de los elementos visuales de foco. El margen predeterminado se encuentra a una distancia de 1 px de los límites del control. Puedes editar este margen según el control si cambias la propiedad [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin):

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Un margen negativo aleja el borde del centro del control, mientras que un margen positivo acerca el borde al centro del control.

## <a name="customize-the-color"></a>Personalizar el color

Para cambiar el color del elemento visual Reveal Focus, usa las propiedades [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) y [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush).

| Propiedad | Recurso predeterminado | Valor del recurso predeterminado |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(La propiedad FocusPrimaryBrush solo se establece en los recursos **SystemControlRevealFocusVisualBrush** de forma predeterminada cuando **FocusVisualKind** se establece en **Reveal**. De lo contrario, usa **SystemControlFocusVisualPrimaryBrush**).


Para cambiar el color de elemento visual de foco de un control individual, establece las propiedades directamente en el control. En este ejemplo se reemplazan los colores de los elementos visuales de foco de un botón.

```xaml

<!-- Specifying a color directly -->
<Button
    FocusVisualPrimaryBrush="DarkRed"
    FocusVisualSecondaryBrush="Pink"/>

<!-- Using theme resources -->
<Button
    FocusVisualPrimaryBrush="{ThemeResource SystemBaseHighColor}"
    FocusVisualSecondaryBrush="{ThemeResource SystemAccentColor}"/>    
```

Para cambiar el color de todos los elementos visuales de foco en toda la aplicación, reemplaza los recursos **SystemControlRevealFocusVisualBrush** y **SystemControlFocusVisualSecondaryBrush** por tu propia definición:

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Para obtener más información sobre la modificación del color del elemento visual de foco, consulta [Personalización de marca de color y personalización](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Mostrar solo el efecto de iluminado

Si quieres usar solo el efecto de iluminado sin el elemento visual de foco principal o secundario, solo tienes que establecer la propiedad [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) del control en `Transparent` y [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) en `0`. En este caso, el efecto de iluminado adoptará el color de fondo del control para proporcionar una apariencia sin bordes. Puedes modificar el grosor del efecto de iluminado con [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Usar tus propios elementos visuales de foco

Otra manera de personalizar Reveal Focus consiste en descartar los elementos visuales de foco proporcionados por el sistema. Para ello, dibuja los tuyos propios mediante los estados visuales. Para obtener más información, consulta el [ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Reveal Focus y Fluent Design System

Reveal Focus es un componente de Fluent Design System que agrega luz a tu aplicación. Para obtener más información sobre Fluent Design System y sus otros componentes, consulta la [introducción a Fluent Design para UWP](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Artículos relacionados

- [Mostrar resaltado](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Diseño para Xbox y televisión](/windows/uwp/design/devices/designing-for-tv)
- [Interacciones con controlador para juegos y control remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
- [Efectos de composición](https://docs.microsoft.com/windows/uwp/graphics/composition-effects)
- [Ciencia en el sistema: Fluent Design y profundidad](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciencia en el sistema: Fluent Design y luz](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
