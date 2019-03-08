---
title: Patrones asincrónicos de llamada de API de C
description: Obtenga información sobre la API de C asincrónica llamando a patrones para la API de C XSAPI
ms.date: 06/10/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, programa para desarrolladores,
ms.localizationpriority: medium
ms.openlocfilehash: edc6248a363b844d94c8fa03ab7ce071cc941908
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608010"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>Patrón de llamada para XSAPI planos las llamadas asincrónicas de capa de C

Un **API asíncrona** es una API que se devuelve rápidamente, pero se inicia una **tarea asincrónica** y el resultado se devuelve una vez finalizada la tarea.

Tradicionalmente juegos tienen poco control sobre qué subproceso se ejecuta el **tarea asincrónica** y qué subproceso devuelve los resultados cuando se usa un **devolución de llamada de finalización**.  Algunos de los juegos están diseñados para que solo se toca una sección del montón de un solo subproceso para evitar cualquier necesidad de sincronización de subprocesos. Si el **devolución de llamada de finalización** no se llama desde un subproceso del juego controles, actualizar el estado compartido con el resultado de una **tarea asincrónica** requerirá la sincronización de subprocesos.

La API de C XSAPI expone una API de C asincrónica nueva que ofrece a los desarrolladores control directamente los subprocesos al realizar una **API asíncrona** llamar, como **XblSocialGetSocialRelationshipsAsync()**,  **XblProfileGetUserProfileAsync()** y **XblAchievementsGetAchievementsForTitleIdAsync()**.

Este es un ejemplo básico de una llamada a la **XblProfileGetUserProfileAsync** API.

```cpp
AsyncBlock* asyncBlock = new AsyncBlock {};
asyncBlock->queue = asyncQueue;
asyncBlock->context = customDataForCallback;
asyncBlock->callback = [](AsyncBlock* asyncBlock)
{
    XblUserProfile profile;
    if( SUCCEEDED( XblProfileGetUserProfileResult(asyncBlock, &profile) ) )
    {
        printf("Profile retrieved successfully\r\n");
    }
    delete asyncBlock;
};
XblProfileGetUserProfileAsync(asyncBlock, xboxLiveContext, xuid);
```

Para entender este patrón de llamada, deberá saber cómo se usa el **AsyncBlock** y **un elemento AsyncQueue**.

* El **AsyncBlock** lleva a cabo toda la información relativa a la **tarea asincrónica** y **devolución de llamada de finalización**.

* El **un elemento AsyncQueue** le permite determinar qué subproceso se ejecuta el **tarea asincrónica** y qué subproceso llama a la AsyncBlock **devolución de llamada de finalización**.

## <a name="the-asyncblock"></a>The **AsyncBlock**

Echemos un vistazo a la **AsyncBlock** en detalle. Es un struct que se define como sigue:

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

El **AsyncBlock** contiene:

* *devolución de llamada* -una función de devolución de llamada opcional que se llamará una vez se ha realizado el trabajo asincrónico.  Si no se especifica una devolución de llamada, puede esperar el **AsyncBlock** en completarse con **GetAsyncStatus** y, a continuación, obtener los resultados.
* *contexto* -le permite pasar datos a la función de devolución de llamada.
* *cola* : una async_queue_handle_t que es un identificador que designa un **un elemento AsyncQueue**. Si no se establece, se usará una cola predeterminada.

Debe crear un nuevo AsyncBlock en el montón para cada async llamada de API.  El AsyncBlock debe vivir hasta que se llama a la devolución de llamada de finalización del AsyncBlock y, a continuación, se puede eliminar.

> [!IMPORTANT]
> Un **AsyncBlock** deben permanecer en memoria hasta que el **tarea asincrónica** se complete. Si se asigna dinámicamente, se puede eliminar dentro de la AsyncBlock **devolución de llamada de finalización**.

### <a name="waiting-for-asynchronous-task"></a>Esperando **tarea asincrónica**

Puede indicar un **tarea asincrónica** es realizar una serie de maneras diferentes:

* el AsyncBlock **devolución de llamada de finalización** se denomina
* Llame a **GetAsyncStatus** por true para esperar hasta que se complete.
* Establecer un waitEvent en el **AsyncBlock** y espere a que se señale el evento

