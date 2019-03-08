---
title: Tratamiento de errores de la API de WinRT
description: Obtenga información sobre cómo controlar los errores al realizar una llamada de servicio de Xbox Live con las APIs de WinRT.
ms.assetid: b4f04798-df91-430e-90f0-83b5a48b9ab2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, control de errores
ms.localizationpriority: medium
ms.openlocfilehash: e72dfa0b6f98284c240cf6af2dde02439d694b48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598640"
---
# <a name="winrt-api-error-handling"></a>Tratamiento de errores de la API de WinRT

En la API de WinRT, que pueden llamarse desde C++ / c++ / CX, C#, o Javascript, los errores se notifican mediante excepciones, por lo que se deben usar bloques try/catch para controlar los errores.

Tenga en cuenta que el de las llamadas asincrónicas, se necesitan dos bloques try/catch similar a éste:

```cpp
    try
    {
        auto asyncOp = xboxLiveContext->TitleStorageService->UploadBlobAsync(
            blobMetadata,
            blobBuffer,
            TitleStorageETagMatchCondition::NotUsed,
            DEFAULT_UPLOAD_BLOCK_SIZE
            );

        create_task(asyncOp)
        .then([this, ui]( task<TitleStorageBlobMetadata^> t )
        {
            try
            {
                TitleStorageBlobMetadata^ blobMetadata = t.get();

                ui->Log(L"UploadBlobAsync succeeded");
                PrintBlobMetadata(ui, blobMetadata);

                SaveNewETag(blobMetadata->ETag);
            }
            catch (Platform::Exception^ ex)
            {
                // This could happen if there's a network error or the service returns an error
                ui->Log("The async task of UploadBlobAsync failed with", ex->ToString());
            }
        });
    }
    catch (Platform::Exception^ ex)
    {
        // This could happen if there's invalid args sent to the API
        ui->Log("The API call to UploadBlobAsync failed with", ex->ToString());
    }
```

Los métodos asincrónicos XSAPI dispone de código que se ejecuta sincrónicamente cuando se llama a un método.  ¿Funciona el código sincrónico como validación de los argumentos de entrada y el inicio de las operaciones asincrónicas o acciones.  Por lo tanto, incluso llamar a los métodos asincrónicos puede producir una excepción, aunque esto no debería ocurrir para escenarios normales (es decir, errores de programador, no el error de red).  Por favor, sea consciente de esta posibilidad y programa defensiva mediante la validación de entrada y probar el código minuciosamente durante el desarrollo.  Si poner un try/catch alrededor de la llamada en Sí o colocar en un nivel superior en la pila de llamadas, lo importante es que uno.

## <a name="help-my-game-crashes-when-i-call-xyz-xbox-service-api"></a>¡Ayuda! Mi juego se bloquea durante la llamada a API de servicios de Xbox XYZ

Por lo que su juego se bloquea y la pila de llamadas se dice es una llamada API de servicio de Xbox, pero lo hace?

```cpp
msvcr110.dll!Concurrency::details::_ReportUnobservedException() Line 2455    C++
Social110Release.exe!Concurrency::details::_ExceptionHolder::~_ExceptionHolder() Line 915    C++
Social110Release.exe!Concurrency::details::_Task_impl_base::~_Task_impl_base() Line 1488    C++
Social110Release.exe!Concurrency::details::_Task_impl<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::`scalar deleting destructor'(unsigned int)    C++
Social110Release.exe!Concurrency::task<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::_ContinuationTaskHandle<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64,void,<lambda_8571b6148830c0805feee6ba9e76a692>,std::integral_constant<bool,1>,Concurrency::details::_TypeSelectorNoAsync>::`scalar deleting destructor'(unsigned int)    C++
msvcr110.dll!Concurrency::details::_TaskCollection::_NotifyCompletedChoreAndFree(Concurrency::details::_UnrealizedChore * pChore) Line 1171    C++
msvcr110.dll!Concurrency::details::_UnrealizedChore::_UnstructuredChoreWrapper(Concurrency::details::_UnrealizedChore * pChore) Line 373    C++
msvcr110.dll!Concurrency::details::InternalContextBase::Dispatch(Concurrency::DispatchState * pDispatchState) Line 1716    C++
msvcr110.dll!Concurrency::details::FreeThreadProxy::Dispatch() Line 203    C++
msvcr110.dll!Concurrency::details::ThreadProxy::ThreadProxyMain(void * lpParameter) Line 174    C++
ntdll.dll!RtlUserThreadStart(long (void *) * StartAddress, void * Argument) Line 822    C++
```

Estas pilas de llamadas son bastante confusas.  Simultaneidad, simultaneidad que...  AH Microsoft::Xbox::Services::Social::XboxUserProfile está en la pila de llamadas, por lo debe ser el culpable.  En realidad, se trata de una pila de llamadas generada por una llamada a GetUserProfileAsync para un identificador de usuario de Xbox no válido.  El código de ejemplo que generó esta pila de llamadas tiene este aspecto:

```cpp
    auto pAsyncOp = requester->ProfileService->GetUserProfileAsync("abc123"); //passing invalid Xbox User Id;

    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)
    {
        // Oops, I forgot my exception handling code here.
        // If I don't call resultTask.get() and catch any potential exception it may throw,
        // then PPL will report an unobserved exception.  That unobserved exception will cause your
        // app to crash.
    });
```

¿La pila de llamadas contiene Concurrency::_ReportUnobservedException()?  Echemos otro vistazo a la pila de llamadas anterior.  Esto es importante.  Si la pila de llamadas contiene Concurrency:: _ReportUnobservedException() ha encontrado un error en el código de juego.

