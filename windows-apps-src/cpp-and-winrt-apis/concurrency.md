---
author: stevewhims
description: Este tema muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.
title: Operaciones simultáneas y asincrónicas con C++/WinRT
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, simultaneidad, async, asincrónico, asincronía
ms.localizationpriority: medium
ms.openlocfilehash: 3af9125abc3abf41327f5b49e6a05d81e214f89f
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831839"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Operaciones simultáneas y asincrónicas con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Este tema muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT.

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>Operaciones asincrónicas y funciones "Async" de Windows Runtime
Cualquier API de Windows Runtime que tenga el potencial de tardar más de 50 milisegundos en completarse se implementa como una función asincrónica (con un nombre terminado en "Async"). La implementación de una función asincrónica inicia el trabajo en otro subproceso y regresa inmediatamente con un objeto que representa la operación asincrónica. Cuando se completa la operación asincrónica, dicho objeto devuelto contiene cualquier valor que resultase del trabajo. El espacio de nombres de Windows Runtime **Windows::Foundation** contiene cuatro tipos de objetos de la operación asincrónica, a saber,

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) y
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

Cada uno de estos tipos de la operación asincrónica se proyecta en un correspondiente tipo en el espacio de nombres **winrt::Windows::Foundation** de C++/WinRT. C++ / WinRT también contiene una estructura adaptadora await interna. No la usas directamente, pero gracias a dicha estructura puedes escribir una instrucción **co_await** para esperar de forma cooperativa el resultado de cualquier función que devuelva uno de estos tipos de la operación asincrónica. Y puedes crear tus propias corrutinas que devuelvan estos tipos.

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

Una llamada a **get** hace que la codificación sea adecuada, aunque no es lo que llamaríamos cooperativa. No es simultánea ni asincrónica. Para evitar retener subprocesos del sistema operativo desde otro trabajo útil, necesitamos una técnica diferente.

## <a name="write-a-coroutine"></a>Escribir una corrutina
C++/WinRT integra las corrutinas de C++ en el modelo de programación para proporcionar una forma natural de esperar de forma cooperativa un resultado. Puedes producir tu propia operación asincrónica de Windows Runtime escribiendo un corrutina. En el ejemplo de código siguiente, **ProcessFeedAsync** es la corrutina.

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = co_await syndicationClient.RetrieveFeedAsync(rssFeedUri);
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

Un corrutina es una función que se suspende y reanuda. En la anterior corrutina **ProcessFeedAsync**, cuando se alcanza la instrucción **co_await**, la corrutina inicia de forma asincrónica la llamada **RetrieveFeedAsync**, después se suspende inmediatamente y devuelve el control al autor de la llamada (que es **main** en el ejemplo anterior). **main** puede seguir funcionando mientras la fuente se recupera y se imprime. Una vez hecho esto (cuando se completa la llamada **RetrieveFeedAsync**), la corrutina **ProcessFeedAsync** se reanuda en la próxima instrucción.

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

void PrintFeed(SyndicationFeed syndicationFeed)
{
    for (SyndicationItem syndicationItem : syndicationFeed.Items())
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

Si vas a devolver asincrónicamente un tipo de Windows Runtime (ya sea un tipo de origen o de terceros), debes devolver un [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) o un [**IAsyncOperationWithProgress&lt;TResult, TProgress &gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_).

El compilador te ayudará con un error "*debe ser de tipo WinRT*" si intentas usar uno de estos tipos de la operación asincrónica con un tipo que no sea de Windows Runtime.

## <a name="asychronously-return-a-non-windows-runtime-type"></a>Devolver de forma asincrónica un tipo que no sea de Windows Runtime
Si vas a devolver de forma asincrónica un tipo que *no* sea de Windows Runtime, debes devolver una biblioteca de modelos de procesamiento paralelo [**task**](https://msdn.microsoft.com/library/hh750113). Te recomendamos **task** porque te dará un mejor rendimiento (y mejor compatibilidad en el futuro) que **std::future**.

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
    // do other work.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="important-apis"></a>API importantes
* [concurrency::task](https://msdn.microsoft.com/library/hh750113)
* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)
