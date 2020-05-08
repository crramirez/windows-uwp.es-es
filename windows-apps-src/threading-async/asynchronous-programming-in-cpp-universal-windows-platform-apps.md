---
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: Este artículo describe la manera recomendada de consumir métodos asincrónicos en las extensiones de componentes de Visual C++ (C++/CX) usando la clase de tarea que se define en el espacio de nombres concurrency en ppltasks.h.
title: Programación asincrónica en C++
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, subprocesos, asincrónicos, C++
ms.localizationpriority: medium
ms.openlocfilehash: 73268a2aab0212277fb1caa158220cb7dbc180bf
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730049"
---
# <a name="asynchronous-programming-in-ccx"></a>Programación asincrónica en C++/CX
> [!NOTE]
> Este tema le ayudará a mantener su aplicación de C++/CX. Pero se recomienda usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para las nuevas aplicaciones. C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows.

Este artículo describe la manera recomendada de consumir métodos asincrónicos en las extensiones de componentes de Visual C++ (C++/CX) usando la clase `task` que se define en el espacio de nombres `concurrency` en ppltasks.h.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>Tipos asincrónicos de Plataforma universal de Windows (UWP)
La Plataforma universal de Windows (UWP) cuenta con un modelo bien definido para llamar a métodos asincrónicos y proporciona los tipos que necesitas para consumirlos. Si no está familiarizado con el modelo asincrónico de UWP, lea [programación asincrónica][AsyncProgramming] antes de leer el resto de este artículo.

Aunque puede consumir las API asincrónicas de Windows Runtime directamente en C++, el método preferido es usar la [**clase de tarea**][task-class] y sus tipos y funciones relacionados, que están contenidos en el espacio de nombres de [**simultaneidad**][concurrencyNamespace] y `<ppltasks.h>`se definen en. **concurrency::task** es un tipo de uso general, pero cuando se usa el conmutador de compilador **/ZW**, que es obligatorio para los componentes y las aplicaciones de la Plataforma universal de Windows (UWP), la clase task incluye los tipos asincrónicos de UWP para que sea más fácil:

-   encadenar múltiples operaciones sincrónicas y asincrónicas;

-   administrar excepciones en las cadenas de tareas;

-   realizar cancelaciones en las cadenas de tareas;

-   garantizar que las tareas individuales se ejecuten en el contexto o contenedor de subproceso apropiado.

En este artículo se proporcionan pautas básicas acerca de cómo usar la clase **task** con API asincrónicas de UWP. Para obtener documentación completa sobre la **tarea** y sus métodos relacionados, incluida la [**creación\_de tareas**][createTask], consulte paralelismo de [tareas (Runtime de simultaneidad)][taskParallelism]. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>Consumo de una operación asincrónica mediante el uso de una tarea
En el ejemplo siguiente se muestra cómo usar la clase de tarea para consumir un método **asincrónico** que devuelve una interfaz [**IAsyncOperation**][IAsyncOperation] y cuya operación genera un valor. Estos son los pasos básicos:

1.  Llama al método `create_task` y pásale el objeto **IAsyncOperation^**.

