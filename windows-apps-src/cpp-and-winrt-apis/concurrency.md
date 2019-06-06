---
description: Este tema muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.
title: Operaciones simultáneas y asincrónicas con C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, simultaneidad, async, asincrónico, asincronía
ms.localizationpriority: medium
ms.openlocfilehash: 910d7a7ca2aaebac6dd462d7104b26a989cf8814
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721657"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Operaciones simultáneas y asincrónicas con C++/WinRT

En este tema se muestra las maneras en que ambos pueden creación y consumen objetos asincrónicos de Windows en tiempo de ejecución con [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operaciones asincrónicas y funciones "Async" de Windows Runtime

Cualquier API de Windows Runtime que tenga el potencial de tardar más de 50 milisegundos en completarse se implementa como una función asincrónica (con un nombre terminado en "Async"). La implementación de una función asincrónica inicia el trabajo en otro subproceso y regresa inmediatamente con un objeto que representa la operación asincrónica. Cuando se completa la operación asincrónica, dicho objeto devuelto contiene cualquier valor que resultase del trabajo. El espacio de nombres de Windows Runtime **Windows::Foundation** contiene cuatro tipos de objetos de la operación asincrónica.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_), y
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada uno de estos tipos de la operación asincrónica se proyecta en un correspondiente tipo en el espacio de nombres **winrt::Windows::Foundation** de C++/WinRT. C++ / WinRT también contiene una estructura adaptadora await interna. No usa directamente, pero gracias a esa estructura, puede escribir un `co_await` instrucción para esperar el resultado de cualquier función que devuelve uno de estos tipos de operación asincrónica de forma cooperativa. Y puedes crear tus propias corrutinas que devuelvan estos tipos.