Con **GetAsyncStatus** y waitEvent, el **tarea asincrónica** se considera completado después de la AsyncBlock **devolución de llamada de finalización** pero se ejecuta el AsyncBlock **devolución de llamada de finalización** es opcional.

Una vez el **tarea asincrónica** está completa, puede obtener los resultados.

### <a name="getting-the-result-of-the-asynchronous-task"></a>Obtener el resultado de la **tarea asincrónica**

Para obtener el resultado, la mayoría **API asíncrona** funciones tienen su correspondiente \[nombre de función\]dar lugar a la función para recibir el resultado de la llamada asincrónica. En nuestro ejemplo de código, **XblProfileGetUserProfileAsync** le corresponde una **XblProfileGetUserProfileResult** función. Puede usar esta función para devolver el resultado de la función y actuar en consecuencia.  Consulte la documentación de cada **API asíncrona** función para obtener detalles completos sobre cómo recuperar los resultados.

## <a name="the-asyncqueue"></a>El **un elemento AsyncQueue**

El **un elemento AsyncQueue** le permite determinar qué subproceso se ejecuta el **tarea asincrónica** y qué subproceso llama a la AsyncBlock **devolución de llamada de finalización**.

Puede controlar el subproceso que realiza esta operación estableciendo un *modo envío*. Hay tres modos de envío:

* *Manual* -la cola manual no se envían automáticamente.  Es el desarrollador para enviarlos en cualquier subproceso que desean. Esto puede usarse para asignar el trabajo o la devolución de llamada del lado de una llamada asincrónica a un subproceso específico.  Esto se explica más detalladamente a continuación.

* *Grupo de subprocesos* : envía mediante un grupo de subprocesos.  El grupo de subprocesos invoca las llamadas en paralelo, realizar una llamada para ejecutar desde la cola a su vez, cuando los subprocesos de threadpool estén disponibles.  Esto es el más para usar, pero le ofrece la menor cantidad de control sobre el que se utiliza el subproceso.

* *Se ha corregido el subproceso* : envía utilizando QueueUserAPC en el subproceso que creó la cola asincrónica. Cuando se pone en cola un APC de modo de usuario, el subproceso no se dirige al llamar a la función APC, a menos que se encuentra en un estado de alerta. Un subproceso entra en un estado de alerta mediante el uso de **SleepEx**, **SignalObjectAndWait**, **WaitForSingleObjectEx**, **WaitForMultipleObjectsEx**, o **MsgWaitForMultipleObjectsEx** para realizar una operación de espera de alerta

Para crear un nuevo **un elemento AsyncQueue** deberá llamar a **CreateAsyncQueue**.

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

donde AsyncQueueDispatchMode contiene los modos de tres envío introducidos anteriormente:

```cpp
typedef enum AsyncQueueDispatchMode
{
    /// <summary>
    /// Async callbacks are invoked manually by DispatchAsyncQueue
    /// </summary>
    AsyncQueueDispatchMode_Manual,

    /// <summary>
    /// Async callbacks are queued to the thread that created the queue
    /// and will be processed when the thread is alertable.
    /// </summary>
    AsyncQueueDispatchMode_FixedThread,

    /// <summary>
    /// Async callbacks are queued to the system thread pool and will
    /// be processed in order by the threadpool.
    /// </summary>
    AsyncQueueDispatchMode_ThreadPool

} AsyncQueueDispatchMode;
```

**workDispatchMode** determina el modo de envío para el subproceso que administra el trabajo asincrónico mientras **completionDispatchMode** determina el modo de envío para el subproceso que controla la finalización de async operación.

También puede llamar a **CreateSharedAsyncQueue** para crear un **un elemento AsyncQueue** con el mismo tipo de cola especificando un identificador para la cola.

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> Si ya existe una cola con los modos de este identificador y el envío, se hará referencia.  En caso contrario, se creará una nueva cola.

Una vez que haya creado su **un elemento AsyncQueue** simplemente agréguelo a la **AsyncBlock** que controlan los subprocesos en las funciones de trabajo y la finalización.
Cuando haya terminado con el **un elemento AsyncQueue**, normalmente cuando finaliza el juego, puede cerrar con **CloseAsyncQueue**:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>Envío de forma manual un **un elemento AsyncQueue**

