---
description: Windows Runtime es un sistema con recuento de referencias y en este tipo de sistemas es importante conocer el significado de referencias fuertes y débiles y la diferencia entre ellas.
title: Referencias fuertes y débiles de C++/WinRT
ms.date: 05/16/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, strong, weak, reference
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c8ca914737698c22d52657d20ee655d20491b3e8
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2020
ms.locfileid: "88180770"
---
# <a name="strong-and-weak-references-in-cwinrt"></a>Referencias fuertes y débiles de C++/WinRT

Windows Runtime es un sistema con recuento de referencias y en este tipo de sistemas es importante conocer el significado de referencias fuertes y débiles y la diferencia entre ellas (y referencias que no son ninguna de ellas, como el puntero implícito *this*). Como verás en este tema, saber cómo administrar correctamente estas referencias puede significar la diferencia entre un sistema confiable que funciona sin problemas y otro que se bloquea de forma impredecible. Al proporcionar funciones auxiliares que cuentan con compatibilidad completa en la proyección del lenguaje, [C+++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) se encuentra a medio camino en su trabajo de crear sistemas más complejos de forma sencilla y correcta.

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acceso de forma segura al puntero *this* en una corrutina de miembro de clase

Para obtener más información sobre las corrutinas y ejemplos de código, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

En la lista de código siguiente se muestra un ejemplo típico de una corrutina que es una función miembro de una clase. Puedes copiar y pegar este ejemplo en los archivos especificados en un nuevo proyecto de la **aplicación de consola Windows (C++/WinRT)**.

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

**MyClass::RetrieveValueAsync** pasa algún tiempo trabajando, y, finalmente, devuelve una copia del miembro de datos `MyClass::m_value`. Una llamada a **RetrieveValueAsync** hace que se cree un objeto asincrónico, y ese objeto tiene un puntero *this* implícito (a través del cual, eventualmente, se accede a `m_value`).

Recuerde que, en una corrutina, la ejecución es sincrónica hasta el primer punto de suspensión, donde el control se devuelve al autor de la llamada. En **RetrieveValueAsync**, la primera `co_await` es el primer punto de suspensión. Para cuando se reanude la corrutina (unos cinco segundos más tarde, en este caso), podría haber ocurrido algo implícito con el obtjeto *this* del puntero que usamos para obtener acceso a `m_value`.

Esta es la secuencia completa de eventos.

1. En **main**, se crea una instancia de **MyClass** (`myclass_instance`).
2. Se crea el objeto `async`, que señala (mediante el puntero *this*) a `myclass_instance`.
3. La función **winrt::Windows::Foundation::IAsyncAction::get** alcanza su primer punto de suspensión, se bloquea durante unos segundos y luego devuelve el resultado **RetrieveValueAsync**.
4. **RetrieveValueAsync** devuelve el valor de `this->m_value`.

El paso 4 es seguro solo mientras *este* siga siendo válido.

Pero, ¿qué ocurre si se destruye la instancia de clase antes de que finalice la operación asincrónica? Hay todo tipo de formas en las que la instancia de clase podría estar fuera del ámbito de aplicación antes de que el método asíncrono se haya completado. Aún así, podemos simularla al establecer la instancia de clase en `nullptr`.

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

Después del punto en el que se destruye la instancia de clase, parece que no se hace referencia directamente a ella de nuevo. Pero por supuesto el objeto asincrónico tiene un puntero *this* a ella e intenta usarlo para copiar el valor almacenado dentro de la instancia de clase. La corrutina es una función miembro, y espera que pueda usar su puntero *this* con impunidad.

Con este cambio en el código, nos encontramos con un problema en el paso 4, porque se ha destruido la instancia de clase y el puntero *this* ya no es válido. Tan pronto como el objeto asincrónico intenta obtener acceso a la variable dentro de la instancia de clase, se bloqueará (o hará algo totalmente indefinido).

La solución consiste en proporcionar a la operación asincrónica (la corrutina) su propia referencia fuerte a la instancia de clase. Como está escrito actualmente, la corrutina contiene efectivamente un puntero *this* básico a la instancia de clase; pero eso no es suficiente para mantener activa la instancia de clase.

Para mantenerla activa, cambie la implementación de **RetrieveValueAsync** por la que se muestra a continuación.

```cppwinrt
IAsyncOperation<winrt::hstring> RetrieveValueAsync()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    co_await 5s;
    co_return m_value;
}
```

