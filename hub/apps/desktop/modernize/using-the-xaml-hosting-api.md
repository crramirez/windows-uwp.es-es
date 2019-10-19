---
description: En este artículo se describe cómo hospedar la interfaz de usuario C++ XAML de UWP en la aplicación Win32 de escritorio.
title: Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Win32, Islas XAML
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: cdcef66dc1f0026ff369eeb3f3c7881385d6e5ba
ms.sourcegitcommit: 412bf5bb90e1167d118699fbf71d0e6864ae79bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "71339293"
---
# <a name="using-the-uwp-xaml-hosting-api-in-a-c-win32-app"></a>Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++

A partir de Windows 10, versión 1903, las aplicaciones de escritorio que no C++ son de UWP (incluidas las aplicaciones Win32, WPF y Windows Forms) pueden usar la *API de hospedaje XAML de UWP* para hospedar controles de UWP en cualquier elemento de la interfaz de usuario que esté asociado a un identificador de ventana (HWND). Esta API permite a las aplicaciones de escritorio que no son de UWP usar las últimas características de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles de UWP. Por ejemplo, las aplicaciones de escritorio que no son de UWP pueden usar esta API para hospedar controles de UWP que usan el [sistema de diseño fluida](/windows/uwp/design/fluent-design-system/index) y admiten [Windows Ink](/windows/uwp/design/input/pen-and-stylus-interactions).

La API de hospedaje XAML de UWP proporciona la base para un conjunto más amplio de controles que se proporcionan para permitir que los desarrolladores incorporen la interfaz de usuario fluida a aplicaciones de escritorio que no son de UWP. Esta característica se denomina *islas XAML*. Para obtener información general sobre esta característica, consulte [controles XAML de UWP de host en aplicaciones de escritorio (Islas XAML)](xaml-islands.md).

> [!NOTE]
> Si tiene comentarios sobre las islas XAML, cree un nuevo problema en el [repositorio Microsoft. Toolkit. Win32](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/issues) y deje los comentarios allí. Si prefiere enviar sus comentarios de forma privada, puede enviarlos a XamlIslandsFeedback@microsoft.com. Sus conocimientos y escenarios son muy importantes para nosotros.

## <a name="should-you-use-the-uwp-xaml-hosting-api"></a>¿Debe usar la API de hospedaje XAML de UWP?

La API de hospedaje XAML de UWP proporciona la infraestructura de bajo nivel para hospedar controles de UWP en aplicaciones de escritorio. Algunos tipos de aplicaciones de escritorio tienen la opción de usar API alternativas más adecuadas para lograr este objetivo.  

* Si tiene una C++ aplicación de escritorio de Win32 y desea hospedar controles de UWP en la aplicación, debe usar la API de hospedaje XAML de UWP. No hay alternativas para estos tipos de aplicaciones.

