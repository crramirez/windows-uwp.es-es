---
description: En este tema se muestra cómo crear las API de C++/WinRT mediante la estructura base **winrt::implements**, directa o indirectamente.
title: Crear API con C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, standard, estándar, c++, cpp, winrt, projected, proyectado, projection, proyección, implementation, implementación, implement, implementar, runtime class, clase en tiempo de ejecución, activation, activación
ms.localizationpriority: medium
ms.openlocfilehash: 64f605fc716970d2fd4ca534a0c31fb62baa34d4
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493670"
---
# <a name="author-apis-with-cwinrt"></a>Crear API con C++/WinRT

En este tema se muestra cómo crear las API de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) mediante la estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), directa o indirectamente. Algunos sinónimos de *crear* en este contexto son *producir* o *implementar*. En este tema se tratan las siguientes situaciones de implementación de las API en un tipo C++/WinRT, en este orden.

> [!NOTE]
> Este tema aborda el tema de los componentes de Windows Runtime, pero solo en el contexto de C++/WinRT. Si estás buscando contenido sobre los componentes de Windows Runtime que cubra todos los lenguajes de Windows Runtime, consulta [Componentes de Windows Runtime](/windows/uwp/winrt-components/).

- *No* vas a crear una clase de Windows Runtime (clase en tiempo de ejecución); tan solo quieres implementar una o más interfaces de Windows Runtime para el consumo local dentro de tu app. Derivas directamente desde **winrt::implements** en este caso e implementas funciones.
- *Sí* que vas a crear una clase en tiempo de ejecución. Es posible que vayas a crear un componente para consumirse desde una app. O puede que vayas a crear un tipo para consumir desde la interfaz de usuario XAML. En este caso, vas tanto a implementar como a consumir una clase en tiempo de ejecución en la misma unidad de compilación. En estos casos, permites que las herramientas generen clases para ti que derivan desde **winrt::implements**.

En ambos casos, el tipo que implementa tus API de C++/WinRT se denomina *tipo de implementación*.

> [!IMPORTANT]
> Es importante distinguir el concepto entre un tipo de implementación y un tipo proyectado. El tipo proyectado se describe en [Consumir API con C++/WinRT](consume-apis.md).

## <a name="if-youre-not-authoring-a-runtime-class"></a>Si *no* vas a crear una clase en tiempo de ejecución

El escenario más sencillo es aquel en el que implementas una interfaz de Windows Runtime para consumo local. No necesitas una clase en tiempo de ejecución, solo una clase C++ normal. Por ejemplo, podrías estar escribiendo una app basada en [**CoreApplication** ](/uwp/api/windows.applicationmodel.core.coreapplication).

> [!NOTE]
> Para más información sobre cómo instalar y usar la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

En Visual Studio, la plantilla de proyecto **Visual C++**  > **Windows Universal** > **Core App (C++/WinRT)** muestra el patrón **CoreApplication**. El patrón empieza pasando una implementación de [**Windows::ApplicationModel::Core::IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) a [**CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run).

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    IFrameworkViewSource source = ...
    CoreApplication::Run(source);
}
```

**CoreApplication** usa la interfaz para crear la primera vista de la app. En términos conceptuales, **IFrameworkViewSource** tiene este aspecto.

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

Así que tú, como desarrollador, implementas la interfaz **IFrameworkViewSource**. C++/WinRT tiene la plantilla de estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) para facilitar la implementación de una interfaz (o varias) sin tener que recurrir a la programación de estilo COM. Solo derivas tu tipo desde **implements** y luego implementas las funciones de la interfaz. A continuación se muestra cómo hacerlo.

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

**IFrameworkViewSource** está listo. El siguiente paso es devolver un objeto que implemente la interfaz **IFrameworkView**. También puedes elegir implementar esta interfaz en **App**. En el siguiente ejemplo de código se representa una app mínima que abrirá al menos una ventana en funcionamiento en el dispositivo de escritorio.

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

Dado que el tipo de **App** *es* **IFrameworkViewSource**, solo puedes pasar uno a **Run**.

```cppwinrt
using namespace Windows::ApplicationModel::Core;
int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="if-youre-authoring-a-runtime-class-in-a-windows-runtime-component"></a>Si vas a crear una clase en tiempo de ejecución en un componente de Windows Runtime