2.  Llame a la función miembro [**Task:: then**][taskThen] en la tarea y proporcione una expresión lambda que se invocará cuando se complete la operación asincrónica.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
using namespace Windows::Devices::Enumeration;
...
void App::TestAsync()
{    
    //Call the *Async method that starts the operation.
    IAsyncOperation<DeviceInformationCollection^>^ deviceOp =
        DeviceInformation::FindAllAsync();

    // Explicit construction. (Not recommended)
    // Pass the IAsyncOperation to a task constructor.
    // task<DeviceInformationCollection^> deviceEnumTask(deviceOp);

    // Recommended:
    auto deviceEnumTask = create_task(deviceOp);

    // Call the task's .then member function, and provide
    // the lambda to be invoked when the async operation completes.
    deviceEnumTask.then( [this] (DeviceInformationCollection^ devices )
    {       
        for(int i = 0; i < devices->Size; i++)
        {
            DeviceInformation^ di = devices->GetAt(i);
            // Do something with di...          
        }       
    }); // end lambda
    // Continue doing work or return...
}
```

La tarea que se crea y devuelve mediante la función [**Task:: then**][taskThen] se denomina *continuación.* El argumento de entrada (en este caso) al lambda proporcionado por el usuario es el resultado que la operación de la tarea produce cuando se completa. Es el mismo valor que se recuperaría al llamar a [**IAsyncOperation::GetResults**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_#Windows_Foundation_IAsyncOperation_1_GetResults) si estuvieras usando la interfaz **IAsyncOperation** directamente.

El método [**Task:: then**][taskThen] vuelve inmediatamente y su delegado no se ejecuta hasta que el trabajo asincrónico se complete correctamente. En este ejemplo, si la operación asincrónica hace que se inicie una excepción o finaliza en el estado Cancelado como resultado de una solicitud de cancelación, la continuación nunca se ejecutará. Más adelante, describiremos el modo en que se escriben las continuaciones que se ejecutan incluso si se canceló la tarea previa o fue errónea.

Aunque declares la variable de tarea en la pila local, administra su vigencia para que no se elimine hasta que todas las operaciones se completen y todas las referencias a ella queden fuera del alcance, incluso si el método vuelve antes de que la operación se complete.

## <a name="creating-a-chain-of-tasks"></a>Creación de una cadena de tareas
En la programación asincrónica, es común definir una secuencia de operaciones, también conocida como *cadenas de tareas*, en la que cada continuación se ejecuta solamente cuando se completa una de las anteriores. En algunos casos, la tarea anterior (o *antecedente*) produce un valor que la continuación acepta como entrada. Mediante el método [**Task:: then**][taskThen] , puede crear cadenas de tareas de forma intuitiva y directa. el método devuelve una **tarea<T> ** donde **T** es el tipo de valor devuelto de la función lambda. Puedes componer múltiples continuaciones en una cadena de tareas: `myTask.then(…).then(…).then(…);`

Las cadenas de tareas son especialmente útiles cuando una continuación crea una nueva operación asincrónica; como una tarea conocida como tarea asincrónica. El siguiente ejemplo ilustra una cadena de tareas que tiene dos continuaciones. La tarea inicial adquiere el controlador de un archivo existente, y cuando la operación se completa, la primera continuación inicia una nueva operación asincrónica para eliminar el archivo. Cuando la operación se completa, se ejecuta la segunda continuación y produce un mensaje de confirmación.

``` cpp
#include <ppltasks.h>
using namespace concurrency;
...
void App::DeleteWithTasks(String^ fileName)
{    
    using namespace Windows::Storage;
    StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;
    auto getFileTask = create_task(localFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample) ->IAsyncAction^ {       
        return storageFileSample->DeleteAsync();
    }).then([](void) {
        OutputDebugString(L"File deleted.");
    });
}
```

El ejemplo anterior ilustra cuatro puntos importantes:

-   La primera continuación convierte el objeto [**IAsyncAction ^**][IAsyncAction] en una **tarea<void> ** y devuelve la **tarea**.

-   La segunda continuación no realiza un control de errores y, por lo tanto, toma **void** y no **task<void>** como entrada. Es una continuación basada en valores.

-   La segunda continuación no se ejecuta hasta que se completa la operación [**DeleteAsync**][deleteAsync] .

-   Dado que la segunda continuación se basa en el valor, si la operación iniciada por la llamada a [**DeleteAsync**][deleteAsync] produce una excepción, la segunda continuación no se ejecuta en absoluto.

**Nota**  la creación de una cadena de tareas es solo una de las formas de utilizar la clase de **tarea** para crear operaciones asincrónicas. También puede crear operaciones mediante operadores **&&** de combinación y elección y. **||** Para obtener más información, consulte [paralelismo de tareas (Runtime de simultaneidad)][taskParallelism].

## <a name="lambda-function-return-types-and-task-return-types"></a>Tipos devueltos de tareas y tipos devueltos de función lambda
En una continuación de tarea, el tipo devuelto de la función lambda se incluye en un objeto **task**. Si el lambda devuelve una **double**, el tipo de tarea de continuación es **task<double>**. No obstante, el objeto de tarea está diseñado para que no produzca tipos devueltos anidados sin necesidad. Si un lambda devuelve una **IAsyncOperation<SyndicationFeed^>^**, la continuación devuelve una **task<SyndicationFeed^>**, no una **task<task<SyndicationFeed^>>** o **task<IAsyncOperation<SyndicationFeed^>^>^**. Este proceso se conoce como *desencapsulación asincrónica* y también garantiza que la operación asincrónica dentro de la continuación se complete antes de que se invoque a la siguiente continuación.

En el ejemplo anterior, observe que la tarea devuelve una **tarea<void> ** aunque su expresión lambda devolviera un objeto [**IAsyncInfo**][IAsyncInfo] . En la siguiente tabla se resumen los tipos de conversiones que se producen entre una función lambda y la tarea envolvente:

| | |
|--------------------------------------------------------|---------------------|
| Tipo devuelto lambda                                     | Tipo devuelto `.then` |
| TResult                                                | Task<TResult> |
| IAsyncOperation<TResult>^                        | Task<TResult> |
| IAsyncOperationWithProgress<TResult, TProgress>^ | Task<TResult> |
|IAsyncAction^                                           | Task<void>    |
| IAsyncActionWithProgress<TProgress>^             |Task<void>     |
| Task<TResult>                                    |Task<TResult>  |


## <a name="canceling-tasks"></a>Cancelar tareas
Normalmente, es bueno dar al usuario la opción de cancelar una operación asincrónica. Y en algunos casos, probablemente tengas que cancelar una operación mediante programación desde fuera de la cadena de tareas. Aunque cada \*tipo de valor devuelto **asincrónico** tiene un método [**Cancel**][IAsyncInfoCancel] que hereda de [**IAsyncInfo**][IAsyncInfo], es complicado exponerlo a métodos externos. La mejor manera de admitir la cancelación en una cadena de tareas es usar [**un\_origen\_de token de cancelación**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-source-class) para crear un [**token de cancelación\_**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-class)y, a continuación, pasar el token al constructor de la tarea inicial. Si una tarea asincrónica se crea con un token de cancelación y se [**llama\_al\_token de cancelación Source:: CANCEL**](https://docs.microsoft.com/cpp/parallel/concrt/reference/cancellation-token-source-class?view=vs-2017) , la tarea llama automáticamente a **Cancel** en la operación **IAsync\* ** y pasa la solicitud de cancelación a la cadena de continuación. El siguiente seudocódigo demuestra el enfoque básico.

``` cpp
//Class member:
cancellation_token_source m_fileTaskTokenSource;

