---
author: stevewhims
description: En este tema se muestra cómo migrar código C++/CX a su equivalente en C++/WinRT.
title: Migrar a C++/WinRT desde C++/CX
ms.author: stwhi
ms.date: 07/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 4aba8f559b7b6f0518a620d5127692d541953255
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "2892406"
---
# <a name="move-to-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-from-ccx"></a>Migrar a [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) desde C++/CX
En este tema se muestra cómo migrar código [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a su equivalente en C++/WinRT.

> [!IMPORTANT]
> Si desea trasladar gradualmente su [C + + / CX](/cpp/cppcx/visual-c-language-reference-c-cx) código a C + + / WinRT, a continuación, se puede. C + + / CX y C + + / WinRT código puede coexistir en el mismo proyecto, con la excepción de compatibilidad de compilador XAML y los componentes de tiempo de ejecución de Windows. Para esas excepciones, debe dirigir puede ser C + + / CX o C + + / WinRT dentro del mismo proyecto. Pero puede usar un componente de tiempo de ejecución de Windows para factorizar el código fuera de la aplicación XAML como puertos de él. Mover cualquiera tanta C + + / CX de código tal y como puede en un componente y, a continuación, cambie el proyecto XAML a C + + / WinRT. O else deje el proyecto XAML como C + + / CX, cree un nuevo C + + / WinRT componente y empezar a trasladar C + + / código CX fuera del proyecto XAML y en el componente. También podría tener un C + + / proyecto de componente de CX junto con un C + + / proyecto de componente de WinRT dentro de la misma solución, ambas de ellas referencia desde el proyecto de aplicación y el puerto gradualmente de uno a otro.

> [!NOTE]
> Tanto [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) como el Windows SDK declaran tipos en el espacio de nombres raíz **Windows**. Un tipo de Windows proyectado en C++/WinRT tiene el mismo nombre totalmente cualificado que el tipo de Windows, pero se coloca en el espacio de nombres **winrt** de C++. Estos espacios de nombres diferentes te permiten migrar de C++/CX a C++/WinRT a tu propio ritmo.

Teniendo en cuenta las excepciones que se mencionó anteriormente, el primer paso en trasladar un proyecto a C + + / WinRT es agregar manualmente C + + / WinRT soporte (vea [soporte técnico de Visual Studio para C + + / WinRT y la extensión VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). Para ello, edita tu archivo `.vcxproj`, encuentra `<PropertyGroup Label="Globals">` y, dentro de ese grupo de propiedades, establece la propiedad `<CppWinRTEnabled>true</CppWinRTEnabled>`. Un efecto de ese cambio es que el soporte para C++ / CX está desactivado en el proyecto. Es una buena idea dejar desactivado de forma que los mensajes de compilación le ayudará a buscar (y puerto) de compatibilidad con todas sus dependencias en C + + / CX, o se puede volver a activar soporte técnico (en Propiedades del proyecto, **C o C++** \> **General** \> **Consume tiempo de ejecución de Windows Extensión de** \> **Sí (/ZW)**) y el puerto gradualmente.

Establece la propiedad de proyecto **General** \> **Versión de la plataforma de destino** en 10.0.17134.0 (Windows 10, versión 1803) o superior.

En el archivo de encabezado precompilado (normalmente `pch.h`), incluye `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si incluyes cualquier encabezado de API de Windows proyectado de C++/ WinRT (por ejemplo, `winrt/Windows.Foundation.h`), no necesitas incluir explícitamente `winrt/base.h` así porque se incluirá automáticamente para ti.

Si el proyecto también está usando tipos de la [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl), consulta [Mover a C++/WinRT desde WRL](move-to-winrt-from-wrl.md).

## <a name="parameter-passing"></a>Paso de parámetros
Al escribir código fuente de C++/CX, pasas tipos C++/CX como parámetros de función como referencias de circunflejo(\^).

```cpp
void LogPresenceRecord(PresenceRecord^ record);
```

En C++/WinRT, para las funciones sincrónicas, debes usar los parámetros `const&` de manera predeterminada. Eso evitará copias y sobrecarga entrelazada. Pero tus corrutinas deben usar paso-por-valor para garantizar que capturan por valor y evitar los problemas de ciclo de vida (para obtener más información, consulta [Operaciones simultáneas y asincrónicas con C++ / WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Un objeto de C++/WinRT es esencialmente un valor que contiene un puntero de interfaz para el objeto de Windows Runtime de copia de seguridad. Cuando copias un objeto de C++/WinRT, el compilador copia el puntero de interfaz encapsulado, incrementando su recuento de referencia. La destrucción final de la copia conlleva la disminución del recuento de referencia. Por lo tanto, incurra solo la sobrecarga de una copia cuando sea necesario.

## <a name="variable-and-field-references"></a>Referencias de variables y campo
Al escribir código fuente de C++/CX, usas variables de circunflejo (\^) para hacer referencia a objetos de Windows Runtime y el operador de flecha (-&gt;) para desreferenciar una variable de circunflejo.

```cpp
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Al convertir al código de C++/ WinRT equivalente, básicamente quitas los circunflejos y cambia el operador de flecha (-&gt;) al operador de punto (.), porque los tipos C++/ WinRT proyectados son valores y no punteros.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

## <a name="properties"></a>Propiedades
Las extensiones de lenguaje C++/CX incluyen el concepto de propiedades. Al escribir código fuente de C++/CX, puedes obtener acceso a una propiedad como si es un campo. C++ estándar no tiene el concepto de un propiedad por lo que, en C++/ WinRT, puedes llamar a las funciones get y set.

En los ejemplos siguientes, los valores **XboxUserId**, **UserState**, **PresenceDeviceRecords** y **Size** son todas propiedades.

### <a name="retrieving-a-value-from-a-property"></a>Recuperar un valor de una propiedad
Esta es la manera de obtener un valor de propiedad en C++/CX.

```cpp
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

El código fuente de C++/WinRT equivalente llama a una función con el mismo nombre que la propiedad, pero sin parámetros.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Ten en cuenta que la función **PresenceDeviceRecords** devuelve un objeto de Windows Runtime que tiene una función **Tamaño**. Como el objeto devuelto también es un tipo proyectado de C++/WinRT, desreferenciamos mediante el operador de punto para llamar a **Tamaño**.

### <a name="setting-a-property-to-a-new-value"></a>Establecer una propiedad en un valor nuevo
El establecimiento de una propiedad en un valor nuevo sigue un patrón similar. En primer lugar, en C++/CX.

```cpp
record->UserState = newValue;
```

Para hacer lo equivalente en C++/ WinRT, llama a una función con el mismo nombre que la propiedad y pasa un argumento.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Crear una instancia de una clase
Trabajas con un objeto de C++/ CX mediante un manipulador para él, normalmente conocido como una referencia de acento circunflejo (\^). Crea un objeto nuevo mediante la palabra clave `ref new`, que a su vez llama a [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) para activar una instancia nueva de la clase en tiempo de ejecución.

```cpp
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Un objeto de C++/WinRT es un valor, por lo que puede asignarlo en la pila, o como un campo de un objeto. No uses *nunca* `ref new` (ni `new`) para asignar un objeto de C++/WinRT. En segundo plano, se sigue llamando a **RoActivateInstance**.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Si un recurso es caro de inicializar, es habitual retrasar su inicialización hasta que no sea realmente necesario.

```cpp
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

El mismo código migrado a C++/WinRT. Ten en cuenta el uso del constructor `nullptr`. Para obtener más información acerca del constructor, consulta [Consumir API con C++/WinRT](consume-apis.md).

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="event-handling-with-a-delegate"></a>Control de eventos con un delegado
Este es un ejemplo típico del control de un evento en C++/CX, usando una función lambda como delegado en este caso.

```cpp
auto token = myButton->Click += ref new RoutedEventHandler([&](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
});
```

Este es el equivalente en C++/WinRT.

```cppwinrt
auto token = myButton().Click([&](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
});
```

En lugar de una función lambda, puedes elegir implementar tu delegado como una función libre o como un puntero a una función miembro. Para obtener más información, consulta [Controlar eventos usando delegados en C ++/WinRT](handle-events.md).

Si vas a portar desde una base de código de C++/CX donde se usan internamente eventos y delegados (no a través de archivos binarios), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) te ayudará a replicar dicho patrón en C++/WinRT. Vea también [parametrizada delegados, señales simples y las devoluciones de llamada dentro de un proyecto](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Revocación de un delegado
En C++/CX, usas el operador `-=` para revocar un registro de eventos anterior.

```cpp
myButton->Click -= token;
```

Este es el equivalente en C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Para obtener más información y opciones, consulta [Revocar un delegado registrado](handle-events.md#revoke-a-registered-delegate).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Asignación de tipos de **Plataforma** de C++/CX a C++/WinRT
C++/CX proporciona varios tipos de datos en el espacio de nombres de **Plataforma**. Estos tipos no son C++ estándar, por lo que solo puedes usarlos al habilitar extensiones de lenguaje de Windows Runtime (propiedad de proyecto de Visual Studio **C o C++** > **General** > **Usar extensión de Windows Runtime** > **Sí (/ZW)**). La tabla siguiente te ayuda a migrar desde tipos de **Plataforma** a sus equivalentes en C++/WinRT. Una vez que lo hayas hecho, puesto que C++/WinRT es C++ estándar, puedes desactivar la opción `/ZW`.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Plataforma:: Agile\ ^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Puerto **plataforma:: Agile\ ^** a **winrt::agile_ref**
El **plataforma:: Agile\ ^** tipo en C + + / CX representa una clase de tiempo de ejecución de Windows que se puede tener acceso desde cualquier subproceso. C + + / equivalente de WinRT es [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

En C++/CX.

```cpp
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

En C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformexception-to-winrthresulterror"></a>Migrar **Platform:: Exception\^** a **winrt::hresult_error**
El tipo **Platform::Exception\^** se produce en C++/CX cuando una API de Windows Runtime devuelve un HRESULT que no es S\_OK. El equivalente de C++/WinRT es [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Para migrar a C++/WinRT, cambia todo el código que usa **Platform::Exception\^** para que use **winrt::hresult_error**.

En C++/CX.

```cpp
catch (Platform::Exception^ ex)
```

En C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT proporciona estas clases de excepción.

| Tipo de excepción | Clase base | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | llamar a [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

Ten en cuenta que cada clase (a través de la clase base **hresult_error**) proporciona una función [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function), que devuelve el HRESULT del error, y una función [**mensaje**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function), que devuelve la representación de la cadena de ese HRESULT.

Este es un ejemplo de generación de una excepción en C++/CX.

```cpp
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Y el equivalente en C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Migrar **Platform::Object\^** a **winrt::Windows::Foundation::IInspectable**
Como todos los tipos de C++/WinRT, **winrt::Windows::Foundation::IInspectable** es un tipo de valor. Aquí te mostramos cómo inicializar una variable de ese tipo en null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Migrar **Platform::String\^** a **winrt::hstring**
**Platform::String\^** equivale a que el tipo de ABI HSTRING de Windows Runtime. Para C++/WinRT, el equivalente es [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Pero con C++/WinRT, puedes llamar a las API de Windows Runtime mediante tipos de cadenas de caracteres anchos de la biblioteca estándar de C++ como **std::wstring**, y/o literales de cadena de caracteres anchos. Para obtener más información y ejemplos de código, consulta [Control de cadenas en C++/WinRT](strings.md).

Con C++/CX, puedes obtener acceso a la propiedad [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) para recuperar la cadena como una matriz **const wchar_t\*** de estilo C (por ejemplo, para pasarla a **std::wcout**).

```C++
auto var = titleRecord->TitleName->Data();
```

Para hacer lo mismo con C++/WinRT, puedes usar la función [**hstring::c_str**](/uwp/api/windows.foundation.uri#hstringcstr-function) para obtener una versión de cadena de estilo C terminada en null, al igual que puede hacerlo desde **std:: wstring **.

```C++
auto var = titleRecord.TitleName().c_str();
```

En cuanto a la implementación de las API que toman o devuelven cadenas, por lo general cambias cualquier código C++/CX que usa **Platform::String\^** para que utilice **winrt::hstring** en su lugar.

Este es un ejemplo de una API de C++/ CX que toma una cadena.

```cpp
void LogWrapLine(Platform::String^ str);
```

Para C++/WinRT podría declarar esa API en [MIDL 3.0](/uwp/midl-3) como esto.

```idl
// LogType.idl
void LogWrapLine(String str);
```

La cadena de herramientas de C++/WinRT generará entonces código fuente para ti que tenga este aspecto.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

## <a name="important-apis"></a>API importantes
* [plantilla de estructura winrt::delegate](/uwp/cpp-ref-for-winrt/delegate)
* [Estructura winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Artículos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Crear eventos en C++/WinRT](author-events.md)
* [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md)
* [Consumir API con C++/WinRT](consume-apis.md)
* [Controlar eventos usando delegados en C++/WinRT](handle-events.md)
* [Referencia de Lenguaje de definición de interfaz de Microsoft 3.0](/uwp/midl-3)
* [Mover a C++/WinRT desde WRL](move-to-winrt-from-wrl.md)
* [Control de cadenas en C++/WinRT](strings.md)
