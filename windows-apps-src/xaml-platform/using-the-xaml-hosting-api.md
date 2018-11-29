---
description: En este artículo se describe cómo hospedar la interfaz de usuario de XAML de UWP en la aplicación de escritorio.
title: Usar la API de hospedaje en una aplicación de escritorio de XAML de UWP
ms.date: 11/27/2018
ms.topic: article
keywords: Windows 10, uwp, formularios windows forms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: df6c47fd93c3f42721fd072d6406a2d32f7889db
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "7990122"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Usar la API de hospedaje en una aplicación de escritorio de XAML de UWP

> [!NOTE]
> El hospedaje Islas XAML de UWP API y XAML están actualmente disponibles como una vista previa de desarrollador. Aunque te animamos a probarlas en su propio código prototipo ahora, no recomendamos que uses ellos en el código de producción en este momento. Estas características seguirán madurando y estabilizar en futuras versiones de Windows. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.
>
> Si tienes comentarios sobre el código XAML que aloja Islas API y XAML, enviar tus comentarios a XamlIslandsFeedback@microsoft.com. La información y escenarios son muy importantes para nosotros.

A partir de Windows 10 Insider Preview SDK compilación 17709, aplicaciones de escritorio no UWP (incluidas las aplicaciones de Win32 de C++, Windows Forms y WPF) pueden usar la *API de hospedaje de XAML de UWP* para hospedar los controles de UWP en cualquier elemento de la interfaz de usuario que está asociado con un identificador de ventana) HWND). Esta API permite a las aplicaciones de escritorio no UWP usar las características de la interfaz de usuario de Windows 10 más recientes que solo están disponibles a través de los controles de UWP. Por ejemplo, las aplicaciones de escritorio no UWP pueden usar esta API para hospedar los controles de UWP que usan el [Sistema Fluent Design](../design/fluent-design-system/index.md) y admiten [Windows Ink](../design/input/pen-and-stylus-interactions.md).

La API de hospedaje de XAML de UWP proporciona la base para un amplio conjunto de controles que proporcionamos para permitir a los desarrolladores incorporar Fluent de la interfaz de usuario para que no sean de UWP aplicaciones de escritorio. Este escenario se conoce como *Islas XAML*. Para obtener más detalles sobre este escenario de desarrollador, consulta [controles de UWP en aplicaciones de escritorio](xaml-host-controls.md).

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>¿Es el XAML de UWP, API que aloja adecuado para la aplicación de escritorio?

La API de hospedaje de XAML de UWP proporciona la infraestructura de bajo nivel para el hospedaje de controles de UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la posibilidad de usar las API de alternativas, más conveniente para lograr este objetivo.  

* Si tienes una aplicación de escritorio Win32 de C++ y quieres hospedar los controles de UWP en tu aplicación, debes usar la API de hospedaje de XAML de UWP. No hay ningún alternativas para estos tipos de aplicaciones.

* Para las aplicaciones de WPF y Windows Forms, te recomendamos que uses el [encapsulado controles](xaml-host-controls.md#wrapped-controls) y los [controles de host](xaml-host-controls.md#host-controls) en el Kit de herramientas de comunidad de Windows en lugar del XAML de UWP que aloja API. Estos controles usan la API de hospedaje internamente de XAML de UWP y proporcionan una experiencia de desarrollo más sencilla. Sin embargo, puedes usar la API directamente en estos tipos de aplicaciones de hospedaje si eliges de XAML de UWP.

## <a name="related-samples"></a>Muestras relacionadas

La manera que usas la API de hospedaje en el código de XAML de UWP depende de su tipo de aplicación, el diseño de la aplicación y otros factores. Con el fin de ilustrar cómo usar esta API en el contexto de una aplicación completa, en este artículo se hace referencia al código de las siguientes muestras.

### <a name="c-win32"></a>Win32 de C++

Hay varios ejemplos en GitHub que muestran cómo usar la API de hospedaje en una aplicación Win32 de C++ de XAML de UWP:

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). En este ejemplo se muestra cómo agregar los controles UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)y [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) a una aplicación Win32 de C++.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). En este ejemplo se muestra cómo agregar varios controles UWP básicos a una aplicación de Win32 de C++ y controlar los cambios de PPP.

### <a name="wpf-and-windows-forms"></a>Aplicaciones de WPF y Windows Forms

