---
author: normesta
ms.assetid: 34C00F9F-2196-46A3-A32F-0067AB48291B
description: Este artículo describe la manera recomendada de consumir métodos asincrónicos en las extensiones de componentes VisualC ++ (C++ / CX) mediante el uso de la clase de tarea definida en el espacio de nombres concurrency en ppltasks.h.
title: Programación asincrónica en C++
ms.author: normesta
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, subprocesos, asincrónicos, C++
ms.localizationpriority: medium
ms.openlocfilehash: 33b110e713608260cd5c19544292e9211904a730
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5762300"
---
# <a name="asynchronous-programming-in-ccx"></a>Programación asincrónica en C++/CX
> [!NOTE]
> Este tema existe para ayudar a mantener tu aplicación de C++/CX. Pero te recomendamos que uses [C++ / WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para nuevas aplicaciones. C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime, implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows.

Este artículo describe la manera recomendada de consumir métodos asincrónicos en las extensiones de componentes VisualC ++ (C++ / CX) mediante el uso de la `task` clase que se define en el `concurrency` espacio de nombres en ppltasks.h.

## <a name="universal-windows-platform-uwp-asynchronous-types"></a>Tipos asincrónicos de Plataforma universal de Windows (UWP)
La Plataforma universal de Windows (UWP) cuenta con un modelo bien definido para llamar a métodos asincrónicos y proporciona los tipos que necesitas para consumirlos. Si no estás familiarizado con el modelo asincrónico de UWP, lee [Programación asincrónica][AsyncProgramming] antes de leer el resto de este artículo.

Aunque puedes consumir las API de UWP asincrónicas directamente en C++, el método preferido es usar la [**clase task**][task-class] y sus funciones y tipos relacionados, que se encuentran dentro del espacio de nombres [**concurrency**][concurrencyNamespace] y están definidos en `<ppltasks.h>`. **concurrency::task** es un tipo de uso general, pero cuando se usa el conmutador de compilador **/ZW**, que es obligatorio para los componentes y las aplicaciones de la Plataforma universal de Windows (UWP), la clase task incluye los tipos asincrónicos de UWP para que sea más fácil:

-   encadenar múltiples operaciones sincrónicas y asincrónicas;

-   administrar excepciones en las cadenas de tareas;

-   realizar cancelaciones en las cadenas de tareas;

-   garantizar que las tareas individuales se ejecuten en el contexto o contenedor de subproceso apropiado.

En este artículo se proporcionan pautas básicas acerca de cómo usar la clase **task** con API asincrónicas de UWP. Para obtener documentación más completa acerca de **task** y sus métodos relacionados, incluido [**create\_task**][createTask], consulta [Paralelismo de tareas (tiempo de ejecución de simultaneidad)][taskParallelism]. 

## <a name="consuming-an-async-operation-by-using-a-task"></a>Consumo de una operación asincrónica mediante el uso de una tarea
En el siguiente ejemplo se muestra cómo usar la clase task para consumir un método **async** que devuelve una interfaz de [**IAsyncOperation**][IAsyncOperation] y cuya operación produce un valor. He aquí los pasos básicos:

1.  Llama al método `create_task` y pásale el objeto **IAsyncOperation^**.

2.  Llama a la función miembro [**task::then**][taskThen] en la tarea y proporciona un lambda que se invocará cuando se complete la operación asincrónica.

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

La tarea que la función [**task::then**][taskThen] crea y devuelve se conoce como una *continuación*. El argumento de entrada (en este caso) al lambda proporcionado por el usuario es el resultado que produce la operación de la tarea cuando se completa. Es el mismo valor que se recuperaría al llamar a [**IAsyncOperation::GetResults**](https://msdn.microsoft.com/library/windows/apps/br206600) si estuvieras usando la interfaz **IAsyncOperation** directamente.

El método [**task::then**][taskThen] vuelve de inmediato y su delegado no se ejecuta hasta que el trabajo asincrónico se completa satisfactoriamente. En este ejemplo, si la operación asincrónica hace que se inicie una excepción o finaliza en el estado Cancelado como resultado de una solicitud de cancelación, la continuación nunca se ejecutará. Más adelante, describiremos el modo en que se escriben las continuaciones que se ejecutan incluso si se canceló la tarea previa o fue errónea.

Aunque declares la variable de tarea en la pila local, administra su vigencia para que no se elimine hasta que todas las operaciones se completen y todas las referencias a ella queden fuera del alcance, incluso si el método vuelve antes de que la operación se complete.

## <a name="creating-a-chain-of-tasks"></a>Creación de una cadena de tareas
En la programación asincrónica, es común definir una secuencia de operaciones, también conocida como *cadenas de tareas*, en la que cada continuación se ejecuta solamente cuando se completa una de las anteriores. En algunos casos, la tarea anterior (o *antecedente*) produce un valor que la continuación acepta como entrada. Mediante el uso del método [**task::then**][taskThen], puedes crear cadenas de tareas de una manera sencilla e intuitiva. El método devuelve **task<T>**, donde **T** es el tipo de devolución de la función lambda. Puedes componer múltiples continuaciones en una cadena de tareas:  `myTask.then(…).then(…).then(…);`

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

-   La primera continuación convierte el objeto [**IAsyncAction^**][IAsyncAction] en **task<void>** y devuelve **task**.

-   La segunda continuación no realiza un control de errores y, por lo tanto, toma **void** y no **task<void>** como entrada. Es una continuación basada en valores.

-   La segunda continuación no se ejecuta hasta que la operación de [**DeleteAsync**][deleteAsync] se completa.

-   Dado que la segunda continuación se basa en valores, si la operación que la llamada a [**DeleteAsync**][deleteAsync] comenzó inicia una excepción, la segunda continuación no se ejecuta.

**Nota**crear una cadena de tareas es solo uno de las formas de usar la clase **task** para componer operaciones asincrónicas. También puedes componer operaciones usando los operadores de unión y elección **&&** y **||**. Para obtener más información, consulta [Paralelismo de tareas (tiempo de ejecución de simultaneidad)][taskParallelism].

## <a name="lambda-function-return-types-and-task-return-types"></a>Tipos devueltos de tareas y tipos devueltos de función Lambda
En una continuación de tarea, el tipo devuelto de la función lambda se incluye en un objeto **task**. Si el lambda devuelve una **double**, el tipo de tarea de continuación es **task<double>**. No obstante, el objeto de tarea está diseñado para que no produzca tipos devueltos anidados sin necesidad. Si un lambda devuelve una **IAsyncOperation<SyndicationFeed^>^**, la continuación devuelve una **task<SyndicationFeed^>**, no una **task<task<SyndicationFeed^>>** o **task<IAsyncOperation<SyndicationFeed^>^>^**. Este proceso se conoce como *desencapsulación asincrónica* y también garantiza que la operación asincrónica dentro de la continuación se complete antes de que se invoque a la siguiente continuación.

En el ejemplo anterior, observa que la tarea devuelve **task<void>** incluso cuando la función lambda devuelve un objeto [**IAsyncInfo**][IAsyncInfo]. En la siguiente tabla se resumen los tipos de conversiones que se producen entre una función lambda y la tarea envolvente:

| | |
|--------------------------------------------------------|---------------------|
| Tipo devuelto lambda                                     | `.then` tipo devuelto |
| TResult                                                | tarea<TResult> |
| IAsyncOperation<TResult>^                        | tarea<TResult> |
| IAsyncOperationWithProgress<TResult, TProgress>^ | tarea<TResult> |
|IAsyncAction^                                           | tarea<void>    |
| IAsyncActionWithProgress<TProgress>^             |tarea<void>     |
| tarea<TResult>                                    |tarea<TResult>  |


## <a name="canceling-tasks"></a>Tareas de cancelación
Normalmente, es bueno dar al usuario la opción de cancelar una operación asincrónica. Y en algunos casos, probablemente tengas que cancelar una operación mediante programación desde fuera de la cadena de tareas. Aunque cada tipo devuelto \***Async** tenga un método [**Cancel**][IAsyncInfoCancel] que hereda de [**IAsyncInfo**][IAsyncInfo], es extraño exponerlo a métodos externos. La manera que se prefiere para admitir la cancelación en una cadena de tareas es usar un [**cancellation\_token\_source**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749985.aspx) para crear un [**cancellation\_token**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749975.aspx) y, después, pasar el token al constructor de la tarea inicial. Si se crea una tarea asincrónica con un token de cancelación y se llama a [**cancellation\_token\_source::cancel**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750076.aspx), la tarea automáticamente llama a **Cancel** en la operación **IAsync\*** y pasa la solicitud de cancelación a su cadena de continuación. El siguiente seudocódigo demuestra el enfoque básico.

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

Cuando se cancela una tarea, se propaga una excepción [**task\_canceled**][taskCanceled] hacia abajo en la cadena de tareas. Las continuaciones basadas en valores sencillamente no se ejecutarán, pero las continuaciones basadas en tareas harán que se inicie la excepción cuando se llame a [**task::get**][taskGet]. Si tienes una continuación de control de errores, asegúrate de que almacene en memoria la excepción **task\_canceled** explícitamente. (Esta excepción no se deriva de [**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755825.aspx).)

La cancelación es cooperativa. Si tu continuación realiza trabajo de larga duración que es más que simplemente invocar un método de UWP, es tu responsabilidad comprobar el estado del token de cancelación periódicamente y detener la ejecución si se cancela. Después de que limpies todos los recursos que se asignaron en la continuación, llama a [**cancel\_current\_task**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749945.aspx) para cancelar la tarea y propagar la cancelación hacia abajo a cualquier continuación basada en valores que la siga. He aquí otro ejemplo: puedes crear una cadena de tareas que represente el resultado de una operación [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/BR207871). Si el usuario elige el botón **Cancelar**, no se llama al método [**IAsyncInfo::Cancel**][IAsyncInfoCancel]. En cambio, la operación se realiza correctamente, pero devuelve **nullptr**. La continuación puede probar el parámetro de entrada y llamar a **cancel\_current\_task** si la entrada es **nullptr**.

Para obtener más información, consulta el tema sobre la [cancelación en la PPL](https://msdn.microsoft.com/library/windows/apps/xaml/dd984117.aspx)

## <a name="handling-errors-in-a-task-chain"></a>Control de errores en una cadena de tareas
Si quieres que una continuación se ejecute incluso si se canceló el antecedente o se inició una excepción, convierte la continuación en una continuación basada en tareas especificando la entrada en su función lambda como **task<TResult>** o **task<void>** si la función lambda de la tarea antecedente devuelve [**IAsyncAction**][IAsyncAction].

Para administrar errores y la cancelación en una cadena de tareas, no tienes que hacer que todas las continuaciones se basen en tareas ni incluir cada operación que pueda iniciarse dentro de un bloque `try…catch`. En cambio, puedes agregar una continuación basada en tareas al final de la cadena y controlar todos los errores allí. Cualquier excepción, y esto incluye una excepción [**task\_canceled**][taskCanceled], se propagará hacia abajo en la cadena de tareas y eludirá cualquier continuación basada en valores para que puedas administrarla en la continuación basada en tareas de control de errores. Podemos reescribir el ejemplo anterior para usar una continuación basada en tareas de control de errores:

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

En una continuación basada en tareas, llamamos a la función miembro [**task::get**][taskGet] para obtener los resultados de la tarea. Todavía tenemos que llamar a **task::get**, incluso si la operación fue una acción [**IAsyncAction**][IAsyncAction] que no produce ningún resultado, porque **task::get** también obtiene cualquier excepción que se haya transportado hacia abajo en la tarea. Si la tarea de entrada está almacenando una excepción, se inicia en la llamada a **task::get**. Si no llamas a **task::get** o no usas una continuación basada en tareas al final de la cadena, o no capturas el tipo de excepción iniciado, se inicia una **unobserved\_task\_exception** cuando se hayan eliminado todas las referencias a la tarea.

Solamente captura las excepciones que puedas administrar. Si tu aplicación se encuentra con un error del que no puedes recuperarte, es mejor dejar que se bloquee a dejar que continúe ejecutándose en un estado desconocido. Además, en general, no intentes capturar la propia **unobserved\_task\_exception**. Esta excepción principalmente se usa para fines de diagnóstico. Generalmente, cuando se inicia **unobserved\_task\_exception**, indica un error en el código. Normalmente, la causa es una excepción que debe controlarse o una excepción irrecuperable que otro error en el código causó.

## <a name="managing-the-thread-context"></a>Administración del contenido del subproceso
La interfaz de usuario de una aplicación para UWP se ejecuta en un contenedor uniproceso (STA). Una tarea cuyo lambda devuelve [**IAsyncAction**][IAsyncAction] o [**IAsyncOperation**][IAsyncOperation] reconoce contenedores. De manera predeterminada, si la tarea se crea en el STA, todas sus continuaciones se ejecutarán también en él, a menos que especifiques lo contrario. En otras palabras, toda la cadena de tareas hereda el reconocimiento de apartamentos de la tarea primaria. Este comportamiento ayuda a simplificar las interacciones con los controles de la interfaz de usuario, a la que solamente puede obtenerse acceso desde el STA.

Por ejemplo, en una aplicación para UWP, en la función miembro de cualquier clase que representa una página XAML, puedes rellenar un control [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) desde dentro de un método [**task::then**][taskThen] sin tener que usar el objeto [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211).

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

Si una tarea no devuelve [**IAsyncAction**][IAsyncAction] o [**IAsyncOperation**][IAsyncOperation], no reconoce contenedores y, de manera predeterminada, sus continuaciones se ejecutan en el primer subproceso en segundo plano disponible.

Puedes invalidar el contexto de subproceso predeterminado para cualquier tipo de tarea mediante el uso de la sobrecarga de [**task::then**][taskThen] que toma un [**task\_continuation\_context**](https://msdn.microsoft.com/library/windows/apps/xaml/hh749968.aspx). Por ejemplo, en algunos casos, probablemente quieras programar la continuación de una tarea que reconoce contenedores en un subproceso en segundo plano. En tal caso, puedes pasar [**task\_continuation\_context::use\_arbitrary**][useArbitrary] para programar el trabajo de la tarea en el siguiente subproceso disponible en un contenedor multiproceso. Esto puede mejorar el rendimiento de la continuación porque su trabajo no tiene que estar sincronizado con otro trabajo que se esté realizando en el subproceso de interfaz de usuario.

El siguiente ejemplo demuestra cuándo es útil especificar la opción [**task\_continuation\_context::use\_arbitrary**][useArbitrary] y también muestra cómo el contexto de continuación predeterminado es útil para sincronizar operaciones simultáneas en colecciones no seguras para subprocesos. En este código, repetimos una lista de URL para fuentes RSS, y para cada URL, iniciamos una operación asincrónica para recuperar los datos de fuente. No podemos controlar el orden en el que se recuperan las fuentes. En realidad, no nos interesa hacerlo. Cuando se completa cada operación [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/BR210642), la primera continuación acepta el objeto [**SyndicationFeed^**](https://msdn.microsoft.com/library/windows/apps/BR243485) y lo usa para inicializar un objeto `FeedData^` definido por la aplicación. Dado que cada una de estas operaciones es independiente de las otras, podemos acelerar el proceso especificando el contexto de continuación **task\_continuation\_context::use\_arbitrary**. No obstante, después de que se inicialice cada objeto `FeedData`, tenemos que agregarlo a un [**Vector**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441570.aspx), que no es una colección segura para subprocesos. Por lo tanto, podemos crear una continuación y especificar [**task\_continuation\_context::use\_current**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) para garantizar que todas las llamadas a [**Append**](https://msdn.microsoft.com/library/windows/apps/BR206632) se produzcan en el mismo contexto ASTA (contenedor de subproceso único de aplicaciones). Dado que [**task\_continuation\_context::use\_default**](https://msdn.microsoft.com/library/windows/apps/xaml/hh750085.aspx) es el contexto predeterminado, no tenemos que especificarlo explícitamente, pero lo hacemos aquí por razones de claridad.

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
Los métodos que admiten [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx) o [**IAsyncActionWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206581.aspx) proporcionan actualizaciones de progreso periódicamente mientras que la operación está en curso, antes de que se complete. Los informes de progreso son independientes de la noción de tareas y continuaciones. Eres tan solo el delegado de la propiedad [**Progress**](https://msdn.microsoft.com/library/windows/apps/br206594) del objeto. Un uso típico del delegado es actualizar una barra de progreso en la interfaz de usuario.

## <a name="related-topics"></a>Temas relacionados
* [Creación de operaciones asincrónicas en C++/CX aplicaciones de UWP](https://msdn.microsoft.com/library/hh750082)
* [Referencia del lenguaje Visual C++](http://msdn.microsoft.com/library/windows/apps/hh699871.aspx)
* [Programación asincrónica][AsyncProgramming]
* [Paralelismo de tareas (tiempo de ejecución de simultaneidad)][taskParallelism]
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)

<!-- LINKS -->
[AsyncProgramming]: <https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps> "AsyncProgramming"
[concurrencyNamespace]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace> "Espacio de nombres Concurrency"
[createTask]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task> "CreateTask"
[deleteAsync]: <https://msdn.microsoft.com/library/windows/apps/BR227199> "DeleteAsync"
[IAsyncAction]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.aspx> "IAsyncAction"
[IAsyncOperation]: <https://msdn.microsoft.com/library/windows/apps/BR206598> "IAsyncOperation"
[IAsyncInfo]: <https://msdn.microsoft.com/library/windows/apps/BR206587> "IAsyncInfo"
[IAsyncInfoCancel]: <https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel> "IAsyncInfoCancel"
[taskCanceled]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-canceled-class> "TaskCancelled"
[task-class]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#get> "Clase Task"
[taskGet]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750017.aspx> "TaskGet"
[taskParallelism]: <https://docs.microsoft.com/cpp/parallel/concrt/task-parallelism-concurrency-runtime> "Paralelismo de tareas"
[taskThen]: <https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class#then> "TaskThen"
[useArbitrary]: <https://msdn.microsoft.com/library/windows/apps/xaml/hh750036.aspx> "UseArbitrary"
