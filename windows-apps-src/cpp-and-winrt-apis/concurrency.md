---
author: stevewhims
description: Este tema muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.
title: Operaciones simultáneas y asincrónicas con C++/WinRT
ms.author: stwhi
ms.date: 10/27/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, simultaneidad, async, asincrónico, asincronía
ms.localizationpriority: medium
ms.openlocfilehash: d7807b71f1c775493e525284e61c093081eb2c2b
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5754848"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Operaciones simultáneas y asincrónicas con C++/WinRT

Este tema se muestra en la que puede crear y consumir objetos asincrónicos de Windows Runtime con [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operaciones asincrónicas y funciones "Async" de Windows Runtime

Cualquier API de Windows Runtime que tenga el potencial de tardar más de 50 milisegundos en completarse se implementa como una función asincrónica (con un nombre terminado en "Async"). La implementación de una función asincrónica inicia el trabajo en otro subproceso y regresa inmediatamente con un objeto que representa la operación asincrónica. Cuando se completa la operación asincrónica, dicho objeto devuelto contiene cualquier valor que resultase del trabajo. El espacio de nombres de Windows Runtime **Windows::Foundation** contiene cuatro tipos de objetos de la operación asincrónica.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) y
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada uno de estos tipos de la operación asincrónica se proyecta en un correspondiente tipo en el espacio de nombres **winrt::Windows::Foundation** de C++/WinRT. C++ / WinRT también contiene una estructura adaptadora await interna. No la usas directamente, pero gracias a dicha estructura, puedes escribir un `co_await` declaración de cooperativa el resultado de cualquier función que devuelva uno de estos tipos de la operación. Y puedes crear tus propias corrutinas que devuelvan estos tipos.

