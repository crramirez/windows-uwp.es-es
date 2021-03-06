---
description: En este tema se muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que genera eventos. También se muestra una aplicación que consume el componente y controla los eventos.
title: Crear eventos en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, author, event
ms.localizationpriority: medium
ms.openlocfilehash: 66691b1cd75a27e683261c12b7a3056c53160079
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297625"
---
# <a name="author-events-in-cwinrt"></a>Crear eventos en C++/WinRT

Este tema se basa en el componente de Windows Runtime, y la aplicación que lo usa, que en el tema [Componentes de Windows Runtime con C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) se muestra cómo compilar.

Estas son las nuevas características que agrega este tema.
- Actualización de la clase de tiempo de ejecución de termómetro para que genere un evento cuando su temperatura cae por debajo del congelamiento.
- Actualización de la aplicación principal que usa la clase de tiempo de ejecución del termómetro para que controle ese evento.

> [!NOTE]
> Para más información sobre cómo instalar y usar [C++/WinRT](./intro-to-using-cpp-with-winrt.md) Visual Studio Extension (VSIX) y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="create-thermometerwrc-and-thermometercoreapp"></a>Creación de**ThermometerWRC** y **ThermometerCoreApp**

Si desea continuar con las actualizaciones que se muestran en este tema, para que pueda compilar y ejecutar el código, el primer paso es seguir el tutorial del tema [Componentes de Windows Runtime con C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md). Después de hacerlo, tendrá el componente de Windows Runtime **ThermometerWRC** y la aplicación principal **ThermometerCoreApp** que lo usa.

## <a name="update-thermometerwrc-to-raise-an-event"></a>Actualización de **ThermometerWRC** para generar un evento

Actualice `Thermometer.idl` para que se parezca al listado siguiente. Así es cómo se declara un evento cuyo tipo de delegado es [**EventHandler**](/uwp/api/windows.foundation.eventhandler-1) con un argumento de un número de punto flotante de precisión simple.

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
        event Windows.Foundation.EventHandler<Single> TemperatureIsBelowFreezing;
    };
}
```

Guarde el archivo. El proyecto no se compilará completamente en su estado actual, pero realice una compilación ahora de todas formas para generar versiones actualizadas de los archivos de código auxiliar `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` y `Thermometer.cpp`. Dentro de esos archivos, podrá ver implementaciones de código auxiliar del evento **TemperatureIsBelowFreezing**. En C++ / WinRT, un evento declarado IDL se implementa como un conjunto de funciones sobrecargadas (similares a la forma en que se implementa una propiedad como un par de funciones get y set sobrecargadas). Una sobrecarga toma un delegado que se va a registrar y devuelve un token ([**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)). La otra toma un token y revoca el registro del delegado asociado.

Ahora abra `Thermometer.h` y `Thermometer.cpp`, y actualice la implementación de la clase de tiempo de ejecución **Thermometer**. En `Thermometer.h`, agregue las dos funciones sobrecargadas **TemperatureIsBelowFreezing**, así como un miembro de datos de evento privado que se usará en la implementación de esas funciones.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...
        winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler);
        void TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_temperatureIsBelowFreezingEvent;
        ...
    };
}
...
```

Como puede ver más arriba, un evento se representa mediante la plantilla de estructura [**winrt::event**](/uwp/cpp-ref-for-winrt/event), parametrizada por un tipo de delegado determinado (que se puede parametrizar mediante un tipo args).

