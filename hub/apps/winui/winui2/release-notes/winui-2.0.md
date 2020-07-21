---
title: Notas de la versión de WinUI 2.0
description: Notas de la versión de WinUI 2.0.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 8518f528c67e11b5ac895bb4e000751993fa1a91
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493600"
---
# <a name="windows-ui-library-20"></a>Biblioteca de interfaz de usuario de Windows 2.0

WinUI 2.0 es la primera versión pública de la biblioteca de interfaz de usuario de Windows.

WinUI es la forma más fácil de crear excelentes experiencias de Fluent Design para Windows.

Incluye dos paquetes NuGet:

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml): controles y Fluent Design para aplicaciones para UWP. Este es el paquete principal de WinUI.

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct): API de bajo nivel para su uso en componentes de software intermedio.

Puede descargar y usar paquetes de WinUI en la aplicación mediante el administrador de paquetes NuGet: consulte [Introducción a la biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/getting-started) para obtener más información.

WinUI es un proyecto de código abierto que se hospeda en GitHub. Agradecemos los informes de errores, las solicitudes de características y las contribuciones de código de la comunidad en el [repositorio de la biblioteca de interfaz de usuario de Windows](https://aka.ms/winui).

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

Octubre de 2018

Esta es la primera versión del [paquete Microsoft.UI.Xaml NuGet](https://www.nuget.org/packages/Microsoft.UI.Xaml). Incluye las características y los controles de Fluent nativos para las aplicaciones para UWP de Windows.

### <a name="new-features"></a>Nuevas características

Los controles y patrones de esta versión incluyen:

| Característica | Descripción |
| --- | --- |
|[AcrylicBrush]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.acrylicbrush)| Pinta un área con un material semitransparente que usa varios efectos, como desenfoque y textura de ruido.|
|[BitmapIconSource]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| Representa un origen de icono que usa un mapa de bits como su contenido.|
|[ColorPicker]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.colorpicker)| Representa un control que permite a un usuario seleccionar un color mediante un espectro de colores, controles deslizantes y entradas de texto.|
|[CommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|Representa un control flotante especializado que proporciona el diseño del control AppBarButton y los elementos de comando relacionados.|
|[DropDownButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|Representa un botón de contenido adicional destinado a abrir un menú.|
|[FontIconSource ](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|Representa un origen de icono que usa un glifo de la fuente especificada.|
|[MenuBar](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)|Representa un contenedor especializado que presenta un conjunto de menús en una fila horizontal, normalmente en la parte superior de una ventana de aplicación.|
|[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)|Representa un menú de nivel superior en un control MenuBar.|
|[NavigationView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.navigationview)|Representa un contenedor que permite la navegación por el contenido de la aplicación. Tiene un encabezado, una vista para el contenido principal y un panel de menús para los comandos de navegación.|
|[ParallaxView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.parallaxview)|Representa un contenedor que vincula la posición de desplazamiento de un elemento en primer plano, como una lista, a un elemento en el fondo, como una imagen. Al desplazarte por el elemento en primer plano, se anima el elemento en el fondo para crear un efecto parallax.|
|[PersonPicture](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.personpicture)|Representa un control que muestra la imagen de avatar de una persona, si está disponible; de lo contrario, muestra las iniciales de la persona o un glifo genérico.|
|[RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|Representa un control que permite a un usuario escribir una clasificación por estrellas.|
|[RefreshContainer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|Representa un control de contenedor que proporciona un control RefreshVisualizer y la funcionalidad de extracción para actualizar para el contenido desplazable.|
|[RefreshVisualizer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|Representa un control que proporciona indicadores de estado animados para la actualización de contenido.|
|[RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|Pinta un fondo de control con un efecto de visualización mediante el pincel de composición y efectos de luz.|
|[RevealBorderBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|Pinta un borde de control con un efecto de visualización mediante el pincel de composición y efectos de luz.|
|[RevealBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbrush)|Clase base para los pinceles que usan efectos de composición y la iluminación para implementar el tratamiento de visualización del diseño visual.|
|[SplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.splitbutton)|Representa un botón con dos partes que se pueden invocar por separado. Una parte se comporta como un botón estándar y la otra invoca un control flotante.|
|[SwipeControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|Representa un contenedor que proporciona acceso a los comandos contextuales a través de interacciones táctiles.|
|[SymbolIconSource](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|Representa un origen de icono que usa un glifo de la fuente Segoe MDL2 Assets como su contenido.|
|[TextCommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|Representa un control flotante de la barra de comandos especializado que contiene comandos para editar texto.|
|[ToggleSplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|Representa un botón con dos partes que se pueden invocar por separado. Una parte se comporta como un botón de alternancia y la otra invoca un control flotante.|
|[TreeView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.treeview)|Representa una lista jerárquica con nodos que se expanden y se contraen, y que contienen elementos anidados.|

## <a name="examples"></a>Ejemplos

La aplicación de ejemplo XAML Controls Gallery incluye demostraciones interactivas y código de ejemplo para usar controles WinUI.

* Instalación de la aplicación XAML Controls Gallery desde [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt).

* La aplicación XAML Controls Gallery también está disponible en [código abierto en GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="documentation"></a>Documentación

Se incluyen artículos sobre procedimientos para los controles de la biblioteca de interfaz de usuario de Windows con la [documentación sobre controles de la Plataforma universal de Windows](/windows/uwp/design/controls-and-patterns/).

Los documentos de referencia de API se encuentran aquí: [API de la biblioteca de interfaz de usuario de Windows](/uwp/api/overview/winui/).
