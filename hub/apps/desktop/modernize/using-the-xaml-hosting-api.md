---
description: This article describes how to host UWP XAML UI in your desktop C++ Win32 app.
title: Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, win32, islas xaml
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 36c3aeb7a51c84e92c5bca461aee7efe50740237
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80218465"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++

A partir de Windows 10, versión 1903, las aplicaciones de escritorio que no son de UWP (incluidas las aplicaciones Win32 en C++, WPF y Windows Forms) pueden usar la *API de hospedaje XAML para UWP* para hospedar controles de UWP en cualquier elemento de la interfaz de usuario que esté asociado a un identificador de ventana (HWND). Esta API permite que las aplicaciones de escritorio que no son de UWP usen las últimas características de la interfaz de usuario de Windows 10 que solo están disponibles mediante los controles de UWP. Por ejemplo, las aplicaciones de escritorio que no son de UWP pueden usar esta API para hospedar controles de UWP que usan el [Sistema Fluent Design](/windows/uwp/design/fluent-design-system/index) y admiten [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

La API de hospedaje XAML para UWP proporciona la base para un conjunto más amplio de controles que se proporcionan para permitir que los desarrolladores incorporen la interfaz de usuario de Fluent a aplicaciones de escritorio que no son de UWP. Esta característica se conoce como *islas XAML*. Para obtener información general sobre esta característica, consulta [Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md).

> [!NOTE]
> Si tienes comentarios sobre las islas XAML, crea un nuevo problema en el [repositorio de Microsoft.Toolkit.Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deja los comentarios allí. Si prefieres enviar los comentarios de forma privada, puedes enviarlos a XamlIslandsFeedback@microsoft.com. Tus aportes y escenarios son muy importantes para nosotros.

## <a name="is-the-uwp-xaml-hosting-api-the-right-choice-for-your-desktop-app"></a>¿La API de hospedaje XAML para UWP es la opción adecuada para tu aplicación de escritorio?

La API de hospedaje XAML para UWP proporciona la infraestructura de bajo nivel para hospedar controles de UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la opción de usar API alternativas más adecuadas para lograr este objetivo.

* Si tienes una aplicación de escritorio Win32 de C++ nativa y quieres hospedar los controles de UWP en tu aplicación, debes usar la API de hospedaje XAML para UWP. No hay alternativas para estos tipos de aplicaciones.

* En el caso de las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente usar los [controles .NET de las islas XAML](xaml-islands.md#wpf-and-windows-forms-applications) en el kit de herramientas de la comunidad de Windows en lugar de usar directamente la API de hospedaje XAML para UWP. Estos controles usan la API de hospedaje XAML para UWP internamente e implementan todo el comportamiento que, de lo contrario, tendrías que controlar tú mismo si usaras la API de hospedaje XAML para UWP directamente, incluida la navegación con el teclado y los cambios de diseño.

Dado que se recomienda que solo las aplicaciones Win32 de C++ usen la API de hospedaje XAML para UWP, en este artículo principalmente se proporcionan instrucciones y ejemplos para aplicaciones Win32 de C++. Sin embargo, puedes usar la API de hospedaje XAML para UWP en aplicaciones de WPF y Windows Forms si lo deseas. En este artículo se hace referencia al código fuente pertinente para los [controles host](xaml-islands.md#host-controls) para WPF y Windows Forms en el kit de herramientas de la comunidad de Windows, de modo que puedas ver cómo dichos controles usan la API de hospedaje XAML para UWP.

## <a name="learn-how-to-use-the-xaml-hosting-api"></a>Información sobre cómo usar la API de hospedaje XAML

Para seguir las instrucciones detalladas con ejemplos de código para usar la API de hospedaje XAML en aplicaciones Win32 de C++, consulta estos artículos:

* [Hospedaje de un control estándar de UWP](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedaje de un control personalizado de UWP](host-custom-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados](advanced-scenarios-xaml-islands-cpp.md)

## <a name="samples"></a>Ejemplos

La forma de usar la API de hospedaje XAML para UWP en el código depende del tipo de aplicación, el diseño de la aplicación y otros factores. Para ayudar a ilustrar el uso de esta API en el contexto de una aplicación completa, en este artículo se hace referencia al código de los ejemplos siguientes.

### <a name="c-win32"></a>Win32 de C++

Los ejemplos siguientes demuestran cómo usar la API de hospedaje XAML para UWP en una aplicación Win32 de C++:

* [Ejemplo sencillo de islas XAML](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App). En este ejemplo se muestra una implementación básica del hospedaje de un control de UWP en una aplicación Win32 de C++ desempaquetada (es decir, una aplicación que no está integrada en un paquete MSIX).

* [Ejemplo de islas XAML con controles personalizados](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32). En este ejemplo se muestra una implementación completa del hospedaje de un control de UWP personalizado en una aplicación Win32 de C++ empaquetada, así como el control de otro comportamiento, como la entrada de teclado y la navegación del foco.

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

El control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) del kit de herramientas de la comunidad de Windows sirve como ejemplo de referencia para usar la API de hospedaje de UWP en aplicaciones de WPF y Windows Forms. El código fuente se encuentra disponible en las siguientes ubicaciones:

* Para la versión de WPF del control, [haz clic aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión de WPF deriva de [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Para la versión de Windows Forms del control, [haz clic aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La versión de Windows Forms deriva de [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

> [!NOTE]
> Se recomienda encarecidamente usar los [controles .NET de las islas XAML](xaml-islands.md#wpf-and-windows-forms-applications) en el kit de herramientas de la comunidad de Windows en lugar de usar directamente la API de hospedaje XAML para UWP en aplicaciones de WPF y Windows Forms. Los vínculos e ejemplo de WPF y Windows Forms de este artículo se ofrecen solo para fines ilustrativos.

## <a name="architecture-of-the-api"></a>Arquitectura de la API

La API de hospedaje XAML para UWP incluye estos tipos principales de Windows Runtime e interfaces COM.

|  Tipo o interfaz | Descripción |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Esta clase representa el marco XAML de UWP. Esta clase proporciona un solo método estático [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) que inicializa el marco XAML de UWP en el subproceso actual en la aplicación de escritorio. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Esta clase representa una instancia de contenido XAML de UWP que se hospeda en la aplicación de escritorio. El miembro más importante de esta clase es la propiedad [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content). Esta propiedad se asigna a un elemento [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que se desea hospedar. Esta clase también tiene otros miembros para la navegación del foco de teclado de enrutamiento dentro y fuera de las islas XAML. |
| IDesktopWindowXamlSourceNative | Esta interfaz COM proporciona el método **AttachToWindow**, que se usa para adjuntar una isla XAML en la aplicación a un elemento primario de la interfaz de usuario. Cada objeto **DesktopWindowXamlSource** implementa esta interfaz. Esta interfaz se declara en windows.ui.xaml.hosting.desktopwindowxamlsource.h. |
| IDesktopWindowXamlSourceNative2 | Esta interfaz COM proporciona el método **PreTranslateMessage**, que permite al marco XAML de UWP procesar correctamente determinados mensajes de Windows. Cada objeto **DesktopWindowXamlSource** implementa esta interfaz. Esta interfaz se declara en windows.ui.xaml.hosting.desktopwindowxamlsource.h. |

En el diagrama siguiente se muestra la jerarquía de objetos de una isla XAML que se hospeda en una aplicación de escritorio.

* En el nivel base, es el elemento de la interfaz de usuario de la aplicación donde deseas hospedar la isla XAML. Este elemento de la interfaz de usuario debe tener un identificador de ventana (HWND). Entre los ejemplos de elementos de interfaz de usuario en los que puedes hospedar una isla XAML de C++, se incluyen una [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para las aplicaciones Win32 de C++, un elemento [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para las aplicaciones de WPF y un elemento [System.Windows.Forms.Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para las aplicaciones de Windows Forms.

* En el siguiente nivel, se encuentra un objeto **DesktopWindowXamlSource**. Este objeto proporciona la infraestructura para hospedar la isla XAML. El código es responsable de crear este objeto y adjuntarlo al elemento primario de la interfaz de usuario.

* Al crear un elemento **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativa para hospedar el control de UWP. Esta ventana secundaria nativa se abstrae en su mayoría del código, pero puedes acceder a su identificador (HWND) si es necesario.

* Por último, en el nivel superior se encuentra el control de UWP que quieres hospedar en la aplicación de escritorio. Puede ser cualquier objeto de UWP que se derive de [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP proporcionado por Windows SDK, así como controles de usuario personalizados.

![Arquitectura de DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

> [!NOTE]
> Al hospedar islas XAML en una aplicación de escritorio, puedes tener varios árboles de contenido XAML que se ejecuten en el mismo subproceso a la vez. Para acceder al elemento raíz de un árbol de contenido XAML en una isla XAML y obtener la información relacionada sobre el contexto en el que se hospeda, usa la clase [XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot). Las API [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow), [ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) y [Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window) no proporcionan la información correcta para las islas XAML. Para obtener más información, consulta [esta sección](xaml-islands.md#window-host-context-for-xaml-islands).

## <a name="troubleshooting"></a>Solucionar problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al usar la API de hospedaje XAML para UWP en una aplicación para UWP

| Problema | Solución |
|-------|------------|
| La aplicación recibe un elemento **COMException** con el siguiente mensaje: "No se puede activar DesktopWindowXamlSource. Este tipo no se puede usar en una aplicación para UWP" o "No se puede activar WindowsXamlManager. Este tipo no se puede usar en una aplicación para UWP". | Este error indica que estás intentando usar la API de hospedaje XAML para UWP (en concreto, estás intentando crear una instancia de los tipos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)) en una aplicación para UWP. La API de hospedaje XAML para UWP solo está pensada para usarse en aplicaciones de escritorio que no son de UWP, como aplicaciones de WPF, de Windows Forms y Win32 de C++. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Error al intentar usar los tipos WindowsXamlManager o DesktopWindowXamlSource

| Problema | Solución |
|-------|------------|
| La aplicación recibe una excepción con el siguiente mensaje: "WindowsXamlManager y DesktopWindowXamlSource se admiten para aplicaciones destinadas a Windows 10.0.18226.0 y posteriores. Comprueba el manifiesto de aplicación o el manifiesto de paquete y asegúrate de que la propiedad MaxTestedVersion esté actualizada". | Este error indica que la aplicación intentó usar los tipos **WindowsXamlManager** o **DesktopWindowXamlSource** de la API de hospedaje XAML para UWP, pero el sistema operativo no puede determinar si la aplicación se ha creado para tener como destino Windows 10, versión 1903, o posterior. La API de hospedaje XAML para UWP se presentó por primera vez como una versión preliminar en una versión anterior de Windows 10, pero solo se admite a partir de Windows 10, versión 1903.</p></p>Para resolver este problema, crea un paquete MSIX para la aplicación y ejecútala desde el paquete, o bien instala el paquete NuGet [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) en el proyecto.  |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Error al adjuntar a una ventana en un subproceso diferente

| Problema | Solución |
|-------|------------|
| La aplicación recibe un elemento **COMException** con el siguiente mensaje: "Error en el método AttachToWindow porque el HWND especificado se creó en un subproceso diferente". | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative::AttachToWindow** y le pasó el HWND de una ventana creada en un subproceso diferente. Debes pasar a este método el HWND de una ventana creada en el mismo subproceso que el código desde el que llamas al método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al adjuntar a una ventana en una ventana de primer nivel distinta

| Problema | Solución |
|-------|------------|
| La aplicación recibe un elemento **COMException** con el siguiente mensaje: "Error en el método AttachToWindow porque el HWND especificado desciende de una ventana de primer nivel distinta de la del HWND que se pasó previamente a AttachToWindow en el mismo subproceso". | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative::AttachToWindow** y le pasó el HWND de una ventana que desciende de una ventana de primer nivel distinta de la ventana especificada en una llamada anterior a este método en el mismo subproceso.</p></p>Una vez que la aplicación llama a **AttachToWindow** en un subproceso determinado, todos los demás objetos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) del mismo subproceso solo pueden asociarse a ventanas que son descendientes de la misma ventana de primer nivel que se pasó en la primera llamada a **AttachToWindow**. Cuando se cierran todos los objetos **DesktopWindowXamlSource** para un subproceso determinado, el siguiente elemento **DesktopWindowXamlSource** es gratis para volver a asociarlo a cualquier ventana.</p></p>Para resolver este problema, cierra todos los objetos **DesktopWindowXamlSource** que estén enlazados a otras ventanas de primer nivel en este subproceso, o crea un subproceso para este elemento **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md)
* [Hospedaje de un control estándar de UWP en una aplicación Win32 de C++](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedaje de un control personalizado de UWP en una aplicación Win32 de C++](host-custom-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados para islas XAML en aplicaciones Win32 en C++](advanced-scenarios-xaml-islands-cpp.md)
* [Ejemplos de código de las islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [Ejemplo de islas XAML en Win32 de C++](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
