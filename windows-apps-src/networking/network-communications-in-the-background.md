---
description: Para continuar la comunicación de red mientras no está en segundo plano, una aplicación puede usar tareas en segundo plano y un agente de socket o desencadenadores de canal de control.
title: Comunicaciones de red en segundo plano
ms.assetid: 537F8E16-9972-435D-85A5-56D5764D3AC2
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8db29561afa06a2f6a2be67565d59e9387240d1c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259193"
---
# <a name="network-communications-in-the-background"></a>Comunicaciones de red en segundo plano
Para continuar con la comunicación de red mientras no está en primer plano, la aplicación puede usar tareas en segundo plano y una de estas dos opciones.
- Agente de socket. Las aplicaciones que usan sockets para conexiones a largo plazo pueden delegar la propiedad de un socket a un agente de sockets del sistema cuando abandonan el primer plano. A continuación, el agente activa la aplicación cuando llega el tráfico al socket, vuelve a transferir la propiedad a la aplicación y esta procesa el tráfico que llega.
- Desencadenadores del canal de control. 

## <a name="performing-network-operations-in-background-tasks"></a>Realizar operaciones de red en tareas en segundo plano
- Use un [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.socketactivitytrigger) para activar la tarea en segundo plano cuando se recibe un paquete y necesite realizar una tarea de corta duración. Después de realizar la tarea, debe finalizar la tarea en segundo plano para ahorrar potencia.
- Use un [ControlChannelTrigger](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) para activar la tarea en segundo plano cuando se recibe un paquete y necesite realizar una tarea de larga duración.

**Marcas y condiciones relacionadas con la red**

