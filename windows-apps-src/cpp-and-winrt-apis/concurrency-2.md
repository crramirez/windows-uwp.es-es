---
description: Escenarios más avanzados con simultaneidad y asincronía en C++/WinRT.
title: Simultaneidad y asincronía más avanzadas con C++/WinRT
ms.date: 07/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, concurrency, async, asynchronous, asynchrony
ms.localizationpriority: medium
ms.openlocfilehash: ff00264d0806e7fbdfcabd000ec68857b1485dcd
ms.sourcegitcommit: 1e8f51d5730fe748e9fe18827895a333d94d337f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2020
ms.locfileid: "87296148"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>Simultaneidad y asincronía más avanzadas con C++/WinRT

En este tema se describen escenarios más avanzados con simultaneidad y asincronía en C++/WinRT.

Para ver una introducción a este asunto, lee primero [Operaciones simultáneas y asincrónicas](concurrency.md).

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

    // Switch to the foreground thread associated with textblock.
    co_await winrt::resume_foreground(textblock.Dispatcher());

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

La función **winrt::resume_foreground** toma un parámetro de prioridad opcional. Si utiliza ese parámetro, el patrón mostrado anteriormente es adecuado. Si no es así, puede optar por simplificar `co_await winrt::resume_foreground(someDispatcherObject);` en `co_await someDispatcherObject;`.

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

La razón por la que puedes confiar en este comportamiento es que C++/WinRT proporciona código para adaptar estos tipos de la operación asincrónica de Windows Runtime a la compatibilidad del lenguaje de corrutinas C++ (estos fragmentos de código se denominan adaptadores de espera). El resto de tipos que admite await en C++/WinRT simplemente son contenedores del grupo de subprocesos o aasistentes, por lo que se completan en el grupo de subprocesos.

