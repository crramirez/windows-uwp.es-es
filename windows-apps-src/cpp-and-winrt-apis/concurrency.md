---
description: En este tema se muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.
title: Operaciones simultáneas y asincrónicas con C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, concurrency, async, asynchronous, asynchrony
ms.localizationpriority: medium
ms.openlocfilehash: cbabf38f41ae940f5c92944154638eae7016e043
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660095"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Operaciones simultáneas y asincrónicas con C++/WinRT

En este tema se muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operaciones asincrónicas y funciones "Async" de Windows Runtime

Cualquier API de Windows Runtime que tenga el potencial de tardar más de 50 milisegundos en completarse se implementa como una función asincrónica (con un nombre terminado en "Async"). La implementación de una función asincrónica inicia el trabajo en otro subproceso y se devuelve inmediatamente con un objeto que representa la operación asincrónica. Cuando se completa la operación asincrónica, dicho objeto devuelto contiene cualquier valor que resulte del trabajo. El espacio de nombres de Windows Runtime **Windows::Foundation** contiene cuatro tipos de objeto de la operación asincrónica.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) y
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada uno de estos tipos de la operación asincrónica se proyecta en un tipo correspondiente en el espacio de nombres **winrt::Windows::Foundation** de C++/WinRT. C++/WinRT también contiene una estructura adaptadora await interna. No la usas directamente, pero gracias a dicha estructura puedes escribir una instrucción `co_await` para esperar de forma cooperativa el resultado de cualquier función que devuelva uno de estos tipos de la operación asincrónica. Y puedes crear tus propias corrutinas que devuelvan estos tipos.

