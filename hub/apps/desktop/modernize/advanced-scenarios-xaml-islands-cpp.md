---
description: En este artículo se describen escenarios avanzados de hospedaje de islas XAML para aplicaciones Win32 de C++.
title: Escenarios avanzados para islas XAML en aplicaciones Win32 de C++
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10;uwp;cpp;win32;xaml islands;wrapped controls;standard controls;islas XAML;controles ajustados;controles estándar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226239"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Escenarios avanzados para islas XAML en aplicaciones Win32 de C++

Los artículos sobre cómo [hospedar un control estándar de UWP](host-standard-control-with-xaml-islands-cpp.md) y [hospedar un control UWP personalizado](host-custom-control-with-xaml-islands-cpp.md) incluyen instrucciones y ejemplos para hospedar islas XAML en una aplicación Win32 de C++. Sin embargo, los ejemplos de código de estos artículos no controlan muchos escenarios avanzados que las aplicaciones de escritorio podrían necesitar para proporcionar una experiencia de usuario fluida. En este artículo se incluyen instrucciones para algunos de estos escenarios y se proporcionan vínculos a ejemplos de código relacionados.

## <a name="keyboard-input"></a>Entrada de teclado

Para controlar correctamente la entrada del teclado para cada isla XAML, la aplicación debe pasar todos los mensajes de Windows al marco XAML de UWP para que ciertos mensajes se puedan procesar correctamente. Para ello, en algún lugar de la aplicación que pueda acceder al bucle de mensajes, convierte el objeto **DesktopWindowXamlSource** de cada isla XAML en una interfaz COM **IDesktopWindowXamlSourceNative2**. A continuación, llama al método **PreTranslateMessage** de esta interfaz y pasa el mensaje actual.

  * **Win32 de C++:** La aplicación puede llamar a **PreTranslateMessage** directamente en el bucle de mensajes principal. Para ver un ejemplo, consulta el archivo [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16).

  * **WPF:** La aplicación puede llamar a **PreTranslateMessage** desde el controlador de eventos del evento [ComponentDispatcher.ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage). Para ver un ejemplo, consulta el archivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) en el kit de herramientas de la comunidad de Windows.

  * **Windows Forms:** La aplicación puede llamar a **PreTranslateMessage** desde una invalidación para el método [control.PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage). Para ver un ejemplo, consulta el archivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) en el kit de herramientas de la comunidad de Windows.

## <a name="keyboard-focus-navigation"></a>Navegación del foco con el teclado

Cuando el usuario navega por los elementos de la interfaz de usuario de la aplicación mediante el teclado (por ejemplo, al presionar la tecla **TAB** o las teclas de dirección), deberás mover el foco dentro y fuera del objeto **DesktopWindowXamlSource** mediante programación. Cuando la navegación del usuario mediante el teclado alcance al objeto **DesktopWindowXamlSource**, mueve el foco al primer objeto [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en el orden de navegación de la interfaz de usuario. Continúa moviendo el foco a los siguientes objetos **Windows.UI.Xaml.UIElement** a medida que el usuario recorra los elementos y, a continuación, quita el foco de **DesktopWindowXamlSource** y regresa al elemento de interfaz de usuario principal.  

La API de hospedaje XAML de UWP proporciona varios tipos y miembros para ayudarte a llevar a cabo estas tareas.

* Cuando la navegación del teclado entra en **DesktopWindowXamlSource**, se genera el evento [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus). Controla este evento y mueve mediante programación el foco al primer objeto **Windows.UI.Xaml.UIElement** mediante el método [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus).

* Cuando el usuario está en el último elemento activable de **DesktopWindowXamlSource** y presiona la tecla **TAB** o una tecla de dirección, se genera el evento [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested). Controla este evento y mueve mediante programación el foco al siguiente elemento activable de la aplicación host. Por ejemplo, en una aplicación WPF en la que el objeto **DesktopWindowXamlSource** está hospedado en una clase [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puedes usar el método [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir el foco al siguiente elemento activable de la aplicación host.

Para ver ejemplos que muestran cómo realizar estas acciones en el contexto de una aplicación de ejemplo funcional, consulta los siguientes archivos de código:

  * **C++/Win32**: Consulta el archivo [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp).

  * **WPF:** Consulta el archivo [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) en el kit de herramientas de la comunidad de Windows.  

  * **Windows Forms:** Consulta el archivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="handle-layout-changes"></a>Control de cambios de diseño

Cuando el usuario cambia el tamaño del elemento de la interfaz de usuario principal, debes controlar los cambios de diseño necesarios para asegurarte de que los controles de UWP se muestran según lo previsto. Estos son algunos escenarios importantes que se deben tener en cuenta.

* En una aplicación Win32 de C++, cuando la aplicación controla el mensaje de WM_SIZE, se puede cambiar la posición de la isla XAML hospedada mediante la función [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos). Para ver un ejemplo, consulta el archivo de código [SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170).

* Cuando el elemento principal de la interfaz de usuario necesita obtener el tamaño del área rectangular necesaria para ajustarse al objeto **Windows.UI.Xaml.UIElement** que hospedas en **DesktopWindowXamlSource**, llama al método [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows.UI.Xaml.UIElement**. Por ejemplo:

    * En una aplicación de WPF, puedes hacerlo desde el método [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de la clase [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda a **DesktopWindowXamlSource**. Para ver un ejemplo, consulta el archivo [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación de Windows Forms, puedes hacerlo desde el método [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) de la clase [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda a **DesktopWindowXamlSource**. Para ver un ejemplo, consulta el archivo [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

* Cuando se cambie el tamaño del elemento principal de la interfaz de usuario, llama al método [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) del objeto raíz **Windows.UI.Xaml.UIElement** que se hospeda en **DesktopWindowXamlSource**. Por ejemplo:

    * En una aplicación de WPF, puedes hacerlo desde el método [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) del objeto [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda a **DesktopWindowXamlSource**. Para ver un ejemplo, consulta el archivo [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación de Windows Forms, puedes hacerlo desde el controlador del evento [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) de la clase [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda a **DesktopWindowXamlSource**. Para ver un ejemplo, consulta el archivo [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="handle-dpi-changes"></a>Administración de los cambios de PPP

El marco XAML de UWP administra automáticamente los cambios de PPP para los controles de UWP hospedados (por ejemplo, cuando el usuario arrastra una ventana entre monitores con distintos PPP de pantalla). Para disfrutar de la mejor experiencia, se recomienda que la aplicación de Windows Forms, WPF o Win32 de C++ esté configurada para incluir reconocimiento de PPP por monitor.

Para realizar esta configuración en la aplicación, agrega un [manifiesto del ensamblado en paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y establece el elemento **\<dpiAwareness\>** en **PerMonitorV2**. Para obtener más información acerca de este valor, consulta la descripción de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

* [Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md)
* [Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++](using-the-xaml-hosting-api.md)
* [Hospedaje de un control estándar de UWP en una aplicación Win32 de C++](host-standard-control-with-xaml-islands-cpp.md)
* [Hospedaje de un control personalizado de UWP en una aplicación Win32 de C++](host-custom-control-with-xaml-islands-cpp.md)
* [Ejemplos de código de las islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [Ejemplo de islas XAML en Win32 de C++](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