```cppwinrt
using namespace std::chrono_literals;
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

## <a name="a-deeper-dive-into-winrtresume_foreground"></a>Análisis más profundo de **winrt::resume_foreground**

A partir de [C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20), la función [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) se suspende incluso si se llama desde el subproceso del distribuidor (en versiones anteriores, podía introducir interbloqueos en algunos escenarios porque solo se suspendía si aún no estaba en el subproceso del distribuidor).

El comportamiento actual implica que puedes confiar en que se produzca el desenredo de la pila y se vuelva a poner en cola. Esto es importante para la estabilidad del sistema, especialmente en el código de sistemas de bajo nivel. En la última lista de código de la sección anterior [Programación teniendo en cuenta la afinidad de subprocesos](#programming-with-thread-affinity-in-mind) se muestra cómo realizar un cálculo complejo en un subproceso en segundo plano y, después, cambiar al subproceso de interfaz de usuario adecuado para actualizar la interfaz de usuario (UI).

Este es el aspecto interno de **winrt::resume_foreground**.

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

Este comportamiento actual, frente al anterior, es análogo a la diferencia entre [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew) y [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) en el desarrollo de aplicaciones de Win32. **PostMessage** pone en cola el trabajo y luego desenreda la pila sin esperar a que el trabajo se complete. El desenredo de la pila puede ser esencial.

Al principio, la función **winrt::resume_foreground** también admitía solo [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) (vinculado a [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)), que se presentó antes de Windows 10. Desde entonces, hemos introducido un distribuidor más flexible y eficaz: [**DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Puedes crear un **DispatcherQueue** para tus propios fines. Presta atención a esta aplicación de consola sencilla.

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

En el ejemplo anterior se crea una cola (contenida en un controlador) en un subproceso privado y después se pasa el controlador a la corrutina. La corrutina puede usar la cola para esperar (suspender y reanudar) en el subproceso privado. Otro uso común de **DispatcherQueue** es crear una cola en el subproceso de interfaz de usuario actual de una aplicación de Win32 o de escritorio tradicional.

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

Esto muestra cómo puedes llamar e incorporar funciones de Win32 en los proyectos de C++/WinRT; para ello, solo tienes que llamar a la función [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller) de tipo Win32 para crear el controlador y después transferir la propiedad del controlador de cola resultante al autor de la llamada como un objeto de WinRT. Precisamente, de esta forma puedes admitir la puesta en cola eficaz y sin problemas en la aplicación de escritorio de Win32 de tipo Petzold existente.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

En el ejemplo anterior, la función **main** simple comienza creando una ventana. Puedes imaginar que esto registra una clase de ventana y llama a [**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww) para crear la ventana de escritorio de nivel superior. Después, se llama a la función **CreateDispatcherQueueController** para crear el controlador de cola antes de llamar a alguna de las corrutinas con la cola del distribuidor que posee este controlador. Luego se introduce un suministro de mensajes tradicional en el que se produce la reanudación de la corrutina de forma natural en este subproceso. Una vez hecho esto, puedes volver al elegante mundo de las corrutinas para tu flujo de trabajo asincrónico o basado en mensajes dentro de la aplicación.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

La llamada a **winrt::resume_foreground** siempre *pondrá en cola* y después desenredará la pila. También puedes establecer la prioridad de reanudación.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

O bien, usar el orden de cola predeterminado.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

O bien, en este caso, detectar el cierre de la cola y controlarlo correctamente.

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

La expresión `co_await` devuelve `true`, lo que indica que la reanudación se producirá en el subproceso del distribuidor. En otras palabras, se ha puesto en cola correctamente. Por el contrario, devuelve `false` para indicar que la ejecución permanece en el subproceso que realiza la llamada porque el controlador de la cola se está cerrando y ya no atiende solicitudes de cola.

Por lo tanto, tienes una gran capacidad a tu alcance al combinar C++/WinRT con corrutinas; y especialmente al desarrollar aplicaciones de escritorio de tipo Petzold a la antigua usanza.

## <a name="canceling-an-asynchronous-operation-and-cancellation-callbacks"></a>Cancelación de una operación asincrónica y devoluciones de llamadas de cancelación

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

Si la corrutina devuelve [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress-1) o [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2), puedes recuperar el objeto devuelto por la función [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) y usarlo para notificar el progreso a un controlador de progreso. Aquí tienes un ejemplo de código.

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

El primer argumento (el *remitente*) queda sin nombre, porque no se utiliza nunca. Por este motivo, estamos seguros si lo dejamos como una referencia. Pero observa que *args* se pasa por valor. Consulta la sección [Paso de parámetros](concurrency.md#parameter-passing) más arriba.

## <a name="awaiting-a-kernel-handle"></a>Esperar un controlador de kernel

C++/WinRT proporciona una función [**winrt::resume_on_signal**](/uwp/cpp-ref-for-winrt/resume-on-signal), que puede usar para suspender hasta que se señale un evento de kernel. Es tu responsabilidad asegurar que el controlador siga siendo válido hasta que se devuelva tu `co_await resume_on_signal(h)`. La clase **resume_on_signal** no puede hacerlo por ti, porque es posible que hayas perdido el controlador incluso antes de que se inicie **resume_on_signal**, como en este primer ejemplo.

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

El evento **HANDLE** de entrada solo es válido hasta que vuelve la función, y esta función (que es una corrutina) vuelve en el primer punto de suspensión (el primer `co_await` en este caso). Mientras esperas por **DoWorkAsync**, el control ha vuelto al autor de la llamada, el marco de la llamada ha salido del ámbito y ya no sabes si el controlador será válido cuando se reanude la corrutina.

Técnicamente, la corrutina recibe los parámetros por valor, como debería (consulta [Paso de parámetros](concurrency.md#parameter-passing) anteriormente). Con todo, en este caso, debemos dar un paso más para seguir el *espíritu* de esa guía (en lugar de solo la letra). Debemos pasar una referencia segura (en otras palabras, la propiedad) junto con el controlador. A continuación se muestra cómo hacerlo.

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* valid here.
}
```

Pasar [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) por valor proporciona la semántica de propiedad, lo que garantiza que el controlador de kernel siga siendo válido durante la vigencia de la corrutina.

Aquí se muestra cómo puedes llamar a esa corrutina.

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

Puede pasar un valor de tiempo de expiración a **resume_on_signal**, como en este ejemplo.

```cppwinrt
winrt::handle event = ...

if (co_await winrt::resume_on_signal(event.get(), std::literals::2s))
{
    puts("signaled");
}
else
{
    puts("timed out");
}
```

## <a name="asynchronous-timeouts-made-easy"></a>Simplificación de los tiempos de expiración asincrónicos

C++/WinRT ha invertido en gran medida en las corrutinas de C++. Su efecto en la escritura de código de simultaneidad es transformador. En esta sección se describen los casos en los que los detalles de la asincronía no son importantes y lo único que quieres es el resultado directamente. Por ese motivo, la implementación de C++/WinRT de la interfaz de operaciones asincrónicas [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) de Windows Runtime tiene una función **get** similar a la que proporciona **std::future**.

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

La función **get** se bloquea de forma indefinida, mientras se completa el objeto asincrónico. Los objetos asincrónicos tienden a tener una duración muy corta, de modo que esto suele ser todo lo que necesitas.

