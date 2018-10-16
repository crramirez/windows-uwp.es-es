---
author: stevewhims
description: C++ / WinRT puede ayudar a crear componentes COM clásicos, al igual que le ayuda a crear clases en tiempo de ejecución de Windows.
title: Crear componentes COM con C++ / WinRT
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, autor, COM, componente
ms.localizationpriority: medium
ms.openlocfilehash: 94f59833f4c657445b7135b1158974d8a553813f
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4618092"
---
# <a name="author-com-components-with-cwinrt"></a>Crear componentes COM con C++ / WinRT

[C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) puede ayudar a crear clásico modelo de objetos componentes (COM) componentes (o coclases), al igual que le ayuda a crear clases en tiempo de ejecución de Windows. Esta es una ilustración simple, que puede probar si se pega el código en el `pch.h` y `main.cpp` de un nuevo **aplicación de consola de Windows (C++ / WinRT)** proyecto.

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

Consulta también [componentes de consumir COM con C++ / WinRT](consume-com.md).

## <a name="a-more-realistic-and-interesting-example"></a>Un ejemplo más interesante y realista

El resto de este tema te guiará a través de crear un proyecto de aplicación de consola mínima que usa C++ / WinRT para implementar una coclase básica (componente COM o clase de COM) y el generador de clases. La aplicación de ejemplo muestra cómo entregar una notificación del sistema con un botón de devolución de llamada en él, y la coclase (que implementa la interfaz **INotificationActivationCallback** COM) permite que la aplicación se inicia y se denomina atrás cuando el usuario Haga clic en ese botón en la notificación del sistema.

Obtener más información sobre el área de característica de notificación del sistema puede encontrarse en [Enviar una notificación del sistema local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast). Ninguno de los ejemplos de código en la sección de la documentación de usar C++ / WinRT, sin embargo, por lo tanto, te recomendamos que prefieres que el código que se muestra en este tema.

## <a name="create-a-windows-console-application-project-toastandcallback"></a>Crear un proyecto de aplicación de consola de Windows (ToastAndCallback)

Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **Visual C++** > **Escritorio de Windows** > **aplicación de consola de Windows (C++ / WinRT)** del proyecto y el nombre *ToastAndCallback*.

Abre `pch.h`y agrega `#include <unknwn.h>` antes de la incluye cualquier c++ / WinRT encabezados.

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

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

En C++ / WinRT, implementar coclases y fábricas de clase, derivando de la estructura base [**Implements**](/uwp/cpp-ref-for-winrt/implements) . Inmediatamente después de las tres directivas using anteriores (y antes de `main`), pega este código para implementar el componente de activador de notificaciones COM de notificación del sistema.

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

