---
description: En este artículo se muestra cómo hospedar un control estándar de C++ UWP en una aplicación Win32 mediante la API de hospedaje de XAML.
title: Hospedar un control estándar de UWP C++ en una aplicación Win32 mediante islas XAML
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, Islas XAML, controles ajustados, controles estándar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 08308c7bca3cd7f39b08c836e43d791a3fda048f
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226279"
---
# <a name="host-a-standard-uwp-control-in-a-c-win32-app"></a>Hospedar un control estándar de UWP C++ en una aplicación Win32

En este artículo se muestra cómo usar la [API de hospedaje XAML de UWP](using-the-xaml-hosting-api.md) para hospedar un control estándar de UWP (es decir, un control proporcionó por el C++ Windows SDK) en una nueva aplicación de Win32. El código se basa en el [ejemplo de isla XAML simple](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App)y en esta sección se describen algunas de las partes más importantes del código. Si tiene un proyecto de C++ aplicación de Win32 existente, puede adaptar estos pasos y ejemplos de código para el proyecto.

> [!NOTE]
> El escenario que se muestra en este artículo no admite la edición directa del marcado XAML para los controles de UWP hospedados en la aplicación. Este escenario restringe la modificación de la apariencia y el comportamiento de los controles de UWP hospedados a través del código. Para obtener instrucciones que le permitan editar directamente el marcado XAML al hospedar controles de UWP, vea [hospedar un C++ control de UWP personalizado en una aplicación Win32](host-custom-control-with-xaml-islands-cpp.md).

## <a name="create-a-desktop-application-project"></a>Crear un proyecto de aplicación de escritorio

1. En Visual Studio 2019 con el SDK de Windows 10, versión 1903 (versión 10.0.18362) o una versión posterior instalada, cree un nuevo proyecto de **aplicación de escritorio de Windows** y asígnele el nombre **MyDesktopWin32App**. Este tipo de proyecto está disponible en **C++** los filtros de proyectos de **escritorio** , **Windows**y.

2. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución, haga clic en **redestinar solución**, seleccione el **10.0.18362.0** o una versión posterior del SDK y, a continuación, haga clic en **Aceptar**.

