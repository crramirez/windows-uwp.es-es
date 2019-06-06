---
description: Windows Runtime es un sistema con recuento de referencias y en este tipo de sistemas es importante conocer el significado de referencias fuertes y débiles y la diferencia entre ellas.
title: Referencias débiles en C++/WinRT
ms.date: 05/16/2019
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, referencia fuerte y débil,
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 46a0e21295ba430671be4e36ab213e182c2b1737
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721634"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Las referencias fuertes y débiles en C / c++ / WinRT

El tiempo de ejecución de Windows es un sistema de recuento de referencias; y, en este sistema es importante conocer el significado de y la distinción entre, las referencias fuertes y débiles (y las referencias que son ninguno, como implícito *esto* puntero). Como verá en este tema, saber cómo administrar estas referencias correctamente puede significar la diferencia entre un sistema confiable que se ejecuta sin problemas y que se bloquea de forma impredecible. Al proporcionar funciones auxiliares que cuenta con compatibilidad completa en la proyección del lenguaje, [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) cumple sus a medio camino en el trabajo de creación de sistemas más complejos simplemente y correctamente.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acceso de forma segura la *esto* puntero en una corrutina de miembro de clase

La lista de código siguiente muestra un ejemplo típico de una corrutina que es una función miembro de una clase. Se puede copiar y pegar este ejemplo en los archivos especificados en una nueva **aplicación de consola de Windows (C++/WinRT)** proyecto.

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

**MyClass::RetrieveValueAsync** invierte en algún trabajo de duración, y, finalmente, devuelve una copia de la `MyClass::m_value` miembro de datos. Una llamada a **RetrieveValueAsync** hace que un objeto asincrónico en crearse, y ese objeto tiene implícita *esto* puntero (a través del cual, finalmente, `m_value` se tiene acceso).

Esta es la secuencia completa de eventos.

1. En **principal**, una instancia de **MyClass** se crea (`myclass_instance`).
2. El `async` se crea el objeto, que señala (mediante su *esto*) a `myclass_instance`.
3. El **winrt::Windows::Foundation::IAsyncAction::get** función bloquea durante unos segundos y, a continuación, devuelve el resultado de **RetrieveValueAsync**.
4. **RetrieveValueAsync** devuelve el valor de `this->m_value`.

Paso 4 es seguro, siempre *esto* es válido.

Pero, ¿qué ocurre si se destruye la instancia de clase antes de que finalice la operación asincrónica? Hay todo tipo de formas de que la instancia de clase podría estar fuera del ámbito antes de que finalice el método asincrónico. Pero nos podemos simular estableciendo la instancia de clase en `nullptr`.

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

Después del punto donde se destruye la instancia de clase, parece que se no hacer referencia directamente a ella nuevamente. Pero por supuesto el objeto asincrónico tiene un *esto* puntero a la base de datos e intenta usarlo para copiar el valor almacenado dentro de la instancia de clase. La corrutina es una función miembro, y espera que pueda usar su *esto* puntero con impunidad.

Con este cambio en el código, nos encontramos con un problema en el paso 4, porque se ha destruido la instancia de clase, y *esto* ya no es válido. Tan pronto como el objeto asincrónico intenta obtener acceso a la variable dentro de la instancia de clase, se bloqueará (o hacer algo completamente sin definir).

La solución consiste en proporcionar la operación asincrónica&mdash;la corrutina&mdash;su propia referencia fuerte a la instancia de clase. Tal como está escrito, la corrutina contiene eficazmente un raw *esto* puntero a la instancia de clase; pero que no es suficiente para mantener activa la instancia de clase.

Para mantener activa la instancia de clase, cambie la implementación de **RetrieveValueAsync** a la que se muestra a continuación.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Un C++/WinRT clase directa o indirectamente se deriva el [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements) plantilla. Por este motivo, el C++/objeto WinRT puede llamar a su [ **implements.get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) protegidos de la función miembro para recuperar una referencia fuerte a su *esto* puntero. Tenga en cuenta que no hay ninguna necesidad de usar realmente el `strong_this` variable en el ejemplo de código anterior; basta con llamar a **get_strong** incrementos el C++/WinRT objeto del recuento de referencias y mantiene su implícita *este* puntero válido.