El control de [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el Kit de herramientas de comunidad Windows actúa como una muestra de referencia para el uso de la UWP en la API de hospedaje en aplicaciones de WPF y Windows Forms. El código fuente está disponible en las siguientes ubicaciones:

  * La versión de WPF del control, [dirígete aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión WPF se deriva de [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).
  * La versión de Windows Forms del control, [dirígete aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La versión de Windows Forms se deriva de [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje de XAML de UWP tiene estos requisitos previos.

* Windows 10 Insider Preview build 17709 (o una compilación posterior) y la compilación de Insider Preview correspondiente del SDK de Windows. Dado que es una característica en constante evolución, para una mejor experiencia se recomienda usar la compilación más reciente disponible.

* Para usar la API de hospedaje en la aplicación de escritorio de XAML de UWP, tendrás que configurar el proyecto por lo que puedes llamar a las API de UWP:

    * **Win32 de C++:** Te recomendamos que configures el proyecto para usar [C++ / WinRT](../cpp-and-winrt-apis/index.md). Descargar e instalar la [C++ / extensión de Visual Studio (VSIX) de WinRT](https://aka.ms/cppwinrt/vsix) desde Visual Studio Marketplace y, a continuación, agrega el ```<CppWinRTEnabled>true</CppWinRTEnabled>``` propiedad a tu archivo .vcxproj como se describe [aquí](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

    * **Windows Forms y WPF:** Sigue [estas instrucciones](../porting/desktop-to-uwp-enhance.md).

## <a name="architecture-of-xaml-islands"></a>Arquitectura de islas XAML

La API de hospedaje de XAML de UWP incluye [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)y otros tipos relacionados en el espacio de nombres [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) . Las aplicaciones de escritorio usar esta API para representar los controles de UWP y la ruta de navegación de foco del teclado dentro y fuera de los elementos. Las aplicaciones de escritorio, pueden cambiar el tamaño y colocar los controles UWP según tus preferencias.

Al crear una isla XAML mediante la API de hospedaje en una aplicación de escritorio de XAML, tienes la jerarquía de objetos siguiente:

* En el nivel de base es el elemento de interfaz de usuario en la aplicación donde quieras hospedar la isla XAML. Este elemento de interfaz de usuario debe tener un identificador de ventana (HWND). Algunos ejemplos de elementos de interfaz de usuario en el que puedes alojar una isla XAML son [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicaciones WPF, [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicaciones de Windows Forms y una [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para las aplicaciones Win32 de C++.

* En el siguiente nivel es un objeto **DesktopWindowXamlSource** . Este objeto proporciona la infraestructura para hospedar la isla XAML. El código es responsable de crear este objeto y asociarlo al elemento de interfaz de usuario principal.

* Cuando creas un **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativo para hospedar el control de UWP. Esta ventana secundaria nativo principalmente se abstrae el código, pero puedes acceder a su identificador (HWND) si es necesario.

* Por último, en el nivel superior es el control UWP que quieras hospedar en tu aplicación de escritorio. Esto puede ser cualquier objeto UWP que se deriva de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP proporcionado el SDK de Windows, así como los controles de usuario personalizados.

El siguiente diagrama muestra la jerarquía de objetos en una isla XAML.

![Arquitectura de DesktopWindowXamlSource](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>Cómo hospedar los controles de XAML de UWP

Estos son los pasos principales para usar la API de hospedaje de XAML de UWP para hospedar un control UWP en tu aplicación.

1. Inicializar el marco XAML de UWP para el subproceso actual antes de que la aplicación crea cualquiera de los objetos [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que alojará en el [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Si la aplicación crea el objeto **DesktopWindowXamlSource** antes de crear cualquiera de los objetos **Windows.UI.Xaml.UIElement** , este marco se inicializará automáticamente cuando crees una instancia del objeto **DesktopWindowXamlSource** . En este escenario, no necesitas agregar cualquier código de tu propia para inicializar el marco de trabajo.

    * Sin embargo, si la aplicación crea los objetos **Windows.UI.Xaml.UIElement** antes de que crea el objeto **DesktopWindowXamlSource** que se alojará en ellos, la aplicación debe llamar estático [** WindowsXamlManager.InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) método explícitamente inicializar el marco XAML de UWP antes de que se crean instancias de los objetos **Windows.UI.Xaml.UIElement** . La aplicación por lo general, debe llamar a este método cuando se crea una instancia del elemento de la interfaz de usuario primario que hospeda el **DesktopWindowXamlSource** .

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Este método devuelve un objeto [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contiene una referencia al marco de XAML de UWP. Puedes crear todos los objetos de **WindowsXamlManager** que quieras en un subproceso determinado. Sin embargo, dado que cada objeto contiene una referencia al marco de XAML de UWP, se deben eliminar los objetos para garantizar que finalmente se liberan recursos XAML.

2. Crea un objeto [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) y conectarlo a un elemento de interfaz de usuario principal de la aplicación que está asociado con un identificador de ventana.

    Para ello, tendrás que seguir estos pasos:

    1. Crea un objeto **DesktopWindowXamlSource** y convertirlo a la interfaz **IDesktopWindowXamlSourceNative** COM. Esta interfaz se declara en el ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` archivo de encabezado en el SDK de Windows. En un proyecto de Win32 de C++, puedes hacer referencia directamente este archivo de encabezado. En un proyecto WPF o Windows Forms, tendrás que declarar esta interfaz en el código de aplicación con el atributo [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) . Asegúrese de que la declaración de la interfaz coincida exactamente con la declaración de interfaz en ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. Llama al método **AttachToWindow** de la interfaz de **IDesktopWindowXamlSourceNative** y pasar el identificador de ventana del elemento de interfaz de usuario principal de la aplicación.

    3. Establece el tamaño inicial de la ventana de secundaria interno contenida en el **DesktopWindowXamlSource**. De manera predeterminada, esta ventana secundaria interno se establece en un ancho y alto de 0. Si no estableces el tamaño de la ventana, los controles UWP que agregues a la **DesktopWindowXamlSource** no estará visibles. Para acceder a la ventana secundaria interno en el **DesktopWindowXamlSource**, usa la propiedad **WindowHandle** de la interfaz **IDesktopWindowXamlSourceNative** . Los siguientes ejemplos, usan la función [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para establecer el tamaño de la ventana.

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

3. Establece el **Windows.UI.Xaml.UIElement** que desea alojar a la propiedad de [**contenido**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) del objeto **DesktopWindowXamlSource** . El siguiente ejemplo establece un [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) denominado ```myGrid``` a la propiedad de **contenido** .

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Para obtener ejemplos completos que muestran estas tareas en el contexto de una aplicación de ejemplo de trabajo, consulta los siguientes archivos de código:

  * **Win32 de C++:** Consulta el archivo [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) en la muestra de [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) o [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) en la muestra de [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) .
  * **WPF:** Ver los archivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) en el Kit de herramientas de comunidad Windows.  
  * **Windows Forms:** Ver los archivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) en el Kit de herramientas de comunidad Windows.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>Cómo al host personalizado controles XAML de UWP

> [!IMPORTANT]
> Actualmente, los controles personalizados de XAML de UWP desde 3 partes solo se admiten en aplicaciones de C# WPF y Windows Forms. Debes tener el código fuente para los controles para que se puede compilar con él en la aplicación.

Si quieres hospedar un control personalizado de XAML de UWP (un control que defines tú mismo o un control de un 3 º), debe realizar las siguientes tareas adicionales además el proceso descrito en la [sección anterior](#how-to-host-uwp-xaml-controls).

1. Definir un tipo personalizado que deriva de [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) y también implementa [**IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Este tipo actúa como un proveedor de metadatos de raíz para cargar los metadatos para los tipos personalizados de XAML de UWP en los ensamblados en el directorio actual de la aplicación.

    Para obtener un ejemplo que muestra cómo hacerlo, consulta el archivo de código de [XamlApplication.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/XamlApplication.cs) en el Kit de herramientas de comunidad Windows. Este archivo es parte de la implementación de las clases de **WindowsXamlHost** compartida para aplicaciones de WPF y Windows Forms, lo que ayudará a ilustrar cómo usar la API de esos tipos de aplicaciones de hospedaje de XAML de UWP.

2. Llama al método de [**GetXamlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) de tu proveedor de metadatos de raíz cuando se le asigna el nombre de tipo del control XAML de UWP (Esto podría asignarse en el código en tiempo de ejecución, o puede elegir habilitar esto asignarse en la ventana de propiedades de Visual Studio).

    Para obtener un ejemplo que muestra cómo hacerlo, consulta el archivo de código de [UWPTypeFactory.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Win32.UI.XamlHost/UWPTypeFactory.cs) en el Kit de herramientas de comunidad Windows. Este archivo es parte de la implementación de las clases de **WindowsXamlHost** compartida para aplicaciones de WPF y Windows Forms.

3. Integrar el código fuente para el control personalizado de XAML de UWP en la solución de aplicación host, el control personalizado de compilación y usarla en tu aplicación mediante los siguientes [estas instrucciones](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="how-to-handle-keyboard-focus-navigation"></a>Cómo controlar la navegación de foco del teclado

Cuando el usuario navega por los elementos de la interfaz de usuario de la aplicación mediante el teclado (por ejemplo, al presionar la tecla de **dirección/TAB o** ), tendrás que mover el foco mediante programación desde y hacia el objeto **DesktopWindowXamlSource** . Cuando la navegación del usuario teclado llega a la **DesktopWindowXamlSource**, mover el foco en el primer objeto [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en el orden de navegación de la interfaz de usuario continúa mover el foco al siguiente ** Windows.UI.Xaml.UIElement** objetos durante el ciclo del usuario a través de los elementos y, a continuación, mover el foco vuelve fuera de la **DesktopWindowXamlSource** y en el elemento de interfaz de usuario principal.  

La API de hospedaje de XAML de UWP proporciona varios tipos y miembros para realizar estas tareas.

1. Cuando la navegación por teclado introduce tu **DesktopWindowXamlSource**, se genera el evento [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) . Controlar este evento y mover el foco mediante programación a la primera hospedado **Windows.UI.Xaml.UIElement** mediante el método [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

2. Cuando el usuario está en el último elemento activable en tu **DesktopWindowXamlSource** y presiona la tecla **Tab** o una tecla de dirección, se genera el evento [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) . Controlar este evento y mover el foco mediante programación en el siguiente elemento activable en la aplicación host. Por ejemplo, en una aplicación de WPF donde se hospeda el **DesktopWindowXamlSource** en un [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puedes usar el método [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir el foco al siguiente elemento activable en la aplicación host.

Para ver ejemplos que muestran cómo hacer esto en el contexto de una aplicación de ejemplo de trabajo, consulta los siguientes archivos de código:
  * **WPF:** Consulta el archivo de [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) en el Kit de herramientas de comunidad Windows.  
  * **Windows Forms:** Consulta el archivo de [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) en el Kit de herramientas de comunidad Windows.

## <a name="how-to-handle-layout-changes"></a>Cómo controlar los cambios de diseño

Cuando el usuario cambia el tamaño del elemento de interfaz de usuario principal, tendrás que controlar los cambios de diseño es necesario para asegurarse de que los controles UWP mostrar según lo esperado. Estos son algunos escenarios importantes a tener en cuenta.

1. Cuando el elemento de interfaz de usuario principal que se necesita obtener el tamaño del área rectangular necesario para ajustar el **Windows.UI.Xaml.UIElement** que aloja en el **DesktopWindowXamlSource**, llama al método de [**medición**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de la **Windows.UI.Xaml.UIElement **. Por ejemplo:
    * En una aplicación WPF puede hacerlo desde el método [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda el **DesktopWindowXamlSource**.
    * En una aplicación de Windows Forms puede hacerlo desde el método [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) del [**Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda el **DesktopWindowXamlSource**.

2. Cuando el tamaño de los cambios de elemento de interfaz de usuario principal, llama al método [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la raíz **Windows.UI.Xaml.UIElement** que aloja en el **DesktopWindowXamlSource**. Por ejemplo:
    * En una aplicación WPF puede hacerlo desde el método [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) del objeto [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda el **DesktopWindowXamlSource**.
    * En una aplicación de Windows Forms puede hacerlo desde el controlador para el evento [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) del [**Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda el **DesktopWindowXamlSource**.

Para ver ejemplos que muestran cómo hacer esto en el contexto de una aplicación de ejemplo de trabajo, consulta los siguientes archivos de código:
  * **WPF:** Consulta el archivo de [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el Kit de herramientas de comunidad Windows.  
  * **Windows Forms:** Consulta el archivo de [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el Kit de herramientas de comunidad Windows.

## <a name="how-to-handle-dpi-changes"></a>Cómo controlar los cambios de PPP

Si desea controlar los cambios de PPP en la ventana que hospeda el UWP controlar (por ejemplo, si el usuario arrastra la ventana entre los monitores con pantallas de diferentes PPP), tendrás que configurar el control UWP con una transformación de representación, escuchar los cambios de PPP en la aplicación y actualizar la posición de la ventana y representar la transformación del control para UWP en respuesta a los cambios de PPP.

Los pasos siguientes ilustran una manera de controlar este proceso en el contexto de una aplicación Win32 de C++. Para obtener un ejemplo completo, consulta los archivos de código [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) y [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) en la muestra de [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) en GitHub.

1. Mantener un objeto [**ScaleTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) en la aplicación y asignarlo al método [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) del control para UWP. El siguiente ejemplo hace esto para un control [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) en una aplicación Win32 de C++.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. En la función [**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) , escuchar el mensaje [**WM_DPICHANGED**](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) . En respuesta a este mensaje:
    * Usa la función [**SetWindowPos**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para cambiar el tamaño de la ventana que contiene el control UWP en el rectángulo que se pasa al mensaje.
    * Actualizar los factores de escala de ejes x y y del objeto **ScaleTransform** según el nuevo valor de PPP.
    * Realizar los ajustes necesarios en la apariencia y el diseño de tu control UWP. El siguiente ejemplo de código ajusta el [**espaciado interno**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) del control **Windows.UI.Xaml.Controls.Grid** hospedado en respuesta a cambios de PPP.

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

2. Para configurar la aplicación para tener en cuenta PPP por el monitor, agrega un [manifiesto en paralelo ensamblado](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y establecer ```<dpiAwareness>``` elemento a ```PerMonitorV2```. Para obtener más información sobre este valor, vea la descripción de [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

    Para un manifiesto de ensamblado side-by-side ejemplo completo, consulta el archivo [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) en la muestra de [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) en GitHub.

## <a name="limitations"></a>Limitaciones

La API de hospedaje de XAML comparte las mismas limitaciones que todos los otros tipos de controles de host XAML para Windows 10. Para obtener una lista detallada, consulta [las limitaciones de control de host XAML](xaml-host-controls.md#limitations).

## <a name="troubleshooting"></a>Solucionar problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al usar la API de hospedaje en una aplicación para UWP de XAML de UWP

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una **COMException** con el siguiente mensaje: "no se puede activar DesktopWindowXamlSource. Este tipo no puede usarse en una aplicación para UWP." o "no se puede activar WindowsXamlManager. Este tipo no puede usarse en una aplicación para UWP." | Este error indica que intentas usar la API de hospedaje de XAML de UWP (en concreto, intenta crear instancias de los tipos de [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) en una aplicación para UWP. La API de hospedaje de XAML de UWP solo está pensada para usarse en aplicaciones de escritorio no UWP, como aplicaciones WPF, Windows Forms y Win32 de C++. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Asociar a una ventana en un subproceso distinto del error

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una **COMException** con el siguiente mensaje: "AttachToWindow error del método porque el HWND especificado se creó en un subproceso diferente." | Este error indica que la aplicación llama al método **IDesktopWindowXamlSourceNative.AttachToWindow** y pasa el HWND de una ventana que se creó en un subproceso diferente. Debes pasar este método HWND de una ventana que se creó en el mismo subproceso que el código desde el que está llamando al método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al asociar a una ventana en una ventana de nivel superior diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una **COMException** con el siguiente mensaje: "AttachToWindow error del método porque el HWND especificado desciende desde una ventana de nivel superior diferente que el HWND que anteriormente se pasó a AttachToWindow en el mismo subproceso." | Este error indica que la aplicación llama al método **IDesktopWindowXamlSourceNative.AttachToWindow** y pasa el HWND de una ventana que desciende desde una ventana de nivel superior diferentes que una ventana que especificaste en una llamada a este método anterior en el mismo subproceso.</p></p>Después de que tu aplicación llama **IDesktopWindowXamlSourceNative.AttachToWindow** en un subproceso en particular, todos los demás objetos de [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) en el mismo subproceso solo pueden adjuntar a windows que son descendientes de la misma ventana de nivel superior pasada en la primera llamada a **IDesktopWindowXamlSourceNative.AttachToWindow**. Cuando se cierran todos los objetos de **DesktopWindowXamlSource** para un subproceso en particular, el siguiente **DesktopWindowXamlSource** , a continuación, es gratuito adjuntar a cualquier ventana nuevo.</p></p>Para resolver este problema, ya sea cierra todos los objetos de **DesktopWindowXamlSource** que están enlazadas a otras ventanas de nivel superior en este subproceso, o crean un nuevo subproceso para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Controles UWP en aplicaciones de escritorio](xaml-host-controls.md)
