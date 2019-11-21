---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: Tareas iniciales con controles habituales
title: Tareas iniciales con controles habituales
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bc45f21acf5b9a485cf6bd5ead18482f1920bf99
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260149"
---
# <a name="getting-started-common-controls"></a>Introducción: controles habituales


## <a name="common-controls-list"></a>Lista de controles habituales

En la sección anterior, trabajaste con tan solo dos controles: botones y bloques de texto. Por supuesto, hay muchos más controles que están a su disposición. Estos son algunos controles comunes que usarás en las aplicaciones y sus equivalentes de iOS. Los controles de iOS se enumeran en orden alfabético, junto con los controles de la Plataforma universal de Windows (UWP) más similares.

Los controles de UWP son inteligentes en el sentido de que pueden detectar el tipo de dispositivo en el que se ejecutan y cambiar su apariencia y funcionalidad según corresponda. Por ejemplo, si el proyecto usa el control [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)), funciona de manera suficientemente inteligente para optimizarse solo y adoptar un aspecto y comportamiento en un equipo de sobremesa y otros diferentes en un teléfono. No necesitas hacer nada: los controles se ajustan solos en tiempo de ejecución.

| Control de iOS (clase/protocolo) | Control de UWP equivalente |
|------------------------------|--------------------------------------|
| Indicador de actividad (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de banner de publicidad (**ADBannerView**) y delegado de vista de banner de publicidad (**ADBannerViewDelegate**) | [Control](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> Consulta también [Mostrar anuncios en tu aplicación](../monetize/display-ads-in-your-app.md) |
| Botón (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> Consulta también [Inicio rápido: agregar controles de botón](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| Selector de fecha (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| Vista de imagen (UIImageView) | [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> Consulta también [Image e ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| Etiqueta (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> Consulta también [Inicio rápido: mostrar texto](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vista de mapa (MKMapView) y delegado de vista de mapa (MKMapViewDelegate) | Vea [mapas de Bing para aplicaciones para UWP](https://msdn.microsoft.com/library/hh846481) |
| Controlador de navegación (UINavigationController) y delegado de controlador de navegación (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Control de páginas (UIPageControl) | [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Vista de selector (UIPickerView) y delegado de vista de selector (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> Consulta también [Agregar cuadros combinados y cuadros de lista](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| Barra de progreso (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de desplazamiento (UIScrollView) y delegado de vista de desplazamiento (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  Consulta también [Ejemplo de desplazamiento, movimiento panorámico y zoom con lenguaje XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9) |
| Barra de búsqueda (UISearchBar) y delegado de barra de búsqueda (UISearchBarDelegate) | Consulta [Agregar búsqueda a una aplicación](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  Consulta también [Inicio rápido: agregar búsqueda a una aplicación](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| Control segmentado (UISegmentedControl) | Ninguno |
| Control deslizante (UISlider) | [Control deslizante](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  Consulta también [Cómo agregar un control deslizante](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| Controlador de vista dividida (UISplitViewController) y delegado de controlador de vista dividida (UISplitViewControllerDelegate) | Ninguno |
| Modificador (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  Consulta también [Cómo agregar un modificador de alternancia](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| Controlador de barra de pestañas (UITabBarController) y delegado de controlador de barra de pestañas (UITabBarControllerDelegate) | Ninguno |
| Controlador de vista de tabla (UITableViewController), vista de tabla (UITableView), delegado de vista de tabla (UITableViewDelegate) y celda de tabla (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  Consulta también [Inicio rápido: agregar controles ListView y GridView](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| Campo de texto (UITextField) y delegado de campo de texto (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  Consulta también [Mostrar y editar texto](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| Vista de texto (UITextView) y delegado de vista de texto (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  Consulta también [Inicio rápido: mostrar texto](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vista (UIView) y controlador de vista (UIViewController) | [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Vista web (UIWebView) y delegado de vista web (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  Consulta también [Ejemplo de control WebView XAML](https://code.msdn.microsoft.com/windowsapps/XAML-WebView-control-sample-58ad63f7) |
| Ventana (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |

Para ver aún más controles, consulta [Lista de controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/).

**Tenga en cuenta**  para obtener una lista de controles para aplicaciones UWP que usan JavaScript y HTML, consulte la [lista de controles](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>Paso siguiente

[Introducción: navegación](getting-started-navigation.md)

## <a name="related-topics"></a>Temas relacionados

* [compilación 2014: ¿Qué sucede con la interfaz de usuario y los controles XAML?](https://channel9.msdn.com/Events/Build/2014/2-516)
* [compilación 2014: desarrollo de aplicaciones con el marco de interfaz de usuario XAML común](https://channel9.msdn.com/Events/Build/2014/2-507)
* [compilación 2014: uso de Visual Studio para compilar aplicaciones convergidas en XAML](https://channel9.msdn.com/Events/Build/2014/3-591)
