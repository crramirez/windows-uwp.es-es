---
description: En este tema se muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.
title: Operaciones simultáneas y asincrónicas con C++/WinRT
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, concurrency, async, asynchronous, asynchrony
ms.localizationpriority: medium
ms.openlocfilehash: 1dd6ac2760189578932fc22db89c7091f2e527ab
ms.sourcegitcommit: 8179902299df0f124dd770a09a5a332397970043
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68428636"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>Operaciones simultáneas y asincrónicas con C++/WinRT

En este tema se muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

Después de leer este tema, consulta también [Simultaneidad y asincronía más avanzadas](concurrency-2.md) para conocer más escenarios.

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

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

En una corrutina, la ejecución es sincrónica hasta el primer punto de suspensión, donde el control se devuelve al autor de la llamada, y el marco de la llamada sale del ámbito. Cuando se reanuda la corrutina, puede haber ocurrido cualquier cosa en el valor de origen que hace referencia a un parámetro de referencia. Desde la perspectiva de la corrutina, un parámetro de referencia tiene un ciclo de vida incontrolado. Por tanto, en el ejemplo anterior, estamos seguros al acceder a *value* hasta `co_await`, pero no tras él. En caso de que el autor de llamada destruya *value*, intenta acceder a él dentro de la corrutina después de que eso provoque un daño de memoria. Tampoco podemos pasar con seguridad *value* a **DoOtherWorkAsync** si hay algún riesgo de que esa función se suspenda e intente usar *value* después de que se reanude.

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

## <a name="important-apis"></a>API importantes
* [Clase concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [Interfaz IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [Interfaz IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [Interfaz IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [Interfaz IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Clase SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>Temas relacionados
* [Simultaneidad y asincronía más avanzados](concurrency-2.md)
* [Control de eventos mediante delegados en C++/WinRT](handle-events.md)
* [Tipos de datos de C++ estándar y C++/WinRT](std-cpp-data-types.md)