Una clase C++/WinRT se deriva directa o indirectamente de la plantilla [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). Por eso, el objeto C++/WinRT puede llamar a su función miembro protegida [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) para recuperar una referencia fuerte al puntero *this*. Ten en cuenta que no hay necesidad de usar la variable `strong_this` en el ejemplo de código anterior; simplemente al llamar a **get_strong**, se incrementa el recuento de referencias del objeto C++/WinRT, y mantiene su puntero *this* implícito válido.

> [!IMPORTANT]
> Dado que **get_trong** es una función miembro de la plantilla de estructura **winrt::implements**, puedes llamarla solo desde una clase que derive directa o indirectamente de **winrt::implements**, como por ejemplo una clase C++/WinRT. Para más información acerca de cómo derivar desde **winrt::implements** y ver ejemplos, consulta [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Esto resuelve el problema que teníamos anteriormente cuando llegamos al paso 4. Incluso si todas las demás referencias a la instancia de clase desaparecen, la corrutina ha tomado la precaución de garantizar que sus dependencias sean estables.

Si una referencia fuerte no es apropiada, entonces puedes llamar a [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) para recuperar una referencia débil al puntero *this*. Solo tienes que confirmar que puedes recuperar una referencia fuerte antes de acceder al puntero *this*. De nuevo, **get_weak** es una función miembro de la plantilla de estructura **winrt::implements**.

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

En el ejemplo anterior, la referencia débil no impide que la instancia de clase se destruya cuando no quedan referencias fuertes. Pero le da una manera de comprobar si se puede adquirir una referencia fuerte antes de acceder a la variable miembro.

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acceso de forma segura al puntero *this* con un delegado de control de eventos

### <a name="the-scenario"></a>El escenario

Para obtener información general sobre el control de eventos, consulta [Control de eventos mediante delegados en C++/WinRT](handle-events.md).

En la sección anterior se resaltan los posibles problemas de duración de las áreas de corrutinas y simultaneidad. Sin embargo, si controlas un evento con la función miembro de un objeto o desde dentro de una función lambda dentro de la función miembro de un objeto, debes tener en cuenta las duraciones relativas del destinatario del evento (el objeto que controla el evento) y el origen del evento (el objeto que genera el evento). Echemos un vistazo a algunos ejemplos de código.

En la lista de códigos que aparece a continuación se define en primer lugar una clase sencilla **EventSource**, que provoca un evento genérico que controlan los delegados que se hayan agregado a él. Este ejemplo de evento utiliza el tipo de delegado [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler), pero los problemas y soluciones se aplican aquí a todos los tipos de delegado.

Después, la clase **EventRecipient** proporciona un controlador para el evento **EventSource::Event** en forma de una función lambda.

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

El patrón es que el receptor del evento tiene un controlador de eventos lambda con dependencias en su puntero *this*. Cada vez que el destinatario del evento sobrevive al origen del evento, sobrevive a esas dependencias. Y en esos casos, que son comunes, el patrón funciona bien. Algunos de estos casos son evidentes, por ejemplo, cuando una página de interfaz de usuario controla un evento generado por un control que se encuentra en la página. La página sobrevive al botón, por lo que el controlador también lo sobrevive. Esto es válido siempre que el destinatario posea el origen (como un miembro de datos, por ejemplo), o cada vez que el destinatario y el origen estén relacionados o pertenezcan directamente a otro objeto.

Cuando estés seguro de que tienes un caso en el que el controlador no sobrevivirá al objeto *this* del que depende, puedes capturar *this* de forma normal, sin tener en cuenta una duración segura o no segura.

Pero todavía hay casos donde *this* no sobrevive a su uso en un controlador (incluidos los controladores para eventos de finalización y progreso generados por acciones y operaciones asincrónicas) y es importante saber cómo lidiar con ellos.

- Cuando un origen de eventos genera sus eventos *sincrónicamente*, puedes revocar el controlador y estar seguro de que no recibirás más eventos. Pero para los eventos asincrónicos, incluso después de la revocación (y especialmente al revocar dentro del destructor), un evento en curso podría alcanzar el objeto después de que se haya iniciado la destrucción. Buscar un lugar para cancelar la suscripción antes de la destrucción puede mitigar el problema, pero sigue leyendo para conocer una solución más estable.
- Si vas a crear una corrutina para implementar un método asincrónico, entonces es posible.
- En raras ocasiones con ciertos objetos del marco de la interfaz de usuario XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), por ejemplo) es posible, siempre que se haya finalizado el destinatario sin anular el registro del origen del evento.

### <a name="the-issue"></a>El problema