Un ejemplo de una función asincrónica de Windows es [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que devuelve un objeto de la operación asincrónica de tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Echemos un vistazo a algunas maneras de&mdash;bloqueo primero y, a continuación, sin bloqueo&mdash;del uso de C++ / WinRT para llamar a una API como esta.

## <a name="block-the-calling-thread"></a>Bloquear el subproceso de llamada

El siguiente ejemplo de código recibe un objeto de la operación asincrónica desde **RetrieveFeedAsync** y llama a **get** en dicho objeto para bloquear el subproceso de llamada hasta que los resultados de la operación asincrónica estén disponibles.

```cppwinrt
// main.cpp

#include "pch.h"
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
> La función **obtener** existe en C++ / WinRT proyección escribe **winrt::Windows::Foundation::IAsyncAction**, por lo que puedes llamar a la función desde cualquier C++ / WinRT proyecto. No podrás encontrar la función aparece como un miembro de la interfaz [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) , porque no es parte de la superficie de interfaz binaria (ABI) de la aplicación del tipo de Windows Runtime real **IAsyncAction** **obtener** .

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

En el siguiente ejemplo, encapsulamos una llamada a **RetrieveFeedAsync** de un URI específico para que nos dé una función **RetrieveBlogFeedAsync** que devuelve asincrónicamente un [**SyndicationFeed **](/uwp/api/windows.web.syndication.syndicationfeed).

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

Si vas a devolver asincrónicamente un tipo de Windows Runtime, debes devolver un [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) o un [**IAsyncOperationWithProgress&lt;TResult, TProgress &gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Cualquier clase de tiempo de ejecución propia o de terceros cualifica o cualquier tipo que se pueda pasar a o desde una función de Windows Runtime (por ejemplo, `int` o **winrt::hstring**). El compilador te ayudará con un error "*debe ser de tipo WinRT*" si intentas usar uno de estos tipos de la operación asincrónica con un tipo que no sea de Windows Runtime.

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

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
#include <ppltasks.h>

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

Una corrutina es una función como cualquier otro en que un autor de la llamada se bloquea hasta que una función devuelve la ejecución a ella. Y, en la primera oportunidad para una corrutina devolver es la primera `co_await`, `co_return`, o `co_yield`.

Por lo tanto, antes de realizar trabajos de cálculo en una corrutina, debes devolver la ejecución al llamador (en otras palabras, introducir un punto de suspensión) para que no se bloquea el llamador. Si aún no lo estás haciendo que `co-await`- en alguna otra operación, puedes `co-await` la función [**winrt:: resume_background**](/uwp/cpp-ref-for-winrt/resume-background) . Eso devuelve el control a la persona que llama y, a continuación, reanuda inmediatamente en un subproceso del grupo de subprocesos.

El grupo de subprocesos que se va a usar en la implementación es el [grupo de subprocesos de Windows](https://msdn.microsoft.com/library/windows/desktop/ms686766) de bajo nivel, por lo que es eficaz en forma óptima.

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
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

El código anterior lanza una excepción [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread), porque debe actualizarse un **TextBlock** desde el subproceso que lo creó, que es el subproceso de interfaz de usuario. Una solución es capturar el contexto del subproceso dentro del cual se llamó originalmente a nuestra corrutina. Para ello, crea una instancia de un objeto [**apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context) , trabajo, en segundo plano y, después, `co_await` la **apartment_context** para volver al contexto de llamada.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

Mientras se llame la corrutina anterior desde el subproceso de interfaz de usuario que creó **TextBlock**, esta técnica funciona. Habrá muchos casos en la aplicación en los que estés seguro de ello.

Una solución más general para actualizar la interfaz de usuario, que cubre los casos donde no estás seguro sobre el subproceso de llamada, puedes `co-await` la función [**winrt:: resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) para cambiar a un subproceso en primer plano específico. En el siguiente ejemplo de código, especificamos el subproceso de primer plano, pasando el objeto del distribuidor asociado a **TextBlock** (mediante el acceso a su propiedad [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). La implementación de **winrt::resume_foreground** llama a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) en ese objeto de distribuidor para ejecutar el trabajo que viene después de él en la corrutina.

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction DoWorkAsync(TextBlock const& textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>Contextos de ejecución, reanudación y cambiar en una corrutina

En términos generales, después de un punto de suspensión en una corrutina, el subproceso original de ejecución puede desaparecer y reanudación puede producirse en cualquier subproceso (en otras palabras, cualquier subproceso puede llamar al método **Completed** para la operación asincrónica).

Pero si te `co-await` cualquiera de los tipos de operación asincrónica de Windows Runtime cuatro (**IAsyncXxx**), a continuación, C++ / WinRT captura el contexto de llamada en el punto `co-await`. Y garantiza que estás aún en ese contexto cuando se reanuda la continuación. C++ / WinRT hace esto al comprobar si ya estás en el contexto de llamada y, si no, cambiar a ella. Si estabas en un subproceso de contenedor uniproceso (STA) antes de `co-await`, a continuación, estarás más tarde; en el mismo Si estabas en un subproceso de contenedor multiproceso (MTA) antes de `co-await`, a continuación, estarás más adelante en uno.

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

La razón puede confiar en este comportamiento es que C++ / WinRT proporciona el código para adaptarse a esos tipos de operación asincrónica de Windows Runtime para la compatibilidad del lenguaje C++ corrutina (estos fragmentos de código se denominan espera adaptadores). Los tipos de esperables restantes en C++ / WinRT son simplemente contenedores de grupo de subprocesos o aplicaciones auxiliares; por lo tanto, realiza en el grupo de subprocesos.

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

Si te `co_await` algún otro tipo&mdash;incluso dentro de C++ / WinRT implementación de corrutina&mdash;, a continuación, otra biblioteca proporciona los adaptadores y tendrás que comprender lo que esos adaptadores hacer en términos de reanudación y contextos.

Para mantener los cambios de contexto reduce al mínimo, puedes usar algunas de las técnicas que ya hemos visto en este tema. Vamos a ver algunas ilustraciones de hacerlo. En este ejemplo de seudocódigo siguiente, te mostramos el contorno de un controlador de eventos que llama a una API de Windows en tiempo de ejecución para cargar una imagen, cae en un subproceso en segundo plano para procesar esa imagen y, a continuación, se devuelve al subproceso de interfaz de usuario para mostrar la imagen en la interfaz de usuario.

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

Para este escenario, hay un poco de ineffiency alrededor de la llamada a **StorageFile::OpenAsync**. Hay un cambio de contexto necesario a un fondo de subprocesos (de modo que el controlador puede devolver la ejecución al llamador), en la reanudación después de que C++ / WinRT restaura el contexto del subproceso de interfaz de usuario. Pero, en este caso, no es necesario que sea en el subproceso de interfaz de usuario hasta que se va a actualizar la interfaz de usuario. El tiempo de ejecución de Windows más APIs llamamos a *antes de* la llamada a **winrt:: resume_background**, los modificadores de contexto atrás y adelante más innecesarios se incurre. La solución es no llamar a *cualquier* Windows Runtime APIs antes. Moverlos todos después de la **winrt:: resume_background**.

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

Si quieres hacer algo más avanzados, a continuación, se podría escribir tu propio await adaptadores. Por ejemplo, si quieres que un `co_await` reanudar en el mismo subproceso que se complete la acción async en (por lo tanto, no hay ningún cambio de contexto), a continuación, podrías comenzar escribiendo await adaptadores similares a los que se muestra a continuación.

> [!NOTE]
> El siguiente ejemplo de código se proporciona para fines educativos solo; es para que puedas empezar a comprender cómo await adaptadores trabajo. Si quieres usar esta técnica en tu propio código base, a continuación, te recomendamos que desarrollar y probar tus propios await struct(s) adaptador. Por ejemplo, podría escribir **complete_on_any**, **complete_on_current**y **complete_on(dispatcher)**. Ten en cuenta también que les plantillas que toman el tipo de **IAsyncXxx** como un parámetro de plantilla.

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

Para comprender cómo usar el **argumento de sin cambios** await adaptadores, primero tendrás que saber que cuando el compilador de C++ se encuentra un `co_await` busca las funciones de expresión denomina **await_ready**, **await_suspend**y **await_resume**. C++ / WinRT biblioteca proporciona esas funciones para que Obtén el comportamiento razonable de manera predeterminada, como este.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

Para usar el **argumento de sin cambios** await adaptadores, cambie el tipo de ese `co_await` expresión **IAsyncXxx** al **argumento de sin cambios**, como este.

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

A continuación, en lugar de buscar las tres funciones **await_xxx** que coinciden con **IAsyncXxx**, el compilador de C++ busca funciones que coinciden con el **argumento de sin cambios**.

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>Cancelar una operación asincrónica y las devoluciones de llamada de cancelación

Las características de Windows Runtime para la programación asincrónica permiten cancelar una operación o la acción asincrónica en curso. Este es un ejemplo que llama a [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) para recuperar una colección de archivos potencialmente grande y almacena el objeto de operación asincrónica resultante en un miembro de datos. El usuario tiene la opción de cancelar la operación.

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Storage.Search.h>
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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
```

Para que el lado de la implementación de cancelación, vamos a comenzar con un ejemplo sencillo.

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

Si ejecutas el ejemplo anterior, a continuación, podrás ver un mensaje de impresión de **ImplicitCancellationAsync** por segundo de tres segundos, tras lo cual vez que automáticamente se termina como resultado que se cancela. Esto funciona porque se produzca en un `co_await` una corrutina de expresión, comprueba si se ha cancelado. Si tiene, a continuación, cortocircuita y si no es así, a continuación, se suspende como normal.

Por supuesto, puede suceder cancelación mientras está suspendida la corrutina. Solo cuando se reanude la corrutina, o alcanza la otra `co_await`, se comprueba para cancelación. El problema es uno de latencia potencialmente demasiado grueso-más preciso responder a la cancelación.

Por lo tanto, otra opción es explícitamente hace un sondeo de cancelación desde dentro de la corrutina. Actualiza el ejemplo anterior con el código de la siguiente lista. En este ejemplo nuevo, **ExplicitCancellationAsync** recupera el objeto devuelto por la función [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) y lo usa para comprobar periódicamente si se ha cancelado la corrutina. Siempre y cuando no se cancela, la corrutina se repite indefinidamente. una vez que se cancela, el bucle y la función salir con normalidad. El resultado es el mismo a medida que el ejemplo anterior, pero aquí salir sucede explícitamente y bajo el control.

```cppwinrt
...
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

Esperando en **winrt::get_cancellation_token** recupera un token de cancelación con conocimiento de **IAsyncAction** que genera la corrutina en tu nombre. Puedes usar el operador de llamada de función en ese token para consultar el estado de cancelación&mdash;básicamente sondeo de cancelación. Si estás realizando operaciones cálculo o iteración a través de una gran colección, es una técnica razonable.

### <a name="register-a-cancellation-callback"></a>Registrar una devolución de llamada de cancelación

Cancelación de Windows Runtime no fluirá automáticamente a otros objetos asincrónicos. Pero&mdash;incluido en la versión 10.0.17763.0 (Windows 10, versión 1809) del Windows SDK&mdash;puedes registrar una devolución de llamada de cancelación. Se trata de un enlace preventivo por el que se puede propagar cancelación y hace posible integrar con las bibliotecas de simultaneidad existentes.

En este ejemplo de código siguiente, **NestedCoroutineAsync** realiza el trabajo, pero no tiene ninguna lógica especial de cancelación en él. **CancellationPropagatorAsync** es básicamente un contenedor en la corrutina anidada; el contenedor reenvía cancelación antelación.

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

    cancellation_token.callback([&]
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

**CancellationPropagatorAsync** registra una función lambda para su propia devolución de llamada de cancelación y, a continuación, espera a que lo (se suspende) hasta que se complete el trabajo anidado. Cuando o si se cancela **CancellationPropagatorAsync** , propaga la cancelación a la corrutina anidada. No es necesario para realizar un sondeo de cancelación; Tampoco se cancelación bloquea indefinidamente. Este mecanismo es lo suficientemente flexible como para su uso a la interoperabilidad con una biblioteca de corrutina o simultaneidad que no sabe nada de C++ / WinRT.

## <a name="reporting-progress"></a>Informes de progreso

Si la corrutina devuelve [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)o [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_), a continuación, puedes recuperar el objeto devuelto por la función [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) y usarlo para informar del progreso a un progreso controlador. Aquí tienes un ejemplo de código.

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
> No es correcto implementar más de un *controlador de finalización* para una acción asincrónica o la operación. Puedes tener un solo delegado para su evento completado, o bien puedes `co_await` él. Si tienes ambos, se producirá un error en la segunda. Ya sea uno de los siguientes dos tipos de los controladores de finalización es apropiado; no tanto para el mismo objeto asincrónico.

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

Para obtener más información acerca de los controladores de finalización, consulta [los tipos de delegados para acciones y operaciones asincrónicas](handle-events.md#delegate-types-for-asynchronous-actions-and-operations).

## <a name="fire-and-forget"></a>Desencadenar y omitir

En ocasiones, tienes una tarea que se puede realizar simultáneamente con otro trabajo, y no es necesario que esperar para que esa tarea completar (ningún otro trabajo depende de ella), tampoco es necesario para devolver un valor. En ese caso, puede lanzar la tarea y la olvide. Puedes hacerlo mediante la escritura de una corrutina cuyo tipo devuelto es [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) (en lugar de uno de los tipos de operación asincrónica de Windows Runtime o **Concurrency:: Task**).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
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
* [IAsyncActionWithProgress&lt;TProgress&gt; interfaz](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; interfaz](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; interfaz](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método syndicationclient:: Retrievefeedasync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Clase SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>Artículos relacionados
* [Controlar eventos usando delegados en C ++/WinRT](handle-events.md)
* [Tipos de datos C++ estándar y C++/WinRT](std-cpp-data-types.md)