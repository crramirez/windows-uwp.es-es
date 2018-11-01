---
author: mcleanbyron
description: Este artículo describe cómo alojar UWP XAML IU en la aplicación de escritorio.
title: Usando el XAML UWP API de hospedaje en una aplicación de escritorio
ms.author: mcleans
ms.date: 09/21/2018
ms.topic: article
keywords: Windows 10 uwp, formularios windows forms, wpf, win32
ms.localizationpriority: medium
ms.openlocfilehash: 2ba64e32a25feaee9245bbfe2b598c756b29df98
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5919703"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-desktop-application"></a>Usando el XAML UWP API de hospedaje en una aplicación de escritorio

> [!NOTE]
> El XAML UWP API de hospedaje está disponible actualmente como una vista previa para desarrolladores. Aunque le recomendamos que pruebe ahora esta API en su propio código de prototipo, no recomendamos que utilizarlo en código de producción en este momento. Esta API se seguirá madurar y estabilizar en futuras versiones de Windows. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

A partir de SDK de Windows 10 especialista en vista previa de generación 17709, aplicaciones de escritorio no UWP (incluidas las aplicaciones de WPF, Windows Forms y Win32 de C++) pueden utilizar la *API de hospedaje de XAML UWP* para hospedar controles de UWP en cualquier elemento de interfaz de usuario está asociado con un identificador de ventana) HWND). Esta API permite a las aplicaciones de escritorio UWP no usar las últimas características de interfaz de usuario de Windows 10 que sólo están disponibles a través de controles UWP. Por ejemplo, aplicaciones de escritorio no UWP pueden utilizar esta API para hospedar controles de UWP que utiliza el [Diseño fluida del sistema](../design/fluent-design-system/index.md) y admite [Tinta de Windows](../design/input/pen-and-stylus-interactions.md).

El XAML UWP API de hospedaje proporciona la base para un conjunto más amplio de controles que estamos proporcionando para permitir a los desarrolladores incorporar la interfaz de usuario Fluent a UWP no aplicaciones de escritorio. Esta situación se denomina *Islas XAML*. Para obtener más detalles acerca de esta situación de programador, vea [controles UWP en aplicaciones de escritorio](xaml-host-controls.md).

## <a name="is-the-uwp-xaml-hosting-api-right-for-your-desktop-application"></a>¿Es el XAML UWP el API de hospedaje adecuado para la aplicación de escritorio?

El XAML UWP API de hospedaje proporciona la infraestructura de bajo nivel para alojar controles UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la opción de utilizar la API alternativas más convenientes para lograr este objetivo.  

* Si tiene una aplicación de escritorio de Win32 de C++ y desea que los controles host UWP en su aplicación, debe utilizar el XAML UWP API de hospedaje. No hay ninguna alternativa para estos tipos de aplicaciones.

