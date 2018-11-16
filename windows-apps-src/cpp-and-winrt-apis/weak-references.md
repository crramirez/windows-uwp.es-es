---
author: stevewhims
description: El tiempo de ejecución de Windows es un sistema de recuento de referencia; y en un sistema de este tipo es importante saber sobre el significado de y la distinción entre, referencias fuertes y débiles.
title: Referencias débiles en C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, sólida, débil, referencia
ms.localizationpriority: medium
ms.openlocfilehash: c37319ce7f7d29acfc2c1822e76fadc29b5ec863
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6978344"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Referencias fuertes y débiles en C++ / WinRT

El tiempo de ejecución de Windows es un sistema de recuento de referencia; y en un sistema de este tipo es importante saber sobre el significado de y la distinción entre, seguro y débil referencias (y referencias que no están instaladas, como el puntero implícito *este* ). Como se verá en este tema, saber cómo administrar estas referencias correctamente puede significar la diferencia entre un sistema confiable que se ejecuta sin problemas y que se bloquea impredecible. Proporcionando funciones auxiliares que tienen compatibilidad detallado en la proyección de lenguaje, [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) cumple medio en el trabajo de la creación de sistemas más complejos simplemente y correctamente.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acceso de forma segura al puntero *this* en una corrutina de miembro de clase

El código de la descripción a continuación muestra un ejemplo típico de una corrutina que es una función miembro de una clase.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

struct MyClass : winrt::implements<MyClass, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    IAsyncOperation<winrt::hstring> RetrieveValueAsync()
    {
        co_await 5s;
        co_return m_value;
    }
};

int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };

    winrt::hstring result{ async.get() };
    std::wcout << result.c_str() << std::endl;
}
```

**MyClass::RetrieveValueAsync** trabajar durante un tiempo y, a continuación, finalmente devuelve una copia de la `MyClass::m_value` miembro de datos. Una llamada a **RetrieveValueAsync** hace que el objeto asincrónico crearse y ese objeto tiene un puntero implícito *este* (a través del cual, finalmente, `m_value` se accede a).

Esta es la secuencia de eventos completa.

1. En **principal**, se crea una instancia de **MyClass** (`myclass_instance`).
2. El `async` se crea el objeto, que señala (a través de su *este*) a `myclass_instance`.
3. La función **winrt::Windows::Foundation::IAsyncAction::get** bloquea durante unos segundos y, a continuación, devuelve el resultado de **RetrieveValueAsync**.
4. **RetrieveValueAsync** devuelve el valor de `this->m_value`.

Paso 4 es seguro como *esta* es válido.

Pero, ¿qué ocurre si se destruye la instancia de clase antes de que finalice la operación asincrónica? Existen todos los tipos de formas de que la instancia de clase podías fuera del ámbito antes de que finalice el método asincrónico. No obstante, podemos la simulamos estableciendo la instancia de clase en `nullptr`.

```cppwinrt
int main()
{
    winrt::init_apartment();

    auto myclass_instance{ winrt::make_self<MyClass>() };
    auto async{ myclass_instance->RetrieveValueAsync() };
    myclass_instance = nullptr; // Simulate the class instance going out of scope.

    winrt::hstring result{ async.get() }; // Behavior is now undefined; crashing is likely.
    std::wcout << result.c_str() << std::endl;
}
```

Después del punto donde se destruir la instancia de clase, parece que no directamente llamamos a él otra vez. Pero supuesto objeto asincrónico tiene un puntero *this* a ella e intenta usa para copiar el valor almacenado dentro de la instancia de clase. La corrutina es una función miembro y espera que pueda usar su puntero *this* con impunidad.

Con este cambio en el código, nos encontramos con un problema en el paso 4, dado que haber destruido la instancia de clase y *este* ya no es válida. Tan pronto como el objeto asincrónico intenta tener acceso a la variable dentro de la instancia de clase, se bloquee (o hacer algo totalmente undefined).

La solución consiste en dar la operación asincrónica&mdash;la corrutina&mdash;su propia referencia fuerte a la instancia de clase. Tal como está escrito, la corrutina eficazmente contiene un puntero sin procesar *este* a la instancia de clase; pero eso no es suficiente para mantener activa la instancia de clase.

Para mantener activa la instancia de clase, cambiar la implementación de **RetrieveValueAsync** para se muestra a continuación.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Dado que C++ / objeto de WinRT directa o indirectamente se deriva de la plantilla de [**winrt:: Implements**](/uwp/cpp-ref-for-winrt/implements) , C++ / WinRT objeto puede llamar a su función de miembro [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) protegido para recuperar una referencia fuerte a su puntero *this* . Ten en cuenta que no es necesario para usar realmente la `strong_this` variable; solo una llamada a **get_strong** incrementa el recuento de referencia y mantiene el puntero implícito *este* válido.

Esto resuelve el problema que teníamos anteriormente cuando que obtuvimos en el paso 4. Aunque todas las otras referencias a la instancia de clase desaparecen, la corrutina ha tomado la precaución de garantizar que sus dependencias están estables.

Si una referencia fuerte no es adecuada, a continuación, en su lugar se puede llamar [**Implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) para recuperar una referencia débil a *este*. Confirma que se puede recuperar una referencia fuerte antes de acceder a *esta*.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto weak_this{ get_weak() }; // Maybe keep *this* alive.

    co_await 5s;

    if (auto strong_this{ weak_this.get() })
    {
        co_return m_value;
    }
    else
    {
        co_return L"";
    }
}
```

