---
description: Revelar el que foco es un efecto de iluminación que anima el borde de elementos puede recibir el foco cuando el usuario mueve el foco de teclado o el controlador para juegos a ellos.
title: Revelar el foco
template: detail.hbs
ms.date: 03/01/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: medium
ms.openlocfilehash: 7bcceb8d44b6d92cab05a9c077531b3fe1b05c79
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651760"
---
# <a name="reveal-focus"></a>Revelar el foco

![imagen principal](images/header-reveal-focus.svg)

Mostrar el foco es un efecto de iluminación para [experiencias de 10 pies](/windows/uwp/design/devices/designing-for-tv), como Xbox One y pantallas de televisión. Anima el borde de los elementos activables, como los botones, cuando el usuario mueve el foco del teclado o del controlador para juegos a ellos. Está desactivado de manera predeterminada, pero es muy sencillo habilitarlo. 

(Para el efecto de revelar resaltar un efecto de iluminación que resalta elementos interactivos, consulte el [artículo revelar resaltar](/windows/uwp/design/style/reveal).)


> **API importantes**: [Propiedad Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.FocusVisualKind), [FocusVisualKind enum](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [Control.UseSystemFocusVisuals propiedad](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals)

## <a name="how-it-works"></a>Cómo funciona
Revelar centrar la atención de llamadas a los elementos con foco mediante la adición de un efecto de iluminado animado alrededor del borde del elemento:

![Elemento visual de Mostrar](images/traveling-focus-fullscreen-light-rf.gif)

Esto es especialmente útil en escenarios de 10 pies donde el usuario podría no estar pagando toda la atención a toda la pantalla de TV. 

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/RevealFocus">abra la aplicación y ver revelar el foco en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Cómo se usa

Revelar el que foco está desactivada de forma predeterminada. Para habilitarla:
1. En el constructor de tu aplicación, llama a la propiedad [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo.DeviceFamily) y comprueba si la familia de dispositivos actual es `Windows.Xbox`.
2. Si la familia de dispositivos es `Windows.Xbox`, establece la propiedad [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application.FocusVisualKind) en `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Después de establecer el **FocusVisualKind** propiedad, el sistema aplica automáticamente el efecto de foco revelar todos los controles cuya propiedad [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control.UseSystemFocusVisuals) propiedad está establecida en **True** (el valor predeterminado para la mayoría de los controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>¿Por qué no revelar el foco en forma predeterminada? 
Como puede ver, es bastante fácil activar revelar el foco cuando la aplicación detecta que se está ejecutando en una consola Xbox. Por lo tanto, ¿por qué no el sistema no lo activa por ti? Dado que revelan foco aumenta el tamaño de foco visual, lo que podría producir problemas con el diseño de interfaz de usuario. En algunos casos, desea personalizar el efecto de foco revelar para optimizar la aplicación.

## <a name="customizing-reveal-focus"></a>Personalización de revelar foco

Puede personalizar el efecto de foco revelar modificando las propiedades visuales de foco para cada control: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush), y [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush). Estas propiedades te permiten personalizar el color y el grosor del rectángulo del foco. (Son las mismas propiedades que usas para crear [Elementos visuales de foco de alta visibilidad](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Pero antes de empezar a personalizar, resulta útil saber un poco más acerca de los componentes que constituyen el foco revelar.

Hay tres partes a los objetos visuales de foco revelar de forma predeterminada: el borde principal, secundaria borde y la revelación brillante. El borde principal es de **2px** de espesor y se ejecuta alrededor de la parte de *fuera* del borde secundario. El borde secundario es de **1px** de espesor y se ejecuta alrededor de la parte de *dentro* del borde primario. Efecto de iluminado del foco revelar tiene un grosor proporcional al grosor del borde principal y se ejecuta en torno a la *fuera* del borde principal.

Además de los elementos estáticos, objetos visuales de foco revelar cuentan con una luz animada que pulsates en pausas y al mover el foco se mueve en la dirección de foco.

![Mostrar las capas de foco](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personalizar el grosor del borde

Para cambiar el grosor de los tipos de borde de un control, usa estas propiedades:

| Tipo de borde | Propiedad |
| --- | --- |
| Principal, Resplandor   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness)<br/> (Al cambiar el borde principal se cambia el grosor del resplandor de manera proporcional.)   |
| Secundario   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness)   |


Este ejemplo cambia el grosor del borde del elemento visual del foco de un botón:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personalizar el margen

El margen es el espacio entre los límites de los elementos visuales de control y el inicio del borde secundario de los elementos visuales de foco. El margen predeterminado está 1px por encima de los límites del control. Puedes editar este margen según el control, cambiando la propiedad [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualMargin):

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Un margen negativo aleja el borde del centro del control y un margen positivo acerca el borde hacia el centro del control.

## <a name="customize-the-color"></a>Personalizar el color

Para cambiar el color del objeto visual de foco revelar, utilice el [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) y [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush) propiedades.

| Propiedad | Recurso predeterminado | Valor del recurso predeterminado |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

(La propiedad FocusPrimaryBrush solo se establece en los recursos **SystemControlRevealFocusVisualBrush** de forma predeterminada cuando **FocusVisualKind** se establece en **Reveal**. De lo contrario, usa **SystemControlFocusVisualPrimaryBrush**.)


Para cambiar el color de elemento visual de foco de un control individual, establece las propiedades directamente en el control. Este ejemplo reemplaza los colores de los elementos visuales de foco de un botón.

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

Para cambiar el color de todos los elementos visuales de foco en toda la aplicación, invalida los recursos **SystemControlRevealFocusVisualBrush** y **SystemControlFocusVisualSecondaryBrush** con tu propia definición:

```xaml
<!-- App.xaml -->
<Application.Resources>

    <!-- Override Reveal Focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Para obtener más información sobre la modificación del elemento visual de foco, consulta [Personalización de marca de color y personalización](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Mostrar solo el resplandor

Si quieres usar solo el resplandor sin el foco visual principal o secundario, solo tienes que establecer la propiedad [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryBrush) del control en `Transparent` y [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualSecondaryThickness) en `0`. En este caso, el resplandor adoptará el color de fondo del control para proporcionar una apariencia sin bordes. Puedes modificar el grosor del resplandor con [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Usar tus propios elementos visuales de foco

Otra manera de personalizar revelar enfoque es dejar de participar en los objetos visuales de foco proporcionada por el sistema dibujando las suyas propias mediante estados visuales. Para obtener más información, consulta la [Muestra de elementos visuales de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Revelar el foco y el sistema Fluent Design

Revelar el que foco es un componente de sistema de diseño Fluent que agrega luz a la aplicación. Para obtener más información sobre el sistema de Fluent Design y sus otros componentes, consulta la [Introducción de Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artículos relacionados

- [Mostrar resaltado](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Diseño para Xbox y televisión](/windows/uwp/design/devices/designing-for-tv)
- [Interacciones de gamepad y control remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Ejemplo de elementos visuales de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Efectos de composición](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Ciencia del sistema: Diseño Fluent y profundidad](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Ciencia del sistema: Luz y diseño Fluent](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
