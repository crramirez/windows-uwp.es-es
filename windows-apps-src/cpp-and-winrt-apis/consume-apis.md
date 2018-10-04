---
author: stevewhims
description: Este tema muestra cómo usar API C++/WinRT, ya se hayan implementado por Windows, por un proveedor de componentes de terceros o por ti mismo.
title: Consumir API con C++/WinRT
ms.author: stwhi
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: wndows 10, uwp, estándar, c++, cpp, winrt, proyectado, proyección, implementación, implementar, clase en tiempo de ejecución, activación
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cd05f974bf9193e84919a5e679ef996746d7e
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4357305"
---
# <a name="consume-apis-with-cwinrt"></a>Consumir API con C++/WinRT

En este tema muestra cómo usar [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) API, ya formen parte de Windows, implementada por un proveedor de componentes de terceros o por TI mismo.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Si la API está en un espacio de nombres de Windows
Este es el caso más común en el que consumirás una API de Windows Runtime. Para cada tipo en un espacio de nombres de Windows definido en los metadatos, C++/WinRT define un equivalente de C++ descriptivo (denominado el *tipo proyectado*). Un tipo proyectado tiene el mismo nombre totalmente cualificado que el tipo de Windows, pero se coloca en el espacio de nombres **winrt** de C++ mediante la sintaxis de C++. Por ejemplo, [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri) se proyecta en C++/WinRT como **winrt::Windows::Foundation::Uri**.

Este es un ejemplo de código sencillo.

