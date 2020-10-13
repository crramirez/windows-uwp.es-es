---
title: Notas de la versión de WinUI 2.4
description: Notas de la versión de WinUI 2.4, incluidas las nuevas características y correcciones de errores.
ms.date: 07/15/2020
ms.topic: reference
ms.openlocfilehash: 5e2ff23b3b0ea63002ad54a367e82e81ee1cd542
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762917"
---
# <a name="windows-ui-library-24"></a>Biblioteca de interfaz de usuario de Windows 2.4

WinUI 2.4 es la última versión oficial de la biblioteca de interfaz de usuario de Windows (WinUI).

WinUI es un proyecto de código abierto que se hospeda en GitHub en el [repositorio de la biblioteca de interfaz de usuario de Windows](https://aka.ms/winui). Registre todos los informes de errores, las solicitudes de características y las contribuciones de código de la comunidad en este repositorio.

Versiones de WinUI: [Página de versiones de GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

Los paquetes de WinUI se pueden agregar a los proyectos de Visual Studio a través del administrador de paquetes NuGet. Para obtener más información, consulte [Introducción a la biblioteca de interfaz de usuario de Windows](../getting-started.md).

Descarga de paquetes NuGet: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Nuevas características

### <a name="radialgradientbrush"></a>RadialGradientBrush

Se dibuja un objeto RadialGradientBrush en una elipse definida por las propiedades Center, RadiusX y RadiusY. Los colores del degradado se inician en el centro de la elipse y terminan en el radio.

![Breve vídeo que muestra el comportamiento de RadialGradientBrush.](../images/radialgradientbrush.gif)<br>
*Pincel de degradado radial*

[Instrucciones de uso](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[Referencia de las API](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

El control ProgressRing se usa para las interacciones modales, de modo que el usuario se bloquea hasta que desaparece el control ProgressRing. Usa este control si una operación requiere que la mayor parte de la interacción con la aplicación se suspenda hasta que se complete la operación.

![Breve vídeo que muestra el comportamiento del control ProgressRing.](../images/progressring.gif)<br>
*Control ProgressRing*

[Instrucciones de uso](/windows/uwp/design/controls-and-patterns/progress-controls)

[Referencia de las API](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>Actualizaciones de TabView

Las actualizaciones del control TabView te proporcionan más control sobre cómo representar las pestañas.

Puedes establecer el ancho de las pestañas no seleccionadas y mostrar solo un icono para ahorrar espacio en la pantalla:

![Tamaños de las pestañas del control TabView](..\images\tabview-sizing.gif)<br>
*Tamaños de las pestañas del control TabView*

También puedes ocultar el botón Cerrar de las pestañas no seleccionadas hasta que el usuario mantenga el puntero encima de estas (en las versiones anteriores se mostraba siempre):

![Mantenimiento del puntero para cerrar pestañas del control TabView](..\images\tabview-closebuttononhover.gif)<br>
*Mantenimiento del puntero para cerrar pestañas del control TabView*

[Instrucciones de uso](/windows/uwp/design/controls-and-patterns/tab-view)

[Referencia de las API](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>Actualizaciones del tema oscuro en la familia de controles TextBox

Cuando el tema oscuro está habilitado, el color de fondo de los controles de la familia TextBox permanece oscuro de forma predeterminada en la inserción de texto (en versiones anteriores, el color de fondo cambiaba a blanco durante la inserción del texto).

| Antes | Después |
| - | - |
| ![Breve vídeo que muestra el comportamiento del tema oscuro de TextBox antes de las actualizaciones.](..\images\textbox-darkthemeupdates-before1.gif)<br>*Actualizaciones del tema oscuro de TextBox (antes)* | ![Breve vídeo que muestra el comportamiento del tema oscuro de TextBox después de las actualizaciones.](..\images\textbox-darkthemeupdates-after1.gif)<br>*Actualizaciones del tema oscuro de TextBox (después)* |
| ![Otro breve vídeo que muestra el comportamiento del tema oscuro de TextBox antes de las actualizaciones.](..\images\textbox-darkthemeupdates-before2.gif)<br>*Actualizaciones del tema oscuro de TextBox (antes)* | ![Otro breve vídeo que muestra el comportamiento del tema oscuro de TextBox después de las actualizaciones.](..\images\textbox-darkthemeupdates-after2.gif)<br>*Actualizaciones del tema oscuro de TextBox (después)* |

A continuación, se muestran algunos de los controles incluidos en la familia de controles TextBox:

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [Cuadro combinado editable](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>Navegación jerárquica

El control [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4) ahora admite la navegación jerárquica e incluye los modos de presentación Left, Top y LeftCompact. El control NavigationView jerárquico resulta útil para mostrar categorías de páginas, identificar páginas con páginas secundarias relacionadas o usarse en aquellas aplicaciones que tienen páginas de estilo de concentrador que se vinculan a muchas otras páginas.

![Control NavigationView jerárquico](..\images\HierarchicalNavView.gif)<br>*Control NavigationView jerárquico*

[Instrucciones de uso](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[Referencia de las API](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>Ejemplos

Los ejemplos de cada una de las características de WinUI 2.4 descritas están disponibles en **XAML Controls Gallery**.

Si no tienes instalada la aplicación XAML Controls Gallery, puedes obtenerla en [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

También puedes ver el código fuente de XAML Controls Gallery en [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery).
