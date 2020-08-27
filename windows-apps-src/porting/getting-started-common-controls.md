---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
title: Tareas iniciales con controles habituales
description: Vea una lista de vínculos a temas sobre controles de Plataforma universal de Windows comunes (UWP) y sus controles de iOS equivalentes.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 86debeae4a4d8d3e3fb1a4084a3cf1ebef10f9d6
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943075"
---
# <a name="getting-started-common-controls"></a>Introducción: controles habituales


## <a name="common-controls-list"></a>Lista de controles habituales

En la sección anterior, trabajaste con tan solo dos controles: botones y bloques de texto. Por supuesto, hay muchos más controles que están a su disposición. Estos son algunos controles comunes que usarás en las aplicaciones y sus equivalentes de iOS. Los controles de iOS se enumeran en orden alfabético, junto con los controles de la Plataforma universal de Windows (UWP) más similares.

Los controles de UWP son inteligentes en el sentido de que pueden detectar el tipo de dispositivo en el que se ejecutan y cambiar su apariencia y funcionalidad según corresponda. Por ejemplo, si el proyecto usa el control [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)), funciona de manera suficientemente inteligente para optimizarse solo y adoptar un aspecto y comportamiento en un equipo de sobremesa y otros diferentes en un teléfono. No necesitas hacer nada: los controles se ajustan solos en tiempo de ejecución.

| Control de iOS (clase/protocolo) | Control de UWP equivalente |
|------------------------------|--------------------------------------|
| Indicador de actividad (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de banner de publicidad (**ADBannerView**) y delegado de vista de banner de publicidad (**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> Vea también [mostrar anuncios en la aplicación](../monetize/display-ads-in-your-app.md) |
| Botón (UIButton) | [Botón](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> Consulta también [Inicio rápido: agregar controles de botón](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| Selector de fecha (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| Vista de imagen (UIImageView) | [Imagen](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> Consulta también [Image e ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| Etiqueta (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> Consulta también [Inicio rápido: mostrar texto](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vista de mapa (MKMapView) y delegado de vista de mapa (MKMapViewDelegate) | Vea [mapas de Bing para aplicaciones para UWP](https://msdn.microsoft.com/library/hh846481) |
| Controlador de navegación (UINavigationController) y delegado de controlador de navegación (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Control de páginas (UIPageControl) | [Page](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Vista de selector (UIPickerView) y delegado de vista de selector (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> Consulta también [Agregar cuadros combinados y cuadros de lista](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| Barra de progreso (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de desplazamiento (UIScrollView) y delegado de vista de desplazamiento (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  Consulta también [Ejemplo de desplazamiento, movimiento panorámico y zoom con lenguaje XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208)) |
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
| Vista web (UIWebView) y delegado de vista web (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  Consulta también [Ejemplo de control WebView XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208)) |
| Ventana (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |

Para ver aún más controles, consulta [Lista de controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/).

**Nota:**    Para obtener una lista de controles para las aplicaciones para UWP con JavaScript y HTML, consulte la [lista de controles](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>Paso siguiente

[Introducción: navegación](getting-started-navigation.md)

## <a name="related-topics"></a>Temas relacionados

* [Build 2014: Información sobre los controles y la interfaz de usuario de XAML](https://channel9.msdn.com/Events/Build/2014/2-516)
* [Build 2014: Desarrollar aplicaciones con el marco de trabajo de la interfaz de usuario de XAML común](https://channel9.msdn.com/Events/Build/2014/2-507)
* [Build 2014: Usar Visual Studio para crear aplicaciones XAML convergidas](https://channel9.msdn.com/Events/Build/2014/3-591)
