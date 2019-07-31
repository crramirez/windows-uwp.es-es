---
description: En este tema se muestra cómo usar las API de C++/WinRT, tanto si se han implementado mediante Windows, un proveedor de componentes de terceros o por tu cuenta.
title: Consumo de API con C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projected, projection, implementation, runtime class, activation
ms.localizationpriority: medium
ms.openlocfilehash: 5a3d4b554fafeb2053e4e6af831c224b5eacd151
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328867"
---
# <a name="consume-apis-with-cwinrt"></a>Consumo de API con C++/WinRT

En este tema se muestra cómo consumir las API de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), independientemente de que formen parte de Windows, las haya implementado un proveedor de componentes de terceros o las hayas implementado tú mismo.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Si la API está en un espacio de nombres de Windows
Este es el caso más común en el que consumirás una API de Windows Runtime. Para cada tipo de un espacio de nombres de Windows definido en los metadatos, C++/WinRT define un equivalente de C++ descriptivo (denominado el *tipo proyectado*). Un tipo proyectado tiene el mismo nombre completo que el tipo de Windows, pero se colocado en el espacio de nombres **winrt** de C++ mediante la sintaxis de C++. Por ejemplo, [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) se proyecta en C++/WinRT como **winrt::Windows::Foundation::Uri**.

Este es un ejemplo de código sencillo. Si quieres copiar y pegar los siguientes ejemplos de código directamente en el archivo de código fuente principal de un proyecto de **aplicación de consola Windows (C++/WinRT)** , establece primero **No utilizar encabezados precompilados** en las propiedades del proyecto.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
    Uri combinedUri = contosoUri.CombineUri(L"products");
}
```

El encabezado incluido `winrt/Windows.Foundation.h` forma parte del SDK, que se encuentra dentro de la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Los encabezados de dicha carpeta contienen tipos de espacios de nombres de Windows proyectados en C++/WinRT. En este ejemplo, `winrt/Windows.Foundation.h` contiene **winrt::Windows::Foundation::Uri**, que es el tipo proyectado para la clase en tiempo de ejecución [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Cada vez que quieras usar un tipo desde un espacio de nombres de Windows, incluye el encabezado C++/WinRT correspondiente a dicho espacio de nombres. Las directivas `using namespace` son opcionales, pero recomendables.

En el ejemplo de código anterior, tras inicializar C++/WinRT, apilamos/asignamos un valor del tipo proyectado **winrt::Windows::Foundation::Uri** a través de uno de sus constructores documentados públicamente [[**Uri(String)** ](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_), en este ejemplo]. Para este ejemplo, el caso de uso más común, eso es normalmente todo lo que tienes que hacer. Una vez que tengas un valor del tipo proyectado de C++/WinRT, puedes tratarlo como si fuera una instancia del tipo Windows Runtime real, ya que tiene los mismos miembros.

De hecho, dicho valor proyectado es un proxy; esencialmente no es más que un puntero inteligente a un objeto de respaldo. Los constructores del valor proyectado llaman a [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) para crear una instancia de la clase de Windows Runtime de respaldo (en este caso es **Windows.Foundation.Uri**) y almacenan la interfaz predeterminada de dicho objeto en el nuevo valor proyectado. Como se ilustra a continuación, las llamadas a los miembros del valor proyectado realmente se delegan, a través del puntero inteligente, al objeto de respaldo, que es donde se producen los cambios de estado.

![El tipo Windows::Foundation::Uri proyectado](images/uri.png)

Cuando el valor de `contosoUri` queda fuera de ámbito, se destruye y libera su referencia a la interfaz predeterminada. Si esta es la última referencia al objeto **Windows.Foundation.Uri** de Windows Runtime de respaldo, el objeto de respaldo se destruye también.

> [!TIP]
> Un *tipo proyectado* es un contenedor sobre una clase en tiempo de ejecución que se usa para consumir sus API. Una *interfaz proyectada* es un contenedor sobre una interfaz de Windows Runtime.

## <a name="cwinrt-projection-headers"></a>Encabezados de proyección de C++/WinRT
Para consumir las API de espacio de nombres de Windows desde C++/WinRT, incluye encabezados desde la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Es común que un tipo de un espacio de nombres subordinado haga referencia a tipos de su espacio de nombres principal inmediato. En consecuencia, cada encabezado de proyección de C++/WinRT incluye automáticamente su archivo de encabezado de espacio de nombres principal; por lo que no *es preciso* incluirlo de manera explícita. Sin embargo, si lo haces, no se producirá ningún error.

Por ejemplo, para el espacio de nombres [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates), las definiciones de tipo de C++/WinRT equivalentes residen en `winrt/Windows.Security.Cryptography.Certificates.h`. Los tipos de **Windows::Security::Cryptography::Certificates** requieren tipos en el espacio de nombres **Windows::Security::Cryptography** principal y los tipos de dicho espacio de nombres podrían requerir tipos en su propio **Windows::Security** principal.

Por consiguiente, si incluyes `winrt/Windows.Security.Cryptography.Certificates.h`, dicho archivo a su vez incluye `winrt/Windows.Security.Cryptography.h` y `winrt/Windows.Security.Cryptography.h` incluye `winrt/Windows.Security.h`. Ahí es donde se detiene el seguimiento, ya que no hay ningún `winrt/Windows.h`. Este proceso de inclusión transitivo se detiene en el espacio de nombres de segundo nivel.

Este proceso incluye transitivamente los archivos de encabezado que proporcionan las *declaraciones* e *implementaciones* necesarias para las clases definidas en los espacios de nombres principales.

Un miembro de un tipo de un espacio de nombres puede hacer referencia a uno o varios tipos de otros espacios de nombres no relacionados. Para que el compilador compile correctamente estas definiciones de miembros, es preciso que vea las declaraciones de tipo de la clausura de todos estos tipos. En consecuencia, cada encabezado de proyección de C++/WinRT incluye los encabezados de espacio de nombres que necesita para *declarar* los tipos dependientes. A diferencia de los espacios de nombres principales, este proceso *no* incorpora las *implementaciones* de los tipos a los que se hace referencia.

> [!IMPORTANT]
> Cuando quieras *usar* realmente un tipo (crear una instancia, llamar a métodos, etc.) declarado en un espacio de nombres no relacionado, debes incluir el archivo de encabezado de espacio de nombres apropiado de dicho tipo. Solo se incluyen automáticamente *declaraciones*, no *implementaciones*.

Por ejemplo, si solo incluyes `winrt/Windows.Security.Cryptography.Certificates.h`, eso hace que las declaraciones se incorporen desde estos espacios de nombres (y así sucesivamente, de forma transitiva).

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

En otras palabras, algunas API se declaran más adelante en un encabezado que hayas incluido. Pero sus definiciones están en un encabezado que aún no has incluido. Por consiguiente, si después llamas a [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri), recibirás un error de enlazador que indica que el miembro no está definido. La solución es usar `#include <winrt/Windows.Foundation.h>` explícitamente. En general, cuando veas un error del vinculador como este, incluye el encabezado asignado para el espacio de nombres de la API y vuelve a compilar.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>Acceso a miembros a través del objeto, de una interfaz o de la ABI
Con la proyección de C++/WinRT, la representación del runtime de una clase de Windows Runtime no es más que las interfaces ABI subyacentes. Sin embargo, para tu comodidad, puedes escribir código frente a las clases de la manera que el autor pretendía. Por ejemplo, puedes llamar al método **ToString** de un [**identificador URI**](/uwp/api/windows.foundation.uri) como si fuese un método de la clase (de hecho, en modo no visible, es un método en la otra interfaz **IStringable**).

`WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

Esta comodidad se logra a través de una consulta en la interfaz adecuada. Pero siempre tienes el control. Puedes optar por sacrificar un poco de dicha comodidad a cambio de mejorar algo el rendimiento, lo que se logra recuperando la interfaz IStringable y usándola directamente. En el siguiente ejemplo de código, se obtiene un puntero de interfaz IStringable real en tiempo de ejecución (a través de una consulta puntual). Después, la llamada a **ToString** es directa y evita cualquier otra llamada a **QueryInterface** .

```cppwinrt
...
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

Puedes elegir esta técnica si sabes que vas a llamar a varios métodos en la misma interfaz.

A propósito, si deseas acceder a miembros en el nivel de la ABI, puedes hacerlo. El ejemplo de código siguiente muestra cómo. En [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md), hay más detalles y ejemplos de código.

```cppwinrt
#include <Windows.Foundation.h>
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
using namespace winrt::Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };

    int port{ contosoUri.Port() }; // Access the Port "property" accessor via C++/WinRT.

    winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri{
        contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>() };
    HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
}
```

## <a name="delayed-initialization"></a>Inicialización demorada

En C++/WinRT, cada tipo proyectado tiene un constructor especial **std::nullptr_t** de C++/WinRT. A excepción de este, todos los constructores de tipo proyectado &mdash;incluido el constructor predeterminado&mdash; hacen que se cree un objeto de Windows Runtime de respaldo y te proporcionan un puntero inteligente a él. Por lo tanto, esa regla se aplica siempre que se use el constructor predeterminado, como en variables locales no inicializadas, variables globales no inicializadas y variables de miembro no inicializadas.