En el ejemplo anterior, la referencia débil no mantiene la instancia de clase de destruirse cuando no permanecen ninguna referencia fuerte. Pero te dará una forma de comprobar si se puede adquirir una referencia fuerte antes de acceder a la variable de miembro.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acceso con seguridad a *este* puntero con un delegado de controlador de eventos

### <a name="the-scenario"></a>El escenario

Para obtener información general sobre el control de eventos, consulta [controlar eventos usando delegados en C++ / WinRT](handle-events.md).

La sección anterior resaltado posibles problemas de ciclo de vida en las áreas de las corrutinas y la simultaneidad. No obstante, si controlas un evento con la función de miembro de un objeto o desde una función lambda dentro de la función de miembro de un objeto, a continuación, se necesitan pensar en las duraciones relativas del destinatario del evento (el objeto que controla el evento) y el origen del evento (el objeto genera el evento). Echemos un vistazo a algunos ejemplos de código.

En primer lugar, el listado de código a continuación define una clase de **origen de eventos** simple, lo cual genera un evento genérico que se controla mediante los delegados que se han agregado a él. Se produce este evento de ejemplo usar el tipo de delegado [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) , pero los problemas y soluciones aquí se aplican a los tipos de delegado todos.

A continuación, la clase **EventRecipient** proporciona un controlador para el evento **EventSource::Event** en forma de una función lambda.

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;

struct EventSource
{
    winrt::event<EventHandler<int>> m_event;

    void Event(EventHandler<int> const& handler)
    {
        m_event.add(handler);
    }

    void RaiseEvent()
    {
        m_event(nullptr, 0);
    }
};

struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event([&](auto&& ...)
        {
            std::wcout << m_value.c_str() << std::endl;
        });
    }
};

