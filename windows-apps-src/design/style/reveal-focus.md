---
author: cphilippona
description: Reveal focus es un efecto de iluminación que anima el borde de los elementos activables cuando el usuario mueve el foco del teclado o del controlador para juegos a ellos.
title: Reveal focus
template: detail.hbs
ms.author: mijacobs
ms.date: 03/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: chphilip
design-contact: ''
dev-contact: stevenki
ms.localizationpriority: high
ms.openlocfilehash: 0a5f3dca3c8310bddbcd63c814d2d883151ff1f3
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2018
---
# <a name="reveal-focus"></a>Reveal focus

Reveal focus es un efecto de iluminación para [experiencias de 3 metros](/windows/uwp/design/devices/designing-for-tv), como las pantallas de televisión y la Xbox One. Anima el borde de los elementos activables, como los botones, cuando el usuario mueve el foco del teclado o del controlador para juegos a ellos. Está desactivado de manera predeterminada, pero es muy sencillo habilitarlo. 

(Para el efecto Mostrar resaltado, un efecto de iluminación que resalta elementos interactivos, consulta el [artículo de Mostrar resaltado](/windows/uwp/design/style/reveal).)


> **API importantes**: [propiedad Application.FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_FocusVisualKind), [enumeración FocusVisualKind](https://docs.microsoft.com/uwp/api/windows.ui.xaml.focusvisualkind), [propiedad Control.UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_UseSystemFocusVisuals)

## <a name="how-it-works"></a>Funcionamiento
Reveal focus llama la atención de los elementos enfocados agregando un resplandor animado alrededor del borde del elemento:

![Elemento visual de Mostrar](images/traveling-focus-fullscreen-light-rf.gif)

Esto es especialmente útil en escenarios de 3 metros, donde el usuario podría no estar prestando toda su atención a la pantalla de televisión completa. 

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RevealFocus">abrir la aplicación y ver Reveal focus en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-to-use-it"></a>Cómo se usa

La opción Reveal focus está desactivada de manera predeterminada. Para habilitarla:
1. En el constructor de tu aplicación, llama a la propiedad [AnalyticsInfo.VersionInfo.DeviceFamily](/uwp/api/windows.system.profile.analyticsversioninfo#Windows_System_Profile_AnalyticsVersionInfo_DeviceFamily) y comprueba si la familia de dispositivos actual es `Windows.Xbox`.
2. Si la familia de dispositivos es `Windows.Xbox`, establece la propiedad [Application.FocusVisualKind](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_FocusVisualKind) en `FocusVisualKind.Reveal`. 

```csharp
    if(AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
    {
        this.FocusVisualKind = FocusVisualKind.Reveal;
    }
```

Después de establecer la propiedad **FocusVisualKind**, el sistema aplica automáticamente el efecto de foco para mostrar en todos los controles cuya propiedad [UseSystemFocusVisuals](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_UseSystemFocusVisuals) se establece en **True** (el valor predeterminado para la mayoría de los controles). 

## <a name="why-isnt-reveal-focus-on-by-default"></a>¿Por qué no se encuentra Reveal focus de manera predeterminada? 
Como puedes ver, es bastante sencillo activar Reveal focus cuando la aplicación detecta que se está ejecutando en una Xbox. Por lo tanto, ¿por qué no el sistema no lo activa por ti? Porque Reveal focus aumenta el tamaño del elemento visual de foco, que podría causar problemas con tu diseño de la interfaz de usuario. En algunos casos, querrás personalizar el efecto Reveal focus para optimizarlo para tu aplicación.

## <a name="customizing-reveal-focus"></a>Personalización de Reveal focus

Puedes personalizar el efecto de Reveal focus modificando las propiedades visuales de foco para cada control: [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryThickness), [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryThickness), [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) y [FocusVisualSecondaryBrush ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryBrush). Estas propiedades te permiten personalizar el color y el grosor del rectángulo del foco. (Son las mismas propiedades que usas para crear [Elementos visuales de foco de alta visibilidad](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#high-visibility-focus-visuals).) 

Pero antes de empezar la personalización, es útil saber un poco más acerca de los componentes que componen Reveal focus.

Hay tres partes en los elementos visuales de enfoque de Reveal predeterminados: el borde principal, el borde secundario y el resplandor de Reveal. El borde principal es de **2px** de espesor y se ejecuta alrededor de la parte de *fuera* del borde secundario. El borde secundario es de **1px** de espesor y se ejecuta alrededor de la parte de *dentro* del borde primario. El resplandor de Reveal focus tiene un grosor proporcional al grosor del borde principal y se ejecuta alrededor de la parte de *fuera* del borde principal.

Además de los elementos estáticos, los elementos visuales de Reveal focus presentan una luz animada que late cuando está en reposo y que se mueve en la dirección del foco al moverlo.

![Capas de foco Reveal](images/reveal-breakdown.svg)

## <a name="customize-the-border-thickness"></a>Personalizar el grosor del borde

Para cambiar el grosor de los tipos de borde de un control, usa estas propiedades:

| Tipo de borde | Propiedad |
| --- | --- |
| Principal, Resplandor   | [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryThickness)<br/> (Al cambiar el borde principal se cambia el grosor del resplandor de manera proporcional.)   |
| Secundario   | [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryThickness)   |


Este ejemplo cambia el grosor del borde del elemento visual del foco de un botón:

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1"/>
```

## <a name="customize-the-margin"></a>Personalizar el margen

El margen es el espacio entre los límites de los elementos visuales de control y el inicio del borde secundario de los elementos visuales de foco. El margen predeterminado está 1px por encima de los límites del control. Puedes editar este margen según el control, cambiando la propiedad [FocusVisualMargin](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualMargin):

```xaml
<Button FocusVisualPrimaryThickness="2" FocusVisualSecondaryThickness="1" FocusVisualMargin="-3"/>
```

Un margen negativo aleja el borde del centro del control y un margen positivo acerca el borde hacia el centro del control.

## <a name="customize-the-color"></a>Personalizar el color

Para cambiar el color del elemento visual del foco Reveal, usa las propiedades [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) y [FocusVisualSecondaryBrush](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryBrush).

| Propiedad | Recurso predeterminado | Valor del recurso predeterminado |
| ---- | ---- | --- | 
| [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) | SystemControlRevealFocusVisualBrush  | SystemAccentColor |
| [FocusVisualSecondaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryBrush)  | SystemControlFocusVisualSecondaryBrush  | SystemAltMediumColor |

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

    <!-- Override Reveal focus default resources. -->
    <SolidColorBrush x:Key="SystemControlRevealFocusVisualBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    <SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="{ThemeResource SystemAccentColor}"/>
</Application.Resources>
```

Para obtener más información sobre la modificación del elemento visual de foco, consulta [Personalización de marca de color y personalización](https://docs.microsoft.com/windows/uwp/design/input/guidelines-for-visualfeedback#color-branding--customizing).


## <a name="show-just-the-glow"></a>Mostrar solo el resplandor

Si quieres usar solo el resplandor sin el foco visual principal o secundario, solo tienes que establecer la propiedad [FocusVisualPrimaryBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryBrush) del control en `Transparent` y [FocusVisualSecondaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualSecondaryThickness) en `0`. En este caso, el resplandor adoptará el color de fondo del control para proporcionar una apariencia sin bordes. Puedes modificar el grosor del resplandor con [FocusVisualPrimaryThickness](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement#Windows_UI_Xaml_FrameworkElement_FocusVisualPrimaryThickness).

```xaml

<!-- Show just the glow -->
<Button
    FocusVisualPrimaryBrush="Transparent"
    FocusVisualSecondaryThickness="0" />    
```


## <a name="use-your-own-focus-visuals"></a>Usar tus propios elementos visuales de foco

Otra manera de personalizar Reveal focus es descartar los focos visuales proporcionados por el sistema. Para ello, dibuja los tuyos propios mediante los estados visuales. Para obtener más información, consulta la [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895).


## <a name="reveal-focus-and-the-fluent-design-system"></a>Reveal focus y Fluent Design System

Reveal focus es un componente de Fluent Design System que agrega luz a tu aplicación. Para obtener más información sobre el sistema de Fluent Design y sus otros componentes, consulta la [Introducción de Fluent Design para UWP](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artículos relacionados

- [Mostrar resaltado](https://docs.microsoft.com/windows/uwp/design/style/reveal)
- [Diseño para Xbox y televisión](/windows/uwp/design/devices/designing-for-tv)
- [Interacciones con el controlador para juegos y el control remoto](https://docs.microsoft.com/windows/uwp/design/input/gamepad-and-remote-interactions)
- [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
- [Efectos de composición](https://msdn.microsoft.com/windows/uwp/graphics/composition-effects)
- [Science in the System: Fluent Design and Depth (Ciencia en el sistema: Fluent Design y profundidad)](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
- [Science in the System: Fluent Design and Depth (Ciencia en el sistema: Fluent Design y luz)](https://medium.com/microsoft-design/the-science-in-the-system-fluent-design-and-light-94a17e0b3a4f)
