---
author: stevewhims
description: Este tema muestra cómo crear API C++/WinRT mediante el uso de la estructura base **winrt::implements**, directa o indirectamente.
title: Crear API con C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyectado, proyección, implementación, implementar, clase en tiempo de ejecución, activación
ms.localizationpriority: medium
ms.openlocfilehash: 21670e0908a212341d401b4cbca314a9242b26a2
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5683984"
---
# <a name="author-apis-with-cwinrt"></a>Crear API con C++/WinRT

En este tema muestra cómo crear [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) API mediante el uso de la [**Implements**](/uwp/cpp-ref-for-winrt/implements) estructura, de base directa o indirectamente. Algunos sinónimos de *crear* en este contexto son *producir* o *implementar*. Este tema trata las siguientes situaciones de implementación de las API en un tipo C++/WinRT, en este orden.

- *No* vas a crear una clase de Windows Runtime (clase en tiempo de ejecución); tan solo quieres implementar una o más interfaces de Windows Runtime para el consumo local dentro de tu aplicación. Derivas directamente desde **winrt::implements** en este caso e implementas funciones.
- *Sí* que vas a crear una clase en tiempo de ejecución. Es posible que vayas a crear un componente para consumirse desde una aplicación. O puede que vayas a crear un tipo para consumir desde la interfaz de usuario XAML. En este caso, vas tanto a implementar como a consumir una clase en tiempo de ejecución en la misma unidad de compilación. En estos casos, permites que las herramientas generen clases para ti que derivan desde **winrt::implements**

En ambos casos, el tipo que implementa tus API C++/WinRT se denomina *tipo de implementación*.

> [!IMPORTANT]
> Es importante distinguir el concepto entre un tipo de implementación y un tipo proyectado. El tipo proyectado se describe en [Consumir API con C++/WinRT](consume-apis.md).

## <a name="if-youre-not-authoring-a-runtime-class"></a>Si *no* vas a crear una clase en tiempo de ejecución.
El escenario más sencillo es aquel en el que implementas una interfaz de Windows Runtime para consumo local. No necesitas una clase en tiempo de ejecución, solo una clase C++ normal. Por ejemplo, podrías estar escribiendo una aplicación basada en [**CoreApplication **](/uwp/api/windows.applicationmodel.core.coreapplication).

> [!NOTE]
> Para obtener información sobre la instalación y uso de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBuild de C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

