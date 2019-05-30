---
description: En este tema se muestra cómo migrar código C++/CX a su equivalente en C++/WinRT.
title: Migrar a C++/WinRT desde C++/CX
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 7fbe10e41da1b330d6f5042bea109a8a0e04f8ad
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360162"
---
# <a name="move-to-cwinrt-from-ccx"></a>Migrar a C++/WinRT desde C++/CX

Este tema muestra cómo trasladar el código en un [C++ / c++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) proyecto a su equivalente de [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Estrategias de migración

Si desea trasladar gradualmente C++ / c++ / CX código C++ / c++ / WinRT, puede realizar. C++ / c++ / CX y c++ / WinRT código puede coexistir en el mismo proyecto, con las excepciones de compatibilidad con el compilador XAML y componentes de Windows en tiempo de ejecución. Para esas dos excepciones, debe tener como destino cualquier C++ / c++ / CX o c++ / WinRT dentro del mismo proyecto.

> [!IMPORTANT]
> Si el proyecto compila una aplicación XAML, a continuación, un flujo de trabajo que recomendamos es crear primero un nuevo proyecto en Visual Studio mediante uno de C++ / c++ / WinRT plantillas de proyecto (vea [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). A continuación, inicie copiar marcado y código fuente a través de C++ / c++ / proyecto CX. Puede agregar nuevas páginas XAML con **proyecto** \> **Agregar nuevo elemento...** \> **Visual C++**  > **página en blanco (C++ / c++ / WinRT)** .
>
> Como alternativa, puede usar un componente de Windows en tiempo de ejecución para factorizar el código fuera de XAML C++ / c++ / CX proyecto mientras lo traslada. Mueva tanto C++ / c++ / CX de código como puede en un componente y, a continuación, cambie el proyecto XAML en C++ / c++ / WinRT. O de lo contrario, deje el proyecto XAML como C++ / c++ / CX, cree un nuevo C++ / c++ / componentes de WinRT y empezar a trasladar C++ / c++ / código CX fuera del proyecto XAML y en el componente. También podría tener C++ / c++ / proyecto de componente CX junto con C++ / c++ / WinRT proyecto del componente dentro de la misma solución, ambos referencia desde el proyecto de aplicación y el puerto gradualmente de uno a otro. Consulte [interoperabilidad entre C++ / c++ / WinRT y C / c++ / CX](interop-winrt-cx.md) para obtener más detalles sobre el uso de las proyecciones de dos lenguaje en el mismo proyecto.

> [!NOTE]
> Tanto [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) como el Windows SDK declaran tipos en el espacio de nombres raíz **Windows**. Un tipo de Windows proyectado en C++/WinRT tiene el mismo nombre totalmente cualificado que el tipo de Windows, pero se coloca en el espacio de nombres **winrt** de C++. Estos espacios de nombres diferentes te permiten migrar de C++/CX a C++/WinRT a tu propio ritmo.

Teniendo en cuenta las excepciones que se mencionó anteriormente, el primer paso en la migración de C++ / c++ / CX proyecto C++ / c++ / WinRT es agregar manualmente C++ / c++ / WinRT soporte a él (vea [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para ello, instale el [paquete Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto. Haga clic en el proyecto en Visual Studio, abra **proyecto** \> **administrar paquetes NuGet...** \> **Examinar**, escriba o pegue **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, seleccione el elemento en los resultados de búsqueda y, a continuación, haga clic en **instalar** para instalar el paquete para ese proyecto. Un efecto de ese cambio es que el soporte para C++ / CX está desactivado en el proyecto. Es una buena idea dejar todas sus dependencias compatibilidad desactivada para que los mensajes de compilación le ayudarán a encontrar (y puerto) en C++ / c++ / CX, o bien puede volver a activar soporte técnico (en las propiedades del proyecto, **C o C++** \> **General** \> **Usar extensión de Windows en tiempo de ejecución** \> **Sí (/ZW)** ) y el puerto gradualmente.

Asegúrese de esa propiedad de proyecto **General** \> **versión de la plataforma de destino** está establecido en 10.0.17134.0 (Windows 10, versión 1803) o superior.

En el archivo de encabezado precompilado (normalmente `pch.h`), incluye `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si incluyes cualquier encabezado de API de Windows proyectado de C++/ WinRT (por ejemplo, `winrt/Windows.Foundation.h`), no necesitas incluir explícitamente `winrt/base.h` así porque se incluirá automáticamente para ti.

Si el proyecto también está usando tipos de la [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl), consulta [Mover a C++/WinRT desde WRL](move-to-winrt-from-wrl.md).

## <a name="parameter-passing"></a>Paso de parámetros
Cuando se escribe en C++ / c++ / CX código de origen, pasa C++ / c++ / tipos CX como parámetros de función como hat (\^) las referencias.

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

En C++/WinRT, para las funciones sincrónicas, debes usar los parámetros `const&` de manera predeterminada. Eso evitará copias y sobrecarga entrelazada. Pero tus corrutinas deben usar paso-por-valor para garantizar que capturan por valor y evitar los problemas de ciclo de vida (para obtener más información, consulta [Operaciones simultáneas y asincrónicas con C++ / WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Un objeto de C++/WinRT es esencialmente un valor que contiene un puntero de interfaz para el objeto de Windows Runtime de copia de seguridad. Cuando copias un objeto de C++/WinRT, el compilador copia el puntero de interfaz encapsulado, incrementando su recuento de referencia. La destrucción final de la copia conlleva la disminución del recuento de referencia. Por lo tanto, incurra solo la sobrecarga de una copia cuando sea necesario.

## <a name="variable-and-field-references"></a>Referencias de variables y campo
Cuando se escribe en C++ / c++ / código fuente CX, utilizar hat (\^) variables hagan referencia a objetos en tiempo de ejecución de Windows y la flecha (-&gt;) operador de desreferencia una variable de hat.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Al trasladar C++ equivalente / c++ / WinRT código, puede obtener un largo camino quitando los sombreros, y cambiando el operador de flecha (-&gt;) para el operador punto (.). C++ / c++ / WinRT proyectada tipos son los valores y no los punteros.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

El constructor predeterminado para un C++puntero de hat /CX lo inicializa en null. Aquí es C++ / c++ / CX ejemplo de código se en el que se cree una variable o campo del tipo correcto, pero que se no inicializada. En otras palabras, inicialmente no hace referencia a un **TextBlock**; se va a asignar una referencia más adelante.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Para el equivalente en C / c++ / WinRT, consulte [inicialización demorada](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Propiedades
Las extensiones de lenguaje C++/CX incluyen el concepto de propiedades. Al escribir código fuente de C++/CX, puedes obtener acceso a una propiedad como si es un campo. C++ estándar no tiene el concepto de un propiedad por lo que, en C++/ WinRT, puedes llamar a las funciones get y set.

En los ejemplos siguientes, los valores **XboxUserId**, **UserState**, **PresenceDeviceRecords** y **Size** son todas propiedades.

### <a name="retrieving-a-value-from-a-property"></a>Recuperar un valor de una propiedad
Esta es la manera de obtener un valor de propiedad en C++/CX.

```cppcx
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

```cppcx
record->UserState = newValue;
```

Para hacer lo equivalente en C++/ WinRT, llama a una función con el mismo nombre que la propiedad y pasa un argumento.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Crear una instancia de una clase
Trabaja con C / c++ / objeto CX a través de un controlador al mismo, normalmente se conoce como un "sombrero" (\^) referencia. Crea un objeto nuevo mediante la palabra clave `ref new`, que a su vez llama a [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) para activar una instancia nueva de la clase en tiempo de ejecución.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Un objeto de C++/WinRT es un valor, por lo que puede asignarlo en la pila, o como un campo de un objeto. No uses *nunca*`ref new` (ni `new`) para asignar un objeto de C++/WinRT. En segundo plano, se sigue llamando a **RoActivateInstance**.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Si un recurso es caro de inicializar, es habitual retrasar su inicialización hasta que no sea realmente necesario.

```cppcx
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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Conversión de una clase base en tiempo de ejecución a uno derivada
Es habitual tener una referencia a base que sabe que hace referencia a un objeto de un tipo derivado. En C++ / c++ / CX, usar `dynamic_cast` a *cast* la referencia de base en una referencia a derivado. El `dynamic_cast` es simplemente una llamada oculta a [ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Este es un ejemplo típico&mdash;se controla un evento de cambio de propiedad de dependencia, y desea realizar la conversión de **DependencyObject** al tipo real que posee la propiedad de dependencia.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

El equivalente C++/WinRT código reemplaza el `dynamic_cast` con una llamada a la [ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) función, que encapsula **QueryInterface**. También tiene la opción de llamar a [ **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function), en su lugar, lo que produce una excepción si no se devuelve la consulta de la interfaz necesaria (la interfaz predeterminada del tipo que está solicitando). Aquí es C++ / c++ / WinRT ejemplo de código.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>Control de eventos con un delegado
Este es un ejemplo típico del control de un evento en C++/CX, usando una función lambda como delegado en este caso.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Este es el equivalente en C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

En lugar de una función lambda, puedes elegir implementar tu delegado como una función libre o como un puntero a una función miembro. Para obtener más información, consulta [Controlar eventos usando delegados en C ++/WinRT](handle-events.md).

Si vas a portar desde una base de código de C++/CX donde se usan internamente eventos y delegados (no a través de archivos binarios), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) te ayudará a replicar dicho patrón en C++/WinRT. Consulte también [parámetros delegados, señales simple y las devoluciones de llamada dentro de un proyecto](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Revocación de un delegado
En C++/CX, usas el operador `-=` para revocar un registro de eventos anterior.

```cppcx
myButton->Click -= token;
```

Este es el equivalente en C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Para obtener más información y opciones, consulta [Revocar un delegado registrado](handle-events.md#revoke-a-registered-delegate).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Asignación de tipos de **Plataforma** de C++/CX a C++/WinRT
C++/CX proporciona varios tipos de datos en el espacio de nombres de **Plataforma**. Estos tipos no son C++ estándar, por lo que solo puedes usarlos al habilitar extensiones de lenguaje de Windows Runtime (propiedad de proyecto de Visual Studio **C o C++**  > **General** > **Usar extensión de Windows Runtime** > **Sí (/ZW)** ). La tabla siguiente te ayuda a migrar desde tipos de **Plataforma** a sus equivalentes en C++/WinRT. Una vez que lo hayas hecho, puesto que C++/WinRT es C++ estándar, puedes desactivar la opción `/ZW`.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform:: Array\^** | Consulte [puerto **Platform:: Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Puerto **Platform:: Agile\^**  a **winrt::agile_ref**
El **Platform:: Agile\^**  tipo en C / c++ / CX representa una clase en tiempo de ejecución de Windows que se puede acceder desde cualquier subproceso. El C++/es el equivalente de WinRT [ **winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

En C++/CX.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

En C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Puerto **Platform:: Array\^**
Las opciones incluyen el uso de una lista de inicializadores, un **std:: Array**, o un **std:: vector**. Para obtener más información y ejemplos de código, vea [listas de inicializadores estándar](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) y [estándares matrices y vectores](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresulterror"></a>Puerto **Platform:: Exception\^**  a **winrt::hresult_error**
El **Platform:: Exception\^**  tipo se genera en C++ / c++ / CX cuando una API de Windows en tiempo de ejecución devuelve un que no son S\_Aceptar HRESULT. El equivalente de C++/WinRT es [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

En el puerto a C++/WinRT, cambiar todo el código que usa **Platform:: Exception\^**  usar **winrt::hresult_error**.

En C++/CX.

```cppcx
catch (Platform::Exception^ ex)
```

En C++/WinRT.

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT proporciona estas clases de excepción.

| Tipo de excepción | Clase base | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | llamar a [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
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

Ten en cuenta que cada clase (a través de la clase base **hresult_error**) proporciona una función [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function), que devuelve el HRESULT del error, y una función [**mensaje**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function), que devuelve la representación de la cadena de ese HRESULT.

Este es un ejemplo de generación de una excepción en C++/CX.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Y el equivalente en C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Puerto **Platform:: Object\^**  a **winrt::Windows::Foundation::IInspectable**
Como todos los tipos de C++/WinRT, **winrt::Windows::Foundation::IInspectable** es un tipo de valor. Aquí te mostramos cómo inicializar una variable de ese tipo en null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Puerto **Platform:: String\^**  a **winrt::hstring**
**Platform:: String\^**  es equivalente al tipo HSTRING ABI de Windows en tiempo de ejecución. Para C++/WinRT, el equivalente es [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Pero con C++/WinRT, puedes llamar a las API de Windows Runtime mediante tipos de cadenas de caracteres anchos de la biblioteca estándar de C++ como **std::wstring**, y/o literales de cadena de caracteres anchos. Para obtener más información y ejemplos de código, consulta [Control de cadenas en C++/WinRT](strings.md).

Con C++/CX, puede tener acceso a la [ **Platform::String::Data** ](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) propiedad para recuperar la cadena como un estilo de C **const wchar_t\***  (por ejemplo, de matriz para pasarlo a **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Para hacer lo mismo con C++/WinRT, puedes usar la función [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) para obtener una versión de cadena de estilo C terminada en null, al igual que puede hacerlo desde **std:: wstring** .

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

En cuanto a la implementación de API que toman o devuelven cadenas, normalmente cambiará C++ / c++ / código CX que utiliza **Platform:: String\^**  usar **winrt::hstring** en su lugar.

Este es un ejemplo de una API de C++/ CX que toma una cadena.

```cppcx
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

#### <a name="tostring"></a>ToString()

C++ / c++ / CX proporciona el [Object:: ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) método.

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++ / c++ / WinRT no ofrece directamente esta función, pero puede desactivar las alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>API importantes
* [winrt::delegate struct template](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Crear eventos en C / c++ / WinRT](author-events.md)
* [Simultaneidad y operaciones asincrónicas con C++ / c++ / WinRT](concurrency.md)
* [Consumir API con C++/WinRT](consume-apis.md)
* [Controlar eventos mediante el uso de delegados en C / c++ / WinRT](handle-events.md)
* [Interoperabilidad entre C++/WinRT y C++/CX](interop-winrt-cx.md)
* [Referencia de Microsoft 3.0 de lenguaje de definición de interfaz](/uwp/midl-3)
* [Migrar a C++/WinRT desde WRL](move-to-winrt-from-wrl.md)
* [Cadena de control en C++ / c++ / WinRT](strings.md)