- Agregue la condición **InternetAvailable** a su tarea en segundo plano [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para retrasar la activación de la tarea en segundo plano hasta que la pila de red se ejecute. Esta condición ahorra potencia porque la tarea en segundo plano no se ejecutará hasta que al red esté conectada. Esta condición no proporciona una activación en tiempo real.

Independientemente del desencadenador que use, establezca [IsNetworkRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder) en la tarea en segundo plano para garantizar que la red se mantenga conectada mientras se ejecuta la tarea en segundo plano. Esto indica a la infraestructura de tareas en segundo plano que debe mantener la red mientras se esté ejecutando la tarea, incluso si el dispositivo ha entrado en modo de espera conectado. Si la tarea en segundo plano no usa **IsNetworkRequested**, la tarea en segundo plano no podrá tener acceso a la red cuando esté en modo de espera conectado (por ejemplo, cuando se deshabilite la pantalla del teléfono).

## <a name="socket-broker-and-the-socketactivitytrigger"></a>El agente de sockets y SocketActivityTrigger
Si la aplicación usa las conexiones [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) o [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener), debes usar [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) y el agente de sockets para recibir una notificación cuando llegue el tráfico de la aplicación mientras no esté en primer plano.

Para que la aplicación reciba y procese los datos recibidos en un socket cuando no esté activa, debe realizar algunas configuraciones únicas al inicio y, a continuación, transferir la propiedad al agente de sockets cuando esté realizando la transición a un estado en que no esté activa.

Los pasos de configuración única son crear un desencadenador, registrar una tarea en segundo plano para el desencadenador y habilitar el socket para el agente de sockets:
  - Crea un **SocketActivityTrigger** y registra una tarea en segundo plano para el desencadenador con el parámetro TaskEntryPoint establecido en tu código para procesar un paquete recibido.
```csharp
            var socketTaskBuilder = new BackgroundTaskBuilder();
            socketTaskBuilder.Name = _backgroundTaskName;
            socketTaskBuilder.TaskEntryPoint = _backgroundTaskEntryPoint;
            var trigger = new SocketActivityTrigger();
            socketTaskBuilder.SetTrigger(trigger);
            _task = socketTaskBuilder.Register();
```
  - Llama al objeto **EnableTransferOwnership** en el socket, antes de enlazar el socket.
```csharp
           _tcpListener = new StreamSocketListener();

           // Note that EnableTransferOwnership() should be called before bind,
           // so that tcpip keeps required state for the socket to enable connected
           // standby action. Background task Id is taken as a parameter to tie wake pattern
           // to a specific background task.  
           _tcpListener. EnableTransferOwnership(_task.TaskId,SocketActivityConnectedStandbyAction.Wake);
           _tcpListener.ConnectionReceived += OnConnectionReceived;
           await _tcpListener.BindServiceNameAsync("my-service-name");
```

Cuando el socket se haya configurado correctamente y la aplicación esté a punto de suspenderse, llama a **TransferOwnership** en el socket para transferirlo a un agente de sockets. El agente supervisa el socket y activa la tarea en segundo plano cuando se reciben los datos. En el ejemplo siguiente se incluye una función de utilidad **TransferOwnership** para realizar la transferencia de sockets **StreamSocketListener**. (Ten en cuenta que cada tipo de socket tiene su propio método **TransferOwnership**, por lo que debes llamar al método apropiado del socket cuya titularidad estés transfiriendo. Es posible que el código contenga una aplicación auxiliar **TransferOwnership** sobrecargada con una implementación para cada tipo de socket que uses; de esta manera, el código **OnSuspending** seguirá siendo fácil de leer).

La aplicación transfiere la titularidad de un socket a un agente de sockets y pasa el identificador de la tarea en segundo plano usando el más apropiado de los siguientes métodos:
-   Uno de los métodos [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.datagramsocket.transferownership) en una clase [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket).
-   Uno de los métodos [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.transferownership) en una clase [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket).
-   Uno de los métodos de [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocketlistener.transferownership) en una clase [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener).

```csharp

// declare int _transferOwnershipCount as a field.

private void TransferOwnership(StreamSocketListener tcpListener)
{
    await tcpListener.CancelIOAsync();

    var dataWriter = new DataWriter();
    ++_transferOwnershipCount;
    dataWriter.WriteInt32(transferOwnershipCount);
    var context = new SocketActivityContext(dataWriter.DetachBuffer());
    tcpListener.TransferOwnership(_socketId, context);
}

private void OnSuspending(object sender, SuspendingEventArgs e)
{
    var deferral = e.SuspendingOperation.GetDeferral();

    TransferOwnership(_tcpListener);
    deferral.Complete();
}
```
En el controlador de eventos de la tarea en segundo plano:
   -  Primero, obtén un aplazamiento de la tarea en segundo plano para poder controlar el evento mediante métodos asincrónicos.
```csharp
var deferral = taskInstance.GetDeferral();
```
   -  A continuación, extrae SocketActivityTriggerDetails de los argumentos del evento y busca el motivo por el cual se generó el evento:
```csharp
var details = taskInstance.TriggerDetails as SocketActivityTriggerDetails;
    var socketInformation = details.SocketInformation;
    switch (details.Reason)
```
   -   Si el evento se generó debido a la actividad del socket, crea un DataReader en el socket, carga el lector de manera asíncrona y, a continuación, usa los datos datos según el diseño de la aplicación. Ten en cuenta que debes devolver la propiedad del socket al agente de sockets para volver a recibir notificaciones de actividad de sockets adicional.

   En el siguiente ejemplo, el texto recibido en el socket se muestra en una notificación del sistema.

```csharp
case SocketActivityTriggerReason.SocketActivity:
            var socket = socketInformation.StreamSocket;
            DataReader reader = new DataReader(socket.InputStream);
            reader.InputStreamOptions = InputStreamOptions.Partial;
            await reader.LoadAsync(250);
            var dataString = reader.ReadString(reader.UnconsumedBufferLength);
            ShowToast(dataString);
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Si el evento se generó porque caducó un temporizador persistente, entonces tu código debería enviar algún dato por el socket para mantenerlo activo y reiniciar el temporizador persistente. De nuevo, es importante devolver la propiedad del socket al agente de sockets para recibir más notificaciones de eventos:

```csharp
case SocketActivityTriggerReason.KeepAliveTimerExpired:
            socket = socketInformation.StreamSocket;
            DataWriter writer = new DataWriter(socket.OutputStream);
            writer.WriteBytes(Encoding.UTF8.GetBytes("Keep alive"));
            await writer.StoreAsync();
            writer.DetachStream();
            writer.Dispose();
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   Si el evento se generó porque el socket estaba cerrado, vuelve a establecerlo y asegúrate de que después de crear el nuevo socket transfieres su propiedad al agente de sockets. En este ejemplo, el nombre de host y el puerto se almacenan en la configuración local para que se puedan utilizar para establecer una nueva conexión de socket:

```csharp
case SocketActivityTriggerReason.SocketClosed:
            socket = new StreamSocket();
            socket.EnableTransferOwnership(taskInstance.Task.TaskId, SocketActivityConnectedStandbyAction.Wake);
            if (ApplicationData.Current.LocalSettings.Values["hostname"] == null)
            {
                break;
            }
            var hostname = (String)ApplicationData.Current.LocalSettings.Values["hostname"];
            var port = (String)ApplicationData.Current.LocalSettings.Values["port"];
            await socket.ConnectAsync(new HostName(hostname), port);
            socket.TransferOwnership(socketId);
            break;
```

   -   No olvides de completar el aplazamiento una vez hayas terminado de procesar la notificación de eventos:

```csharp
  deferral.Complete();
```

Para obtener un ejemplo completo que demuestre el uso de la clase [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) y del agente de sockets, consulta la muestra de [SocketActivityStreamSocket](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SocketActivityStreamSocket). La inicialización del socket se realiza en Scenario1\_Connect\_Connect.xaml.cs y la implementación de la tarea en segundo plano está en SocketActivityTask.cs.

Probablemente observes que la muestra llama a **TransferOwnership** en cuanto crea un nuevo socket o adquiere un socket existente, en lugar de usar el controlador de eventos **OnSuspending** para llevar a cabo esta opción tal y como se describe en este tema. Esto ocurre porque la muestra se centra en demostrar la clase [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) y no usa el socket para ninguna otra actividad mientras se está ejecutando. Es probable que la aplicación sea más compleja, por lo que debería usar **OnSuspending** para determinar cuándo llamar a **TransferOwnership**.

## <a name="control-channel-triggers"></a>Desencadenadores del canal de control
Primero, asegúrate de que estás usando los desencadenadores del canal de control (CCTs) correctamente. Si está usando las conexiones [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) o [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener), te recomendamos que use [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger). Puedes usar CCT para el elemento **StreamSocket**, pero ten en cuenta que usan más recursos y podrían no funcionar en el modo de espera conectado.

Si usa los WebSockets, [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2), [**System.Net.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) o [**Windows.Web.Http.HttpClient**](/uwp/api/windows.web.http.httpclient), debe usar [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger).

## <a name="controlchanneltrigger-with-websockets"></a>WebSockets con ControlChannelTrigger
Debes tener en cuenta algunos aspectos especiales cuando uses [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Existen algunos patrones de uso y procedimientos recomendados específicos de transporte que debes seguir al usar la clase **MessageWebSocket** o **StreamWebSocket** con **ControlChannelTrigger**. Asimismo, estos aspectos también afectan la manera en que se controlan las solicitudes para que reciban paquetes en la clase **StreamWebSocket**. Las solicitudes que reciban paquetes en la clase **MessageWebSocket** no se verán afectadas.

Los siguientes patrones de uso y procedimientos recomendados deben cumplirse cuando uses [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger):

-   Una recepción de socket pendiente debe estar expuesta en todo momento. Esto es necesario para permitir que se lleven a cabo las tareas de notificación de inserción.
-   El protocolo WebSocket define un modelo estándar para mensajes de conexión persistente. La clase [**WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) puede enviar al servidor los mensajes de conexión persistente del protocolo WebSocket que hayan iniciado los clientes. Igualmente, la aplicación debe registrar la clase **WebSocketKeepAlive** como TaskEntryPoint para KeepAliveTrigger.

Algunos aspectos especiales afectan a la manera de controlar la forma en la que las solicitudes reciben paquetes en [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket). En particular, cuando usas **StreamWebSocket** con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), la aplicación debe controlar las lecturas mediante un patrón asincrónico sin procesar, en lugar de usar el modelo **await** de C# y VB.NET o Tareas en C++. El patrón asincrónico sin procesar se ilustra en una muestra de código que encontrarás más adelante en esta sección.

Si usas el patrón asincrónico sin procesar, Windows podrá sincronizar el método [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de la tarea en segundo plano de la clase [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) mediante la devolución de la llamada de finalización recibida. Una vez devuelta la llamada de finalización, se invoca al método **Run**. Esto garantiza que la aplicación haya recibido los datos o errores antes de que se invoque al método **Run**.

Es importante observar que la aplicación debe publicar otra lectura antes de devolver el control de la llamada de finalización. Igualmente, es importante que tengas en cuenta que no se puede usar [**DataReader**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) directamente con el transporte [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket); de lo contrario, se interrumpiría la sincronización explicada anteriormente. No es posible usar el método [**DataReader.LoadAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.loadasync) en el inicio del transporte directamente. En vez de eso, puedes pasar más adelante la interfaz [**IBuffer**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IBuffer) que devolvió el método [**IInputStream.ReadAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.iinputstream.readasync) en la propiedad [**StreamWebSocket.InputStream**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocket.inputstream) al método [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.frombuffer), para continuar con el procesamiento.

En la siguiente muestra podrás ver cómo usar un patrón asincrónico sin procesar para controlar lecturas en [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket).

```csharp
void PostSocketRead(int length)
{
    try
    {
        var readBuf = new Windows.Storage.Streams.Buffer((uint)length);
        var readOp = socket.InputStream.ReadAsync(readBuf, (uint)length, InputStreamOptions.Partial);
        readOp.Completed = (IAsyncOperationWithProgress<IBuffer, uint>
            asyncAction, AsyncStatus asyncStatus) =>
        {
            switch (asyncStatus)
            {
                case AsyncStatus.Completed:
                case AsyncStatus.Error:
                    try
                    {
                        // GetResults in AsyncStatus::Error is called as it throws a user friendly error string.
                        IBuffer localBuf = asyncAction.GetResults();
                        uint bytesRead = localBuf.Length;
                        readPacket = DataReader.FromBuffer(localBuf);
                        OnDataReadCompletion(bytesRead, readPacket);
                    }
                    catch (Exception exp)
                    {
                        Diag.DebugPrint("Read operation failed:  " + exp.Message);
                    }
                    break;
                case AsyncStatus.Canceled:

                    // Read is not cancelled in this sample.
                    break;
           }
       };
   }
   catch (Exception exp)
   {
       Diag.DebugPrint("failed to post a read failed with error:  " + exp.Message);
   }
}
```

El controlador de finalización de lecturas se activará antes de invocar al método [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de la tarea en segundo plano de la clase [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Recuerda que la sincronización interna de Windows debe esperar que la aplicación devuelva la llamada de finalización de lectura. A continuación, la aplicación procesa rápidamente los datos o el error de [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) en la devolución de llamada de finalización de lectura. El mensaje mismo se procesará dentro del contexto del método **IBackgroundTask.Run**. En la siguiente muestra, te explicamos el funcionamiento de este punto mediante el uso de una cola de mensajes donde el controlador de finalización de lectura inserta el mensaje que luego se procesa en la tarea en segundo plano.

En la siguiente muestra podrás ver cómo usar el controlador de finalización de lectura mediante un patrón asincrónico sin procesar para controlar lecturas en [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket).

```csharp
public void OnDataReadCompletion(uint bytesRead, DataReader readPacket)
{
    if (readPacket == null)
    {
        Diag.DebugPrint("DataReader is null");

        // Ideally when read completion returns error,
        // apps should be resilient and try to
        // recover if there is an error by posting another recv
        // after creating a new transport, if required.
        return;
    }
    uint buffLen = readPacket.UnconsumedBufferLength;
    Diag.DebugPrint("bytesRead: " + bytesRead + ", unconsumedbufflength: " + buffLen);

    // check if buffLen is 0 and treat that as fatal error.
    if (buffLen == 0)
    {
        Diag.DebugPrint("Received zero bytes from the socket. Server must have closed the connection.");
        Diag.DebugPrint("Try disconnecting and reconnecting to the server");
        return;
    }

    // Perform minimal processing in the completion
    string message = readPacket.ReadString(buffLen);
    Diag.DebugPrint("Received Buffer : " + message);

    // Enqueue the message received to a queue that the push notify
    // task will pick up.
    AppContext.messageQueue.Enqueue(message);

    // Post another receive to ensure future push notifications.
    PostSocketRead(MAX_BUFFER_LENGTH);
}
```

Otro detalle que ofrece Websocket es el controlador de conexión persistente. El protocolo WebSocket define un modelo estándar para mensajes de conexión persistente.

Cuando use [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket), debe registrar una instancia de clase [**WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) como [**TaskEntryPoint**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint) en KeepAliveTrigger, para así permitir que se anule la suspensión de la aplicación y que esta pueda enviar mensajes de conexión persistente al servidor (punto de conexión remoto) de forma periódica. Esto debe hacerse como parte del código de registro de la aplicación en segundo plano, así como en el manifiesto del paquete.

Este punto de entrada de tarea de la clase [**Windows.Sockets.WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) debe especificarse en dos lugares:

-   Al crear el desencadenador KeepAliveTrigger en el código de origen (consulta el ejemplo siguiente).
-   En el manifiesto del paquete de la aplicación para la declaración de la tarea en segundo plano de conexión persistente.

En la siguiente muestra, se agrega una notificación del desencadenador de red y una notificación de la conexión persistente bajo el elemento &lt;Application&gt; en un manifiesto de la aplicación.

```xml
  <Extensions>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Background.PushNotifyTask">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Windows.Networking.Sockets.WebSocketKeepAlive">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
  </Extensions>
```

Las aplicaciones deben ser sumamente cuidadosas cuando deban usar una instrucción **await** en el contexto de una clase [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) y de una operación asincrónica de las clases [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket), [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket). Asimismo, puedes usar un objeto **Task&lt;bool&gt;** para registrar una clase **ControlChannelTrigger** que corresponda con las notificaciones de inserción y las conexiones persistentes de WebSocket de la clase **StreamWebSocket**, y así poder conectar el transporte. Como parte del registro, el transporte **StreamWebSocket** se establece como transporte de la clase **ControlChannelTrigger** y, a continuación, se publica una lectura. El elemento **Task.Result** se encargará de bloquear el subproceso actual hasta que todos los pasos de la tarea se ejecuten y devuelvan instrucciones en el cuerpo del mensaje. La tarea no se resolverá hasta que el método devuelva los valores "true" o "false". Gracias a ello, se garantiza que se ejecute el método completo. De igual manera, el elemento **Task** puede contener varias instrucciones de tipo **await** protegidas por el mismo elemento **Task**. Ten en cuenta que este patrón debe usarse con el objeto **ControlChannelTrigger** cuando uses **StreamWebSocket** o **MessageWebSocket** como transporte. Para aquellas operaciones que podrían tardar mucho Tiempo para completarlo (por ejemplo, una operación de lectura asincrónica típica), la aplicación debe usar el patrón asincrónico sin procesar descrito anteriormente.

La siguiente muestra registra [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) para notificaciones de inserción y conexiones persistentes de WebSocket en la clase [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket).

```csharp
private bool RegisterWithControlChannelTrigger(string serverUri)
{
    // Make sure the objects are created in a system thread
    // Demonstrate the core registration path
    // Wait for the entire operation to complete before returning from this method.
    // The transport setup routine can be triggered by user control, by network state change
    // or by keepalive task
    Task<bool> registerTask = RegisterWithCCTHelper(serverUri);
    return registerTask.Result;
}

async Task<bool> RegisterWithCCTHelper(string serverUri)
{
    bool result = false;
    socket = new StreamWebSocket();

    // Specify the keepalive interval expected by the server for this app
    // in order of minutes.
    const int serverKeepAliveInterval = 30;

    // Specify the channelId string to differentiate this
    // channel instance from any other channel instance.
    // When background task fires, the channel object is provided
    // as context and the channel id can be used to adapt the behavior
    // of the app as required.
    const string channelId = "channelOne";

    // For websockets, the system does the keepalive on behalf of the app
    // But the app still needs to specify this well known keepalive task.
    // This should be done here in the background registration as well
    // as in the package manifest.
    const string WebSocketKeepAliveTask = "Windows.Networking.Sockets.WebSocketKeepAlive";

    // Try creating the controlchanneltrigger if this has not been already
    // created and stored in the property bag.
    ControlChannelTriggerStatus status;

    // Create the ControlChannelTrigger object and request a hardware slot for this app.
    // If the app is not on LockScreen, then the ControlChannelTrigger constructor will
    // fail right away.
    try
    {
        channel = new ControlChannelTrigger(channelId, serverKeepAliveInterval,
                                   ControlChannelTriggerResourceType.RequestHardwareSlot);
    }
    catch (UnauthorizedAccessException exp)
    {
        Diag.DebugPrint("Is the app on lockscreen? " + exp.Message);
        return result;
    }

    Uri serverUriInstance;
    try
    {
        serverUriInstance = new Uri(serverUri);
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Error creating URI: " + exp.Message);
        return result;
    }

    // Register the apps background task with the trigger for keepalive.
    var keepAliveBuilder = new BackgroundTaskBuilder();
    keepAliveBuilder.Name = "KeepaliveTaskForChannelOne";
    keepAliveBuilder.TaskEntryPoint = WebSocketKeepAliveTask;
    keepAliveBuilder.SetTrigger(channel.KeepAliveTrigger);
    keepAliveBuilder.Register();

    // Register the apps background task with the trigger for push notification task.
    var pushNotifyBuilder = new BackgroundTaskBuilder();
    pushNotifyBuilder.Name = "PushNotificationTaskForChannelOne";
    pushNotifyBuilder.TaskEntryPoint = "Background.PushNotifyTask";
    pushNotifyBuilder.SetTrigger(channel.PushNotificationTrigger);
    pushNotifyBuilder.Register();

    // Tie the transport method to the ControlChannelTrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the ControlChannelTrigger object can be reused to plug in a new transport by
    // calling UsingTransport API again.
    try
    {
        channel.UsingTransport(socket);

        // Connect the socket
        //
        // If connect fails or times out it will throw exception.
        // ConnectAsync can also fail if hardware slot was requested
        // but none are available
        await socket.ConnectAsync(serverUriInstance);

        // Call WaitForPushEnabled API to make sure the TCP connection has
        // been established, which will mean that the OS will have allocated
        // any hardware slot for this TCP connection.
        //
        // In this sample, the ControlChannelTrigger object was created by
        // explicitly requesting a hardware slot.
        //
        // On systems that without connected standby, if app requests hardware slot as above,
        // the system will fallback to a software slot automatically.
        //
        // On systems that support connected standby,, if no hardware slot is available, then app
        // can request a software slot by re-creating the ControlChannelTrigger object.
        status = channel.WaitForPushEnabled();
        if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
            && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
        {
            throw new Exception(string.Format("Neither hardware nor software slot could be allocated. ChannelStatus is {0}", status.ToString()));
        }

        // Store the objects created in the property bag for later use.
        CoreApplication.Properties.Remove(channel.ControlChannelTriggerId);

        var appContext = new AppContext(this, socket, channel, channel.ControlChannelTriggerId);
        ((IDictionary<string, object>)CoreApplication.Properties).Add(channel.ControlChannelTriggerId, appContext);
        result = true;

        // Almost done. Post a read since we are using streamwebsocket
        // to allow push notifications to be received.
        PostSocketRead(MAX_BUFFER_LENGTH);
    }
    catch (Exception exp)
    {
         Diag.DebugPrint("RegisterWithCCTHelper Task failed with: " + exp.Message);

         // Exceptions may be thrown for example if the application has not
         // registered the background task class id for using real time communications
         // broker in the package manifest.
    }
    return result
}
```

Para obtener más información sobre cómo usar [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), consulta [ControlChannelTrigger StreamWebSocket sample (Muestra de ControlChannelTrigger StreamWebSocket)](https://code.msdn.microsoft.com/windowsapps/ControlChannelTrigger-91f6bed8).

## <a name="controlchanneltrigger-with-httpclient"></a>ControlChannelTrigger con HttpClient
Debes tener en cuenta algunos aspectos especiales cuando uses [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Hay algunos patrones de uso y procedimientos recomendados específicos de transporte que debes seguir al usar [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) con **ControlChannelTrigger**. Estos aspectos también afectan la manera de controlar la forma en que las solicitudes reciben paquetes en [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx).

**Nota**            La clase   [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) que usa SSL ya no se admite cuando se usa la característica de desencadenador de red y [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger).
 
Los siguientes patrones de uso y procedimientos recomendados deben cumplirse cuando uses [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger):

-   La aplicación puede necesitar establecer varias propiedades y encabezados en el objeto [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) o [HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(VS.110).aspx) en el espacio de nombres [System.Net.Http](https://msdn.microsoft.com/library/system.net.http(VS.110).aspx), antes de enviar la solicitud al URI específico.
-   Una aplicación puede necesitar una solicitud inicial para probar y configurar el transporte de manera apropiada antes de crear el transporte [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) que se va a usar con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Una vez que la aplicación determine que el transporte puede configurarse correctamente, se puede configurar un objeto [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) como el objeto de transporte que se usará con el objeto **ControlChannelTrigger**. Este proceso está diseñado para evitar que algunos escenarios interrumpan la conexión establecida en el transporte. Si se usa SSL con un certificado, una aplicación puede solicitar que se muestre un cuadro de diálogo con la entrada de PIN o si hay varios certificados entre los cuales elegir. Es posible que se requiera la autenticación de proxy y servidor. Si la autenticación de proxy o servidor expira, la conexión puede cerrarse. Una forma en que la aplicación puede abordar estos problemas de expiración de la autenticación es establecer un temporizador. Cuando se requiere una redirección HTTP, no se garantiza que pueda establecerse una segunda conexión fiable. Una solicitud de prueba inicial asegurará que la aplicación pueda usar la dirección URL redirigida más reciente antes de usar el objeto [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) como transporte con el objeto **ControlChannelTrigger**.

A diferencia de otros transportes de red, el objeto [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) no se puede pasar directamente al método [**UsingTransport**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.controlchanneltrigger.usingtransport) del objeto [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). En cambio, debes crear específicamente un objeto [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage) para usarlo con el objeto [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) y **ControlChannelTrigger**. El objeto [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage) se crea mediante el método [RtcRequestFactory.Create](https://msdn.microsoft.com/library/system.net.http.rtcrequestfactory.create). Posteriormente, el objeto [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage) creado se pasara al método **UsingTransport**.

En la siguiente muestra podrás ver cómo crear un objeto [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage) para usarlo con el objeto [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) y la clase [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger).

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

public HttpRequestMessage httpRequest;
public HttpClient httpClient;
public HttpRequestMessage httpRequest;
public ControlChannelTrigger channel;
public Uri serverUri;

private void SetupHttpRequestAndSendToHttpServer()
{
    try
    {
        // For HTTP based transports that use the RTC broker, whenever we send next request, we will abort the earlier
        // outstanding http request and start new one.
        // For example in case when http server is taking longer to reply, and keep alive trigger is fired in-between
        // then keep alive task will abort outstanding http request and start a new request which should be finished
        // before next keep alive task is triggered.
        if (httpRequest != null)
        {
            httpRequest.Dispose();
        }

        httpRequest = RtcRequestFactory.Create(HttpMethod.Get, serverUri);

        SendHttpRequest();
    }
        catch (Exception e)
    {
        Diag.DebugPrint("Connect failed with: " + e.ToString());
        throw;
    }
}
```

Algunos aspectos especiales afectan a la manera en que se controla el envío de solicitudes HTTP en la clase [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx), a la hora de iniciar la recepción de una respuesta. En particular, cuando usas [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), la aplicación debe usar un elemento "Task" para controlar los envíos en lugar de controlar el modelo **await**.

Si se usa [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx), no habrá sincronización entre el método [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de la tarea en segundo plano de [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) y la devolución de llamada de finalización recibida. Por esta razón, la aplicación solo puede usar la técnica de bloqueo HttpResponseMessage en el método **Run** y esperar hasta que se reciba la respuesta completa.

Es notablemente diferente usar [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), que usar los transportes [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket), [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) o [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket). La devolución de llamada recibida de [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) se entrega a la aplicación mediante un elemento "Task" desde el código [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx). Esto significa que la tarea de notificación de inserción **ControlChannelTrigger** se activará tan pronto como los datos o el error se despachen a la aplicación. En la siguiente muestra, el código almacenará el elemento responseTask que devolvió el método [HttpClient.SendAsync](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx), en el almacenamiento global que la tarea de notificación de inserción tomará y procesará en línea.

En la siguiente muestra, podrás ver cómo controlar solicitudes de envío en [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) cuando se usa con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger).

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

private void SendHttpRequest()
{
    if (httpRequest == null)
    {
        throw new Exception("HttpRequest object is null");
    }

    // Tie the transport method to the controlchanneltrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the controlchanneltrigger object can be reused to plugin a new transport by
    // calling UsingTransport API again.
    channel.UsingTransport(httpRequest);

    // Call the SendAsync function to kick start the TCP connection establishment
    // process for this http request.
    Task<HttpResponseMessage> httpResponseTask = httpClient.SendAsync(httpRequest);

    // Call WaitForPushEnabled API to make sure the TCP connection has been established,
    // which will mean that the OS will have allocated any hardware slot for this TCP connection.
    ControlChannelTriggerStatus status = channel.WaitForPushEnabled();
    Diag.DebugPrint("WaitForPushEnabled() completed with status: " + status);
    if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
        && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
    {
        throw new Exception("Hardware/Software slot not allocated");
    }

    // The HttpClient receive callback is delivered via a Task to the app.
    // The notification task will fire as soon as the data or error is dispatched
    // Enqueue the responseTask returned by httpClient.sendAsync
    // into a queue that the push notify task will pick up and process inline.
    AppContext.messageQueue.Enqueue(httpResponseTask);
}
```

En la siguiente muestra, se indica cómo leer las respuestas recibidas en [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) cuando se usa con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger).

```csharp
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

public string ReadResponse(Task<HttpResponseMessage> httpResponseTask)
{
    string message = null;
    try
    {
        if (httpResponseTask.IsCanceled || httpResponseTask.IsFaulted)
        {
            Diag.DebugPrint("Task is cancelled or has failed");
            return message;
        }
        // We' ll wait until we got the whole response.
        // This is the only supported scenario for HttpClient for ControlChannelTrigger.
        HttpResponseMessage httpResponse = httpResponseTask.Result;
        if (httpResponse == null || httpResponse.Content == null)
        {
            Diag.DebugPrint("Cannot read from httpresponse, as either httpResponse or its content is null. try to reset connection.");
        }
        else
        {
            // This is likely being processed in the context of a background task and so
            // synchronously read the Content' s results inline so that the Toast can be shown.
            // before we exit the Run method.
            message = httpResponse.Content.ReadAsStringAsync().Result;
        }
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Failed to read from httpresponse with error:  " + exp.ToString());
    }
    return message;
}
```

Para obtener más información sobre cómo usar [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), consulta [ControlChannelTrigger HttpClient sample (Muestra de ControlChannelTrigger HttpClient)](https://code.msdn.microsoft.com/windowsapps/ControlChannelTrigger-HTTP-9d7a6b3d).

## <a name="controlchanneltrigger-with-ixmlhttprequest2"></a>ControlChannelTrigger con IXMLHttpRequest2
Debes tener en cuenta algunos aspectos especiales cuando uses [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Existen algunos patrones de uso y procedimientos recomendados específicos de transporte que debes seguir al usar **IXMLHTTPRequest2** con **ControlChannelTrigger**. El uso de **ControlChannelTrigger** no afecta la manera en que se controlan las solicitudes para enviar o recibir solicitudes HTTP en **IXMLHTTPRequest2**.

Patrones de uso y procedimientos recomendados para usar [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)

-   Cuando se usa un objeto [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) como transporte, tiene una duración de solo una solicitud o respuesta. Cuando se usa con el objeto [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), es conveniente crear y establecer el objeto **ControlChannelTrigger** una vez y, a continuación, llamar al método [**UsingTransport**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.controlchanneltrigger.usingtransport) repetidamente, asociando cada vez un nuevo objeto **IXMLHTTPRequest2**. Una aplicación debe eliminar el objeto **IXMLHTTPRequest2** anterior antes de suministrar un nuevo objeto **IXMLHTTPRequest2**, para asegurarse de no exceder los límites de recursos asignados.
-   La aplicación puede necesitar llamar a los métodos [**SetProperty**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-setproperty) y [**SetRequestHeader**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-setrequestheader) para establecer el transporte HTTP antes de llamar al método [**Send**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-send).
-   Una aplicación puede necesitar una solicitud [**Send**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-send) inicial para probar y configurar el transporte de manera apropiada, antes de crear el transporte que se va a usar con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Una vez que la aplicación determina que el transporte está configurado correctamente, el objeto [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) puede configurarse como el objeto de transporte que se usa con **ControlChannelTrigger**. Este proceso está diseñado para evitar que algunos escenarios interrumpan la conexión establecida en el transporte. Si se usa SSL con un certificado, una aplicación puede solicitar que se muestre un cuadro de diálogo con la entrada de PIN o si hay varios certificados entre los cuales elegir. Es posible que se requiera la autenticación de proxy y servidor. Si la autenticación de proxy o servidor expira, la conexión puede cerrarse. Una forma en que la aplicación puede abordar estos problemas de expiración de la autenticación es establecer un temporizador. Cuando se requiere una redirección HTTP, no se garantiza que pueda establecerse una segunda conexión fiable. Una solicitud de prueba inicial asegurará que la aplicación pueda usar la dirección URL redirigida más reciente antes de usar el objeto **IXMLHTTPRequest2** como transporte con el objeto **ControlChannelTrigger**.

Para obtener más información sobre cómo usar [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) con [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger), consulta [ControlChannelTrigger with IXMLHTTPRequest2 sample (Muestra de ControlChannelTrigger con IXMLHTTPRequest2)](https://code.msdn.microsoft.com/windowsapps/ControlChannelTrigger-HTTP-9d7a6b3d/).

## <a name="important-apis"></a>API importantes
* [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)
* [ControlChannelTrigger](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)
