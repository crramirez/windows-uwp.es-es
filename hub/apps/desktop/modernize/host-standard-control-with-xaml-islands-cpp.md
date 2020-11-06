---
description: En este artículo se muestra cómo hospedar un control XAML estándar de WinRT en una aplicación Win32 de C++ mediante la API de hospedaje de XAML.
title: Hospedaje de un control XAML estándar de WinRT en una aplicación Win32 de C++ mediante islas XAML
ms.date: 10/02/2020
ms.topic: article
keywords: Windows 10;uwp;cpp;win32;xaml islands;wrapped controls;standard controls;islas XAML;controles ajustados;controles estándar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: fcad3bfeb5c31a6b3af85e5fd9a0ea72f11d65da
ms.sourcegitcommit: caf4dba6bdfc3c6d9685d10aa9924b170b00bed8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93049516"
---
# <a name="host-a-standard-winrt-xaml-control-in-a-c-win32-app"></a>Hospedaje de un control XAML estándar de WinRT en una aplicación Win32 de C++

En este artículo se muestra cómo usar la [API de hospedaje de XAML de UWP](using-the-xaml-hosting-api.md) para hospedar un control XAML estándar de WinRT (es decir, un control proporcionado por Windows SDK) en una nueva aplicación Win32 de C++. El código se basa en el [ejemplo sencillo de isla XAML](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Standalone_Samples/CppWinRT_Basic_Win32App) y en esta sección se describen algunas de las partes más importantes del código. Si tienes un proyecto de aplicación Win32 de C++ actual, puedes adaptar estos pasos y ejemplos de código para dicho proyecto.

> [!NOTE]
> El escenario que se muestra en este artículo no permite editar directamente el marcado XAML de los controles XAML de WinRT hospedados en la aplicación. Este escenario solo permite modificar la apariencia y el comportamiento de los controles hospedados mediante código. Si quiere ver las instrucciones para editar directamente el marcado XAML al hospedar controles XAML de WinRT, consulte [Hospedaje de un control XAML personalizado de WinRT en una aplicación Win32 de C++](host-custom-control-with-xaml-islands-cpp.md).

## <a name="create-a-desktop-application-project"></a>Creación de un proyecto de aplicación de escritorio

1. En Visual Studio 2019 con el SDK de Windows 10, versión 1903 (versión 10.0.18362) o una versión posterior instalada, crea un nuevo proyecto de **aplicación de escritorio de Windows** y asígnale el nombre **MyDesktopWin32App**. Este tipo de proyecto está disponible en los filtros de proyecto **C++** , **Windows** y **Escritorio**.

2. En el **Explorador de soluciones** , haz clic con el botón derecho en el nodo de la solución, haz clic en **Redestinar solución** , selecciona la versión **10.0.18362.0** del SDK o una posterior y después haz clic en **Aceptar**.