> [!IMPORTANT]
> Dado que **get_strong** es una función miembro de la **winrt::implements** struct de plantilla, puede llamar solo a partir de una clase que deriva directa o indirectamente **winrt::implements**, como un C++/WinRT clase. Para obtener más información acerca de cómo derivar desde **winrt::implements**, y ejemplos, vea [API autor con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Esto resuelve el problema que teníamos anteriormente cuando se llegó al paso 4. Incluso si desaparecen todas las demás referencias a la instancia de clase, la corrutina ha tomado la precaución de garantizar que sus dependencias son estables.

Si una referencia segura no es adecuada, a continuación, en su lugar, puede llamar a [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) para recuperar una referencia débil a *esto*. Confirma que puede recuperar una referencia segura antes de acceder a *esto*. Nuevamente, **get_weak** es una función miembro de la **winrt::implements** plantilla struct.

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

En el ejemplo anterior, la referencia débil no tenga la instancia de clase impiden la destrucción cuando no hay referencias fuertes permanecen. Pero le da una forma de comprobar si se puede adquirir una referencia segura antes de acceder a la variable de miembro.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acceso de forma segura la *esto* puntero con un delegado de control de eventos

### <a name="the-scenario"></a>El escenario

Para obtener información general sobre el control de eventos, consulte [controlar eventos mediante el uso de delegados en C / c++ / WinRT](handle-events.md).

La sección anterior resalta los posibles problemas de duración de las áreas de corrutinas y simultaneidad. Pero, si controla un evento con la función de miembro de un objeto, o desde una función lambda dentro de la función de miembro de un objeto, a continuación, se debe considerar la duración relativa del destinatario de eventos (el objeto que controla el evento) y el origen del evento (el objeto Provoca el evento). Echemos un vistazo a algunos ejemplos de código.

La lista de código siguiente primero define una sencilla **EventSource** (clase), lo que provoca un evento genérico que controla todos los delegados que se han agregado a ella. Se produce este evento de ejemplo usar el [ **Windows::Foundation::EventHandler** ](/uwp/api/windows.foundation.eventhandler) tipo de delegado, pero los problemas y soluciones aquí se aplican a tipos de todos los delegados.

A continuación, la **EventRecipient** clase proporciona un controlador para el **EventSource::Event** eventos en forma de una función lambda.

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

El patrón es que el destinatario de eventos tiene un controlador de eventos de la expresión lambda con dependencias en sus *esto* puntero. Cada vez que el destinatario del evento sobrevive el origen del evento, sobrevive esas dependencias. Y, en esos casos, que son comunes, el modelo funciona bien. Algunos de estos casos son evidentes, por ejemplo, cuando una página de interfaz de usuario controla un evento generado por un control que se encuentra en la página. La página sobrevive el botón&mdash;por lo tanto, el controlador también sobrevive el botón. Esto es válido siempre que el destinatario posea el origen (como un miembro de datos, por ejemplo), o cada vez que el destinatario y el origen estén relacionados o pertenezcan directamente a otro objeto. Si estás seguro de que tienes un caso en el que el controlador no sobrevivirá al objeto *this* del que depende, puedes capturar *this* de forma normal, sin tener en cuenta una duración fuerte o débil.

Pero todavía hay casos donde *esto* no sobreviven a su uso en un controlador (incluidos los controladores de finalización y el progreso de los eventos generados por operaciones y acciones asincrónicas), y es importante saber cómo lidiar con ellos.

- Si vas a crear una corrutina para implementar un método asincrónico, entonces es posible.
- En raras ocasiones con ciertos objetos del marco de la interfaz de usuario XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), por ejemplo) es posible, siempre que se haya finalizado el destinatario sin anular el registro del origen del evento.

### <a name="the-issue"></a>El problema

Esta versión de la **principal** función simula lo que sucede cuando se destruye el destinatario de eventos (quizás sale del ámbito) mientras que el origen del evento todavía está generando eventos.

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

Se destruye el destinatario de eventos, pero el controlador de eventos de la expresión lambda dentro de él todavía está suscrita a la **eventos** eventos. Cuando se produce ese evento, la expresión lambda intenta desreferenciar el *esto* puntero, que no es válido en ese momento. Por lo tanto, se produce una infracción de acceso del código en el controlador (o en la continuación de una corrutina) intentar utilizarlo.