* Para las aplicaciones de formularios Windows Forms y WPF, se recomienda utilizar los [controles de host](xaml-host-controls.md#host-controls) y [controles ajustados](xaml-host-controls.md#wrapped-controls) en el Kit de herramientas de comunidad de Windows en lugar del XAML UWP API de hospedaje. Estos controles utilizan el XAML UWP internamente de la API de hospedaje y proporcionan una experiencia de desarrollo más sencilla. Sin embargo, puede usar el XAML UWP API directamente en estos tipos de aplicaciones de hospedaje si elige.

## <a name="related-samples"></a>Muestras relacionadas

La forma en que se usa el XAML UWP API de hospedaje en el código depende del tipo de aplicación, el diseño de la aplicación y otros factores. Con el fin de ilustrar el uso de esta API en el contexto de una aplicación completa, este artículo hace referencia al código de los ejemplos siguientes.

### <a name="c-win32"></a>Win32 de C++

Hay varios ejemplos en GitHub que muestran cómo usar el XAML UWP API de hospedaje en una aplicación Win32 de C++:

  * [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting). Este ejemplo muestra cómo agregar los controles UWP [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)y [MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) a una aplicación Win32 de C++.
  * [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32). Este ejemplo muestra cómo agregar varios controles UWP básicos a una aplicación Win32 de C++ y controlar los cambios de PPP.

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

El control de [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el Kit de herramientas de la Comunidad de Windows actúa como una muestra de referencia para el uso de la API de hospedaje en aplicaciones de formularios Windows Forms y WPF de UWP. El código fuente está disponible en las siguientes ubicaciones:

  * Para la versión WPF de control, [vaya aquí](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión de WPF se deriva de [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).
  * Para la versión de formularios Windows Forms del control, [vaya aquí](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost). La versión de formularios Windows Forms se deriva de [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="prerequisites"></a>Requisitos previos

El XAML UWP API de hospedaje tiene estos requisitos previos.

* Vista previa de Windows 10 Insider build 17709 (o una versión posterior) y la generación de vista previa de Insider correspondiente del SDK de Windows. Dado que se trata de una característica en constante evolución, para tener una mejor experiencia se recomienda utilizar la compilación más reciente disponible.

* Para usar el XAML UWP API de hospedaje en su aplicación de escritorio, debe configurar el proyecto para que se pueden llamar a las API de UWP:

    * **Win32 de C++:** Se recomienda que configure el proyecto para utilizar [C + + / WinRT](../cpp-and-winrt-apis/index.md). Descargar e instalar la [C + + / extensión de Visual Studio (VSIX) WinRT](https://aka.ms/cppwinrt/vsix) desde el catálogo de soluciones de Visual Studio y, a continuación, agregue el ```<CppWinRTEnabled>true</CppWinRTEnabled>``` propiedad al archivo .vcxproj como se describe [aquí](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

    * **Windows Forms y WPF:** Siga [estas instrucciones](../porting/desktop-to-uwp-enhance.md).

## <a name="architecture-of-xaml-islands"></a>Arquitectura de las Islas XAML

El XAML UWP API de hospedaje incluye [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource), [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager)y otros tipos relacionados en el espacio de nombres [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting) . Aplicaciones de escritorio pueden utilizar esta API para representar controles UWP y exploración de foco de teclado dentro y fuera de los elementos de la ruta. Aplicaciones de escritorio también pueden cambiar el tamaño y colocar los controles UWP como desee.

Cuando se crea una isla XAML con el XAML de la API de hospedaje en una aplicación de escritorio, tendrá la siguiente jerarquía de objetos:

* En el nivel base es el elemento de interfaz de usuario de la aplicación donde desea alojar la isla XAML. Este elemento de la interfaz de usuario debe tener un identificador de ventana (HWND). Ejemplos de elementos de interfaz de usuario en el que puede alojar una isla XAML son [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para aplicaciones WPF, [**System.Windows.Forms.Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicaciones de formularios Windows Forms y una [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para aplicaciones Win32 de C++.

* En el siguiente nivel es un objeto **DesktopWindowXamlSource** . Este objeto proporciona la infraestructura para alojar la isla XAML. El código es responsable de crear este objeto y asociarlo al elemento de interfaz de usuario principal.

* Cuando se crea un **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativo para alojar su control UWP. Esta ventana secundaria nativo principalmente se abstrae el código, pero puede tener acceso a su controlador (HWND) si es necesario.

* Por último, en el nivel superior es el control UWP que desea alojar en la aplicación de escritorio. Esto puede ser cualquier objeto UWP que deriva de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluyendo cualquier control UWP proporcionado por el SDK de Windows, así como controles de usuario personalizados.

El siguiente diagrama ilustra la jerarquía de objetos en una isla de XAML.

![Arquitectura DesktopWindowXamlSource](images/xaml-hosting-api-rev2.png)

## <a name="how-to-host-uwp-xaml-controls"></a>Cómo hospedar controles de XAML UWP

Estos son los pasos principales para el uso de XAML UWP API de hospedaje para hospedar un control UWP en su aplicación.

1. Inicializar el marco UWP XAML para el subproceso actual antes de que la aplicación crea cualquiera de los objetos [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que alojará en el [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource).

    * Si la aplicación crea el objeto **DesktopWindowXamlSource** antes de crear cualquiera de los objetos **Windows.UI.Xaml.UIElement** , este marco se inicializará automáticamente cuando se crean instancias del objeto **DesktopWindowXamlSource** . En este escenario, no es necesario agregar ningún código propio para inicializar el marco.

    * Sin embargo, si la aplicación crea los objetos de **Windows.UI.Xaml.UIElement** antes de crear el objeto **DesktopWindowXamlSource** que se alojará, la aplicación debe llamar estático [** WindowsXamlManager.InitializeForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) método para inicializar explícitamente el marco UWP XAML antes de que se crean instancias de los objetos **Windows.UI.Xaml.UIElement** . La aplicación normalmente debe llamar a este método cuando se crea una instancia del elemento de interfaz de usuario primario que aloja el **DesktopWindowXamlSource** .

    ```cppwinrt
    Windows::UI::Xaml::Hosting::WindowsXamlManager windowsXamlManager =
        Windows::UI::Xaml::Hosting::WindowsXamlManager::InitializeForCurrentThread();
    ```

    ```csharp
    global::Windows.UI.Xaml.Hosting.WindowsXamlManager windowsXamlManager =
        global::Windows.UI.Xaml.Hosting.WindowsXamlManager.InitializeForCurrentThread();
    ```

    > [!NOTE]
    > Este método devuelve un objeto [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contiene una referencia a la estructura XAML UWP. Puede crear tantos objetos **WindowsXamlManager** como desee en un subproceso determinado. Sin embargo, dado que cada objeto contiene una referencia al marco UWP XAML, debería eliminar los objetos para asegurarse de que finalmente se liberan recursos XAML.

2. Crear un objeto [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) y adjuntarlo a un elemento de interfaz de usuario principal de la aplicación que está asociada con un identificador de ventana.

    Para ello, debe seguir estos pasos:

    1. Crear un objeto **DesktopWindowXamlSource** y lo convierte en la interfaz de **IDesktopWindowXamlSourceNative** COM. Esta interfaz se declara en el ```windows.ui.xaml.hosting.desktopwindowxamlsource.h``` archivo de encabezado de Windows SDK. En un proyecto Win32 de C++, puede hacer referencia directamente este archivo de encabezado. En un proyecto de formularios Windows Forms o WPF, debe declarar esta interfaz en su código de aplicación con el atributo [**ComImport**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) . Asegúrese de que la declaración de interfaz coincide exactamente con la declaración de interfaz en ```windows.ui.xaml.hosting.desktopwindowxamlsource.h```.

    2. Llame al método **AttachToWindow** de la interfaz de **IDesktopWindowXamlSourceNative** y pase el identificador de ventana del elemento de interfaz de usuario principal en la aplicación.

    3. Establezca el tamaño inicial de la ventana secundaria interna contenida en el **DesktopWindowXamlSource**. De forma predeterminada, esta ventana secundaria interna se establece en un ancho y un alto de 0. Si no se establece el tamaño de la ventana, los controles UWP que se agrega a la **DesktopWindowXamlSource** no será visibles. Para acceder a la ventana secundaria interna en el **DesktopWindowXamlSource**, utilice la propiedad **WindowHandle** de la interfaz **IDesktopWindowXamlSourceNative** . Los ejemplos siguientes utilizan la función [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para establecer el tamaño de la ventana.

    Estos son algunos ejemplos de código que se muestran este proceso.

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

3. Establecer el **Windows.UI.Xaml.UIElement** que desea alojar en la propiedad [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) del objeto **DesktopWindowXamlSource** . En el ejemplo siguiente se establece un [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) denominado ```myGrid``` a la propiedad de **contenido** .

   ```cppwinrt
   desktopWindowXamlSource.Content(myGrid);
   ```

   ```csharp
   desktopWindowXamlSource.Content = myGrid;
   ```

Para obtener ejemplos completos que muestran estas tareas en el contexto de una aplicación de ejemplo de trabajo, consulte los siguientes archivos de código:

  * **Win32 de C++:** Consulte el archivo [Main.cpp](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting/blob/master/XamlHostingSample/Main.cpp) en la muestra de [XamlHostingSample](https://github.com/Microsoft/Windows-appsample-Xaml-Hosting) o el archivo [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) en el ejemplo [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) .
  * **WPF:** Consulte los archivos [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) en el Kit de herramientas de la Comunidad de Windows.  
  * **Formularios Windows Forms:** Consulte los archivos [WindowsXamlHostBase.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) en el Kit de herramientas de la Comunidad de Windows.


## <a name="how-to-host-custom-uwp-xaml-controls"></a>Cómo host personalizado UWP XAML controles

> [!IMPORTANT]
> Actualmente, los controles personalizados de XAML UWP de 3 partes sólo se admiten en aplicaciones de formularios Windows Forms y WPF C#. Debe tener el código fuente de los controles para compilar contra él en su aplicación.

Si desea alojar un control UWP XAML personalizado (un control que haya definido usted o un control que proporciona un 3 º), debe realizar las siguientes tareas adicionales además del proceso descrito en la [sección anterior](#how-to-host-uwp-xaml-controls).

1. Define un tipo personalizado que deriva de [**Windows.UI.Xaml.Application**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application) y también implementa [**IXamlMetadataProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider). Este tipo actúa como un proveedor de metadatos de raíz para cargar los metadatos para los tipos XAML UWP personalizados en ensamblados en el directorio actual de la aplicación.

    Para obtener un ejemplo que muestra cómo hacer esto, consulte el archivo de código [XamlApplication.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/XamlApplication.cs) en el Kit de herramientas de la Comunidad de Windows. Este archivo forma parte de la implementación de las clases de **WindowsXamlHost** compartida para WPF y formularios Windows Forms, que ayudan a ilustrar cómo usar XAML UWP en esos tipos de aplicaciones de la API de hospedaje.

2. Llame al método [**GetXamlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.ixamlmetadataprovider.getxamltype) de su proveedor de metadatos de raíz cuando se asigna el nombre de tipo del control XAML UWP (Esto podría asignarse en código en tiempo de ejecución, o puede optar por habilitar esta opción para asignar en la ventana Propiedades de Visual Studio).

    Para obtener un ejemplo que muestra cómo hacer esto, consulte el archivo de código [UWPTypeFactory.cs](https://github.com/Microsoft/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Win32/Microsoft.Windows.Interop.WindowsXamlHost.Shared/UWPTypeFactory.cs) en el Kit de herramientas de la Comunidad de Windows. Este archivo forma parte de la implementación de las clases de **WindowsXamlHost** compartida para WPF y Windows Forms.

3. Integrar el código fuente para el control personalizado de UWP XAML en la solución de la aplicación host, crear el control personalizado y utilizarlo en la aplicación siguiente [estas instrucciones](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost#add-a-custom-uwp-control).

## <a name="how-to-handle-keyboard-focus-navigation"></a>Cómo controlar el desplazamiento del foco de teclado

Cuando el usuario navega por los elementos de la interfaz de usuario de la aplicación mediante el teclado (por ejemplo, presionando **Tab** o flecha de dirección/clave), debe mover mediante programación el enfoque hacia y desde el objeto **DesktopWindowXamlSource** . Cuando llega a la **DesktopWindowXamlSource**, movimiento de foco en el primer objeto de [**Windows.UI.Xaml.UIElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en el orden de navegación de la interfaz de usuario, navegación mediante el teclado del usuario continúe mover el enfoque al siguiente ** Windows.UI.Xaml.UIElement** objetos como los ciclos de usuario a través de los elementos y, a continuación, foco de mover hacia atrás por el **DesktopWindowXamlSource** y en el elemento de la interfaz de usuario principal.  

El XAML UWP API de hospedaje proporciona varios tipos y miembros para ayudarle a realizar estas tareas.

1. Cuando la navegación mediante el teclado entra en el **DesktopWindowXamlSource**, se produce el evento [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) . Controle este evento y mover foco a la primera alojado **Windows.UI.Xaml.UIElement** mediante programación utilizando el método [**NavigateFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

2. Cuando el usuario se encuentra en el último elemento enfocable de su **DesktopWindowXamlSource** y presiona la tecla **Tab** o una tecla de flecha, se provoca el evento [**TakeFocusRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) . Controle este evento y mover mediante programación el enfoque al siguiente elemento enfocable en la aplicación host. Por ejemplo, en una aplicación de WPF donde se hospeda el **DesktopWindowXamlSource** en un [**System.Windows.Interop.HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puede utilizar el método [**MoveFocus**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir el foco al siguiente elemento enfocable en la aplicación host.

Para obtener ejemplos que muestran cómo hacerlo en el contexto de una aplicación de ejemplo de trabajo, consulte los siguientes archivos de código:
  * **WPF:** Consulte el archivo [WindowsXamlHostBase.Focus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) en el Kit de herramientas de la Comunidad de Windows.  
  * **Formularios Windows Forms:** Consulte el archivo [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) en el Kit de herramientas de la Comunidad de Windows.

## <a name="how-to-handle-layout-changes"></a>Cómo controlar los cambios de diseño

Cuando el usuario cambia el tamaño del elemento de interfaz de usuario principal, debe administrar los cambios de diseño necesarios para asegurarse de que los controles UWP mostrar como se esperaba. A continuación presentamos algunos escenarios importantes a tener en cuenta.

1. Cuando el elemento de la interfaz de usuario principal que se necesita obtener el tamaño del área rectangular necesario para ajustarse a la **Windows.UI.Xaml.UIElement** que alojan en el **DesktopWindowXamlSource**, llamar al método de [**medida**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de la **Windows.UI.Xaml.UIElement **. Por ejemplo:
    * En una aplicación WPF puede hacerlo desde el método [**MeasureOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) de [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que aloja el **DesktopWindowXamlSource**.
    * En una aplicación de formularios Windows Forms puede hacerlo desde el método [**GetPreferredSize**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) del [**Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que aloja el **DesktopWindowXamlSource**.

2. Cuando el tamaño de los cambios de elemento de interfaz de usuario principal, llamar al método [**Arrange**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la raíz de **Windows.UI.Xaml.UIElement** que se aloja en el **DesktopWindowXamlSource**. Por ejemplo:
    * En una aplicación WPF puede hacerlo desde el método [**ArrangeOverride**](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) del objeto [**HwndHost**](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que aloja el **DesktopWindowXamlSource**.
    * En una aplicación de formularios Windows Forms puede hacerlo desde el controlador para el evento [**SizeChanged**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) del [**Control**](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que aloja el **DesktopWindowXamlSource**.

Para obtener ejemplos que muestran cómo hacerlo en el contexto de una aplicación de ejemplo de trabajo, consulte los siguientes archivos de código:
  * **WPF:** Consulte el archivo [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el Kit de herramientas de la Comunidad de Windows.  
  * **Formularios Windows Forms:** Consulte el archivo [WindowsXamlHost.Layout.cs](https://github.com/Microsoft/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Win32/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el Kit de herramientas de la Comunidad de Windows.

## <a name="how-to-handle-dpi-changes"></a>Cómo controlar los cambios de PPP

Si desea controlar los cambios de PPP en la ventana que aloja su UWP de control (por ejemplo, si el usuario arrastra la ventana entre monitores con pantalla diferentes DPI), necesitará configurar el control UWP con una transformación de representación, escuchas para los cambios de PPP en su aplicación y actualizar la posición de la ventana y representar la transformación del control UWP en respuesta a los cambios de PPP.

Los pasos siguientes ilustran una forma de controlar este proceso en el contexto de una aplicación Win32 de C++. Para obtener un ejemplo completo, vea los archivos de código [Desktop.cpp](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.cpp) y [Desktop.h](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/Desktop.h) en la muestra [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) en GitHub.

1. Mantener un objeto [**ScaleTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform) en su aplicación y asignarla al método del control UWP [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) . En el ejemplo siguiente se hace para un control de [**Windows.UI.Xaml.Controls.Grid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) en una aplicación Win32 de C++.

    ```cppwinrt
    // Private fields maintained by your app, such as in a window class you have defined.
    Windows::UI::Xaml::Media::ScaleTransform m_scale;
    Windows::UI::Xaml::Controls::Grid m_rootGrid;

    // Code that runs during initialization, such as the constructor for a window class you have defined.
    m_rootGrid.RenderTransform(m_scale);
    ```

2. En la función [**WindowProc**](https://msdn.microsoft.com/library/windows/desktop/ms633573.aspx) , escuchar el mensaje [**WM_DPICHANGED**](https://docs.microsoft.com/windows/desktop/hidpi/wm-dpichanged) . En respuesta a este mensaje:
    * Utilice la función [**SetWindowPos**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) para cambiar el tamaño de la ventana que contiene el control UWP el rectángulo que se pasa al mensaje.
    * Actualizar los factores de escala del eje x y el eje y del objeto **ScaleTransform** según el nuevo valor de PPP.
    * Realizar los ajustes necesarios en la apariencia y el diseño de su control UWP. En el ejemplo de código siguiente se ajusta el [**relleno**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid.padding) del control hospedado **Windows.UI.Xaml.Controls.Grid** en respuesta a cambios de PPP.

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

2. Para configurar la aplicación para que se tenga en cuenta PPP por monitor, añadir un [manifiesto del ensamblado en paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) a su proyecto y establecer ```<dpiAwareness>``` elemento a ```PerMonitorV2```. Para obtener más información acerca de este valor, consulte la descripción de la [**DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2**](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

    Para un manifiesto de ensamblado de side-by-side ejemplo completo, vea el archivo [XamlIslandsWin32.exe.manifest](https://github.com/clarkezone/cppwinrt/blob/master/Desktop/XamlIslandsWin32/XamlIslandsWin32.exe.manifest) en la muestra [XamlIslands32](https://github.com/clarkezone/cppwinrt/tree/master/Desktop/XamlIslandsWin32) en GitHub.

## <a name="limitations"></a>Limitaciones

El XAML de la API de hospedaje comparte las mismas limitaciones que todos los demás tipos de controles de XAML de host para Windows 10. Para obtener una lista detallada, vea [limitaciones de control de host XAML](xaml-host-controls.md#limitations).

## <a name="troubleshooting"></a>Solución de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al utilizar XAML UWP API de hospedaje en una aplicación UWP

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una **COMException** con el siguiente mensaje: "no se puede activar DesktopWindowXamlSource. Este tipo no se puede utilizar en una aplicación UWP." o "no se puede activar WindowsXamlManager. Este tipo no se puede utilizar en una aplicación UWP." | Este error indica que está intentando utilizar el XAML UWP API de hospedaje (en concreto, intenta crear instancias de tipos de [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [**WindowsXamlManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) en una aplicación UWP. El XAML UWP API de hospedaje sólo se diseñó para utilizarse en aplicaciones de escritorio no UWP, como las aplicaciones de WPF, Windows Forms y Win32 de C++. |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Error al conectar con una ventana en un subproceso diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una **COMException** con el mensaje siguiente: "AttachToWindow método falló porque el objeto HWND especificado se creó en un subproceso diferente." | Este error indica que la aplicación llama al método **IDesktopWindowXamlSourceNative.AttachToWindow** y pasa el identificador HWND de una ventana en la que se creó en un subproceso diferente. Este método se debe pasar el HWND de una ventana en la que se creó en el mismo subproceso que el código desde el que está llamando el método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al conectar con una ventana en una ventana de nivel superior diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una **COMException** con el mensaje siguiente: "AttachToWindow método falló porque el objeto HWND especificado desciende desde una ventana de nivel superior diferente que el HWND que previamente se ha pasado a AttachToWindow en el mismo subproceso." | Este error indica que la aplicación llama al método **IDesktopWindowXamlSourceNative.AttachToWindow** y pasa el identificador HWND de una ventana que desciende de una ventana de nivel superior diferente que una ventana especificada en una llamada anterior a este método en el mismo subproceso.</p></p>Cuando la aplicación llama a **IDesktopWindowXamlSourceNative.AttachToWindow** en un subproceso concreto, todos los demás objetos [**DesktopWindowXamlSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) en el mismo subproceso sólo pueden adjuntar a windows que descienden de la misma ventana de nivel superior que se ha pasado en la primera llamada a **IDesktopWindowXamlSourceNative.AttachToWindow**. Cuando se cierran todos los objetos de **DesktopWindowXamlSource** para un subproceso determinado, el siguiente **DesktopWindowXamlSource** entonces está libre para conectar a cualquier ventana de nuevo.</p></p>Para resolver este problema, cierre todos los objetos **DesktopWindowXamlSource** que están enlazadas a otras ventanas de nivel superior en este subproceso, o crean un nuevo subproceso para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Controles UWP en aplicaciones de escritorio](xaml-host-controls.md)
