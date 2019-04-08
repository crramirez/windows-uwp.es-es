---
description: Este artículo describe cómo hospedar la interfaz de usuario de XAML para UWP en su aplicación de escritorio.
title: Utilizando el XAML UWP API de hospedaje en una aplicación de escritorio
ms.date: 01/11/2019
ms.topic: article
keywords: Windows 10, uwp, formularios windows forms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: efd7dc687b9aba2e3c07b0afefa2e4fa49b882b1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618840"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Utilizando el XAML UWP API de hospedaje en una aplicación de escritorio

> [!NOTE]
> Las Islas hospedaje XAML y API de UWP XAML están disponibles actualmente como versión preliminar para desarrolladores. Aunque le animamos a probarlas en su propio código del prototipo ahora, no se recomienda usar ellos en código de producción en este momento. Estas características seguirán maduren y estabilizarse en futuras versiones de Windows. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.
>
> Si tiene comentarios acerca de las Islas XAML, cree un nuevo problema en el [WindowsCommunityToolkit repositorio](https://github.com/windows-toolkit/WindowsCommunityToolkit/issues) y deje sus comentarios no existe. Si prefiere enviar sus comentarios de forma privada, puede enviarlo a XamlIslandsFeedback@microsoft.com. Sus opiniones y escenarios son muy importantes para nosotros.

A partir de SDK de Windows 10 Insider Preview compilación 17709, aplicaciones de escritorio no de UWP (incluidas las aplicaciones de WPF, Windows Forms y Win32 de C++) pueden usar el *XAML UWP API de hospedaje* para hospedar controles de UWP en cualquier elemento de interfaz de usuario que está asociado con un identificador de ventana (HWND). Esta API permite que las aplicaciones de escritorio no de UWP usar las características más recientes de la interfaz de usuario de Windows 10 que solo están disponibles a través de controles UWP. Por ejemplo, las aplicaciones de escritorio no de UWP pueden usar esta API para hospedar controles de UWP que usan el [sistema de diseño Fluent](../design/fluent-design-system/index.md) y soporte técnico [Windows Ink](../design/input/pen-and-stylus-interactions.md).

El XAML UWP API de hospedaje proporciona la base para un conjunto más amplio de controles que ofrecemos para permitir a los desarrolladores incorporar la interfaz de usuario Fluent a que no son de UWP de aplicaciones de escritorio. Este escenario se denomina a veces *Islas XAML*. Para obtener más detalles sobre este escenario para desarrolladores, consulte [controles UWP en aplicaciones de escritorio](xaml-host-controls.md).

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>¿Es el XAML UWP el API de hospedaje adecuado para su aplicación de escritorio?

El XAML UWP API de hospedaje proporciona la infraestructura de bajo nivel para hospedar controles UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la opción de usar la API alternativas más convenientes para lograr este objetivo.  

* Si tiene una aplicación de escritorio de Win32 de C++ y desea para hospedar controles de UWP en la aplicación, debe usar el XAML UWP API de hospedaje. No hay ninguna alternativa para estos tipos de aplicaciones.

* Para las aplicaciones de WPF y Windows Forms, se recomienda que use el [ajusta los controles](xaml-host-controls.md#wrapped-controls) y [controles host](xaml-host-controls.md#host-controls) en el Kit de herramientas de comunidad de Windows en lugar del XAML UWP API de hospedaje. Estos controles usar el XAML UWP internamente en la API de hospedaje y proporcionan una experiencia de desarrollo más sencilla. Sin embargo, puede usar el XAML UWP API directamente en estos tipos de aplicaciones de hospedaje si elige.

## <a name="related-samples"></a>Muestras relacionadas

La forma de usar el XAML UWP API de hospedaje en el código depende de su tipo de aplicación, el diseño de la aplicación y otros factores. Para ayudar a ilustrar cómo se usa esta API en el contexto de una aplicación completa, este artículo se refiere al código de los ejemplos siguientes.

### <a name="c-win32"></a>Win32 de C++

Hay varios ejemplos en GitHub que muestra cómo usar el XAML UWP API de hospedaje en una aplicación Win32 de C++:

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). Este ejemplo muestra cómo agregar UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), y [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) controles a una aplicación Win32 de C++.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). Este ejemplo muestra cómo agregar varios controles UWP básicos a una aplicación Win32 de C++ y controlar los cambios de PPP.

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