int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_source.RaiseEvent();
}
```

El patrón es que el destinatario del evento tiene un controlador de eventos de la expresión lambda con dependencias en su puntero de *este* . Siempre que el destinatario del evento sobrevive al origen del evento, sobrevive a estas dependencias. Y en estos casos, que son comunes, el patrón funciona bien. Algunos de estos casos son evidentes, por ejemplo, cuando una página de interfaz de usuario controla un evento generado por un control que se encuentra en la página. El botón de sobrevive a la página&mdash;por lo tanto, el controlador sobrevive también al botón. Esto es válido siempre que el destinatario posea el origen (como un miembro de datos, por ejemplo), o cada vez que el destinatario y el origen estén relacionados o pertenezcan directamente a otro objeto. Si estás seguro de que tienes un caso en el que el controlador no sobrevivirá al objeto *this* del que depende, puedes capturar *this* de forma normal, sin tener en cuenta una duración fuerte o débil.

Pero todavía hay casos donde *este* no sobrevive a su uso en un controlador (incluidos los controladores para eventos de progreso generados por acciones asincrónicas y operaciones y la finalización) y es importante saber cómo tratar con ellos.

- Si vas a crear una corrutina para implementar un método asincrónico, entonces es posible.
- En raras ocasiones con ciertos objetos del marco de la interfaz de usuario XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), por ejemplo) es posible, siempre que se haya finalizado el destinatario sin anular el registro del origen del evento.

### <a name="the-issue"></a>El problema

Esta nueva versión de la función **main** simula lo que sucede cuando se destruye destinatario del evento (quizás sale del ámbito) mientras el origen del evento aún generación de eventos.

```cppwinrt
int main()
{
    winrt::init_apartment();

    EventSource event_source;
    auto event_recipient{ winrt::make_self<EventRecipient>() };
    event_recipient->Register(event_source);
    event_recipient = nullptr; // Simulate the event recipient going out of scope.
    event_source.RaiseEvent(); // Behavior is now undefined within the lambda event handler; crashing is likely.
}
```

Destinatario del evento se destruye, pero el controlador de eventos de la expresión lambda dentro de él aún se suscribe al **evento** . Cuando se genera el evento, la expresión lambda intenta desreferenciar el puntero *this* , que no es válido en ese momento. Por lo tanto, una infracción de acceso de código en el controlador (o en la continuación de una corrutina) intenta utilizarlo.

> [!IMPORTANT]
> Si se producen una situación como este, a continuación, tendrás que pensar en el ciclo de vida de *este* objeto; y si el objeto capturado *este* sobrevive a la captura. Si no es así, captúralo con una referencia fuerte o débil, como a continuación se muestra a continuación.
>
> O&mdash;si tiene sentido para tu escenario y los subprocesos consideraciones lo hacen posible&mdash;, otra opción es revocar el controlador una vez hecho el destinatario con el evento o en el destructor del destinatario. Consulta [revocar a un delegado registrado](handle-events.md#revoke-a-registered-delegate).

Esto es cómo nos estamos registrar el controlador.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

La función lambda captura automáticamente las variables locales por referencia. Por lo tanto, para este ejemplo, hemos podríamos equivalente escrita esto.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

En ambos casos, solo nos estamos captura el puntero sin procesar *este* . Y que no tiene ningún efecto en el recuento de referencias, por lo que nada impide que el objeto actual se destruye.

### <a name="the-solution"></a>La solución

La solución es capturar una referencia fuerte. Una referencia fuerte *hace* incrementa el recuento de referencias y se *hace* mantener el objeto actual que se activa. Sólo es necesario declarar una variable de captura (denominada `strong_this` en este ejemplo) y se inicializa con una llamada a [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function), que recupera una referencia fuerte a nuestro *este* puntero.

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Puedes incluso omitir la captura automática del objeto actual y tener acceso a los miembros de datos a través de la variable de captura en lugar de a través de la implícita *este*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Si una referencia fuerte no es adecuada, a continuación, en su lugar se puede llamar [**Implements:: get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) para recuperar una referencia débil a *este*. Confirma que todavía puede recuperar una referencia fuerte de ella antes de acceder a los miembros.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Si usas una función miembro como un delegado

Así como estos principios funciones lambda, también se aplican al uso de una función miembro como tu delegado. La sintaxis es diferente, por lo tanto, echemos un vistazo a parte del código. En primer lugar, este es el controlador de eventos de la función de miembro potencialmente peligrosos, usa un puntero sin procesar *este* .

```cppwinrt
struct EventRecipient : winrt::implements<EventRecipient, IInspectable>
{
    winrt::hstring m_value{ L"Hello, World!" };

    void Register(EventSource& event_source)
    {
        event_source.Event({ this, &EventRecipient::OnEvent });
    }