* En el caso de las aplicaciones de WPF y Windows Forms, se recomienda encarecidamente usar los [controles .net de la isla XAML](xaml-islands.md#wpf-and-windows-forms-applications) en el kit de herramientas de la comunidad de Windows en lugar de usar directamente la API de hospedaje XAML de UWP. Estos controles usan internamente la API de hospedaje XAML de UWP e implementan todo el comportamiento que, de lo contrario, tendría que administrarse si usara la API de hospedaje XAML de UWP directamente, incluidos los cambios de diseño y navegación del teclado.

Dado que se recomienda que C++ solo las aplicaciones Win32 usen la API de hospedaje XAML de UWP, este artículo proporciona principalmente C++ instrucciones y ejemplos para aplicaciones Win32. Sin embargo, puede usar la API de hospedaje XAML de UWP en WPF y Windows Forms aplicaciones si lo desea. En este artículo se hace referencia al código fuente pertinente de los [controles host](xaml-islands.md#host-controls) para WPF y Windows Forms en el kit de herramientas de la comunidad de Windows para que pueda ver cómo esos controles usan la API de hospedaje XAML de UWP.

## <a name="prerequisites"></a>Requisitos previos

Las islas XAML requieren Windows 10, versión 1903 (o posterior) y la compilación correspondiente del Windows SDK. Para usar islas XAML en la C++ aplicación Win32, primero debe configurar el proyecto.

### <a name="support-for-cwinrt"></a>Compatibilidad con C++/WinRT

Asegúrese de que el proyecto admite [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis):

* En el caso de los proyectos nuevos, puede instalar la [ C++extensión de Visual Studio/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) y C++usar una de las plantillas de proyecto de/WinRT incluidas en esa extensión.
* En el caso de los proyectos existentes, puede instalar el paquete NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto.

Para obtener más información sobre estas opciones, consulte [este artículo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="configure-your-project-for-app-deployment"></a>Configuración del proyecto para la implementación de aplicaciones

Elija una de las siguientes opciones para preparar el proyecto para la implementación:

* **Empaquetar la aplicación en un paquete MSIX**. Empaquetar la aplicación en un [paquete de MSIX](https://docs.microsoft.com/windows/msix/) proporciona muchas ventajas de la implementación y el tiempo de ejecución.
    1. Instale el SDK de Windows 10, versión 1903 (versión 10.0.18362) o una versión posterior.
    2. Empaquete la aplicación en un paquete de MSIX agregando un proyecto de paquete de [aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución y C++agregando una referencia al proyecto de/Win32.

* **Instale el paquete Microsoft. Toolkit. Win32. UI. SDK**. Si no quiere empaquetar la aplicación en un paquete de MSIX, puede instalar [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (versión v 6.0.0-preview7 o posterior). Este paquete proporciona varios recursos de compilación y tiempo de ejecución que permiten a las islas XAML trabajar en la aplicación. Asegúrese de que la opción **incluir versión** preliminar esté seleccionada para que pueda ver las últimas versiones de vista previa de este paquete.

> [!NOTE]
> Las versiones anteriores de estas instrucciones tenían que agregar el elemento `maxversiontested` a un manifiesto de aplicación en el proyecto. Siempre que use una de las opciones enumeradas anteriormente, ya no tendrá que agregar este elemento al manifiesto.

### <a name="additional-requirements-for-custom-uwp-controls"></a>Requisitos adicionales para los controles personalizados de UWP

Si está hospedando un control de UWP personalizado (por ejemplo, un control de usuario que consta de varios controles de UWP que funcionan conjuntamente), deberá seguir las instrucciones adicionales de [esta sección](#host-a-custom-uwp-control). También debe tener el código fuente del control personalizado para que pueda compilarlo con la aplicación.

## <a name="architecture-of-the-api"></a>Arquitectura de la API

La API de hospedaje XAML de UWP incluye estos principales tipos de Windows Runtime e interfaces COM.

|  Tipo o interfaz | Descripción |
|--------------------|-------------|
| [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) | Esta clase representa el marco XAML de UWP. Esta clase proporciona un método [InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) estático único que inicializa el marco XAML de UWP en el subproceso actual en la aplicación de escritorio. |
| [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) | Esta clase representa una instancia de contenido XAML de UWP que se hospeda en la aplicación de escritorio. El miembro más importante de esta clase es la propiedad de [contenido](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) . Esta propiedad se asigna a un [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que desea hospedar. Esta clase también tiene otros miembros para la navegación del foco de teclado de enrutamiento dentro y fuera de las islas XAML. |
| IDesktopWindowXamlSourceNative | Esta interfaz COM proporciona el método **AttachToWindow** , que se usa para adjuntar una isla XAML en la aplicación a un elemento primario de la interfaz de usuario. Cada objeto **DesktopWindowXamlSource** implementa esta interfaz. |
| IDesktopWindowXamlSourceNative2 | Esta interfaz COM proporciona el método **PreTranslateMessage** , que permite que el marco XAML de UWP procese correctamente determinados mensajes de Windows. Cada objeto **DesktopWindowXamlSource** implementa esta interfaz. |

En el diagrama siguiente se muestra la jerarquía de objetos de una isla XAML que se hospeda en una aplicación de escritorio.

* En el nivel base, es el elemento de la interfaz de usuario de la aplicación donde desea hospedar la isla XAML. Este elemento de la interfaz de usuario debe tener un identificador de ventana (HWND). Algunos ejemplos de elementos de la interfaz de usuario en los que puede hospedar una C++ isla de XAML son una [ventana](https://docs.microsoft.com/windows/desktop/winmsg/about-windows) para las aplicaciones Win32, [System. Windows. Interop. HWNDHOST](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) para las aplicaciones de WPF y [System. Windows. Forms. control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) para aplicaciones Windows Forms.

* El siguiente nivel es un objeto **DesktopWindowXamlSource** . Este objeto proporciona la infraestructura para hospedar la isla XAML. El código es responsable de crear este objeto y adjuntarlo al elemento primario de la interfaz de usuario.

* Cuando se crea un **DesktopWindowXamlSource**, este objeto crea automáticamente una ventana secundaria nativa para hospedar el control de UWP. Esta ventana secundaria nativa se abstrae principalmente del código, pero se puede tener acceso a su identificador (HWND) si es necesario.

* Por último, en el nivel superior se encuentra el control UWP que quiere hospedar en la aplicación de escritorio. Puede ser cualquier objeto UWP que se derive de [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), incluido cualquier control UWP proporcionado por el Windows SDK, así como los controles de usuario personalizados.

![Arquitectura de DesktopWindowXamlSource](images/xaml-islands/xaml-hosting-api-rev2.png)

## <a name="related-samples"></a>Muestras relacionadas

La forma de usar la API de hospedaje XAML de UWP en el código depende del tipo de aplicación, el diseño de la aplicación y otros factores. Para ayudar a ilustrar el uso de esta API en el contexto de una aplicación completa, en este artículo se hace referencia al código de los ejemplos siguientes.

### <a name="c-win32"></a>C++32

En los siguientes ejemplos se muestra cómo usar la API de hospedaje XAML de C++ UWP en una aplicación Win32:

* [Ejemplo de isla XAML simple](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp). En este ejemplo se muestra una implementación básica de que hospeda un control de UWP en C++ una aplicación Win32 desempaquetada (es decir, una aplicación que no está integrada en un paquete MSIX).

* [Isla XAML con el ejemplo de control personalizado](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App). En este ejemplo se muestra una implementación completa de que hospeda un control de UWP personalizado en C++ una aplicación Win32 desempaquetada, así como el control de otro comportamiento, como la entrada del teclado y la navegación del foco. 

### <a name="wpf-and-windows-forms"></a>WPF y Windows Forms

El control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows sirve como ejemplo de referencia para usar la API de hospedaje de UWP en WPF y Windows Forms aplicaciones. El código fuente está disponible en las siguientes ubicaciones:

* Para la versión de WPF del control, [vaya aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Wpf.UI.XamlHost). La versión de WPF deriva de [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost).

* Para obtener la versión Windows Forms del control, [vaya aquí](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Forms.UI.XamlHost). La versión Windows Forms se deriva de [System. Windows. Forms. control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control).

## <a name="host-a-standard-uwp-control"></a>Hospedar un control estándar de UWP

Esta sección le guía a través del proceso de uso de la API de hospedaje XAML de UWP para hospedar un control estándar de UWP (es decir, un control proporcionado por la biblioteca Windows SDK C++ o WinUI) en una nueva aplicación de Win32. El código se basa en el [ejemplo de isla XAML simple](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp)y en esta sección se describen algunas de las partes más importantes del código. Si tiene un proyecto de C++ aplicación de Win32 existente, puede adaptar estos pasos y ejemplos de código para el proyecto.

### <a name="configure-the-project"></a>Configurar el proyecto

1. En Visual Studio 2019 con el SDK de Windows 10, versión 1903 (versión 10.0.18362) o una versión posterior instalada, cree un nuevo proyecto de **aplicación de escritorio de Windows** . Este proyecto está disponible en los **C++** filtros de proyectos de **escritorio** , **Windows**y.

2. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución, haga clic en **redestinar solución**, seleccione el **10.0.18362.0** o una versión posterior del SDK y, a continuación, haga clic en **Aceptar**.

3. Instale el paquete NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) :

    1. Haga clic con el botón derecho en el proyecto en **Explorador de soluciones** y elija **administrar paquetes NuGet**.
    2. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instale la versión más reciente de este paquete.

4. Instale el paquete NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) :

    1. En la ventana **Administrador de paquetes NuGet** , asegúrese de que esté seleccionada la opción **incluir versión preliminar** .
    2. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) e instale la versión v 6.0.0-preview7 (o posterior) de este paquete.

### <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Uso de la API de hospedaje de XAML para hospedar un control de UWP

El proceso básico de uso de la API de hospedaje de XAML para hospedar un control de UWP sigue estos pasos generales:

1. Inicialice el marco XAML de UWP para el subproceso actual antes de que la aplicación cree cualquiera de los objetos [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que hospedará. Hay varias maneras de hacerlo, dependiendo de Cuándo se planea crear el objeto [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) que hospedará los controles.

    * Si la aplicación crea el objeto **DesktopWindowXamlSource** antes de crear cualquiera de los objetos **Windows. UI. Xaml. UIElement** que va a hospedar, este marco de trabajo se inicializará automáticamente cuando cree una instancia del  **Objeto DesktopWindowXamlSource** . En este escenario, no es necesario agregar ningún código propio para inicializar el marco de trabajo.

    * Sin embargo, si la aplicación crea los objetos **Windows. UI. Xaml. UIElement** antes de crear el objeto **DesktopWindowXamlSource** que los hospedará, la aplicación debe llamar al método estático [. Método WindowsXamlManager. InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) para inicializar explícitamente el marco XAML de UWP antes de que se creen instancias de los objetos **Windows. UI. Xaml. UIElement** . Normalmente, la aplicación debe llamar a este método cuando se crea una instancia del elemento de la interfaz de usuario principal que hospeda el **DesktopWindowXamlSource** .

    > [!NOTE]
    > Este método devuelve un objeto [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contiene una referencia al marco XAML de UWP. Puede crear tantos objetos **WindowsXamlManager** como desee en un subproceso determinado. Sin embargo, dado que cada objeto contiene una referencia al marco XAML de UWP, debe eliminar los objetos para asegurarse de que se liberen los recursos XAML.

2. Cree un objeto [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) y asócielo a un elemento primario de la interfaz de usuario de la aplicación que esté asociada a un identificador de ventana.

    Para ello, debe seguir estos pasos:

    1. Cree un objeto **DesktopWindowXamlSource** y conviértalo en la interfaz com **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** .
        > [!NOTE]
        > Estas interfaces se declaran en el archivo de encabezado **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h** en el Windows SDK. De forma predeterminada, este archivo se encuentra en% ProgramFiles (x86)% \ Windows Kits\10\Include \\ < número de compilación \> \um.

    2. Llame al método **AttachToWindow** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** y pase el identificador de ventana del elemento primario de la interfaz de usuario en la aplicación.

    3. Establezca el tamaño inicial de la ventana secundaria interna contenida en **DesktopWindowXamlSource**. De forma predeterminada, esta ventana secundaria interna se establece en un ancho y un alto de 0. Si no establece el tamaño de la ventana, los controles de UWP que agregue a **DesktopWindowXamlSource** no estarán visibles. Para tener acceso a la ventana secundaria interna de **DesktopWindowXamlSource**, use la propiedad **WindowHandle** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** .

3. Por último, asigne el objeto **Windows. UI. Xaml. UIElement** que quiere hospedar a la propiedad [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) del objeto **DesktopWindowXamlSource** .

En los siguientes pasos y ejemplos de código se muestra cómo implementar el proceso anterior:

1. En la carpeta **archivos de código fuente** del proyecto, abra el archivo predeterminado **WindowsProject. cpp** . Elimine todo el contenido del archivo y agregue las siguientes instrucciones `include` y `using`. Además de los encabezados C++ y espacios de nombres estándar y UWP, estas instrucciones incluyen varios elementos específicos de las islas XAML.

    ```cppwinrt
    #include <windows.h>
    #include <stdlib.h>
    #include <string.h>

    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    ```

3. Copie el código siguiente después de la sección anterior. Este código define la función **WinMain** para la aplicación. Esta función Inicializa una ventana básica y usa la API de hospedaje XAML para hospedar un control **TextBlock** de UWP sencillo en la ventana.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND, UINT, WPARAM, LPARAM);

    HWND _hWnd;
    HWND _childhWnd;
    HINSTANCE _hInstance;

    int CALLBACK WinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE hPrevInstance, _In_ LPSTR lpCmdLine, _In_ int nCmdShow)
    {
        _hInstance = hInstance;

        // The main window class name.
        const wchar_t szWindowClass[] = L"Win32DesktopApp";
        WNDCLASSEX windowClass = { };

        windowClass.cbSize = sizeof(WNDCLASSEX);
        windowClass.lpfnWndProc = WindowProc;
        windowClass.hInstance = hInstance;
        windowClass.lpszClassName = szWindowClass;
        windowClass.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1);

        windowClass.hIconSm = LoadIcon(windowClass.hInstance, IDI_APPLICATION);

        if (RegisterClassEx(&windowClass) == NULL)
        {
            MessageBox(NULL, L"Windows registration failed!", L"Error", NULL);
            return 0;
        }

        _hWnd = CreateWindow(
            szWindowClass,
            L"Windows c++ Win32 Desktop App",
            WS_OVERLAPPEDWINDOW | WS_VISIBLE,
            CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT,
            NULL,
            NULL,
            hInstance,
            NULL
        );
        if (_hWnd == NULL)
        {
            MessageBox(NULL, L"Call to CreateWindow failed!", L"Error", NULL);
            return 0;
        }


        // Begin XAML Island section.

        // The call to winrt::init_apartment initializes COM; by default, in a multithreaded apartment.
        winrt::init_apartment(apartment_type::single_threaded);

        // Initialize the XAML framework's core window for the current thread.
        WindowsXamlManager winxamlmanager = WindowsXamlManager::InitializeForCurrentThread();

        // This DesktopWindowXamlSource is the object that enables a non-UWP desktop application 
        // to host UWP controls in any UI element that is associated with a window handle (HWND).
        DesktopWindowXamlSource desktopSource;

        // Get handle to the core window.
        auto interop = desktopSource.as<IDesktopWindowXamlSourceNative>();

        // Parent the DesktopWindowXamlSource object to the current window.
        check_hresult(interop->AttachToWindow(_hWnd));

        // This HWND will be the window handler for the XAML Island: A child window that contains XAML.  
        HWND hWndXamlIsland = nullptr;

        // Get the new child window's HWND. 
        interop->get_WindowHandle(&hWndXamlIsland);

        // Update the XAML Island window size because initially it is 0,0.
        SetWindowPos(hWndXamlIsland, 0, 200, 100, 800, 200, SWP_SHOWWINDOW);

        // Create the XAML content.
        Windows::UI::Xaml::Controls::StackPanel xamlContainer;
        xamlContainer.Background(Windows::UI::Xaml::Media::SolidColorBrush{ Windows::UI::Colors::LightGray() });

        Windows::UI::Xaml::Controls::TextBlock tb;
        tb.Text(L"Hello World from Xaml Islands!");
        tb.VerticalAlignment(Windows::UI::Xaml::VerticalAlignment::Center);
        tb.HorizontalAlignment(Windows::UI::Xaml::HorizontalAlignment::Center);
        tb.FontSize(48);

        xamlContainer.Children().Append(tb);
        xamlContainer.UpdateLayout();
        desktopSource.Content(xamlContainer);

        // End XAML Island section.

        ShowWindow(_hWnd, nCmdShow);
        UpdateWindow(_hWnd);

        //Message loop:
        MSG msg = { };
        while (GetMessage(&msg, NULL, 0, 0))
        {
            TranslateMessage(&msg);
            DispatchMessage(&msg);
        }

        return 0;
    }
    ```

4. Copie el código siguiente después de la sección anterior. Este código define el [procedimiento de ventana](https://docs.microsoft.com/en-us/windows/win32/learnwin32/writing-the-window-procedure) para la ventana.

    ```cppwinrt
    LRESULT CALLBACK WindowProc(HWND hWnd, UINT messageCode, WPARAM wParam, LPARAM lParam)
    {
        PAINTSTRUCT ps;
        HDC hdc;
        wchar_t greeting[] = L"Hello World in Win32!";
        RECT rcClient;

        switch (messageCode)
        {
            case WM_PAINT:
                if (hWnd == _hWnd)
                {
                    hdc = BeginPaint(hWnd, &ps);
                    TextOut(hdc, 300, 5, greeting, wcslen(greeting));
                    EndPaint(hWnd, &ps);
                }
                break;
            case WM_DESTROY:
                PostQuitMessage(0);
                break;

            // Create main window
            case WM_CREATE:
                _childhWnd = CreateWindowEx(0, L"ChildWClass", NULL, WS_CHILD | WS_BORDER, 0, 0, 0, 0, hWnd, NULL, _hInstance, NULL);
                return 0;

            // Main window changed size
            case WM_SIZE:
                // Get the dimensions of the main window's client
                // area, and enumerate the child windows. Pass the
                // dimensions to the child windows during enumeration.
                GetClientRect(hWnd, &rcClient);
                MoveWindow(_childhWnd, 200, 200, 400, 500, TRUE);
                ShowWindow(_childhWnd, SW_SHOW);

                return 0;

                // Process other messages.

            default:
                return DefWindowProc(hWnd, messageCode, wParam, lParam);
                break;
        }

        return 0;
    }
    ```

5. Guarde el archivo de código y compile y ejecute la aplicación. Confirma que ves el control **TextBlock** de UWP en la ventana de la aplicación.
    > [!NOTE]
    > Es posible que vea varias advertencias de compilación, como `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` y `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Estas advertencias son problemas conocidos con las herramientas actuales y los paquetes NuGet, y se pueden omitir.

Para obtener ejemplos completos que muestran estas tareas, vea los siguientes archivos de código:

* **C++32**
  * Vea el archivo [HelloWindowsDesktop. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_SimpleApp/Win32DesktopApp/HelloWindowsDesktop.cpp) en el [ejemplo de isla XAML simple](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_SimpleApp).
  * Vea el archivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) en la [isla de XAML con el ejemplo de control personalizado](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

* **WPF:** Consulte los archivos [WindowsXamlHostBase.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.  

* **Windows Forms:** Consulte los archivos [WindowsXamlHostBase.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="host-a-custom-uwp-control"></a>Hospedar un control UWP personalizado

El proceso para hospedar un control de UWP personalizado (ya sea un control definido por el usuario o un control proporcionado por un tercero C++ ) en una aplicación Win32 comienza con los mismos pasos que [hospedan un control estándar](#host-a-standard-uwp-control). Sin embargo, el hospedaje de un control UWP personalizado requiere proyectos y pasos adicionales.

Para hospedar un control UWP personalizado, necesitará los siguientes proyectos y componentes:

* **El proyecto y el código fuente de la aplicación**. Asegúrese de haber configurado el proyecto para cumplir los [requisitos previos](#prerequisites) para hospedar islas XAML.

* **Control UWP personalizado**. Necesitará el código fuente para el control personalizado de UWP que quiera hospedar para que pueda compilarlo con la aplicación. Normalmente, el control personalizado se define en un proyecto de biblioteca de clases de UWP al que se hace referencia en C++ la misma solución que el proyecto de Win32.

* **Un proyecto de aplicación para UWP que define un objeto XamlApplication**. El C++ proyecto de Win32 debe tener acceso a una instancia de la clase `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` proporcionada por el kit de herramientas de la comunidad de Windows. Este tipo actúa como proveedor de metadatos raíz para cargar los metadatos de los tipos XAML de UWP personalizados en los ensamblados del directorio actual de la aplicación. La manera recomendada de hacerlo es agregar un proyecto **aplicación vacía (Windows universal)** a la misma solución que el proyecto de C++ Win32 y revisar la clase `App` predeterminada en este proyecto.
  > [!NOTE]
  > La solución solo puede contener un proyecto que define un objeto de `XamlApplication`. Todos los controles personalizados de UWP de la aplicación comparten el mismo objeto de `XamlApplication`. El proyecto que define el objeto de `XamlApplication` debe incluir referencias a todas las demás bibliotecas y proyectos de UWP que se usan para hospedar controles de UWP en la isla XAML.

Para hospedar un control personalizado de UWP C++ en una aplicación Win32, siga estos pasos generales.

1. En la solución que contiene el C++ proyecto de aplicación de escritorio de Win32, agregue un proyecto **aplicación vacía (Windows universal)** y configúrelo siguiendo las instrucciones detalladas de [esta sección](host-custom-control-with-xaml-islands.md#create-a-xamlapplication-object-in-a-uwp-app-project) en el tutorial de WPF relacionado.

2. En la misma solución, agregue el proyecto que contiene el código fuente para el control XAML de UWP personalizado (suele ser un proyecto de biblioteca de clases de UWP) y compile el proyecto.

3. En el proyecto de aplicación de UWP, agregue una referencia al proyecto de biblioteca de clases de UWP.

4. En el C++ proyecto de Win32, agregue una referencia al proyecto de aplicación de UWP y al proyecto de biblioteca de clases de UWP en la solución.

5. Siga el proceso descrito en la sección [uso de la API de hospedaje de XAML para hospedar un control de UWP](#use-the-xaml-hosting-api-to-host-a-uwp-control) para hospedar el control personalizado en una isla de XAML de la aplicación. Asigne una instancia del control personalizado para hospedar la propiedad [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) del objeto **DesktopWindowXamlSource** en el código.

Para obtener un ejemplo completo de C++ una aplicación Win32, vea los siguientes proyectos en el ejemplo de la [isla XAML con control personalizado](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App):

* [SampleUserControl](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleUserControl): este proyecto implementa un control XAML de UWP personalizado denominado `MyUserControl` que contiene un cuadro de texto, varios botones y un cuadro combinado.
* [MyApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/MyApp): se trata de un proyecto de aplicación para UWP con los cambios descritos anteriormente.
* [SampleCppApp](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp): este es el C++ proyecto de aplicación de Win32 que hospeda el control XAML de UWP personalizado en una isla XAML.

## <a name="handle-keyboard-layout-and-dpi"></a>Controlar el teclado, el diseño y el PPP

En las secciones siguientes se proporcionan instrucciones y vínculos a ejemplos de código sobre cómo controlar los escenarios de teclado y de diseño.

* [Entrada de teclado](#keyboard-input)
* [Navegación con el foco de teclado](#keyboard-focus-navigation)
* [Controlar los cambios de diseño](#handle-layout-changes)
* [Controlar los cambios de PPP](#handle-dpi-changes)

### <a name="keyboard-input"></a>Entrada de teclado

Para controlar correctamente la entrada del teclado para cada isla XAML, la aplicación debe pasar todos los mensajes de Windows al marco XAML de UWP para que ciertos mensajes se puedan procesar correctamente. Para ello, en algún lugar de la aplicación que pueda tener acceso al bucle de mensajes, convierta el objeto **DesktopWindowXamlSource** de cada isla XAML en una interfaz com **IDesktopWindowXamlSourceNative2** . A continuación, llame al método **PreTranslateMessage** de esta interfaz y pase el mensaje actual.

  * Win32:: la aplicación puede llamar a **PreTranslateMessage** directamente en su bucle de mensajes principal. **C++** Para obtener un ejemplo, vea el archivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp#L6) en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).

  * **WPF:** La aplicación puede llamar a **PreTranslateMessage** desde el controlador de eventos para el evento [ComponentDispatcher. ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) . Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.Focus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) en el kit de herramientas de la comunidad de Windows.

  * **Windows Forms:** La aplicación puede llamar a **PreTranslateMessage** desde una invalidación para el método [control. PreprocessMessage](https://docs.microsoft.com/en-us/dotnet/api/system.windows.forms.control.preprocessmessage) . Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.KeyboardFocus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) en el kit de herramientas de la comunidad de Windows.

### <a name="keyboard-focus-navigation"></a>Navegación con el foco de teclado

Cuando el usuario navega por los elementos de la interfaz de usuario de la aplicación mediante el teclado (por ejemplo, presionando la tecla **Tab** o dirección/flecha), tendrá que trasladar el foco al objeto **DesktopWindowXamlSource** mediante programación. Cuando la navegación del teclado del usuario alcance el **DesktopWindowXamlSource**, mueva el foco al primer objeto [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) en el orden de navegación de la interfaz de usuario, continúe moviendo el foco a lo siguiente  **Los objetos Windows. UI. Xaml. UIElement** a medida que el usuario recorre los elementos y, a continuación, mueve el foco de nuevo fuera de **DesktopWindowXamlSource** y al elemento de la interfaz de usuario principal.  

La API de hospedaje XAML de UWP proporciona varios tipos y miembros para ayudarle a llevar a cabo estas tareas.

* Cuando la navegación del teclado entra en el **DesktopWindowXamlSource**, se genera el evento [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) . Controle este evento y mueva mediante programación el foco al primer host **Windows. UI. Xaml. UIElement** hospedado mediante el método [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus) .

* Cuando el usuario está en el último elemento enfocable en el **DesktopWindowXamlSource** y presiona la tecla **Tab** o una tecla de dirección, se genera el evento [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested) . Controla este evento y mueve mediante programación el foco al siguiente elemento enfocable en la aplicación host. Por ejemplo, en una aplicación WPF donde **DesktopWindowXamlSource** se hospeda en [System. Windows. Interop. HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost), puede usar el método [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) para transferir el foco al siguiente elemento enfocable en la aplicación host.

Para obtener ejemplos que muestran cómo hacerlo en el contexto de una aplicación de ejemplo funcional, vea los siguientes archivos de código:

  * /Win32: vea el archivo [XamlBridge. cpp](https://github.com/marb2000/XamlIslands/blob/master/1903_Samples/CppWinRT_Win32_App/SampleCppApp/XamlBridge.cpp) en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/tree/master/1903_Samples/CppWinRT_Win32_App).  **C++**

  * **WPF:** Vea el archivo [WindowsXamlHostBase.Focus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) en el kit de herramientas de la comunidad de Windows.  

  * **Windows Forms:** Vea el archivo [WindowsXamlHostBase.KeyboardFocus.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) en el kit de herramientas de la comunidad de Windows.

### <a name="handle-layout-changes"></a>Controlar los cambios de diseño

Cuando el usuario cambie el tamaño del elemento de la interfaz de usuario principal, deberá controlar los cambios de diseño necesarios para asegurarse de que los controles de UWP se muestran como se espera. Estos son algunos escenarios importantes que se deben tener en cuenta.

* En una C++ aplicación Win32, cuando la aplicación controla el mensaje WM_SIZE, puede cambiar la posición de la isla XAML hospedada mediante la función [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) . Para obtener un ejemplo, vea el archivo de código [SampleApp. cpp](https://github.com/marb2000/XamlIslands/blob/master/19H1_Insider_Samples/CppWin32App_With_Island/SampleCppApp/SampleApp.cpp#L191) en el [ C++ ejemplo de Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island).

* Cuando el elemento primario de la interfaz de usuario debe obtener el tamaño del área rectangular necesaria para ajustarse a **Windows. UI. Xaml. UIElement** que hospeda en **DesktopWindowXamlSource**, llame al método [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure) de **Windows. UI. Xaml. UIElement.** . Por ejemplo:

    * En una aplicación de WPF podría hacerlo desde el método [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride) del [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación Windows Forms puede hacer esto desde el método [getPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize) del [control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHostBase.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

* Cuando cambie el tamaño del elemento primario de la interfaz de usuario, llame al método [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange) de la raíz **Windows. UI. Xaml. UIElement** que hospeda en **DesktopWindowXamlSource**. Por ejemplo:

    * En una aplicación de WPF podría hacerlo desde el método [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride) del objeto [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHost.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

    * En una aplicación Windows Forms puede hacer esto desde el controlador del evento [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged) del [control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control) que hospeda **DesktopWindowXamlSource**. Para obtener un ejemplo, vea el archivo [WindowsXamlHost.layout.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) en el kit de herramientas de la comunidad de Windows.

### <a name="handle-dpi-changes"></a>Controlar los cambios de PPP

El marco XAML de UWP administra automáticamente los cambios de PPP para los controles de UWP hospedados (por ejemplo, cuando el usuario arrastra la ventana entre monitores con distintos PPP de pantalla). Para disfrutar de la mejor experiencia, se recomienda que la aplicación Windows Forms, C++ WPF o Win32 esté configurada para que sea compatible con PPP por monitor.

Para configurar la aplicación para que tenga en cuenta el valor de PPP por monitor, agregue un [manifiesto de ensamblado en paralelo](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) al proyecto y establezca el **\<dpiAwareness \>** elemento en **PerMonitorV2**. Para obtener más información sobre este valor, vea la descripción de [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

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

## <a name="troubleshooting"></a>de solución de problemas

### <a name="error-using-uwp-xaml-hosting-api-in-a-uwp-app"></a>Error al usar la API de hospedaje XAML de UWP en una aplicación de UWP

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "no se puede activar DesktopWindowXamlSource. Este tipo no se puede usar en una aplicación de UWP ". o "no se puede activar WindowsXamlManager. Este tipo no se puede usar en una aplicación de UWP ". | Este error indica que está intentando usar la API de hospedaje XAML de UWP (específicamente, está intentando crear una instancia de los tipos [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) o [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) ) en una aplicación para UWP. La API de hospedaje XAML de UWP solo está pensada para usarse en aplicaciones de escritorio que no son de UWP, como C++ WPF, Windows Forms y aplicaciones Win32. |

### <a name="error-trying-to-use-the-windowsxamlmanager-or-desktopwindowxamlsource-types"></a>Error al intentar usar los tipos WindowsXamlManager o DesktopWindowXamlSource

| Problema | Resolución |
|-------|------------|
| La aplicación recibe una excepción con el siguiente mensaje: "WindowsXamlManager y DesktopWindowXamlSource son compatibles con las aplicaciones que tienen como destino la versión de Windows 10.0.18226.0 y versiones posteriores. Compruebe el manifiesto de aplicación o el manifiesto del paquete y asegúrese de que la propiedad MaxTestedVersion esté actualizada. " | Este error indica que la aplicación intentó usar los tipos **WindowsXamlManager** o **DESKTOPWINDOWXAMLSOURCE** en la API de hospedaje XAML de UWP, pero el sistema operativo no puede determinar si la aplicación se ha creado para tener como destino Windows 10, versión 1903 o posterior. La API de hospedaje XAML de UWP se presentó por primera vez como una vista previa en una versión anterior de Windows 10, pero solo se admite a partir de Windows 10, versión 1903.</p></p>Para resolver este problema, cree un paquete de MSIX para la aplicación y ejecútelo desde el paquete, o bien instale el paquete NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) en el proyecto. Para obtener más información, consulta [esta sección](#configure-your-project-for-app-deployment). |

### <a name="error-attaching-to-a-window-on-a-different-thread"></a>Error al asociar a una ventana en un subproceso diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "error en el método AttachToWindow porque el HWND especificado se creó en un subproceso diferente". | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative:: AttachToWindow** y le pasó el HWND de una ventana creada en un subproceso diferente. Debe pasar este método el HWND de una ventana creada en el mismo subproceso que el código desde el que llama al método. |

### <a name="error-attaching-to-a-window-on-a-different-top-level-window"></a>Error al asociar a una ventana en una ventana de nivel superior diferente

| Problema | Resolución |
|-------|------------|
| La aplicación recibe un **COMException** con el siguiente mensaje: "error en el método AttachToWindow porque el HWND especificado desciende de una ventana de nivel superior distinta de la del HWND que se pasó previamente a AttachToWindow en el mismo subproceso". | Este error indica que la aplicación llamó al método **IDesktopWindowXamlSourceNative:: AttachToWindow** y le pasó el HWND de una ventana que desciende de una ventana de nivel superior diferente de la ventana especificada en una llamada anterior a este método. en el mismo subproceso.</p></p>Una vez que la aplicación llama a **AttachToWindow** en un subproceso determinado, todos los demás objetos de [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) del mismo subproceso solo pueden asociarse a ventanas que son descendientes de la misma ventana de nivel superior que se pasó en la primera llamada a **AttachToWindow**. Cuando se cierran todos los objetos **DesktopWindowXamlSource** para un subproceso determinado, el siguiente **DesktopWindowXamlSource** se vuelve a adjuntar a cualquier ventana de nuevo.</p></p>Para resolver este problema, cierre todos los objetos **DesktopWindowXamlSource** enlazados a otras ventanas de nivel superior en este subproceso o cree un nuevo subproceso para este **DesktopWindowXamlSource**. |

## <a name="related-topics"></a>Temas relacionados

* [Controles de UWP en aplicaciones de escritorio](xaml-islands.md)
* [C++Ejemplo de islas XAML de Win32](https://github.com/marb2000/XamlIslands/tree/master/19H1_Insider_Samples/CppWin32App_With_Island)
