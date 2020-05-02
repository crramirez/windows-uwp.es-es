---
description: En este tema se describen los detalles técnicos implicados en la portabilidad del código fuente de un proyecto de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a su equivalente de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
title: Migrar a C++/WinRT desde C++/CX
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrate, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: c5f8b9548bba704a7035b014ca3728db8bcbcc16
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80662395"
---
# <a name="move-to-cwinrt-from-ccx"></a>Migrar a C++/WinRT desde C++/CX

En este tema se describen los detalles técnicos implicados en la portabilidad del código fuente de un proyecto de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a su equivalente de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="porting-strategies"></a>Estrategias de portabilidad

Si deseas trasladar gradualmente tu código de C++/CX a C++/WinRT, puedes hacerlo. El código de C++/CX y de C++/WinRT puede coexistir en el mismo proyecto, a excepción de la compatibilidad con el compilador XAML y los componentes de Windows Runtime. Para estos dos casos necesitarás tener como destino C++/CX o C++/WinRT dentro del mismo proyecto.

> [!IMPORTANT]
> Si el proyecto compila una aplicación XAML, uno de los flujos de trabajo recomendados es crear primero un proyecto en Visual Studio mediante una de las plantillas de proyecto de C++/WinRT (consulta el artículo de [soporte de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Después, empieza a copiar código fuente y revisa desde el proyecto de C++/CX. Puedes agregar nuevas páginas XAML con **Proyecto** \> **Agregar nuevo elemento...** \> **Visual C++**  > **Página en blanco (C++/WinRT)** .
>
> Como alternativa, puedes usar un componente de Windows Runtime para factorizar el código fuera del proyecto XAML de C++/CX al trasladarlo. Puedes trasladar el máximo de código de C++/CX a un componente y cambiar el proyecto XAML a C++/WinRT. O bien, dejar el proyecto XAML como C++/CX, crear un nuevo componente de C++/WinRT y empezar a trasladar el código de C++/CX fuera del proyecto XAML al componente. También puedes tener un proyecto de componente C++/CX junto con uno de C++/WinRT en la misma solución, hacer referencia a ambos desde el proyecto de aplicación y migrar gradualmente de uno al otro. Consulta [Interop between C++/WinRT and C++/CX](interop-winrt-cx.md) (Interoperabilidad entre C++/WinRT y C++/CX) para más detalles sobre el uso de proyecciones de dos lenguajes en el mismo proyecto.

> [!NOTE]
> Tanto [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) como el SDK de Windows declaran tipos en el espacio de nombres raíz **Windows**. Un tipo de Windows proyectado en C++/WinRT tiene el mismo nombre totalmente cualificado que el tipo de Windows, pero se coloca en el espacio de nombres **winrt** de C++. Estos espacios de nombres diferentes te permiten migrar de C++/CX a C++/WinRT a tu propio ritmo.

Teniendo en cuenta las excepciones que se mencionaron anteriormente, el primer paso al trasladar un proyecto de C++/CX a C++/WinRT es agregarle manualmente el soporte para C++/WinRT (consulta el artículo sobre el [soporte de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para ello, instala el [paquete NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto. Abre el proyecto en Visual Studio, haz clic en **Proyecto** \> **Administrar paquetes NuGet...** \> **Busca**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete de ese proyecto. Un efecto de ese cambio es que el soporte para C++/CX está desactivado en el proyecto. Es buena idea dejar el soporte desactivado para que los mensajes compilados te ayuden a encontrar y migrar todas tus dependencias en C++/CX; o bien, puedes volver a activar el soporte (en las propiedades del proyecto, **C/C++** \> **General** \> **Usar extensión de Windows Runtime** \> **Sí (/ZW)** ) y migrar de manera gradual.

Como alternativa, puedes agregar manualmente la siguiente propiedad al archivo `.vcxproj` mediante la página de propiedades del proyecto de C++/WinRT en Visual Studio. Para obtener una lista de opciones de personalización similares (que ajustan el comportamiento de la herramienta `cppwinrt.exe`), consulta [readme](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) del paquete NuGet Microsoft.Windows.CppWinRT.

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

Luego, establece la propiedad de proyecto **General** \> **Versión de la plataforma de destino** en 10.0.17134.0 (Windows 10, versión 1803) o posterior.

En el archivo de encabezado precompilado (normalmente `pch.h`), incluye `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si incluyes cualquier encabezado de API de Windows proyectado de C++/ WinRT (por ejemplo, `winrt/Windows.Foundation.h`), no necesitas incluir explícitamente `winrt/base.h` así, ya que se incluirá automáticamente.

Si el proyecto también está usando tipos de [Windows Runtime C++ Template Library (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) (Biblioteca de plantillas C++ de Windows Runtime [WRL]), consulta [Move to C++/WinRT from WRL](move-to-winrt-from-wrl.md) (Migración de C++/WinRT desde WRL).

## <a name="file-naming-conventions"></a>Convenciones de nomenclatura de archivos

### <a name="xaml-markup-files"></a>Archivos de marcado XAML

| | C++/CX | C++/WinRT |
| - | - | - |
| **Archivos XAML para desarrolladores** | MyPage.xaml<br/>MyPage.xaml.h<br/>MyPage.xaml.cpp | MyPage.xaml<br/>MyPage.h<br/>MyPage.cpp<br/>MyPage.idl (ver a continuación) |
| **Archivos XAML generados** | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp<br/>MyPage.g.h |

Ten en cuenta que C++/WinRT quita `.xaml` de los nombres de archivo `*.h` y `*.cpp`.

C++/WinRT agrega un archivo de desarrollador adicional, el **archivo MIDL (.idl)** . C++/CX genera automáticamente este archivo de manera interna y lo agrega a todos los miembros públicos y protegidos. En C++/WinRT, puedes agregar y crear el archivo tú mismo. Para más detalles, ejemplos de código y un tutorial de creación de IDL, consulta [Controles de XAML; enlazar a una propiedad de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property).

Consulta también [Factorizar clases en tiempo de ejecución en archivos Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

### <a name="runtime-classes"></a>Clases en tiempo de ejecución

C++/CX no impone restricciones sobre los nombres de los archivos de encabezado; es habitual colocar varias definiciones de clase en tiempo de ejecución en un único archivo de encabezado, en especial en el caso de las clases pequeñas. Pero C++/WinRT requiere que el nombre de archivo de encabezado de cada clase en tiempo de ejecución aparezca después del nombre de clase. 

| C++/CX | C++/WinRT |
| - | - |
| **Common.h**<br>`ref class A { ... }`<br>`ref class B { ... }` | **Common.idl**<br>`runtimeclass A { ... }`<br>`runtimeclass B { ... }` |
|  | **A.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct A { ... };`<br>`}` |
|  | **B.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct B { ... };`<br>`}` |

Menos común (aunque sigue siendo válido) en C++/CX es usar archivos de encabezado con nombres diferentes para los controles personalizados de XAML. Tendrás que cambiar el nombre de este archivo de encabezado para que coincida con el nombre de la clase.

| C++/CX | C++/WinRT |
| - | - |
| **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` | **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` |
| **A.h**<br>`partial ref class LongNameForA { ... }` | **LongNameForA.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct LongNameForA { ... };`<br>`}` |

## <a name="header-file-requirements"></a>Requisitos de archivos de encabezado

C++/CX no requiere que incluyas ningún archivo de encabezado especial, ya que de forma interna genera automáticamente archivos de encabezado a partir de archivos `.winmd`. En C++/CX es habitual usar directivas de `using` para los espacios de nombres que consumes por nombre.

```cppcx
using namespace Windows::Media::Playback;

String^ NameOfFirstVideoTrack(MediaPlaybackItem^ item)
{
    return item->VideoTracks->GetAt(0)->Name;
}
```

La directiva `using namespace Windows::Media::Playback` nos permite escribir `MediaPlaybackItem` sin un prefijo de espacio de nombres. También hemos tocado el espacio de nombres `Windows.Media.Core`, ya que `item->VideoTracks->GetAt(0)` devuelve **Windows.Media.Core.VideoTrack**. Pero no hemos tenido que escribir el nombre **VideoTrack** en ningún lugar, por lo que no necesitamos una directiva de `using Windows.Media.Core`.

Sin embargo, C++/WinRT te exige incluir un archivo de encabezado que se corresponda con cada espacio de nombres que consumes, incluso aunque no lo nombres.

```cppwinrt
#include <winrt/Windows.Media.Playback.h>
#include <winrt/Windows.Media.Core.h> // !!This is important!!

using namespace winrt;
using namespace Windows::Media::Playback;

winrt::hstring NameOfFirstVideoTrack(MediaPlaybackItem const& item)
{
    return item.VideoTracks().GetAt(0).Name();
}
```

Por otro lado, aunque el evento **MediaPlaybackItem.AudioTracksChanged** es de tipo **TypedEventHandler\<MediaPlaybackItem, Windows.Foundation.Collections.IVectorChangedEventArgs\>** , no es necesario incluir `winrt/Windows.Foundation.Collections.h`, porque no usamos ese evento.

Además, C++/WinRT requiere que incluyas los archivos de encabezado para los espacios de nombres que consume el marcado XAML.

```xaml
<!-- MainPage.xaml -->
<Rectangle Height="400"/>
```

El uso de la clase **Rectangle** significa que tienes que agregar este include.

```cppwinrt
// MainPage.h
#include <winrt/Windows.UI.Xaml.Shapes.h>
```

Si olvidas un archivo de encabezado, todo se compilará correctamente, pero obtendrá errores del enlazador, ya que faltan las clases `consume_`.

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

El constructor predeterminado para una referencia de acento circunflejo de C++/CX lo inicializa en null. Este es un ejemplo de código de C++/CX en el que se crea una variable o un campo del tipo correcto, pero que se no inicializa. En otras palabras, inicialmente no hace referencia a **TextBlock**; pensamos asignar una referencia más adelante.

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

Un objeto de C++/WinRT es un valor, por lo que puede asignarlo en la pila o como campo de un objeto. No uses *nunca*`ref new` (ni `new`) para asignar un objeto de C++/WinRT. En segundo plano, se sigue llamando a **RoActivateInstance**.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Si un recurso es caro de inicializar, es habitual retrasar su inicialización hasta que sea realmente necesario. Como ya se ha mencionado, el constructor predeterminado para una referencia de acento circunflejo de C++/CX lo inicializa en null.

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

El mismo código migrado a C++/WinRT. Ten en cuenta el uso del constructor **std::nullptr_t**. Para más información acerca del constructor, consulta [Inicialización demorada](consume-apis.md#delayed-initialization).

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

## <a name="how-the-default-constructor-affects-collections"></a>De qué manera el constructor predeterminado afecta a las colecciones

Los tipos de colección de C++ usan el constructor predeterminado, lo que puede dar lugar a la construcción de objetos imprevistos.

| Escenario | C++/CX | C++/WinRT (incorrecto) | C++/WinRT (correcto) |
| - | - | - | - |
| Variable local, inicialmente vacía | `TextBox^ textBox;` | `TextBox textBox; // Creates a TextBox!` | `TextBox textBox{ nullptr };` |
| Variable miembro, inicialmente vacía | `class C {`<br/>&nbsp;&nbsp;`TextBox^ textBox;`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textBox; // Creates a TextBox!`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textbox{ nullptr };`<br/>`};` |
| Variable global, inicialmente vacía | `TextBox^ g_textBox;` | `TextBox g_textBox; // Creates a TextBox!` | `TextBox g_textBox{ nullptr };` |
| Vector de referencias vacías | `std::vector<TextBox^> boxes(10);` | `// Creates 10 TextBox objects!`<br/>`std::vector<TextBox> boxes(10);` | `std::vector<TextBox> boxes(10, nullptr);` |
| Establecer un valor en un mapa | `std::map<int, TextBox^> boxes;`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`// Creates a TextBox at 2,`<br/>`// then overwrites it!`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`boxes.insert_or_assign(2, value);` |
| Matriz de referencias vacías | `TextBox^ boxes[2];` | `// Creates 2 TextBox objects!`<br/>`TextBox boxes[2];` | `TextBox boxes[2] = { nullptr, nullptr };` |
| Pareja | `std::pair<TextBox^, String^> p;` | `// Creates a TextBox!`<br/>`std::pair<TextBox, String> p;` | `std::pair<TextBox, String> p{ nullptr, nullptr };` |

### <a name="more-about-collections-of-empty-references"></a>Más información sobre las colecciones de referencias vacías

Siempre que tengas **Platform::Array\^** (consulta [Port **Platform::Array\^** ](#port-platformarray)) en C++/CX, tendrás la opción de migrar a **std::vector** en C++/WinRT (de hecho, cualquier contenedor contiguo) en lugar de dejarlo como matriz. Elegir **std::vector** tiene sus ventajas.

Por ejemplo, si bien hay una forma abreviada de crear un vector de tamaño fijo de referencias vacías (consulta la tabla anterior), no existe tal forma abreviada para crear una *matriz* de referencias vacías. Tienes que repetir `nullptr` para cada elemento de una matriz. Si no tiene suficientes, los adicionales se construirán de forma predeterminada.

En el caso de un vector, puedes rellenarlo con referencias vacías en la inicialización (como en la tabla anterior), o puedes rellenarlo con referencias vacías después de la inicialización, con código como este.

```cppwinrt
std::vector<TextBox> boxes(10); // 10 default-constructed TextBoxes.
boxes.resize(10, nullptr); // 10 empty references.
```

### <a name="more-about-the-stdmap-example"></a>Más información sobre el ejemplo de **std::map**

El operador de subíndice `[]` para **std::map** se comporta de la siguiente manera.

- Si la clave se encuentra en el mapa, devuelve una referencia al valor existente (que puedes sobrescribir).
- Si la clave no se encuentra en el mapa, crea una nueva entrada en el mapa que conste de la clave (movida, si es movible) y un *valor construido de forma predeterminada*, y devuelve una referencia al valor (que luego puedes sobrescribir).

En otras palabras, el operador `[]` siempre crea una entrada en el mapa. Esto es diferente de C#, Java y JavaScript.

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

## <a name="derived-classes"></a>Clases derivadas

Para derivar a partir de una clase en tiempo de ejecución, la clase base debe *admitir la composición*. C++/CX no requiere que sigas ningún paso en particular para hacer que estas clases admitan composición, pero C++/WinRT sí. Usa la [palabra clave unsealed](/uwp/midl-3/intro#base-classes) para indicar que quieres que la clase se pueda usar como clase base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

En la clase de encabezado de tu implementación, tienes que incluir el archivo de encabezado de la clase base antes de incluir el encabezado generado automáticamente para la clase derivada. De lo contrario, obtendrás errores como "uso no válido de este tipo como expresión".

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
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

## <a name="boxing-and-unboxing"></a>Conversiones boxing y unboxing

C++/CX aplica automáticamente boxing a los valores escalares para convertirlos en objetos. C++/WinRT requiere que llames a la función [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) de manera explícita. Ambos lenguajes requieren que apliques la conversión unboxing de manera explícita. Consulta [Conversión boxing y unboxing con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

En las tablas siguientes, usaremos estas definiciones.

| C++/CX | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `String^ s;` | `winrt::hstring s;` |
| `Object^ o;` | `IInspectable o;`|

| Operación | C++/CX | C++/WinRT|
|-|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (String^)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX y C# producen excepciones si intentas aplicar unboxing para convertir un puntero nulo en a un tipo de valor. C++/WinRT considera que se trata de un error de programación y se bloquea. En C++/WinRT, usa la función [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) si quieres controlar el caso en el que el objeto no sea del tipo que pensabas que era.

| Escenario | C++/CX | C++/WinRT|
|-|-|-|-|
| Conversión unboxing de un entero conocido | `i = (int)o;` | `i = unbox_value<int>(o);` |
| Si o es null | `Platform::NullReferenceException` | Bloqueo |
| Si o no es un entero con conversión boxing | `Platform::InvalidCastException` | Bloqueo |
| Unboxing de entero, usar reserva si es null; bloquear en cualquier otro caso | `i = o ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Unboxing de entero, si es posible; usar reversión en cualquier otro caso | `auto box = dynamic_cast<IBox<int>^>(o);`<br>`i = box ? box->Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Conversiones boxing y unboxing en una cadena

En cierto modo, una cadena es un tipo de valor y, de otro modo, un tipo de referencia. C++/CX y C++/WinRT tratan las cadenas de manera diferente.

El tipo de ABI [**HSTRING**](/windows/win32/winrt/hstring) es un puntero a una cadena de recuento de referencias. Pero no se obtiene de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable), por lo que, técnicamente, no es un *objeto*. Además, un **HSTRING** nulo representa la cadena vacía. La conversión boxing aplicada a cosas que no se derivan de **IInspectable** se realiza encapsulándolas dentro de un [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_), y Windows Runtime proporciona una implementación estándar en forma de objeto [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (los tipos personalizados se notifican como [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

C++/CX representa una cadena de Windows Runtime como tipo de referencia; mientras que C++/WinRT proyecta una cadena como tipo de valor. Es decir, una cadena nula a la que se ha aplicado la conversión boxing puede tener distintas representaciones en función de cómo llegue allí.

Además, C++/CX te permite desreferenciar una **String^** null, en cuyo caso se comporta como la cadena `""`.

| Comportamiento | C++/CX | C++/WinRT|
|-|-|-|
| Declaraciones | `Object^ o;`<br>`String^ s;` | `IInspectable o;`<br>`hstring s;` |
| Categoría de tipo de cadena | Tipo de referencia | Tipo de valor |
| **HSTRING** null se proyecta como | `(String^)nullptr` | `hstring{}` |
| ¿Son null y `""` idénticos? | Sí | Sí |
| Validez de null | `s = nullptr;`<br>`s->Length == 0` (válido) | `s = hstring{};`<br>`s.size() == 0` (válido) |
| Si asignas una cadena null a un objeto | `o = (String^)nullptr;`<br>`o == nullptr` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Si asignas `""` a un objeto | `o = "";`<br>`o == nullptr` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Conversiones boxing y unboxing básicas.

| Operación | C++/CX | C++/WinRT|
|-|-|-|
| Aplicar boxing a una cadena | `o = s;`<br>Una cadena vacía se convierte en nullptr. | `o = box_value(s);`<br>Una cadena vacía se convierte en un objeto con un valor distinto de null. |
| Unboxing a una cadena conocida | `s = (String^)o;`<br>El objeto null se convierte en una cadena vacía.<br>InvalidCastException si no es una cadena. | `s = unbox_value<hstring>(o);`<br>Un objeto null se bloquea.<br>Se bloquea si no es una cadena. |
| Conversión unboxing de una posible cadena | `s = dynamic_cast<String^>(o);`<br>Un objeto null o que no es una cadena se convierte en una cadena vacía. | `s = unbox_value_or<hstring>(o, fallback);`<br>Un valor null o que no es una cadena se convierte en un elemento Fallback.<br>Una cadena vacía se conserva. |

## <a name="concurrency-and-asynchronous-operations"></a>Operaciones simultáneas y asincrónicas

La biblioteca de patrones de procesamiento paralelo (PPL) ([**concurrency::task**](/cpp/parallel/concrt/reference/task-class), por ejemplo) se actualizó para admitir las referencias de acento circunflejo de C++/CX.

En el caso de C++/WinRT, debes usar las corrutinas y `co_await` en su lugar. Para obtener más información y ejemplos de código, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

## <a name="consuming-objects-from-xaml-markup"></a>Consumir objetos a partir del marcado XAML

En un proyecto de C++/CX, puedes consumir miembros privados y elementos con nombre del marcado XAML. Pero en C++/WinRT, todas las entidades consumidas por la [**extensión de marcado {x:Bind}** ](/windows/uwp/xaml-platform/x-bind-markup-extension) de XAML deben exponerse públicamente en IDL.

Además, el enlace a un valor booleano muestra `true` o `false`en C++/CX, pero muestra **Windows.Foundation.IReference`1\<valor booleano\>** en C++/WinRT.

Para más información y ejemplos de código, consulta [Consumir objetos a partir del marcado](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

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

### <a name="port-platformagile-to-winrtagile_ref"></a>Migración de **Platform::Agile\^** a **winrt::agile_ref**

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

En los casos donde C++/CX necesita usar una matriz, C++/WinRT te permite usar cualquier contenedor contiguo. Consulta [De qué manera el constructor predeterminado afecta a las colecciones](#how-the-default-constructor-affects-collections) para ver el motivo por el que **std::vector** es una buena elección.

Por lo tanto, siempre que tengas **Platform::Array\^** en C++/CX, las opciones de portabilidad incluyen el uso de una lista de inicializadores, una **std::array** o un **std::vector**. Para más información y ejemplos de código, consulta [Standard initializer lists](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) (Listas de inicializadores estándar) y [Standard arrays and vectors](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors) (Vectores y matrices estándar).

### <a name="port-platformexception-to-winrthresult_error"></a>Migración de **Platform::Exception\^** a **winrt::hresult_error**

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

Con C++/CX puedes acceder a la propiedad [**Platform::String::Data**](https://docs.microsoft.com/cpp/cppcx/platform-string-class?view=vs-2019#data) para recuperar la cadena como una matriz **const wchar_t\*** de estilo C (por ejemplo, para pasarla a **std::wcout**).

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

Los tipos de C++/CX proporcionan el método [Object::ToString](/cpp/cppcx/platform-object-class#tostring).

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/ WinRT no ofrece directamente esta función, pero puedes elegir alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT también admite [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) para un número limitado de tipos. Tendrás que agregar sobrecargas para los tipos adicionales a los que quieras aplicar stringify.

| Language | Stringify int | Stringify enum |
| - | - | - |
| C++/CX | `String^ result = "hello, " + intValue.ToString();` | `String^ result = "status: " + status.ToString();` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

En caso de aplicar stringify en una enumeración, debes proporcionar la implementación de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

El enlace de datos suele consumir implícitamente estas aplicaciones de stringify.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Estos enlaces ejecutarán **winrt::to_hstring** de la propiedad enlazada. En el caso del segundo ejemplo (**StatusEnum**), debes proporcionar tu propia sobrecarga de **winrt::to_hstring**; de lo contrario, recibirás un error del compilador.

#### <a name="string-building"></a>Creación de cadenas

C++/CX y C++/WinRT se aplazan a **std::wstringstream** estándar para la creación de cadenas.

| | C++/CX | C++/WinRT |
|-|-|-|
| Anexar cadena, conservando valores null | `stream.print(s->Data(), s->Length);` | `stream << std::wstring_view{ s };` |
| Anexar cadena, detener en el primer valor null | `stream << s->Data();` | `stream << s.c_str();` |
| Extraer resultado | `ws = stream.str();` | `ws = stream.str();` |

#### <a name="more-examples"></a>Más ejemplos

En los ejemplos siguientes, *ws* es una variable de tipo **std::wstring**. Además, mientras que C++/CX puede construir **Platform::String** a partir de una cadena de 8 bits, C++/WinRT no puede.

| Operación | C++/CX | C++/WinRT |
| - | - | - |
| Construir una cadena a partir de un literal | `String^ s = "hello";`<br>`String^ s = L"hello";` | `// winrt::hstring s{ "hello" }; // Doesn't compile`<br>`winrt::hstring s{ L"hello" };` |
| Convertir de **std::wstring**, conservando valores null | `String^ s = ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size());` | `winrt::hstring s{ ws };`<br>`s = winrt::hstring(ws);`<br>`// s = ws; // Doesn't compile` |
| Convertir de **std::wstring**, detener en el primer valor null | `String^ s = ref new String(ws.c_str());` | `winrt::hstring s{ ws.c_str() };`<br>`s = winrt::hstring(ws.c_str());`<br>`// s = ws.c_str(); // Doesn't compile` |
| Convertir a **std::wstring**, conservando valores null | `std::wstring ws{ s->Data(), s->Length };`<br>`ws = std::wstring(s>Data(), s->Length);` | `std::wstring ws{ s };`<br>`ws = s;` |
| Convertir a **std::wstring**, detener en el primer valor null | `std::wstring ws{ s->Data() };`<br>`ws = s->Data();` | `std::wstring ws{ s.c_str() };`<br>`ws = s.c_str();` |
| Pasar un literal a un método | `Method("hello");`<br>`Method(L"hello");` | `// Method("hello"); // Doesn't compile`<br>`Method(L"hello");` |
| Pasar **std::wstring** a un método | `Method(ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size()); // Stops on first null` | `Method(ws);`<br>`// param::winrt::hstring accepts std::wstring_view` |

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