PPL crea tareas, que pueden ir seguidas de otras tareas.  En el ejemplo anterior, create_task() compila la tarea para llamar a GetUserProfileAsync() y el .then() crea la siguiente tarea.  A menudo se conocen como la tarea antecedente (primero uno) y la continuación (segundo).  En el ejemplo, la continuación no tiene control de errores.  El runtime finaliza la aplicación si una tarea produce una excepción y no se detecta la excepción por la tarea o uno de sus continuaciones.

Cuando se trata de tareas de continuación, tenga en cuenta que hay realmente dos tipos diferentes.  Un tipo, la continuación basada en tareas, toma la tarea anterior como el argumento de entrada.  Esta tarea se ejecuta siempre, incluso si la tarea antecedente produce una excepción.  Para obtener el resultado de la tarea anterior, se debe llamar a .get() en el argumento.  El segundo, basado en valores, recibe el resultado de la tarea anterior directamente – es realmente azúcar, excepto por una cosa: continuaciones en función del valor no se ejecutan en absoluto si el antecedente produce una excepción.

Recomendación:  Para evitar bloqueos, usar una continuación basada en tareas al final de la cadena de continuación y rodear todas concurrency:: task::get() o simultaneidad:: task::wait() llama en bloques try/catch para controlar los errores que pueden recuperarse.

Echemos un vistazo a algunos ejemplos:

Ejemplo de continuación basada en tareas

```cpp
    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)   // Task-based continuation
    {
        try
        {
            XboxUserProfile^ result = resultTask.get();

            // success, do something
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
        }
    });
```

Ejemplo de continuación basada en valores

```cpp
    create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
        // if the task didn't complete successfully, you'd better have a task-based
        // continuation at the end of the continuation chain or the app will crash.
    })
    .then( [this] (task<void> previousTask) // Task-based continuation
    {
        try
        {
            // DO NOT IGNORE THIS THINKING IT'S NOT IMPORTANT.

            // call concurrency::task::get and handle any unobserved exception
            // so the application doesn't crash.
            previousTask.get();

            // success, continue
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
            // By handling this exception, you ensure your application will not
            // crash when calling Xbox Service APIs
        }
    });
```

Hay una tercera solución: usar continuaciones basadas en valores completamente, pero llamar a .get() o .wait() en otro subproceso y capturar la excepción no existe.  Este es un ejemplo sencillo:

```cpp
    auto getProfileTask = create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
    });
    // Note the lack of a task-based continuation with error handling at the end

    // You may call .get() or .wait() on a value-based only chain, but
    // must ensure you surround the call in a try/catch block and handle errors
    try
    {
        getProfileTask.get();     // or getProfileTask.wait();
    }
    catch (Platform::Exception^ ex)
    {
        // concurrency::task::get threw an exception
        // safely handle the error here
        // By handling this exception, you ensure your application will not
        // crash when calling Xbox Service APIs
    }
```

**Si no usas PPL** si usas AsyncOperationCompletionHandler o AsyncActionCompletionHandler en lugar de PPL, entonces tiene también un control de errores si se hace incorrectamente, se producirá en la aplicación se bloquea.  A continuación es un ejemplo que muestra cómo controlar los errores

```cpp
    try
    {
        // Example is making a service call with an invalid XboxUserId which will result in an error.
        // The completion handler properly detects the error and does not crash the app.
        requester->ProfileService->GetUserProfileAsync("abc123")->Completed
            = ref new AsyncOperationCompletedHandler<XboxUserProfile^>([=] (IAsyncOperation<XboxUserProfile^>^ operation, Windows::Foundation::AsyncStatus status)
        {
            if( status == Windows::Foundation::AsyncStatus::Completed)
            {
                // Always check the AsyncStatus before calling GetResults().
                // If status is not AsyncStatus::Completed, calls to operation->GetResults()
                // may throw an exception.
                // You can also surround this call in a try/catch block for added safety.

                XboxUserProfile^ result = operation->GetResults();

                // success, do something with the result
            }
            else if( status == Windows::Foundation::AsyncStatus::Error )
            {
                // Failed
            }
        });
    }
    catch ( Platform::COMException^ ex )
    {
        // What is this try/catch block for?
        //
        // Xbox Service APIs do have some code that runs synchronously and errors need
        // to be safely handled.  In this example, if “” was passed instead of “abc123”,
        // then an invalid argument exception would be thrown when calling GetUserProfileAsync
    // See the next section for more a more detailed explanation.
        //
        // Note: this catch block will NOT catch exceptions thrown within the completion handler.
    }
```

**¿Las excepciones y GetNextAsync()** mediante la API de paginación?  Todos los objetos de AchievementResult, LeaderboardResult, InventoryItemResult y TitleStorageBlobMetadataResult contienen un método GetNextAsync() para solicitar la página siguiente de resultados.  Hay un caso especial, cuando no hay más datos disponibles, que desencadena una excepción al llamar a GetNextAsync().  Esta excepción se produce durante la ejecución sincrónica de GetNextAsync().  En este caso, el método GetNextAsync produce INET_E_DATA_NOT_AVAILABLE (0x800C0007).

Recomendación: Asegúrese de que ajustar GetNextAsync() método llama en un bloque try/catch y controlar correctamente el caso INET_E_DATA_NOT_AVAILABLE.

```cpp
    try
    {
        // AchievementResult^ LastResult

        // Get next page of achievement results
        if(LastResult != nullptr)
        {
            auto getNextPage = LastResult->GetNextAsync(10);

            // create_task( getNextPage ) ...
        }
    }
    catch (Platform::Exception^ ex)
    {
        if (ex->HResult == INET_E_DATA_NOT_AVAILABLE)
        {
                // we hit the end of the achievements
        }
        else
        {
            // failed for unexpected reason
        }
    }
```