Un ejemplo de una función asincrónica de Windows es [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que devuelve un objeto de la operación asincrónica de tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Echemos un vistazo a algunas maneras &mdash;primero de bloqueo y luego de no bloqueo&mdash; de utilizar C++/WinRT para llamar a una API como esta.

## <a name="block-the-calling-thread"></a>Bloqueo del subproceso de llamada

El siguiente ejemplo de código recibe un objeto de la operación asincrónica desde **RetrieveFeedAsync** y llama a **get** en dicho objeto para bloquear el subproceso de llamada hasta que los resultados de la operación asincrónica estén disponibles.

Si deseas copiar y pegar este ejemplo directamente en el archivo de código fuente principal de un proyecto de **aplicación de consola Windows (C++/WinRT)** , establece primero **No utilizar encabezados precompilados** en las propiedades del proyecto.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

Una llamada a **get** ofrece una codificación cómoda y es ideal para aplicaciones de consola o subprocesos en segundo plano donde no quieres usar una corrutina por el motivo que sea. Pero no es simultánea ni asincrónica, por lo que no es apropiada para un subproceso de interfaz de usuario (y se activará una aserción en compilaciones no optimizadas si intentas usarla en una). Para evitar retener subprocesos del sistema operativo y que no puedan realizar otros trabajos útiles, necesitamos una técnica diferente.

## <a name="write-a-coroutine"></a>Escritura de una corrutina

C++/WinRT integra las corrutinas de C++ en el modelo de programación para proporcionar un modo natural de esperar un resultado de forma cooperativa. Puedes producir tu propia operación asincrónica de Windows Runtime escribiendo un corrutina. En el ejemplo de código siguiente, **ProcessFeedAsync** es la corrutina.

> [!NOTE]
> La función **get** existe en el tipo de proyección de C++/WinRT **winrt::Windows::Foundation::IAsyncAction**, para que puedas llamar a la función desde cualquier proyecto de C++/WinRT. No encontrarás la función enumerada como miembro de la interfaz [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) porque **get** no forma parte de la superficie de interfaz binaria de aplicaciones (ABI) del tipo de Windows Runtime real **IAsyncAction**.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

Una corrutina es una función que se puede suspender y reanudar. En la anterior corrutina **ProcessFeedAsync**, cuando se alcanza la instrucción `co_await`, la corrutina inicia de forma asincrónica la llamada **RetrieveFeedAsync**, después se suspende inmediatamente y devuelve el control al autor de la llamada (que es **main** en el ejemplo anterior). **main** puede seguir funcionando mientras la fuente se recupera y se imprime. Una vez hecho esto (cuando se completa la llamada **RetrieveFeedAsync**), la corrutina **ProcessFeedAsync** se reanuda en la próxima instrucción.

Puedes agregar una corrutina en otras corrutinas. O puedes llamar a **get** para bloquear y esperar a que se complete (y obtener el resultado, si lo hay). O bien, puedes pasarla a otro lenguaje de programación compatible con Windows Runtime.

También es posible controlar los eventos de acciones y operaciones asincrónicas completados o en progreso mediante delegados. Para información más detallada y ejemplos de código, consulta [Tipos de delegados para acciones y operaciones asincrónicas](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>Devolución de forma asincrónica de un tipo de Windows Runtime

En el siguiente ejemplo, encapsulamos una llamada a **RetrieveFeedAsync** de un URI específico para que nos dé una función **RetrieveBlogFeedAsync** que devuelve asincrónicamente [**SyndicationFeed** ](/uwp/api/windows.web.syndication.syndicationfeed).

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

En el ejemplo anterior, **RetrieveBlogFeedAsync** devuelve **IAsyncOperationWithProgress**, que tiene progreso y un valor devuelto. Podemos hacer otro trabajo mientras **RetrieveBlogFeedAsync** está haciendo el suyo y recuperando la fuente. A continuación, llamamos a **get** en este objeto de la operación asincrónica para bloquear, esperamos a que se complete y obtenemos los resultados de la operación.

Si vas a devolver asincrónicamente un tipo de Windows Runtime, debes devolver [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) o [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Se cualifica cualquier clase de tiempo de ejecución propia o de terceros o cualquier tipo que se pueda pasar a una función de Windows Runtime o desde ella (por ejemplo, `int` o **winrt::hstring**). El compilador te ayudará con un error "*debe ser de tipo WinRT*" si intentas usar uno de estos tipos de la operación asincrónica con un tipo que no sea de Windows Runtime.

Si una corrutina no tiene al menos una instrucción `co_await`, para que se cualifique como un corrutina, debe tener al menos una instrucción `co_return` o `co_yield`. Habrá casos en los que la corrutina pueda devolver un valor sin introducir ninguna asincronía y, por lo tanto, sin bloquear ni cambiar el contexto. Este ejemplo lo hace (la segunda y subsiguientes veces que se realice la llamada) almacenando en caché un valor.

```cppwinrt
winrt::hstring m_cache;

IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Devolución de forma asincrónica de un tipo que no sea de Windows Runtime

Si vas a devolver de forma asincrónica un tipo que *no* sea de Windows Runtime, debes devolver una biblioteca de patrones de procesamiento paralelo (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class). Te recomendamos utilizar **concurrency::task** porque te proporcionará un mejor rendimiento (y una mejor compatibilidad en el futuro) que **std::future**.

> [!TIP]
> Si incluyes `<pplawait.h>`, puedes usar **concurrency::task** como un tipo de corrutina.

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>Paso de parámetros

Para las funciones sincrónicas, debes usar los parámetros `const&` de manera predeterminada. Eso evitará la sobrecarga de copias (que implican recuento de referencias, lo que significa incrementos y decrementos entrelazados).

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

Pero puedes experimentar problemas si pasas un parámetro de referencia a una corrutina.

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

En una corrutina, la ejecución es sincrónica hasta el primer punto de suspensión, donde el control se devuelve al autor de la llamada. Cuando se reanuda la corrutina, puede haber ocurrido cualquier cosa en el valor de origen que hace referencia a un parámetro de referencia. Desde la perspectiva de la corrutina, un parámetro de referencia tiene un ciclo de vida incontrolado. Por tanto, en el ejemplo anterior, estamos seguros al acceder a *value* hasta `co_await`, pero no tras él. En caso de que el autor de llamada destruya *value*, intenta acceder a él dentro de la corrutina después de que eso provoque un daño de memoria. Tampoco podemos pasar con seguridad *value* a **DoOtherWorkAsync** si hay algún riesgo de que esa función se suspenda e intente usar *value* después de que se reanude.

Para que los parámetros sean seguros de usar tras suspenderse y reanudarse, las corrutinas deben usar paso-por-valor de manera predeterminada para garantizar que capturen por valor y eviten los problemas de ciclo de vida. Serán excepcionales los casos en los que puedas desviarte de esa orientación porque estés convencido de que sea seguro hacerlo.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

Pasar por valor requiere que el argumento resulte económico de mover o copiar; y normalmente es el caso de un puntero inteligente.

También es discutible que (a menos que quieras mover el valor) pasar objetos por valor const sea una buena práctica. No tendrá ningún efecto en el valor de origen desde el que estás haciendo una copia, pero hace que el propósito sea claro y sirve de ayuda si modificas accidentalmente la copia.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Consulta también [Matrices y vectores estándares](std-cpp-data-types.md#standard-arrays-and-vectors), que trata de cómo pasar un vector estándar a un destinatario asincrónico.

Si no puedes cambiar la firma de la corrutina, pero puedes cambiar la implementación, podrás realizar una copia local antes de la primera instrucción `co_await`.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

Si el valor `Param` es difícil de copiar, extrae solo los elementos necesarios antes de la primera instrucción `co_await`.

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>Acceso de forma segura al puntero *this* en una corrutina de miembro de clase

Consulta el artículo [Referencias fuertes y débiles de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine).

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Trabajo de descarga en el grupo de subprocesos de Windows

Una corrutina es una función como cualquier otra en la que se bloquea al autor de una llamada hasta que una función le devuelva la ejecución. Y la primera oportunidad para que se devuelva una corrutina es el primer operador `co_await`, `co_return` o `co_yield`.

Antes de hacer un trabajo unido al cálculo en una corrutina, deberás devolver la ejecución al autor de la llamada para que no se bloquee (en otras palabras, introducir un punto de suspensión). Si aún no lo estás haciendo mediante la aplicación de `co_await` en alguna otra operación, puedes aplicar `co_await` en la función [**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background). Esto devuelve el control al autor de la llamada y reanuda inmediatamente la ejecución en un subproceso del grupo de subprocesos.

El grupo de subprocesos que se va a usar en la implementación es el [grupo de subprocesos de Windows](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api) de bajo nivel, por lo que es eficaz en forma óptima.

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>Programación teniendo en cuenta la afinidad de subprocesos

Este escenario se expande en el anterior. Descargas determinado trabajo en el grupo de subprocesos, pero quieres mostrar el progreso en la interfaz de usuario (UI).

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

El código anterior lanza una excepción [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) porque se debe actualizar **TextBlock** desde el subproceso que lo creó, que es el subproceso de interfaz de usuario. Una solución es capturar el contexto del subproceso dentro del cual se llamó originalmente a nuestra corrutina. Para ello, creas una instancia de un objeto [**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context), realizas el trabajo en segundo plano y después aplicas `co_await` a **apartment_context** para volver al contexto de llamada.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

Mientras se llame a la corrutina anterior desde el subproceso de interfaz de usuario que creó **TextBlock**, esta técnica funciona. Habrá muchos casos en la aplicación en los que estés seguro de ello.

Para una solución más general de actualización de la interfaz de usuario, que incluye los casos en los que no estés seguro del subproceso que realiza la llamada, puedes aplicar `co_await` a la función [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) para cambiar a un determinado subproceso en primer plano. En el siguiente ejemplo de código, especificamos el subproceso de primer plano, pasando el objeto del distribuidor asociado a **TextBlock** (mediante el acceso a su propiedad [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). La implementación de **winrt::resume_foreground** llama a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) en ese objeto de distribuidor para ejecutar el trabajo que viene después de él en la corrutina.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Contextos de ejecución, reanudación y cambio de corrutina

En términos generales, después de un punto de suspensión en una corrutina, el subproceso de ejecución original puede desaparecer y se puede producir la reanudación en cualquier subproceso (en otras palabras, cualquier subproceso puede llamar al método **Completed** para la operación asincrónica).

No obstante, si aplicas `co_await` a cualquiera de los cuatro tipos de la operación asincrónica de Windows Runtime (**IAsyncXxx**), C++/WinRT captura el contexto de llamada en el punto en el que has aplicado `co_await`. Y garantiza que todavía estarás en ese contexto cuando se reanude la continuación. Para ello, C++/ WinRT comprueba si ya estás en el contexto de llamada y, si no, te cambiará a él. Si estabas en un subproceso de contenedor uniproceso (STA) antes de `co_await`, estarás en el mismo subproceso posteriormente; si estabas en un subproceso de contenedor multiproceso (MTA) antes de `co_await`, estarás en otro igual después.

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

La razón por la que puedes confiar en este comportamiento es que C++/WinRT proporciona código para adaptar estos tipos de la operación asincrónica de Windows Runtime a la compatibilidad del lenguaje de corrutinas C++ (estos fragmentos de código se denominan adaptadores de espera). El resto de tipos que admite await en C++/WinRT simplemente son contenedores del grupo de subprocesos o aplicaciones auxiliares, por lo que se completan en el grupo de subprocesos.

```cppwinrt
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

Si aplicas `co_await` a algún otro tipo &mdash;incluso dentro una implementación de corrutina de C++/WinRT&mdash;, otra biblioteca proporcionará los adaptadores, y tendrás que entender lo que hacen esos adaptadores en términos de reanudación y contextos.

Para mantener los cambios de contexto al mínimo, puedes usar algunas de las técnicas que ya hemos visto en este tema. Vamos a ver algunas ilustraciones para hacer esto. En el ejemplo de pseudocódigo siguiente, mostramos el esquema de un controlador de eventos que llama a una API de Windows Runtime para cargar una imagen, la coloca directamente en un subproceso en segundo plano para procesar dicha imagen y, después, vuelve al subproceso de interfaz de usuario para mostrar la imagen en la interfaz de usuario.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

En este escenario, existe cierta ineficacia en torno a la llamada a **StorageFile::OpenAsync**. Hay un cambio de contexto necesario para un subproceso en segundo plano (de modo que el controlador pueda devolver la ejecución al autor de la llamada), tras cuya reanudación C++/WinRT restaura el contexto del subproceso de interfaz de usuario. Pero, en este caso, no es necesario estar en el subproceso de interfaz de usuario hasta que vayamos a actualizar la interfaz de usuario. A cuántas más API de Windows Runtime llamemos *antes* de nuestra llamada a **winrt::resume_background**, mayor será el número de cambios de contexto innecesarios en los que incurramos. La solución es no llamar a *ninguna* API de Windows Runtime hasta entonces. Muévelas todas después de **winrt::resume_background**.

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

Si deseas hacer algo más avanzado,puedes escribir tus propios adaptadores await. Por ejemplo, si quieres que `co_await` se reanude en el mismo subproceso en el que se completa la acción asincrónica (por lo tanto, no hay ningún cambio de contexto), podrías empezar escribiendo adaptadores await similares a los que se muestran a continuación.

> [!NOTE]
> El ejemplo de código siguiente se proporciona solo con fines de formación; es para ayudarte a comprender cómo funcionan los adaptadores await. Si deseas utilizar esta técnica en tu propio código base, te recomendamos que desarrolles y pruebes tus propias estructuras adaptadoras await. Por ejemplo, podrías escribir **complete_on_any**, **complete_on_current** y **complete_on(dispatcher)** . Considera también la posibilidad de hacer que sean plantillas que toman el tipo **IAsyncXxx** como parámetro de plantilla.

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

Para entender cómo se utilizan los adaptadores await **no_switch**, primero debes saber que cuando el compilador de C++ encuentra una expresión `co_await`, busca funciones llamadas **await_ready**, **await_suspend** y **await_resume**. La biblioteca de C++/WinRT proporciona estas funciones para que obtengas un comportamiento razonable, de manera predeterminada, como en este ejemplo.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Para usar los adaptadores await **no_switch**, cambia el tipo de esa expresión `co_await` de **IAsyncXxx** a **no_switch**, del modo siguiente.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

En lugar de buscar las tres funciones **await_xxx** que coincidan con **IAsyncXxx**, el compilador de C++ busca funciones que coincidan con **no_switch**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Cancelación de una operación asincrónica y devoluciones de llamadas de cancelación

Las características de Windows Runtime para la programación asincrónica permiten cancelar una operación o acción asincrónica en proceso. Este es un ejemplo que llama a [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) para recuperar una colección potencialmente grande de archivos y almacena el objeto resultante de la operación asincrónica en un miembro de datos. El usuario tiene la opción de cancelar la operación.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

En lo que respecta a la implementación de la cancelación, vamos a comenzar por un ejemplo sencillo.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

Si ejecutas el ejemplo anterior, verás que **ImplicitCancellationAsync** imprime un mensaje por segundo durante tres segundos, tras lo cual finaliza automáticamente como resultado de la cancelación. Esto funciona porque, al encontrar una expresión `co_await`, una corrutina comprueba si se ha cancelado. Si lo ha hecho, se genera un cortocircuito; y si no, se suspende de la forma habitual.

La cancelación, por supuesto, se puede producir mientras la corrutina está suspendida. Solo cuando se reanuda la corrutina, o llega a otro `co_await`, comprobará la cancelación. El problema es una latencia potencialmente demasiado general en respuesta a la cancelación.

Por lo tanto, otra opción es sondear explícitamente la cancelación dentro de la corrutina. Actualiza el ejemplo anterior con el código de la lista siguiente. En este nuevo ejemplo, **ExplicitCancellationAsync** recupera el objeto devuelto por la función [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) y lo usa para comprobar periódicamente si se ha cancelado la corrutina. Mientras no se cancele, la corrutina se repite indefinidamente; una vez cancelada, el bucle y la función se salen de la forma habitual. El resultado es el mismo que en el ejemplo anterior, pero aquí la salida se realiza de forma explícita y bajo control.

```cppwinrt
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

Al esperar en **winrt::get_cancellation_token**, se recupera un token de cancelación con conocimiento de la interfaz **IAsyncAction** que la corrutina está produciendo en tu nombre. Puedes usar el operador de llamada de la función en ese token para consultar el estado de una cancelación, básicamente, sondear en busca de una cancelación. Si vas a realizar una operación unida al cálculo o recorrer en iteración una colección de gran tamaño, entonces esta es una técnica razonable.

### <a name="register-a-cancellation-callback"></a>Registro de una devolución de llamada de cancelación

La cancelación de Windows Runtime no fluye automáticamente a otros objetos asincrónicos. Pero &mdash;desde que se introdujo en la versión 10.0.17763.0 (Windows 10, versión 1809) del SDK de Windows&mdash;, puedes registrar una devolución de llamada de cancelación. Se trata de un enlace preventivo por el que se puede propagar la cancelación y se hace posible la integración con bibliotecas de simultaneidad existentes.

En el ejemplo de código siguiente, **NestedCoroutineAsync** realiza el trabajo, pero no tiene una lógica de cancelación especial. **CancellationPropagatorAsync** es esencialmente un contenedor de la corrutina anidada; el contenedor reenvía una cancelación de manera preventiva.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([=]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync** registra una función lambda para su propia devolución de llamada de cancelación y, después, espera (se suspende) hasta que finalice el trabajo anidado. Cuando se cancela o si está cancelado **CancellationPropagatorAsync**, la cancelación se propaga a la corrutina anidada. No hay ninguna necesidad de sondear en busca de una cancelación, ni tampoco la cancelación se bloquea de forma indefinida. Este mecanismo es lo suficientemente flexible como para que puedas usarlo para interoperar con una biblioteca de corrutinas o de simultaneidad que no sabe nada de C++/WinRT.

## <a name="reporting-progress"></a>Notificación del progreso

Si la corrutina devuelve [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) o [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), puedes recuperar el objeto devuelto por la función [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) y usarlo para notificar el progreso a un controlador de progreso. Aquí tienes un ejemplo de código.

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> No es correcto implementar más de un *controlador de finalización* para una operación o acción asincrónica. Puedes tener un solo delegado para su evento completado o bien puedes aplicar `co_await`. Si tienes ambos, se producirá un error en el segundo. Uno de los dos tipos de controladores de finalización siguientes es adecuado, pero no ambos para el mismo objeto asincrónico.

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

Para más información sobre los controladores de finalización, consulta [Tipos de delegados para acciones y operaciones asincrónicas](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Desencadenamiento y olvido

A veces, tienes una tarea que se puede realizar simultáneamente con otro trabajo y no necesitas esperar a que la tarea se complete (ningún otro trabajo depende de él), ni tampoco que devuelva un valor. En ese caso, puedes lanzar la tarea y olvidarte de ella. Puedes hacerlo escribiendo una corrutina cuyo tipo de valor devuelto sea [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (en lugar de uno de los tipos de la operación asincrónica de Windows Runtime o **concurrency::task**).

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

La estructura **winrt::fire_and_forget** también resulta útil como el tipo de valor devuelto del controlador de eventos cuando necesites realizar operaciones asincrónicas en él. Este es un ejemplo (consulta también [Referencias fuertes y débiles de C++/WinRT](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)).

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

El primer argumento (el *remitente*) queda sin nombre, porque no se utiliza nunca. Por este motivo, estamos seguros si lo dejamos como una referencia. Pero observa que *args* se pasa por valor. Consulta la sección [Paso de parámetros](#parameter-passing) más arriba.

## <a name="important-apis"></a>API importantes
* [Clase concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [Interfaz IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [Interfaz IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [Interfaz IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [Interfaz IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Clase SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Temas relacionados
* [Control de eventos mediante delegados en C++/WinRT](handle-events.md)
* [Tipos de datos de C++ estándar y C++/WinRT](std-cpp-data-types.md)