En Visual Studio, **Visual C++** > **Windows Universal** > **Core App (C++ / WinRT)** plantilla de proyecto de muestra el modelo de **CoreApplication** . El patrón empieza pasando una implementación de [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) a [**CoreApplication: Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** usa la interfaz para crear la primera vista de la aplicación. En términos conceptuales, **IFrameworkViewSource** tiene este aspecto.

```cppwinrt
struct IFrameworkViewSource : IInspectable
{
    IFrameworkView CreateView();
};
```

Nuevamente en términos conceptuales, la implementación de **CoreApplication::Run** hace esto.

```cppwinrt
void Run(IFrameworkViewSource viewSource) const
{
    IFrameworkView view = viewSource.CreateView();
    ...
}
```

Así que tú, como desarrollador, implementas la interfaz **IFrameworkViewSource**. C++/WinRT tiene la plantilla de estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) para facilitar la implementación de una interfaz (o varias) sin tener que recurrir a la programación de estilo COM. Solo derivas tu tipo desde **implements**y luego implementas las funciones de la interfaz. A continuación se muestra cómo hacerlo.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource>
{
    IFrameworkView CreateView()
    {
        return ...
    }
}
...
```

Se encarga de **IFrameworkViewSource**. El siguiente paso es devolver un objeto que implemente la interfaz **IFrameworkView**. También puedes elegir implementar esta interfaz en **App**. El siguiente ejemplo de código representa una aplicación mínima que abrirá al menos una ventana en funcionamiento en el dispositivo de escritorio.

```cppwinrt
// App.cpp
...
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const &) {}

    void Load(hstring const&) {}

    void Run()
    {
        CoreWindow window = CoreWindow::GetForCurrentThread();
        window.Activate();

        CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const & window)
    {
        // Prepare app visuals here
    }

    void Uninitialize() {}
};
...
```

Dado que el tipo de tu **App** *es ***IFrameworkViewSource**, solo puedes pasar uno a **Run **.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App{});
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Si vas a crear una clase en tiempo de ejecución en un componente de Windows Runtime.
Si tu tipo está incluido en un componente de Windows Runtime para el consumo desde una aplicación, es necesario que sea una clase en tiempo de ejecución.

> [!TIP]
> Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo de lenguaje de definición de interfaz (IDL) (.idl), para optimizar el rendimiento de la compilación cuando edita un archivo IDL y para la correspondencia lógica de un archivo IDL con sus archivos de código fuente generados. Visual Studio combina todos los archivos `.winmd` resultantes en un solo archivo con el mismo nombre que el espacio de nombres de raíz. Ese archivo `.winmd` final será al que harán referencia los consumidores de tu componente.

Aquí tienes un ejemplo.

```idl
// MyRuntimeClass.idl
namespace MyProject
{
    runtimeclass MyRuntimeClass
    {
        // Declaring a constructor (or constructors) in the IDL causes the runtime class to be
        // activatable from outside the compilation unit.
        MyRuntimeClass();
        String Name;
    }
}
```

Este IDL declara una clase de Windows Runtime (tiempo de ejecución) Una clase en tiempo de ejecución es un tipo que puede activarse y consumirse a través de interfaces de COM modernas, normalmente a través de límites de archivo ejecutables. Cuando agregues un archivo IDL a tu proyecto y compiles, la cadena de herramientas C++/WinRT (`midl.exe` y `cppwinrt.exe`) generará un tipo de implementación para ti. En el ejemplo anterior de IDL, el tipo de implementación es un código auxiliar de estructura C++ llamado **winrt::MyProject::implementation::MyRuntimeClass** en archivos de código fuente llamados `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` y `MyRuntimeClass.cpp`.

El tipo de implementación tiene este aspecto.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        hstring Name();
        void Name(hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Ten en cuenta el patrón de polimorfismo F enlazado que se está usando (**MyRuntimeClass** se usa como un argumento de plantilla para su base, **MyRuntimeClassT**). Esto también se conoce como el patrón de plantilla curiosamente periódico (CRTP). Si sigues la cadena de herencia hacia arriba encontrarás **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

Por lo tanto, en este contexto, en la raíz de la jerarquía de herencia, vuelve a ser la plantilla de la estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

Para obtener más detalles, el código y un tutorial acerca de la creación de API en un componente de Windows Runtime, consulta [Crear eventos en C++/ WinRT](author-events.md#create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Si vas a crear una clase en tiempo de ejecución a la que se hará referencia en tu interfaz de usuario de XAML
Si tu interfaz de usuario de XAML hace referencia a tu tipo, entonces debe ser una clase en tiempo de ejecución, aunque esté en el mismo proyecto que el XAML. Aunque generalmente se activan en los límites de archivo ejecutables, una clase en tiempo de ejecución puede usarse en su lugar dentro de la unidad de compilación que la implementa.

En este caso, estás creando *y* consumiendo las API. El procedimiento para implementar tu clase en tiempo de ejecución es esencialmente el mismo que para un componente de Windows Runtime. Consulta la sección anterior&mdash;[si vas a crear una clase en tiempo de ejecución en un componente de Windows Runtime](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component) El único detalle que difiere es que, desde el archivo IDL, la cadena de herramientas C++/WinRT no solo genera un tipo de implementación sino también un tipo proyectado. Es importante señalar que decir solo "**MyRuntimeClass**" en este contexto puede ser ambiguo ya que hay varias entidades con este nombre de diferentes tipos.

- **MyRuntimeClass** es el nombre de una clase en tiempo de ejecución. Pero esto es realmente una abstracción: declarada en IDL e implementada en algún lenguaje de programación.
- **MyRuntimeClass** es el nombre de la estructura de C++ **winrt::MyProject::implementation::MyRuntimeClass**, lo cual es la implementación C++/WinRT de la clase en tiempo de ejecución. Como ya hemos visto, si hay proyectos independientes de implementación y consumo, esta estructura solo existe en el proyecto de implementación. Esto es *el tipo de implementación* o *la implementación *. Este tipo se genera (mediante la herramienta `cppwinrt.exe`) en los archivos `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` y `MyRuntimeClass.cpp`.
- **MyRuntimeClass** es el nombre del tipo proyectado en el formulario de la estructura de C++ **winrt::MyProject::MyRuntimeClass **. Si hay proyectos independientes de implementación y consumo, esta estructura solo existe en el proyecto de consumo. Esto es *el tipo proyectado* o *la proyección*. Este tipo se genera (mediante `cppwinrt.exe`) en el archivo `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h`.

![Tipo proyectado y tipo de implementación](images/myruntimeclass.png)

Estas son las partes del tipo proyectado que son relevantes para este tema.

```cppwinrt
// MyProject.2.h
...
namespace winrt::MyProject
{
    struct MyRuntimeClass : MyProject::IMyRuntimeClass
    {
        MyRuntimeClass(std::nullptr_t) noexcept {}
        MyRuntimeClass();
    };
}
```

Para ver un tutorial de ejemplo de la implementación de la interfaz **INotifyPropertyChanged** en una clase en tiempo de ejecución, consulta [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md).

El procedimiento para consumir tu clase en tiempo de ejecución en este contexto se describe en [Consumir API con C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).

## <a name="runtime-class-constructors"></a>Constructores de clases en tiempo de ejecución
Estos son algunos de los puntos que retirar de las listas que hemos visto anteriormente.

- Cada constructor que declaras en el archivo IDL hace que se genere un constructor tanto en tu tipo de implementación como en tu tipo proyectado. Los constructores declarados en IDL se usan para consumir la clase en tiempo de ejecución desde una unidad de compilación *diferente*.
- Ya tengas constructores declarados en IDL o no, se genera una sobrecarga de constructores que toma `nullptr_t` en tu tipo proyectado. Llamar al constructor `nullptr_t` es *el primero de dos pasos* en el consumo de la clase en tiempo de ejecución de *la misma* unidad de compilación. Para obtener más información y un ejemplo de código, consulta [Consumir API con C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Si vas a consumir la clase en tiempo de ejecución de *la misma* unidad de compilación, después también puedes implementar los constructores que no sean predeterminados en el tipo de implementación (que, recuerda, está en `MyRuntimeClass.h`).

> [!NOTE]
> Si esperas que tu clase en tiempo de ejecución se consuma desde una unidad de compilación diferente (lo cual es común), incluye constructores en el archivo IDL (al menos un constructor predeterminado). Con esto también obtendrás una implementación de fábrica junto con tu tipo de implementación.
> 
> Si quieres crear y consumir tu clase en tiempo de ejecución solo dentro de la misma unidad de compilación, no declares todos los constructores en tu archivo IDL. No necesitas una implementación de fábrica y no se generará ninguna. Se eliminará al constructor predeterminado de tu tipo de implementación, pero en cambio podrás editarlo y predeterminarlo fácilmente.
> 
> Si quieres crear y consumir tu clase en tiempo de ejecución solo dentro de la misma unidad de compilación y necesitas parámetros del constructor, crea los constructores que necesites directamente en tu tipo de implementación.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>Crear instancias y devolver tipos de implementación e interfaces
En esta sección analizaremos un ejemplo de un tipo de implementación llamado **MyType**, que implementa las interfaces [**IStringable**](/uwp/api/windows.foundation.istringable) y [**IClosable**](/uwp/api/windows.foundation.iclosable).

Puedes derivar **MyType** directamente desde [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) (no es una clase en tiempo de ejecución).

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable, IClosable>
{
    winrt::hstring ToString(){ ... }
    void Close(){}
};
```

