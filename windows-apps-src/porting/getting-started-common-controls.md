---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
title: Tareas iniciales con controles habituales
description: Vea una lista de vínculos a temas sobre controles de Plataforma universal de Windows comunes (UWP) y sus controles de iOS equivalentes.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50678f0bd5eb3cb48c6bcd915fd88bdfb460118b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174879"
---
# <a name="getting-started-common-controls"></a>Introducción: controles habituales


## <a name="common-controls-list"></a>Lista de controles habituales

En la sección anterior, trabajaste con tan solo dos controles: botones y bloques de texto. Por supuesto, hay muchos más controles que están a su disposición. Estos son algunos controles comunes que usarás en las aplicaciones y sus equivalentes de iOS. Los controles de iOS se enumeran en orden alfabético, junto con los controles de la Plataforma universal de Windows (UWP) más similares.

Los controles de UWP son inteligentes en el sentido de que pueden detectar el tipo de dispositivo en el que se ejecutan y cambiar su apariencia y funcionalidad según corresponda. Por ejemplo, si el proyecto usa el control [**DatePicker**](/previous-versions/windows/apps/br211681(v=win.10)), funciona de manera suficientemente inteligente para optimizarse solo y adoptar un aspecto y comportamiento en un equipo de sobremesa y otros diferentes en un teléfono. No necesitas hacer nada: los controles se ajustan solos en tiempo de ejecución.

| Control de iOS (clase/protocolo) | Control de UWP equivalente |
|------------------------------|--------------------------------------|
| Indicador de actividad (**UIActivityIndicatorView**) | [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> Consulta también [Inicio rápido: agregar controles de progreso](/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de banner de publicidad (**ADBannerView**) y delegado de vista de banner de publicidad (**ADBannerViewDelegate**) | [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> Vea también [mostrar anuncios en la aplicación](../monetize/display-ads-in-your-app.md) |
| Botón (UIButton) | [Botón](/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> Consulta también [Inicio rápido: agregar controles de botón](/previous-versions/windows/apps/jj153346(v=win.10)) |
| Selector de fecha (UIDatePicker) | [DatePicker](/previous-versions/windows/apps/br211681(v=win.10)) |
| Vista de imagen (UIImageView) | [Imagen](/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> Consulta también [Image e ImageBrush](../design/controls-and-patterns/images-imagebrushes.md) |
| Etiqueta (UILabel) | [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> Consulta también [Inicio rápido: mostrar texto](/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vista de mapa (MKMapView) y delegado de vista de mapa (MKMapViewDelegate) | Vea [mapas de Bing para aplicaciones para UWP](/previous-versions/windows/apps/dn642089(v=win.10)) |
| Controlador de navegación (UINavigationController) y delegado de controlador de navegación (UINavigationControllerDelegate) | [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> Consulta también [Navegación](../design/basics/navigation-basics.md). |
| Control de páginas (UIPageControl) | [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> Consulta también [Navegación](../design/basics/navigation-basics.md). |
| Vista de selector (UIPickerView) y delegado de vista de selector (UIPickerViewDelegate) | [ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> Consulta también [Agregar cuadros combinados y cuadros de lista](/previous-versions/windows/apps/hh780616(v=win.10)) |
| Barra de progreso (UIProgressView) | [ProgressBar](/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> Consulta también [Inicio rápido: agregar controles de progreso](/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de desplazamiento (UIScrollView) y delegado de vista de desplazamiento (UIScrollViewDelegate) | [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  Consulta también [Ejemplo de desplazamiento, movimiento panorámico y zoom con lenguaje XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample%20(Windows%208)) |
| Barra de búsqueda (UISearchBar) y delegado de barra de búsqueda (UISearchBarDelegate) | Consulta [Agregar búsqueda a una aplicación](/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  Consulta también [Inicio rápido: agregar búsqueda a una aplicación](/previous-versions/windows/apps/hh868180(v=win.10)) |
| Control segmentado (UISegmentedControl) | None |
| Control deslizante (UISlider) | [Control deslizante](/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  Consulta también [Cómo agregar un control deslizante](/previous-versions/windows/apps/hh868197(v=win.10)) |
| Controlador de vista dividida (UISplitViewController) y delegado de controlador de vista dividida (UISplitViewControllerDelegate) | None |
| Modificador (UISwitch) | [ToggleSwitch](/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  Consulta también [Cómo agregar un modificador de alternancia](/previous-versions/windows/apps/hh868198(v=win.10)) |
| Controlador de barra de pestañas (UITabBarController) y delegado de controlador de barra de pestañas (UITabBarControllerDelegate) | None |
| Controlador de vista de tabla (UITableViewController), vista de tabla (UITableView), delegado de vista de tabla (UITableViewDelegate) y celda de tabla (UITableViewCell) | [ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  Consulta también [Inicio rápido: agregar controles ListView y GridView](/previous-versions/windows/apps/hh780650(v=win.10)) |
| Campo de texto (UITextField) y delegado de campo de texto (UITextFieldDelegate) | [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  Consulta también [Mostrar y editar texto](../design/controls-and-patterns/text-controls.md) |
| Vista de texto (UITextView) y delegado de vista de texto (UITextViewDelegate) | [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  Consulta también [Inicio rápido: mostrar texto](/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vista (UIView) y controlador de vista (UIViewController) | [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  Consulta también [Navegación](../design/basics/navigation-basics.md). |
| Vista web (UIWebView) y delegado de vista web (UIWebViewDelegate) | [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  Consulta también [Ejemplo de control WebView XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20WebView%20control%20sample%20(Windows%208)) |
| Ventana (UIWindow) | [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  Consulta también [Navegación](../design/basics/navigation-basics.md). |

Para ver aún más controles, consulta [Lista de controles](../design/controls-and-patterns/index.md).

**Nota:**    Para obtener una lista de controles para las aplicaciones para UWP con JavaScript y HTML, consulte la [lista de controles](/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>Paso siguiente

[Introducción: navegación](getting-started-navigation.md)

## <a name="related-topics"></a>Temas relacionados

* [Build 2014: Información sobre los controles y la interfaz de usuario de XAML](https://channel9.msdn.com/Events/Build/2014/2-516)
* [Build 2014: Desarrollar aplicaciones con el marco de trabajo de la interfaz de usuario de XAML común](https://channel9.msdn.com/Events/Build/2014/2-507)
* [Build 2014: Usar Visual Studio para crear aplicaciones XAML convergidas](https://channel9.msdn.com/Events/Build/2014/3-591)