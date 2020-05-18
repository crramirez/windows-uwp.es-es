---
description: Esta guía te ayuda a crear interfaces de usuario de UWP basadas en Fluent directamente en tus aplicaciones de WPF y Windows Forms
title: Controles de UWP en aplicaciones de escritorio
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: d050e2b4a7659f8910ce603ec7e90b703cc7722f
ms.sourcegitcommit: 2571af6bf781a464a4beb5f1aca84ae7c850f8f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606244"
---
# <a name="host-uwp-xaml-controls-in-desktop-apps-xaml-islands"></a>Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)

A partir de Windows 10, versión 1903, puedes hospedar controles de UWP en aplicaciones de escritorio que no son de UWP mediante una característica denominada *islas de XAML*. Esta característica te permite mejorar el aspecto y la funcionalidad de las aplicaciones de WPF, Windows Forms y Win32 de C++ existentes, con las características más recientes de la UI de Windows 10 que solo están disponibles a través de controles de UWP. Esto significa que puedes usar características de UWP como [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions) y controles que admiten el [Sistema Fluent Design](/windows/uwp/design/fluent-design-system/index) en las aplicaciones de WPF, Windows Forms y Win32 de C++ existentes.

Puedes hospedar cualquier control de UWP que se derive de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido:

* Cualquier control de UWP de origen proporcionado por el Windows SDK.
* Cualquier control de UWP personalizado (por ejemplo, un control de usuario que conste de varios controles de UWP que funcionen conjuntamente). Debes tener el código fuente del control personalizado para poder compilarlo con la aplicación.

Fundamentalmente, las islas XAML se crean mediante la *API de hospedaje XAML de UWP*. Esta API consta de varias clases de Windows Runtime e interfaces COM que se introdujeron en el SDK de Windows 10, versión 1903. También proporcionamos un conjunto de controles .NET de las islas XAML en el [kit de herramientas de la Comunidad Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/) que usan internamente la API de hospedaje XAML de UWP y proporcionan una experiencia de desarrollo más cómoda para las aplicaciones de WPF y Windows Forms.

La forma de usar las islas XAML depende del tipo de aplicación y de los tipos de controles de UWP que quieras hospedar.