```cppwinrt
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

El encabezado incluido `winrt/Windows.Foundation.h` forma parte del SDK, que se encuentra dentro de la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Los encabezados de esta carpeta contienen tipos de espacios de nombres de Windows proyectados en C++/WinRT. En este ejemplo, `winrt/Windows.Foundation.h` contiene **winrt::Windows::Foundation::Uri**, que es el tipo proyectado para la clase en tiempo de ejecución [**Windows::Foundation::Uri**](/uwp/api/windows.foundation.uri).

> [!TIP]
> Siempre que quieras usar un tipo desde un espacio de nombres de Windows, incluye el encabezado C++/WinRT correspondiente a dicho espacio de nombres. Las directivas `using namespace` son opcionales, pero adecuadas.

En el ejemplo de código anterior, tras inicializar C++/WinRT, apilamos/asignamos un valor del tipo proyectado **winrt::Windows::Foundation::Uri** a través de uno de sus constructores públicamente documentados ([**Uri(String)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_), en este ejemplo). Para este ejemplo, el caso de uso más común, es normalmente todo lo que tienes que hacer. Una vez que tengas un valor del tipo proyectado de C++/WinRT, puedes tratarlo como si fuera una instancia de Windows Runtime real, ya que tiene los mismos miembros.

De hecho, dicho valor proyectado es un proxy; esencialmente es solo un puntero inteligente a un objeto de respaldo. Los constructores del valor proyectado llaman a [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) para crear una instancia de la clase de Windows Runtime de respaldo (**Windows.Foundation.Uri**, en este caso) y almacenan dicha interfaz predeterminada del objeto dentro del nuevo valor proyectado. Como se ilustra a continuación, las llamadas a los miembros del valor proyectado realmente delegan, a través del puntero inteligente, al objeto de respaldo, que es donde se producen los cambios de estado.

![El tipo proyectado de Windows::Foundation::Uri](images/uri.png)

Cuando el valor de `contosoUri` queda fuera de ámbito, se destruye y libera su referencia a la interfaz predeterminada. Si esa referencia es la última referencia al objeto de respaldo **Windows.Foundation.Uri** de Windows Runtime, el objeto de respaldo también se destruye.

> [!TIP]
> Un *tipo proyectado* es un contenedor a través de una clase en tiempo de ejecución para fines de consumo de sus API. Una *interfaz proyectada* es un contenedor sobre una interfaz de Windows Runtime.

## <a name="cwinrt-projection-headers"></a>Encabezados de proyección de C++/WinRT
Para consumir las API de espacio de nombres de Windows desde C++/WinRT, los encabezados se incluyen desde la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Es común que un tipo en un espacio de nombres subordinado haga referencia a tipos en su espacio de nombres principal inmediato. Por lo tanto, cada encabezado de proyección de C++/WinRT incluye su archivo de encabezado de espacio de nombres principal; por lo que no *necesitas* incluirlo de manera explícita. Aunque, si lo haces, no habrá ningún error.

Por ejemplo, para el espacio de nombres [**Windows::Security::Cryptography::Certificates**](/uwp/api/windows.security.cryptography.certificates), las definiciones de tipo de C++/WinRT equivalentes residen en `winrt/Windows.Security.Cryptography.Certificates.h`. Los tipos en **Windows::Security::Cryptography::Certificates** requieren tipos en el espacio de nombres **Windows::Security::Cryptography** principal; y los tipos de dicho espacio de nombres podrían requerir tipos en su propio **Windows::Security** principal.

Por lo tanto, si incluyes `winrt/Windows.Security.Cryptography.Certificates.h`, dicho archivo a su vez incluye `winrt/Windows.Security.Cryptography.h`; y `winrt/Windows.Security.Cryptography.h` incluye `winrt/Windows.Security.h`. Que es donde se detiene el seguimiento, ya que no hay ningún `winrt/Windows.h`. Este proceso de inclusión transitivo se detiene en el espacio de nombres de segundo nivel.

Este proceso transitivamente incluye los archivos de encabezado que proporcionan las *declaraciones* e *implementaciones* necesarias para las clases definidas en espacios de nombres principales.

Un miembro de un tipo en un espacio de nombres puede hacer referencia a uno o más tipos en otros espacios de nombres no relacionados. Para que el compilador compile correctamente estas definiciones de miembros, el compilador debe ver las declaraciones de tipo de la clausura de todos estos tipos. Por lo tanto, cada encabezado de proyección de C++/WinRT incluye los encabezados de espacio de nombres que necesita para *declarar* los tipos dependientes. A diferencia de los espacios de nombres principales, este proceso *no* incorpora las *implementaciones* para tipos de referencia.

> [!IMPORTANT]
> Cuando quieras *usar* realmente un tipo (crear una instancia, llamar a métodos, etc.) declarado en un espacio de nombres no relacionado, debes incluir el archivo de encabezado de espacio de nombres apropiado de dicho tipo. Solo se incluyen automáticamente *declaraciones*, no *implementaciones*.

Por ejemplo, si solo incluyes `winrt/Windows.Security.Cryptography.Certificates.h`, eso hace que las declaraciones se incorporen desde estos espacios de nombres (y así sucesivamente, de forma transitiva).

- Windows.Foundation
- Windows.Foundation.Collections
- Windows.Networking
- Windows.Storage.Streams
- Windows.Security.Cryptography

En otras palabras, algunas API se declaran más adelante en un encabezado que hayas incluido. Pero sus definiciones están en un encabezado que aún no has incluido. Por lo tanto, si, a continuación, llamas a [**Windows::Foundation::Uri::RawUri**](/uwp/api/windows.foundation.uri.rawuri), recibirás un error de vinculador que indica que el miembro no está definido. La solución es explícitamente `#include <winrt/Windows.Foundation.h>`. En general, cuando veas un error del vinculador como este, incluye el encabezado asignado para el espacio de nombres de la API y vuelve a compilar.

## <a name="accessing-members-via-the-object-via-an-interface-or-via-the-abi"></a>Acceso a los miembros a través del objeto, a través de una interfaz o a través de la ABI
Con la proyección de C++/WinRT, la representación en tiempo de ejecución de una clase de Windows Runtime no es más que las interfaces ABI subyacentes. Sin embargo, para tu comodidad, puedes codificar frente a las clases de la manera que el autor pretendía. Por ejemplo, puedes llamar al método **ToString** de un [**Uri**](/uwp/api/windows.foundation.uri) como si fuese un método de la clase (de hecho, en modo no visible, es un método en la otra interfaz **IStringable**).