En `Thermometer.cpp`, implemente las dos funciones sobrecargadas **TemperatureIsBelowFreezing**.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_temperatureIsBelowFreezingEvent.add(handler);
    }

    void Thermometer::TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept
    {
        m_temperatureIsBelowFreezingEvent.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f) m_temperatureIsBelowFreezingEvent(*this, m_temperatureFahrenheit);
    }
}
```

> [!NOTE]
> Para conocer de qué se trata un revocador automático de eventos, consulta [Revocación de un delegado registrado](handle-events.md#revoke-a-registered-delegate). La implementación del revocador automático de eventos se incluye de forma gratuita para el evento. Es decir, no es necesario implementar la sobrecarga para el revocador de eventos &mdash;que la proyección de C++/WinRT proporciona automáticamente.

Las otras sobrecargas (las sobrecargas de revocación manual y registro) *no* se incluyen en la proyección. Esto te proporciona la flexibilidad para implementarlas de forma óptima para tu escenario. Una llamada a [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) y a [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) tal como se muestra en estas implementaciones es un valor predeterminado eficaz y seguro para simultaneidad o subprocesos. Pero si tienes un gran número de eventos, es posible que no desees un campo de evento para cada uno, sino optar en su lugar por algún tipo de implementación dispersa.

Además de esto, también puede ver que la implementación de la función **AdjustTemperature** se ha actualizado para generar el evento **TemperatureIsBelowFreezing** si la temperatura cae por debajo del congelamiento.

## <a name="update-thermometercoreapp-to-handle-the-event"></a>Actualización de **ThermometerCoreApp** para controlar el evento

En el proyecto **ThermometerCoreApp**, en `App.cpp`, realice los cambios siguientes en el código para registrar un controlador de eventos y, a continuación, haga que la temperatura caiga por debajo del congelamiento.

`WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto &, float temperatureFahrenheit)
        {
            WINRT_ASSERT(temperatureFahrenheit < 32.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.TemperatureIsBelowFreezing(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

Tenga en cuenta el cambio en el método **OnPointerPressed**. Ahora, cada vez que haga clic en la ventana, *resta* 1 grado de Fahrenheit de la temperatura del termómetro. Y ahora, la aplicación controla el evento que se genera cuando la temperatura cae por debajo del congelamiento. Para demostrar que se genera el evento según lo esperado, coloca un punto de interrupción en la expresión lambda que controla el evento **TemperatureIsBelowFreezing**, ejecuta la aplicación y haz clic dentro de la ventana.

## <a name="parameterized-delegates-across-an-abi"></a>Delegados con parámetros mediante una ABI

Si el evento debe ser accesible mediante una interfaz binaria de aplicación (ABI), &mdash;por ejemplo, entre un componente y su aplicación consumidora&mdash;, el evento debe usar un tipo delegado de Windows Runtime. En el ejemplo anterior se usa el tipo delegado [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) de Windows Runtime. [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) es otro ejemplo de un tipo delegado de Windows Runtime.

Los parámetros de tipo de esos dos tipos delegados deben cruzar la ABI, por lo que los parámetros deben ser también tipos de Windows Runtime. Eso incluye clases en tiempo de ejecución de Windows y de terceros, y tipos primitivos, como cadenas y números. El compilador le ayuda con el error "*T debe ser de tipo WinRT*" si olvida esta restricción.

A continuación, se muestra un ejemplo en formato de listados de código. Comience con los proyectos **ThermometerWRC** y **ThermometerCoreApp** que creó anteriormente en este tema y edite el código de dichos proyectos para que se parezcan al de los siguientes listados.

Este primer listado es para el proyecto **ThermometerWRC**. Después de editar `ThermometerWRC.idl` tal y como se muestra a continuación, compile el proyecto y, a continuación, copie `MyEventArgs.h` y `.cpp` en él (desde la carpeta `Generated Files`) como hizo anteriormente con `Thermometer.h` y `.cpp`.

```cppwinrt
// ThermometerWRC.idl
namespace ThermometerWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single TemperatureFahrenheit{ get; };
    }

    [default_interface]
    runtimeclass Thermometer
    {
        ...
        event Windows.Foundation.EventHandler<ThermometerWRC.MyEventArgs> TemperatureIsBelowFreezing;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::ThermometerWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float temperatureFahrenheit);
        float TemperatureFahrenheit();

    private:
        float m_temperatureFahrenheit{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::ThermometerWRC::implementation
{
    MyEventArgs::MyEventArgs(float temperatureFahrenheit) : m_temperatureFahrenheit(temperatureFahrenheit)
    {
    }

    float MyEventArgs::TemperatureFahrenheit()
    {
        return m_temperatureFahrenheit;
    }
}

// Thermometer.h
...
struct Thermometer : ThermometerT<Thermometer>
{
...
    winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs>> m_temperatureIsBelowFreezingEvent;
...
}
...

// Thermometer.cpp
#include "MyEventArgs.h"
...
winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler) { ... }
...
void Thermometer::AdjustTemperature(float deltaFahrenheit)
{
    m_temperatureFahrenheit += deltaFahrenheit;

    if (m_temperatureFahrenheit < 32.f)
    {
        auto args = winrt::make_self<winrt::ThermometerWRC::implementation::MyEventArgs>(m_temperatureFahrenheit);
        m_temperatureIsBelowFreezingEvent(*this, *args);
    }
}
...
```

Este listado es para el proyecto **ThermometerCoreApp**.

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto&, ThermometerWRC::MyEventArgs args)
    {
        float degrees = args.TemperatureFahrenheit();
        WINRT_ASSERT(degrees < 32.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>Señales simples mediante una ABI

Si no tienes que pasar los parámetros o los argumentos con el evento, puedes definir tu propio tipo delegado simple de Windows Runtime. En el ejemplo siguiente se muestra una versión más sencilla de la clase en tiempo de ejecución **Thermometer**. Declara un tipo delegado llamado **SignalDelegate** y lo usa para generar un evento de tipo de señal en lugar de un evento con un parámetro.

```idl
// ThermometerWRC.idl
namespace ThermometerWRC
{
    delegate void SignalDelegate();

    runtimeclass Thermometer
    {
        Thermometer();
        event ThermometerWRC.SignalDelegate SignalTemperatureIsBelowFreezing;
        void AdjustTemperature(Single value);
    };
}
```

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

        winrt::event_token SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler);
        void SignalTemperatureIsBelowFreezing(winrt::event_token const& token);
        void AdjustTemperature(float deltaFahrenheit);

    private:
        winrt::event<ThermometerWRC::SignalDelegate> m_signal;
        float m_temperatureFahrenheit{ 0.f };
    };
}
```

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void Thermometer::SignalTemperatureIsBelowFreezing(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.SignalTemperatureIsBelowFreezing([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.SignalTemperatureIsBelowFreezing(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Delegados con parámetros, señales simples y devoluciones de llamadas dentro de un proyecto

Si necesitas que tus eventos se mantengan dentro de un proyecto de Visual Studio (no en varios archivos binarios) y que dichos eventos no se limiten a los tipos de Windows Runtime, todavía puedes usar la plantilla de clase [**winrt::event**](/uwp/cpp-ref-for-winrt/event)\<Delegate\>. Simplemente usa [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) en lugar de un tipo de delegado de Windows Runtime real, ya que **winrt::delegate** también admite parámetros que no son de Windows Runtime.

El ejemplo siguiente muestra primero una firma delegada que no toma ningún parámetro (básicamente una señal simple) y, a continuación, una que toma una cadena.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

Observa que puedes agregar al evento tantos delegados de suscripción como quieras. Sin embargo, los eventos conllevan una cierta sobrecarga. Si todo lo que necesitas es una devolución de llamada simple con un solo delegado de suscripción, puedes usar [**winrt::delegate&lt;. T&gt;** ](/uwp/cpp-ref-for-winrt/delegate) por sí misma.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si vas a migrar desde una base de código de C++/CX en la que los eventos y delegados se usan internamente (no en los archivos binarios), **winrt::delegate** te ayudará a replicar dicho patrón en C++/WinRT.

## <a name="design-guidelines"></a>Directrices de diseño

Te recomendamos que pases eventos, no delegados, como parámetros de función. La función **add** de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) es la única excepción, porque tienes que pasar un delegado en ese caso. La razón de esta directriz es que los delegados pueden adoptar diferentes formas en los diferentes lenguajes de Windows Runtime (en términos de si son compatibles con el registro de uno o varios clientes). Los eventos, con su modelo de suscriptor múltiple, constituyen una opción mucho más coherente y predecible.

La firma de un delegado de controlador de eventos debe constar de dos parámetros: *sender* (**IInspectable**) y *args* (algún tipo de argumento de evento, por ejemplo [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Observa que estas instrucciones no se aplican necesariamente si vas a diseñar una API interna. Sin embargo, las API internas terminan por convertirse en públicas con el tiempo.

## <a name="related-topics"></a>Temas relacionados
* [Crear API con C++/WinRT](author-apis.md)
* [Consumir API con C++/WinRT](consume-apis.md)
* [Control de eventos mediante delegados en C++/WinRT](handle-events.md)
* [Componentes de Windows Runtime con C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)