---
author: DelfCo
description: "Los WebSockets ofrecen un mecanismo para una comunicación bidireccional, rápida y segura entre un cliente y un servidor a través de Internet mediante HTTP(S)."
title: WebSockets
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 203face64ddb925601d23274c4e9cf9ab6d7c6f8
ms.lasthandoff: 02/07/2017

---

# <a name="websockets"></a>WebSockets

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

-   [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)
-   [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)

Los WebSockets ofrecen un mecanismo para una comunicación bidireccional, rápida y segura entre un cliente y un servidor a través de Internet mediante HTTP(S).

En el [protocolo de WebSocket](http://tools.ietf.org/html/rfc6455), los datos se transfieren inmediatamente a través de una conexión de dúplex completo y de socket único que permite que los mensajes se envíen y se reciba desde los dos extremos en tiempo real. Los WebSockets son ideales para juegos en tiempo real en los que las notificaciones instantáneas de redes sociales y la presentación actualizada de información (estadísticas de juegos) requieren una transferencia de datos segura y rápida. Los desarrolladores de Plataforma universal de Windows (UWP) pueden usar las clases [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) y [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) para conectar con los servidores que admiten el protocolo Websocket.

| MessageWebSocket                                                         | StreamWebSocket                                                                               |
|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| Apropiado para escenarios típicos en los que los mensajes no son extremadamente largos.   | Apropiado para escenarios en los que se transfieren archivos grandes (como fotos y vídeos). |
| Habilita una notificación de que se recibió un mensaje de WebSocket completo. | Permite que se lean secciones de un mensaje con cada operación de lectura.                             |
| Admite mensajes binarios y de UTF-8.                                 | Admite solamente mensajes binarios.                                                                |
| Es similar a un socket UDP o de datagramas.                                     | Es similar a un socket TCP o de flujo.                                                            |

En la mayoría de los casos, querrás usar una conexión WebSocket segura para cifrar datos enviados y recibidos. Esto también aumentará las posibilidades de que tu conexión se realice correctamente, ya que muchos servidores proxy rechazarán las conexiones WebSocket no cifradas. El protocolo WebSocket define los dos esquemas siguientes de URI.

-   ws: usado para conexiones sin cifrar
-   wss: usado para conexiones seguras que deben estar cifradas.

Para cifrar una conexión WebSocket, usa el esquema URI wss:, por ejemplo, `wss://www.contoso.com/mywebservice`.

## <a name="using-messagewebsocket"></a>Usar MessageWebSocket

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) permite que se lean secciones de un mensaje con cada operación de lectura. **MessageWebSocket** se suele usar en escenarios en los que los mensajes no son extremadamente grandes. Se admiten archivos UTF-8 y binarios.

El código de esta sección crea un nuevo [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842), se conecta a un servidor WebSocket y envía datos al servidor. Una vez que se ha establecido una conexión correctamente, la aplicación espera a que el evento [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) se active e indique que se recibieron los datos.