Si, por otro lado, quieres crear una variable de un tipo proyectado sin ella y construir en su lugar un objeto de Windows Runtime de respaldo (con el fin de poder retrasar ese trabajo hasta más adelante), puedes hacerlo. Declara la variable o el campo con ese constructor especial **std::nullptr_t** de C++/WinRT (que la proyección de C++/WinRT inyecta en cada clase en tiempo de ejecución). Usamos ese constructor especial con *m_gamerPicBuffer* en el ejemplo de código siguiente.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
using namespace winrt::Windows::Storage::Streams;

#define MAX_IMAGE_SIZE 1024

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

int main()
{
    winrt::init_apartment();
    Sample s;
    // ...
    s.DelayedInit();
}
```

Todos los constructores del tipo proyectado *excepto* el constructor **std::nullptr_t** hacen que se cree un objeto de Windows Runtime de respaldo. El constructor **std:: nullptr_t** es básicamente una operación nula. Espera que el objeto proyectado se inicialice más adelante. Por tanto, independientemente de que una clase en tiempo de ejecución tenga un constructor predeterminado, puedes usar esta técnica para que una inicialización retrasada sea eficiente.

Esta consideración afecta a otros lugares en los que se invocando al constructor predeterminado, como en vectores y mapas. Fíjate en este ejemplo de código, para el que necesitarás un proyecto de **Aplicación vacía (C++/WinRT)** .

```cppwinrt
std::map<int, TextBlock> lookup;
lookup[2] = value;
```

La asignación crea un objeto **TextBlock**e inmediatamente lo sobrescribe con `value`. Este es el recurso.

```cppwinrt
std::map<int, TextBlock> lookup;
lookup.insert_or_assign(2, value);
```

Además, consulta [De qué manera el constructor predeterminado afecta a las colecciones](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#how-the-default-constructor-affects-collections).

### <a name="dont-delay-initialize-by-mistake"></a>No inicializar con retraso por error

Ten cuidado de no invocar el constructor **std:: nullptr_t** por error. La resolución de conflictos del compilador favorece dicho constructor por sobre los constructores de fábrica. Por ejemplo, observa estas dos definiciones de clase en tiempo de ejecución.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox();
}

// Gift.idl
runtimeclass Gift
{
    Gift(GiftBox giftBox); // You can create a gift inside a box.
}
```

Supongamos que queremos construir un objeto **Gift** que no está dentro de un cuadro (un **Gift** que se construye con un tipo **GiftBox** sin inicializar). En primer lugar, echemos un vistazo a la forma *incorrecta* de hacerlo. Sabemos que hay un constructor de **Gift** que usa un tipo **GiftBox**. Sin embargo, nos tienta pasar un valor **GiftBox** nulo (invocar el constructor de **Gift** a través de la inicialización uniforme tal como lo hacemos a continuación), pero *no* obtendremos el resultado que deseamos.

```cppwinrt
// These are *not* what you intended. Doing it in one of these two ways
// actually *doesn't* create the intended backing Windows Runtime Gift object;
// only an empty smart pointer.

Gift gift{ nullptr };
auto gift{ Gift(nullptr) };
```

Lo que obtenemos aquí es un objeto **Gift** sin inicializar. No obtenemos un **Gift** con un tipo **GiftBox** sin inicializar. Esta es la forma *correcta* de hacerlo.

```cppwinrt
// Doing it in one of these two ways creates an initialized
// Gift with an uninitialized GiftBox.

Gift gift{ GiftBox{ nullptr } };
auto gift{ Gift(GiftBox{ nullptr }) };
```

En el ejemplo incorrecto, pasar un valor `nullptr` literal se resuelve en favor del constructor de inicialización con retraso. Para resolver en favor del constructor de fábrica, el tipo del parámetro debe ser un valor **GiftBox**. Aun así, tienes la opción de pasar un valor **GiftBox** con inicialización demorada de forma explícita, tal y como se muestra en el ejemplo correcto.

El siguiente ejemplo *también* es correcto, ya que el parámetro tiene el tipo GiftBox en vez de **std::nullptr_t**.

```cppwinrt
GiftBox giftBox{ nullptr };
Gift gift{ giftBox }; // Calls factory constructor.
```

La ambigüedad se produce únicamente cuando pasas un valor `nullptr` literal.

## <a name="dont-copy-construct-by-mistake"></a>No construir con copia por error