3. Instala el paquete NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para incluir compatibilidad con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) en el proyecto:

    1. Haz clic con el botón derecho en el proyecto en el **Explorador de soluciones** y elige **Administrar paquetes NuGet**.
    2. Selecciona la pestaña **Examinar** , busca el paquete [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instala la última versión de dicho paquete.

    > [!NOTE]
    > En el caso de los proyectos nuevos, puedes instalar la [extensión de Visual Studio para C++/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) y usar una de las plantillas de proyecto de C+/WinRT incluidas en esa extensión. Para obtener más información, consulta [este artículo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

4. En la pestaña **Examinar** de la ventana **Administrador de paquetes NuGet** , busque el paquete [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) de NuGet e instale la versión estable más reciente de dicho paquete. Este paquete incluye varios recursos de compilación y tiempo de ejecución que permiten que las islas XAML funcionen en la aplicación.

5. Establece el valor de `maxVersionTested` en el [manifiesto de aplicación](/windows/desktop/SbsCs/application-manifests) para especificar que la aplicación es compatible con Windows 10, versión 1903 o posterior.

    1. Si aún no tienes un manifiesto de aplicación en el proyecto, agrega un nuevo archivo XML al proyecto y asígnale el nombre **app.manifest**.
    2. En el manifiesto de aplicación, incluye el elemento **compatibility** y los elementos secundarios que se muestran en el ejemplo siguiente. Reemplaza el atributo **Id** del elemento **maxVersionTested** por el número de versión de Windows 10 que tienes como destino (debe ser 10.0.18362 o una versión posterior).

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

6. Agregue una referencia a los metadatos de Windows Runtime:
   1. En el **Explorador de soluciones** , haga clic con el botón derecho en el nodo **Referencias** del proyecto y seleccione **Agregar referencia**.
   2. Haga clic en el botón **Examinar** situado en la parte inferior de la página y navegue hasta la carpeta UnionMetadata en la ruta de instalación del SDK. De forma predeterminada, el SDK se instalará en `C:\Program Files (x86)\Windows Kits\10\UnionMetadata`. 
   3. A continuación, seleccione la carpeta con el nombre de la versión de Windows de destino (por ejemplo, 10.0.18362.0) y, dentro de esa carpeta, seleccione el archivo `Windows.winmd`.
   4. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Agregar referencia**.

## <a name="use-the-xaml-hosting-api-to-host-a-winrt-xaml-control"></a>Uso de la API de hospedaje de XAML para hospedar un control XAML de WinRT

El proceso básico para usar la API de hospedaje de XAML para hospedar un control XAML de WinRT sigue estos pasos generales:

1. Inicializa el marco XAML de UWP para el subproceso actual antes de que la aplicación cree cualquiera de los objetos [Windows.UI.Xaml.UIElement](/uwp/api/windows.ui.xaml.uielement) que hospedará. Hay varias maneras de hacerlo, en función del momento en que planeas crear el objeto [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) que hospedará a los controles.

    * Si la aplicación crea el objeto **DesktopWindowXamlSource** antes de crear cualquiera de los objetos **Windows.UI.Xaml.UIElement** que se van a hospedar, el marco de trabajo se inicializará al crear una instancia del objeto **DesktopWindowXamlSource**. En este escenario, no es necesario agregar ningún código propio para inicializar el marco de trabajo.

    * Sin embargo, si la aplicación crea los objetos **Windows.UI.Xaml.UIElement** antes de crear el objeto **DesktopWindowXamlSource** que los hospedará, la aplicación debe llamar al método estático [WindowsXamlManager.InitializeForCurrentThread](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager.initializeforcurrentthread) para inicializar explícitamente el marco XAML de UWP antes de crear instancias de los objetos **Windows.UI.Xaml.UIElement**. Normalmente, la aplicación debe llamar a este método cuando se crea una instancia del elemento principal de la interfaz de usuario que hospeda a **DesktopWindowXamlSource**.

    > [!NOTE]
    > Este método devuelve un objeto [WindowsXamlManager](/uwp/api/windows.ui.xaml.hosting.windowsxamlmanager) que contiene una referencia al marco XAML de UWP. Puedes crear tantos objetos **WindowsXamlManager** como quieras en un subproceso determinado. Sin embargo, dado que cada objeto contiene una referencia al marco XAML de UWP, debes eliminarlos para asegurarte de que se liberen los recursos XAML.

2. Crea un objeto [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) y asócialo a un elemento principal de la interfaz de usuario de la aplicación que esté asociado con un identificador de ventana.

    Para ello, debes seguir estos pasos:

    1. Crea un objeto **DesktopWindowXamlSource** y conviértelo a la interfaz COM **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2**.
        > [!NOTE]
        > Estas interfaces se declaran en el archivo de encabezado **windows.ui.xaml.hosting.desktopwindowxamlsource.h** en el Windows SDK. De forma predeterminada, este archivo se encuentra en %programfiles(x86)%\Windows Kits\10\Include\\< número de compilación\>\um.

    2. Llama al método **AttachToWindow** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2** y pasa el identificador de ventana del elemento principal de la interfaz de usuario de la aplicación.

    3. Establece el tamaño inicial de la ventana secundaria interna contenida en el objeto **DesktopWindowXamlSource**. De forma predeterminada, esta ventana secundaria interna tiene configurado un ancho y un alto de 0. Si no establece el tamaño de la ventana, los controles XAML de WinRT que agregue al objeto **DesktopWindowXamlSource** no serán visibles. Para acceder a la ventana secundaria interna en **DesktopWindowXamlSource** , utiliza la propiedad **WindowHandle** de la interfaz **IDesktopWindowXamlSourceNative** o **IDesktopWindowXamlSourceNative2**.

3. Por último, asigna el elemento **Windows.UI.Xaml.UIElement** que quieres hospedar a la propiedad [Content](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.content) del objeto **DesktopWindowXamlSource**.

En los pasos y ejemplos de código siguientes se muestra cómo implementar el proceso anterior:

1. En la carpeta **Source files** del proyecto, abre el archivo predeterminado **MyDesktopWin32App.cpp**. Elimina todo el contenido del archivo y agrega las siguientes instrucciones `include` y `using`. Además de los encabezados y espacios de nombre estándar de C++ y UWP, estas instrucciones incluyen varios elementos específicos de las islas XAML.

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

3. Copia el código siguiente después de la sección anterior. En este código se define la función **WinMain** de la aplicación. Esta función inicializa una ventana básica y usa la API de hospedaje de XAML para hospedar un control sencillo **TextBlock** de UWP en la ventana.

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
        // to host WinRT XAML controls in any UI element that is associated with a window handle (HWND).
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

4. Copia el código siguiente después de la sección anterior. En este código se define el [procedimiento de la ventana](/windows/win32/learnwin32/writing-the-window-procedure).

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

5. Guarda el archivo de código y compila y ejecuta la aplicación. Confirma que el control **TextBlock** de UWP se muestra en la ventana de la aplicación.
    > [!NOTE]
    > Es posible que aparezcan varias advertencias de compilación, como `warning C4002:  too many arguments for function-like macro invocation 'GetCurrentTime'` y `manifest authoring warning 81010002: Unrecognized Element "maxversiontested" in namespace "urn:schemas-microsoft-com:compatibility.v1"`. Estas advertencias son sobre problemas conocidos con las herramientas actuales y los paquetes NuGet, por lo que pueden omitirse.

Para obtener ejemplos completos que muestran cómo usar la API de hospedaje de XAML para hospedar un control XAML estándar de WinRT, consulte los siguientes archivos de código:

* **Win32 de C++:**
  * Consulta el archivo [HelloWindowsDesktop.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Standalone_Samples/CppWinRT_Basic_Win32App/Win32DesktopApp/HelloWindowsDesktop.cpp).
  * Consulta el archivo [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp).
* **WPF:** Consulta los archivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.  
* **Windows Forms:** Consulta los archivos [WindowsXamlHostBase.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.cs) y [WindowsXamlHost.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHost.cs) en el kit de herramientas de la comunidad de Windows.

## <a name="package-the-app"></a>Empaquetado de la aplicación

También pueden empaquetar la aplicación en un [paquete MSIX](/windows/msix) para implementarla. MSIX es una tecnología de empaquetado de aplicaciones moderna para Windows y está basada en una combinación de las tecnologías de instalación .msi, .appx, App-V y ClickOnce.

Las instrucciones siguientes muestran cómo empaquetar todos los componentes de la solución en un paquete MSIX mediante el [proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) en Visual Studio 2019. Estos pasos solo son necesarios si quieres empaquetar la aplicación en un paquete MSIX.

> [!NOTE]
> Si decides no empaquetar la aplicación en un [paquete MSIX](/windows/msix) para su implementación, los equipos que ejecuten dicha aplicación deberán tener instalado el [Tiempo de ejecución de Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads).

1. Agrega un nuevo [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Al crear el proyecto, selecciona **Windows 10, versión 1903 (10.0; compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haz clic con el botón derecho en el nodo **Aplicaciones** y elige **Agregar referencia**. En la lista de proyectos, selecciona el proyecto de aplicación de escritorio C++/Win32 en la solución y haz clic en **Aceptar**.

3. Compila y ejecuta el proyecto de empaquetado. Confirme si la aplicación se ejecuta y muestra los controles XAML de WinRT según lo previsto.

## <a name="next-steps"></a>Pasos siguientes

Los ejemplos de código de este artículo son una introducción al escenario básico para hospedar un control XAML estándar de WinRT en una aplicación Win32 de C++. En las secciones siguientes se presentan escenarios adicionales que la aplicación quizá debería admitir.

### <a name="host-a-custom-winrt-xaml-control"></a>Hospedaje de un control XAML personalizado de WinRT

En muchos casos, quizá necesites hospedar un control XAML de UWP personalizado que contenga varios controles individuales que funcionan de forma conjunta. El proceso para hospedar un control personalizado (ya sea un control definido por el usuario o un control proporcionado por un tercero) en una aplicación Win32 de C++ es más complejo que hospedar un control estándar y requiere código adicional.

Para ver un tutorial completo, consulte [Hospedaje de un control XAML personalizado de WinRT en una aplicación Win32 de C++ mediante la API de hospedaje de XAML](host-custom-control-with-xaml-islands-cpp.md).

### <a name="advanced-scenarios"></a>Escenarios avanzados

Muchas aplicaciones de escritorio que hospedan islas XAML deberán controlar escenarios adicionales para ofrecer una experiencia de usuario fluida. Por ejemplo, es posible que las aplicaciones de escritorio deban controlar la entrada del teclado en las islas XAML y la navegación del foco entre islas XAML y otros elementos de la interfaz de usuario, así como los cambios de diseño.

Para obtener más información acerca de cómo controlar estos escenarios y obtener vínculos a ejemplos de código relacionados, consulta [Escenarios avanzados para islas XAML en aplicaciones Win32 de C++](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Temas relacionados

* [Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md)
* [Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++](using-the-xaml-hosting-api.md)
* [Hospedaje de un control XAML personalizado de WinRT en una aplicación Win32 de C++](host-custom-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados para islas XAML en aplicaciones Win32 en C++](advanced-scenarios-xaml-islands-cpp.md)
* [Ejemplos de código de las islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