> [!NOTE]
> Si tienes comentarios sobre las islas XAML, crea un nuevo problema en el [repositorio de Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deja los comentarios allí. Si prefieres enviar los comentarios de forma privada, puedes enviarlos a XamlIslandsFeedback@microsoft.com. Tus aportes y escenarios son muy importantes para nosotros.

## <a name="requirements"></a>Requisitos

Las islas XAML tienen estos requisitos de tiempo de ejecución:

* Windows 10, versión 1903, o una versión posterior.
* Si tu aplicación no está empaquetada en un [paquete MSIX](https://docs.microsoft.com/windows/msix) para su implementación, el equipo debe tener instalado el [Tiempo de ejecución de Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads).

## <a name="wpf-and-windows-forms-applications"></a>Aplicaciones de WPF y Windows Forms

Se recomienda que las aplicaciones de WPF y Windows Forms usen los controles .NET de las islas XAML que están disponibles en el kit de herramientas de la Comunidad Windows. Estos controles ofrecen un modelo de objetos que imita (o proporciona acceso a) las propiedades, los métodos y los eventos de los controles de UWP correspondientes. También controlan el comportamiento, como la navegación con el teclado y los cambios de diseño.

Hay dos conjuntos de controles de islas XAML para aplicaciones de WPF y Windows Forms: *controles encapsulados* y *controles host*. 

### <a name="wrapped-controls"></a>Controles encapsulados

Las aplicaciones de WPF y Windows Forms pueden utilizar una selección de controles de islas XAML que encapsulan la interfaz y la funcionalidad de un control de UWP específico. Puedes agregar estos controles directamente en la superficie de diseño de tu proyecto de WPF o Windows Forms y, luego, usarlos como cualquier otro control de WPF o Windows Forms en el diseñador.

Los siguientes controles encapsulados de UWP están disponibles actualmente en el kit de herramientas de la Comunidad Windows. 

| Control | Sistema operativo mínimo compatible | Descripción |
|-----------------|-------------------------------|-------------|
| [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)<br>[InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) | Windows 10, versión 1903 | Proporciona una superficie y barras de herramientas relacionadas para la interacción del usuario basada en Windows Ink en la aplicación de escritorio de Windows Forms o WPF. |
| [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement) | Windows 10, versión 1903 | Incrusta una vista que transmite y representa contenido multimedia, como vídeo, en la aplicación de escritorio de Windows Forms o WPF. |
| [MapControl](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mapcontrol) | Windows 10, versión 1903 | Te permite mostrar un mapa simbólico o fotorrealista en la aplicación de escritorio de Windows Forms o WPF. |

Para ver un tutorial que muestra cómo usar los controles encapsulados de UWP, consulta [Hospedaje de un control estándar de UWP en una aplicación WPF](host-standard-control-with-xaml-islands.md).

### <a name="host-controls"></a>Controles host

En el caso de controles personalizados y otros escenarios más allá de los cubiertos por los controles encapsulados disponibles, las aplicaciones de WPF y Windows Forms también pueden usar el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) que está disponible en el kit de herramientas de la Comunidad Windows.

| Control | Sistema operativo mínimo compatible | Descripción |
|-----------------|-------------------------------|-------------|
| [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) | Windows 10, versión 1903 | Puede hospedar cualquier control de UWP que se derive de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP de origen proporcionado por el Windows SDK, así como controles personalizados. |

Para ver tutoriales que muestran cómo usar el control **WindowsXamlHost**, consulta [Hospedaje de un control estándar de UWP en una aplicación WPF](host-standard-control-with-xaml-islands.md) y [Hospedaje de un control personalizado de UWP en una aplicación WPF](host-custom-control-with-xaml-islands.md).

> [!NOTE]
> El uso del control **WindowsXamlHost** para hospedar controles de UWP personalizados solo se admite en las aplicaciones de WPF y Windows Forms que tienen como destino .NET Core 3. Hospedar controles de UWP de origen proporcionados por el Windows SDK se admite en las aplicaciones que tienen como destino .NET Framework o .NET Core 3.

<span id="requirements" />

### <a name="configure-your-project-to-use-the-xaml-island-net-controls"></a>Configuración del proyecto para usar los controles .NET de las islas XAML

Los controles .NET de las islas XAML requieren Windows 10, versión 1903, o una versión posterior. Para usar estos controles, instala uno de los paquetes NuGet que se enumeran a continuación. Estos paquetes ofrecen todo lo necesario para usar los controles encapsulados y los controles host de las islas XAML, e incluyen otros paquetes NuGet relacionados que también son necesarios.

| Tipo de control | Paquete NuGet  | Artículos relacionados |
|-----------------|----------------|---------------------|
| [Controles encapsulados](#wrapped-controls) | Versión 6.0.0 o posterior de estos paquetes: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls)</li></ul>  | [Hospedaje de un control estándar de UWP en una aplicación WPF](host-standard-control-with-xaml-islands.md)  |
| [Control host](#host-controls) | Versión 6.0.0 o posterior de estos paquetes: <ul><li>WPF: [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost)</li><li>Windows Forms: [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost)</li></ul>  | [Hospedaje de un control estándar de UWP en una aplicación WPF](host-standard-control-with-xaml-islands.md)<br/>[Hospedaje de un control personalizado de UWP en una aplicación WPF](host-custom-control-with-xaml-islands.md)  |

Ten en cuenta lo siguiente:

* Los paquetes de controles host también se incluyen en los paquetes de controles encapsulados. Puedes instalar los paquetes de controles encapsulados si quieres usar ambos conjuntos de controles.

* Si hospedas un control personalizado de UWP, el proyecto de WPF o Windows Forms debe tener como destino .NET Core 3. No se admite hospedar controles personalizados de UWP en aplicaciones destinadas a .NET Framework. También tendrás que seguir algunos pasos adicionales para hacer referencia al control personalizado. Para más información, consulta [Hospedaje de un control personalizado de UWP en una aplicación WPF](host-custom-control-with-xaml-islands.md).

### <a name="web-view-controls"></a>Controles de vista web

El kit de herramientas de la Comunidad Windows también proporciona los siguientes controles .NET para hospedar contenido web en aplicaciones de WPF y Windows Forms. Estos controles se usan a menudo en escenarios similares de modernización de aplicaciones de escritorio que los controles de las islas XAML, y se mantienen en el mismo repositorio de [Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32) que los controles de las islas XAML.

| Control | Sistema operativo mínimo compatible | Descripción |
|-----------------|-------------------------------|-------------|
| [WebView](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview) | Windows 10, versión 1803 | Usa el motor de representación de Microsoft Edge para mostrar el contenido web. |
| [WebViewCompatible](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webviewcompatible) | Windows 7 | Proporciona una versión de **WebView** compatible con más versiones de sistemas operativos. Este control usa el motor de representación de Microsoft Edge para mostrar el contenido web en Windows 10, versión 1803, y versiones posteriores, y el motor de representación de Internet Explorer para mostrar el contenido web en versiones anteriores de Windows 10, Windows 8.x y Windows 7. |

Para usar estos controles, instala uno de los paquetes NuGet siguientes:

* WPF: [Microsoft.Toolkit.Wpf.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls.WebView)
* Windows Forms: [Microsoft.Toolkit.Forms.UI.Controls.WebView](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls.WebView)

## <a name="c-win32-applications"></a>Aplicaciones Win32 de C++

Los controles .NET de las islas XAML no se admiten en aplicaciones Win32 de C++. En su lugar, estas aplicaciones deben usar la *API de hospedaje XAML de UWP* proporcionada por el SDK de Windows 10 (versión 1903 y posterior).

La API de hospedaje XAML de UWP consta de varias clases de Windows Runtime e interfaces COM que la aplicación Win32 de C++ puede usar para hospedar cualquier control de UWP que derive de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement). Puedes hospedar controles de UWP en cualquier elemento de UI de la aplicación, que tenga un identificador de ventana asociado (HWND). Para más información sobre esta API, consulta los artículos siguientes.

* [Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++](using-the-xaml-hosting-api.md)
* [Hospedaje de un control estándar de UWP en una aplicación Win32 de C++](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedaje de un control personalizado de UWP en una aplicación Win32 de C++](host-custom-control-with-xaml-islands-cpp.md)

> [!NOTE]
> Los controles encapsulados y los controles host del kit de herramientas de la Comunidad Windows usan la API de hospedaje XAML de UWP internamente, e implementan todo el comportamiento que, de lo contrario, tendrías que controlar tú mismo si usaras la API de hospedaje XAML de UWP directamente, incluida la navegación con el teclado y los cambios de diseño. En el caso de las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente usar estos controles en lugar de la API de hospedaje XAML de UWP directamente, ya que abstraen muchos de los detalles de implementación del uso de la API.

## <a name="architecture-of-xaml-islands"></a>Arquitectura de las islas XAML

A continuación se muestra una visión rápida de cómo los distintos tipos de controles de las islas XAML se organizan arquitectónicamente sobre la API de hospedaje de XAML de UWP.

![Arquitectura de los controles host](images/xaml-islands/host-controls.png)

Las API que aparecen en la parte inferior de este diagrama se incluye con el Windows SDK. Los controles encapsulados y los controles host están disponibles a través de paquetes NuGet en el kit de herramientas de la Comunidad Windows.

## <a name="limitations-and-workarounds"></a>Limitaciones y soluciones alternativas

En las secciones siguientes se describen las limitaciones y las soluciones alternativas para ciertos escenarios de desarrollo de UWP en aplicaciones de escritorio que usan islas XAML. 

### <a name="supported-only-with-workarounds"></a>Compatible solo con soluciones alternativas

:heavy_check_mark: El hospedaje de controles de UWP de la [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) en una isla XAML se admite de forma condicional en la versión actual de las islas XAML. Si la aplicación de escritorio usa un [paquete MSIX](https://docs.microsoft.com/windows/msix) para la implementación, puedes hospedar controles de WinUI de versiones preliminares o de lanzamiento del paquete NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). Si la aplicación de escritorio no está empaquetada con MSIX, puedes hospedar controles de WinUI solo si instalas una versión preliminar del paquete NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

:heavy_check_mark: Para acceder al elemento raíz de un árbol de contenido XAML en una isla XAML y obtener la información relacionada sobre el contexto en el que se hospeda, no uses las clases[CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) y [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window). En su lugar, usa la clase [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). Para obtener más información, consulta [esta sección](#window-host-context-for-xaml-islands).

:heavy_check_mark: Para admitir el [Contrato para contenido compartido](/windows/uwp/app-to-app/share-data) desde una aplicación de WPF, Windows Forms o C++ Win32, la aplicación tiene que usar la interfaz [IDataTransferManagerInterop](https://docs.microsoft.com/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) para obtener el objeto [DataTransferManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) para iniciar la operación de uso compartido para una ventana específica. Para obtener un ejemplo que muestra cómo usar esta interfaz en una aplicación de WPF, consulta el [ejemplo de ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource).

:heavy_check_mark: No se admite el uso de `x:Bind` con controles hospedados en islas XAML. Tendrás que declarar el modelo de datos en una biblioteca de .NET Standard.

### <a name="not-supported"></a>Incompatible

:no_entry_sign: Usar el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles de UWP externos basados en C# en aplicaciones de WPF y Windows Forms que tienen como destino .NET Framework. Este escenario solo se admite en aplicaciones con destinos en .NET Core 3.

:no_entry_sign: El contenido XAML de UWP en las islas XAML no responde a los cambios de tema de Windows de oscuro a claro, ni viceversa, en tiempo de ejecución. El contenido responde a los cambios de contraste alto en tiempo de ejecución.

:no_entry_sign: Agregar un control **WebView** a un control de usuario personalizado (en subproceso, fuera de subproceso o fuera de proceso).

:no_entry_sign: En el modo de pantalla completa no se admiten el control [MediaPlayer](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) y el control de host [MediaPlayerElement](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/mediaplayerelement).

:no_entry_sign: Entrada de texto con la vista de escritura a mano. Para más información sobre esta característica, consulta [este artículo](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-handwriting-view).

:no_entry_sign: Controles de texto que usan vínculos de contenido `@Places` y `@People`. Para más información sobre esta característica, consulta [este artículo](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/content-links).

:no_entry_sign: Las islas XAML no admiten el hospedaje de un elemento [ContentDialog](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) que contiene un control que acepta entrada de texto, como [TextBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) o [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox). Si lo hace, el control de entrada no responderá correctamente a las pulsaciones de teclas. Para lograr una funcionalidad similar con una isla XAML, se recomienda hospedar un elemento [Popup](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) que contenga el control de entrada.

:no_entry_sign: Las islas XAML no admiten actualmente la visualización de archivos SVG en un control [Windows.UI.Xaml.Controls.Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) hospedado ni mediante un objeto [Windows.UI.Xaml.Media.Imaging.SvgImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.svgimagesource). Como alternativa, convierte los archivos de imagen que quieras mostrar en formatos basados en tramas como JPG o PNG.

### <a name="window-host-context-for-xaml-islands"></a>Contexto de hosts de ventanas para islas XAML

Al hospedar islas XAML en una aplicación de escritorio, puedes tener varios árboles de contenido XAML que se ejecuten en el mismo subproceso a la vez. Para acceder al elemento raíz de un árbol de contenido XAML en una isla XAML y obtener la información relacionada sobre el contexto en el que se hospeda, usa la clase [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). Las clases [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) y [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) no proporcionan la información correcta para las islas XAML. Los objetos [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) y [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) existen en el subproceso, y la aplicación puede acceder a ellos, pero no devolverán límites ni visibilidad significativos (son siempre invisibles y tienen un tamaño de 1×1). Para más información, consulta [Hosts de ventanas](/windows/uwp/design/layout/show-multiple-views#windowing-hosts).

Por ejemplo, para obtener el rectángulo delimitador de la ventana que contiene un control de UWP que se hospeda en una isla XAML, usa la propiedad [XamlRoot.Size](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot.size) del control. Dado que todos los controles de UWP que se pueden hospedar en una isla XAML derivan de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), puedes usar la propiedad [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.xamlroot) del control para acceder al objeto **XamlRoot**.

```csharp
Size windowSize = myUWPControl.XamlRoot.Size;
```

No uses la propiedad [CoreWindows.Bounds](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.bounds) para obtener el rectángulo delimitador.

```csharp
// This will return incorrect information for a UWP control that is hosted in a XAML Island.
Rect windowSize = CoreWindow.GetForCurrentThread().Bounds;
```

Para obtener una tabla de las API comunes relacionadas con ventanas que debes evitar en el contexto de las islas XAML y los reemplazos de [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot) recomendados, consulta la tabla de [esta sección](/windows/uwp/design/layout/show-multiple-views#make-code-portable-across-windowing-hosts).

Para obtener un ejemplo que muestra cómo usar esta interfaz en una aplicación de WPF, consulta el ejemplo de [ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource).

## <a name="feature-roadmap"></a>Hoja de ruta de las características

Este es el estado actual de las características relacionadas con las islas XAML:

* **Aplicaciones Win32 de C++** : La API de hospedaje XAML de UWP se considera la versión 1.0 a partir de Windows 10, versión 1903.
* **Aplicaciones administradas que tienen como destino .NET Framework 4.6.2 y versiones posteriores:** Los controles de las islas XAML que están disponibles en los [paquetes NuGet, versión 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls), se consideran la versión 1.0 para las aplicaciones que tienen como destino .NET Framework 4.6.2 y versiones posteriores.
* **Aplicaciones administradas que tienen como destino .NET Core 3.0 y versiones posteriores:** Los controles que están disponibles en los [paquetes NuGet, versión 6.0.0](#configure-your-project-to-use-the-xaml-island-net-controls), aún se encuentran en versión preliminar para desarrolladores para las aplicaciones que tienen como destino .NET Core 3.0 y versiones posteriores. La versión 1.0 de estos controles para .NET Core 3.0 y versiones posteriores está planeada para su publicación posterior.

## <a name="additional-resources"></a>Recursos adicionales

Para obtener información general y tutoriales sobre el uso de las islas XAML, consulta los siguientes artículos y recursos:

* [Tutorial de modernización de una aplicación de WPF](modernize-wpf-tutorial.md): En este tutorial se proporcionan instrucciones paso a paso para usar los controles encapsulados y los controles host en el kit de herramientas de la Comunidad Windows para agregar controles de UWP a una aplicación de línea de negocio de WPF existente. En este tutorial se incluye el código completo de la aplicación de WPF, así como instrucciones detalladas para cada paso del proceso.
* [Ejemplos de código de las islas XAML](https://github.com/microsoft/Xaml-Islands-Samples): En este repositorio se incluyen ejemplos de Windows Forms, WPF y C++ o Win32 que muestran cómo utilizar las islas XAML.
* [Islas XAML v1, actualizaciones y hoja de ruta](https://blogs.windows.com/windowsdeveloper/2019/06/13/xaml-islands-v1-updates-and-roadmap): En esta entrada de blog se describen muchas preguntas comunes sobre las islas XAML, y se proporciona una hoja de ruta de desarrollo detallada.