// Cancel button event handler:
m_fileTaskTokenSource.cancel();

// task chain
auto getFileTask2 = create_task(documentsFolder->GetFileAsync(fileName),
                                m_fileTaskTokenSource.get_token());
//getFileTask2.then ...
```

Cuando se cancela una tarea, se propaga una excepción de [**\_tarea cancelada**][taskCanceled] en la cadena de tareas. Las continuaciones basadas en valores simplemente no se ejecutarán, pero las continuaciones basadas en tareas harán que se produzca la excepción cuando se llame a [**Task:: get**][taskGet] . Si tiene una continuación de control de errores, asegúrese de que detecta la excepción de **la\_tarea cancelada** explícitamente. (Esta excepción no se deriva de [**Platform::Exception**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class).)

La cancelación es cooperativa. Si tu continuación realiza trabajo de larga duración que es más que simplemente invocar un método de UWP, es tu responsabilidad comprobar el estado del token de cancelación periódicamente y detener la ejecución si se cancela. Después de limpiar todos los recursos que se asignaron en la continuación, llame a la [**tarea cancelar\_actual\_**](https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) para cancelar esa tarea y propagar la cancelación a cualquier continuación basada en valores que la siga. He aquí otro ejemplo: puedes crear una cadena de tareas que represente el resultado de una operación [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker). Si el usuario elige el botón **Cancelar** , no se llama al método [**IAsyncInfo:: CANCEL**][IAsyncInfoCancel] . En cambio, la operación se realiza correctamente, pero devuelve **nullptr**. La continuación puede probar el parámetro de entrada y llamar a la **tarea cancelar\_actual\_** si la entrada es **nullptr**.

Para obtener más información, consulta el tema sobre la [cancelación en la PPL](https://docs.microsoft.com/cpp/parallel/concrt/cancellation-in-the-ppl)

## <a name="handling-errors-in-a-task-chain"></a>Control de errores en una cadena de tareas
Si desea que una continuación se ejecute incluso si el antecedente se canceló o produjo una excepción, haga que la continuación sea una continuación basada en tareas mediante la especificación de la entrada a su función lambda como **tarea<TResult> ** o **tarea<void> ** si la expresión lambda de la tarea antecedente devuelve un [**IAsyncAction ^**][IAsyncAction].

Para administrar errores y la cancelación en una cadena de tareas, no tienes que hacer que todas las continuaciones se basen en tareas ni incluir cada operación que pueda iniciarse dentro de un bloque `try…catch`. En cambio, puedes agregar una continuación basada en tareas al final de la cadena y controlar todos los errores allí. Cualquier excepción (esto incluye una excepción de [**\_tarea cancelada**][taskCanceled] ) se propagará hacia abajo en la cadena de tareas y omitirá cualquier continuación basada en valores, de modo que pueda controlarla en la continuación basada en tareas de control de errores. Podemos reescribir el ejemplo anterior para usar una continuación basada en tareas de control de errores:

``` cpp
#include <ppltasks.h>
void App::DeleteWithTasksHandleErrors(String^ fileName)
{    
    using namespace Windows::Storage;
    using namespace concurrency;

    StorageFolder^ documentsFolder = KnownFolders::DocumentsLibrary;
    auto getFileTask = create_task(documentsFolder->GetFileAsync(fileName));

    getFileTask.then([](StorageFile^ storageFileSample)
    {       
        return storageFileSample->DeleteAsync();
    })

    .then([](task<void> t)
    {

        try
        {
            t.get();
            // .get() didn' t throw, so we succeeded.
            OutputDebugString(L"File deleted.");
        }
        catch (Platform::COMException^ e)
        {
            //Example output: The system cannot find the specified file.
            OutputDebugString(e->Message->Data());
        }

    });
}
```

En una continuación basada en tareas, se llama a la función miembro [**Task:: get**][taskGet] para obtener los resultados de la tarea. Todavía tenemos que llamar a **Task:: get** aunque la operación fuera un [**IAsyncAction**][IAsyncAction] que no produce ningún resultado porque **Task:: get** también obtiene cualquier excepción que se haya transportado a la tarea. Si la tarea de entrada está almacenando una excepción, se inicia en la llamada a **task::get**. Si no llama a **Task:: get**o no utiliza una continuación basada en tareas al final de la cadena, o no detecta el tipo de excepción que se produjo, se produce una **excepción de\_tarea\_no observada** cuando se han eliminado todas las referencias a la tarea.

Solamente captura las excepciones que puedas administrar. Si tu aplicación se encuentra con un error del que no puedes recuperarte, es mejor dejar que se bloquee a dejar que continúe ejecutándose en un estado desconocido. Además, en general, no intente detectar la propia **excepción de\_tarea\_no observada** . Esta excepción principalmente se usa para fines de diagnóstico. Cuando se produce una **excepción de\_tarea\_no observada** , normalmente indica un error en el código. Normalmente, la causa es una excepción que debe controlarse o una excepción irrecuperable que otro error en el código causó.

## <a name="managing-the-thread-context"></a>Administración del contenido del subproceso
La interfaz de usuario de una aplicación para UWP se ejecuta en un contenedor uniproceso (STA). Una tarea cuya expresión lambda devuelve [**IAsyncAction**][IAsyncAction] o [**IAsyncOperation**][IAsyncOperation] es compatible con apartamentos. De manera predeterminada, si la tarea se crea en el STA, todas sus continuaciones se ejecutarán también en él, a menos que especifiques lo contrario. En otras palabras, toda la cadena de tareas hereda el reconocimiento de apartamentos de la tarea primaria. Este comportamiento ayuda a simplificar las interacciones con los controles de la interfaz de usuario, a la que solamente puede obtenerse acceso desde el STA.

Por ejemplo, en una aplicación de UWP, en la función miembro de cualquier clase que represente una página XAML, puede rellenar un control [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) desde dentro de un método [**Task:: then**][taskThen] sin tener que usar el objeto [**Dispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) .

``` cpp
#include <ppltasks.h>
void App::SetFeedText()
{    
    using namespace Windows::Web::Syndication;
    using namespace concurrency;
    String^ url = "http://windowsteamblog.com/windows_phone/b/wmdev/atom.aspx";
    SyndicationClient^ client = ref new SyndicationClient();
    auto feedOp = client->RetrieveFeedAsync(ref new Uri(url));

    create_task(feedOp).then([this]  (SyndicationFeed^ feed)
    {
        m_TextBlock1->Text = feed->Title->Text;
    });
}
```

Si una tarea no devuelve un [**IAsyncAction**][IAsyncAction] o [**IAsyncOperation**][IAsyncOperation], no es compatible con apartamentos y, de forma predeterminada, sus continuaciones se ejecutan en el primer subproceso en segundo plano disponible.

Puede invalidar el contexto de subproceso predeterminado para cualquier tipo de tarea mediante la sobrecarga de [**Task:: then**][taskThen] que toma [**un\_contexto\_de continuación de tarea**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class). Por ejemplo, en algunos casos, probablemente quieras programar la continuación de una tarea que reconoce contenedores en un subproceso en segundo plano. En tal caso, puede pasar el [**contexto de\_continuación\_de la tarea:\_: usar arbitrario**][useArbitrary] para programar el trabajo de la tarea en el siguiente subproceso disponible en un apartamento multiproceso. Esto puede mejorar el rendimiento de la continuación porque su trabajo no tiene que estar sincronizado con otro trabajo que se esté realizando en el subproceso de interfaz de usuario.

En el ejemplo siguiente se muestra Cuándo es útil especificar el [**contexto\_de\_continuación de la tarea\_:: Use**][useArbitrary] la opción arbitraria y también se muestra cómo el contexto de continuación predeterminado es útil para sincronizar operaciones simultáneas en colecciones que no son seguras para subprocesos. En este código, repetimos una lista de URL para fuentes RSS, y para cada URL, iniciamos una operación asincrónica para recuperar los datos de fuente. No podemos controlar el orden en el que se recuperan las fuentes. En realidad, no nos interesa hacerlo. Cuando se completa cada operación [**RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.isyndicationclient.retrievefeedasync), la primera continuación acepta el objeto [**SyndicationFeed^**](https://docs.microsoft.com/uwp/api/Windows.Web.Syndication.SyndicationFeed) y lo usa para inicializar un objeto `FeedData^` definido por la aplicación. Dado que cada una de estas operaciones es independiente de las demás, podemos acelerar las cosas especificando el **contexto de\_continuación\_de la tarea:\_: usar** contexto de continuación arbitrario. No obstante, después de que se inicialice cada objeto `FeedData`, tenemos que agregarlo a un [**Vector**](https://docs.microsoft.com/cpp/cppcx/platform-collections-vector-class), que no es una colección segura para subprocesos. Por lo tanto, se crea una continuación y se especifica el [**contexto de continuación\_\_de la tarea::\_use Current**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) para asegurarse de que todas las llamadas a [**Append**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.ivector_t_.append) se producen en el mismo contexto de apartamento de un solo subproceso (asta) de la aplicación. Dado que el [**contexto de continuación\_\_de\_la tarea:: usar el valor predeterminado**](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-continuation-context-class?view=vs-2017) es el contexto predeterminado, no es necesario especificarlo explícitamente, pero lo hacemos aquí para mayor claridad.

``` cpp
#include <ppltasks.h>
void App::InitDataSource(Vector<Object^>^ feedList, vector<wstring> urls)
{
                using namespace concurrency;
    SyndicationClient^ client = ref new SyndicationClient();

    std::for_each(std::begin(urls), std::end(urls), [=,this] (std::wstring url)
    {
        // Create the async operation. feedOp is an
        // IAsyncOperationWithProgress<SyndicationFeed^, RetrievalProgress>^
        // but we don't handle progress in this example.

        auto feedUri = ref new Uri(ref new String(url.c_str()));
        auto feedOp = client->RetrieveFeedAsync(feedUri);

        // Create the task object and pass it the async operation.
        // SyndicationFeed^ is the type of the return value
        // that the feedOp operation will eventually produce.

        // Then, initialize a FeedData object by using the feed info. Each
        // operation is independent and does not have to happen on the
        // UI thread. Therefore, we specify use_arbitrary.
        create_task(feedOp).then([this]  (SyndicationFeed^ feed) -> FeedData^
        {
            return GetFeedData(feed);
        }, task_continuation_context::use_arbitrary())

        // Append the initialized FeedData object to the list
        // that is the data source for the items collection.
        // This all has to happen on the same thread.
        // By using the use_default context, we can append
        // safely to the Vector without taking an explicit lock.
        .then([feedList] (FeedData^ fd)
        {
            feedList->Append(fd);
            OutputDebugString(fd->Title->Data());
        }, task_continuation_context::use_default())

        // The last continuation serves as an error handler. The
        // call to get() will surface any exceptions that were raised
        // at any point in the task chain.
        .then( [this] (task<void> t)
        {
            try
            {
                t.get();
            }
            catch(Platform::InvalidArgumentException^ e)
            {
                //TODO handle error.
                OutputDebugString(e->Message->Data());
            }
        }); //end task chain

    }); //end std::for_each
}
```

Las tareas anidadas, que son tareas nuevas que se crean dentro de una continuación, no heredan el reconocimiento de apartamentos de la tarea inicial.

## <a name="handing-progress-updates"></a>Control de actualizaciones de progreso
Los métodos que admiten [**IAsyncOperationWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) o [**IAsyncActionWithProgress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncActionWithProgress_TProgress_) proporcionan actualizaciones de progreso periódicamente mientras que la operación está en curso, antes de que se complete. Los informes de progreso son independientes de la noción de tareas y continuaciones. Eres tan solo el delegado de la propiedad [**Progress**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) del objeto. Un uso típico del delegado es actualizar una barra de progreso en la interfaz de usuario.

## <a name="related-topics"></a>Temas relacionados
* [Crear operaciones asincrónicas en C++/CX para aplicaciones para UWP](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps)
* [Referencia del lenguaje Visual C++](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx)
* [Programación asincrónica][AsyncProgramming]
* [Paralelismo de tareas (Runtime de simultaneidad)][taskParallelism]
* [Concurrency:: Task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "Espacio de nombres de simultaneidad"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "Task (clase)"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "Paralelismo de tareas"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
