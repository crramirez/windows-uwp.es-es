---
Description: Ver diferentes partes de la aplicación en ventanas distintas.
title: Mostrar varias vistas de una aplicación
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e7d6ea614a9d85eadfcb807c6e6100dbe15ed0c4
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970740"
---
# <a name="show-multiple-views-for-an-app"></a>Mostrar varias vistas de una aplicación

![Wireframe que muestra una aplicación con varias ventanas](images/multi-view.gif)

Ayuda a tus usuarios a ser más productivos y permíteles ver partes independientes de la aplicación en ventanas distintas. Cuando se crean varias ventanas para una aplicación, la barra de tareas muestra cada ventana por separado. Los usuarios pueden mover, cambiar de tamaño, mostrar y ocultar ventanas de la aplicación de manera independiente, así como cambiar entre ventanas de la aplicación como si usaran aplicaciones distintas.

> **API importantes**: [Espacio de nombres Windows.UI.ViewManagement](/uwp/api/windows.ui.viewmanagement), [espacio de nombres Windows.UI.WindowManagement](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>¿Cuándo debe una aplicación usar vistas múltiples?

Hay distintos escenarios que pueden beneficiarse de las vistas múltiples. Estos son algunos ejemplos:

- Una aplicación de correo electrónico que permite a los usuarios ver una lista de mensajes recibidos mientras redactan un correo electrónico nuevo.
- Una aplicación de libreta de direcciones que permite a los usuarios comparar la información de contacto de varias personas en paralelo.
- Una aplicación de reproducción de música que permite a los usuarios ver lo que se está reproduciendo mientras exploran una lista de otra música disponible.
- Una aplicación de notas que permite a los usuarios copiar información de una página de notas en otra.
- Una aplicación de lectura que permite a los usuarios abrir varios artículos para leerlos más tarde, después de haber leído todos los titulares generales.

Aunque cada diseño de aplicación es único, te recomendamos que incluyas un botón de "ventana nueva" en una ubicación predecible, como la esquina superior derecha del contenido que se pueda abrir en una ventana nueva. También puedes incluir una opción de [menú contextual](../controls-and-patterns/menus.md) "Abrir en una nueva ventana".

Para crear instancias independientes de la aplicación (en lugar de ventanas independientes de la misma instancia), consulta [Crear una aplicación de Windows con instancias múltiples](../../launch-resume/multi-instance-uwp.md).

## <a name="windowing-hosts"></a>Hosts de ventanas

Hay diferentes maneras en que se puede hospedar el contenido de Windows dentro de una aplicación.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     Una vista de la aplicación es el emparejamiento 1:1 de un subproceso y una ventana que la aplicación usa para mostrar contenido. La primera vista que se crea al iniciar la aplicación se denomina *vista principal*. Cada instancia de CoreWindow o ApplicationView funciona en su propio subproceso. Tener que trabajar en distintos subprocesos de la interfaz de usuario puede complicar las aplicaciones con ventanas múltiples.

    La vista principal de la aplicación siempre se hospeda en una instancia de ApplicationView. El contenido de una ventana secundaria se puede hospedar en una instancia de ApplicationView o de AppWindow.

    Para más información sobre cómo usar ApplicationView para mostrar ventanas secundarias en una aplicación, consulta [Usar ApplicationView](application-view.md).
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow simplifica la creación de aplicaciones de Windows con ventanas múltiples porque funciona en el mismo subproceso de interfaz de usuario en el que se crea.

    La clase AppWindow y otras API del espacio de nombres [WindowManagement](/uwp/api/windows.ui.windowmanagement) están disponibles a partir de Windows 10, versión 1903 (SDK 18362). Si tu aplicación tiene como destino versiones anteriores de Windows 10, debes usar ApplicationView para crear ventanas secundarias.

    Para más información sobre cómo usar AppWindow para mostrar ventanas secundarias en una aplicación, consulta [Usar AppWindow](app-window.md).

    > [!NOTE]
    > AppWindow se encuentra actualmente en versión preliminar. Esto significa que puedes enviar aplicaciones que usan AppWindow a Microsoft Store, pero se sabe que algunos componentes de la plataforma y el marco no funcionan con AppWindow (consulta [Limitaciones](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (islas XAML)

     El contenido XAML para UWP en una aplicación de Win32 (usando HWND), también conocido como islas XAML, se hospeda en DesktopWindowXamlSource.

    Para más información sobre las islas XAML, consulta [Uso de la API de hospedaje XAML de UWP en una aplicación de escritorio](/windows/apps/desktop/modernize/using-the-xaml-hosting-api).

### <a name="make-code-portable-across-windowing-hosts"></a>Crear código portátil entre hosts de ventanas

Cuando el contenido XAML se muestra en [CoreWindow](/uwp/api/windows.ui.core.corewindow), siempre hay una instancia de [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) y un objeto XAML [Window](/uwp/api/windows.ui.xaml.window) asociados. Puedes usar las API en estas clases para obtener información, como los límites de la ventana. Para recuperar una instancia de estas clases, usa el método estático [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread), el método [ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) o la propiedad [Window.Current](/uwp/api/windows.ui.xaml.window.current). Además, hay muchas clases que usan el patrón `GetForCurrentView` para recuperar una instancia de la clase, como [DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview).

Estas API funcionan porque solo hay un árbol de contenido XAML para una instancia de CoreWindow o ApplicationView, por lo que el código XAML conoce el contexto en el que se hospeda esa instancia de CoreWindow o ApplicationView.

Cuando el contenido XAML se ejecuta dentro de una instancia de AppWindow o DesktopWindowXamlSource, puedes tener varios árboles de contenido XAML en ejecución en el mismo subproceso al mismo tiempo. En este caso, estas API no proporcionan la información correcta, porque el contenido ya no se ejecuta dentro de la instancia actual de CoreWindow o ApplicationView (o el objeto XAML Window).

Para asegurarte de que el código funcione correctamente en todos los hosts de ventanas, debes reemplazar las API que se basan en [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) y [Window](/uwp/api/windows.ui.xaml.window) por las nuevas API que obtienen el contexto de la clase [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot).
La clase XamlRoot representa un árbol de contenido XAML y la información sobre el contexto en el que se hospeda, ya sea CoreWindow, AppWindow o DesktopWindowXamlSource. Esta capa de abstracción permite escribir el mismo código independientemente del host de ventanas en el que se ejecute el código XAML.

En esta tabla se muestra el código que no funciona correctamente entre los hosts de ventanas y el nuevo código portátil con el que se puede reemplazar, así como algunas API que no es necesario cambiar.

| Si has usado... | Reemplázalo por... |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Sin cambios. Esto se admite en AppWindow y DesktopWindowXamlSource. |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Sin cambios. Esto se admite en AppWindow y DesktopWindowXamlSource. |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | Devuelve el objeto XAML Window principal que está estrechamente enlazado a la instancia de CoreWindow actual. Consulta la nota al final de esta tabla. |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | Sin cambios. Esto se admite en AppWindow y DesktopWindowXamlSource. |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>En las aplicaciones con islas XAML, se producirá un error. En las aplicaciones con AppWindow, se devolverán los elementos emergentes abiertos en la ventana principal. | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> En el caso del contenido XAML en DesktopWindowXamlSource, hay un instancia de CoreWindow/Window en el subproceso, pero siempre es invisible y tiene un tamaño de 1x1. La aplicación todavía puede acceder a ella, pero no devolverá unos límites o visibilidad significativos.
>
>En el caso de contenido XAML en una instancia de AppWindow, siempre habrá exactamente una instancia de CoreWindow en el mismo subproceso. Si llamas a una API `GetForCurrentView` o `GetForCurrentThread`, esa API devolverá un objeto que refleja el estado de CoreWindow en el subproceso, no de las instancias de AppWindow que se puedan estar ejecutando en ese subproceso.


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Proporciona un punto de entrada claro a la vista secundaria usando el glifo "abrir ventana nueva".
- Comunica el objetivo de la vista secundaria a los usuarios.
- Asegúrate de que tu aplicación es totalmente funcional en una sola vista y que los usuarios solo abrirán una vista secundaria para mayor comodidad.
- No uses la vista secundaria para proporcionar notificaciones u otros tipos de visualizaciones transitorias.

## <a name="related-topics"></a>Temas relacionados

- [Usar AppWindow](app-window.md)
- [Usar ApplicationView](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