El [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) control en el Kit de herramientas de la Comunidad Windows actúa como un ejemplo de referencia para usar la API de hospedaje en aplicaciones WPF y Windows Forms UWP. El código fuente está disponible en las siguientes ubicaciones:

  * Para la versión WPF del control, [acá](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión de WPF se deriva de [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).
  * Para la versión de Windows Forms del control, [acá](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La versión de Windows Forms se deriva de [ **System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="prerequisites"></a>Requisitos previos

El XAML UWP API de hospedaje tiene estos requisitos previos.

* Compilación de Windows 10 Insider Preview 17709 (o una compilación posterior) y la compilación de Insider Preview correspondiente del SDK de Windows. Dado que ésta es una característica en constante evolución, para una mejor experiencia se recomienda usar la compilación más reciente disponible.

* Para usar el XAML UWP API de hospedaje en su aplicación de escritorio, deberá configurar el proyecto para que se pueden llamar a las API de UWP.

    * **Win32 de C++:** Le recomendamos que configure el proyecto para usar [C++ / c++ / WinRT](../cpp-and-winrt-apis/index.md). Para obtener instrucciones, consulte [modificar un proyecto de aplicación de escritorio de Windows para agregar C++ / c++ / WinRT soporte](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

    * **Windows Forms y WPF:** Siga [estas instrucciones](../porting/desktop-to-uwp-enhance.md).

## <a name="architecture-of-xaml-islands"></a>Arquitectura de islas XAML

Incluye el XAML UWP API de hospedaje [ **DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [ **WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)y otros tipos relacionados en el [ **Windows.UI.Xaml.Hosting** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) espacio de nombres. Las aplicaciones de escritorio pueden utilizar esta API para representar los controles UWP y ruta de navegación del foco de teclado dentro y fuera de los elementos. Las aplicaciones de escritorio también pueden cambiar el tamaño y colocar los controles UWP según sea necesario.

Cuando se crea una isla de XAML mediante la API de hospedaje en una aplicación de escritorio de XAML, tendrá la siguiente jerarquía de objetos:

* En el nivel de base es el elemento de interfaz de usuario en la aplicación donde desea hospedar la isla de XAML. Este elemento de interfaz de usuario debe tener un identificador de ventana (HWND). Ejemplos de elementos de interfaz de usuario en el que puede hospedar una isla de XAML [ **System.Windows.Interop.HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para las aplicaciones de WPF, [ **System.Windows.Forms.Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicaciones de Windows Forms y un [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para aplicaciones Win32 de C++.

* En el siguiente nivel es un **DesktopWindowXamlSource** objeto. Este objeto proporciona la infraestructura para hospedar la isla de XAML. El código es responsable de crear este objeto y asociarlo al elemento de interfaz de usuario primario.

* Cuando creas un **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativo para hospedar el control UWP. Esta ventana secundaria nativo principalmente se abstrae el código, pero puede tener acceso a su controlador (HWND) si es necesario.

* Por último, en el nivel superior es el control UWP que desea hospedar en su aplicación de escritorio. Esto puede ser cualquier objeto UWP que se derive de [ **Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluyendo cualquier control que UWP proporcionada el SDK de Windows, así como los controles de usuario personalizados.

El siguiente diagrama muestra la jerarquía de objetos en una isla de XAML.

![Arquitectura de DesktopWindowXamlSource](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>Cómo hospedar controles de UWP XAML

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

    1. Crear un **DesktopWindowXamlSource** objeto y conviértalo a la **IDesktopWindowXamlSourceNative** interfaz COM. Esta interfaz se declara en el ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` archivo de encabezado en el SDK de Windows. En un proyecto Win32 de C++, puede hacer referencia directamente este archivo de encabezado. En un proyecto WPF o Windows Forms, deberá declarar esta interfaz en el código de aplicación utilizando el [ **ComImport** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) atributo. Asegúrese de que la declaración de interfaz coincide exactamente con la declaración de interfaz en ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. Llame a la **AttachToWindow** método de la **IDesktopWindowXamlSourceNative** interfaz y pase el identificador de ventana del elemento de interfaz de usuario primario de la aplicación.

    3. Establezca el tamaño inicial de la ventana de secundaria internos contenido en el **DesktopWindowXamlSource**. De forma predeterminada, esta ventana secundaria interno se establece en un ancho y alto igual a 0. Si no establece el tamaño de la ventana, todos los controles UWP se agrega a la **DesktopWindowXamlSource** no serán visibles. Para tener acceso a la ventana secundaria interno en el **DesktopWindowXamlSource**, utilice el **WindowHandle** propiedad de la **IDesktopWindowXamlSourceNative** interfaz. Los ejemplos siguientes usan la [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) función para establecer el tamaño de la ventana.

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
    // that will act as the parent UI elemnt for your XAML island. It also assumes
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

  * **Win32 de C++:** Consulte la [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) de archivos en el [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) ejemplo o el [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) de archivos en el [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) ejemplo.
  * **WPF:** Consulte la [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) archivos en el Kit de herramientas de la Comunidad de Windows.  
  * **Windows Forms:** Consulte la [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) archivos en el Kit de herramientas de la Comunidad de Windows.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>Cómo host personalizado UWP XAML controles

> [!IMPORTANT]
> Actualmente, solo se admiten controles de UWP XAML personalizados de 3 partes en C# aplicaciones WPF y Windows Forms. Debe tener el código fuente para los controles, por lo que puede compilar en él en la aplicación.

Si desea hospedar un control personalizado de UWP XAML (un control que define usted mismo o un control proporcionado por un 3rd party), debe realizar las siguientes tareas adicionales además del proceso se describe en el [sección anterior](#how-to-host-uwp-xaml-controls).

1. Define un tipo personalizado que se derive de [ **Windows.UI.Xaml.Application** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) y también implementa [ **IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Este tipo actúa como un proveedor de metadatos de raíz para cargar los metadatos para los tipos personalizados de UWP XAML en los ensamblados en el directorio actual de la aplicación.

    Para obtener un ejemplo que muestra cómo hacerlo, consulte el [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) archivo de código en el Kit de herramientas de la Comunidad de Windows. Este archivo es parte de la implementación compartida de la **WindowsXamlHost** clases de WPF y Windows Forms, que ayudan a ilustrar cómo se usa el XAML UWP en esos tipos de aplicaciones de la API de hospedaje.

2. Llame a la [ **GetXamlType** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) método de su proveedor de metadatos de raíz cuando se asigna el nombre de tipo del control UWP XAML (Esto podría asignarse en el código en tiempo de ejecución, o puede elegir habilitar esta opción para ser asignar en la ventana Propiedades de Visual Studio).

    Para obtener un ejemplo que muestra cómo hacerlo, consulte el [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) archivo de código en el Kit de herramientas de la Comunidad de Windows. Este archivo es parte de la implementación compartida de la **WindowsXamlHost** clases de WPF y Windows Forms.

3. Integrar el código fuente para el control personalizado de UWP XAML en su solución de aplicación de host, compilar el control personalizado y usarlo en la aplicación siguiendo [estas instrucciones](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="how-to-handle-keyboard-focus-navigation"></a>Cómo controlar la navegación del foco de teclado

Cuando el usuario navega por los elementos de interfaz de usuario de la aplicación mediante el teclado (por ejemplo, presionando **ficha** o tecla de dirección o dirección), deberá mover el foco mediante programación dentro y fuera de la  **DesktopWindowXamlSource** objeto. Cuando llega la navegación con el teclado del usuario la **DesktopWindowXamlSource**, mover el foco en la primera [ **Windows.UI.Xaml.UIElement** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) objeto en el orden de navegación para la interfaz de usuario, continúe mover el foco al siguiente **Windows.UI.Xaml.UIElement** ciclos de objetos como el usuario a través de los elementos y vuelva a mover el foco de la **DesktopWindowXamlSource**y en el elemento de interfaz de usuario primario.  

El XAML UWP API de hospedaje proporciona varios tipos y miembros para ayudarle a realizar estas tareas.

1. Cuando entra en el panel de navegación teclado su **DesktopWindowXamlSource**, [ **GotFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) provoca el evento. Controle este evento y mediante programación mover el foco al primer hospedado **Windows.UI.Xaml.UIElement** utilizando el [ **NavigateFocus** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) método.

2. Cuando el usuario está en el último elemento enfocable en el **DesktopWindowXamlSource** y presiona la **ficha** clave o una tecla de dirección, el [ **TakeFocusRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) provoca el evento. Controle este evento y mover el foco al siguiente elemento enfocable situado en la aplicación host mediante programación. Por ejemplo, en una aplicación WPF donde el **DesktopWindowXamlSource** se hospeda en un [ **System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puede usar el [  **MoveFocus** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) método para transferir el foco al siguiente elemento enfocable situado en la aplicación host.

Para obtener ejemplos que muestran cómo hacer esto en el contexto de una aplicación de ejemplo funcional, consulte los archivos de código siguiente:
  * **WPF:** Consulte la [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) archivo en el Kit de herramientas de la Comunidad de Windows.  
  * **Windows Forms:** Consulte la [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) archivo en el Kit de herramientas de la Comunidad de Windows.

## <a name="how-to-handle-layout-changes"></a>Cómo controlar los cambios de diseño

Cuando el usuario cambia el tamaño del elemento de interfaz de usuario principal, deberá controlar los cambios de diseño necesarias para asegurarse de que los controles UWP mostrar según lo esperado. Estos son algunos escenarios importantes a tener en cuenta.

1. Cuando el elemento de interfaz de usuario primario debe obtener el tamaño del área rectangular que sea necesario para ajustarse el **Windows.UI.Xaml.UIElement** que hospedan en el **DesktopWindowXamlSource**, llame a la [ **Medida** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) método de la **Windows.UI.Xaml.UIElement**. Por ejemplo:
    * En una aplicación WPF podría hacer esto desde el [ **MeasureOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) método de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda el  **DesktopWindowXamlSource**.
    * En una aplicación de Windows Forms podría hacer esto desde el [ **GetPreferredSize** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) método de la [ **Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda el **DesktopWindowXamlSource**.

2. Cuando se cambia el tamaño del elemento de interfaz de usuario primario, llame a la [ **organizar** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) método de la raíz **Windows.UI.Xaml.UIElement** que hospedan en el  **DesktopWindowXamlSource**. Por ejemplo:
    * En una aplicación WPF podría hacer esto desde el [ **ArrangeOverride** ](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) método de la [ **HwndHost** ](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) objeto que hospeda el **DesktopWindowXamlSource**.
    * En una aplicación de Windows Forms podría hacer esto desde el controlador para el [ **SizeChanged** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) eventos de la [ **Control** ](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda el **DesktopWindowXamlSource**.

Para obtener ejemplos que muestran cómo hacer esto en el contexto de una aplicación de ejemplo funcional, consulte los archivos de código siguiente:
  * **WPF:** Consulte la [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) archivo en el Kit de herramientas de la Comunidad de Windows.  
  * **Windows Forms:** Consulte la [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) archivo en el Kit de herramientas de la Comunidad de Windows.

## <a name="how-to-handle-dpi-changes"></a>Cómo controlar los cambios de PPP

Si desea controlar los cambios de PPP en la ventana que hospeda la UWP controlar (por ejemplo, si el usuario arrastra la ventana entre monitores con valores de PPP de pantalla diferentes), deberá configurar el control UWP con una transformación de representación, escucharán los cambios de PPP en la aplicación y actualizar la posición de la ventana y transformación del control UWP en respuesta a los cambios de PPP de representar.

Los siguientes pasos muestran cómo controlar este proceso en el contexto de una aplicación Win32 de C++. Para obtener un ejemplo completo, vea el [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) y [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) en los archivos de código la [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) ejemplo en GitHub.

1. Mantener un [ **ScaleTransform** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) objeto en la aplicación y asígnela a la [ **RenderTransform** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) método del control de UWP. El ejemplo siguiente hace esto para un [ **Windows.UI.Xaml.Controls.Grid** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) control en una aplicación Win32 de C++.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. En su [ **WindowProc** ](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) de función, escuchar la [ **WM_DPICHANGED** ](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) mensaje. En respuesta a este mensaje:
    * Use la [ **SetWindowPos** ](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) función para cambiar el tamaño de la ventana que contiene el control UWP para el rectángulo que se pasa al mensaje.
    * Actualización de los ejes x y y escalan los factores de su **ScaleTransform** objeto según el nuevo valor de PPP.
    * Realizar los ajustes necesarios a la apariencia y el diseño del control de UWP. El siguiente ejemplo de código se ajusta el [ **relleno** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) de hospedado **Windows.UI.Xaml.Controls.Grid** control en respuesta a cambios de PPP.

    ```cppwinrt
    LRESULT HandleDpiChange(HWND hWnd, WPARAM wParam, LPARAM lParam)
    {
        HWND hWndStatic = GetWindow(hWnd, GW_CHILD);
        if (hWndStatic != nullptr)
        {
            UINT uDpi = HIWORD(wParam);

            // Resize the window
            auto lprcNewScale = reinterpret_cast<RECT*>(lParam);

            SetWindowPos(hWnd, nullptr, lprcNewScale->left, lprcNewScale->top,
                lprcNewScale->right - lprcNewScale->left, lprcNewScale->bottom - lprcNewScale->top,
                SWP_NOZORDER | SWP_NOACTIVATE);

            NewScale(uDpi);
          }
          return 0;
    }

    void NewScale(UINT dpi) {

        auto scaleFactor = (float)dpi / 100;

        if (m_scale != nullptr) {
            m_scale.ScaleX(scaleFactor);
            m_scale.ScaleY(scaleFactor);
        }

        ApplyCorrection(scaleFactor);
    }

    void ApplyCorrection(float scaleFactor) {
        float rightCorrection = (m_rootGrid.Width() * scaleFactor - m_rootGrid.Width()) / scaleFactor;
        float bottomCorrection = (m_rootGrid.Height() * scaleFactor - m_rootGrid.Height()) / scaleFactor;

        m_rootGrid.Padding(Windows::UI::Xaml::ThicknessHelper::FromLengths(0, 0, rightCorrection, bottomCorrection));
    }
    ```

2. Para configurar la aplicación sea compatible con PPP por monitor, agregue un [manifiesto del ensamblado en paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y establezca ```<dpiAwareness>``` elemento ```PerMonitorV2```. Para obtener más información acerca de este valor, vea la descripción de [ **DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

    Para un manifiesto de ensamblado en paralelo de ejemplo completo, vea el [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) de archivos en el [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) ejemplo en GitHub.

## <a name="limitations"></a>Limitaciones

La API de hospedaje de XAML comparte las mismas limitaciones que los demás tipos de controles de host XAML para Windows 10. Para obtener una lista detallada, consulte [limitaciones de control de host XAML](xaml-host-controls.md#limitations).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al utilizar XAML UWP API de hospedaje en una aplicación para UWP

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "No se puede activar DesktopWindowXamlSource. Este tipo no se puede usar en una aplicación para UWP." o "no se puede activar WindowsXamlManager. Este tipo no se puede usar en una aplicación para UWP." | Este error indica que está intentando usar el XAML UWP API de hospedaje (en concreto, se intenta crear una instancia de la [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [  **WindowsXamlManager** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) tipos) en una aplicación para UWP. El XAML UWP API de hospedaje sólo está pensado para usarse en aplicaciones de escritorio no de UWP, como las aplicaciones de WPF, Windows Forms y Win32 de C++. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Error al adjuntar a una ventana en un subproceso diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "Método AttachToWindow error porque se creó el HWND especificado en un subproceso diferente". | Este error indica que la aplicación llama a la **IDesktopWindowXamlSourceNative.AttachToWindow** método y lo ha pasado el HWND de una ventana que se creó en un subproceso diferente. Este método se debe pasar el HWND de una ventana que se creó en el mismo subproceso que el código desde el que está llamando al método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al adjuntar a una ventana en una ventana de nivel superior diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "Método AttachToWindow error porque el HWND especificado desciende de una ventana de nivel superior diferente que el HWND que anteriormente se pasó a AttachToWindow en el mismo subproceso". | Este error indica que la aplicación llama a la **IDesktopWindowXamlSourceNative.AttachToWindow** método y lo ha pasado el HWND de una ventana que desciende de una ventana de nivel superior diferente que una ventana especificada en un llamada anterior a este método en el mismo subproceso.</p></p>Después de la aplicación llama **IDesktopWindowXamlSourceNative.AttachToWindow** en un subproceso determinado, todas las demás [ **DesktopWindowXamlSource** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) objetos en el mismo subproceso solo se puede conectar a windows que son descendientes de la misma ventana de nivel superior que se pasó en la primera llamada a **IDesktopWindowXamlSourceNative.AttachToWindow**. Cuando todas las **DesktopWindowXamlSource** se cierran los objetos de un subproceso en particular, la siguiente **DesktopWindowXamlSource** entonces está libre para volver a adjuntar a cualquier ventana.</p></p>Para resolver este problema, cierre todos los **DesktopWindowXamlSource** objetos que están enlazados a otras ventanas de nivel superior en este subproceso, o crear un nuevo subproceso para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Controles UWP en aplicaciones de escritorio](xaml-host-controls.md)