Si usa el modo de envío manual de cola para un **un elemento AsyncQueue** cola de trabajo o la finalización, tendrá que distribuir manualmente.
Supongamos que un **un elemento AsyncQueue** se creó en la cola de trabajo y la cola de finalización se establecen para enviar de forma manual de este modo:

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

Con el fin de distribuir el trabajo que se ha asignado **AsyncQueueDispatchMode_Manual** , tendrá que enviar con el **DispatchAsyncQueue** función.

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *cola* -qué cola para distribuir el trabajo en
* *tipo* : una instancia de la **AsyncQueueCallbackType** enum
* *timeoutInMs* -un uint32_t para el tiempo de espera en milisegundos.

Hay dos tipos de devolución de llamada definidos por el **AsyncQueueCallbackType** enum:

```cpp
typedef enum AsyncQueueCallbackType
{
    /// <summary>
    /// Async work callbacks
    /// </summary>
    AsyncQueueCallbackType_Work,

    /// <summary>
    /// Completion callbacks after work is done
    /// </summary>
    AsyncQueueCallbackType_Completion
} AsyncQueueCallbackType;
```

### <a name="when-to-call-dispatchasyncqueue"></a>Cuándo se debe llamar a **DispatchAsyncQueue**

Para comprobar si la cola ha recibido un nuevo elemento puede llamar a **AddAsyncQueueCallbackSubmitted** para establecer un controlador de eventos para que el código sepa que trabajo o las finalizaciones están listas para enviarse.

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted** toma los parámetros siguientes:

* *cola* -envía la cola asincrónica para la devolución de llamada.
* *contexto* -un puntero a datos que se deben pasar a la devolución de llamada de envío.
* *devolución de llamada* -la función que se invocará cuando se envía una devolución de llamada nuevo a la cola.
* *token* -un token que se usará en una llamada posterior a **RemoveAsynceCallbackSubmitted** para quitar la devolución de llamada.

Por ejemplo, esta es una llamada a **AddAsyncQueueCallbackSubmitted**:

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

El correspondiente **AsyncQueueCallbackSubmitted** devolución de llamada podría implementarse como sigue:

```cpp
void CALLBACK HandleAsyncQueueCallback(
    _In_ void* context,
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type)
{
    switch (type)
    {
    case AsyncQueueCallbackType::AsyncQueueCallbackType_Work:
        ReleaseSemaphore(g_workReadyHandle, 1, nullptr);
        break;
    }
}
```

A continuación, en un subproceso en segundo plano puede escuchar para que este semáforo reactivar y llamar a **DispatchAsyncQueue**.

```cpp
DWORD WINAPI BackgroundWorkThreadProc(LPVOID lpParam)
{
    HANDLE hEvents[2] =
    {
        g_workReadyHandle.get(),
        g_stopRequestedHandle.get()
    };

    async_queue_handle_t queue = static_cast<async_queue_handle_t>(lpParam);

    bool stop = false;
    while (!stop)
    {
        DWORD dwResult = WaitForMultipleObjectsEx(2, hEvents, false, INFINITE, false);
        switch (dwResult)
        {
        case WAIT_OBJECT_0: 
            // Background work is ready to be dispatched
            DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);
            break;

        case WAIT_OBJECT_0 + 1:
        default:
            stop = true;
            break;
        }
    }

    CloseAsyncQueue(queue);
    return 0;
}
```

Es mejor implementar práctica para usar con el objeto semáforo de Win32.  Si en su lugar se implementa mediante un objeto de evento de Win32, deberá asegurarse de no pierda cualquier evento con el código, como:

```cpp
    case WAIT_OBJECT_0: 
        // Background work is ready to be dispatched
        DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);        
        
        if (!IsAsyncQueueEmpty(queue, AsyncQueueCallbackType_Work))
        {
            // If there's more pending work, then set the event to process them
            SetEvent(g_workReadyHandle.get());
        }
        break;
```


Puede ver un ejemplo de los procedimientos recomendados para la integración de async en [AsyncIntegration.cpp sociales de ejemplo de C](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp)