Esta advertencia es similar a la descrita en la sección [No inicializar con retraso por error](#dont-delay-initialize-by-mistake) anterior.

Además del constructor de inicialización con retraso, la proyección de C++/WinRT también inserta un constructor de copia en todas las clases en tiempo de ejecución. Se trata de un constructor de parámetro único que acepta el mismo tipo que el objeto que se va a construir. El puntero inteligente resultante apunta al mismo objeto de Windows Runtime que apunta su parámetro de constructor. El resultado es que dos objetos de puntero inteligente apuntan al mismo objeto de respaldo.

Esta es una definición de clase en tiempo de ejecución que vamos a usar en los ejemplos de código.

```idl
// GiftBox.idl
runtimeclass GiftBox
{
    GiftBox(GiftBox biggerBox); // You can place a box inside a bigger box.
}
```

Supongamos que queremos construir un tipo **GiftBox** dentro de un **GiftBox** más grande.

```cppwinrt
GiftBox bigBox{ ... };

// These are *not* what you intended. Doing it in one of these two ways
// copies bigBox's backing-object-pointer into smallBox.
// The result is that smallBox == bigBox.

GiftBox smallBox{ bigBox };
auto smallBox{ GiftBox(bigBox) };
```

La forma *correcta* de hacerlo es llamar explícitamente al generador de activación.

```cppwinrt
GiftBox bigBox{ ... };

// These two ways call the activation factory explicitly.

GiftBox smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
auto smallBox{
    winrt::get_activation_factory<GiftBox, IGiftBoxFactory>().CreateInstance(bigBox) };
```

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Si la API se implementa en un componente de Windows Runtime
Esta sección se aplica si creaste el componente tú mismo o si este procedió de un proveedor.

> [!NOTE]
> Para más información sobre cómo instalar y usar la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

En tu proyecto de aplicación, haz referencia al archivo de metadatos de Windows Runtime del componente de Windows Runtime (`.winmd`) y realiza la compilación. Durante la compilación, la herramienta `cppwinrt.exe` genera una biblioteca de C++ estándar que describe completamente&mdash;o *proyecta*&mdash;la superficie de la API para el componente. En otras palabras, la biblioteca generada contiene los tipos proyectados para el componente.

Después, igual que harías en un tipo de espacio de nombres de Windows, incluye un encabezado y crea el tipo proyectado a través de uno de sus constructores. El código de inicio del proyecto de tu aplicación registra la clase en tiempo de ejecución y el constructor del tipo proyectado llama a [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) para activar dicha clase desde el componente al que hace referencia.

```cppwinrt
#include <winrt/BankAccountWRC.h>

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    ...
};
```

Para obtener más detalles, el código y un tutorial acerca de consumir API en un componente de Windows Runtime implementado, consulta [Crear eventos en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-the-api-is-implemented-in-the-consuming-project"></a>Si la API se implementa en el proyecto de consumo
Un tipo que se consume desde la interfaz de usuario de XAML debe ser una clase en tiempo de ejecución, aunque se encuentre en el mismo proyecto que el XAML.

Para este escenario, genera un tipo proyectado desde los metadatos de Windows Runtime de la clase en tiempo de ejecución (`.winmd`). Una vez más, incluye un encabezado, pero esta vez construye el tipo proyectado mediante su constructor **std::nullptr_t**. Dicho constructor no realiza ninguna inicialización, por lo que debes asignar un valor a la instancia a través de la función auxiliar [**winrt::make**](/uwp/cpp-ref-for-winrt/make), pasando todos los argumentos de constructor necesarios. Las clases en tiempo de ejecución implementadas en el mismo proyecto que el código de consumo no es preciso registrarlas ni crear instancias de ellas a través de la activación de Windows Runtime/COM.

Necesitarás un proyecto de **Aplicación vacía (C++/WinRT)** para este código de ejemplo.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
    ...
    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
        ...
    };
}
...
// MainPage.cpp
...
#include "BookstoreViewModel.h"