O bien puedes generarlo desde IDL (es una clase en tiempo de ejecución).

```idl
// MyType.idl
namespace MyProject
{
    runtimeclass MyType: Windows.Foundation.IStringable, Windows.Foundation.IClosable
    {
        MyType();
    }    
}
```

Para ir desde **MyType** a un objeto **IStringable** o **IClosable** que puedas usar o devolver como parte de tu proyección, puedes llamar a la plantilla de función [**winrt::make**](/uwp/cpp-ref-for-winrt/make). **hacer que** devuelve la interfaz predeterminada del tipo de implementación.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> Sin embargo, si vas a hacer referencia a tu tipo desde tu interfaz de usuario de XAML, entonces habrá un tipo de implementación y un tipo proyectado en el mismo proyecto. En ese caso, **hacer que** devuelve una instancia del tipo proyectado. Para ver un ejemplo de código de este escenario, consulta [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

Solo podemos usar `istringable` (en el ejemplo de código anterior) para llamar a los miembros de la interfaz **IStringable**. Pero una interfaz C++/WinRT (que es una interfaz proyectada) se deriva desde [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Por lo tanto, puedes llamar a [**IUnknown:: As**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (o [**IUnknown:: Try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) en ella para consultar para otros tipos proyectados o interfaces, que también pueden usar o devolver.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Si necesitas obtener acceso a todos los miembros de la implementación y más adelante devolver una interfaz al autor de la llamada, usa la plantilla de función [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** devuelve un [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que ajusta el tipo de implementación. Puedes tener acceso a los miembros de todas sus interfaces (con el operador de flecha), puedes devolverlo al autor de la llamada tal cual o puedes llamar a **as** en él y devolver el objeto de la interfaz resultante al autor de la llamada.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

La clase **MyType** no forma parte de la proyección, es la implementación. Pero de este modo puedes llamar a sus métodos de implementación directamente, sin la sobrecarga de una llamada de función virtual. En el ejemplo anterior, aunque **MyType::ToString** usa la misma firma que el método proyectado en **IStringable**, llamamos al método no virtual directamente, sin cruzar la interfaz binaria de la aplicación (ABI) El **com_ptr** simplemente tiene un puntero en la estructura **MyType**, por lo que también puedes acceder a los demás detalles internos de **MyType** a través de la variable `myimpl` y el operador de flecha.

En el caso donde tienes un objeto de interfaz y descubres que es una interfaz en la implementación, a continuación, puedes obtener vuelve a la implementación mediante la plantilla de función [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self) . De nuevo, es una técnica que evita llamadas a funciones virtuales y te permite acceder directamente a la implementación.

> [!NOTE]
> Si no has instalado Windows SDK versión 10.0.17763.0 (Windows 10, versión 1809) o una versión posterior, a continuación, tienes que llamar [**winrt:: from_abi**](/uwp/cpp-ref-for-winrt/from-abi) en lugar de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

A continuación te mostramos un ejemplo. Hay otro ejemplo en [implementa la clase de control personalizado **BgLabelControl** ](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class).

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Pero solo el objeto de interfaz original guarda una referencia. Si *tú*quieres guardarlo, puedes llamar a [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function).

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

El propio tipo de implementación no deriva desde **winrt::Windows::Foundation::IUnknown**, de modo que no tiene ninguna función **as**. Aun así, puedes crear una instancia y acceder a los miembros de todas las interfaces. Pero si haces esto, no devuelvas la instancia del tipo de implementación sin procesar al autor de la llamada. En su lugar, usa una de las técnicas que se muestran más arriba y devuelve una interfaz proyectada o un **com_ptr**.

```cppwinrt
MyType myimpl;
myimpl.ToString();
myimpl.Close();
IClosable ic1 = myimpl.as<IClosable>(); // error
```

Si tienes una instancia de tu tipo de implementación y necesitas pasarla a una función que espera el correspondiente tipo proyectado, puedes hacerlo. Existe un operador de conversión en tu tipo de implementación (siempre que el tipo de implementación se ha generado por el `cppwinrt.exe` herramienta) que hace que esto sea posible.

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Derivar de un tipo que tiene un constructor no predeterminado
[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)**](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) es un ejemplo de un constructor no predeterminado. No hay ningún constructor predeterminado, por lo tanto, para construir un **ToggleButtonAutomationPeer**, necesitas pasar un *propietario*. Como consecuencia, si derivas desde **ToggleButtonAutomationPeer**, tienes que proporcionar un constructor que tome un *propietario* y que lo pase a la base. Veamos cómo se ve esto en la práctica.

```idl
// MySpecializedToggleButton.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButton :
        Windows.UI.Xaml.Controls.Primitives.ToggleButton
    {
        ...
    };
}
```

```idl
// MySpecializedToggleButtonAutomationPeer.idl
namespace MyNamespace
{
    runtimeclass MySpecializedToggleButtonAutomationPeer :
        Windows.UI.Xaml.Automation.Peers.ToggleButtonAutomationPeer
    {
        MySpecializedToggleButtonAutomationPeer(MySpecializedToggleButton owner);
    };
}
```

El constructor generado para tu tipo de implementación tiene este aspecto.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner)
{
    ...
}
...
```

Lo único que falta es pasar dicho parámetro del constructor a la clase base. ¿Recuerdas el patrón de polimorfismo F enlazado que hemos mencionado anteriormente? Una vez familiarizado con los detalles de este patrón usado por C++/WinRT, entenderás qué llama a tu clase base (o simplemente podrás buscar en el archivo de encabezado de tu clase de implementación). Esto trata cómo llamar al constructor de la clase base en este caso.

```cppwinrt
// MySpecializedToggleButtonAutomationPeer.cpp
...
MySpecializedToggleButtonAutomationPeer::MySpecializedToggleButtonAutomationPeer
    (MyNamespace::MySpecializedToggleButton const& owner) : 
    MySpecializedToggleButtonAutomationPeerT<MySpecializedToggleButtonAutomationPeer>(owner)
{
    ...
}
...
```

El constructor de clase base espera un **ToggleButton**. Y **MySpecializedToggleButton** *es un***ToggleButton**.

Hasta que no hayas realizado la modificación descrita anteriormente (pasar dicho parámetro del constructor a la clase base), el compilador marcará el constructor e indicará que no hay ningún constructor predeterminado adecuado disponible en un tipo llamado, en este caso, **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;**. En realidad es la clase base de la clase base de tu tipo de implementación.

## <a name="important-apis"></a>API importantes
* [Plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [función winrt::com_ptr::copy_from](/uwp/cpp-ref-for-winrt/com-ptr#comptrcopyfrom-function)
* [Plantilla de función winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)
* [plantilla de función winrt::get_self](/uwp/cpp-ref-for-winrt/get-self)
* [Plantilla de estructura winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Plantilla de función winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)
* [Función winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Artículos relacionados
* [Consumir API con C++/WinRT](consume-apis.md)
* [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage)
