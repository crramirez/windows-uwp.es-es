---
description: En este artículo se describen escenarios avanzados de hospedaje de C++ la isla XAML para aplicaciones Win32.
title: Escenarios avanzados para Islas XAML en C++ aplicaciones Win32
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, Islas XAML, controles ajustados, controles estándar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226239"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Escenarios avanzados para Islas XAML en C++ aplicaciones Win32

Los artículos [hospedar un control estándar de UWP](host-standard-control-with-xaml-islands-cpp.md) y [hospedar un control de UWP personalizado](host-custom-control-with-xaml-islands-cpp.md) proporcionan instrucciones y ejemplos para hospedar islas XAML en una C++ aplicación Win32. Sin embargo, los ejemplos de código de estos artículos no controlan muchos escenarios avanzados que es posible que las aplicaciones de escritorio deban controlar para proporcionar una experiencia de usuario fluida. En este artículo se proporcionan instrucciones para algunos de estos escenarios y punteros a ejemplos de código relacionados.

## <a name="keyboard-input"></a>Entrada de teclado

Para controlar correctamente la entrada del teclado para cada isla XAML, la aplicación debe pasar todos los mensajes de Windows al marco XAML de UWP para que ciertos mensajes se puedan procesar correctamente. Para ello, en algún lugar de la aplicación que pueda tener acceso al bucle de mensajes, convierta el objeto **DesktopWindowXamlSource** de cada isla XAML en una interfaz com **IDesktopWindowXamlSourceNative2** . A continuación, llame al método **PreTranslateMessage** de esta interfaz y pase el mensaje actual.

  * Win32:: la aplicación puede llamar a **PreTranslateMessage** directamente en su bucle de mensajes principal. **C++** Para obtener un ejemplo, vea el archivo [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16) .

  * **WPF:** La aplicación puede llamar a **PreTranslateMessage** desde el controlador de eventos para el evento [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) . Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.Focus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) en el kit de herramientas de la comunidad de Windows.

  * **Windows Forms:** La aplicación puede llamar a **PreTranslateMessage** desde una invalidación para el método [control. PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage) . Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.KeyboardFocus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) en el kit de herramientas de la comunidad de Windows.

## <a name="keyboard-focus-navigation"></a>Navegación con el foco de teclado

Cuando el usuario navega por los elementos de la interfaz de usuario de la aplicación mediante el teclado (por ejemplo, presionando la tecla **Tab** o dirección/flecha), tendrá que trasladar el foco al objeto **DesktopWindowXamlSource** mediante programación. Cuando la navegación del teclado del usuario alcance el **DesktopWindowXamlSource**, mueva el foco al primer objeto [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en el orden de navegación de la interfaz de usuario, continúe moviendo el foco a los siguientes objetos de **Windows. UI. Xaml. UIElement** cuando el usuario recorra los elementos y, a continuación, mueva el foco de **DesktopWindowXamlSource** y al elemento de la interfaz de usuario principal.  

La API de hospedaje XAML de UWP proporciona varios tipos y miembros para ayudarle a llevar a cabo estas tareas.

* Cuando la navegación del teclado entra en el **DesktopWindowXamlSource**, se genera el evento [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) . Controle este evento y mueva mediante programación el foco al primer host **Windows. UI. Xaml. UIElement** hospedado mediante el método [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Cuando el usuario está en el último elemento enfocable en el **DesktopWindowXamlSource** y presiona la tecla **Tab** o una tecla de dirección, se genera el evento [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) . Controla este evento y mueve mediante programación el foco al siguiente elemento enfocable en la aplicación host. Por ejemplo, en una aplicación WPF donde **DesktopWindowXamlSource** se hospeda en [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puede usar el método [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir el foco al siguiente elemento enfocable en la aplicación host.

Para obtener ejemplos que muestran cómo hacerlo en el contexto de una aplicación de ejemplo funcional, vea los siguientes archivos de código:

  * /Win32: vea el archivo [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) .  **C++**

  * **WPF:** Vea el archivo [WindowsXamlHostBase.Focus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) en el kit de herramientas de la comunidad de Windows.  

  * **Windows Forms:** Vea el archivo [WindowsXamlHostBase.KeyboardFocus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="handle-layout-changes"></a>Controlar los cambios de diseño

Cuando el usuario cambie el tamaño del elemento de la interfaz de usuario principal, deberá controlar los cambios de diseño necesarios para asegurarse de que los controles de UWP se muestran como se espera. Estos son algunos escenarios importantes que se deben tener en cuenta.

* En una C++ aplicación de Win32, cuando la aplicación controla el mensaje de WM_SIZE, puede cambiar la posición de la isla XAML hospedada mediante la función [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Para obtener un ejemplo, vea el archivo de código [SampleApp. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170) .

* Cuando el elemento primario de la interfaz de usuario debe obtener el tamaño del área rectangular necesaria para ajustarse a **Windows. UI. Xaml. UIElement** que hospeda en **DesktopWindowXamlSource**, llame al método [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows. UI. Xaml. UIElement**. Por ejemplo:

    * En una aplicación de WPF podría hacerlo desde el método [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) del [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación Windows Forms puede hacer esto desde el método [getPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) del [control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

* Cuando cambie el tamaño del elemento primario de la interfaz de usuario, llame al método [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la raíz **Windows. UI. Xaml. UIElement** que hospeda en **DesktopWindowXamlSource**. Por ejemplo:

    * En una aplicación de WPF podría hacerlo desde el método [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) del objeto [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHost.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación Windows Forms puede hacer esto desde el controlador del evento [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) del [control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHost.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="handle-dpi-changes"></a>Controlar los cambios de PPP

El marco XAML de UWP administra automáticamente los cambios de PPP para los controles de UWP hospedados (por ejemplo, cuando el usuario arrastra la ventana entre monitores con distintos PPP de pantalla). Para disfrutar de la mejor experiencia, se recomienda que la aplicación Windows Forms, C++ WPF o Win32 esté configurada para que sea compatible con PPP por monitor.

Para configurar la aplicación para que tenga en cuenta el valor de PPP por monitor, agregue un [manifiesto de ensamblado en paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y establezca el elemento **\<DpiAwareness\>** en **PerMonitorV2**. Para obtener más información acerca de este valor, vea la descripción de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="related-topics"></a>Temas relacionados

* [Hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)](xaml-islands.md)
* [Uso de la API de hospedaje XAML de C++ UWP en una aplicación Win32](using-the-xaml-hosting-api.md)
* [Hospedar un control estándar de UWP C++ en una aplicación Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedar un control personalizado de UWP C++ en una aplicación Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Ejemplos de código de islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++Ejemplo de islas XAML de Win32](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
