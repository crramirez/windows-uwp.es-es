---
author: stevewhims
description: Este tema muestra cómo usar API C++/WinRT, ya se hayan implementado por Windows, por un proveedor de componentes de terceros o por ti mismo.
title: Consumir API con C++/WinRT
ms.author: stwhi
ms.date: 04/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: wndows 10, uwp, estándar, c++, cpp, winrt, proyectado, proyección, implementación, implementar, clase en tiempo de ejecución, activación
ms.localizationpriority: medium
ms.openlocfilehash: e777aca8f1d9f3892f67b10c785c056b14c8070c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832249"
---
# <a name="consume-apis-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Consumir API con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Este tema muestra cómo usar API C++/WinRT, ya formen parte de Windows, se hayan implementado por un proveedor de componentes de terceros o por ti mismo.

## <a name="if-the-api-is-in-a-windows-namespace"></a>Si la API está en un espacio de nombres de Windows
Este es el caso más común en el que consumirás una API de Windows Runtime. Este es un ejemplo de código sencillo.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();
    Uri contosoUri{ L"http://www.contoso.com" };
}
```

El encabezado incluido `winrt/Windows.Foundation.h` forma parte del SDK, que se encuentra dentro de la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Los encabezados de esta carpeta contienen las API de Windows proyectadas en C++/WinRT. Siempre que quieras usar un tipo desde un espacio de nombres de Windows, incluye el encabezado de proyección C++/WinRT correspondiente a dicho espacio de nombres. Las directivas `using namespace` son opcionales, pero adecuadas.

En este ejemplo, `winrt/Windows.Foundation.h` contiene el tipo proyectado para la clase en tiempo de ejecución [**Windows::Foundation::Uri **](/uwp/api/windows.foundation.uri).

> [!TIP]
> Un *tipo proyectado* es un contenedor a través de una clase en tiempo de ejecución para fines de consumo de sus API. Una *interfaz proyectada* es un contenedor sobre una interfaz de Windows Runtime.

En el ejemplo de código anterior, tras inicializar C++/WinRT, construimos el tipo proyectado **Uri** a través de uno de sus constructores públicamente documentados ([**Uri(String)**](/uwp/api/windows.foundation.uri#Windows_Foundation_Uri__ctor_System_String_), en este ejemplo). Para este ejemplo, el caso de uso más común, es todo lo que tienes que hacer.

## <a name="if-the-api-is-implemented-in-a-windows-runtime-component"></a>Si la API se implementa en un componente de Windows Runtime
Esta sección se aplica si creaste el componente tú mismo o si procedió de un proveedor.

> [!NOTE]
> Para obtener información sobre la actual disponibilidad de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBBuild de C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

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

## <a name="important-apis"></a>API importantes
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)
* [winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Artículos relacionados
* [Crear eventos en C++/WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component)
* [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
