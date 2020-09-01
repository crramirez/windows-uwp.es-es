---
description: Obtén instrucciones de diseño y de codificación para agregar controles y patrones a tu aplicación de Windows. Descubre más de 45 controles eficaces para su uso con la aplicación.
title: 'Controles y patrones de Windows: desarrollo de aplicaciones de Windows'
keywords: controles de UWP, interfaz de usuario, controles de la aplicación, controles de windows
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 08471b8a04d37c34378b549b8534979a4188d7b8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173961"
---
# <a name="controls-for-windows-apps"></a>Controles para aplicaciones de Windows

![Controles](../images/controls-2x.png)

En el desarrollo de aplicaciones de Windows, un <i>control</i> es un elemento de la interfaz de usuario que muestra contenido o permite la interacción. Los controles son los elementos esenciales de la interfaz de usuario. Un <i>patrón</i> es una receta para combinar varios controles con el fin de hacer algo nuevo.

Ponemos a tu disposición más de 45 controles, que van desde botones simples hasta controles de datos de enorme eficacia, como la vista de cuadrícula.  Estos controles forman parte de Fluent Design System y pueden ayudarte a crear una interfaz de usuario llamativa y escalable que quede bien en todos los dispositivos y tamaños de pantalla.

Los artículos de esta sección proporcionan instrucciones de diseño y de codificación para agregar controles y patrones a tu aplicación de Windows.

## <a name="intro"></a>Introducción

Instrucciones generales y ejemplos de código para agregar y aplicar estilos a controles en XAML y C#.

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">Agregar controles y controlar eventos</a></b> <br/>
Hay 3 pasos clave para agregar controles a la aplicación: agregar un control a la interfaz de usuario de la aplicación, establecer propiedades en el control y agregar código a los controladores de eventos del control para que realice una acción.</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">Estilo de los controles</a></b> <br/>
El marco XAML te permite personalizar la apariencia de tus aplicaciones de varias maneras. Los estilos te permiten establecer propiedades de control y reusar esa configuración para mantener un aspecto uniforme en varios controles.</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>Obtener la biblioteca de interfaz de usuario de Windows

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | Algunos controles solo están disponibles en la biblioteca de interfaz de usuario de Windows (WinUI), un paquete NuGet que contiene nuevos controles y características de interfaz de usuario. Para obtenerla, consulta la [introducción a la biblioteca de interfaz de usuario de Windows y las instrucciones de instalación](/uwp/toolkits/winui/).<br/>A partir de WinUI 2.2, el estilo predeterminado de muchos controles se actualizó para usar esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](../style/rounded-corner.md). |

## <a name="alphabetical-index"></a>Índice alfabético

Obtener información detallada sobre los patrones y controles específicos. (Para obtener una lista ordenada por función, consulta [Índice de controles por función](controls-by-function.md)).

:::row:::
    :::column:::

- Reproductor visual animado (consulte [Lottie](/windows/communitytoolkit/animations/lottie)) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Cuadro de sugerencias automáticas](auto-suggest-box.md)
- [Botón](buttons.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Vista del calendario](calendar-view.md)
- [Casilla](checkbox.md)
- [Selector de color](color-picker.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Cuadro combinado](combo-box.md)
- [Barra de comandos](app-bars.md)
- [Control flotante de la barra de comandos](command-bar-flyout.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Tarjeta de contacto](contact-card.md)
- [Cuadro de diálogo de contenido](dialogs-and-flyouts/dialogs.md)
- [Vínculo de contenido](content-links.md)
- [Menú contextual](menus.md)
- [Selector de fecha](date-picker.md)
- [Cuadros de diálogo y controles flotantes](dialogs-and-flyouts/index.md)
- [Botón de lista desplegable](buttons.md#create-a-drop-down-button) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Vista para alternar](flipview.md)
- [control flotante](dialogs-and-flyouts/flyouts.md)
- [Formularios](forms.md) (patrón)
- [Vista de cuadrícula](listview-and-gridview.md)
- [Hipervínculo](hyperlinks.md)
- [Botón de hipervínculo](hyperlinks.md#create-a-hyperlinkbutton)
- [Imágenes y pinceles de imagen](images-imagebrushes.md)
- [Controles de entrada manuscrita](inking-controls.md)
- [Vista de lista](listview-and-gridview.md)
- [Control de mapa](../../maps-and-location/display-maps.md)
- [Maestro/detalles](master-details.md) (patrón)
- [Reproducción de multimedia](media-playback.md)
- [Barra de menús](menus.md#create-a-menu-bar) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Control flotante de menú](menus.md)
- [Vista de navegación](navigationview.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)

    :::column-end:::
    :::column:::

- [Cuadro de número](number-box.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Vista de Parallax](..\motion\parallax.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Cuadro de contraseña](password-box.md)
- [Imagen de persona](person-picture.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Pivot](pivot.md)
- [Barra de progreso](progress-controls.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Círculo de progreso](progress-controls.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Botón de radio](radio-button.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Control de clasificación](rating.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Botón Repetir](buttons.md#create-a-repeat-button)
- [Cuadro de texto enriquecido](rich-edit-box.md)
- [Bloque de texto enriquecido](rich-text-block.md)
- [Visor de desplazamiento](scroll-controls.md)
- [Búsqueda](search.md) (patrón)
- [Zoom semántico](semantic-zoom.md)
- [Formas](shapes.md)
- [Control deslizante](slider.md)
- [Botón Dividir](buttons.md#create-a-split-button) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Vista dividida](split-view.md)
- [Control Deslizar rápidamente](swipe.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Vista de pestañas](tab-view.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Sugerencia de enseñanza](dialogs-and-flyouts/teaching-tip.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Bloque de texto](text-block.md)
- [Cuadro de texto](text-box.md)
- [Selector de hora](time-picker.md)
- [Modificador para alternar](toggles.md)
- [Botón de alternancia](buttons.md)
- [Botón Activar o desactivar división](buttons.md#create-a-toggle-split-button)
- [Información sobre herramientas](tooltips.md)
- [Vista de árbol](tree-view.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Vista de dos paneles](two-pane-view.md) ![Logotipo de WinUI](images/winui-logo-16x16.png)
- [Vista web](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>XAML Controls Gallery

Obtén la aplicación _XAML Controls Gallery_ de Microsoft Store para ver en acción estos controles y Fluent Design System. La aplicación es un asistente interactivo para este sitio web. Una vez instalada, puedes usar los vínculos de las páginas de control individuales para iniciar la aplicación y ver el control en acción.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>Controles adicionales

Hay disponibles controles adicionales para el desarrollo de Windows de empresas como <a href="https://www.telerik.com/">Telerik</a>, <a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>, <a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>, <a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>, <a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> y <a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a>. Estos controles proporcionan compatibilidad adicional para desarrolladores de empresa y .NET, ya que mejoran los controles estándar del sistema con controles y servicios personalizados.