Esta versión de la función **main** simula lo que sucede cuando el destinatario del evento se destruye (tal vez se salga de ámbito) mientras el origen del evento sigue generando eventos.

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

Se destruye el destinatario del evento, pero el controlador del evento lambda dentro de él todavía sigue suscrito al evento **Event**. Cuando se produce ese evento, la expresión lambda intenta desreferenciar el puntero *this*, que no es válido en ese momento. Por lo tanto, se produce una infracción de acceso desde el código en el controlador (o en la continuación de una corrutina) que intenta utilizarlo.

> [!IMPORTANT]
> Si encuentras una situación como esta, tendrás que pensar en la duración del objeto *this*; y si el objeto *this* capturado sobrevive o no a la captura. Si no es así, captúralo con una referencia fuerte o débil, como te mostramos a continuación.
>
> O bien&mdash;si tiene sentido para tu escenario y si las consideraciones de subprocesos lo hacen posible&mdash;, otra opción es revocar el controlador una vez hecho el destinatario con el evento o en el destructor del destinatario. Consulta [Revocación de un delegado registrado](handle-events.md#revoke-a-registered-delegate).

Así es cómo registramos el controlador.

```cppwinrt
event_source.Event([&](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

La expresión lambda captura automáticamente las variables locales por referencia. Así que, para este ejemplo, podríamos haber escrito esto de forma equivalente.

```cppwinrt
event_source.Event([this](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

En ambos casos, solo estamos capturando el puntero *this* básico. Y esto no tiene ningún efecto en el recuento de referencias, por lo que nada impide que el objeto actual se destruya.

### <a name="the-solution"></a>La solución

La solución consiste en capturar una referencia fuerte (o, como veremos, una referencia débil si es más adecuada). Una referencia fuerte *incrementa* el recuento de referencias y *mantiene* activo el objeto actual. Solo tienes que declarar una variable de captura (llamada `strong_this` en este ejemplo) e inicializarla con una llamada a [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function), que recupera una referencia fuerte a nuestro puntero *this*.

> [!IMPORTANT]
> Dado que **get_trong** es una función miembro de la plantilla de estructura **winrt::implements**, puedes llamarla solo desde una clase que derive directa o indirectamente de **winrt::implements**, como por ejemplo una clase C++/WinRT. Para más información acerca de cómo derivar desde **winrt::implements** y ver ejemplos, consulta [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
event_source.Event([this, strong_this { get_strong()}](auto&& ...)
{
    std::wcout << m_value.c_str() << std::endl;
});
```

Incluso puedes omitir la captura automática del objeto actual y acceder al miembro de datos mediante la variable de captura en lugar de mediante la variable *this* implícita.

```cppwinrt
event_source.Event([strong_this { get_strong()}](auto&& ...)
{
    std::wcout << strong_this->m_value.c_str() << std::endl;
});
```

Si una referencia fuerte no es apropiada, entonces puedes llamar a [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) para recuperar una referencia débil al puntero *this*. Una referencia débil *no* mantiene activo el objeto actual. Por tanto, solo tienes que confirmar que todavía puedes recuperar una referencia fuerte desde la referencia débil antes de acceder a los miembros.

```cppwinrt
event_source.Event([weak_this{ get_weak() }](auto&& ...)
{
    if (auto strong_this{ weak_this.get() })
    {
        std::wcout << strong_this->m_value.c_str() << std::endl;
    }
});
```

Si capturas un puntero sin formato, tienes que asegurarte de mantener activo el objeto al que apuntas.

### <a name="if-you-use-a-member-function-as-a-delegate"></a>Si usas una función miembro como delegado

Además de las funciones lambda, estos principios también se aplican al uso de una función miembro como tu delegado. La sintaxis es diferente, así que veamos algún código. En primer lugar, este es el controlador de eventos de la función miembro potencialmente inseguro, que utiliza un puntero *this* básico.

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

Se trata de la manera estándar y convencional de hacer referencia a un objeto y a su función miembro. Para que sea seguro, puedes (a partir de la versión 10.0.17763.0 [Windows 10, versión 1809] de Windows SDK) establecer una referencia fuerte o débil en el punto en el que está registrado el controlador. En ese momento, se sabe que el objeto de destinatario del evento sigue activo.

Para obtener una referencia fuerte, solo tienes que llamar a [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) en lugar al puntero *this* básico. C++/WinRT garantiza que el delegado resultante tenga contiene una referencia fuerte al objeto actual.

```cppwinrt
event_source.Event({ get_strong(), &EventRecipient::OnEvent });
```

La captura de una referencia fuerte significa que el objeto será pasible de ser destruido solo después de que se haya anulado el registro del controlador y se hayan devuelto todas las devoluciones de llamada pendientes. Sin embargo, esa garantía solo es válida en el momento en que se genera el evento. Si el controlador de eventos es asincrónico, tendrás que dar a la corrutina una referencia fuerte a la instancia de clase antes del primer punto de suspensión (para más detalles y el código, consulta la sección [Acceso de forma segura al puntero *this* en una corrutina de miembro de clase](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) anteriormente en este tema). Pero esto crea una referencia circular entre el origen del evento y tu objeto, por lo que debes interrumpirlo explícitamente revocando el evento.

Para obtener una referencia débil, llama a [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function). C++/ WinRT garantiza que el delegado resultante contiene una referencia débil. En el último momento, y en segundo plano, el delegado intenta resolver la referencia débil a una fuerte y solo llama a la función miembro si es correcto.

```cppwinrt
event_source.Event({ get_weak(), &EventRecipient::OnEvent });
```

Si el delegado *llama* a la función miembro, C++/WinRT mantendrá el objeto activo hasta que vuelva el controlador. Sin embargo, si el controlador es asincrónico, se devuelve en los puntos de suspensión, por lo que tendrás que dar a la corrutina una referencia fuerte a la instancia de clase antes del primer punto de suspensión. De nuevo, para más información, consulta la sección [Acceso de forma segura al puntero *this* en una corrutina de miembro de clase](#safely-accessing-the-this-pointer-in-a-class-member-coroutine) anteriormente en este tema.

### <a name="a-weak-reference-example-using-swapchainpanelcompositionscalechanged"></a>Ejemplo de referencia débil con **SwapChainPanel::CompositionScaleChanged**

En este ejemplo de código, usamos el evento [**SwapChainPanel::CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) a modo de otra ilustración de referencias débiles. El código registra un controlador de eventos mediante un lambda que captura una referencia débil al destinatario.

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

## <a name="weak-references-in-cwinrt"></a>Referencias débiles de C++/WinRT

Anteriormente, hemos visto que se utilizaban referencias débiles. En general, son buenas para interrumpir las referencias cíclicas. Por ejemplo, para la implementación nativa del marco de la interfaz de usuario basada en XAML (debido al diseño histórico del marco), el mecanismo de referencia débil en C++/WinRT es necesario para controlar referencias cíclicas. Sin embargo, fuera de XAML es probable que no necesites usar referencias débiles (no es que haya algo inherentemente específico de XAML en ellas). Más bien, la mayoría de las veces deberías poder diseñar tu propia API de C++/WinRT de modo que se evite la necesidad de referencias cíclicas y referencias débiles. 

Para cualquier tipo en particular que declares, a C++/WinRT no le resulta inmediatamente evidente saber si se necesitan referencias débiles o cuándo se necesitan. De este modo, C++/WinRT proporciona soporte de referencia débil automáticamente en la plantilla de estructura [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), desde la que tus propios tipos C++/WinRT derivan directa o indirectamente. Es un sistema de pago pay-to-play, es decir, no te cuesta nada a menos que en realidad se consulte tu objeto para [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource). Y puedes [optar por no recibir tal soporte](#opting-out-of-weak-reference-support) explícitamente.

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

Crear una referencia débil no afecta el recuento de referencias en el propio objeto; simplemente hace que se asigne un bloque de control. Dicho bloque de control se encarga de implementar la semántica de la referencia débil. Puedes intentar promover la referencia débil a una referencia fuerte y, si se realiza correctamente, utilizarla.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

Siempre que exista alguna otra referencia fuerte, la llamada [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weak_refget-function) incrementa el recuento de referencias y devuelve la referencia fuerte al autor de la llamada.

### <a name="opting-out-of-weak-reference-support"></a>Optar por no recibir el soporte de referencia débil
El soporte de referencia débil es automático. Pero puedes optar por no recibir explícitamente tal soporte pasando la estructura del marcador [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) como un argumento de plantilla a tu clase base.

Si derivas directamente de **winrt::implements**.

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

No importa dónde aparezca la estructura del marcador dentro del paquete de parámetro variádicas. Si solicitas una referencia débil para un tipo que has optado por no recibir, el compilador te ayudará con "*This is only for weak ref support*" (Esto es solo para soporte de referencia débil).

## <a name="important-apis"></a>API importantes
* [Función winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [Plantilla de función winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [Estructura de marcador winrt::no_weak_ref](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Plantilla de estructura winrt::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)