Un ejemplo de una función asincrónica de Windows es [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que devuelve un objeto de la operación asincrónica de tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Echemos un vistazo a algunas formas&mdash;primer bloqueo y, a continuación, sin bloqueo&mdash;del uso de C++ / c++ / WinRT para llamar a una API como que.

## <a name="block-the-calling-thread"></a>Bloquear el subproceso de llamada

El siguiente ejemplo de código recibe un objeto de la operación asincrónica desde **RetrieveFeedAsync** y llama a **get** en dicho objeto para bloquear el subproceso de llamada hasta que los resultados de la operación asincrónica estén disponibles.

Si desea copiar y pegar este ejemplo directamente en el archivo de código fuente principal de un **aplicación de consola de Windows (C++/WinRT)** proyecto y, a continuación, primer conjunto **no utilizar encabezados precompilados** en el proyecto Propiedades.

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

Una llamada a **get** ofrece una codificación cómoda y es ideal para aplicaciones de consola o subprocesos en segundo plano donde no querrás usar una corrutina por cualquier motivo. Pero no es simultánea ni asincrónica, por lo que no es apropiado para un subproceso de interfaz de usuario (y una aserción se activará en compilaciones no optimizados si intentas usarla en una). Para evitar retener subprocesos del sistema operativo desde otro trabajo útil, necesitamos una técnica diferente.

## <a name="write-a-coroutine"></a>Escribir una corrutina

C++/WinRT integra las corrutinas de C++ en el modelo de programación para proporcionar una forma natural de esperar de forma cooperativa un resultado. Puedes producir tu propia operación asincrónica de Windows Runtime escribiendo un corrutina. En el ejemplo de código siguiente, **ProcessFeedAsync** es la corrutina.

> [!NOTE]
> El **obtener** función existe en C++ / c++ / tipo de proyección de WinRT **winrt::Windows::Foundation::IAsyncAction**, por lo que puede llamar a la función desde C++ / c++ / WinRT proyecto. No encontrará la función aparece como un miembro de la [ **IAsyncAction** ](/uwp/api/windows.foundation.iasyncaction) interfaz porque **obtener** no forma parte de la superficie de aplicación (ABI) de interfaz binaria de la tipo de Windows en tiempo de ejecución real **IAsyncAction**.

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

Un corrutina es una función que se suspende y reanuda. En la anterior corrutina **ProcessFeedAsync**, cuando se alcanza la instrucción `co_await`, la corrutina inicia de forma asincrónica la llamada **RetrieveFeedAsync**, después se suspende inmediatamente y devuelve el control al autor de la llamada (que es **main** en el ejemplo anterior). **main** puede seguir funcionando mientras la fuente se recupera y se imprime. Una vez hecho esto (cuando se completa la llamada **RetrieveFeedAsync**), la corrutina **ProcessFeedAsync** se reanuda en la próxima instrucción.

Puedes agregar un corrutina en las otras corrutinas. O puedes llamar a **get** para bloquear y esperar a que se complete (y obtener el resultado, si lo hay). O bien, puedes pasar a otro lenguaje de programación compatible con Windows Runtime.

También es posible controlar los eventos de acciones y operaciones asincrónicas completados o en progreso usando delegados. Para obtener información detallada y ejemplos de código, consulta [Delegar tipos para acciones y operaciones asincrónicas](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="asychronously-return-a-windows-runtime-type"></a>Devolver de forma asincrónica un tipo de Windows Runtime

En el siguiente ejemplo, encapsulamos una llamada a **RetrieveFeedAsync** de un URI específico para que nos dé una función **RetrieveBlogFeedAsync** que devuelve asincrónicamente un [**SyndicationFeed** ](/uwp/api/windows.web.syndication.syndicationfeed).

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

En el ejemplo anterior, **RetrieveBlogFeedAsync** devuelve un **IAsyncOperationWithProgress**, que tiene progreso y un valor devuelto. Podemos hacer otro trabajo mientras **RetrieveBlogFeedAsync** está haciendo el suyo y recuperando la fuente. A continuación, llamamos a **get** en este objeto de la operación asincrónica para bloquear, esperar a que se complete y obtener los resultados de la operación.

Si vas a devolver asincrónicamente un tipo de Windows Runtime, debes devolver un [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) o un [**IAsyncOperationWithProgress&lt;TResult, TProgress &gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Cualquier clase de tiempo de ejecución propia o de terceros cualifica o cualquier tipo que se pueda pasar a o desde una función de Windows Runtime (por ejemplo, `int` o **winrt::hstring**). El compilador te ayudará con un error "*debe ser de tipo WinRT*" si intentas usar uno de estos tipos de la operación asincrónica con un tipo que no sea de Windows Runtime.

Si una corrutina no tiene al menos una declaración `co_await`, para que cualifique como un corrutina, debe tener al menos una declaración `co_return` o `co_yield`. Habrá casos donde la corrutina pueda devolver un valor sin introducir cualquier asincronía y, por lo tanto, sin bloquear ni cambiar de contexto. Este es un ejemplo que hace que (se llame a los tiempos segundo y subsiguientes) al almacenar en caché un valor.

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

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Devolver de forma asincrónica un tipo que no sea de Windows Runtime

Si vas a devolver de forma asincrónica un tipo que *no* sea de Windows Runtime, debes devolver una biblioteca de modelos de procesamiento paralelo [**concurrency::task**](/cpp/parallel/concrt/reference/task-class). Te recomendamos **concurrency::task** porque te dará un mejor rendimiento (y mejor compatibilidad en el futuro) que **std::future**.

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

Para las funciones sincrónicas, debes usar los parámetros `const&` de manera predeterminada. Eso evitará la sobrecarga de copias (que impliquen recuento de referencias, lo que significa incrementos y decrementos entrelazados).

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

En una corrutina, la ejecución es sincrónica hasta el primer punto de suspensión, donde el control se devuelve a la persona que llama. Cuando se reanude la corrutina, podría haber ocurrido cualquier cosa en el valor de origen que haga referencia a un parámetro de referencia. Desde la perspectiva de la corrutina, un parámetro de referencia tiene un ciclo de vida incontrolado. Por tanto, en el ejemplo anterior, estamos seguros al acceder a *value* hasta el `co_await`, pero no tras él. Ni podemos pasar con seguridad *value* a **DoOtherWorkAsync** si no hay ningún riesgo de que esa función se suspenda e intente usar *value* después de que se reanude. Para que los parámetros sean seguros de usar después de suspender y reanudar, tus corrutinas deben utilizar paso-por-valor de forma predeterminada para garantizar que capturan por valor y evitan los problemas de ciclo de vida. Serán excepcionales casos en los que puedas desviarte de esa orientación porque estés convencido de que es seguro hacerlo.

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value);
```

También es discutible que (a menos que quieras mover el valor) pasar objetos por valor const es una buena práctica. No tendrá ningún efecto en el valor de origen desde el que estás haciendo una copia, pero hace que el propósito sea claro y ayuda si modificas accidentalmente la copia.

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

Consulta también [Matrices y vectores estándar](std-cpp-data-types.md#standard-arrays-and-vectors), que se ocupa de cómo pasar un vector estándar a un destinatario asíncrono.

## <a name="offloading-work-onto-the-windows-thread-pool"></a>Trabajo de descarga en el grupo de subprocesos de Windows

Una corrutina es una función como cualquier otro que un autor de llamada se bloquea hasta que una función devuelve la ejecución a él. Y, en la primera oportunidad para una corrutina a devolver es la primera `co_await`, `co_return`, o `co_yield`.

Por lo tanto, antes de realizar el trabajo enlazado a proceso en una corrutina, necesario devolver la ejecución al llamador (es decir, introducir un punto de suspensión) para que el llamador no se bloquea. Si aún no está haciendo que `co_await`- ing alguna otra operación, puede realizar `co_await` el [ **winrt::resume_background** ](/uwp/cpp-ref-for-winrt/resume-background) función. Eso devuelve el control a la persona que llama y, a continuación, reanuda inmediatamente en un subproceso del grupo de subprocesos.

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

## <a name="programming-with-thread-affinity-in-mind"></a>Programación con afinidad de subprocesos en mente

Este escenario se expande en el anterior. Descargas cierto trabajo en el grupo de subprocesos, pero quieres mostrar el progreso en la interfaz de usuario (UI).

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

El código anterior lanza una excepción [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread), porque debe actualizarse un **TextBlock** desde el subproceso que lo creó, que es el subproceso de interfaz de usuario. Una solución es capturar el contexto del subproceso dentro del cual se llamó originalmente a nuestra corrutina. Para ello, cree una instancia un [ **winrt::apartment_context** ](/uwp/cpp-ref-for-winrt/apartment-context) de objetos, el trabajo, en segundo plano y, a continuación, `co_await` el **apartment_context** para volver a la llamada a contexto.

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

Mientras se llame la corrutina anterior desde el subproceso de interfaz de usuario que creó **TextBlock**, esta técnica funciona. Habrá muchos casos en la aplicación en los que estés seguro de ello.

Una solución más general para actualizar la interfaz de usuario, que incluye los casos donde no está seguro sobre el subproceso que realiza la llamada, puede `co_await` el [ **winrt::resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) función para cambiar a un determinado subproceso en primer plano. En el siguiente ejemplo de código, especificamos el subproceso de primer plano, pasando el objeto del distribuidor asociado a **TextBlock** (mediante el acceso a su propiedad [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). La implementación de **winrt::resume_foreground** llama a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) en ese objeto de distribuidor para ejecutar el trabajo que viene después de él en la corrutina.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Contextos de ejecución, reanudación y el cambio de una corrutina

En términos generales, después de un punto de suspensión en una corrutina, el subproceso de ejecución original puede desaparecer y reanudación puede producirse en cualquier subproceso (en otras palabras, puede llamar cualquier subproceso del **completado** método para la operación asincrónica).

Pero si le `co_await` cualquiera de los cuatro tipos de operación asincrónica de Windows en tiempo de ejecución (**IAsyncXxx**), a continuación, C++ / c++ / WinRT captura el contexto de llamada en el momento `co_await`. Y le garantiza que todavía está en ese contexto cuando se reanuda la continuación. C++ / c++ / WinRT para ello, comprueba si ya está en el contexto de llamada y, si no, cambiar a él. Si estaba en un subproceso de contenedor uniproceso (STA) antes de `co_await`, acto seguido, estará en el mismo posteriormente; si estaba en un subproceso de apartamento multiproceso (MTA) antes de `co_await`, a continuación, estará más adelante en uno.

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

La razón puede confiar en este comportamiento es que C++ / c++ / WinRT proporciona código para adaptarse a esos tipos de operación asincrónica de Windows en tiempo de ejecución para la compatibilidad de idioma de corrutina de C++ (estos fragmentos de código se denominan adaptadores de espera). El resto de tipos que admite await en C++ / c++ / WinRT simplemente son contenedores del grupo de subprocesos o las aplicaciones auxiliares; por lo que se completan en el grupo de subprocesos.

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

Si se `co_await` algún otro tipo&mdash;incluso en C++ / c++ / WinRT implementación de corrutina&mdash;, a continuación, otra biblioteca proporciona los adaptadores y tendrá que comprender lo que hacen esos adaptadores en términos de reanudación y contextos.

Para mantener los cambios de contexto reduce al mínimo, puede usar algunas de las técnicas que ya hemos visto en este tema. Veamos algunas de las ilustraciones de hacer eso. En este ejemplo de pseudocódigo siguiente, se muestra el esquema de un controlador de eventos que llama a una API de Windows en tiempo de ejecución para cargar una imagen, directamente a un subproceso en segundo plano para procesar dicha imagen y, a continuación, se devuelve al subproceso de interfaz de usuario para mostrar la imagen en la interfaz de usuario.

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

En este escenario, hay un poco de ineffiency alrededor de la llamada a **StorageFile::OpenAsync**. Hay un cambio de contexto necesario para un fondo de subprocesos (de modo que el controlador puede devolver la ejecución al autor de llamada), en la reanudación después de que C++ / c++ / WinRT restaura el contexto del subproceso de interfaz de usuario. Pero, en este caso, no es necesario que sea en el subproceso de interfaz de usuario hasta que se van a actualizar la interfaz de usuario. Las más Windows en tiempo de ejecución APIs llamamos *antes* nuestra llamada a **winrt::resume_background**, los modificadores de contexto atrás y hacia más innecesarios incurrimos. La solución es no llamar a *cualquier* Windows en tiempo de ejecución APIs antes. Moverlos todos después la **winrt::resume_background**.

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

Si desea hacer algo más avanzado y, a continuación, puede escribir la suya: adaptadores await. Por ejemplo, si desea que un `co_await` reanudar en el mismo subproceso que se completa la acción de async en (por lo tanto, no hay ningún cambio de contexto), a continuación, podría empezar escribiendo await adaptadores similares a los que se muestra a continuación.

> [!NOTE]
> El ejemplo de código siguiente se proporciona con fines formativos; es para ayudarle a comenzar a comprender cómo await funcionan los adaptadores. Si desea utilizar esta técnica en su propio código base, se recomienda que desarrolle y pruebe sus propios await struct(s) del adaptador. Por ejemplo, podría escribir **complete_on_any**, **complete_on_current**, y **complete_on(dispatcher)** . También considere la posibilidad de plantillas que toman el **IAsyncXxx** tipo como un parámetro de plantilla.

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

Para aprender a usar el **argumento de sin cambios** : adaptadores await, primero deberá saber que, cuando el C++ compilador encuentra un `co_await` expresión busca funciones llamadas **await_ready**, **await_suspend**, y **await_resume**. C++ / c++ / WinRT biblioteca proporciona estas funciones para que obtenga de forma predeterminada, al igual que este comportamiento razonable.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Para usar el **argumento de sin cambios** : adaptadores await, cambie el tipo del que `co_await` expresión desde **IAsyncXxx** a **argumento de sin cambios**, como en este ejemplo.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

A continuación, en lugar de buscar los tres **await_xxx** funciones que coinciden con **IAsyncXxx**, el C++ compilador busca funciones que coinciden con **argumento de sin cambios**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Cancelar una operación asincrónica y las devoluciones de llamada de cancelación

Características del Runtime de Windows para la programación asincrónica permiten cancelar una operación o acción asincrónica en curso. Este es un ejemplo que llama a [ **StorageFolder::GetFilesAsync** ](/uwp/api/windows.storage.storagefolder.getfilesasync) recuperar una colección potencialmente grande de archivos y almacena el objeto resultante de la operación asincrónica en un miembro de datos. El usuario tiene la opción de cancelar la operación.

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

Para el lado de la implementación de la cancelación, comencemos con un ejemplo sencillo.

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

Si ejecuta el ejemplo anterior, verá **ImplicitCancellationAsync** imprimir un mensaje por segundo durante tres segundos, tras el cual finaliza automáticamente el tiempo como resultado de ser cancelada. Esto funciona porque, en encontrar una `co_await` una corrutina de expresión, comprueba si se ha cancelado. Si es así, a continuación, provoca un cortocircuito en horizontal; y si no es así, a continuación, suspende con normalidad.

Cancelación por supuesto, puede ocurrir mientras se suspende la corrutina. Solo cuando se reanuda la corrutina, o presiona otra `co_await`, comprobará para la cancelación. El problema es uno de latencia potencialmente demasiado general-más preciso en responder a la cancelación.

Por lo tanto, otra opción es sondear explícitamente la cancelación dentro de la corrutina. Actualice el ejemplo anterior con el código en la lista siguiente. En este ejemplo nuevo, **ExplicitCancellationAsync** recupera el objeto devuelto por la [ **winrt::get_cancellation_token** ](/uwp/cpp-ref-for-winrt/get-cancellation-token) función y lo usa para periódicamente Compruebe si se ha cancelado la corrutina. No se cancela, siempre y cuando la corrutina se repite indefinidamente. una vez que se cancele, normalmente salir el bucle y la función. El resultado es el mismo, ya que el ejemplo anterior, pero aquí salir se realiza explícitamente y bajo control.

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

Esperando **winrt::get_cancellation_token** recupera un token de cancelación con conocimientos de la **IAsyncAction** que está produciendo la corrutina en su nombre. Puede usar el operador de llamada de función en ese token para consultar el estado de cancelación&mdash;esencialmente de sondeo de cancelación. Si va a realizar alguna operación enlazada o recorrer en iteración una colección de gran tamaño, a continuación, se trata de una técnica razonable.

### <a name="register-a-cancellation-callback"></a>Registrar una devolución de llamada de cancelación

Cancelación del Runtime de Windows no fluye automáticamente a otros objetos asincrónicas. Pero&mdash;introducidas en la versión 10.0.17763.0 (Windows 10, versión 1809) del SDK de Windows&mdash;puede registrar una devolución de llamada de cancelación. Se trata de un enlace preferente por el que se deben propagar de cancelación y hace que sea posible realizar la integración con las bibliotecas existentes de simultaneidad.

En este ejemplo de código siguiente, **NestedCoroutineAsync** realiza el trabajo, pero no tiene ninguna lógica especial de cancelación en ella. **CancellationPropagatorAsync** es esencialmente un contenedor de la corrutina anidada; el contenedor reenvía cancelación de manera preventiva.

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

**CancellationPropagatorAsync** registra una función lambda para su propia devolución de llamada de cancelación y, a continuación, espera (suspende lo) hasta que se complete el trabajo anidado. Cuando o si **CancellationPropagatorAsync** está cancelado, se propaga a la cancelación de la corrutina anidada. No hay ninguna necesidad de sondear en busca de cancelación; Tampoco se cancelación bloquea indefinidamente. Este mecanismo es lo suficientemente flexible como para que pueda usarlo para interoperar con una biblioteca de corrutina o simultaneidad que no sabe nada de C++ / c++ / WinRT.

## <a name="reporting-progress"></a>Informes de progreso

Si devuelve la corrutina [ **IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_), o [ **IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), entonces puede recuperar el objeto devuelto por la [ **winrt::get_progress_token** ](/uwp/cpp-ref-for-winrt/get-progress-token) funcionando y usarlo para notificar el progreso hacia un controlador de progreso. Aquí tienes un ejemplo de código.

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
> No es correcto implementar más de un *controlador de finalización* para una operación o acción asincrónica. Puede tener un solo delegado para su evento completado, o bien puede `co_await` lo. Si tiene ambos, se producirá un error en la segunda. Ya sea uno de los siguientes dos tipos de controladores de finalización es adecuado; no tanto para el mismo objeto asincrónico.

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

Para obtener más información sobre los controladores de finalización, consulte [tipos para las operaciones y acciones asincrónicas de delegado](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Desencadenar y omitir

A veces, tiene una tarea que se puede realizar simultáneamente con otro trabajo, y no es necesario esperar a que se complete (ningún otro trabajo depende de él), tampoco es necesario para devolver un valor. En ese caso, puede lanzar la tarea y la olvide. Puede hacerlo escribiendo una corrutina cuyo tipo de valor devuelto es [ **winrt::fire_and_forget** ](/uwp/cpp-ref-for-winrt/fire-and-forget) (en lugar de uno de los tipos de operación asincrónica de Windows en tiempo de ejecución, o **Concurrency::Task**).

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

## <a name="important-apis"></a>API importantes
* [clase Concurrency:: Task](/cpp/parallel/concrt/reference/task-class)
* [Interfaz IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; interface](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; interfaz](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Clase SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Temas relacionados
* [Controlar eventos mediante el uso de delegados en C / c++ / WinRT](handle-events.md)
* [Tipos de datos de C++ estándar y C++/WinRT](std-cpp-data-types.md)
