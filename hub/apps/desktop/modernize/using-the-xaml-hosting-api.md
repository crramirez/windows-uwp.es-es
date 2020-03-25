---
description: En este artículo se describe cómo hospedar la interfaz de usuario C++ XAML de UWP en la aplicación Win32 de escritorio.
title: Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, Islas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218465"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++

A partir de Windows 10, versión 1903, las aplicaciones de escritorio que no C++ son de UWP (incluidas las aplicaciones Win32, WPF y Windows Forms) pueden usar la *API de hospedaje XAML de UWP* para hospedar controles de UWP en cualquier elemento de la interfaz de usuario que esté asociado a un identificador de ventana (HWND). Esta API permite a las aplicaciones de escritorio que no son de UWP usar las últimas características de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles de UWP. Por ejemplo, las aplicaciones de escritorio que no son de UWP pueden usar esta API para hospedar controles de UWP que usan el [sistema de diseño fluida](/windows/uwp/design/fluent-design-system/index) y admiten [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

La API de hospedaje XAML de UWP proporciona la base para un conjunto más amplio de controles que se proporcionan para permitir que los desarrolladores incorporen la interfaz de usuario fluida a aplicaciones de escritorio que no son de UWP. Esta característica se denomina *islas XAML*. Para obtener información general sobre esta característica, consulte [controles XAML de UWP de host en aplicaciones de escritorio (Islas XAML)](xaml-islands.md).

> [!NOTE]
> Si tienes comentarios sobre las islas XAML, crea un nuevo problema en el [repositorio de Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deja los comentarios allí. Si prefieres enviar los comentarios de forma privada, puedes enviarlos a XamlIslandsFeedback@microsoft.com. Tus aportes y escenarios son muy importantes para nosotros.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>¿La API de hospedaje XAML de UWP es la opción adecuada para su aplicación de escritorio?

La API de hospedaje XAML de UWP proporciona la infraestructura de bajo nivel para hospedar controles de UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la opción de usar API alternativas más adecuadas para lograr este objetivo.

* Si tiene una C++ aplicación de escritorio de Win32 y desea hospedar controles de UWP en la aplicación, debe usar la API de hospedaje XAML de UWP. No hay alternativas para estos tipos de aplicaciones.

* En el caso de las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente usar los [controles .net de la isla XAML](xaml-islands.md#wpf-and-windows-forms-applications) en el kit de herramientas de la comunidad de Windows en lugar de usar directamente la API de hospedaje XAML de UWP. Estos controles usan internamente la API de hospedaje XAML de UWP e implementan todo el comportamiento que, de lo contrario, tendría que administrarse si usara la API de hospedaje XAML de UWP directamente, incluidos los cambios de diseño y navegación del teclado.

Dado que se recomienda que C++ solo las aplicaciones Win32 usen la API de hospedaje XAML de UWP, este artículo proporciona principalmente C++ instrucciones y ejemplos para aplicaciones Win32. Sin embargo, puede usar la API de hospedaje XAML de UWP en WPF y Windows Forms aplicaciones si lo desea. En este artículo se hace referencia al código fuente pertinente de los [controles host](xaml-islands.md#host-controls) para WPF y Windows Forms en el kit de herramientas de la comunidad de Windows para que pueda ver cómo esos controles usan la API de hospedaje XAML de UWP.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Aprenda a usar la API de hospedaje de XAML

Para seguir las instrucciones paso a paso con ejemplos de código para usar la API de hospedaje XAML C++ en aplicaciones Win32, consulte estos artículos:

* [Hospedar un control estándar de UWP](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar un control UWP personalizado](host-custom-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Muestras

La forma de usar la API de hospedaje XAML de UWP en el código depende del tipo de aplicación, el diseño de la aplicación y otros factores. Para ayudar a ilustrar el uso de esta API en el contexto de una aplicación completa, en este artículo se hace referencia al código de los ejemplos siguientes.

### <a name="c-win32"></a>C++32

En los siguientes ejemplos se muestra cómo usar la API de hospedaje XAML de C++ UWP en una aplicación Win32:

* [Ejemplo de isla XAML simple](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). En este ejemplo se muestra una implementación básica de que hospeda un control de UWP en C++ una aplicación Win32 desempaquetada (es decir, una aplicación que no está integrada en un paquete MSIX).

* [Isla XAML con el ejemplo de control personalizado](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). En este ejemplo se muestra una implementación completa de que hospeda un control de UWP personalizado C++ en una aplicación de Win32 empaquetada, así como el control de otro comportamiento, como la entrada del teclado y la navegación del foco.

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

El control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows sirve como ejemplo de referencia para usar la API de hospedaje de UWP en WPF y Windows Forms aplicaciones. El código fuente está disponible en las siguientes ubicaciones:

* Para la versión de WPF del control, [vaya aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión de WPF deriva de [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Para obtener la versión Windows Forms del control, [vaya aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La versión Windows Forms se deriva de [System. Windows. Forms. control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

> [!NOTE]
> Se recomienda encarecidamente usar los [controles .net de la isla XAML](xaml-islands.md#wpf-and-windows-forms-applications) en el kit de herramientas de la comunidad de Windows en lugar de usar la API de hospedaje XAML de UWP directamente en WPF y Windows Forms aplicaciones. Los vínculos de ejemplo de WPF y Windows Forms de este artículo son solo para fines ilustrativos.

## <a name="architecture-of-the-api"></a>Arquitectura de la API

La API de hospedaje XAML de UWP incluye estos principales tipos de Windows Runtime e interfaces COM.

|  Tipo o interfaz | Descripción |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Esta clase representa el marco XAML de UWP. Esta clase proporciona un método [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) estático único que inicializa el marco XAML de UWP en el subproceso actual en la aplicación de escritorio. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Esta clase representa una instancia de contenido XAML de UWP que se hospeda en la aplicación de escritorio. El miembro más importante de esta clase es la propiedad de [contenido](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Esta propiedad se asigna a un [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que desea hospedar. Esta clase también tiene otros miembros para la navegación del foco de teclado de enrutamiento dentro y fuera de las islas XAML. |
| IDesktopWindowXamlSourceNative | Esta interfaz COM proporciona el método **AttachToWindow** , que se usa para adjuntar una isla XAML en la aplicación a un elemento primario de la interfaz de usuario. Cada objeto **DesktopWindowXamlSource** implementa esta interfaz. Esta interfaz se declara en Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h. |
| IDesktopWindowXamlSourceNative2 | Esta interfaz COM proporciona el método **PreTranslateMessage** , que permite que el marco XAML de UWP procese correctamente determinados mensajes de Windows. Cada objeto **DesktopWindowXamlSource** implementa esta interfaz. Esta interfaz se declara en Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h. |

En el diagrama siguiente se muestra la jerarquía de objetos de una isla XAML que se hospeda en una aplicación de escritorio.

* En el nivel base, es el elemento de la interfaz de usuario de la aplicación donde desea hospedar la isla XAML. Este elemento de la interfaz de usuario debe tener un identificador de ventana (HWND). Algunos ejemplos de elementos de la interfaz de usuario en los que puede hospedar una C++ isla de XAML son una [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para las aplicaciones Win32, [System. Windows. Interop. HWNDHOST](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para las aplicaciones de WPF y [System. Windows. Forms. control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicaciones Windows Forms.

* El siguiente nivel es un objeto **DesktopWindowXamlSource** . Este objeto proporciona la infraestructura para hospedar la isla XAML. El código es responsable de crear este objeto y adjuntarlo al elemento primario de la interfaz de usuario.

* Cuando se crea un **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativa para hospedar el control de UWP. Esta ventana secundaria nativa se abstrae principalmente del código, pero se puede tener acceso a su identificador (HWND) si es necesario.

* Por último, en el nivel superior se encuentra el control UWP que quiere hospedar en la aplicación de escritorio. Puede ser cualquier objeto UWP que se derive de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP proporcionado por el Windows SDK, así como los controles de usuario personalizados.

![Arquitectura de DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Al hospedar islas XAML en una aplicación de escritorio, puedes tener varios árboles de contenido XAML que se ejecuten en el mismo subproceso a la vez. Para acceder al elemento raíz de un árbol de contenido XAML en una isla XAML y obtener la información relacionada sobre el contexto en el que se hospeda, usa la clase [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). Las API [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)y [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) no proporcionan la información correcta para las islas XAML. Para obtener más información, vea [esta sección ](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Solucionar problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al usar la API de hospedaje XAML de UWP en una aplicación de UWP

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "no se puede activar DesktopWindowXamlSource. Este tipo no se puede usar en una aplicación de UWP ". o "no se puede activar WindowsXamlManager. Este tipo no se puede usar en una aplicación de UWP ". | Este error indica que está intentando usar la API de hospedaje XAML de UWP (específicamente, está intentando crear una instancia de los tipos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) en una aplicación para UWP. La API de hospedaje XAML de UWP solo está pensada para usarse en aplicaciones de escritorio que no son de UWP, como C++ WPF, Windows Forms y aplicaciones Win32. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Error al intentar usar los tipos WindowsXamlManager o DesktopWindowXamlSource

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una excepción con el siguiente mensaje: "WindowsXamlManager y DesktopWindowXamlSource son compatibles con las aplicaciones que tienen como destino la versión de Windows 10.0.18226.0 y versiones posteriores. Compruebe el manifiesto de aplicación o el manifiesto del paquete y asegúrese de que la propiedad MaxTestedVersion esté actualizada. " | Este error indica que la aplicación intentó usar los tipos **WindowsXamlManager** o **DESKTOPWINDOWXAMLSOURCE** en la API de hospedaje XAML de UWP, pero el sistema operativo no puede determinar si la aplicación se ha creado para tener como destino Windows 10, versión 1903 o posterior. La API de hospedaje XAML de UWP se presentó por primera vez como una vista previa en una versión anterior de Windows 10, pero solo se admite a partir de Windows 10, versión 1903.</p></p>Para resolver este problema, cree un paquete de MSIX para la aplicación y ejecútelo desde el paquete, o bien instale el paquete NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) en el proyecto.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Error al asociar a una ventana en un subproceso diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "error en el método AttachToWindow porque el HWND especificado se creó en un subproceso diferente". | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative:: AttachToWindow** y le pasó el HWND de una ventana creada en un subproceso diferente. Debe pasar este método el HWND de una ventana creada en el mismo subproceso que el código desde el que llama al método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al asociar a una ventana en una ventana de nivel superior diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "error en el método AttachToWindow porque el HWND especificado desciende de una ventana de nivel superior distinta de la del HWND que se pasó previamente a AttachToWindow en el mismo subproceso". | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative:: AttachToWindow** y le pasó el HWND de una ventana que desciende de una ventana de nivel superior diferente de la ventana especificada en una llamada anterior a este método en el mismo subproceso.</p></p>Una vez que la aplicación llama a **AttachToWindow** en un subproceso determinado, todos los demás objetos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) del mismo subproceso solo pueden asociarse a ventanas que son descendientes de la misma ventana de nivel superior que se pasó en la primera llamada a **AttachToWindow**. Cuando se cierran todos los objetos **DesktopWindowXamlSource** para un subproceso determinado, el siguiente **DesktopWindowXamlSource** se vuelve a adjuntar a cualquier ventana de nuevo.</p></p>Para resolver este problema, cierre todos los objetos **DesktopWindowXamlSource** enlazados a otras ventanas de nivel superior en este subproceso o cree un nuevo subproceso para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)](xaml-islands.md)
* [Hospedar un control estándar de UWP C++ en una aplicación Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar un control personalizado de UWP C++ en una aplicación Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados para Islas XAML en C++ aplicaciones Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Ejemplos de código de islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Ejemplo de islas XAML de Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
