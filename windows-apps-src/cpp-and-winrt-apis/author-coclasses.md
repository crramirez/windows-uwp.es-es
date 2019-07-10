---
description: C++/WinRT puede ayudarte a crear componentes COM clásicos, igual que te ayuda a crear clases de Windows Runtime.
title: Crear componentes COM con C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, author, COM, component
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3badcd59155bc4bb5ef8d9e29271b853c245c24e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360322"
---
# <a name="author-com-components-with-cwinrt"></a>Crear componentes COM con C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) puede ayudarte a crear componentes del Modelo de objetos componentes (COM) clásicos, igual que te ayuda a crear clases de Windows Runtime. Esta es una ilustración simple, que puedes probar si pegas el código en `pch.h` y `main.cpp` de un nuevo proyecto de la **aplicación de consola Windows (C++/WinRT)** .

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

También, consulta [Consumir componentes COM con C++/WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un ejemplo más interesante y realista

El resto de este tema se centra en la creación de un proyecto mínimo de la aplicación de consola que utiliza C++/WinRT para implementar una coclase básica (componente COM o clase COM) y un generador de clases. La aplicación de ejemplo muestra cómo entregar una notificación del sistema con un botón de devolución de llamada y la coclase (que implementa la interfaz COM **INotificationActivationCallback**) permite que la aplicación se inicie y vuelva a llamarse cuando el usuario hace clic en ese botón en la notificación del sistema.

Encontrarás más información sobre el área de características de notificación del sistema en [Enviar una notificación del sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Sin embargo, ninguno de los ejemplos de código de esa sección de la documentación utiliza C++/WinRT, por lo que recomendamos que prefieras el código que se muestra en este tema.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Creación de un proyecto de la aplicación de consola Windows (ToastAndCallback)

Para empezar, crea un proyecto en Microsoft Visual Studio. Crea un proyecto de la **aplicación de consola Windows (C++/WinRT)** y llámalo *ToastAndCallback*.

Abre `pch.h` y agrega `#include <unknwn.h>` antes de cualquier encabezado de C++/WinRT. Este es el resultado; puedes reemplazar el contenido de tu `pch.h` por esta lista.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

Abre `main.cpp` y quita las directivas using que genera la plantilla de proyecto. En su lugar, inserta el código siguiente (que nos proporciona las bibliotecas, los encabezados y los nombres de tipo que necesitamos). Este es el resultado; puedes reemplazar el contenido de tu `main.cpp` por esta lista (también hemos quitado el código de `main` en la lista siguiente, ya que vamos a sustituir esa función más adelante).

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
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

int main() { }
```

Todavía no se compilará el proyecto; cuando hayamos terminado de agregar el código, se te pedirá que lo compiles y ejecutes.

## <a name="implement-the-coclass-and-class-factory"></a>Implementación de la coclase y el generador de clases

En C++/WinRT, vas a implementar coclases y fábricas de clases al derivar de la estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). Inmediatamente después de las tres directivas using mostradas más arriba (y antes de `main`), pega este código para implementar el componente del activador COM de la notificación del sistema.

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

La implementación de la coclase anterior sigue el mismo patrón que se muestra en [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Por lo tanto, puedes usar la misma técnica para implementar las interfaces COM, así como las interfaces de Windows Runtime. Los componentes COM y las clases de Windows Runtime exponen sus características mediante interfaces. Todas las interfaces COM derivan en última instancia de la [**interfaz IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown). Windows Runtime se basa en COM: una distinción es que las interfaces de Windows Runtime derivan en última instancia de la [**interfaz IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (e **IInspectable**  deriva de **IUnknown**).

En la coclase del código anterior, implementamos el método **INotificationActivationCallback::Activate**, que es la función que se llama cuando el usuario hace clic en el botón de devolución de llamada en una notificación del sistema. Pero antes de llamar a esa función, debe crearse una instancia de la coclase y ese es el trabajo de la función **IClassFactory:: CreateInstance**.

La coclase que acabas de implementar se conoce como el *activador COM* para las notificaciones y tiene su identificador de clase (CLSID) en forma del identificador `callback_guid` (de tipo **GUID**) que ves más arriba. Vamos a usar ese identificador más adelante, en forma de un acceso directo del menú Inicio y una entrada del Registro de Windows. El CLSID del activador COM y la ruta de acceso a su servidor COM asociado (que es la ruta de acceso al ejecutable que estamos creando aquí) es el mecanismo mediante el cual una notificación del sistema conoce la clase para crear una instancia cuando se hace clic en el botón de devolución de llamada (tanto si se hace clic en la notificación en el Centro de actividades como si no).

## <a name="best-practices-for-implementing-com-methods"></a>Procedimientos recomendados para implementar los métodos COM

Las técnicas para el control de errores y para la administración de recursos pueden ir de la mano. Resulta más cómodo y práctico utilizar excepciones que códigos de error. Y si empleas la expresión de resource-acquisition-is-initialization (RAII), puedes evitar explícitamente la comprobación de códigos de error y, después, liberar explícitamente los recursos. Estas comprobaciones explícitas hacen que el código sea más complicado de lo necesario y da a los errores una gran cantidad de lugares para ocultarse. Por el contrario, usa RAII y las excepciones de inicio y detección. De este modo, tus asignaciones de recursos estarán a prueba de excepciones y el código será sencillo.

Sin embargo, no debes permitir excepciones para escapar las implementaciones del método COM. Puedes garantizarlo mediante el especificador `noexcept` en los métodos COM. Es correcto que las excepciones se inicien en cualquier parte del gráfico de llamadas del método, siempre y cuando las controles antes de que el método salga. Si usas `noexcept`, pero permites que una excepción se escape al método, en este caso, finalizará la aplicación.

## <a name="add-helper-types-and-functions"></a>Adición de funciones y tipos auxiliares

En este paso, vamos a agregar algunos tipos y funciones auxiliares que el resto del código utiliza. Por tanto, inmediatamente antes de `main`, agregue lo siguiente.

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>Implementación de las funciones restantes y la función de punto de entrada de wmain

Elimina tu función `main` y, en su lugar, pega esta lista de código, que incluye el código para registrar la coclase, y después entrega una notificación del sistema capaz de devolver la llamada a la aplicación.

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
    ::Sleep(50); // Give the callback chance to display.
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
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

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

## <a name="how-to-test-the-example-application"></a>Prueba de la aplicación de ejemplo

Compila la aplicación y, después, ejecútala al menos una vez como administrador para que se ejecute el código de registro y otros códigos de configuración. Una manera de hacerlo es ejecutar Visual Studio como administrador y, después, ejecutar la aplicación desde Visual Studio. Haz clic con el botón derecho en Visual Studio en la barra de tareas para mostrar la lista de accesos directos, haz clic con el botón derecho en Visual Studio en la lista de accesos directos y, a continuación, haz clic en **Ejecutar como administrador**. Acepta el aviso y abre el proyecto. Al ejecutar la aplicación, se muestra un mensaje que indica si la aplicación se está ejecutando como administrador. Si no lo está, el registro y otras configuraciones no se ejecutarán. Este registro y otras configuraciones deben ejecutarse al menos una vez para que la aplicación funcione correctamente.

Con independencia de si estás ejecutando la aplicación como administrador, presiona 'T' para que se muestre una notificación del sistema. Después puedes hacer clic en el botón **Call back ToastAndCallback** (Devolver la llamada a ToastAndCallback) directamente desde la notificación del sistema que se abre, o desde el Centro de actividades y la aplicación se iniciará, la coclase creará una instancia y el método  **INotificationActivationCallback::Activate** se ejecutará.

## <a name="in-process-com-server"></a>Servidor COM en proceso

La aplicación de ejemplo *ToastAndCallback* anterior funciona como un servidor COM local (o fuera del proceso). Esto se indica mediante la clave del Registro de Windows [LocalServer32](/windows/desktop/com/localserver32) que se usa para registrar el CLSID de su coclase. Un servidor COM local hospeda sus coclases en un ejecutable binario (un `.exe`).

Como alternativa (y más probable), puede elegir hospedar las coclases dentro de una biblioteca de vínculos dinámicos (una `.dll`). Un servidor COM en forma de DLL se conoce como un servidor COM en proceso y se indica mediante CLSID que se registran mediante la clave del Registro de Windows [InprocServer32](/windows/desktop/com/inprocserver32).

### <a name="create-a-dynamic-link-library-dll-project"></a>Creación de un proyecto de biblioteca de vínculos dinámicos (DLL)

Para comenzar la tarea de crear un servidor COM en proceso, crea un nuevo proyecto en Microsoft Visual Studio. Crea un proyecto **Visual C++**  > **Escritorio de Windows** > **Biblioteca de vínculos dinámicos (DLL)** .

Para agregar compatibilidad con C++/WinRT para el nuevo proyecto, sigue los pasos descritos en [Modificación de un proyecto de aplicación de escritorio de Windows para agregar compatibilidad de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implementación de la coclase, el generador de clases y las exportaciones del servidor en proceso

Abre `dllmain.cpp` y agrégale la lista de código que se muestra a continuación.

Si ya tienes un archivo DLL que implementa clases de Windows Runtime de C++/WinRT, ya tienes la función **DllCanUnloadNow** que se muestra a continuación. Si quieres agregar las coclases a ese archivo DLL, puedes agregar la función **DllGetClassObject**.

Si no tienes el código existente de la [Biblioteca de plantillas C++ de Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) con el que quieres seguir siendo compatible, puedes eliminar las partes de WRL del código mostrado.

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

Consulta también [Referencias débiles de C++/WinRT](weak-references.md#weak-references-in-cwinrt).

C++/WinRT (específicamente, la plantilla de estructura base [**winrt::implementa**](/uwp/cpp-ref-for-winrt/implements)) implementa [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) para ti si tu tipo implementa [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (o cualquier interfaz que se derive de **IInspectable**).

Esto se debe a que **IWeakReferenceSource** y [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) están diseñados para tipos de Windows Runtime. Por lo tanto, puedes activar la compatibilidad de referencia débil para tu coclase simplemente agregando **winrt::Windows::Foundation::IInspectable** (o una interfaz que se derive de **IInspectable**) a tu implementación.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>API importantes
* [Interfaz IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interfaz IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [Plantilla de estructura winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Temas relacionados
* [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Consumir componentes COM con C++/WinRT](consume-com.md)
* [Enviar una notificación del sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
