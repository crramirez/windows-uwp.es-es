---
description: En este artículo se describe cómo hospedar la interfaz de usuario XAML de UWP en la aplicación de escritorio.
title: Uso de la API de hospedaje XAML de UWP en una aplicación de escritorio
ms.date: 07/26/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, Islas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 14607dc3e3b32f39a840d623c7a887bb8b3687c5
ms.sourcegitcommit: f6af7aeb8506379a184207035c8e43288cb31453
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68601532"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Uso de la API de hospedaje XAML de UWP en una aplicación de escritorio

A partir de Windows 10, versión 1903, las aplicaciones de escritorio que no son de UWP (incluidas C++ las aplicaciones WPF, Windows Forms y Win32) pueden usar la *API de hospedaje XAML de UWP* para hospedar controles de UWP en cualquier elemento de la interfaz de usuario que esté asociado a un identificador de ventana (HWND). Esta API permite a las aplicaciones de escritorio que no son de UWP usar las últimas características de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles de UWP. Por ejemplo, las aplicaciones de escritorio que no son de UWP pueden usar esta API para hospedar controles de UWP que usan el [sistema de diseño fluida](/windows/uwp/design/fluent-design-system/index) y admiten [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

La API de hospedaje XAML de UWP proporciona la base para un conjunto más amplio de controles que se proporcionan para permitir que los desarrolladores incorporen la interfaz de usuario fluida a aplicaciones de escritorio que no son de UWP. Esta característica se denomina *islas XAML*. Para obtener información general sobre esta característica, consulte [controles de UWP en aplicaciones de escritorio](xaml-islands.md).

> [!NOTE]
> Si tiene comentarios sobre las islas XAML, cree un nuevo problema en el [repositorio Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deje los comentarios allí. Si prefiere enviar sus comentarios de forma privada, puede enviarlos a XamlIslandsFeedback@microsoft.com. Sus conocimientos y escenarios son muy importantes para nosotros.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>¿Debe usar la API de hospedaje XAML de UWP?

La API de hospedaje XAML de UWP proporciona la infraestructura de bajo nivel para hospedar controles de UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la opción de usar API alternativas más adecuadas para lograr este objetivo.  

* Si tiene una C++ aplicación de escritorio de Win32 y desea hospedar controles de UWP en la aplicación, debe usar la API de hospedaje XAML de UWP. No hay alternativas para estos tipos de aplicaciones.

* En el caso de las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente que use los [controles ajustados](xaml-islands.md#wrapped-controls) y los [controles host](xaml-islands.md#host-controls) en el kit de herramientas de la comunidad de Windows en lugar de usar directamente la API de hospedaje XAML de UWP. Estos controles usan internamente la API de hospedaje XAML de UWP e implementan todo el comportamiento que, de lo contrario, tendría que administrarse si usara la API de hospedaje XAML de UWP directamente, incluidos los cambios de diseño y navegación del teclado. Sin embargo, puede usar la API de hospedaje XAML de UWP directamente en estos tipos de aplicaciones si lo desea.

En este artículo se describe cómo usar la API de hospedaje XAML de UWP directamente en la aplicación en lugar de los controles que proporciona el kit de herramientas de la comunidad de Windows.

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje XAML de UWP tiene estos requisitos previos:

* Windows 10, versión 1903 (o posterior) y la compilación correspondiente del Windows SDK.
* Configure el proyecto para usar Windows Runtime API y habilitar islas XAML siguiendo [estas instrucciones](xaml-islands.md#requirements).

## <a name="architecture-of-xaml-islands"></a>Arquitectura de islas XAML

La API de hospedaje XAML de UWP incluye estos principales tipos de Windows Runtime e interfaces COM:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Esta clase representa el marco XAML de UWP. Esta clase proporciona un método [**InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) estático único que inicializa el marco XAML de UWP en el subproceso actual en la aplicación de escritorio.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Esta clase representa una instancia de contenido XAML de UWP que se hospeda en la aplicación de escritorio. El miembro más importante de esta clase es la propiedad de [**contenido**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Esta propiedad se asigna a un [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que desea hospedar. Esta clase también tiene otros miembros para la navegación del foco de teclado de enrutamiento dentro y fuera de las islas XAML.

* Interfaces com **IDesktopWindowXamlSourceNative** y **IDesktopWindowXamlSourceNative2** . **IDesktopWindowXamlSourceNative** proporciona el método **AttachToWindow** , que se usa para adjuntar una isla XAML en la aplicación a un elemento primario de la interfaz de usuario. **IDesktopWindowXamlSourceNative2** proporciona el método **PreTranslateMessage** , que permite que el marco XAML de UWP procese correctamente determinados mensajes de Windows.
    > [!NOTE]
    > Estas interfaces se declaran en el archivo de encabezado **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h** en el Windows SDK. De forma predeterminada, este archivo se encuentra en% ProgramFiles (x86)% \\\Windows Kits\10\Include <\>número de compilación \um. En un C++ proyecto de Win32, puede hacer referencia directamente a este archivo de encabezado. En un proyecto de WPF o Windows Forms, debe declarar las interfaces en el código de aplicación mediante el atributo [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) . Asegúrese de que las declaraciones de interfaz coinciden exactamente con las declaraciones de **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h**.

En el diagrama siguiente se muestra la jerarquía de objetos de una isla XAML que se hospeda en una aplicación de escritorio.

![Arquitectura de DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

* En el nivel base, es el elemento de la interfaz de usuario de la aplicación donde desea hospedar la isla XAML. Este elemento de la interfaz de usuario debe tener un identificador de ventana (HWND). Ejemplos de elementos de la interfaz de usuario en los que puede hospedar una isla de XAML: [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicaciones WPF, [System. Windows. Forms. Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para C++ aplicaciones Windows Forms y una [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para Win32 programas.

* El siguiente nivel es un objeto **DesktopWindowXamlSource** . Este objeto proporciona la infraestructura para hospedar la isla XAML. El código es responsable de crear este objeto y adjuntarlo al elemento primario de la interfaz de usuario.

* Cuando se crea un **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativa para hospedar el control de UWP. Esta ventana secundaria nativa se abstrae principalmente del código, pero se puede tener acceso a su identificador (HWND) si es necesario.

* Por último, en el nivel superior se encuentra el control de UWP que quiere hospedar en la aplicación de escritorio. Puede ser cualquier objeto UWP que se derive de [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP proporcionado por el Windows SDK, así como los controles de usuario personalizados.

## <a name="related-samples"></a>Muestras relacionadas

La forma de usar la API de hospedaje XAML de UWP en el código depende del tipo de aplicación, el diseño de la aplicación y otros factores. Para ayudar a ilustrar el uso de esta API en el contexto de una aplicación completa, en este artículo se hace referencia al código de los ejemplos siguientes.

### <a name="c-win32"></a>C++32

Ejemplo de Win32. [ C++ ](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App) En este ejemplo se muestra una implementación completa de que hospeda un control de usuario de UWP C++ en una aplicación Win32 desempaquetada (es decir, una aplicación que no está integrada en un paquete MSIX).

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

El control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows actúa como ejemplo de referencia para usar la API de hospedaje de UWP en WPF y en aplicaciones Windows Forms. El código fuente está disponible en las siguientes ubicaciones:

  * Para la versión de WPF del control, [vaya aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión de WPF deriva de [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

  * Para obtener la versión Windows Forms del control, [vaya aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La versión Windows Forms se deriva de [**System. Windows. Forms. control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-uwp-xaml-controls"></a>Controles XAML de UWP de host

Estos son los pasos principales para usar la API de hospedaje XAML de UWP para hospedar un control de UWP en la aplicación.

1. Inicialice el marco XAML de UWP para el subproceso actual antes de que la aplicación cree cualquiera de los objetos [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que hospedará en [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Si la aplicación crea el objeto **DesktopWindowXamlSource** antes de crear cualquiera de los objetos **Windows. UI. Xaml. UIElement** , este marco de trabajo se inicializará automáticamente al crear una instancia del objeto **DesktopWindowXamlSource** . En este escenario, no es necesario agregar ningún código propio para inicializar el marco de trabajo.

    * Sin embargo, si la aplicación crea los objetos **Windows. UI. Xaml. UIElement** antes de crear el objeto **DesktopWindowXamlSource** que los hospedará, la aplicación debe llamar al método estático [ **. Método WindowsXamlManager. InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) para inicializar explícitamente el marco XAML de UWP antes de que se creen instancias de los objetos **Windows. UI. Xaml. UIElement** . Normalmente, la aplicación debe llamar a este método cuando se crea una instancia del elemento de la interfaz de usuario principal que hospeda el **DesktopWindowXamlSource** .

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Este método devuelve un objeto [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contiene una referencia al marco XAML de UWP. Puede crear tantos objetos **WindowsXamlManager** como desee en un subproceso determinado. Sin embargo, dado que cada objeto contiene una referencia al marco XAML de UWP, debe eliminar los objetos para asegurarse de que se liberen los recursos XAML.

2. Cree un objeto [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) y asócielo a un elemento primario de la interfaz de usuario de la aplicación que esté asociada a un identificador de ventana.

    Para ello, debe seguir estos pasos:

    1. Cree un objeto **DesktopWindowXamlSource** y conviértalo en la interfaz com **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** .

    2. Llame al método **AttachToWindow** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** y pase el identificador de ventana del elemento primario de la interfaz de usuario en la aplicación.

    3. Establezca el tamaño inicial de la ventana secundaria interna contenida en **DesktopWindowXamlSource**. De forma predeterminada, esta ventana secundaria interna se establece en un ancho y un alto de 0. Si no establece el tamaño de la ventana, los controles de UWP que agregue a **DesktopWindowXamlSource** no estarán visibles. Para tener acceso a la ventana secundaria interna de **DesktopWindowXamlSource**, use la propiedad **WindowHandle** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** . En los ejemplos siguientes se usa la función [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para establecer el tamaño de la ventana.

    Estos son algunos ejemplos de código que muestran este proceso.

    ```cppwinrt
    // This example assumes you already have an HWND variable named 'parentHwnd' that
    // contains the handle of the parent window.
    Windows::UI::Xaml::Hosting::DesktopWindowXamlSource desktopWindowXamlSource;
    auto interop = desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
    check_hresult(interop->AttachToWindow(parentHwnd));

    HWND childInteropHwnd = nullptr;
    interop->get_WindowHandle(&childInteropHwnd);

    SetWindowPos(childInteropHwnd, 0, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

    ```csharp
    // This WPF example assumes you already have an HwndHost named 'parentHwndHost'
    // that will act as the parent UI element for your XAML Island. It also assumes
    // you have used the DllImport attribute to import SetWindowPos from user32.dll
    // as a static method into a class named NativeMethods.
    Windows.UI.Xaml.Hosting.DesktopWindowXamlSource desktopWindowXamlSource =
        new Windows.UI.Xaml.Hosting.DesktopWindowXamlSource();

    IntPtr iUnknownPtr = System.Runtime.InteropServices.Marshal.GetIUnknownForObject(
        desktopWindowXamlSource);
    IDesktopWindowXamlSourceNative desktopWindowXamlSourceNative =
        System.Runtime.InteropServices.Marshal.Marshal.GetTypedObjectForIUnknown(
            iUnknownPtr, typeof(IDesktopWindowXamlSourceNative))
            as IDesktopWindowXamlSourceNative;

    desktopWindowXamlSourceNative.AttachToWindow(parentHwndHost.Handle);

    var childInteropHwnd = desktopWindowXamlSourceNative.WindowHandle;
    NativeMethods.SetWindowPos(childInteropHwnd, HWND_TOP, 0, 0, 300, 300, SWP_SHOWWINDOW);
    ```

3. Establezca el valor de **Windows. UI. Xaml. UIElement** que quiere hospedar en la propiedad [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) del objeto **DesktopWindowXamlSource** . En el ejemplo siguiente se establece un [**Windows. UI. Xaml. Controls. Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) denominado ```myGrid``` para la propiedad de **contenido** .

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Para obtener ejemplos completos que muestran estas tareas en el contexto de una aplicación de ejemplo funcional, vea los siguientes archivos de código:

  * **C++32** Vea el archivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WPF** Consulte los archivos [WindowsXamlHostBase.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.  

  * **Windows Forms:** Consulte los archivos [WindowsXamlHostBase.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Controlar la navegación de entrada y foco del teclado

Hay varias cosas que debe hacer la aplicación para controlar correctamente la entrada del teclado y centrar la navegación cuando hospeda islas XAML.

### <a name="keyboard-input"></a>Entrada de teclado

Para controlar correctamente la entrada del teclado para cada isla XAML, la aplicación debe pasar todos los mensajes de Windows al marco XAML de UWP para que ciertos mensajes se puedan procesar correctamente. Para ello, en algún lugar de la aplicación que pueda tener acceso al bucle de mensajes, convierta el objeto **DesktopWindowXamlSource** de cada isla XAML en una interfaz com **IDesktopWindowXamlSourceNative2** . A continuación, llame al método **PreTranslateMessage** de esta interfaz y pase el mensaje actual.

  * Una C++ aplicación Win32 puede llamar a **PreTranslateMessage** directamente en su bucle de mensajes principal. Para obtener un ejemplo, vea el archivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * Una aplicación WPF puede llamar a **PreTranslateMessage** desde el controlador de eventos para el evento [**ComponentDispatcher. ThreadFilterMessage**](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) . Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.Focus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) en el kit de herramientas de la comunidad de Windows.

  * Una aplicación Windows Forms puede llamar a **PreTranslateMessage** desde una invalidación para el método [**control. PreprocessMessage**](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) . Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.KeyboardFocus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) en el kit de herramientas de la comunidad de Windows.

### <a name="keyboard-focus-navigation"></a>Navegación con el foco de teclado

Cuando el usuario navega por los elementos de la interfaz de usuario de la aplicación mediante el teclado (por ejemplo, presionando la tecla **Tab** o dirección/flecha), tendrá que trasladar el foco al objeto **DesktopWindowXamlSource** mediante programación. Cuando la navegación del teclado del usuario alcance el **DesktopWindowXamlSource**, mueva el foco al primer objeto [**Windows. UI. Xaml. UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en el orden de navegación de la interfaz de usuario, continúe moviendo el foco a lo siguiente  **Los objetos Windows. UI. Xaml. UIElement** a medida que el usuario recorre los elementos y, a continuación, mueve el foco de nuevo fuera de **DesktopWindowXamlSource** y al elemento de la interfaz de usuario principal.  

La API de hospedaje XAML de UWP proporciona varios tipos y miembros para ayudarle a llevar a cabo estas tareas.

* Cuando la navegación del teclado entra en el **DesktopWindowXamlSource**, se genera el evento [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) . Controle este evento y mueva mediante programación el foco al primer host **Windows. UI. Xaml. UIElement** hospedado mediante el método [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Cuando el usuario está en el último elemento enfocable en el **DesktopWindowXamlSource** y presiona la tecla **Tab** o una tecla de dirección, se genera el evento [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) . Controla este evento y mueve mediante programación el foco al siguiente elemento enfocable en la aplicación host. Por ejemplo, en una aplicación WPF donde **DesktopWindowXamlSource** se hospeda en [**System. Windows. Interop. HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puede usar el método [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir el foco al siguiente elemento enfocable en la aplicación host.

Para obtener ejemplos que muestran cómo hacerlo en el contexto de una aplicación de ejemplo funcional, vea los siguientes archivos de código:

  * **WPF** Vea el archivo [WindowsXamlHostBase.Focus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) en el kit de herramientas de la comunidad de Windows.  

  * **Windows Forms:** Vea el archivo [WindowsXamlHostBase.KeyboardFocus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) en el kit de herramientas de la comunidad de Windows.

  * **C++/Win32**: Vea el archivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

## <a name="handle-layout-changes"></a>Controlar los cambios de diseño

Cuando el usuario cambie el tamaño del elemento de la interfaz de usuario principal, deberá controlar los cambios de diseño necesarios para asegurarse de que los controles de UWP se muestran como se espera. Estos son algunos escenarios importantes que se deben tener en cuenta.

* En una C++ aplicación Win32, cuando la aplicación controla el mensaje WM_SIZE, puede cambiar la posición de la isla XAML hospedada mediante la función [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Para obtener un ejemplo, vea el archivo de código [SampleApp. cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Cuando el elemento primario de la interfaz de usuario debe obtener el tamaño del área rectangular necesaria para ajustarse a **Windows. UI. Xaml. UIElement** que hospeda en **DesktopWindowXamlSource**, llame al método [**Measure**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows. UI. Xaml. UIElement.** . Por ejemplo:

    * En una aplicación de WPF podría hacerlo desde el método [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) del [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación Windows Forms puede hacer esto desde el método [**getPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) del [**control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

* Cuando cambie el tamaño del elemento primario de la interfaz de usuario, llame al método [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la raíz **Windows. UI. Xaml. UIElement** que hospeda en **DesktopWindowXamlSource**. Por ejemplo:

    * En una aplicación de WPF podría hacerlo desde el método [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) del objeto [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHost.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación Windows Forms puede hacer esto desde el controlador del evento [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) del [**control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHost.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="handle-dpi-changes"></a>Controlar los cambios de PPP

El marco XAML de UWP administra automáticamente los cambios de PPP para los controles de UWP hospedados (por ejemplo, cuando el usuario arrastra la ventana entre monitores con distintos PPP de pantalla). Para disfrutar de la mejor experiencia, se recomienda que la aplicación Windows Forms, C++ WPF o Win32 esté configurada para que sea compatible con PPP por monitor.

Para configurar la aplicación para que tenga en cuenta el valor de PPP por monitor, agregue un manifiesto de ensamblado en [paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto ```<dpiAwareness>``` y establezca ```PerMonitorV2```el elemento en. Para obtener más información sobre este valor, vea la descripción de [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Hospedar controles XAML de UWP personalizados

Siga estos pasos generales para hospedar un control XAML de UWP personalizado (ya sea un control definido por el usuario o un control proporcionado por un tercero) en una isla XAML. Debe tener el código fuente del control personalizado para que pueda compilarlo con la aplicación.

1. En la solución de aplicación host, integre el código fuente para el control XAML de UWP personalizado y compile el control personalizado.

2. Agregue un proyecto de aplicación para UWP a la solución de aplicación host y agregue el paquete de NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) a este proyecto.

3. En el proyecto de aplicación de UWP, `App` modifique la clase para que sea de la clase **Microsoft. Toolkit. Win32. UI. XamlApplication** expuesta por el paquete NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) . Este tipo actúa como proveedor de metadatos raíz para cargar los metadatos de los tipos XAML de UWP personalizados en los ensamblados del directorio actual de la aplicación.

4. En el constructor de la `App` clase, llame al método **Initialize** de la clase base **Microsoft. Toolkit. Win32. UI. XamlApplication** .

5. En el proyecto de la aplicación host, siga el proceso descrito en la [sección anterior](#host-uwp-xaml-controls) para hospedar el control personalizado en una isla XAML.

Para obtener un ejemplo de C++ una aplicación Win32, vea los siguientes proyectos en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App):

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): Este proyecto implementa un control XAML personalizado de UWP denominado `MyUserControl` que contiene un cuadro de texto, varios botones y un cuadro combinado.
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): Se trata de un proyecto de aplicación para UWP con los cambios descritos en los pasos anteriores.
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): Este es el C++ proyecto de aplicación de Win32 que hospeda el control XAML de UWP personalizado en una isla XAML.

Para obtener instrucciones detalladas y ejemplos de una aplicación de WPF o Windows Forms, consulte [estas instrucciones](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al usar la API de hospedaje XAML de UWP en una aplicación de UWP

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "No se puede activar DesktopWindowXamlSource. Este tipo no se puede usar en una aplicación de UWP ". o "no se puede activar WindowsXamlManager. Este tipo no se puede usar en una aplicación de UWP ". | Este error indica que está intentando usar la API de hospedaje XAML de UWP (específicamente, está intentando crear una instancia de los tipos [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) en una aplicación para UWP. La API de hospedaje XAML de UWP solo está pensada para usarse en aplicaciones de escritorio que no son de UWP, como C++ WPF, Windows Forms y aplicaciones Win32. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Error al asociar a una ventana en un subproceso diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "Error en el método AttachToWindow porque el HWND especificado se creó en un subproceso diferente". | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative:: AttachToWindow** y le pasó el HWND de una ventana creada en un subproceso diferente. Debe pasar este método el HWND de una ventana creada en el mismo subproceso que el código desde el que llama al método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al asociar a una ventana en una ventana de nivel superior diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "Error en el método AttachToWindow porque el HWND especificado desciende de una ventana de nivel superior distinta de la del HWND que se pasó previamente a AttachToWindow en el mismo subproceso." | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative:: AttachToWindow** y le pasó el HWND de una ventana que desciende de una ventana de nivel superior diferente de la ventana especificada en una llamada anterior a este método. en el mismo subproceso.</p></p>Una vez que la aplicación llama a **IDesktopWindowXamlSourceNative:: AttachToWindow** en un subproceso determinado, todos los demás objetos [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) del mismo subproceso solo pueden asociarse a ventanas que sean descendientes del mismo nivel superior. ventana que se pasó en la primera llamada a **IDesktopWindowXamlSourceNative:: AttachToWindow**. Cuando se cierran todos los objetos **DesktopWindowXamlSource** para un subproceso determinado, el siguiente **DesktopWindowXamlSource** se vuelve a adjuntar a cualquier ventana de nuevo.</p></p>Para resolver este problema, cierre todos los objetos **DesktopWindowXamlSource** enlazados a otras ventanas de nivel superior en este subproceso o cree un nuevo subproceso para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Controles de UWP en aplicaciones de escritorio](xaml-islands.md)
* [C++Ejemplo de islas XAML de Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