Si tu tipo está incluido en un componente de Windows Runtime para el consumo desde una aplicación, es necesario que sea una clase en tiempo de ejecución. Declaras una clase en tiempo de ejecución en un archivo de Lenguaje de definición de interfaz de Microsoft (IDL) (.idl) (consulta [Factorizar clases en tiempo de ejecución en archivos Midl [.idl]](#factoring-runtime-classes-into-midl-files-idl)).

Cada archivo IDL genera un archivo `.winmd`, y Visual Studio los combina todos en un único archivo con el mismo nombre que el espacio de nombres raíz. Ese archivo `.winmd` final será al que harán referencia los consumidores de tu componente.

A continuación se muestra un ejemplo de cómo declarar una clase en tiempo de ejecución en un archivo IDL.

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

Este IDL declara una clase de Windows Runtime (tiempo de ejecución). Una clase en tiempo de ejecución es un tipo que puede activarse y consumirse a través de interfaces COM modernas, normalmente a través de límites de archivo ejecutables. Cuando agregues un archivo IDL a tu proyecto y compiles, la cadena de herramientas C++/WinRT (`midl.exe` y `cppwinrt.exe`) generará un tipo de implementación para ti. Para ver un ejemplo del flujo de trabajo del archivo IDL en acción, consulta [XAML controls; bind to a C++/WinRT property](binding-property.md) (Enlazar los controles XAML a una propiedad de C++/WinRT).

En el ejemplo anterior de IDL, el tipo de implementación es un código auxiliar de estructura C++ llamado **winrt::MyProject::implementation::MyRuntimeClass** en archivos de código fuente llamados `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` y `MyRuntimeClass.cpp`.

El tipo de implementación tiene este aspecto.

```cppwinrt
// MyRuntimeClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyRuntimeClass : MyRuntimeClassT<MyRuntimeClass>
    {
        MyRuntimeClass() = default;

        winrt::hstring Name();
        void Name(winrt::hstring const& value);
    };
}

// winrt::MyProject::factory_implementation::MyRuntimeClass is here, too.
```

Ten en cuenta el patrón de polimorfismo F enlazado que se está usando (**MyRuntimeClass** se usa como un argumento de plantilla para su base, **MyRuntimeClassT**). Esto también se conoce como el patrón de plantilla curiosamente periódico (CRTP). Si sigues la cadena de herencia hacia arriba, encontrarás **MyRuntimeClass_base**.

```cppwinrt
template <typename D, typename... I>
struct MyRuntimeClass_base : implements<D, MyProject::IMyRuntimeClass, I...>
```

Por lo tanto, en este contexto, en la raíz de la jerarquía de herencia, vuelve a ser la plantilla de la estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements).

Para obtener más detalles, el código y un tutorial sobre la creación de API en un componente de Windows Runtime, consulte [Componentes de Windows Runtime con C++/WinRT](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt) y [Creación de eventos en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-events).

## <a name="if-youre-authoring-a-runtime-class-to-be-referenced-in-your-xaml-ui"></a>Si vas a crear una clase en tiempo de ejecución a la que se hará referencia en tu interfaz de usuario de XAML

Si tu interfaz de usuario de XAML hace referencia a tu tipo, entonces debe ser una clase en tiempo de ejecución, aunque esté en el mismo proyecto que el XAML. Aunque generalmente se activan en los límites de archivo ejecutables, una clase en tiempo de ejecución puede usarse en su lugar dentro de la unidad de compilación que la implementa.

En este caso, estás creando *y* consumiendo las API. El procedimiento para implementar tu clase en tiempo de ejecución es esencialmente el mismo que para un componente de Windows Runtime. Consulta la sección anterior &mdash;[Si vas a crear una clase en tiempo de ejecución en un componente de Windows Runtime](#if-youre-authoring-a-runtime-class-in-a-windows-runtime-component). El único detalle que difiere es que, desde el archivo IDL, la cadena de herramientas C++/WinRT genera no solo un tipo de implementación sino también un tipo proyectado. Es importante señalar que decir solo "**MyRuntimeClass**" en este contexto puede ser ambiguo, ya que hay varias entidades con este nombre de diferentes tipos.

- **MyRuntimeClass** es el nombre de una clase en tiempo de ejecución. Pero esto es realmente una abstracción: declarada en IDL e implementada en algún lenguaje de programación.
- **MyRuntimeClass** es el nombre de la estructura de C++ **winrt::MyProject::implementation::MyRuntimeClass**, lo cual es la implementación C++/WinRT de la clase en tiempo de ejecución. Como ya hemos visto, si hay proyectos independientes de implementación y consumo, esta estructura solo existe en el proyecto de implementación. Esto es *el tipo de implementación* o *la implementación*. Este tipo se genera (mediante la herramienta `cppwinrt.exe`) en los archivos `\MyProject\MyProject\Generated Files\sources\MyRuntimeClass.h` y `MyRuntimeClass.cpp`.
- **MyRuntimeClass** es el nombre del tipo proyectado en el formulario de la estructura de C++ **winrt::MyProject::MyRuntimeClass**. Si hay proyectos independientes de implementación y consumo, esta estructura solo existe en el proyecto de consumo. Esto es *el tipo proyectado* o *la proyección*. Este tipo se genera (mediante `cppwinrt.exe`) en el archivo `\MyProject\MyProject\Generated Files\winrt\impl\MyProject.2.h`.

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

Para ver un tutorial de ejemplo sobre la implementación de la interfaz **INotifyPropertyChanged** en una clase en tiempo de ejecución, consulta [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md).

El procedimiento para consumir tu clase en tiempo de ejecución en este contexto se describe en [Consumir API con C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).

## <a name="factoring-runtime-classes-into-midl-files-idl"></a>Factorizar clases en tiempo de ejecución en archivos Midl (.idl)

El proyecto de Visual Studio y las plantillas de elementos generan un archivo IDL independiente para cada clase en tiempo de ejecución. Esto da origen a una correspondencia lógica entre un archivo IDL y sus archivos de código fuente generados.

Sin embargo, si consolidas todas las clases en tiempo de ejecución del proyecto en un único archivo IDL, el tiempo de compilación podrá mejorar significativamente. Si, por otro lado, tienes dependencias `import` complejas (o circulares) entre ellas, puede que la consolidación sea en verdad necesaria. Además, es posible que te resulte más fácil crear y revisar las clases en tiempo de ejecución si están juntas.

## <a name="runtime-class-constructors"></a>Constructores de clases en tiempo de ejecución

Estos son algunos de los puntos que recordar de las listas que hemos visto anteriormente.

- Cada constructor que declaras en el archivo IDL hace que se genere un constructor tanto en tu tipo de implementación como en tu tipo proyectado. Los constructores declarados en IDL se usan para consumir la clase en tiempo de ejecución desde una unidad de compilación *diferente*.
- Aunque tengas constructores declarados en IDL o no, se genera una sobrecarga de constructores que crea **std::nullptr_t** en tu tipo proyectado. Llamar al constructor **std::nullptr_t** es el *primero de dos pasos* en el consumo de la clase en tiempo de ejecución de *la misma* unidad de compilación. Para obtener más información y un ejemplo de código, consulta [Consumir API con C++/WinRT](consume-apis.md#if-the-api-is-implemented-in-the-consuming-project).
- Si vas a consumir la clase en tiempo de ejecución de *la misma* unidad de compilación, después también puedes implementar los constructores que no sean predeterminados en el tipo de implementación (que, recuerda, está en `MyRuntimeClass.h`).

> [!NOTE]
> Si esperas que tu clase en tiempo de ejecución se consuma desde una unidad de compilación diferente (lo cual es común), incluye constructores en el archivo IDL (al menos un constructor predeterminado). Con esto también obtendrás una implementación de fábrica junto con tu tipo de implementación.
> 
> Si quieres crear y consumir tu clase en tiempo de ejecución solo dentro de la misma unidad de compilación, no declares todos los constructores en tu archivo IDL. No necesitas una implementación de fábrica y no se generará ninguna. Se eliminará al constructor predeterminado de tu tipo de implementación, pero en cambio podrás editarlo y establecerlo como predeterminado fácilmente.
> 
> Si quieres crear y consumir tu clase en tiempo de ejecución solo dentro de la misma unidad de compilación y necesitas parámetros del constructor, crea los constructores que necesites directamente en tu tipo de implementación.

## <a name="runtime-class-methods-properties-and-events"></a>Métodos, propiedades y eventos de clase de tiempo de ejecución

Hemos visto que el flujo de trabajo consiste en usar IDL para declarar la clase de tiempo de ejecución y sus miembros; una vez hecho esto, las herramientas generan prototipos e implementaciones de código auxiliar. En cuanto a los prototipos generados automáticamente para los miembros de la clase de tiempo de ejecución, *puedes* editarlos para que transmitan diferentes tipos además de los tipos que declaras en tu IDL. Ten en cuenta que solo puedes hacerlo mientras el tipo que declares en IDL se pueda reenviar al tipo que declares en la versión implementada.

A continuación se muestran algunos ejemplos.

- Puedes flexibilizar los tipos de parámetros. Por ejemplo, si en IDL tu método toma un elemento **SomeClass**, puedes cambiarlo a **Ispectable** en la implementación. Esto funciona porque cualquier elemento **SomeClass** puede reenviarse a **IIspectable** (si lo haces al revés, no funcionará).
- Puedes aceptar un parámetro que se pueda copiar en función del valor, en lugar de las referencias. Por ejemplo, cambia `SomeClass const&` a `SomeClass`. Esto es necesario cuando necesites evitar capturar una referencia en una corrutina (consulta[Pasar los parámetros](/windows/uwp/cpp-and-winrt-apis/concurrency#parameter-passing)).
- Puedes flexibilizar el valor devuelto. Por ejemplo, puedes cambiar **void** a [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget).

Los dos últimos son muy útiles al escribir un controlador de eventos asincrónicos.

## <a name="instantiating-and-returning-implementation-types-and-interfaces"></a>Crear instancias y devolver tipos de implementación e interfaces

En esta sección analizaremos un ejemplo de un tipo de implementación llamado **MyType**, que implementa las interfaces [**IStringable**](/uwp/api/windows.foundation.istringable) e [**IClosable**](/uwp/api/windows.foundation.iclosable).

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

No se puede asignar directamente el tipo de implementación.

```cppwinrt
MyType myimpl; // error C2259: 'MyType': cannot instantiate abstract class
```

Sin embargo, puedes pasar de un objeto **MyType** a otro **IStringable** o **IClosable** que puedas usar o devolver como parte de tu proyección. Para ello, puedes llamar a la plantilla de función [**winrt::make**](/uwp/cpp-ref-for-winrt/make). [**make**] devuelve la interfaz predeterminada del tipo de implementación.

```cppwinrt
IStringable istringable = winrt::make<MyType>();
```

> [!NOTE]
> Sin embargo, si vas a hacer referencia a tu tipo desde tu interfaz de usuario de XAML, habrá un tipo de implementación y un tipo proyectado en el mismo proyecto. En este caso, [**make**] devuelve una instancia del tipo proyectado. Para ver un ejemplo de código de este escenario, consulta [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage).

Solo podemos usar `istringable` (en el ejemplo de código anterior) para llamar a los miembros de la interfaz **IStringable**. Pero una interfaz C++/WinRT (que es una interfaz proyectada) se deriva desde [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown). Por lo tanto, puedes llamar a [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (o [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) en él buscar otros tipos proyectados o interfaces, que también puedes usar o devolver.

```cppwinrt
istringable.ToString();
IClosable iclosable = istringable.as<IClosable>();
iclosable.Close();
```

Si necesitas acceder a todos los miembros de la implementación y más adelante devolver una interfaz al autor de la llamada, usa la plantilla de función [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self). **make_self** devuelve un objeto [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que ajusta el tipo de implementación. Puedes acceder a los miembros de todas sus interfaces (con el operador de flecha), puedes devolverlo al autor de la llamada tal cual o puedes llamar a **as** en él y devolver el objeto de la interfaz resultante al autor de la llamada.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
myimpl->ToString();
myimpl->Close();
IClosable iclosable = myimpl.as<IClosable>();
iclosable.Close();
```

La clase **MyType** no forma parte de la proyección, es la implementación. Pero de este modo puedes llamar a sus métodos de implementación directamente, sin la sobrecarga de una llamada de función virtual. En el ejemplo anterior, aunque **MyType::ToString** usa la misma firma que el método proyectado en **IStringable**, llamamos al método no virtual directamente, sin cruzar la interfaz binaria de la aplicación (ABI). El objeto **com_ptr** simplemente tiene un puntero en la estructura **MyType**, por lo que también puedes acceder a los demás detalles internos de **MyType** a través de la variable `myimpl` y el operador de flecha.

Si tienes un objeto de la interfaz y sabes que se trata de una interfaz en tu implementación, puedes volver a la implementación mediante la plantilla de función [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self). De nuevo, es una técnica que evita llamadas a funciones virtuales y te permite acceder directamente a la implementación.

> [!NOTE]
> Si no has instalado el SDK de Windows versión 10.0.17763.0 (Windows 10, versión 1809) o posterior, tendrás que llamar a [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) en lugar de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

A continuación se muestra un ejemplo. Hay otro ejemplo en [Implementan la clase de control personalizada **BgLabelControl**](xaml-cust-ctrl.md#implement-the-bglabelcontrol-custom-control-class).

```cppwinrt
void ImplFromIClosable(IClosable const& from)
{
    MyType* myimpl = winrt::get_self<MyType>(from);
    myimpl->ToString();
    myimpl->Close();
}
```

Sin embargo, solo el objeto de interfaz original guarda una referencia. Si *tú* quieres guardarlo, puedes llamar a [**com_ptr::copy_from**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function).

```cppwinrt
winrt::com_ptr<MyType> impl;
impl.copy_from(winrt::get_self<MyType>(from));
// com_ptr::copy_from ensures that AddRef is called.
```

El propio tipo de implementación no deriva de **winrt::Windows::Foundation::IUnknown**, de modo que no tiene ninguna función **as**. No obstante, como puedes ver en la función **ImplFromIClosable** anterior, puedes tener acceso a los miembros de todas sus interfaces. Pero si lo haces, no devuelvas la instancia del tipo de implementación sin procesar al autor de la llamada. En su lugar, usa una de las técnicas mostradas más arriba y devuelve una interfaz proyectada o un objeto **com_ptr**.

Si tienes una instancia de tu tipo de implementación y necesitas pasarla a una función que espera el correspondiente tipo proyectado, puedes hacerlo tal como se muestra en el ejemplo de código a continuación. Existe un operador de conversión en tu tipo de implementación (siempre y cuando el tipo de implementación se genere mediante la herramienta `cppwinrt.exe`) que hace que esto sea posible. Puedes pasar un valor de tipo de implementación directamente a un método que espera un valor del tipo proyectado correspondiente. Desde una función de miembro de tipo de implementación, puedes pasar `*this` a un método que espera un valor del tipo proyectado correspondiente.

```cppwinrt
// MyClass.idl
import "MyOtherClass.idl";
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void MemberFunction(MyOtherClass oc);
    }
}

// MyClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyClass : MyClassT<MyClass>
    {
        MyClass() = default;
        void MemberFunction(MyProject::MyOtherClass const& oc) { oc.DoWork(*this); }
    };
}
...

// MyOtherClass.idl
import "MyClass.idl";
namespace MyProject
{
    runtimeclass MyOtherClass
    {
        MyOtherClass();
        void DoWork(MyClass c);
    }
}

// MyOtherClass.h
...
namespace winrt::MyProject::implementation
{
    struct MyOtherClass : MyOtherClassT<MyOtherClass>
    {
        MyOtherClass() = default;
        void DoWork(MyProject::MyClass const& c){ /* ... */ }
    };
}
...

//main.cpp
#include "pch.h"
#include <winrt/base.h>
#include "MyClass.h"
#include "MyOtherClass.h"
using namespace winrt;

// MyProject::MyClass is the projected type; the implementation type would be MyProject::implementation::MyClass.

void FreeFunction(MyProject::MyOtherClass const& oc)
{
    auto defaultInterface = winrt::make<MyProject::implementation::MyClass>();
    MyProject::implementation::MyClass* myimpl = winrt::get_self<MyProject::implementation::MyClass>(defaultInterface);
    oc.DoWork(*myimpl);
}
...
```

## <a name="deriving-from-a-type-that-has-a-non-default-constructor"></a>Derivar de un tipo que tiene un constructor no predeterminado

[**ToggleButtonAutomationPeer::ToggleButtonAutomationPeer(ToggleButton)** ](/uwp/api/windows.ui.xaml.automation.peers.togglebuttonautomationpeer.-ctor#Windows_UI_Xaml_Automation_Peers_ToggleButtonAutomationPeer__ctor_Windows_UI_Xaml_Controls_Primitives_ToggleButton_) es un ejemplo de un constructor no predeterminado. No hay ningún constructor predeterminado, por lo tanto, para construir un objeto **ToggleButtonAutomationPeer**, necesitas pasar un *propietario*. Como consecuencia, si derivas de **ToggleButtonAutomationPeer**, tienes que proporcionar un constructor que tome un *propietario* y que lo pase a la base. Veamos cómo se ve esto en la práctica.

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

Lo único que falta es pasar dicho parámetro del constructor a la clase base. ¿Recuerdas el patrón de polimorfismo F enlazado que mencionamos anteriormente? Una vez familiarizado con los detalles de este patrón usado por C++/WinRT, entenderás qué llama a tu clase base (o simplemente podrás buscar en el archivo de encabezado de tu clase de implementación). Esto trata cómo llamar al constructor de la clase base en este caso.

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

El constructor de clase base espera un objeto **ToggleButton**. Y **MySpecializedToggleButton** *es un* **ToggleButton**.

Hasta que no hayas realizado la modificación descrita anteriormente (pasar dicho parámetro del constructor a la clase base), el compilador marcará el constructor e indicará que no hay ningún constructor predeterminado adecuado disponible en un tipo llamado, en este caso, **MySpecializedToggleButtonAutomationPeer_base&lt;MySpecializedToggleButtonAutomationPeer&gt;** . En realidad es la clase base de la clase base de tu tipo de implementación.

## <a name="namespaces-projected-types-implementation-types-and-factories"></a>Espacios de nombres: tipos proyectados, tipos de implementación y factorías

Como has visto anteriormente en este tema, existe una clase de tiempo de ejecución C++/WinRT en forma de más de una clase de C++ que se encuentra en más de un espacio de nombres. Así pues, el nombre **MyRuntimeClass** tiene un significado en el espacio de nombres **winrt::MyProject**, y un significado diferente en el espacio de nombres **winrt::MyProject::implementation**. Ten en cuenta qué espacio de nombres tienes actualmente en contexto, y usa los prefijos de espacio de nombres si necesitas obtener un nombre de un espacio de nombres diferente. Veamos más de cerca los espacios de nombres en cuestión.

- **winrt::MyProject**. Este espacio de nombres contiene tipos proyectados. Un objeto de un tipo proyectado es un proxy; es esencialmente un puntero inteligente hacia un objeto de respaldo y ese objeto de respaldo puede implementarse en tu proyecto o podría implementarse en otra unidad de compilación.
- **winrt::MyProject::implementation**. Este espacio de nombres contiene tipos de implementación. Un objeto de un tipo de implementación no es un puntero; es un valor&mdash;un objeto de pila de C++ completo. No construyas un tipo de implementación directamente; en su lugar, llama a [**winrt::make**](/uwp/cpp-ref-for-winrt/make) y pasa tu tipo de implementación como el parámetro de la plantilla. Te hemos mostrado ejemplos de **winrt::make** en acción en este tema, y tienes otro ejemplo en la sección [Enlazar los controles XAML a una propiedad de C++/WinRT](binding-property.md#add-a-property-of-type-bookstoreviewmodel-to-mainpage). Consulta también el artículo [Diagnóstico de asignaciones directas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).
- **winrt::MyProject::factory_implementation**. Este espacio de nombres contiene factorías. Un objeto en este espacio de nombres admite [**IActivationFactory**](/windows/win32/api/activation/nn-activation-iactivationfactory).

Esta tabla muestra la calificación mínima de espacio de nombres que necesitas usar en diferentes contextos.

|El espacio de nombres que está en contexto|Para especificar el tipo proyectado|Para especificar el tipo de implementación|
|-|-|-|
|**winrt::MyProject**|`MyRuntimeClass`|`implementation::MyRuntimeClass`|
|**winrt::MyProject::implementation**|`MyProject::MyRuntimeClass`|`MyRuntimeClass`|

> [!IMPORTANT]
> Cuando quieras devolver un tipo proyectado desde la implementación, ten cuidado de no crear una instancia del tipo de implementación escribiendo `MyRuntimeClass myRuntimeClass;`. Las técnicas y el código correctos para ese escenario se muestran en este tema en la sección [Creación de instancias y retorno de interfaces y tipos de implementación](#instantiating-and-returning-implementation-types-and-interfaces).
>
> El problema con `MyRuntimeClass myRuntimeClass;` en ese escenario es que crea un objeto **winrt::MyProject::implementación::MyRuntimeClass** en la pila. Ese objeto (del tipo de implementación) se comporta como el tipo proyectado en algunos modos&mdash;también puedes invocar métodos de la misma manera, e incluso se convierte en un tipo proyectado. Sin embargo, el objeto se destruye, según las reglas normales de C++, cuando el ámbito se cierra. Por lo tanto, si devolviste un tipo proyectado (un puntero inteligente) a ese objeto, ese puntero ahora está pendiente.
>
> Este tipo de error de daño de memoria es difícil de diagnosticar. Por lo tanto, para las compilaciones de depuración, una aserción de C++/WinRT te ayudará a detectar este error, mediante un detector de pila. Aún así, las corrutinas se asignan en el montón, por lo que no obtendrás ayuda con este error si se produce en una corrutina. Para obtener más información, consulta [Diagnóstico de asignaciones directas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

## <a name="using-projected-types-and-implementation-types-with-various-cwinrt-features"></a>Usar tipos proyectados y de implementación con varias características de C++/WinRT

Aquí se indican varios lugares donde las características de C++/WinRT esperan un tipo, y qué tipo de tipo se espera (tipo proyectado, tipo de implementación o ambos).

|Característica|Acepta|Notas|
|-|-|-|
|`T` (representa un puntero inteligente)|Proyectado|Consulta el aviso en [Namespaces: projected types, implementation types, and factories](#namespaces-projected-types-implementation-types-and-factories) (Espacios de nombres: tipos proyectados, tipos de implementación y factorías) sobre el uso del tipo de implementación por error.|
|`agile_ref<T>`|Ambas|Si usas el tipo de implementación, entonces el argumento del constructor debe ser `com_ptr<T>`.|
|`com_ptr<T>`|Implementación|El uso del tipo proyectado genera el error: `'Release' is not a member of 'T'`.|
|`default_interface<T>`|Ambas|Si usas el tipo de implementación, se devuelve la primera interfaz implementada.|
|`get_self<T>`|Implementación|El uso del tipo proyectado genera el error: `'_abi_TrustLevel': is not a member of 'T'`.|
|`guid_of<T>()`|Ambas|Devuelve el GUID de la interfaz predeterminada.|
|`IWinRTTemplateInterface<T>`<br>|Proyectado|El uso del tipo de implementación se compila, pero es un error&mdash;consulta el aviso en [Namespaces: projected types, implementation types, and factories](#namespaces-projected-types-implementation-types-and-factories) (Espacios de nombres: tipos proyectados, tipos de implementación y factorías).|
|`make<T>`|Implementación|El uso del tipo proyectado genera el error: `'implements_type': is not a member of any direct or indirect base class of 'T'`.|
| `make_agile(T const&amp;)`|Ambas|Si usad el tipo de implementación, entonces el argumento debe ser `com_ptr<T>`.|
| `make_self<T>`|Implementación|El uso del tipo proyectado genera el error: `'Release': is not a member of any direct or indirect base class of 'T'`.|
| `name_of<T>`|Proyectado|Si usas el tipo de implementación, obtendrás el GUID de cadenas de la interfaz predeterminada.|
| `weak_ref<T>`|Ambas|Si usas el tipo de implementación, entonces el argumento del constructor debe ser `com_ptr<T>`.|

## <a name="opt-in-to-uniform-construction-and-direct-implementation-access"></a>Participación en la construcción uniforme y acceso de implementación directa

En esta sección se describe una característica de C++/WinRT 2.0 que es opcional, aunque está habilitada de forma predeterminada para los proyectos nuevos. Para los proyectos existentes, tendrás que participar en la característica mediante la configuración de la herramienta `cppwinrt.exe`. En Visual Studio, debes establecer la propiedad de proyecto **Propiedades comunes** > **C++/WinRT** > **Optimizado** en *Sí*. Esto agregará `<CppWinRTOptimized>true</CppWinRTOptimized>` al archivo de proyecto. Y tiene el mismo efecto que agregar el modificador al invocar `cppwinrt.exe` desde la línea de comandos.

El modificador `-opt[imize]` permite lo que a menudo se denomina *construcción uniforme*. Con la construcción uniforme (o *unificada*), se usa la propia proyección del lenguaje de C++/WinRT para crear y usar los tipos de implementación (tipos que implementa el componente para el consumo que realizan las aplicaciones) de manera eficaz y sin dificultades relacionadas con el cargador.

Antes de describir la característica, veamos primero cómo sería la situación *sin* la construcción uniforme. Comenzaremos con esta clase de Windows Runtime de ejemplo a modo ilustrativo.

```idl
// MyClass.idl
namespace MyProject
{
    runtimeclass MyClass
    {
        MyClass();
        void Method();
        static void StaticMethod();
    }
}
```

Si eres desarrollador de C++ y estás familiarizado con el uso de la biblioteca de C++/WinRT, es posible que quieras utilizar la clase de esta forma,

```cppwinrt
using namespace winrt::MyProject;

MyClass c;
c.Method();
MyClass::StaticMethod();
```

lo que es perfectamente razonable siempre y cuando el código de consumo que se muestra no resida en el mismo componente que implementa esta clase. En cuanto que proyección de lenguaje, C++/WinRT te protege como desarrollador de ABI (la interfaz binaria de aplicaciones basada en COM que define Windows Runtime). C++/WinRT no llama directamente a la implementación; viaja a través de ABI.

Por lo tanto, en la línea de código en la que estás construyendo un objeto **MyClass** (`MyClass c;`), la proyección C++/WinRT llama a [**RoGetActivationFactory**](/windows/win32/api/roapi/nf-roapi-rogetactivationfactory) para recuperar la clase o el generador de activaciones y, a continuación, usa ese generador para crear el objeto. La última línea también utiliza el generador para hacer lo que parece ser una llamada a un método estático. Todo esto requiere que la clase esté registrada y que el módulo implemente el punto de entrada [**DllGetActivationFactory**](/previous-versions/br205771(v=vs.85)). C++/WinRT tiene una memoria caché de generador muy rápida, por lo que nada de esto causa problemas para una aplicación que consume su componente. El problema es que, dentro del componente, acabas de realizar una acción algo problemática.

En primer lugar, independientemente de lo rápida que sea la caché del generador de C++/WinRT, las llamadas a través de **RoGetActivationFactory** (o incluso las llamadas posteriores a través de la caché del generador) siempre se realizarán de forma más lenta que las llamadas directas a la implementación. Obviamente, una llamada a **RoGetActivationFactory** seguida de [**IActivationFactory::ActivateInstance**](/windows/win32/api/activation/nf-activation-iactivationfactory-activateinstance) seguido de [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)) no será tan eficaz como usar una expresión `new` de C++ para un tipo definido localmente. Como consecuencia, los desarrolladores de C++/WinRT experimentados están acostumbrados a usar las funciones auxiliares [**winrt::make**](/uwp/cpp-ref-for-winrt/make) o [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) al crear objetos en un componente.

```cppwinrt
// MyClass c;
MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
```

Sin embargo, como puedes ver, no es tan cómodo ni conciso. Debes utilizar una función auxiliar para crear el objeto y también debes eliminar la ambigüedad entre el tipo de implementación y el tipo proyectado.

En segundo lugar, el uso de la proyección para crear la clase significa que su generador de activaciones se almacenará en la caché. Normalmente, eso es lo que querrías, pero, si el generador reside en el mismo módulo (DLL) que está haciendo la llamada, significa que has anclado de forma eficaz el DLL y has evitado que tenga que descargarse. En muchos casos, eso no importa; pero algunos componentes del sistema *deben* admitir la descarga.

Aquí es donde entra la *construcción uniforme*. Independientemente de si el código de creación reside en un proyecto que simplemente consumirá la clase o en el proyecto que realmente la *implementará*, puedes usar libremente la misma sintaxis para crear el objeto.

```cppwinrt
// MyProject::MyClass c{ winrt::make<implementation::MyClass>() };
MyClass c;
```

Al compilar el proyecto de componente con el modificador `-opt[imize]`, la llamada a través de la proyección del lenguaje se compila hasta la misma llamada eficaz a la función **winrt::make** que crea directamente el tipo de implementación. Esto hace que la sintaxis sea sencilla y predecible, evita que el rendimiento de la llamada a través del generador se vea afectado y evita que se ancle el componente en el proceso. Además de los proyectos de componentes, esto también es útil para las aplicaciones XAML. Omitir **RoGetActivationFactory** para clases implementadas en la misma aplicación te permite construirlas (sin necesidad de registrarlas) exactamente del mismo modo que si estuvieran fuera del componente.

La construcción uniforme se aplica a *cualquier* llamada que atienda el generador de fondo. A efectos prácticos, esto significa que la optimización satisface tanto a los constructores como a los miembros estáticos. A continuación, se muestra el ejemplo original de nuevo.

```cppwinrt
MyClass c;
c.Method();
MyClass::StaticMethod();
```

Sin `-opt[imize]`, la primera y la última instrucción requieren llamadas a través del objeto de generador. *Con* `-opt[imize]`, ninguna de las dos las requieren. Además, esas llamadas se compilan directamente en la implementación, e incluso pueden insertarse. Esto está relacionado con otro término que a menudo se usa cuando se habla de `-opt[imize]`: el acceso de *implementación directa*.

Las proyecciones de lenguaje son prácticas, pero, cuando puedes acceder directamente a la implementación, puedes y debes aprovechar esta posibilidad para generar el código más eficaz posible. C++/WinRT puede generarlo automáticamente sin que tengas que renunciar a la seguridad y la productividad de la proyección.

Se trata de un cambio importante porque el componente debe cooperar con el fin de permitir que la proyección de lenguaje llegue a sus tipos de implementación y acceda directamente a ellos. Como C++/WinRT es una biblioteca solo de encabezado, puedes ver el contenido y la actividad. Sin `-opt[imize]`, el constructor **MyClass** y el miembro **StaticMethod** se definen mediante la proyección de este modo.

```cppwinrt
namespace winrt::MyProject
{
    inline MyClass::MyClass() :
        MyClass(impl::call_factory<MyClass>([](auto&& f){
            return f.template ActivateInstance<MyClass>(); }))
    {
    }
    inline void MyClass::StaticMethod()
    {
        impl::call_factory<MyClass, MyProject::IClassStatics>([&](auto&& f) {
            return f.StaticMethod(); });
    }
}
```

No es necesario seguir todo lo anterior; la intención es mostrar que ambas llamadas implican una llamada a una función denominada **call_factory**. Esta es la pista de que estas llamadas implican el uso de la caché del generador y no tienen acceso directo a la implementación. *Con* `-opt[imize]`, estas mismas funciones no se definen en absoluto. En su lugar, se declaran mediante la proyección y sus definiciones se dejan al componente.

A continuación, el componente puede proporcionar definiciones que llamen directamente a la implementación. Ya hemos llegado al cambio importante. Estas definiciones se generan automáticamente cuando usas `-component` y `-opt[imize]`, y aparecen en un archivo denominado `Type.g.cpp`, en el que *Type* es el nombre de la clase en tiempo de ejecución que se implementará. Este es el motivo por el que te puedes encontrar varios errores del enlazador al habilitar por primera vez `-opt[imize]` en un proyecto existente. Debes incluir ese archivo generado en la implementación para poder unir elementos.

En nuestro ejemplo, `MyClass.h` podría ser similar a esto (independientemente de si `-opt[imize]` se usa o no).

```cppwinrt
// MyClass.h
#pragma once
#include "MyClass.g.h"
 
namespace winrt::MyProject::implementation
{
    struct MyClass : ClassT<MyClass>
    {
        MyClass() = default;
 
        static void StaticMethod();
        void Method();
    };
}
namespace winrt::MyProject::factory_implementation
{
    struct MyClass : ClassT<MyClass, implementation::MyClass>
    {
    };
}
```

Tu clase `MyClass.cpp` es el lugar en el que se reúne todo.

```cppwinrt
#include "pch.h"
#include "MyClass.h"
#include "MyClass.g.cpp" // !!It's important that you add this line!!
 
namespace winrt::MyProject::implementation
{
    void MyClass::StaticMethod()
    {
    }
 
    void MyClass::Method()
    {
    }
}
```

Por lo tanto, para usar la construcción uniforme en un proyecto existente, debes editar cada archivo `.cpp` de la implementación para que puedas usar `#include <Sub/Namespace/Type.g.cpp>` después de la inclusión (y la definición) de la clase de implementación. Ese archivo proporciona las definiciones de las funciones que la proyección ha dejado sin definir. Este es el aspecto de esas definiciones en el archivo `MyClass.g.cpp`.

```cppwinrt
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

Y eso completa a la perfección la proyección con llamadas eficaces directamente en la implementación, lo que evita llamadas a la caché del generador y satisface al enlazador.

Lo último que `-opt[imize]` hace de forma automática es cambiar la implementación del archivo `module.g.cpp` de tu proyecto (el que te ayuda a implementar las exportaciones de **DllGetActivationFactory** y **DllCanUnloadNow** de tu DLL) de tal forma que las compilaciones incrementales tienden a ser mucho más rápidas, ya que se elimina el acoplamiento de tipos seguros que requería C++/WinRT 1.0. Esto se conoce a menudo como *generadores de borrado de tipos*. Sin `-opt[imize]`, el archivo `module.g.cpp` que se genera para el componente se inicia mediante la inclusión de las definiciones de todas las clases de implementación (la clase `MyClass.h` en este ejemplo). A continuación, crea directamente el generador de implementaciones para cada clase así.

```cppwinrt
if (requal(name, L"MyProject.MyClass"))
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
```

De nuevo, no es necesario que sigas todos los detalles. Lo que resulta útil de ver es que esto requiere la definición completa de todas las clases que implemente el componente. Esto puede afectar de forma drástica al bucle interno, ya que, solo con que se cambie una única implementación, `module.g.cpp` se volverá a compilar. Con `-opt[imize]`, esto ya no sucede. En su lugar, se producen dos cambios en el archivo `module.g.cpp` generado. El primero es que ya no incluye ninguna clase de implementación. En este ejemplo, no incluirá `MyClass.h` en absoluto. En su lugar, creará los generadores de implementaciones sin ningún conocimiento de su implementación.

```cppwinrt
void* winrt_make_MyProject_MyClass();
 
if (requal(name, L"MyProject.MyClass"))
{
    return winrt_make_MyProject_MyClass();
}
```

Obviamente, no es necesario incluir sus definiciones y corresponde al enlazador resolver la definición de la función **winrt_make_Component_Class**. Naturalmente, no es necesario que te preocupes por esto, ya que el archivo `MyClass.g.cpp` que se genera automáticamente (y que has incluido previamente para admitir la construcción uniforme) también define esta función. Este es el archivo `MyClass.g.cpp` completo que se ha generado para este ejemplo.

```cppwinrt
void* winrt_make_MyProject_MyClass()
{
    return winrt::detach_abi(winrt::make<winrt::MyProject::factory_implementation::MyClass>());
}
namespace winrt::MyProject
{
    MyClass::MyClass() :
        MyClass(make<MyProject::implementation::MyClass>())
    {
    }
    void MyClass::StaticMethod()
    {
        return MyProject::implementation::MyClass::StaticMethod();
    }
}
```

Como puedes ver, la función **winrt_make_MyProject_MyClass** crea directamente el generador de implementaciones. Esto significa que puedes cambiar sin problema cualquier implementación y que no será necesario recompilar `module.g.cpp`. `module.g.cpp` solo se actualizará y deberá recompilarse cuando agregues o quites clases de Windows Runtime.

## <a name="overriding-base-class-virtual-methods"></a>Reemplazar los métodos virtuales de la clase base

La clase derivada puede tener problemas con los métodos virtuales si tanto la clase base como la clase derivada son clases definidas por la aplicación, pero el método virtual se define en una clase Windows Runtime de entidad primaria principal. En la práctica, esto sucede si derivas desde clases XAML. El resto de esta sección continúa a partir del ejemplo de [Clases derivadas](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx#derived-classes).

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

La jerarquía es [**Windows::UI::Xaml::Controls::Page**](/uwp/api/windows.ui.xaml.controls.page) \<- **BasePage** \<- **DerivedPage**. El método **BasePage::OnNavigatedFrom** invalida correctamente [**Page::OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom), pero **DerivedPage::OnNavigatedFrom** no invalida **BasePage::OnNavigatedFrom**.

Aquí, **DerivedPage** reutiliza vtable **IPageOverrides** de **BasePage**, es decir, no invalida el método **IPageOverrides::OnNavigatedFrom**. Una posible solución requiere que **BasePage** sea una clase de plantilla y que su implementación se haga completamente en un archivo de encabezado, pero eso hace que las cosas sean excesivamente complicadas.

Como solución alternativa, declara el método **OnNavigatedFrom** como virtual explícitamente en la clase base. De ese modo, cuando la entrada de vtable para **DerivedPage::IPageOverrides::OnNavigatedFrom** llame a **BasePage::IPageOverrides::OnNavigatedFrom**, el productor llama a **BasePage::OnNavigatedFrom**, que (debido a su cualidad de virtual), termina llamando a **DerivedPage::OnNavigatedFrom**.

```cppwinrt
namespace winrt::MyNamespace::implementation
{
    struct BasePage : BasePageT<BasePage>
    {
        // Note the `virtual` keyword here.
        virtual void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };

    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        void OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
    };
}
```

Esto requiere que todos los miembros de la jerarquía de la clase acuerden el valor devuelto y los tipos de parámetros del método **OnNavigatedFrom**. Si no están de acuerdo, debes usar la versión anterior como método virtual y encapsular las alternativas.

## <a name="important-apis"></a>API importantes
* [Plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Función winrt::com_ptr::copy_from](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcopy_from-function)
* [Plantilla de función winrt::from_abi](/uwp/cpp-ref-for-winrt/from-abi)
* [Plantilla de función winrt::get_self](/uwp/cpp-ref-for-winrt/get-self)
* [Plantilla de estructura winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Plantilla de función winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)
* [Función winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)

## <a name="related-topics"></a>Temas relacionados
* [Crear eventos en C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-events)
* [Consumir API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-apis)
* [Componentes de Windows Runtime con C++/WinRT](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)
* [Controles de XAML; enlazar a una propiedad de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-property)
