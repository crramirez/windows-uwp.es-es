---
description: Esta guía te ayuda a crear interfaces de usuario de UWP basadas en Fluent directamente en tus aplicaciones de WPF y Windows Forms
title: Controles de UWP en aplicaciones de escritorio
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 52287576dbc395af60e15b5f4b4a403db7e92900
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72313452"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)

A partir de Windows 10, versión 1903, puede hospedar controles de UWP en aplicaciones de escritorio que no son de UWP con una característica denominada *islas XAML*. Esta característica permite mejorar la apariencia, el funcionamiento y la funcionalidad de las aplicaciones de WPF, Windows Forms y C++ Win32 existentes con las características más recientes de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles de UWP. Esto significa que puede usar las características de UWP, como [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) y los controles que admiten el [sistema de diseño fluida](/windows/uwp/design/fluent-design-system/index) , en las aplicaciones C++ de WPF, Windows Forms y Win32 existentes.

Puede hospedar cualquier control de UWP que derive de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido:

* Cualquier control UWP de primera parte proporcionado por la biblioteca Windows SDK o WinUI.
* Cualquier control de UWP personalizado (por ejemplo, un control de usuario que consta de varios controles de UWP que funcionan conjuntamente). Debe tener el código fuente del control personalizado para que pueda compilarlo con la aplicación.

Fundamentalmente, las islas XAML se crean mediante la *API de hospedaje XAML de UWP*. Esta API consta de varias clases de Windows Runtime e interfaces COM que se introdujeron en el SDK de Windows 10, versión 1903. También proporcionamos un conjunto de controles .NET de isla de XAML en el kit de herramientas de la [comunidad de Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) que usan la API de hospedaje XAML de UWP internamente y proporcionan una experiencia de desarrollo más cómoda para las aplicaciones de WPF y Windows Forms.

La forma de usar islas XAML depende del tipo de aplicación y de los tipos de controles de UWP que desea hospedar.