En esta muestra se usa el servidor eco WebSocket.org, un servicio que simplemente muestra cualquier cadena que se envía al remitente. Mediante el uso del especificador de protocolo "wss:", esta muestra usa una conexión segura para enviar y recibir mensajes.

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void Game::InitWebSockets()
> {
>     // Create a new web socket
>     m_messageWebSocket = ref new MessageWebSocket();
> 
>     // Set the message type to UTF-8
>     m_messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;
> 
>     // Register callbacks for notifications of interest
>     m_messageWebSocket->MessageReceived += 
>        ref new TypedEventHandler<MessageWebSocket^, MessageWebSocketMessageReceivedEventArgs^>(this, &Game::WebSocketMessageReceived);
>     m_messageWebSocket->Closed += ref new TypedEventHandler<IWebSocket^, WebSocketClosedEventArgs^>(this, &Game::WebSocketClosed);
> 
>     // This test code uses the websocket.org echo service to illustrate sending a string and receiving the echoed string back
>     // Note that wss: makes this an encrypted connection.
>     m_serverUri = ref new Uri("wss://echo.websocket.org");
> 
>     // Establish the connection, and set m_socketConnected on success
>     create_task(m_messageWebSocket->ConnectAsync(m_serverUri)).then([this] (task<void> previousTask)
>     {
>         try
>         {
>             // Try getting all exceptions from the continuation chain above this point.
>             previousTask.get();
> 
>             // websocket connected. update state variable
>             m_socketConnected = true;
>             OutputDebugString(L"Successfully initialized websockets\n");
>         }
>         catch (Platform::COMException^ exception)
>         {
>             // Add code here to handle any exceptions
>             // HandleException(exception);
> 
>         }
>     });
> }
> ```
> ```cs
> MessageWebSocket webSock = new MessageWebSocket();
> 
> //In this case we will be sending/receiving a string so we need to set the MessageType to Utf8.
> webSock.Control.MessageType = SocketMessageType.Utf8;
> 
> //Add the MessageReceived event handler.
> webSock.MessageReceived += WebSock_MessageReceived;
> 
> //Add the Closed event handler.
> webSock.Closed += WebSock_Closed;
> 
> Uri serverUri = new Uri("wss://echo.websocket.org");
> 
> try
> {
>     //Connect to the server.
>     await webSock.ConnectAsync(serverUri);
> 
>     //Send a message to the server.
>     await WebSock_SendMessage(webSock, "Hello, world!");
> }
> catch (Exception ex)
> {
>     //Add code here to handle any exceptions
> }
> ```

Una vez que se ha inicializado la conexión WebSocket, el código debe realizar las siguientes actividades para enviar y recibir datos correctamente.

### <a name="implement-a-callback-for-the-messagewebsocketmessagereceived-event"></a>Implementar una devolución de llamada para el evento MessageWebSocket.MessageReceived

Antes de establecer una conexión y enviar datos con un WebSocket, la aplicación debe registrar una devolución de llamada de eventos para recibir una notificación cuando se reciben datos. Cuando ocurre el evento [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358), se llama a la devolución de llamada registrada, la cual recibe datos de [**MessageWebSocketMessageReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br226852). En este ejemplo se escribe con la suposición de que los mensajes que se envíen están en formato UTF-8.

La siguiente función de muestra recibe una cadena de un servidor WebSocket conectado e imprime la cadena en la ventana de salida del depurador.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketMessageReceived(MessageWebSocket^ sender, MessageWebSocketMessageReceivedEventArgs^ args)
>{
>    DataReader^ messageReader = args->GetDataReader();
>    messageReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
>
>    String^ readString = messageReader->ReadString(messageReader->UnconsumedBufferLength);
>    // Data has been read and is now available from the readString variable.
>    swprintf(m_debugBuffer, 511, L"WebSocket Message received: %s\n", readString->Data());
>    OutputDebugString(m_debugBuffer);
>}
>```
>```csharp
>//The MessageReceived event handler.
>private void WebSock_MessageReceived(MessageWebSocket sender, MessageWebSocketMessageReceivedEventArgs args)
>{
>    DataReader messageReader = args.GetDataReader();
>    messageReader.UnicodeEncoding = UnicodeEncoding.Utf8;
>    string messageString = messageReader.ReadString(messageReader.UnconsumedBufferLength);
>
>    //Add code here to do something with the string that is received.
>}
>```

###  <a name="implement-a-callback-for-the-messagewebsocketclosed-event"></a>Registrar la devolución de llamada para el evento MessageWebSocket.Closed

