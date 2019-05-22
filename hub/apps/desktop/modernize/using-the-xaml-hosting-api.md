---
description: Este artículo describe cómo hospedar la interfaz de usuario de XAML para UWP en su aplicación de escritorio.
title: Utilizando el XAML UWP API de hospedaje en una aplicación de escritorio
ms.date: 04/16/2019
ms.topic: article
keywords: Windows 10, uwp, formularios windows forms, wpf, win32, Islas de xaml
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9b318b922541180108dfdd053ba28ce98ad9ebcb
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985015"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Utilizando el XAML UWP API de hospedaje en una aplicación de escritorio

A partir de Windows 10, versión 1903, aplicaciones de escritorio no de UWP (incluido WPF, Windows Forms, y C++ aplicaciones Win32) puede usar el *XAML UWP API de hospedaje* para hospedar controles de UWP en cualquier elemento de interfaz de usuario que está asociado con un identificador de ventana (HWND). Esta API permite que las aplicaciones de escritorio no de UWP usar las características más recientes de la interfaz de usuario de Windows 10 que solo están disponibles a través de controles UWP. Por ejemplo, las aplicaciones de escritorio no de UWP pueden usar esta API para hospedar controles de UWP que usan el [sistema de diseño Fluent](/windows/uwp/design/fluent-design-system/index) y soporte técnico [Windows Ink](/windows/uwp/design/pen-and-stylus-interactions).

El XAML UWP API de hospedaje proporciona la base para un conjunto más amplio de controles que ofrecemos para permitir a los desarrolladores incorporar la interfaz de usuario Fluent a que no son de UWP de aplicaciones de escritorio. Esta característica se denomina *XAML Islas*. Para obtener información general de esta característica, consulte [controles UWP en aplicaciones de escritorio](xaml-islands.md).

