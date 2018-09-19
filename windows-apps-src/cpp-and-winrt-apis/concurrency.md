---
author: stevewhims
description: Este tema muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.
title: Operaciones simultáneas y asincrónicas con C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, simultaneidad, async, asincrónico, asincronía
ms.localizationpriority: medium
ms.openlocfilehash: 85071fb28cb87c991e2f5ba7f64b681c6850c819
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "4061039"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Operaciones simultáneas y asincrónicas con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Este tema muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operaciones asincrónicas y funciones "Async" de Windows Runtime
Cualquier API de Windows Runtime que tenga el potencial de tardar más de 50 milisegundos en completarse se implementa como una función asincrónica (con un nombre terminado en "Async"). La implementación de una función asincrónica inicia el trabajo en otro subproceso y regresa inmediatamente con un objeto que representa la operación asincrónica. Cuando se completa la operación asincrónica, dicho objeto devuelto contiene cualquier valor que resultase del trabajo. El espacio de nombres de Windows Runtime **Windows::Foundation** contiene cuatro tipos de objetos de la operación asincrónica.

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) y
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada uno de estos tipos de la operación asincrónica se proyecta en un correspondiente tipo en el espacio de nombres **winrt::Windows::Foundation** de C++/WinRT. C++ / WinRT también contiene una estructura adaptadora await interna. No la usas directamente, pero gracias a dicha estructura puedes escribir una instrucción "co_await" para esperar de forma cooperativa el resultado de cualquier función que devuelva uno de estos tipos de la operación asincrónica. Y puedes crear tus propias corrutinas que devuelvan estos tipos.

Un ejemplo de una función asincrónica de Windows es [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync), que devuelve un objeto de la operación asincrónica de tipo [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_). Echemos un vistazo a algunas maneras de&mdash;bloqueo y no bloqueo&mdash;del uso de C++/WinRT para llamar a una API como esta.

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
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
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

    auto processOp = ProcessFeedAsync();
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

    auto feedOp = RetrieveBlogFeedAsync();
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
        SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
        return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
    });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp = RetrieveFirstTitleAsync();
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
Antes de hacer un trabajo unido al cálculo en una corrutina, deberás devolver la ejecución a la persona que llama de forma que no se bloquee (en otras palabras, introducir un punto de suspensión). Si aún no lo estás haciendo aplicando `co-await`- en alguna otra operación, puedes aplicar `co-await` en la función **winrt::resume_background**. Eso devuelve el control a la persona que llama y, a continuación, reanuda inmediatamente en un subproceso del grupo de subprocesos.

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
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

El código anterior lanza una excepción [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread), porque debe actualizarse un **TextBlock** desde el subproceso que lo creó, que es el subproceso de interfaz de usuario. Una solución es capturar el contexto del subproceso dentro del cual se llamó originalmente a nuestra corrutina. Crea una instancia de un objeto **winrt::apartment_context** y luego aplica `co_await` en él.

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

> [!NOTE]
> **El siguiente ejemplo de código hace referencia al producto de versión preliminar, que puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Una solución más general para actualizar la interfaz de usuario que cubre los casos donde no estás seguro sobre el subproceso de llamada, puedes instalar la [versión preliminar del SDK de Windows 10 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) o una versión posterior. A continuación, puedes aplicar `co-await` a la función **winrt::resume_foreground** para cambiar a un subproceso en primer plano específico. En el siguiente ejemplo de código, especificamos el subproceso de primer plano, pasando el objeto del distribuidor asociado a **TextBlock** (mediante el acceso a su propiedad [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher)). La implementación de **winrt::resume_foreground** llama a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) en ese objeto de distribuidor para ejecutar el trabajo que viene después de él en la corrutina.

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
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

## <a name="related-topics"></a>Artículos relacionados
* [Controlar eventos usando delegados en C ++/WinRT](handle-events.md)
* [Tipos de datos C++ estándar y C++/WinRT](std-cpp-data-types.md)