    void OnEvent(IInspectable const& /* sender */, int /* args */)
    {
        std::wcout << m_value.c_str() << std::endl;
    }
};
```

Esta es la forma estándar, convencional para hacer referencia a un objeto y su función miembro. Para que esto seguros, puedes&mdash;a partir de la versión 10.0.17763.0 (Windows 10, versión 1809) del Windows SDK&mdash;establecer una referencia fuerte o débil en el punto donde se registra el controlador. En ese momento, el objeto de destinatario del evento se sabe que es aún activo.

Para obtener una referencia fuerte, simplemente llama al [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) en lugar del puntero sin procesar *este* . C++ / WinRT garantiza que el delegado resultante contiene una referencia fuerte al objeto actual.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Para obtener una referencia débil, llamar a [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function). C++ / WinRT garantiza que el delegado resultante contiene una referencia débil. En el último momento y en segundo plano, el delegado intenta resolver la referencia débil a una sólida y solo llama a la función miembro si es correcta.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Un ejemplo de referencia débil con **SwapChainPanel::CompositionScaleChanged**

En este ejemplo de código, se usa el evento [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) por medio de otra ilustración de referencias débiles. El código registra un controlador de eventos usando un lambda que captura una referencia débil al destinatario.

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weak_this{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strong_this{ weak_this.get() })
        {
            strong_this->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

En la cláusula de captura lamba, se crea una variable temporal que representa una referencia débil a *this*. En el cuerpo del lambda, si puede obtenerse una referencia fuerte a *this*, se llama a la función **OnCompositionScaleChanged**. De esta forma, *this* puede usarse con seguridad dentro de ** OnCompositionScaleChanged**.

## <a name="weak-references-in-cwinrt"></a>Referencias débiles en C++/WinRT

Anteriormente, hemos visto referencias débiles que se usa. En general, son un buen recurso para interrumpir las referencias cíclicas. Por ejemplo, para la implementación nativa del marco de interfaz de usuario basada en XAML&mdash;debido al diseño histórico del marco&mdash;la referencia débil mecanismo en C++ / WinRT es necesario para controlar referencias cíclicas. Fuera de XAML, sin embargo, es probable que no tienes que usar referencias débiles (no que no hay nada inherentemente XAML específica sobre ellos). En su lugar, a menudo, podrás diseñar tu propio C++ / WinRT APIs de tal forma que se evite la necesidad de referencias cíclicas y referencias débiles. 

Para cualquier tipo en particular que declares, a C ++/WinRT no le resulta inmediatamente evidente saber si se necesitan referencias débiles o cuándo se necesitan. De este modo, C++ / WinRT proporciona soporte técnico de referencia débil automáticamente en la plantilla de estructura [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), desde la que tus propios tipos C++/WinRT derivan directa o indirectamente. Es un sistema de pago pay-to-play, esto es, no te cuesta nada a menos que en realidad se consulte tu objeto para [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource). Y puedes [rechazar recibir tal soporte](#opting-out-of-weak-reference-support) explícitamente.

### <a name="code-examples"></a>Ejemplos de código
La plantilla de estructura [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) es una opción para obtener una referencia débil a una instancia de clase.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```

O bien, puedes usar la función auxiliar [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

Crear una referencia débil no afecta el recuento de referencias en el propio objeto; simplemente hace que se asigne un bloque de control. El bloque de control se encarga de implementar la semántica de la referencia débil. Puedes intentar promover la referencia débil a una referencia fuerte y, si se realiza correctamente, utilizarla.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

Siempre que exista alguna otra referencia fuerte, la llamada [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) incrementa el recuento de referencias y devuelve la referencia fuerte al autor de la llamada.

### <a name="opting-out-of-weak-reference-support"></a>Rechazar el soporte de referencia débil
El soporte de referencia débil es automático. Pero puedes optar por rechazar explícitamente tal soporte pasando la estructura del marcador [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) como un argumento plantilla a tu clase base.

Si derivas directamente desde **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Si vas a crear una clase en tiempo de ejecución.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

No importa dónde aparezca la estructura del marcador dentro del paquete de parámetro variádicas. Si solicitas una referencia débil para un tipo rechazado, el compilador te ayudará con "*Esto es solo para soporte de referencia débil*".

## <a name="important-apis"></a>API importantes
* [Función winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Plantilla de función winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [Estructura de marcador winrt::no_weak_ref](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Plantilla de estructura winrt::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)