> [!NOTE]
> Si tiene comentarios acerca de las Islas de XAML, cree un nuevo problema en el [Microsoft.Toolkit.Win32 repositorio](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deje sus comentarios no existe. Si prefiere enviar sus comentarios de forma privada, puede enviarlo a XamlIslandsFeedback@microsoft.com. Sus opiniones y escenarios son muy importantes para nosotros.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>¿Debe usar el XAML UWP API de hospedaje?

El XAML UWP API de hospedaje proporciona la infraestructura de bajo nivel para hospedar controles UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la opción de usar la API alternativas más convenientes para lograr este objetivo.  

* Si tiene una aplicación de escritorio de Win32 de C++ y desea para hospedar controles de UWP en la aplicación, debe usar el XAML UWP API de hospedaje. No hay ninguna alternativa para estos tipos de aplicaciones.

* Para las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente que utilice el [ajusta los controles](xaml-islands.md#wrapped-controls) y [controles host](xaml-islands.md#host-controls) en el Kit de herramientas de comunidad de Windows en lugar de usar el XAML UWP directamente de la API de hospedaje. Estos controles usar el XAML UWP internamente en la API de hospedaje e implementan todo el comportamiento en caso contrario, deberá administrar usted mismo si usa el XAML UWP API directamente, incluidos los cambios de navegación y el diseño de teclado de hospedaje. Sin embargo, puede usar el XAML UWP API directamente en estos tipos de aplicaciones de hospedaje si elige.

En este artículo se describe cómo usar el XAML UWP API de hospedaje directamente en la aplicación en lugar de los controles que proporciona el Kit de herramientas de la Comunidad de Windows.

## <a name="prerequisites"></a>Requisitos previos

El XAML UWP API de hospedaje tiene estos requisitos previos:

* Windows 10, versión 1903 (o posterior) y la correspondiente de compilación del SDK de Windows.
* Configurar el proyecto para usar Windows Runtime APIs y habilitar XAML Islas siguiendo [estas instrucciones](xaml-islands.md#requirements).

## <a name="architecture-of-xaml-islands"></a>Arquitectura de islas de XAML

El XAML UWP API de hospedaje incluye estos tipos principales de Windows en tiempo de ejecución y las interfaces COM:

* [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager). Esta clase representa el marco de trabajo de UWP XAML. Esta clase proporciona un único estático [ **InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) método que inicializa el marco de trabajo de UWP XAML en el subproceso actual en la aplicación de escritorio.

* [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource). Esta clase representa una instancia del contenido de UWP XAML que hospedan en su aplicación de escritorio. El miembro más importante de esta clase es el [ **contenido** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) propiedad. Asignar esta propiedad en un [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que desea hospedar. Esta clase también tiene otros miembros para enrutamiento navegación del foco de teclado dentro y fuera de las Islas de XAML.

* **IDesktopWindowXamlSourceNative** y **IDesktopWindowXamlSourceNative2** interfaces COM. **IDesktopWindowXamlSourceNative** proporciona el **AttachToWindow** método, que se usa para adjuntar una isla de XAML en la aplicación a un elemento de interfaz de usuario primario. **IDesktopWindowXamlSourceNative2** proporciona el **PreTranslateMessage** método, que permite que el marco de trabajo de UWP XAML procesar determinados mensajes de Windows correctamente.
    > [!NOTE]
    > Estas interfaces se declaran en el **windows.ui.xaml.hosting.desktopwindowxamlsource.h** archivo de encabezado en el SDK de Windows. De forma predeterminada, este archivo se encuentra en % programfiles (x86) %\Windows Kits\10\Include\\< número de compilación\>\um. En un proyecto Win32 de C++, puede hacer referencia directamente este archivo de encabezado. En un proyecto WPF o Windows Forms, debe declarar las interfaces en el código de aplicación utilizando el [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) atributo. Asegúrese de que las declaraciones de interfaz coinciden exactamente con las declaraciones de **windows.ui.xaml.hosting.desktopwindowxamlsource.h**.

El siguiente diagrama muestra la jerarquía de objetos en una isla de XAML que se hospeda en una aplicación de escritorio.

![Arquitectura de DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

* En el nivel de base es el elemento de interfaz de usuario en la aplicación donde desea hospedar la isla de XAML. Este elemento de interfaz de usuario debe tener un identificador de ventana (HWND). Ejemplos de elementos de interfaz de usuario en el que puede hospedar una isla de XAML [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para las aplicaciones de WPF, [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicaciones de Windows Forms y un [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para C++ aplicaciones Win32.

* En el siguiente nivel es un **DesktopWindowXamlSource** objeto. Este objeto proporciona la infraestructura para hospedar la isla de XAML. El código es responsable de crear este objeto y asociarlo al elemento de interfaz de usuario primario.

* Cuando creas un **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativo para hospedar el control UWP. Esta ventana secundaria nativo principalmente se abstrae el código, pero puede tener acceso a su controlador (HWND) si es necesario.

* Por último, en el nivel superior es el control UWP que desea hospedar en su aplicación de escritorio. Esto puede ser cualquier objeto UWP que se derive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluyendo cualquier control que UWP proporcionada el SDK de Windows, así como los controles de usuario personalizados.

## <a name="related-samples"></a>Muestras relacionadas

La forma de usar el XAML UWP API de hospedaje en el código depende de su tipo de aplicación, el diseño de la aplicación y otros factores. Para ayudar a ilustrar cómo se usa esta API en el contexto de una aplicación completa, este artículo se refiere al código de los ejemplos siguientes.

### <a name="c-win32"></a>C++ Win32

[C++Ejemplo de Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island). Este ejemplo muestra una implementación completa de hospedar un control de usuario UWP en un desempaquetará C++ Win32 (es decir, una aplicación que no está integrado en un paquete MSIX).

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

El [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control en el Kit de herramientas de la Comunidad Windows actúa como un ejemplo de referencia para usar la API de hospedaje en aplicaciones WPF y Windows Forms UWP. El código fuente está disponible en las siguientes ubicaciones:

  * Para la versión WPF del control, [acá](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión de WPF se deriva de [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

  * Para la versión de Windows Forms del control, [acá](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La versión de Windows Forms se deriva de [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-uwp-xaml-controls"></a>Los controles host UWP XAML

Estos son los pasos principales para usar el XAML UWP API de hospedaje para hospedar un control UWP en la aplicación.

1. Inicializar el marco de trabajo de UWP XAML para el subproceso actual antes de que la aplicación cree cualquiera de los [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objetos que va a hospedar el [  **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Si la aplicación crea el **DesktopWindowXamlSource** objeto antes de crear cualquiera de los **Windows.UI.Xaml.UIElement** objetos, este marco se inicializará automáticamente al crear instancias de la **DesktopWindowXamlSource** objeto. En este escenario, no es necesario agregar su propio para inicializar el marco de código.

    * Sin embargo, si la aplicación crea el **Windows.UI.Xaml.UIElement** objetos antes de crear el **DesktopWindowXamlSource** objeto que se va a hospedar, la aplicación debe llamar a estático[ **WindowsXamlManager.InitializeForCurrentThread** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) método para inicializar explícitamente el marco de trabajo de UWP XAML antes de la **Windows.UI.Xaml.UIElement** son objetos crea una instancia. La aplicación normalmente debe llamar a este método cuando el elemento de interfaz de usuario principal que hospeda el **DesktopWindowXamlSource** se crea una instancia.

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Este método devuelve un [ **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) objeto que contiene una referencia a la plataforma UWP XAML. Puede crear tantos **WindowsXamlManager** objetos como desee en un subproceso determinado. Sin embargo, dado que cada objeto contiene una referencia a la plataforma UWP XAML, debe eliminar los objetos para asegurarse de que finalmente se liberan los recursos XAML.

2. Crear un [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) objeto y adjuntarlo a un elemento de interfaz de usuario primario de la aplicación que está asociado con un identificador de ventana.

    Para ello, necesita seguir estos pasos:

    1. Crear un **DesktopWindowXamlSource** objeto y conviértalo a la **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** interfaz COM.

    2. Llame a la **AttachToWindow** método de la **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** interfaz y pase el identificador de ventana de la elemento de interfaz de usuario principal de la aplicación.

    3. Establezca el tamaño inicial de la ventana de secundaria internos contenido en el **DesktopWindowXamlSource**. De forma predeterminada, esta ventana secundaria interno se establece en un ancho y alto igual a 0. Si no establece el tamaño de la ventana, todos los controles UWP se agrega a la **DesktopWindowXamlSource** no serán visibles. Para obtener acceso a la ventana secundaria interno en el **DesktopWindowXamlSource**, use el **WindowHandle** propiedad de la **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** interfaz. Los ejemplos siguientes usan la [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) función para establecer el tamaño de la ventana.

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

3. Establecer el **Windows.UI.Xaml.UIElement** desea hospedar a la [ **contenido** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) propiedad de su **DesktopWindowXamlSource** objeto. El ejemplo siguiente se establece un [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) denominado ```myGrid``` a la **contenido** propiedad.

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Para obtener ejemplos completos que demuestran estas tareas en el contexto de una aplicación de ejemplo funcional, consulte los archivos de código siguiente:

  * **C++ Win32:** Consulte la [XamlBridge.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/XamlBridge.cpp) de archivos en el [ C++ ejemplo Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * **WPF:** Consulte la [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) archivos en el Kit de herramientas de la Comunidad de Windows.  

  * **Windows Forms:** Consulte la [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) archivos en el Kit de herramientas de la Comunidad de Windows.

## <a name="handle-keyboard-input-and-focus-navigation"></a>Controlar la navegación de entrada y el foco de teclado

Hay varias cosas que necesita la aplicación para controlar correctamente la navegación de entrada y el foco de teclado cuando hospeda islas de XAML.

### <a name="keyboard-input"></a>Entrada de teclado

Para controlar correctamente la entrada de teclado para cada isla de XAML, la aplicación debe pasar todos los mensajes de Windows para el marco de trabajo de UWP XAML para que se puedan procesar determinados mensajes correctamente. Para ello, en algún lugar en la aplicación que pueda tener acceso el bucle de mensajes, puede convertir el **DesktopWindowXamlSource** objeto para cada isla de XAML para un **IDesktopWindowXamlSourceNative2** interfaz COM. A continuación, llame a la **PreTranslateMessage** método de esta interfaz y pase el mensaje actual.

  * Un C++ puede llamar la aplicación Win32 **PreTranslateMessage** directamente en su bucle de mensajes principal. Para obtener un ejemplo, vea el [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L61) archivo de código en el [ C++ ejemplo Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

  * Puede llamar una aplicación de WPF **PreTranslateMessage** desde el controlador de eventos para el [ **ComponentDispatcher.ThreadFilterMessage** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage?view=netframework-4.7.2) eventos. Para obtener un ejemplo, vea el [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) archivo en el Kit de herramientas de la Comunidad de Windows.

  * Puede llamar una aplicación de Windows Forms **PreTranslateMessage** desde una invalidación para el [ **Control.PreprocessMessage** ](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage?view=netframework-4.7.2) método. Para obtener un ejemplo, vea el [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) archivo en el Kit de herramientas de la Comunidad de Windows.

### <a name="keyboard-focus-navigation"></a>Navegación del foco de teclado

Cuando el usuario navega por los elementos de interfaz de usuario de la aplicación mediante el teclado (por ejemplo, presionando **ficha** o tecla de dirección o dirección), deberá mover el foco mediante programación dentro y fuera de la  **DesktopWindowXamlSource** objeto. Cuando llega la navegación con el teclado del usuario la **DesktopWindowXamlSource**, mover el foco en la primera [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objeto en el orden de navegación para la interfaz de usuario, continúe mover el foco al siguiente **Windows.UI.Xaml.UIElement** ciclos de objetos como el usuario a través de los elementos y vuelva a mover el foco de la **DesktopWindowXamlSource**y en el elemento de interfaz de usuario primario.  

El XAML UWP API de hospedaje proporciona varios tipos y miembros para ayudarle a realizar estas tareas.

* Cuando entra en el panel de navegación teclado su **DesktopWindowXamlSource**, [ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) provoca el evento. Controle este evento y mediante programación mover el foco al primer hospedado **Windows.UI.Xaml.UIElement** utilizando el [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) método.

* Cuando el usuario está en el último elemento enfocable en el **DesktopWindowXamlSource** y presiona la **ficha** clave o una tecla de dirección, el [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) provoca el evento. Controle este evento y mover el foco al siguiente elemento enfocable situado en la aplicación host mediante programación. Por ejemplo, en una aplicación WPF donde el **DesktopWindowXamlSource** se hospeda en un [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puede usar el [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) método para transferir el foco al siguiente elemento enfocable situado en la aplicación host.

Para obtener ejemplos que muestran cómo hacer esto en el contexto de una aplicación de ejemplo funcional, consulte los archivos de código siguiente:

  * **WPF:** Consulte la [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) archivo en el Kit de herramientas de la Comunidad de Windows.  

  * **Windows Forms:** Consulte la [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) archivo en el Kit de herramientas de la Comunidad de Windows.

## <a name="handle-layout-changes"></a>Controlar los cambios de diseño

Cuando el usuario cambia el tamaño del elemento de interfaz de usuario principal, deberá controlar los cambios de diseño necesarias para asegurarse de que los controles UWP mostrar según lo esperado. Estos son algunos escenarios importantes a tener en cuenta.

* En un C++ aplicación Win32, cuando la aplicación controla el mensaje WM_SIZE puede cambiar la posición de la isla de XAML hospedado mediante el uso de la [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) función. Para obtener un ejemplo, vea el [SampleApp.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) archivo de código en el [ C++ ejemplo Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Cuando el elemento de interfaz de usuario primario debe obtener el tamaño del área rectangular que sea necesario para ajustarse el **Windows.UI.Xaml.UIElement** que hospedan en el **DesktopWindowXamlSource**, llame a la [ **Medida** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) método de la **Windows.UI.Xaml.UIElement**. Por ejemplo:

    * En una aplicación WPF podría hacer esto desde el [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) método de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda el  **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) archivo en el Kit de herramientas de la Comunidad de Windows.

    * En una aplicación de Windows Forms podría hacer esto desde el [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) método de la [ **Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda el **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) archivo en el Kit de herramientas de la Comunidad de Windows.

* Cuando se cambia el tamaño del elemento de interfaz de usuario primario, llame a la [ **organizar** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) método de la raíz **Windows.UI.Xaml.UIElement** que hospedan en el  **DesktopWindowXamlSource**. Por ejemplo:

    * En una aplicación WPF podría hacer esto desde el [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) método de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) objeto que hospeda el **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) archivo en el Kit de herramientas de la Comunidad de Windows.

    * En una aplicación de Windows Forms podría hacer esto desde el controlador para el [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) eventos de la [ **Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda el **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) archivo en el Kit de herramientas de la Comunidad de Windows.

## <a name="handle-dpi-changes"></a>Controlar los cambios de PPP

El marco de trabajo de UWP XAML controla automáticamente los cambios de PPP para controles hospedados de UWP (por ejemplo, cuando el usuario arrastra la ventana entre monitores con valores de PPP de pantalla diferentes). Para obtener la mejor experiencia, se recomienda que los formularios de Windows, WPF, o C++ aplicación Win32 está configurado para ser compatible con PPP por monitor.

Para configurar la aplicación sea compatible con PPP por monitor, agregue un [manifiesto del ensamblado en paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y establezca ```<dpiAwareness>``` elemento ```PerMonitorV2```. Para obtener más información acerca de este valor, vea la descripción de [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="host-custom-uwp-xaml-controls"></a>Controles de host personalizado UWP XAML

> [!IMPORTANT]
> Para hospedar un control personalizado de UWP XAML, debe tener el código fuente para el control, por lo que puede compilar en él en la aplicación.

Si desea hospedar un control personalizado de UWP XAML (un control que define usted mismo o un control proporcionado por un 3rd party), debe realizar las siguientes tareas adicionales además del proceso se describe en el [sección anterior](#host-uwp-xaml-controls).

1. Define un tipo personalizado que se derive de [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) y también implementa [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Este tipo actúa como un proveedor de metadatos de raíz para cargar los metadatos para los tipos personalizados de UWP XAML en los ensamblados en el directorio actual de la aplicación.

2. Llame a la [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) método de su proveedor de metadatos de raíz cuando se asigna el nombre de tipo del control UWP XAML (Esto podría asignarse en el código en tiempo de ejecución, o puede elegir habilitar esta opción para ser asignar en la ventana Propiedades de Visual Studio).

    Para obtener ejemplos, vea los archivos de código siguiente:
      * **C++ Win32:** Consulte la [XamlApplication.cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup/XamlApplication.cpp) archivo de código en el [ C++ ejemplo Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

      * **WPF y Windows Forms**: Consulte la [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) y [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) archivos en el Kit de herramientas de la Comunidad de Windows de código. Estos archivos forman parte de la implementación compartida de la **WindowsXamlHost** clases de WPF y Windows Forms, que ayudan a ilustrar cómo se usa el XAML UWP en esos tipos de aplicaciones de la API de hospedaje.

3. Integrar el código fuente para el control personalizado de UWP XAML en su solución de aplicación de host, compilar el control personalizado y usarlo en la aplicación. Para obtener instrucciones para una aplicación WPF o Windows Forms, consulte [estas instrucciones](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control). Para obtener un ejemplo de un C++ aplicación Win32, vea el [Microsoft.UI.Xaml.Markup](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/Microsoft.UI.Xaml.Markup) y [MyApp](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island/MyApp) proyectos en el [ C++ ejemplo Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al utilizar XAML UWP API de hospedaje en una aplicación para UWP

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "No se puede activar DesktopWindowXamlSource. Este tipo no se puede usar en una aplicación para UWP." o "no se puede activar WindowsXamlManager. Este tipo no se puede usar en una aplicación para UWP." | Este error indica que está intentando usar el XAML UWP API de hospedaje (en concreto, se intenta crear una instancia de la [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) tipos) en una aplicación para UWP. El XAML UWP API de hospedaje sólo está pensado para usarse en aplicaciones de escritorio no de UWP, como las aplicaciones de WPF, Windows Forms y Win32 de C++. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Error al adjuntar a una ventana en un subproceso diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "Método AttachToWindow error porque se creó el HWND especificado en un subproceso diferente". | Este error indica que la aplicación llama a la **IDesktopWindowXamlSourceNative::AttachToWindow** método y lo ha pasado el HWND de una ventana que se creó en un subproceso diferente. Este método se debe pasar el HWND de una ventana que se creó en el mismo subproceso que el código desde el que está llamando al método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al adjuntar a una ventana en una ventana de nivel superior diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "Método AttachToWindow error porque el HWND especificado desciende de una ventana de nivel superior diferente que el HWND que anteriormente se pasó a AttachToWindow en el mismo subproceso". | Este error indica que la aplicación llama a la **IDesktopWindowXamlSourceNative::AttachToWindow** método y lo ha pasado el HWND de una ventana que desciende de una ventana de nivel superior diferente que una ventana especificada en un llamada anterior a este método en el mismo subproceso.</p></p>Después de la aplicación llama **IDesktopWindowXamlSourceNative::AttachToWindow** en un subproceso determinado, todas las demás [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) objetos en el mismo subproceso solo se puede conectar a windows que son descendientes de la misma ventana de nivel superior que se pasó en la primera llamada a **IDesktopWindowXamlSourceNative::AttachToWindow**. Cuando todas las **DesktopWindowXamlSource** se cierran los objetos de un subproceso en particular, la siguiente **DesktopWindowXamlSource** entonces está libre para volver a adjuntar a cualquier ventana.</p></p>Para resolver este problema, cierre todos los **DesktopWindowXamlSource** objetos que están enlazados a otras ventanas de nivel superior en este subproceso, o crear un nuevo subproceso para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Controles UWP en aplicaciones de escritorio](xaml-islands.md)
* [C++Ejemplo de islas de XAML de Win32](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island)
