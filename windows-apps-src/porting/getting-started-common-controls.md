---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: Tareas iniciales con controles habituales
title: Tareas iniciales con controles habituales
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a64dbd6a9530f81c55b0d4b52e4c0fd55c4b9956
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372837"
---
# <a name="getting-started-common-controls"></a>Introducción: Controles comunes


## <a name="common-controls-list"></a>Lista de controles habituales

En la sección anterior, trabajaste con tan solo dos controles: botones y bloques de texto. Por supuesto, hay muchos más controles que están disponibles para usted. Estos son algunos controles comunes que usarás en las aplicaciones y sus equivalentes de iOS. Los controles de iOS se enumeran en orden alfabético, junto con los controles de la Plataforma universal de Windows (UWP) más similares.

Los controles de UWP son inteligentes en el sentido de que pueden detectar el tipo de dispositivo en el que se ejecutan y cambiar su apariencia y funcionalidad según corresponda. Por ejemplo, si el proyecto usa el control [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)), funciona de manera suficientemente inteligente para optimizarse solo y adoptar un aspecto y comportamiento en un equipo de sobremesa y otros diferentes en un teléfono. No necesitas hacer nada: los controles se ajustan solos en tiempo de ejecución.

| Control de iOS (clase/protocolo) | Control equivalente de UWP |
|------------------------------|--------------------------------------|
| Indicador de actividad (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de banner de publicidad (**ADBannerView**) y delegado de vista de banner de publicidad (**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> Consulta también [Mostrar anuncios en tu aplicación](../monetize/display-ads-in-your-app.md) |
| Botón (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> Vea también [inicio rápido: Agregar controles de botón](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| Selector de fecha (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| Vista de imagen (UIImageView) | [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> Consulta también [Image e ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| Etiqueta (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> Consulta también [Inicio rápido: mostrar texto](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vista de mapa (MKMapView) y delegado de vista de mapa (MKMapViewDelegate) | Consulte [mapas de Bing para aplicaciones UWP](https://go.microsoft.com/fwlink/p/?LinkId=263496) |
| Controlador de navegación (UINavigationController) y delegado de controlador de navegación (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Control de páginas (UIPageControl) | [Página](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Vista de selector (UIPickerView) y delegado de vista de selector (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> Consulta también [Agregar cuadros combinados y cuadros de lista](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| Barra de progreso (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Vista de desplazamiento (UIScrollView) y delegado de vista de desplazamiento (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  Consulta también [Ejemplo de desplazamiento, movimiento panorámico y zoom con lenguaje XAML](https://go.microsoft.com/fwlink/p/?LinkId=238577) |
| Barra de búsqueda (UISearchBar) y delegado de barra de búsqueda (UISearchBarDelegate) | Consulta [Agregar búsqueda a una aplicación](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  Vea también [inicio rápido: Adición de búsqueda a una aplicación](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| Control segmentado (UISegmentedControl) | Ninguno |
| Control deslizante (UISlider) | [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  Consulta también [Cómo agregar un control deslizante](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| Controlador de vista dividida (UISplitViewController) y delegado de controlador de vista dividida (UISplitViewControllerDelegate) | Ninguno |
| Modificador (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  Consulta también [Cómo agregar un modificador de alternancia](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| Controlador de barra de pestañas (UITabBarController) y delegado de controlador de barra de pestañas (UITabBarControllerDelegate) | Ninguno |
| Controlador de vista de tabla (UITableViewController), vista de tabla (UITableView), delegado de vista de tabla (UITableViewDelegate) y celda de tabla (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  Consulta también [Inicio rápido: agregar controles ListView y GridView](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| Campo de texto (UITextField) y delegado de campo de texto (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  Consulta también [Mostrar y editar texto](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| Vista de texto (UITextView) y delegado de vista de texto (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  Consulta también [Inicio rápido: mostrar texto](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Vista (UIView) y controlador de vista (UIViewController) | [Página](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |
| Vista web (UIWebView) y delegado de vista web (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  Consulta también [Ejemplo de control WebView XAML](https://go.microsoft.com/fwlink/p/?LinkId=238582) |
| Ventana (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  Consulta también [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics). |

Para ver aún más controles, consulta [Lista de controles](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/).

**Tenga en cuenta**  para obtener una lista de controles para aplicaciones UWP con JavaScript y HTML, vea [lista controles](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>Paso siguiente

[Introducción: Navegación](getting-started-navigation.md)

## <a name="related-topics"></a>Temas relacionados

* [compilación de 2014: ¿Qué sucede con la interfaz de usuario de XAML y controles?](https://go.microsoft.com/fwlink/p/?LinkID=397897)
* [compilación de 2014: Desarrollo de aplicaciones mediante el marco de interfaz de usuario de XAML comunes](https://go.microsoft.com/fwlink/p/?LinkID=397898)
* [compilación de 2014: Con Visual Studio para compilar el XAML de las aplicaciones convergentes](https://go.microsoft.com/fwlink/p/?LinkID=397876)