> [!IMPORTANT]
> Si se produce una situación como esta, a continuación, tendrá que pensar en la duración de la *esto* objeto; y si capturado *esto* objeto sobrevive la captura. Si no es así, a continuación, capturarla con un fuerte o una referencia débil, mientras le mostramos a continuación.
>
> O&mdash;si tiene sentido para su escenario y si threading consideraciones hacen posible que incluso&mdash;, a continuación, otra opción consiste en revocar el controlador después de que el destinatario se realiza con el evento, o en el destructor del destinatario. Consulte [revocar un delegado registrado](handle-events.md#revoke-a-registered-delegate).

Se trata cómo registramos el controlador.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

La expresión lambda captura automáticamente las variables locales por referencia. Por lo tanto, para este ejemplo, de manera equivalente habríamos escrito esto.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

En ambos casos, nos estamos simplemente captura sin formato *esto* puntero. Y que no tiene ningún efecto en el recuento de referencia, por lo que nada impide que el objeto actual que se está destruyendo.

### <a name="the-solution"></a>La solución

La solución consiste en capturar una referencia segura. Una referencia fuerte *does* incrementar el recuento de referencias y *does* mantener activo el objeto actual. Solo tiene que declarar una variable de captura (llamado `strong_this` en este ejemplo) y se inicializa con una llamada a [ **implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function), que recupera una referencia fuerte a nuestro  *Esto* puntero.

> [!IMPORTANT]
> Dado que **get_strong** es una función miembro de la **winrt::implements** struct de plantilla, puede llamar solo a partir de una clase que deriva directa o indirectamente **winrt::implements**, como un C++/WinRT clase. Para obtener más información acerca de cómo derivar desde **winrt::implements**, y ejemplos, vea [API autor con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Incluso puede omitir la captura automática del objeto actual y obtener acceso al miembro de datos a través de la variable de captura en lugar de a través de implícito *esto*.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Si una referencia segura no es adecuada, a continuación, en su lugar, puede llamar a [ **implements::get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) para recuperar una referencia débil a *esto*. Confirma que todavía puede recuperar una referencia segura de ella antes de acceder a los miembros.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Si usa una función miembro como un delegado

Así como las funciones lambda, estos principios se aplican también al uso de una función miembro como el delegado. La sintaxis es diferente, así que veamos algún código. En primer lugar, este es el controlador de eventos de función miembro potencialmente inseguro, mediante un raw *esto* puntero.

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

Se trata de la manera estándar, convencional para hacer referencia a un objeto y su función miembro. Para que esto sea segura, también puede&mdash;a partir de la versión 10.0.17763.0 (Windows 10, versión 1809) del SDK de Windows&mdash;establecer un fuerte o una referencia débil en el punto donde se registra el controlador. En ese momento, el objeto de destinatario de eventos se sabe que continúan activos.

Para obtener una referencia fuerte, simplemente llame a [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) en lugar de sin formato *esto* puntero. C++ / c++ / WinRT garantiza que el delegado resultante contiene una referencia fuerte al objeto actual.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

Para obtener una referencia débil, llame a [ **get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function). C++ / c++ / WinRT garantiza que el delegado resultante contiene una referencia débil. En el último minuto y segundo plano, el delegado intenta resolver la referencia débil a una fuerte y solo llama a la función miembro, si es correcto.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Un ejemplo de referencia débil con **SwapChainPanel::CompositionScaleChanged**

En este ejemplo de código, usamos el [ **SwapChainPanel::CompositionScaleChanged** ](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) eventos por medio de otra ilustración de referencias débiles. El código registra un controlador de eventos mediante una expresión lambda que captura una referencia débil al destinatario.

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

En la cláusula de captura lamba, se crea una variable temporal que representa una referencia débil a *this*. En el cuerpo del lambda, si puede obtenerse una referencia fuerte a *this*, se llama a la función **OnCompositionScaleChanged**. De esta forma, *this* puede usarse con seguridad dentro de  **OnCompositionScaleChanged**.

## <a name="weak-references-in-cwinrt"></a>Referencias débiles en C++/WinRT

Anteriormente, hemos visto referencias débiles que se va a usar. En general, son buenas para interrumpir las referencias cíclicas. Por ejemplo, para la implementación del marco de interfaz de usuario basada en XAML nativa&mdash;debido al diseño de framework histórico&mdash;la referencia débil mecanismo en C++ / c++ / WinRT es necesario para controlar las referencias cíclicas. Fuera de XAML, sin embargo, es probable que no deba usar referencias débiles (not que no hay nada inherentemente XAML específica sobre ellos). En su lugar, más a menudo que no puede diseñar su propio C++ / c++ / WinRT APIs de tal manera que se evite la necesidad de referencias cíclicas y referencias débiles. 

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

Siempre que exista alguna otra referencia fuerte, la llamada [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) incrementa el recuento de referencias y devuelve la referencia fuerte al autor de la llamada.

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
* [función Implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [plantilla de función winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [winrt::no_weak_ref marcador struct](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [winrt::weak_ref struct template](/uwp/cpp-ref-for-winrt/weak-ref)
