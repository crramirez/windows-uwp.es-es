---
author: stevewhims
description: C++ / WinRT puede ayudarte a crear componentes de COM clásicos, al igual que le ayuda a crear clases en tiempo de ejecución de Windows.
title: Crear componentes de COM con C++ / WinRT
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, autor, COM, componente
ms.localizationpriority: medium
ms.openlocfilehash: 729cfae39f302ae6b5bae275d9e28a39f3d9503b
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3935695"
---
# <a name="author-com-components-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Crear componentes de COM con [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

C++ / WinRT puede ayudar a crear clásico modelo de objetos componentes (COM) componentes (o coclases), al igual que le ayuda a crear clases en tiempo de ejecución de Windows. Esta es una ilustración muy sencilla, que puede probar si se pega en la `main.cpp` de un nuevo **aplicación de consola de Windows (C++ / WinRT)** proyecto.

```cppwinrt
// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;

int main()
{
    init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist>
    {
        HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
}
```

Consulta también [componentes de consumir COM con C++ / WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un ejemplo más interesante y realista

El resto de este tema te guiará a través de crear un proyecto de aplicación de consola mínima que usa C++ / WinRT para implementar una fábrica de coclase y la clase básica. La aplicación de ejemplo muestra cómo enviar una notificación del sistema con un botón de devolución de llamada en él y la coclase (que implementa la interfaz **INotificationActivationCallback** COM) permite que la aplicación se inicia y se denomina atrás cuando el usuario Haga clic en ese botón en la notificación del sistema.

Obtener más información general sobre el área de característica de notificación del sistema puede encontrarse en [Enviar una notificación del sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Ninguno de los ejemplos de código en la sección de la documentación de usar C++ / WinRT, sin embargo, por lo tanto, te recomendamos que prefieres que el código se muestra en este tema.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Crear un proyecto de aplicación de consola de Windows (ToastAndCallback)

Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **Visual C++** > **Escritorio de Windows** > **aplicación de consola de Windows (C++ / WinRT)** del proyecto y el nombre *ToastAndCallback*.

Abre `main.cpp`y quitar las directivas using que genera la plantilla de proyecto. En su lugar, pega el siguiente código (que nos da el bibliotecas, los encabezados y los nombres de tipo que necesitamos).

```cppwinrt
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;
```

## <a name="implement-the-coclass-and-class-factory"></a>Implementar la fábrica coclase y clases

En C++ / WinRT, implementan coclases y fábricas de clase, derivando de la estructura base [**Implements**](/uwp/cpp-ref-for-winrt/implements) . Inmediatamente después de las tres directivas using mostradas anteriormente (y antes de `main`), pega este código para implementar el componente de activador de notificaciones COM de notificación del sistema.

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

La implementación de la coclase anterior sigue el mismo patrón que se muestra en [crear API con C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Ten en cuenta que puedes usar esta técnica no solo para las interfaces de Windows Runtime (es decir, cualquier interfaz que finalmente se deriva del objeto [**IInspectable**](https://msdn.microsoft.com/library/br205821)), sino también para implementar interfaces COM (cualquier interfaz que finalmente se deriva del objeto [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)).

En la coclase en el código anterior, se implementa el método **INotificationActivationCallback::Activate** , que es la función que se llama cuando el usuario hace clic en el botón de devolución de llamada en una notificación del sistema. Pero antes de llamar a esa función, debe crearse una instancia de la coclase y es el trabajo de la función **IClassFactory:: CreateInstance** .

La coclase que hemos implementado solo se conoce como el *activador COM* para las notificaciones y tiene su Id. de clase (CLSID) en forma de la `callback_guid` identificador (de tipo **GUID**) que ves anteriormente. Usaremos ese identificador de una versión posterior, en forma de un acceso directo del menú Inicio y una entrada del registro de Windows. El CLSID del activador COM y la ruta de acceso a su servidor COM asociado (que es la ruta de acceso al archivo ejecutable que estamos creando aquí) es el mecanismo por el que una notificación del sistema sabe qué clase para crear una instancia de cuando se hace clic en el botón de devolución de llamada (si el notificación se ha hecho clic en el centro de actividades o no).

## <a name="best-practices-for-implementing-com-methods"></a>Procedimientos recomendados para la implementación de métodos de COM

Técnicas de control de errores y de administración de recursos pueden ir en la mano. Es más cómodo y práctico usar excepciones de códigos de error. Y si se emplee la expresión de recurso adquisición-es-inicialización (RAII), puede evitar explícitamente comprobación de códigos de error y, a continuación, liberar recursos de forma explícita. Dichas comprobaciones explícitas hacer que el código más complicado que sea necesario, y ofrece errores una gran cantidad de lugares para ocultar. En su lugar, usa RAII y produzca/catch excepciones. De este modo, tus asignaciones de recursos son seguro para excepciones y el código es muy sencillo.

Sin embargo, no permitir excepciones para las implementaciones de método COM de escape. Puedes garantizar que mediante el uso de la `noexcept` especificador en los métodos de COM. Es Aceptar para que se produzcan en cualquier lugar en el gráfico de llamada de su método, excepciones, siempre y controlarlos antes de que el método se cierra. Si usas `noexcept`, pero, a continuación, permite que una excepción a su método de escape, a continuación, la aplicación finalizará.

## <a name="add-helper-types-and-functions"></a>Agregar funciones y tipos de ayuda

En este paso, vamos a agregar algunas funciones y tipos de ayuda que hace que el resto del código de usan de. Por lo que, antes de `main`, agrega lo siguiente.

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implementar las funciones restantes y la función de punto de entrada de wmain

La plantilla de proyecto genera un `main` función para TI. Eliminar que `main` funcionar y en su lugar, pega este código de la descripción, que incluye código para registrar tu coclase, y, después, para entregar una notificación del sistema capaz de llamar a volver a la aplicación.

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (Administrator)." : L".") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>Cómo probar la aplicación de ejemplo

Compilar la aplicación y, a continuación, al menos una vez a ejecutarlo como administrador para hacer que el registro y otro programa de instalación, ejecución de código. Si ejecutas como administrador y luego presiona ' t ' para hacer que una notificación del sistema que se muestre. A continuación, hacer clic en el botón de **devolución de llamada ToastAndCallback** directamente desde la notificación del sistema que se iniciará POP hacia arriba, o desde el centro de actividades y la aplicación, la coclase crea una instancia y la **INotificationActivationCallback :: Activar** método ejecutado.

## <a name="important-apis"></a>API importantes
* [Interfaz IInspectable](https://msdn.microsoft.com/library/br205821)
* [Interfaz IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Plantilla de estructura winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Artículos relacionados
* [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Consumir componentes de COM con C++ / WinRT](consume-com.md)
* [Enviar una notificación de icono local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
