---
Description: Ver diferentes partes de la aplicación en ventanas independientes.
title: Mostrar varias vistas de una aplicación
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729516"
---
# <a name="show-multiple-views-for-an-app"></a>Mostrar varias vistas de una aplicación

![Trama reticular que muestra una aplicación con varias ventanas](images/multi-view.gif)

Ayuda a tus usuarios a ser más productivos al permitirles ver partes independientes de la aplicación en ventanas distintas. Cuando se crean varias ventanas para una aplicación, la barra de tareas muestra cada ventana por separado. Los usuarios pueden mover, cambiar de tamaño, mostrar y ocultar ventanas de la aplicación de manera independiente, así como cambiar entre ventanas de la aplicación como si usaran aplicaciones distintas.

> **API importantes**: Espacio de nombres [Windows. UI. ViewManagement](/uwp/api/windows.ui.viewmanagement), [espacio de nombres Windows. UI. WindowManagement](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>¿Cuándo debe una aplicación usar varias vistas?

Hay una variedad de escenarios que se pueden beneficiar de varias vistas. Estos son algunos ejemplos:

- Una aplicación de correo electrónico que permite a los usuarios ver una lista de mensajes recibidos al redactar un correo electrónico nuevo
- Una aplicación de la libreta de direcciones que permite a los usuarios comparar la información de contacto de varias personas en paralelo
- Una aplicación de reproducción de música que permite a los usuarios ver lo que se está reproduciendo mientras exploras una lista de otra música disponible
- Una aplicación de toma de notas que permite a los usuarios copiar información de una página de notas en otra
- Una aplicación de lectura que permite a los usuarios abrir varios artículos para leerlos más tarde, después de la oportunidad de examinar todos los titulares de nivel superior

Aunque cada diseño de aplicación es único, te recomendamos que incluyas un botón de "ventana nueva" en una ubicación predecible, como la esquina superior derecha del contenido que se puede abrir en una ventana nueva. Considere también la posibilidad de incluir una opción de [menú contextual](../controls-and-patterns/menus.md) en "abrir en una nueva ventana".

Para crear instancias independientes de la aplicación (en lugar de ventanas independientes para la misma instancia), consulte [creación de una aplicación para UWP de varias instancias](../../launch-resume/multi-instance-uwp.md).

## <a name="windowing-hosts"></a>Hosts de ventanas

Hay diferentes maneras en que se puede hospedar el contenido de UWP dentro de una aplicación.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     Una vista de la aplicación es el emparejamiento 1:1 de un subproceso y una ventana que la aplicación usa para mostrar contenido. La primera vista que se crea al iniciar la aplicación se denomina *vista principal*. Cada CoreWindow/ApplicationView funciona en su propio subproceso. Tener que trabajar en distintos subprocesos de la interfaz de usuario puede complicar las aplicaciones de varias ventanas.

    La vista principal de la aplicación siempre se hospeda en un ApplicationView. El contenido de una ventana secundaria se puede hospedar en un ApplicationView o en un AppWindow.

    Para obtener información sobre cómo usar ApplicationView para mostrar las ventanas secundarias en la aplicación, consulte [uso de ApplicationView](application-view.md).
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow simplifica la creación de aplicaciones UWP de varias ventanas porque funciona en el mismo subproceso de interfaz de usuario desde el que se crea.

    La clase AppWindow y otras API del espacio de nombres [WindowManagement](/uwp/api/windows.ui.windowmanagement) están disponibles a partir de Windows 10, versión 1903 (SDK 18362). Si su aplicación tiene como destino versiones anteriores de Windows 10, debe usar ApplicationView para crear ventanas secundarias.

    Para obtener información sobre cómo usar AppWindow para mostrar las ventanas secundarias en la aplicación, consulte [uso de AppWindow](app-window.md).

    > [!NOTE]
    > AppWindow se encuentra actualmente en versión preliminar. Esto significa que puede enviar aplicaciones que usan AppWindow a la tienda, pero se sabe que algunos componentes de plataforma y marco no funcionan con AppWindow (vea [limitaciones](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (Islas XAML)

     El contenido XAML de UWP en una aplicación Win32 (mediante HWND), también conocido como islas XAML, se hospeda en un DesktopWindowXamlSource.

    Para obtener más información sobre las islas XAML, consulte [uso de la API de hospedaje XAML de UWP en una aplicación de escritorio](/windows/apps/desktop/modernize/using-the-xaml-hosting-api) .

### <a name="make-code-portable-across-windowing-hosts"></a>Crear código portable entre los hosts de ventanas

Cuando el contenido XAML se muestra en [CoreWindow](/uwp/api/windows.ui.core.corewindow), siempre hay una [ventana](/uwp/api/windows.ui.xaml.window) [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) y XAML asociada. Puede usar las API en estas clases para obtener información como los límites de la ventana. Para recuperar una instancia de estas clases, se usa el método estático [CoreWindow. GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) , el método [ApplicationView. GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) o la propiedad [window. Current](/uwp/api/windows.ui.xaml.window.current) . Además, hay muchas clases que usan el `GetForCurrentView` patrón para recuperar una instancia de la clase, como [DisplayInformation. GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview).

Estas API funcionan porque solo hay un único árbol de contenido XAML para un CoreWindow/ApplicationView, por lo que el XAML conoce el contexto en el que se hospeda es ese CoreWindow/ApplicationView.

Cuando el contenido XAML se ejecuta dentro de un AppWindow o DesktopWindowXamlSource, puede tener varios árboles de contenido XAML que se ejecuten en el mismo subproceso al mismo tiempo. En este caso, estas API no proporcionan la información correcta, ya que el contenido ya no se ejecuta en la ventana actual de CoreWindow/ApplicationView (y la ventana XAML).

Para asegurarse de que el código funciona correctamente en todos los hosts de ventanas, debe reemplazar las API que se basan en [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)y [Window](/uwp/api/windows.ui.xaml.window) con nuevas API que obtienen su contexto de la clase [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) .
La clase XamlRoot representa un árbol de contenido XAML e información sobre el contexto en el que se hospeda, ya sea CoreWindow, AppWindow o DesktopWindowXamlSource. Esta capa de abstracción permite escribir el mismo código independientemente del host de ventanas en el que se ejecute el XAML.

En esta tabla se muestra el código que no funciona correctamente entre los hosts de ventanas y el nuevo código portable con el que se puede reemplazar, así como algunas API que no es necesario cambiar.

| Si ha usado... | Reemplazar por... |
| - | - |
| CoreWindow. GetForCurrentThread (). [Límites](/uwp/api/windows.ui.core.corewindow.bounds) de | _uiElement_. XamlRoot. [Tamaño](/uwp/api/windows.ui.xaml.xamlroot.size) de |
| CoreWindow. GetForCurrentThread (). [SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_. XamlRoot. [Cambiado](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow. [Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_. XamlRoot. [IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow. [VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_. XamlRoot. [Cambiado](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow. GetForCurrentThread (). [GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Sin cambios. Esto se admite en AppWindow y DesktopWindowXamlSource. |
| CoreWindow. GetForCurrentThread (). [GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Sin cambios. Esto se admite en AppWindow y DesktopWindowXamlSource. |
| Ventana. [Actual](/uwp/api/windows.ui.xaml.window.current) | Devuelve el objeto de ventana XAML principal que está estrechamente enlazado al CoreWindow actual. Vea la nota después de esta tabla. |
| Window. Current. [Límites](/uwp/api/windows.ui.xaml.window.bounds) de | _uiElement_. XamlRoot. [Tamaño](/uwp/api/windows.ui.xaml.xamlroot.size) de |
| Window. Current. [Contenido](/uwp/api/windows.ui.xaml.window.content) de | UIElement raíz = _UIElement_. XamlRoot. [Contenido](/uwp/api/windows.ui.xaml.xamlroot.content) de |
| Window. Current. [Compositor](/uwp/api/windows.ui.xaml.window.compositor) | Sin cambios. Esto se admite en AppWindow y DesktopWindowXamlSource. |
| VisualTreeHelper. [GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>En las aplicaciones de las islas XAML, se producirá un error. En las aplicaciones de AppWindow, se devolverán los elementos emergentes abiertos en la ventana principal. | VisualTreeHelper. [GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot) (_uiElement_. XamlRoot) |
| FocusManager. [GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager. [GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_) (_uiElement_. XamlRoot) |
| contentDialog.ShowAsync() | contentDialog. [XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot)  =  _uiElement_. XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout. ShowAt (NULL, nuevo punto (10, 10)); | menuFlyout. [XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot)  =  _uiElement_. XamlRoot;<br/>menuFlyout. ShowAt (NULL, nuevo punto (10, 10)); |

> [!NOTE]
> En el caso de contenido XAML en una DesktopWindowXamlSource, existe una ventana CoreWindow/en el subproceso, pero siempre es invisible y tiene un tamaño de 1 x 1. Todavía es accesible para la aplicación, pero no devolverá límites o visibilidad significativos.
>
>En el caso de contenido XAML en un AppWindow, siempre habrá exactamente un CoreWindow en el mismo subproceso. Si llama a una `GetForCurrentView` API `GetForCurrentThread` o, esa API devolverá un objeto que refleja el estado de CoreWindow en el subproceso, no de ninguno de los AppWindows que se pueden estar ejecutando en ese subproceso.


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- No proporciones un punto de entrada clara a la vista secundaria usando el glifo de "abrir ventana nueva".
- No comuniques el objetivo de la vista secundaria a los usuarios.
- Asegúrese de que la aplicación sea totalmente funcional en una sola vista y que los usuarios abran una vista secundaria solo para mayor comodidad.
- No confíes en la vista secundaria para proporcionar notificaciones u otros tipos de visualizaciones transitorias.

## <a name="related-topics"></a>Temas relacionados

- [Usar AppWindow](app-window.md)
- [Usar ApplicationView](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