3. Instale el paquete NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para habilitar la compatibilidad con [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) en el proyecto:

    1. Haga clic con el botón derecho en el proyecto en **Explorador de soluciones** y elija **administrar paquetes NuGet**.
    2. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instale la versión más reciente de este paquete.

    > [!NOTE]
    > En el caso de los proyectos nuevos, puede instalar la [ C++extensión de Visual Studio (VSIX)/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) y usar una C++de las plantillas de proyecto/WinRT incluidas en esa extensión. Para obtener más información, consulta [este artículo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

4. Instale el paquete NuGet [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) :

    1. En la ventana **Administrador de paquetes NuGet** , asegúrese de que esté seleccionada la opción **incluir versión preliminar** .
    2. Seleccione la pestaña **examinar** , busque el paquete **Microsoft. Toolkit. Win32. UI. SDK** e instale la versión v 6.0.0 (o posterior) de este paquete. Este paquete proporciona varios recursos de compilación y tiempo de ejecución que permiten a las islas XAML trabajar en la aplicación.

5. Establezca el valor de `maxVersionTested` en el [manifiesto de aplicación](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) para especificar que la aplicación es compatible con Windows 10, versión 1903 o posterior.

    1. Si aún no tiene un manifiesto de aplicación en el proyecto, agregue un nuevo archivo XML al proyecto y asígnele el nombre **app. manifest**.
    2. En el manifiesto de aplicación, incluya el elemento de **compatibilidad** y los elementos secundarios que se muestran en el ejemplo siguiente. Reemplace el atributo **ID** del elemento **maxVersionTested** por el número de versión de Windows 10 que tiene como destino (debe ser windows 10, versión 1903 o una versión posterior).

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
            <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">
                <application>
                    <!-- Windows 10 -->
                    <maxversiontested Id="10.0.18362.0"/>
                    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
                </application>
            </compatibility>
        </assembly>
        ```

## <a name="use-the-xaml-hosting-api-to-host-a-uwp-control"></a>Uso de la API de hospedaje de XAML para hospedar un control de UWP

El proceso básico de uso de la API de hospedaje de XAML para hospedar un control de UWP sigue estos pasos generales:

1. Inicialice el marco XAML de UWP para el subproceso actual antes de que la aplicación cree cualquiera de los objetos [Windows. UI. Xaml. UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) que hospedará. Hay varias maneras de hacerlo, dependiendo de Cuándo se planea crear el objeto [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) que hospedará los controles.

    * Si la aplicación crea el objeto **DesktopWindowXamlSource** antes de crear cualquiera de los objetos **Windows. UI. Xaml. UIElement** que va a hospedar, este marco de trabajo se inicializará automáticamente cuando cree una instancia del objeto **DesktopWindowXamlSource** . En este escenario, no es necesario agregar ningún código propio para inicializar el marco de trabajo.

    * Sin embargo, si la aplicación crea los objetos **Windows. UI. Xaml. UIElement** antes de crear el objeto **DesktopWindowXamlSource** que los hospedará, la aplicación debe llamar al método [WindowsXamlManager. InitializeForCurrentThread](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) estático para INICIALIZAr explícitamente el marco XAML de UWP antes de que se creen instancias de los objetos **Windows. UI. Xaml. UIElement** . Normalmente, la aplicación debe llamar a este método cuando se crea una instancia del elemento de la interfaz de usuario principal que hospeda el **DesktopWindowXamlSource** .

    > [!NOTE]
    > Este método devuelve un objeto [WindowsXamlManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contiene una referencia al marco XAML de UWP. Puede crear tantos objetos **WindowsXamlManager** como desee en un subproceso determinado. Sin embargo, dado que cada objeto contiene una referencia al marco XAML de UWP, debe eliminar los objetos para asegurarse de que se liberen los recursos XAML.

2. Cree un objeto [DesktopWindowXamlSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) y asócielo a un elemento primario de la interfaz de usuario de la aplicación que esté asociada a un identificador de ventana.

    Para ello, debe seguir estos pasos:

    1. Cree un objeto **DesktopWindowXamlSource** y conviértalo en la interfaz com **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** .
        > [!NOTE]
        > Estas interfaces se declaran en el archivo de encabezado **Windows. UI. Xaml. Hosting. desktopwindowxamlsource. h** en el Windows SDK. De forma predeterminada, este archivo se encuentra en% ProgramFiles (x86)% \ Windows Kits\10\Include\\< número de compilación\>\um.

    2. Llame al método **AttachToWindow** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** y pase el identificador de ventana del elemento primario de la interfaz de usuario en la aplicación.

    3. Establezca el tamaño inicial de la ventana secundaria interna contenida en **DesktopWindowXamlSource**. De forma predeterminada, esta ventana secundaria interna se establece en un ancho y un alto de 0. Si no establece el tamaño de la ventana, los controles de UWP que agregue a **DesktopWindowXamlSource** no estarán visibles. Para tener acceso a la ventana secundaria interna de **DesktopWindowXamlSource**, use la propiedad **WindowHandle** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** .

3. Por último, asigne el objeto **Windows. UI. Xaml. UIElement** que quiere hospedar a la propiedad [Content](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) del objeto **DesktopWindowXamlSource** .

En los siguientes pasos y ejemplos de código se muestra cómo implementar el proceso anterior:

1. En la carpeta **archivos de código fuente** del proyecto, abra el archivo predeterminado **MyDesktopWin32App. cpp** . Elimine todo el contenido del archivo y agregue las siguientes instrucciones `include` y `using`. Además de los encabezados C++ y espacios de nombres estándar y UWP, estas instrucciones incluyen varios elementos específicos de las islas XAML.

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

4. Copie el código siguiente después de la sección anterior. Este código define el [procedimiento de ventana](https://docs.microsoft.com/windows/win32/learnwin32/writing-the-window-procedure) para la ventana.

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
  * Vea el archivo [HelloWindowsDesktop. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp) .
  * Vea el archivo [XamlBridge. cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp) .
* **WPF:** Consulte los archivos [WindowsXamlHostBase.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.  
* **Windows Forms:** Consulte los archivos [WindowsXamlHostBase.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.CS](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="package-the-app"></a>Empaquetar la aplicación

Opcionalmente, puede empaquetar la aplicación en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación. MSIX es la tecnología moderna de empaquetado de aplicaciones para Windows y se basa en una combinación de tecnologías de instalación de MSI,. appx, App-V y ClickOnce.

Las instrucciones siguientes muestran cómo empaquetar todos los componentes de la solución en un paquete de MSIX mediante el proyecto de paquete de [aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) en Visual Studio 2019. Estos pasos solo son necesarios si desea empaquetar la aplicación en un paquete MSIX.

> [!NOTE]
> Si decide no empaquetar la aplicación en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación, los equipos que ejecutan la aplicación deben tener instalado [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) .

1. Agregue un nuevo [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Cuando cree el proyecto, seleccione **Windows 10, versión 1903 (10,0; Compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haga clic con el botón secundario en el nodo **aplicaciones** y elija **Agregar referencia**. En la lista de proyectos, seleccione el C++proyecto de aplicación de escritorio/Win32 en la solución y haga clic en **Aceptar**.

3. Compile y ejecute el proyecto de empaquetado. Confirme que la aplicación se ejecuta y muestra los controles de UWP según lo previsto.

## <a name="next-steps"></a>Pasos siguientes

Los ejemplos de código de este artículo le ayudarán a comenzar con el escenario básico para hospedar un control C++ estándar de UWP en una aplicación Win32. En las secciones siguientes se presentan escenarios adicionales que es posible que la aplicación necesite admitir.

### <a name="host-a-custom-uwp-control"></a>Hospedar un control UWP personalizado

En muchos escenarios, puede que necesite hospedar un control XAML de UWP personalizado que contenga varios controles individuales que funcionen conjuntamente. El proceso para hospedar un control de UWP personalizado (ya sea un control definido por el usuario o un control proporcionado por un tercero C++ ) en una aplicación Win32 es más complejo que hospedar un control estándar y requiere código adicional.

Para ver un tutorial completo, vea [hospedar un control de UWP C++ personalizado en una aplicación Win32 mediante la API de hospedaje de XAML](host-custom-control-with-xaml-islands-cpp.md).

### <a name="advanced-scenarios"></a>Escenarios avanzados

Muchas aplicaciones de escritorio que hospedan islas XAML deberán controlar escenarios adicionales para proporcionar una experiencia de usuario fluida. Por ejemplo, las aplicaciones de escritorio pueden necesitar controlar la entrada del teclado en las islas XAML, la navegación centrada entre las islas XAML y otros elementos de la interfaz de usuario y los cambios de diseño.

Para obtener más información sobre cómo controlar estos escenarios y punteros a ejemplos de código relacionados, vea [escenarios avanzados para C++ islas XAML en aplicaciones Win32](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Temas relacionados

* [Hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)](xaml-islands.md)
* [Uso de la API de hospedaje XAML de C++ UWP en una aplicación Win32](using-the-xaml-hosting-api.md)
* [Hospedar un control personalizado de UWP C++ en una aplicación Win32](host-custom-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados para Islas XAML en C++ aplicaciones Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Ejemplos de código de islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