Pero hay casos en los que no es suficiente y debes abandonar la espera una vez que ha transcurrido algo de tiempo. Escribir ese código siempre ha sido posible gracias a los bloques de compilación que proporciona Windows Runtime. Pero ahora C++/WinRT hace que sea mucho más fácil mediante la función **wait_for**. También se ha implementado en **IAsyncAction** y, de nuevo, es similar a la que proporciona **std::future**.

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

> [!NOTE]
> **wait_for** usa **std::chrono::duration** en la interfaz, pero está limitado a un intervalo menor que el que proporciona **std::chrono::duration** (aproximadamente 49,7 días).

La función **wait_for** del siguiente ejemplo espera unos cinco segundos y después comprueba si se ha completado. Si la comparación es favorable, sabrás que el objeto asincrónico se ha completado correctamente y que ya has terminado. Si esperas algún resultado, solo tienes que seguirlo con una llamada a la función **get** para recuperar el resultado.

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

Dado que el objeto asincrónico ya se habrá completado para entonces, **get** devuelve el resultado de inmediato, sin tener que esperar más. Como puedes ver, **wait_for** devuelve el estado del objeto asincrónico. Por lo tanto, puedes usarlo para tener un control más específico, como este.

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- Recuerda que **AsyncStatus::Completed** implica que el objeto asincrónico se ha completado correctamente y que puedes llamar a la función **get** para recuperar los resultados.
- **AsyncStatus::Canceled** implica que el objeto asincrónico se ha cancelado. Normalmente, es el autor de la llamada quien solicita una cancelación, por lo que sería raro controlar este estado. Un objeto asincrónico se suele descartar simplemente.
- **AsyncStatus::Error** implica que se ha producido algún error en el objeto asincrónico. Si quieres, puedes hacer que **get** vuelva a producir la excepción.
- **AsyncStatus::Started** implica que el objeto asincrónico todavía se está ejecutando. El patrón asincrónico de Windows Runtime no permite varias esperas ni esperadores. Esto significa que no puedes llamar a **wait_for** en bucle. Si la espera ha agotado realmente el tiempo de expiración, te quedan algunas opciones. Puedes abandonar el objeto o sondear su estado antes de llamar a **get** para recuperar los resultados. Pero es mejor descartar el objeto en este momento.

## <a name="returning-an-array-asynchronously"></a>Devolver una matriz de forma asincrónica

A continuación, se muestra un ejemplo de [MIDL 3.0](/uwp/midl-3/) que genera *error MIDL2025: [msg]syntax error [context]: expecting > or, near "["* .

```idl
Windows.Foundation.IAsyncOperation<Int32[]> RetrieveArrayAsync();
```

El motivo es que no es válido usar una matriz como argumento de tipo de parámetro para una interfaz parametrizada. Por lo tanto, necesitamos una manera menos obvia de lograr pasar de forma asincrónica una matriz desde un método de clase en tiempo de ejecución. 

Puedes devolver la matriz aplicando la conversión boxing a un objeto [PropertyValue](/uwp/api/windows.foundation.propertyvalue). A continuación, el código de llamada le aplica la conversión unboxing. Este es un ejemplo de código, que se puede probar agregando la clase en tiempo de ejecución **SampleComponent** a un proyecto de **componente de Windows Runtime (C++/WinRT)** y, a continuación, consumiéndolo desde (por ejemplo) un proyecto de **aplicación Core (C++/WinRT)** .

```cppwinrt
// SampleComponent.idl
namespace MyComponentProject
{
    runtimeclass SampleComponent
    {
        Windows.Foundation.IAsyncOperation<IInspectable> RetrieveCollectionAsync();
    };
}

// SampleComponent.h
...
struct SampleComponent : SampleComponentT<SampleComponent>
{
    ...
    Windows::Foundation::IAsyncOperation<Windows::Foundation::IInspectable> RetrieveCollectionAsync()
    {
        co_return Windows::Foundation::PropertyValue::CreateInt32Array({ 99, 101 }); // Box an array into a PropertyValue.
    }
}
...

// SampleCoreApp.cpp
...
MyComponentProject::SampleComponent m_sample_component;
...
auto boxed_array{ co_await m_sample_component.RetrieveCollectionAsync() };
auto property_value{ boxed_array.as<winrt::Windows::Foundation::IPropertyValue>() };
winrt::com_array<int32_t> my_array;
property_value.GetInt32Array(my_array); // Unbox back into an array.
...
```

## <a name="important-apis"></a>API importantes
* [Interfaz IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [Interfaz IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [Interfaz IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1)
* [Interfaz IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>Temas relacionados
* [Operaciones simultáneas y asincrónicas](concurrency.md)
* [Control de eventos mediante delegados en C++/WinRT](handle-events.md)