La implementación de la coclase anterior sigue el mismo patrón que se muestra en [crear API con C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class). Por lo tanto, puedes usar la misma técnica para implementar interfaces COM, así como interfaces de Windows Runtime. Los componentes COM y clases de Windows Runtime exponen sus características a través de interfaces. En última instancia, todas las interfaces de COM se deriva de la interfaz de la [**interfaz IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) . El tiempo de ejecución de Windows se basa en COM&mdash;una distinción que se va a interfaces de Windows Runtime en última instancia se derivan de la [**interfaz IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (y **IInspectable** se deriva del objeto **IUnknown**).

En la coclase en el código anterior, se implementa el método **INotificationActivationCallback::Activate** , que es la función que se llama cuando el usuario hace clic en el botón de devolución de llamada en una notificación del sistema. Sin embargo, para poder llamar a esa función, debe crearse una instancia de la coclase y el trabajo de la función **IClassFactory:: CreateInstance** .

La coclase que hemos implementado solo se conoce como el *activador COM* para las notificaciones y tiene su identificador de clase (CLSID) en forma de la `callback_guid` identificador (de tipo **GUID**) que ves anteriormente. Usaremos ese identificador de una versión posterior, en forma de un acceso directo del menú Inicio y una entrada del registro de Windows. El CLSID del activador COM y la ruta de acceso a su servidor COM asociado (que es la ruta de acceso al archivo ejecutable que estamos creando aquí) es el mecanismo por el que una notificación del sistema sabe qué clase para crear una instancia de cuando se hace clic en el botón de devolución de llamada (si el notificación se hace clic en el centro de actividades o no).

## <a name="best-practices-for-implementing-com-methods"></a>Procedimientos recomendados para la implementación de métodos de COM

Técnicas de control de errores y de administración de recursos pueden ir en la mano. Es más cómodo y práctico usar excepciones de códigos de error. Y si se emplee la expresión de recurso adquisición-es-inicialización (RAII), puede evitar explícitamente comprobación de códigos de error y, a continuación, liberar recursos. Dichas comprobaciones explícitas hacer que el código más complicado que sea necesario, y ofrece errores multitud de sitios para ocultar. En su lugar, usa RAII y produzca/catch excepciones. De este modo, las asignaciones de recursos son seguro para excepciones y el código es muy sencillo.

Sin embargo, no permitir excepciones para las implementaciones de método COM de escape. Puedes garantizar que mediante el uso de la `noexcept` especificador en los métodos de COM. Es Aceptar para que se produzcan en cualquier lugar en el gráfico de llamada de su método, excepciones como controlarlos antes de que el método se cierra. Si usas `noexcept`, pero, a continuación, permite que una excepción a su método de escape, a continuación, la aplicación finalizará.

## <a name="add-helper-types-and-functions"></a>Agregar funciones y tipos de ayuda

En este paso, vamos a agregar algunas funciones y tipos de ayuda que hace que el resto del código de usan. Por lo que, antes de `main`, agrega lo siguiente.

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

La plantilla de proyecto genera un `main` función para TI. Eliminar que `main` funcionar y en su lugar, pega este código de la descripción, que incluye código para registrar la coclase, y, después, para ofrecer una notificación del sistema capaz de llamar a volver a la aplicación.

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

Compilar la aplicación y, a continuación, al menos una vez a ejecutarlo como administrador para hacer que el registro y otro programa de instalación, la ejecución de código. Si ejecutas como administrador y luego presiona ' t ' para hacer que una notificación del sistema que se muestre. A continuación, hacer clic en el botón de **la llamada de ToastAndCallback** directamente desde la notificación del sistema que se iniciará POP hacia arriba, o desde el centro de actividades y la aplicación, la coclase crea una instancia y la **INotificationActivationCallback :: Activar** método ejecutado.

## <a name="in-process-com-server"></a>Servidor COM en proceso

La aplicación de ejemplo de *ToastAndCallback* anterior funciona como un servidor COM local (o fuera de proceso). Esto se indica en la clave del registro de [LocalServer32](/windows/desktop/com/localserver32) Windows de manera que usas para registrar el CLSID de su coclase. Un servidor COM local hospeda su coclass(es) dentro de un binario ejecutable (un `.exe`).

Como alternativa (y posiblemente más probable), puedes elegir hospedar su coclass(es) dentro de una biblioteca de vínculos dinámicos (un `.dll`). Un servidor COM en forma de un archivo DLL se conoce como un servidor COM en proceso, y se indica mediante CLSID se registre mediante el uso de la clave del registro de Windows de [InprocServer32](/windows/desktop/com/inprocserver32) .

### <a name="create-a-dynamic-link-library-dll-project"></a>Crear un proyecto de biblioteca de vínculos dinámicos (DLL)

Puede comenzar la tarea de creación de un servidor COM en proceso creando un nuevo proyecto en Microsoft Visual Studio. Crear un **Visual C++** > **Escritorio de Windows** > proyecto de**Biblioteca de vínculos dinámicos (DLL)** .

### <a name="set-project-properties"></a>Establecer las propiedades de proyecto

Ve a **General**de la propiedad de proyecto \> **Versión del SDK de Windows**y selecciona **Todas las configuraciones** y **Todas las plataformas**. Establece la **Versión del SDK de Windows** en *10.0.17134.0 (Windows 10, versión 1803)*, o posterior.

Para agregar soporte de Visual Studio para C++ / WinRT a tu proyecto, edita tu `.vcxproj` de archivos, busca `<PropertyGroup Label="Globals">` y, dentro de ese grupo de propiedades, Establece la propiedad `<CppWinRTEnabled>true</CppWinRTEnabled>`.

Dado que C++ / WinRT usa características del estándar C ++ 17, Establece la propiedad de proyecto **C o C++** > **idioma** > **Estándar de lenguaje de C++** a *ISO C ++ 17 Standard (/ STD: c ++ 17)*.

### <a name="the-precompiled-header"></a>El encabezado precompilado

Cambiar el nombre de tu `stdafx.h` y `stdafx.cpp` a `pch.h` y `pch.cpp`, respectivamente. Establece la propiedad de proyecto **C o C++** > **Encabezados precompilados** > **El archivo de encabezado precompilado** en *pch.h*.

Buscar y todas las reemplazar `#include "stdafx.h"` con `#include "pch.h"`.

En `pch.h`, incluyen `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

Confirma que no se ven afectadas por [¿por qué no compila mi proyecto nuevo?](/windows/uwp/cpp-and-winrt-apis/faq).

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>Implementar la coclase, el generador de clases y las exportaciones de servidor dentro del proceso

Abre `dllmain.cpp`y a agregarle el listado de código se muestra a continuación.

Si ya tienes un archivo DLL que implementa C++ / WinRT Windows Runtime clases, a continuación, verá que ya tiene la función **DllCanUnloadNow** se muestra a continuación. Si quieres agregar coclases a ese archivo DLL, a continuación, puedes agregar la función **DllGetClassObject** .

Si no tienes el código existente de [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) que desea que siga siendo compatible con, a continuación, puedes quitar las partes WRL desde el código de muestra.

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

Consulta también [referencias débiles en C++ / WinRT](weak-references.md#weak-references-in-cwinrt).

C++ / WinRT (en concreto, la plantilla de estructura base [**Implements**](/uwp/cpp-ref-for-winrt/implements) ) implementa [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) automáticamente si el tipo implementa [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (o cualquier interfaz que se deriva de **IInspectable**).

Esto es porque **IWeakReferenceSource** y [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) están diseñados para tipos de Windows Runtime. Por lo tanto, puedes activar el soporte de referencia débil para la coclase simplemente agregando el **winrt::Windows::Foundation::IInspectable** (o una interfaz que se deriva de **IInspectable**) a la implementación.

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>API importantes
* [Interfaz IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Interfaz IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Plantilla de estructura winrt::implements](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>Artículos relacionados
* [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Consumir componentes COM con C++ / WinRT](consume-com.md)
* [Enviar una notificación de icono local](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