```cppwinrt
Uri contosoUri{ L"http://www.contoso.com" };
WINRT_ASSERT(contosoUri.ToString() == L"http://www.contoso.com/"); // QueryInterface is called at this point.
```

Esta conveniencia se logra a través de una consulta de la interfaz adecuada. Pero siempre tienes el control. Puedes optar por sacrificar un poco de esa conveniencia por un poco de rendimiento recuperando la interfaz IStringable tú mismo y usándola directamente. En el siguiente ejemplo de código, se obtiene un puntero de interfaz IStringable real en tiempo de ejecución (a través de una consulta puntual). Después de eso, la llamada a **ToString** es directa y evita cualquier otra llamada a **QueryInterface **.

```cppwinrt
IStringable stringable = contosoUri; // One-off QueryInterface.
WINRT_ASSERT(stringable.ToString() == L"http://www.contoso.com/");
```

Puedes elegir esta técnica si sabes que vas a llamar a varios métodos en la misma interfaz.

Por cierto, si deseas obtener acceder a miembros en el nivel de la ABI, puedes hacerlo. El ejemplo de código siguiente muestra cómo y hay más detalles y ejemplos de código en [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md).

```cppwinrt
int port = contosoUri.Port(); // Access the Port "property" accessor via C++/WinRT.

winrt::com_ptr<ABI::Windows::Foundation::IUriRuntimeClass> abiUri = contosoUri.as<ABI::Windows::Foundation::IUriRuntimeClass>();
HRESULT hr = abiUri->get_Port(&port); // Access the get_Port ABI function.
```

## <a name="delayed-initialization"></a>Inicialización demorada
Incluso el constructor predeterminado de un tipo proyectado hace que se cree un objeto de respaldo de Windows Runtime. Si quieres crear una variable de un tipo proyectado sin ella y construir en su lugar un objeto de Windows Runtime (de modo que puedas retrasar ese trabajo hasta más adelante), puedes hacerlo. Declara la variable o el campo usando el constructor especial `nullptr_t` de C++/WinRT del tipo proyectado.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>
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

Todos los constructores del tipo proyectado *excepto* el constructor `nullptr_t` hacen que se cree un objeto de respaldo de Windows Runtime. El constructor `nullptr_t` no es básicamente ninguna operación. Espera que el objeto proyectado se inicialice en un momento posterior. Por tanto, tenga o no un constructor predeterminado una clase en tiempo de ejecución, puedes usar esta técnica para la inicialización retrasada eficiente.

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Si la API se implementa en un componente de Windows Runtime
Esta sección se aplica si creaste el componente tú mismo o si procedió de un proveedor.

> [!NOTE]
> Para obtener información sobre la instalación y uso de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBuild de C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

En tu proyecto de aplicación, haz referencia al archivo de metadatos de Windows Runtime del componente de Windows Runtime (`.winmd`) y compila. Durante la compilación, la herramienta `cppwinrt.exe` genera una biblioteca de C++ estándar que describe completamente&mdash;o *proyecta*&mdash;la superficie de la API para el componente. En otras palabras, la biblioteca generada contiene los tipos proyectados para el componente.

Después, al igual que con un tipo de espacio de nombres de Windows, incluye un encabezado y construye el tipo proyectado a través de uno de sus constructores. El código de inicio del proyecto de tu aplicación registra la clase en tiempo de ejecución y el constructor del tipo proyectado llama a [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) para activar la clase en tiempo de ejecución del componente al que hace referencia.

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
Un tipo que se consume desde la interfaz de usuario XAML debe ser una clase en tiempo de ejecución, incluso si se encuentra en el mismo proyecto que el XAML.

