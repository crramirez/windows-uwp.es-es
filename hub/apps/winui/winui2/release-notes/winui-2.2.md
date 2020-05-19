---
title: Notas de la versión de WinUI 2.2
description: Notas de la versión de WinUI 2.2, incluidas las nuevas características y correcciones de errores.
ms.date: 04/15/2020
ms.topic: article
ms.openlocfilehash: 2272046bc59865ebcf7958a165805d9ca4c7efce
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580472"
---
# <a name="windows-ui-library-22"></a>Biblioteca de interfaz de usuario de Windows 2.2

WinUI 2.2 es la última versión oficial de la biblioteca de interfaz de usuario de Windows.

Puede agregar paquetes de WinUI a la aplicación mediante el administrador de paquetes NuGet: consulte [Introducción a la biblioteca de interfaz de usuario de Windows](../getting-started.md) para obtener más información.

WinUI es un proyecto de código abierto que se hospeda en GitHub. Agradecemos los informes de errores, las solicitudes de características y las contribuciones de código de la comunidad en el [repositorio de la biblioteca de interfaz de usuario de Windows](https://aka.ms/winui).

## <a name="microsoftuixaml-22-version-history"></a>Historial de versiones de Microsoft.UI.Xaml 2.2

### <a name="windows-ui-library-22-official-release"></a>Versión oficial de la biblioteca de interfaz de usuario de Windows 2.2

Agosto de 2019

[Página de versiones de GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases)

[Descarga de paquetes NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>Nuevas características

#### <a name="tabview"></a>TabView

![Ejemplo](../images/tabview-gif.gif)

#### <a name="description"></a>Descripción

El control TabView es una colección de pestañas, cada una de las cuales representa una página o un documento nuevo en la aplicación. TabView es útil cuando la aplicación tiene varias páginas de contenido y el usuario espera poder agregar, cerrar y reorganizar las pestañas. El nuevo [Terminal Windows](https://github.com/Microsoft/Terminal) usa TabView para mostrar varias interfaces de línea de comandos.

#### <a name="documentation"></a>Documentación

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>Actualizaciones de NavigationView

##### <a name="a-navigationviews-back-button-update"></a>a) Actualización del botón Atrás de NavigationView

![Ejemplo](../images/navigationview-back-button.gif)

##### <a name="description"></a>Descripción

En el modo mínimo de NavigationView, el botón atrás ya no desaparece. Al abrir y cerrar el panel, los usuarios ya no tienen que mover el cursor para hacer clic en el botón de hamburguesa. Esta característica funcionará de forma predeterminada. No es necesario realizar ningún cambio en el código para que funcione.

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView: sin espaciado automático

![Ejemplo](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>Descripción

Los desarrolladores de aplicaciones ahora pueden reclamar todos los píxeles de la ventana de la aplicación cuando usan el control NavigationView y se extienden al área de la barra de título.

##### <a name="documentation"></a>Documentación

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>Actualizaciones de estilos visuales

##### <a name="a-corner-radius-update"></a>a) Actualización del radio de redondeo

![Ejemplo](../images/corner-radius.png)

##### <a name="description"></a>Descripción

Se ha agregado el atributo CornerRadius. Los controles predeterminados se actualizaron para usar esquinas ligeramente redondeadas. Los desarrolladores pueden personalizar fácilmente el radio de redondeo para dar a la aplicación un aspecto único si lo desean.

##### <a name="github-spec-link"></a>Vínculo de especificación de GitHub

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) Actualización del grosor del borde

![Ejemplo](../images/border-thickness.png)

##### <a name="description"></a>Descripción

Se facilitó la personalización de la propiedad BorderThickness. Los controles predeterminados se actualizaron para afinar los contornos a fin de ofrecer un aspecto más limpio y familiar.

##### <a name="github-spec-link"></a>Vínculo de especificación de GitHub

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) Actualización del elemento visual Button

![Ejemplo](../images/button-hover-visual-update.png)

##### <a name="description"></a>Descripción: 
El elemento visual Button predeterminado se actualizó para quitar el contorno que aparecía al pasar el cursor para darle un aspecto más claro.

##### <a name="github-spec-link"></a>Vínculo de especificación de GitHub:  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>c) Actualización visual de SplitButton

![Ejemplo](../images/splitbutton-visual-update.png)

##### <a name="description"></a>Descripción: 
El elemento visual SplitButton predeterminado se actualizó para diferenciarlo de DropDownButton.

##### <a name="github-spec-link"></a>Vínculo de especificación de GitHub: 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) Actualización del elemento visual ToggleSwitch

![Ejemplo](../images/toggleswitch-update.png)

##### <a name="description"></a>Descripción: 
El ancho de ToggleSwitch predeterminado se redujo de 44px a 40px, de modo que está equilibrado visualmente al tiempo que conserva la facilidad de uso.

##### <a name="github-spec-link"></a>Vínculo de especificación de GitHub: 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) Actualización visual de los elementos CheckBox y RadioButton

![Ejemplo](../images/checkbox-radiobutton.png)

##### <a name="description"></a>Descripción: 
Los elementos visuales CheckBox y RadioButton se actualizaron para ser coherentes con los demás cambio de estilos visuales.

##### <a name="github-spec-link"></a>Vínculo de especificación de GitHub: 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>Ejemplos

La aplicación de ejemplo XAML Controls Gallery incluye demostraciones interactivas y código de ejemplo para usar controles WinUI.

* Instalación de la aplicación XAML Controls Gallery desde [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* La aplicación XAML Controls Gallery también está disponible en [código abierto en GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentación

Se incluyen artículos sobre procedimientos para los controles de la biblioteca de interfaz de usuario de Windows con la [documentación sobre controles de la Plataforma universal de Windows](/windows/uwp/design/controls-and-patterns/).

Los documentos de referencia de API se encuentran aquí: [API de la biblioteca de interfaz de usuario de Windows](/uwp/api/overview/winui/).

## <a name="microsoftuixaml-22-version-history"></a>Historial de versiones de Microsoft.UI.Xaml 2.2

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-prerelease

Julio de 2019

[Página de versiones de GitHub](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[Descarga de paquetes NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>Característica experimental

* [TabView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-prerelease

Abril de 2019

[Página de versiones de GitHub](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[Descarga de paquetes NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>Características experimentales

* [FlowLayout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.scrollviewer)