Antes de establecer una conexión y enviar datos con un WebSocket, la aplicación debe registrar una devolución de llamada de eventos para recibir una notificación cuando el servidor WebSocket cierre el WebSocket. Cuando se produce el evento [**MessageWebSocket.Closed**](https://msdn.microsoft.com/library/windows/apps/hh701364), se llama a la devolución de llamada registrada para indicar que el servidor WebSocket cerró la conexión.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketClosed(IWebSocket^ sender, WebSocketClosedEventArgs^ args)
>{
>    // The method may be triggered remotely by the server sending unsolicited close frame or locally by Close()/delete operator.
>    // This method assumes we saved the connected WebSocket to a variable called m_messageWebSocket
>    if (m_messageWebSocket != nullptr)
>    {
>        delete m_messageWebSocket;
>        m_messageWebSocket = nullptr;
>        OutputDebugString(L"Socket was closed\n");
>    }
>    m_socketConnected = false;
> }
>```
>```csharp
>//The Closed event handler
>private void WebSock_Closed(IWebSocket sender, WebSocketClosedEventArgs args)
>{
>    //Add code here to do something when the connection is closed locally or by the server
>}
>```

###  <a name="send-a-message-on-a-websocket"></a>Enviar un mensaje de WebSocket

Una vez establecida una conexión, el cliente de WebSocket puede enviar datos al servidor. El método [**DataWriter.StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) devuelve un parámetro que se asigna a un entero sin signo. Esto cambia la forma en la que definimos la tarea para enviar el mensaje en comparación con la tarea para realizar la conexión.

**Nota** Cuando se crea un nuevo objeto de DataWriter mediante OutputStream de MessageWebSocket, DataWriter toma posesión de OutputStream y cancela la asignación de Outputstream cuando DataWriter queda fuera del ámbito. Esto hace que los intentos posteriores de usar el OutputStream den un error con un valor HRESULT de 0x80000013. Para evitar la cancelación de la asignación del OutputStream, este código llama al método DetachStream de DataWriter, que devuelve la propiedad del flujo del objeto WebSocket.

La siguiente función envía la cadena especificada a un WebSocket conectado e imprime un mensaje de comprobación en la ventana de salida del depurador.

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::SendWebSocketMessage(Windows::Networking::Sockets::MessageWebSocket^ sendingSocket, Platform::String^ message)
>{
>    if (m_socketConnected)
>    {
>        // WebSocket is connected, so send a message
>        m_messageWriter = ref new DataWriter(sendingSocket->OutputStream);
>
>        m_messageWriter->WriteString(message);
>
>        // Send the data as one complete message
>        create_task(m_messageWriter->StoreAsync()).then([this] (unsigned int)
>        {
>            // Send Completed
>            m_messageWriter->DetachStream();    // give the stream back to m_messageWebSocket
>            OutputDebugString(L"Sent websocket message\n");
>        })
>            .then([this] (task<void>> previousTask)
>        {
>            try
>            {
>                // Try getting all exceptions from the continuation chain above this point.
>                previousTask.get();
>            }
>            catch (Platform::COMException ^ex)
>            {
>                // Add code to handle the exception
>                // HandleException(exception);
>            }
>        });
>    }
>}
>```
>```csharp
>//Send a message to the server.
>private async Task WebSock_SendMessage(MessageWebSocket webSock, string message)
>{
>    DataWriter messageWriter = new DataWriter(webSock.OutputStream);
>    messageWriter.WriteString(message);
>    await messageWriter.StoreAsync();
>}
>```

## <a name="using-advanced-controls-with-websockets"></a>Uso de controles avanzados con WebSockets

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) y [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) siguen el mismo modelo a la hora de usar controles avanzados. En correspondencia con cada una de las clases principales mencionadas antes hay clases relacionadas para obtener acceso a los controles avanzados:

[**MessageWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226843) proporciona datos de control de socket en un objeto [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842).
[**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) proporciona datos de control de socket en un objeto [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
El modelo básico para usar controles avanzados es el mismo para ambos tipos de WebSockets. El siguiente análisis usa un [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) como ejemplo, pero el mismo proceso puede aplicarse con un [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842).

1.  Crea el objeto [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
2.  Usa la propiedad [**StreamWebSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226934) para recuperar la instancia [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) asociada con este objeto [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923).
3.  Obtén o establece propiedades en la instancia [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) para obtener o establecer controles avanzados específicos.

Tanto [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) y [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) imponen requisitos cuando se pueden establecer controles avanzados.

-   Para los demás controles avanzados en el [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923), la aplicación debe establecer siempre la propiedad antes de emitir una operación de conexión. Por ello, es recomendable establecer todas las propiedades de control inmediatamente después de crear el objeto **StreamWebSocket**. No intentes establecer una propiedad de control después de que se haya llamado al método [**StreamWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226933).
-   Para todos los controles avanzados del [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) excepto el tipo de mensaje, debes establecer la propiedad antes de emitir una operación de conexión. Es recomendable establecer todas las propiedades de control inmediatamente después de crear el objeto **MessageWebSocket**. Excepto para el tipo de mensaje, no intentes cambiar la propiedad de un control después de que se haya llamado a [**MessageWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226859).

## <a name="websocket-information-classes"></a>Clases de información de WebSocket

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) y [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) tienen una clase correspondiente que proporciona información adicional sobre una instancia de WebSocket.

[**MessageWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226849) proporciona información sobre un [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) y recuperas una instancia de la clase de información con la propiedad [**MessageWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226861).

[**StreamWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226929) proporciona información sobre un [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) y recuperas una instancia de la clase de información con la propiedad [**StreamWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226935).

Ten en cuenta que todas las propiedades de las dos clases de información son de solo lectura, y que se puede recuperar la información actual en cualquier momento durante el ciclo de vida de un objeto de WebSocket.

## <a name="handling-network-exceptions"></a>Controlar excepciones de red

Un error en una operación [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) o [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) se devuelve como un valor **HRESULT**. El método [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) se usa para convertir un error de red de una operación de WebSocket en un valor de enumeración [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). La mayoría de los valores de enumeración **WebErrorStatus** corresponden a un error devuelto por la operación de cliente HTTP nativa. Tu aplicación puede filtrar según valores de enumeración **WebErrorStatus** específicos para modificar el comportamiento de la aplicación en función de la causa de la excepción.

Para los errores de validación de parámetros, una aplicación también puede usar el **HRESULT** de la excepción para obtener información más detallada del error que causó la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Para la mayoría de los errores de validación de parámetros, el valor de **HRESULT** devuelto es **E\_INVALIDARG**.

## <a name="setting-timeouts-on-websocket-operations"></a>Establecimiento de tiempos de espera en operaciones de WebSocket

Las clases MessageWebSocket y StreamWebSocket usan un servicio interno para enviar solicitudes de cliente de WebSocket y recibir respuestas de un servidor. El valor de tiempo de espera predeterminado que se usa en una operación de conexión de WebSocket es 60 segundos. Si el servidor HTTP que admite WebSockets está temporalmente inactivo o bloqueado por una interrupción de la red y el servidor no responde o no puede responder a la solicitud de conexión de WebSocket, el servicio del sistema interno espera los 60 segundos predeterminados antes de devolver un error que provoca el inicio de una excepción en el método WebSocket de ConnectAsync. Si la consulta del nombre de un servidor HTTP del URI devuelve varias direcciones IP para el nombre, el servicio del sistema interno prueba hasta 5 direcciones IP para el sitio, cada una de ellas con un tiempo de espera predeterminado de 60 segundos antes de producir un error. Una aplicación que realiza una solicitud de conexión de WebSocket o servicio web podría esperar varios minutos intentando conectar a diversas direcciones IP antes de que se devolviera un error y se iniciara una excepción. El usuario podría percibir este comportamiento como si la aplicación hubiera dejado de funcionar. El tiempo de espera predeterminado que se usa para las operaciones de envío y recepción después de que se haya establecido una conexión de WebSocket es de 30 segundos.

Para que la aplicación responda mejor y se minimicen estos problemas, puedes establecer un tiempo de espera menor en las solicitudes de conexión para que las operaciones dieran error antes por el tiempo de espera y no por la configuración predeterminada.

Los tiempos de espera se establecen de forma similar para los StreamWebSockets y los MessageWebSockets. El siguiente ejemplo muestra cómo establecer un tiempo de espera en StreamWebSocket, pero el proceso es similar para un MessageWebSocket.

1.  Crea una tarea que finalice tras una demora especificada con un temporizador.
2.  Crea una tarea para la operación de WebSocket con un cancellation\_token\_source para que admita la cancelación.
3.  Si la tarea con el retraso de tiempo de espera especificado finaliza antes que la operación de conexión de WebSocket, cancela la tarea de la operación de WebSocket.

El siguiente ejemplo crea una tarea que finaliza tras una demora especificada y crea una segunda tarea que se cancela tras una demora especificada. Estas clases se pueden usar con StreamWebSocket y MessageWebSocket al intentar establecer una conexión para fijar un tiempo de espera específico. Un ejemplo de uso sería llamar al método StreamWebSocket.ConnectAsync en una tarea con un cancellation\_token\_source que admita la cancelación. Si se completa primero el tiempo de espera, se usa el cancellation\_token\_source para cancelar la tarea en la operación de conexión de WebSocket.

```cpp
    #include <agents.h>
    #include <ppl.h>
    #include <ppltasks.h>

    using namespace concurrency;
    using namespace std;

    // Creates a task that completes after the specified delay.
    task<void> complete_after(unsigned int timeout)
    {
        // A task completion event that is set when a timer fires.
        task_completion_event<void> tce;

        // Create a non-repeating timer.
        shared_ptr<timer<int>> fire_once(new timer<int>(timeout, 0, nullptr, false));
        
        // Create a call object that sets the completion event after the timer fires.
        shared_ptr<call<int>> callback(new call<int>([tce](int)
        {
            tce.set();
        }));

        // Connect the timer to the callback and start the timer.
        fire_once->link_target(callback.get());
        fire_once->start();

        // Create a task that completes after the completion event is set.
        task<void> event_set(tce);

        // Create a continuation task that cleans up resources and
        // and return that continuation task.
        return event_set.then([callback, fire_once]()
        {
        });
    }

    // Cancels the provided task after the specifed delay, if the task
    // did not complete.
    template<typename T>
    task<T> cancel_after_timeout(task<T> t, cancellation_token_source cts, unsigned int timeout)
    {
        // Create a task that returns true after the specified task completes.
        task<bool> success_task = t.then([](T)
        {
            return true;
        });
        // Create a task that returns false after the specified timeout.
        task<bool> failure_task = complete_after(timeout).then([]
        {
            return false;
        });

        // Create a continuation task that cancels the overall task  
        // if the timeout task finishes first. 
        return (failure_task || success_task).then([t, cts](bool success)
        {
            if (!success)
            {
                // Set the cancellation token. The task that is passed as the 
                // t parameter should respond to the cancellation and stop 
                // as soon as it can.
                cts.cancel();
            }
 
            // Return the original task.
            return t;
        });
    }
```