> [!NOTE]
> Si tiene comentarios sobre las islas XAML, cree un nuevo problema en el [repositorio Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deje los comentarios allí. Si prefiere enviar sus comentarios de forma privada, puede enviarlos a XamlIslandsFeedback@microsoft.com. Sus conocimientos y escenarios son muy importantes para nosotros.

## <a name="wpf-and-windows-forms-applications"></a>Aplicaciones WPF y Windows Forms

Se recomienda que las aplicaciones de WPF y Windows Forms usen los controles .NET de la isla de XAML que están disponibles en el kit de herramientas de la comunidad de Windows. Estos controles proporcionan un modelo de objetos que imita (o proporciona acceso a) las propiedades, los métodos y los eventos de los controles de UWP correspondientes. También controlan el comportamiento, como la navegación del teclado y los cambios de diseño.

Hay dos conjuntos de controles de isla XAML para WPF y Windows Forms aplicaciones: *controles encapsulados* y *controles host*. A partir de la versión 1903 de Windows 10, estos controles están [disponibles como vista previa para desarrolladores](#feature-roadmap).

### <a name="wrapped-controls"></a>Controles ajustados

Las aplicaciones de WPF y Windows Forms pueden utilizar una selección de controles de isla de XAML que encapsulan la interfaz y la funcionalidad de un control de UWP específico. Puede agregar estos controles directamente a la superficie de diseño de su proyecto de WPF o Windows Forms y, a continuación, utilizarlos como cualquier otro control de WPF o Windows Forms en el diseñador.

Los siguientes controles de UWP ajustados están disponibles actualmente en el kit de herramientas de la comunidad de Windows. 

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versión 1903 | Proporcione una superficie y barras de herramientas relacionadas para la interacción del usuario basada en Windows Ink en la aplicación de escritorio de Windows Forms o WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versión 1903 | Incrusta una vista que transmite y representa contenido multimedia, como vídeo, en la aplicación de escritorio Windows Forms o WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versión 1903 | Permite mostrar un mapa simbólico o de semireal en la aplicación de escritorio de Windows Forms o WPF. |

Para ver un tutorial que muestra cómo usar los controles de UWP ajustados, vea [hospedar un control estándar de UWP en una aplicación de WPF](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Controles host

En el caso de escenarios más allá de los incluidos en los controles ajustados disponibles, las aplicaciones de WPF y Windows Forms también pueden usar el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) que está disponible en el kit de herramientas de la comunidad de Windows.

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, versión 1903 | Puede hospedar cualquier control de UWP que derive de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP de primera parte proporcionado por el Windows SDK, así como los controles personalizados. |

Para ver tutoriales que muestran cómo usar el control **WindowsXamlHost** , vea [hospedar un control estándar de UWP en una aplicación de WPF](host-standard-control-with-xaml-islands.md) y [hospedar un control de UWP personalizado en una aplicación WPF con islas XAML](host-custom-control-with-xaml-islands.md).

> [!NOTE]
> El uso del control **WindowsXamlHost** para hospedar controles UWP personalizados solo se admite en las aplicaciones de WPF y Windows Forms destinadas a .net Core 3. Hospedar controles UWP de primera parte proporcionados por la Windows SDK se admite en las aplicaciones que tienen como destino el .NET Framework o .NET Core 3.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Configurar el proyecto para usar los controles .NET de la isla XAML

Los controles .NET de la isla XAML requieren Windows 10, versión 1903 o una versión posterior. Para usar estos controles, instale uno de los paquetes de NuGet que se enumeran a continuación. Estos paquetes proporcionan todo lo necesario para usar los controles ajustados de la isla XAML y los controles host, e incluyen otros paquetes de NuGet relacionados que también son necesarios.

| Tipo de control | Paquete NuGet  | Artículos relacionados |
|-----------------|----------------|---------------------|
| [Controles ajustados](#wrapped-controls) | Versión 6.0.0-preview7 o posterior de estos paquetes: <ul><li>WPF [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Hospedar un control estándar de UWP en una aplicación WPF](host-standard-control-with-xaml-islands.md)  |
| [Control host](#host-controls) | Versión 6.0.0-preview7 o posterior de estos paquetes: <ul><li>WPF [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Hospedar un control estándar de UWP en una aplicación WPF](host-standard-control-with-xaml-islands.md)<br/>[Hospedar un control personalizado de UWP en una aplicación WPF](host-custom-control-with-xaml-islands.md)  |

Tenga en cuenta los siguientes detalles:

* Los paquetes de control de host también se incluyen en los paquetes de control encapsulado. Puede instalar los paquetes de control ajustados si desea utilizar ambos conjuntos de controles.

* Si hospeda un control personalizado de UWP, el proyecto de WPF o Windows Forms debe tener como destino .NET Core 3. No se admite el hospedaje de controles de UWP personalizados en aplicaciones destinadas a la .NET Framework. También necesitará realizar algunos pasos adicionales para hacer referencia al control personalizado. Para obtener más información, vea [hospedar un control de UWP personalizado en una aplicación WPF con islas XAML](host-custom-control-with-xaml-islands.md).

* Las versiones anteriores de estas instrucciones tenían que agregar el elemento `maxversiontested` a un manifiesto de aplicación en el proyecto de WPF o Windows Forms. Siempre que esté usando las versiones preliminares más recientes de los paquetes de NuGet mencionados anteriormente, ya no tendrá que agregar este elemento al manifiesto.

### <a name="architecture-of-xaml-island-net-controls"></a>Arquitectura de controles .NET de la isla de XAML

A continuación se muestra una visión rápida de cómo los distintos tipos de controles de isla XAML se organizan arquitectónicamente sobre la API de hospedaje XAML de UWP.

![Arquitectura de control de host](images/xaml-islands/host-controls.png)

Las API que aparecen en la parte inferior de este diagrama se incluye con el Windows SDK. Los controles encapsulados y los controles host están disponibles a través de paquetes NuGet en el kit de herramientas de la comunidad de Windows.

### <a name="web-view-controls"></a>Controles de vista Web

El kit de herramientas de la comunidad de Windows también proporciona los siguientes controles .NET para hospedar contenido web en WPF y aplicaciones Windows Forms. Estos controles se usan a menudo en escenarios de modernización de aplicaciones de escritorio similares a los controles de la isla de XAML y se mantienen en el mismo repositorio de [repositorios de Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) que los controles de la isla XAML.

| Control | Sistema operativo mínimo admitido | Descripción |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versión 1803 | Usa el motor de representación de Microsoft Edge para mostrar el contenido Web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Proporciona una versión de **WebView** que es compatible con más versiones del sistema operativo. Este control usa el motor de representación de Microsoft Edge para mostrar el contenido web en Windows 10 versión 1803 y versiones posteriores, y el motor de representación de Internet Explorer para mostrar el contenido web en versiones anteriores de Windows 10, Windows 8. x y Windows 7. |

Para usar estos controles, instale uno de estos paquetes NuGet:

* WPF [Microsoft. Toolkit. WPF. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft. Toolkit. Forms. UI. Controls. WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>C++Aplicaciones Win32

Los controles .NET de la isla de XAML no C++ se admiten en aplicaciones Win32. En su lugar, estas aplicaciones deben usar la *API de hospedaje XAML de UWP* proporcionada por el SDK de Windows 10 (versión 1903 y posteriores).

La API de hospedaje XAML de UWP consta de varias clases de Windows Runtime e interfaces C++ com que la aplicación Win32 puede usar para hospedar cualquier control de UWP que se derive de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Puede hospedar controles de UWP en cualquier elemento de la interfaz de usuario de la aplicación que tenga un identificador de ventana asociado (HWND). Para obtener más información sobre esta API, incluidos los requisitos previos, consulte [uso de la API de hospedaje C++ XAML de UWP en una aplicación Win32](using-the-xaml-hosting-api.md).

> [!NOTE]
> Los controles encapsulados y los controles host del kit de herramientas de la comunidad de Windows usan internamente la API de hospedaje XAML de UWP e implementan todo el comportamiento que, de lo contrario, tendría que administrarse si usara la API de hospedaje XAML de UWP directamente, incluida la navegación mediante el teclado y cambios de diseño. En el caso de las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente usar estos controles en lugar de la API de hospedaje XAML de UWP directamente porque abstraen muchos de los detalles de implementación del uso de la API.

## <a name="feature-roadmap"></a>Guía básica de características

A partir del lanzamiento de la versión 1903 de Windows 10, los controles .NET de la isla XAML en el kit de herramientas de la comunidad de Windows siguen estando en la vista previa del desarrollador hasta que la versión 1,0 de los controles esté disponible.

* La versión 1,0 de los controles para el .NET Framework 4.6.2 y versiones posteriores está planeada para su lanzamiento en la [versión 6,0 del kit de herramientas](https://github.com/windows-toolkit/WindowsCommunityToolkit/milestones).
* La versión 1,0 de los controles para .NET Core 3 está planeada para una versión posterior del kit de herramientas.
* Si desea probar las versiones preliminares más recientes de la versión 1,0 de estos controles para el .NET Framework y .NET Core 3, consulte los paquetes NuGet de la versión 6.0.0-preview7 (o posterior) en la galería del kit de herramientas de la [comunidad de UWP](https://dotnet.myget.org/gallery/uwpcommunitytoolkit) .

Para obtener más información, consulte [esta entrada de blog](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap).

## <a name="additional-resources"></a>Recursos adicionales

Para obtener información general y tutoriales sobre el uso de las islas XAML, consulte los siguientes artículos y recursos:

* [Modernización de un tutorial de aplicación de WPF](modernize-wpf-tutorial.md): En este tutorial se proporcionan instrucciones paso a paso para usar los controles ajustados y los controles host en el kit de herramientas de la comunidad de Windows para agregar controles de UWP a una aplicación de línea de negocio de WPF existente. En este tutorial se incluye el código completo de la aplicación WPF, así como instrucciones detalladas para cada paso del proceso.
* [Islas de XAML v1: actualizaciones y mapa de ruta](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): En esta entrada de blog se describen muchas preguntas comunes sobre las islas XAML y se proporciona un mapa de ruta de desarrollo detallado.