MainPage::MainPage()
{
    m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

Para más detalles, el código y un tutorial acerca de cómo consumir una clase en tiempo de ejecución implementada en el proyecto de consumo, consulta [Controles XAML; enlazar a una propiedad de C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Creación de instancias y devolución de tipos proyectados e interfaces
Aquí se muestra un ejemplo del aspecto que podrían tener los tipos e interfaces proyectados en tu proyecto de consumo. Recuerda que los tipos proyectado (como el de este ejemplo) se generan mediante herramientas, no los puedes crear tú.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** es un tipo proyectado; las interfaces proyectadas incluyen **IMyRuntimeClass**, **IStringable**, y **IClosable** . En este tema se muestran las distintas formas de crear instancias de un tipo proyectado. Aquí tienes un recordatorio y un resumen, y se usa **MyRuntimeClass** como ejemplo.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- Puedes acceder a los miembros de todas las interfaces de un tipo proyectado.
- Puedes devolver un tipo proyectado a un autor de llamada.
- Los tipos e interfaces proyectados derivan de [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Por consiguiente, puedes llamar a [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) en un tipo o interfaz proyectados para consultar otras interfaces proyectadas, que también puedes usar o devolver al autor de la llamada. La función de miembro **as** funciona como [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)).

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>Generadores de activación
La forma más práctica y directa de crear un objeto de C++/WinRT es la siguiente.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

Sin embargo, puede haber ocasiones en que quieras crear el generador de activación tú mismo y, después, crear objetos a partir de él cuando mejor te venga. Estos son algunos ejemplos, que muestran cómo hacerlo mediante la plantilla de función [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory).

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
auto factory = winrt::get_activation_factory<CurrencyFormatter, ICurrencyFormatterFactory>();
CurrencyFormatter currency = factory.CreateCurrencyFormatterCode(L"USD");
```

```cppwinrt
using namespace winrt::Windows::Foundation;
...
auto factory = winrt::get_activation_factory<Uri, IUriRuntimeClassFactory>();
Uri account = factory.CreateUri(L"http://www.contoso.com");
```

Las clases de los dos ejemplos anteriores son tipos de un espacio de nombres de Windows. En el siguiente ejemplo, **BankAccountWRC::BankAccount** es un tipo personalizado implementado en un componente de Windows Runtime.

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="membertype-ambiguities"></a>Ambigüedades entre miembros y tipos

Cuando una función de miembro tiene el mismo nombre que un tipo, hay una ambigüedad. Las reglas de búsqueda de nombres no completos de C++ en funciones miembro hacen que busque en la clase antes de buscar en los espacios de nombres. La regla *substitution failure is not an error* (SFINAE, o el fallo de sustitución no es un error) no se aplica (se aplica durante la resolución de sobrecarga de las plantillas de función). Por tanto, si el nombre dentro de la clase no tiene sentido, el compilador no sigue buscando una coincidencia mejor &mdash;simplemente informa de un error—.

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // This doesn't compile. You get the error
        // "'winrt::Windows::Foundation::IUnknown::as':
        // no matching overloaded function found".
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Style>() };
    }
}
```

Además, el compilador considera que estás pasando [**FrameworkElement.Style()** ](/uwp/api/windows.ui.xaml.frameworkelement.style) (que, en C++/WinRT, es una función miembro) como parámetro de plantilla a [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function). La solución consiste en forzar que el nombre `Style` se interprete como el tipo [**Windows::UI::XAML::Style**](/uwp/api/windows.ui.xaml.style).

```cppwinrt
struct MyPage : Page
{
    void DoWork()
    {
        // One option is to fully-qualify it.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Windows::UI::Xaml::Style>() };

        // Another is to force it to be interpreted as a struct name.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<struct Style>() };

        // If you have "using namespace Windows::UI;", then this is sufficient.
        auto style{ Application::Current().Resources().
            Lookup(L"MyStyle").as<Xaml::Style>() };

        // Or you can force it to be resolved in the global namespace (into which
        // you imported the Windows::UI::Xaml namespace when you did
        // "using namespace Windows::UI::Xaml;".
        auto style = Application::Current().Resources().
            Lookup(L"MyStyle").as<::Style>();
    }
}
```

La búsqueda de nombres no completos tiene una excepción especial en caso de que el nombre vaya seguido de `::`, en cuyo caso omite las funciones, las variables y los valores de enumeración. Esto te permite hacer cosas como la siguiente.

```cppwinrt
struct MyPage : Page
{
    void DoSomething()
    {
        Visibility(Visibility::Collapsed); // No ambiguity here (special exception).
    }
}
```

La llamada a `Visibility()` se resuelve en el nombre de la función miembro [**UIElement.Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility). Pero el parámetro `Visibility::Collapsed` sigue a la palabra `Visibility` con `::`, por lo que se omite el nombre del método, y el compilador encuentra la clase enum.

## <a name="important-apis"></a>API importantes
* [Interfaz de QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Función RoActivateInstance](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance)
* [Clase Windows::Foundation::Uri](/uwp/api/windows.foundation.uri)
* [Plantilla de función winrt::get_activation_factory](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [Plantilla de función winrt::make ](/uwp/cpp-ref-for-winrt/make)
* [Estructura winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Temas relacionados
* [Crear eventos en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md)
* [Introducción a C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Controles de XAML; enlazar a una propiedad de C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