Para este escenario, generas un tipo proyectado desde los metadatos de Windows Runtime de la clase en tiempo de ejecución (`.winmd`). De nuevo, incluyes un encabezado, pero esta vez construyes el tipo proyectado a través de su constructor `nullptr`. Dicho constructor no realiza ninguna inicialización, por lo que deberás asignar un valor a la instancia a través de la función auxiliar [**winrt::make**](/uwp/cpp-ref-for-winrt/make), pasando cualquier argumento de constructor necesario. Una clase en tiempo de ejecución implementado en el mismo proyecto que el código de consumo no necesita registrarse ni crear una instancia a través de la activación de Windows Runtime/COM.

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
    m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
    ...
}
```

Para obtener más detalles, el código y un tutorial sobre cómo consumir una clase en tiempo de ejecución implementada en el proyecto, consulta [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

## <a name="instantiating-and-returning-projected-types-and-interfaces"></a>Crear instancias y devolver tipos e interfaces proyectados
Aquí se muestra un ejemplo del aspecto que podrían tener los tipos e interfaces proyectados en tu proyecto de consumo.

```cppwinrt
struct MyRuntimeClass : MyProject::IMyRuntimeClass, impl::require<MyRuntimeClass,
    Windows::Foundation::IStringable, Windows::Foundation::IClosable>
```

**MyRuntimeClass** es un tipo proyectado; las interfaces proyectadas incluyen **IMyRuntimeClass**, **IStringable**, y **IClosable **. En este tema se muestra las distintas formas en que puedes crear instancias de un tipo proyectado. Aquí tienes un recordatorio y un resumen con **MyRuntimeClass** como ejemplo.

```cppwinrt
// The runtime class is implemented in another compilation unit (it's either a Windows API,
// or it's implemented in a second- or third-party component).
MyProject::MyRuntimeClass myrc1;

// The runtime class is implemented in the same compilation unit.
MyProject::MyRuntimeClass myrc2{ nullptr };
myrc2 = winrt::make<MyProject::implementation::MyRuntimeClass>();
```

- Puedes acceder a los miembros de todas las interfaces de un tipo proyectado.
- Puede devolver un tipo proyectado a un autor de llamada.
- Los tipos e interfaces proyectados derivan desde [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Por lo tanto, puedes llamar a [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) en un tipo o interfaz proyectado para consultar otras interfaces proyectadas, que también puedes usar o devolver al autor de llamada. La función de miembro **as** funciona como [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521).

```cppwinrt
void f(MyProject::MyRuntimeClass const& myrc)
{
    myrc.ToString();
    myrc.Close();
    IClosable iclosable = myrc.as<IClosable>();
    iclosable.Close();
}
```

## <a name="activation-factories"></a>Fábricas de activación
La forma conveniente y directa de crear un objeto de C++/WinRT es la siguiente.

```cppwinrt
using namespace winrt::Windows::Globalization::NumberFormatting;
...
CurrencyFormatter currency{ L"USD" };
```

Pero puede haber ocasiones en que quieras crear la fábrica de activaciones tú mismo y, a continuación, crear objetos de ella a tu conveniencia. Estos son algunos ejemplos, que muestran cómo, utilizando la plantilla de función [**winrt::get_activation_factory**](/uwp/cpp-ref-for-winrt/get-activation-factory).

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

Las clases en los dos ejemplos anteriores son tipos de un espacio de nombres de Windows. En el siguiente ejemplo, **BankAccountWRC::BankAccount** es un tipo personalizado implementado en un componente de Windows Runtime.

```cppwinrt
auto factory = winrt::get_activation_factory<BankAccountWRC::BankAccount>();
BankAccountWRC::BankAccount account = factory.ActivateInstance<BankAccountWRC::BankAccount>();
```

## <a name="important-apis"></a>API importantes
* [Interfaz de QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Función RoActivateInstance](https://msdn.microsoft.com/library/br224646)
* [Clase Windows::Foundation::URI](/uwp/api/windows.foundation.uri)
* [Plantilla de función winrt::get_activation_factory](/uwp/cpp-ref-for-winrt/get-activation-factory)
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)
* [estructura winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Artículos relacionados
* [Crear eventos en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md)
* [Introducción a C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
