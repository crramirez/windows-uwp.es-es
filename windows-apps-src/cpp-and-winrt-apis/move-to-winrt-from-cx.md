---
description: En este tema se muestra cómo migrar código de C++/CX a su equivalente en C++/WinRT.
title: Migrar a C++/WinRT desde C++/CX
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrate, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 7fbe10e41da1b330d6f5042bea109a8a0e04f8ad
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66360162"
---
# <a name="move-to-cwinrt-from-ccx"></a>Migrar a C++/WinRT desde C++/CX

En este tema se muestra cómo trasladar el código de un proyecto de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a su equivalente en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Estrategias de portabilidad

Si deseas trasladar gradualmente tu código de C++/CX a C++/WinRT, puedes hacerlo. El código de C++/CX y de C++/WinRT puede coexistir en el mismo proyecto, a excepción del soporte para el compilador XAML y los componentes de Windows Runtime. Para estos dos casos necesitarás tener como destino C++/CX o C++/WinRT dentro del mismo proyecto.

> [!IMPORTANT]
> Si el proyecto compila una aplicación XAML, uno de los flujos de trabajo recomendados es crear primero un proyecto en Visual Studio mediante una de las plantillas de proyecto de C++/WinRT (consulta el artículo de [soporte de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Después, empieza a copiar código fuente y revisa desde el proyecto de C++/CX. Puedes agregar nuevas páginas XAML con **proyecto** \> **Agregar nuevo elemento...** \> **Visual C++**  > **Página en blanco (C++/WinRT)** .
>
> Como alternativa, puedes usar un componente de Windows Runtime para factorizar el código fuera del proyecto XAML de C++/CX al trasladarlo. Puedes trasladar el máximo de código de C++/CX a un componente y cambiar el proyecto XAML a C++/WinRT. O bien, dejar el proyecto XAML como C++/CX, crear un nuevo componente de C++/WinRT y empezar a trasladar el código de C++/CX fuera del proyecto XAML al componente. También puedes tener un proyecto de componente C++/CX junto con uno de C++/WinRT en la misma solución, hacer referencia a ambos desde el proyecto de aplicación y migrar gradualmente de uno al otro. Consulta [Interop between C++/WinRT and C++/CX](interop-winrt-cx.md) (Interoperabilidad entre C++/WinRT y C++/CX) para más detalles sobre el uso de proyecciones de dos lenguajes en el mismo proyecto.

> [!NOTE]
> Tanto [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) como el SDK de Windows declaran tipos en el espacio de nombres raíz **Windows**. Un tipo de Windows proyectado en C++/WinRT tiene el mismo nombre totalmente cualificado que el tipo de Windows, pero se coloca en el espacio de nombres **winrt** de C++. Estos espacios de nombres diferentes te permiten migrar de C++/CX a C++/WinRT a tu propio ritmo.

Teniendo en cuenta las excepciones que se mencionaron anteriormente, el primer paso al trasladar un proyecto de C++/CX a C++/WinRT es agregarle manualmente el soporte para C++/WinRT (consulta el artículo sobre el [soporte de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para ello, instala el [paquete NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto. Abre el proyecto en Visual Studio, haz clic en **Proyecto** \> **Administrar paquetes NuGet...** \> **Examinar**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete de ese proyecto. Un efecto de ese cambio es que el soporte para C++/CX está desactivado en el proyecto. Es buena idea dejar el soporte desactivado para que los mensajes compilados te ayuden a encontrar y migrar todas tus dependencias en C++/CX; o bien, puedes volver a activar el soporte (en las propiedades del proyecto, **C/C++** \>**General**\>**Usar extensión de Windows Runtime**\>**Sí (/ZW)** ) y migrar de manera gradual.

Establece la propiedad de proyecto **General** \> **Versión de la plataforma de destino** en 10.0.17134.0 (Windows 10, versión 1803) o posterior.

En el archivo de encabezado precompilado (normalmente `pch.h`), incluye `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si incluyes cualquier encabezado de API de Windows proyectado de C++/ WinRT (por ejemplo, `winrt/Windows.Foundation.h`), no necesitas incluir explícitamente `winrt/base.h` así, ya que se incluirá automáticamente.

Si el proyecto también está usando tipos de [Windows Runtime C++ Template Library (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) (Biblioteca de plantillas C++ de Windows Runtime [WRL]), consulta [Move to C++/WinRT from WRL](move-to-winrt-from-wrl.md) (Migración de C++/WinRT desde WRL).

## <a name="parameter-passing"></a>Paso de parámetros
Al escribir código fuente de C++/CX, pasas tipos C++/CX como parámetros de función, como referencias de circunflejo (\^).

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

En C++/WinRT, para las funciones sincrónicas, debes usar los parámetros `const&` de manera predeterminada. Eso evitará copias y sobrecarga entrelazada. Pero tus corrutinas deben usar paso-por-valor para garantizar que capturan por valor y evitar los problemas de ciclo de vida (para más información, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Un objeto de C++/WinRT es esencialmente un valor que contiene un puntero a interfaz para el objeto de Windows Runtime de copia de seguridad. Cuando copias un objeto de C++/WinRT, el compilador copia el puntero a interfaz encapsulado, lo cual incrementa su recuento de referencia. La destrucción final de la copia conlleva la disminución del recuento de referencia. Por lo tanto, incurre solo en la sobrecarga de una copia cuando sea necesario.

## <a name="variable-and-field-references"></a>Referencias de variables y campos
Al escribir código fuente de C++/CX, usas variables de circunflejo (\^) para hacer referencia a objetos de Windows Runtime y el operador de flecha (-&gt;) para desreferenciarlas.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Al trasladar código al equivalente C++/WinRT, puedes perder mucho tiempo quitando los acentos circunflejos y cambiando el operador de flecha (-&gt;) al punto (.). Los tipos proyectados de C++/ WinRT son valores, no punteros.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

El constructor predeterminado para un puntero circunflejo de C++/CX lo inicializa en null. Este es un ejemplo de código de C++/CX en el que se crea una variable o un campo del tipo correcto, pero que se no inicializa. En otras palabras, inicialmente no hace referencia a **TextBlock**; pensamos asignar una referencia más adelante.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Para el equivalente en C++/WinRT, consulta [Delayes initialization](consume-apis.md#delayed-initialization) (Demora en la inicialización).

## <a name="properties"></a>Propiedades
Las extensiones de lenguaje C++/CX incluyen el concepto de propiedades. Al escribir código fuente de C++/CX, puedes acceder a una propiedad como si fuera un campo. La versión de C++ estándar no tiene el concepto de propiedad, por lo que en C++/WinRT se llama a las funciones get y set.

En los ejemplos siguientes, los valores **XboxUserId**, **UserState**, **PresenceDeviceRecords** y **Size** son propiedades.

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

Ten en cuenta que la función **PresenceDeviceRecords** devuelve un objeto de Windows Runtime que tiene una función **Size**. Como el objeto devuelto también es un tipo proyectado de C++/WinRT, desreferenciamos mediante el operador de punto para llamar a **Size**.

### <a name="setting-a-property-to-a-new-value"></a>Establecer una propiedad en un valor nuevo
El establecimiento de una propiedad en un valor nuevo sigue un patrón similar. En primer lugar, en C++/CX.

```cppcx
record->UserState = newValue;
```

Para hacer lo equivalente en C++/WinRT, llama a una función con el mismo nombre que la propiedad y pasa un argumento.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Crear una instancia de una clase
Trabajas con un objeto de C++/CX mediante un manipulador que se conoce comúnmente como referencia de circunflejo (\^). Crea un objeto mediante la palabra clave `ref new`, que a su vez llama a [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) para activar una instancia nueva de la clase en tiempo de ejecución.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Un objeto de C++/WinRT es un valor, por lo que puede asignarlo en la pila o como campo de un objeto. No uses *nunca* `ref new` (ni `new`) para asignar un objeto de C++/WinRT. En segundo plano, se sigue llamando a **RoActivateInstance**.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Si un recurso es caro de inicializar, es habitual retrasar su inicialización hasta que sea realmente necesario.

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

El mismo código migrado a C++/WinRT. Ten en cuenta el uso del constructor `nullptr`. Para más información acerca del constructor, consulta [Consume APIs with C++/WinRT](consume-apis.md) (Consumo de las API con C++/WinRT).

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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Conversión de una clase base en tiempo de ejecución a una derivada
Es habitual tener una referencia a base que sabes que hace referencia a un objeto de un tipo derivado. En C++/CX, se usa `dynamic_cast` para *convertir* la referencia a base en una referencia a derivado. `dynamic_cast` es simplemente una llamada oculta a [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Este es un ejemplo típico: estás controlando un evento con cambio de propiedad de dependencia y deseas convertir **DependencyObject** al tipo real que posee la propiedad de dependencia.

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

El código de C++/WinRT equivalente reemplaza `dynamic_cast` por una llamada a la función [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) que encapsula **QueryInterface**. También tienes la opción de llamar a [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) en su lugar, lo que produce una excepción si no se devuelve la consulta de la interfaz necesaria (la interfaz predeterminada del tipo que está solicitando). Este es un ejemplo de código de C++/WinRT.

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
Este es un ejemplo típico del control de un evento en C++/CX mediante una función lambda como delegado.

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

En lugar de una función lambda, puedes elegir implementar tu delegado como función libre o como función de puntero a miembro. Para más información, consulta [Control de eventos mediante delegados en C++/WinRT](handle-events.md).

Si vas a portar desde una base de código de C++/CX donde se usan internamente eventos y delegados (no archivos binarios), [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) te ayudará a replicar ese patrón en C++/WinRT. Consulta también [Parameterized delegates, simple signals, and callbacks within a project](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project) (Delegados con parámetros, señales simples y devoluciones de llamada dentro de un proyecto).

## <a name="revoking-a-delegate"></a>Revocación de un delegado
En C++/CX s usa el operador `-=` para revocar un registro de eventos anterior.

```cppcx
myButton->Click -= token;
```

Este es el equivalente en C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Para más información y opciones, consulta [Revoke a registered delegate](handle-events.md#revoke-a-registered-delegate) (Revocación de un delegado registrado).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Asignación de tipos **Plataforma** de C++/CX a C++/WinRT
C++/CX proporciona varios tipos de datos en el espacio de nombres **Plataforma**. Estos tipos no son de la versión C++ estándar, por lo que solo puedes usarlos al habilitar extensiones de lenguaje de Windows Runtime (propiedad de proyecto de Visual Studio **C/C++**  > **General** > **Usar extensión de Windows Runtime** > **Sí (/ZW)** ). La tabla siguiente ayuda a migrar desde tipos **Plataforma** a sus equivalentes en C++/WinRT. Una vez que lo hayas hecho, puesto que C++/WinRT es C++ estándar, puedes desactivar la opción `/ZW`.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | Consulta [Migración de **Platform::Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>Migración de **Platform::Agile\^** a **winrt::agile_ref**
La escritura de **Platform::Agile\^** en C++/CX representa una clase de Windows Runtime que se puede acceder desde cualquier subproceso. El equivalente de C++/WinRT es [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

En C++/CX.

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

En C++/WinRT.

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Migración de **Platform::Array\^**
Las opciones incluyen el uso de una lista de inicializadores, **std::array** o **std::vector**. Para más información y ejemplos de código, consulta [Standard initializer lists](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) (Listas de inicializadores estándar) y [Standard arrays and vectors](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors) (Vectores y matrices estándar).

### <a name="port-platformexception-to-winrthresulterror"></a>Migración de **Platform::Exception\^** a **winrt::hresult_error**
El tipo **Platform::Exception\^** se produce en C++/CX cuando una API de Windows Runtime devuelve un HRESULT que no es S\_OK. El equivalente de C++/WinRT es [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Para migrar a C++/WinRT, cambia todo el código que usa **Platform::Exception\^** para que use **winrt::hresult_error**.

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
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | llamada a [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
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

Ten en cuenta que cada clase (mediante la clase base **hresult_error**) proporciona una función [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) que devuelve el HRESULT del error, y una función [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) que devuelve la representación de la cadena de ese HRESULT.

Este es un ejemplo de generación de una excepción en C++/CX.

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Y el equivalente en C++/WinRT.

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Migración de **Platform::Object\^** a **winrt::Windows::Foundation::IInspectable**
Como todos los tipos de C++/WinRT, **winrt::Windows::Foundation::IInspectable** es un tipo de valor. Aquí te mostramos cómo inicializar una variable de ese tipo en null.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Migración de **Platform::String\^** a **winrt::hstring**
**Platform::String\^** equivale al tipo ABI HSTRING de Windows Runtime. Para C++/WinRT, el equivalente es [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Pero con C++/WinRT puedes llamar a las API de Windows Runtime mediante tipos de cadenas de caracteres anchos de la biblioteca estándar de C++ como **std::wstring** o literales de cadena de caracteres anchos. Para más información y ejemplos de código, consulta [Control de cadenas en C++/WinRT](strings.md).

Con C++/CX puedes acceder a la propiedad [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) para recuperar la cadena como una matriz **const wchar_t\*** de estilo C (por ejemplo, para pasarla a **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Para hacer lo mismo con C++/WinRT, puedes usar la función [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) para obtener una versión de cadena de estilo C terminada en null, al igual que desde **std::wstring**.

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

En cuanto a la implementación de las API que toman o devuelven cadenas, por lo general cambias cualquier código de C++/CX que use **Platform::String\^** para que use **winrt::hstring** en su lugar.

Este es un ejemplo de una API de C++/ CX que toma una cadena.

```cppcx
void LogWrapLine(Platform::String^ str);
```

Para C++/WinRT podrías declarar esa API en [MIDL 3.0](/uwp/midl-3) así.

```idl
// LogType.idl
void LogWrapLine(String str);
```

La cadena de herramientas de C++/WinRT generará entonces código fuente con este aspecto automáticamente.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX proporciona el método [Object::ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring).

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/ WinRT no ofrece directamente esta función, pero puedes elegir alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>API importantes
* [winrt::delegate struct template](/uwp/cpp-ref-for-winrt/delegate)
* [Estructura winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Crear eventos en C++/WinRT](author-events.md)
* [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md)
* [Consumir API con C++/WinRT](consume-apis.md)
* [Control de eventos mediante delegados en C++/WinRT](handle-events.md)
* [Interoperabilidad entre C++/WinRT y C++/CX](interop-winrt-cx.md)
* [Referencia de Lenguaje de definición de interfaz de Microsoft 3.0](/uwp/midl-3)
* [Migrar a C++/WinRT desde WRL](move-to-winrt-from-wrl.md)
* [Control de cadenas en C++/WinRT](strings.md)
