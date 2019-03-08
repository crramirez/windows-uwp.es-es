---
description: C++ / c++ / WinRT puede ayudarle a crear componentes COM clásicos, tal como le ayuda a crear las clases de Windows en tiempo de ejecución.
title: Crear componentes COM con C++ / WinRT
ms.date: 09/06/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, autor, COM, component
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e6b77f8be6c75070336ad48f0c6471fc0a824a4c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616570"
---
# <a name="author-com-components-with-cwinrt"></a>Crear componentes COM con C++ / WinRT

[C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) puede ayudarle a crear el clásico modelo de objetos componentes (COM) componentes (o coclases), tal como le ayuda a crear las clases de Windows en tiempo de ejecución. Aquí es una ilustración simple, que puede probar si pega el código en el `pch.h` y `main.cpp` de un nuevo **aplicación de consola de Windows (C++ / c++ / WinRT)** proyecto.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

Consulte también [componentes COM consumir con C++ / c++ / WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un ejemplo más interesante y realista

El resto de este tema le guía por la creación de un proyecto de aplicación de consola mínimo que usa C++ / c++ / WinRT para implementar una coclase básica (componente COM o clase COM) y el generador de clases. La aplicación de ejemplo muestra cómo entregar una notificación del sistema con un botón de devolución de llamada en él y la coclase (que implementa el **INotificationActivationCallback** interfaz COM) permite que la aplicación se inicia y se llama realice una copia cuando el usuario hace clic en ese botón en la notificación del sistema.

Encontrará más información sobre el área de características de notificación del sistema en [enviar una notificación local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Usar ninguno de los ejemplos de código en esa sección de la documentación de C / c++ / WinRT, sin embargo, por lo que se recomienda que prefiere el código mostrado en este tema.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Crear un proyecto de aplicación de consola de Windows (ToastAndCallback)

Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **Visual C++** > **Windows Desktop** > **aplicación de consola de Windows (C++ / c++ / WinRT)** del proyecto y asígnele el nombre  *ToastAndCallback*.

Abra `pch.h`y agregue `#include <unknwn.h>` antes el incluye para C++ / c++ / WinRT encabezados.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Abra `main.cpp`y quitar las directivas using que genera la plantilla de proyecto. En su lugar, pegue el código siguiente (que nos proporciona las bibliotecas, encabezados y los nombres de tipo que es necesario).

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

## <a name="implement-the-coclass-and-class-factory"></a>Implementar la fábrica coclase y clase

En C / c++ / WinRT, implementa coclases y generadores de clases, derivando de la [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) struct base. Inmediatamente después de las tres directivas using mostradas anteriormente (y antes de `main`), pegue este código para implementar el componente de activador de COM de notificación del sistema.

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

La implementación de la coclase anterior sigue el mismo patrón que se muestra en [API autor con C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Por lo tanto, puede usar la misma técnica para implementar interfaces COM, así como interfaces de Windows en tiempo de ejecución. Los componentes COM y las clases en tiempo de ejecución de Windows exponen sus características a través de interfaces. Todas las interfaces de COM se deriva en última instancia el [ **interfaz IUnknown** ](https://msdn.microsoft.com/library/windows/desktop/ms680509) interfaz. El tiempo de ejecución de Windows se basa en COM&mdash;una diferencia está en última instancia, interfaces de Windows en tiempo de ejecución se derivan de la [ **interfaz IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (y **IInspectable**  deriva **IUnknown**).

En la coclase en el código anterior, implementamos el **INotificationActivationCallback::Activate** método, que es la función que se llama cuando el usuario hace clic en el botón de devolución de llamada en una notificación del sistema. Pero antes de llamar a esa función, debe crearse una instancia de la coclase, y ese es el trabajo de la **IClassFactory:: CreateInstance** función.

La coclase que acaba de implementar se conoce como el *activador COM* para las notificaciones y tiene su Id. de clase (CLSID) en forma de la `callback_guid` identificador (de tipo **GUID**) que ves anteriormente. Vamos a usar ese identificador más adelante, en forma de un acceso directo del menú Inicio y una entrada del registro de Windows. El CLSID del activador de COM y la ruta de acceso a su servidor COM asociado (que es la ruta de acceso al archivo ejecutable que estamos creando aquí) es el mecanismo por el que una notificación del sistema sabe qué clase para crear una instancia de cuando se hace clic en su botón en la devolución de llamada (si el notificación se hace clic en el centro de actividades o no).

## <a name="best-practices-for-implementing-com-methods"></a>Procedimientos recomendados para implementar los métodos COM

Técnicas para controlar errores y para la administración de recursos pueden ir en la mano. Resulta más cómodo y práctico usar excepciones a códigos de error. Y si emplea la expresión de recurso-acquisition-is-initialization (RAII), puede evitar explícitamente la comprobación de códigos de error y, a continuación, liberar explícitamente los recursos. Estas comprobaciones explícitas que el código más complicado que sea necesario y da errores una gran cantidad de lugares para ocultar. En su lugar, use RAII y throw y catch excepciones. De este modo, sus asignaciones de recursos son seguras para excepciones y el código es sencillo.

Sin embargo, no debe permitir excepciones para las implementaciones de método de COM de escape. Puede garantizar que mediante el uso del `noexcept` especificador en los métodos COM. Es correcto para que se produzcan en cualquier lugar en el gráfico de llamadas del método, excepciones como controlarlos antes de que salga el método. Si usa `noexcept`, pero, a continuación, permite que una excepción anular el método, a continuación, finalizará la aplicación.

## <a name="add-helper-types-and-functions"></a>Agregar funciones y tipos auxiliares

En este paso, vamos a agregar algunos tipos de aplicación auxiliar y las funciones que realiza el resto del código de usan de. Por tanto, antes `main`, agregue lo siguiente.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implemente las funciones restantes y la función de punto de entrada de wmain

La plantilla de proyecto genera un `main` función para usted. Eliminar eso `main` función y en su lugar, pegue este código de lista, que incluye código para registrar la coclase y, a continuación, para entregar una notificación del sistema capaz de devolver la llamada la aplicación.

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
    winrt::init_apartment();

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

Compilar la aplicación y, a continuación, ejecútelo de al menos una vez como administrador para hacer que el registro y otro programa de instalación, la ejecución de código. Si se está ejecutando como administrador y luego presione ' t ' para hacer que una notificación del sistema que se mostrará. Puede, a continuación, haga clic en el **ToastAndCallback de devolución de llamada** botón directamente desde la notificación del sistema que se abre, o desde el centro de actividades y la aplicación se iniciará la coclase crea una instancia y el  **INotificationActivationCallback::Activate** método ejecutado.

## <a name="in-process-com-server"></a>Servidor COM en proceso

El *ToastAndCallback* aplicación de ejemplo anterior funciona como un servidor COM local (o fuera de proceso). Esto se indica mediante el [LocalServer32](/windows/desktop/com/localserver32) clave del registro de Windows que se usa para registrar el CLSID de la coclase. Un servidor COM local hospeda su coclass(es) dentro de un ejecutable binario (un `.exe`).

Como alternativa (y posiblemente más probable), puede elegir hospedar su coclass(es) dentro de una biblioteca de vínculos dinámicos (un `.dll`). Un servidor COM en forma de un archivo DLL se conoce como un servidor COM en proceso y se indica por la que se va a registrar con el CLSID del [InprocServer32](/windows/desktop/com/inprocserver32) clave del registro de Windows.

### <a name="create-a-dynamic-link-library-dll-project"></a>Crear un proyecto de biblioteca de vínculos dinámicos (DLL)

Puede iniciar la tarea de creación de un servidor COM en proceso mediante la creación de un nuevo proyecto en Microsoft Visual Studio. Crear un **Visual C++** > **Windows Desktop** > **biblioteca de vínculos dinámicos (DLL)** proyecto.

Para agregar C++ / c++ / WinRT soporte para el nuevo proyecto, siga los pasos descritos en [modificar un proyecto de aplicación de escritorio de Windows para agregar C++ / c++ / WinRT soporte](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implementar la coclase, generador de clases y las exportaciones de servidor en proceso

Abra `dllmain.cpp`y agregarle el listado de código se muestra a continuación.

Si ya tiene un archivo DLL que implementa C++ / c++ / clases en tiempo de ejecución de Windows de WinRT, ya tendrá el **DllCanUnloadNow** función que se muestra a continuación. Si desea agregar las coclases a ese archivo DLL, a continuación, puede agregar el **DllGetClassObject** función.

Si no dispone [biblioteca de plantillas de C++ (WRL) de Windows en tiempo de ejecución](/cpp/windows/windows-runtime-cpp-template-library-wrl) partes de código que desea que siga siendo compatible con, a continuación, puede quitar la WRL desde el código que se muestra.

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            *ppvObject = winrt::make<MyCoclass>().get();
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

### <a name="support-for-weak-references"></a>Compatibilidad con referencias débiles

Consulte también [débil hace referencia en C / c++ / WinRT](weak-references.md#weak-references-in-cwinrt).

C++ / c++ / WinRT (en concreto, el [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) plantilla struct base) implementa [ **IWeakReferenceSource** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) para usted si su Escriba implementa [ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (o cualquier interfaz que se deriva de **IInspectable**).

Esto es porque **IWeakReferenceSource** y [ **IWeakReference** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) están diseñados para los tipos en tiempo de ejecución de Windows. Por lo tanto, puede activar el soporte de referencia débil para la coclase simplemente agregando **winrt::Windows::Foundation::IInspectable** (o una interfaz que se deriva de **IInspectable**) a la implementación.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>API importantes
* [Interfaz IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interfaz IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [winrt::implements struct template](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Temas relacionados
* [Crear las API con C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Consumir componentes COM con C++ / c++ / WinRT](consume-com.md)
* [Enviar una notificación del sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
