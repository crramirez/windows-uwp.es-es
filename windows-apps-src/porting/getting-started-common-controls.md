---
author: stevewhims
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: Tareas iniciales con controles habituales
title: Introducción a los controles habituales
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5b02bd99fdb93fdaaba5dce8f0bb6d25bb190188
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "239266"
---
# <a name="getting-started-common-controls"></a>Introducción: controles habituales


## <a name="common-controls-list"></a>Lista de controles habituales

En la sección anterior, trabajaste con tan solo dos controles: botones y bloques de texto. Por supuesto, hay muchos más controles disponibles. Estos son algunos controles comunes que usarás en las aplicaciones y sus equivalentes de iOS. Los controles de iOS se enumeran en orden alfabético, junto con los controles de la Plataforma universal de Windows (UWP) más similares.

Los controles de UWP son inteligentes en el sentido de que pueden detectar el tipo de dispositivo en el que se ejecutan y cambiar su apariencia y funcionalidad según corresponda. Por ejemplo, si el proyecto usa el control [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681), funciona de manera suficientemente inteligente para optimizarse solo y adoptar un aspecto y comportamiento en un equipo de sobremesa y otros diferentes en un teléfono. No necesitas hacer nada: los controles se ajustan solos en tiempo de ejecución.

| Control de iOS (clase/protocolo) | Control UWP equivalente |
|------------------------------|--------------------------------------|
| Indicador de actividad (**UIActivityIndicatorView**) | [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| Vista de banner de publicidad (**ADBannerView**) y delegado de vista de banner de publicidad (**ADBannerViewDelegate**) | [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) <br/> Consulta también [Mostrar anuncios en tu aplicación](../monetize/display-ads-in-your-app.md) |
| Botón (UIButton) | [Botón](https://msdn.microsoft.com/library/windows/apps/br209265) <br/> Consulta también [Inicio rápido: agregar controles de botón](https://msdn.microsoft.com/library/windows/apps/xaml/jj153346) |
| Selector de fecha (UIDatePicker) | [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681) |
| Vista de imagen (UIImageView) | [Imagen](https://msdn.microsoft.com/library/windows/apps/br242752) <br/> Consulta también [Image e ImageBrush](https://msdn.microsoft.com/library/windows/apps/mt280382) |
| Etiqueta (UILabel) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/> Consulta también [Inicio rápido: mostrar texto](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| Vista de mapa (MKMapView) y delegado de vista de mapa (MKMapViewDelegate) | Vea [mapas de Bing para aplicaciones UWP](http://go.microsoft.com/fwlink/p/?LinkId=263496) |
| Controlador de navegación (UINavigationController) y delegado de controlador de navegación (UINavigationControllerDelegate) | [Frame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/> Consulta también [Navegación](https://msdn.microsoft.com/library/windows/apps/mt187344). |
| Control de páginas (UIPageControl) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/> Consulta también [Navegación](https://msdn.microsoft.com/library/windows/apps/mt187344). |
| Vista de selector (UIPickerView) y delegado de vista de selector (UIPickerViewDelegate) | [ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348) <br/> Consulta también [Agregar cuadros combinados y cuadros de lista](https://msdn.microsoft.com/library/windows/apps/xaml/hh780616) |
| Barra de progreso (UIProgressView) | [ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529) <br/> Consulta también [Inicio rápido: agregar controles de progreso](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| Vista de desplazamiento (UIScrollView) y delegado de vista de desplazamiento (UIScrollViewDelegate) | [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527) <br/>  Consulta también [Ejemplo de desplazamiento, movimiento panorámico y zoom con lenguaje XAML](http://go.microsoft.com/fwlink/p/?LinkId=238577) |
| Barra de búsqueda (UISearchBar) y delegado de barra de búsqueda (UISearchBarDelegate) | Consulta [Agregar búsqueda a una aplicación](https://msdn.microsoft.com/library/windows/apps/xaml/jj130767) <br/>  Consulta también [Inicio rápido: agregar búsqueda a una aplicación](https://msdn.microsoft.com/library/windows/apps/xaml/hh868180) |
| Control segmentado (UISegmentedControl) | Ninguno |
| Control deslizante (UISlider) | [Control deslizante](https://msdn.microsoft.com/library/windows/apps/br209614) <br/>  Consulta también [Cómo agregar un control deslizante](https://msdn.microsoft.com/library/windows/apps/xaml/hh868197) |
| Controlador de vista dividida (UISplitViewController) y delegado de controlador de vista dividida (UISplitViewControllerDelegate) | Ninguno |
| Modificador (UISwitch) | [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/br209712) <br/>  Consulta también [Cómo agregar un modificador de alternancia](https://msdn.microsoft.com/library/windows/apps/xaml/hh868198) |
| Controlador de barra de pestañas (UITabBarController) y delegado de controlador de barra de pestañas (UITabBarControllerDelegate) | Ninguno |
| Controlador de vista de tabla (UITableViewController), vista de tabla (UITableView), delegado de vista de tabla (UITableViewDelegate) y celda de tabla (UITableViewCell) | [ListView](https://msdn.microsoft.com/library/windows/apps/br242878) <br/>  Consulta también [Inicio rápido: agregar controles ListView y GridView](https://msdn.microsoft.com/library/windows/apps/xaml/hh780650) |
| Campo de texto (UITextField) y delegado de campo de texto (UITextFieldDelegate) | [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683) <br/>  Consulta también [Mostrar y editar texto](https://msdn.microsoft.com/library/windows/apps/mt280218) |
| Vista de texto (UITextView) y delegado de vista de texto (UITextViewDelegate) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/>  Consulta también [Inicio rápido: mostrar texto](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| Vista (UIView) y controlador de vista (UIViewController) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/>  Consulta también [Navegación](https://msdn.microsoft.com/library/windows/apps/mt187344). |
| Vista web (UIWebView) y delegado de vista web (UIWebViewDelegate) | [WebView](https://msdn.microsoft.com/library/windows/apps/br227702) <br/>  Consulta también [Ejemplo de control WebView XAML](http://go.microsoft.com/fwlink/p/?LinkId=238582) |
| Ventana (UIWindow) | [Frame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/>  Consulta también [Navegación](https://msdn.microsoft.com/library/windows/apps/mt187344). |

Para ver aún más controles, consulta [Lista de controles](https://msdn.microsoft.com/library/windows/apps/mt185406).

**Nota**  Para obtener una lista de controles para aplicaciones UWP con JavaScript y el código HTML, vea la [lista de controles](https://msdn.microsoft.com/library/windows/apps/hh465453).

### <a name="next-step"></a>Paso siguiente

[Introducción: Navegación](getting-started-navigation.md)

## <a name="related-topics"></a>Temas relacionados

* [Build 2014: Información sobre los controles y la interfaz de usuario de XAML](http://go.microsoft.com/fwlink/p/?LinkID=397897)
* [Build 2014: Desarrollar aplicaciones con el marco de trabajo de la interfaz de usuario de XAML común](http://go.microsoft.com/fwlink/p/?LinkID=397898)
* [Build 2014: Usar Visual Studio para crear aplicaciones XAML convergidas](http://go.microsoft.com/fwlink/p/?LinkID=397876)
