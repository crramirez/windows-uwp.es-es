---
author: DelfCo
description: Puedes usar Windows.Networking.Sockets y Winsock para comunicarte con otros dispositivos como un desarrollador de aplicaciones para la Plataforma universal de Windows (UWP).
title: Sockets
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
translationtype: Human Translation
ms.sourcegitcommit: 4557fa59d377edc2ae5bf5a9be63516d152949bb
ms.openlocfilehash: 432d9849335c537836fd23a4cd95c79c51bc881d

---

# Sockets

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)

Puedes usar [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) y [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523) para comunicarte con otros dispositivos como un desarrollador de aplicaciones de la Plataforma universal de Windows (UWP). Este tema proporciona instrucciones detalladas sobre el uso del espacio de nombres **Windows.Networking.Sockets** para llevar a cabo operaciones de red.

>**Nota** Como parte del [aislamiento de red](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx), el sistema prohíbe establecer conexiones de sockets (Sockets o WinSock) entre dos aplicaciones para UWP que se ejecuten en el mismo equipo, ya sea mediante la dirección de bucle invertido local (127.0.0.0) o especificando explícitamente la dirección IP local. Esto significa que no puedes usar sockets para la comunicación entre dos aplicaciones para UWP. UWP proporciona otros mecanismos para la comunicación entre aplicaciones. Consulta [App-to-app communication (Comunicación entre aplicaciones)](https://msdn.microsoft.com/windows/uwp/app-to-app/index) para obtener más información.

## Operaciones básicas de sockets TCP

Un socket TCP proporciona transferencias de datos de red de bajo nivel en cualquier dirección para conexiones de larga duración. Los sockets TCP son la característica subyacente que usan la mayoría de los protocolos de red que se usan en Internet. Esta sección muestra cómo habilitar una aplicación para UWP para enviar y recibir datos con un socket de secuencias TCP mediante las clases [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) y [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) como parte del espacio de nombres de [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960). Para esta sección, crearemos una aplicación muy sencilla que funcionará como un servidor de eco y un cliente para demostrar las operaciones básicas de TCP.

**Crear un servidor de eco TCP**

El siguiente ejemplo de código muestra cómo crear un objeto [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) y empezar a escuchar las conexiones TCP entrantes.

```csharp
try
{
    //Create a StreamSocketListener to start listening for TCP connections.
    Windows.Networking.Sockets.StreamSocketListener socketListener = new Windows.Networking.Sockets.StreamSocketListener();

    //Hook up an event handler to call when connections are received.
    socketListener.ConnectionReceived += SocketListener_ConnectionReceived;

    //Start listening for incoming TCP connections on the specified port. You can specify any port that' s not currently in use.
    await socketListener.BindServiceNameAsync("1337");
}
catch (Exception e)
{
    //Handle exception.
}
```

El siguiente ejemplo de código implementa el controlador de eventos SocketListener\_ConnectionReceived adjunto al evento [**StreamSocketListener.ConnectionReceived**](https://msdn.microsoft.com/library/windows/apps/hh701494) en el ejemplo anterior. La clase [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) llama a este controlador de eventos cada vez que un cliente remoto establece una conexión con el servidor de eco.

```csharp
private async void SocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, 
    Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    //Read line from the remote client.
    Stream inStream = args.Socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(inStream);
    string request = await reader.ReadLineAsync();
    
    //Send the line back to the remote client.
    Stream outStream = args.Socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(outStream);
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();
}
```

**Crear un cliente de eco TCP**

El siguiente ejemplo de código muestra cómo crear un objeto [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882), establecer una conexión con el servidor remoto, enviar una solicitud y recibir una respuesta.

```csharp
try
{
    //Create the StreamSocket and establish a connection to the echo server.
    Windows.Networking.Sockets.StreamSocket socket = new Windows.Networking.Sockets.StreamSocket();
    
    //The server hostname that we will be establishing a connection to. We will be running the server and client locally,
    //so we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Every protocol typically has a standard port number. For example HTTP is typically 80, FTP is 20 and 21, etc.
    //For the echo server/client application we will use a random port 1337.
    string serverPort = "1337";
    await socket.ConnectAsync(serverHost, serverPort);

    //Write data to the echo server.
    Stream streamOut = socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string request = "test";
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();

    //Read data from the echo server.
    Stream streamIn = socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string response = await reader.ReadLineAsync();
}
catch (Exception e)
{
    //Handle exception here.            
}
```

## Operaciones básicas de sockets UDP

Un socket UDP proporciona transferencias de datos de red de bajo nivel en cualquier dirección para la comunicación de red que no requiere una conexión establecida. Dado que los sockets UDP no mantienen la conexión en ambos extremos, proporcionan una solución rápida y sencilla para redes entre equipos remotos. No obstante, los sockets UDP no garantizan la integridad de los paquetes de red o si llegan al destino remoto. Algunos ejemplos de aplicaciones que usan sockets UDP son la detección de redes locales y los clientes de chat local. Esta sección muestra el uso de la clase [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) para enviar y recibir mensajes UDP mediante la creación de un sencillo servidor de eco y cliente.

**Crear un servidor de eco UDP**

El siguiente ejemplo de código muestra cómo crear un objeto [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) y enlazarlo a un puerto específico para que puedas escuchar los mensajes UDP entrantes.

```csharp
Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();

socket.MessageReceived += Socket_MessageReceived;

//You can use any port that is not currently in use already on the machine.
string serverPort = "1337";

//Bind the socket to the serverPort so that we can start listening for UDP messages from the UDP echo client.
await socket.BindServiceNameAsync(serverPort);
```

El siguiente código de ejemplo implementa el controlador de eventos **Socket\_MessageReceived** para leer un mensaje recibido de un cliente y volver a enviar el mismo mensaje.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo client.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();

    //Create a new socket to send the same message back to the UDP echo client.
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    //Use a separate port number for the UDP echo client because both will be unning on the same machine.
    string clientPort = "1338"
    Stream streamOut = (await socket.GetOutputStreamAsync(args.RemoteAddress, clientPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

**Crear un cliente de eco UDP**

El siguiente ejemplo de código muestra cómo crear un objeto [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) y enlazarlo a un puerto específico para que pueda escuchar los mensajes UDP entrantes y enviar un mensaje UDP al servidor de eco UDP.

```csharp
private async void testUdpSocketServer()
{
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    socket.MessageReceived += Socket_MessageReceived;
    
    //You can use any port that is not currently in use already on the machine. We will be using two separate and random 
    //ports for the client and server because both the will be running on the same machine.
    string serverPort = "1337";
    string clientPort = "1338";
    
    //Because we will be running the client and server on the same machine, we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Bind the socket to the clientPort so that we can start listening for UDP messages from the UDP echo server.
    await socket.BindServiceNameAsync(clientPort);
                
    //Write a message to the UDP echo server.
    Stream streamOut = (await socket.GetOutputStreamAsync(serverHost, serverPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string message = "Hello, world!";
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

El siguiente código de ejemplo implementa el controlador de eventos de **Socket\_MessageReceived** para leer un mensaje recibido de un servidor de eco UDP.

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, 
    Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo server.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();
}
```

## Operaciones en segundo plano y agente de socket

Si la aplicación recibe conexiones o datos en sockets, debes estar preparado para realizar esas operaciones correctamente mientras la aplicación no está en primer plano. Para ello, usa al agente de socket. Para obtener más información sobre cómo usar el agente de socket, consulta [Comunicaciones de red en segundo plano](network-communications-in-the-background.md).

## Envíos por lotes

A partir de Windows 10, Windows.Networking.Sockets admite el envío por lotes, un método para enviar varios búferes de datos con mucha menos sobrecarga por cambio de contexto que si envías cada uno de los búferes de forma independiente. Esto es especialmente útil tu aplicación está usando VoIP, VPN u otras tareas que implican mover una gran cantidad de datos de manera más eficaz posible.

Cada llamada a WriteAsync en un socket desencadena una transición del kernel para llegar a la pila de red. Cuando una aplicación escribe muchos búferes al mismo tiempo, cada escritura incurre en una transición de kernel independiente y esto crea una sobrecarga considerable. El nuevo modelo de envío por lotes optimiza la frecuencia de las transiciones de kernel. Esta funcionalidad actualmente está limitada a [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) e instancias de [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) conectadas.

Este es un ejemplo de cómo una aplicación podría enviar un gran número de búferes de forma no óptima.

```csharp
// Send a set of packets inefficiently, one packet at a time.
// This is not recommended if you have many packets to send
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

foreach (IBuffer packet in packetsToSend)
{
    // Incurs kernel transition overhead for each packet
    await outputStream.WriteAsync(packet);
}
```

Este ejemplo muestra una manera más eficaz de enviar un gran número de búferes. Debido a que esta técnica usa características únicas del lenguaje C#, solo está disponible para los programadores de C#. Mediante el envío de varios paquetes a la vez, este ejemplo permite al sistema envíos por lotes y, por consiguiente, optimiza las transiciones de kernel para mejorar el rendimiento.

```csharp
// More efficient way to send packets.
// This way enables the system to do batched sends
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

int i = 0;
Task[] pendingTasks = new Tast[packetsToSend.Count];
foreach (IBuffer packet in packetsToSend)
{
    // track all pending writes as tasks, but do not wait on one before peforming the next
    pendingTasks[i++] = outputStream.WriteAsync(packet).AsTask();
    // Do not modify any buffer' s contents until the pending writes are complete.
}
// Now, wait for all of the pending writes to complete
await Task.WaitAll(pendingTasks);
```

Este ejemplo muestra otra forma de enviar un gran número de búferes de manera que sea compatible con el envío por lotes. Y dado que no usa características específicas de C#, es aplicable para todos los lenguajes (aunque se muestra aquí en C#). En su lugar, usa un comportamiento modificado en el miembro **OutputStream** de las clases [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) y [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) que es nuevo en Windows 10.

```csharp
// More efficient way to send packets in Windows 10, using the new behavior of OutputStream.FlushAsync().
int i = 0;
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = socket.OutputStream;

var pendingWrites = new IAsyncOperationWithProgress<uint,uint> [packetsToSend.Count];

foreach (IBuffer packet in packetsToSend)
{
    pendingWrites[i++] = outputStream.WriteAsync(packet);
    // Do not modify any buffer' s contents until the pending writes are complete.
}

// Wait for all pending writes to complete. This step enables batched sends on the output stream.
await outputStream.FlushAsync();
```

En versiones anteriores de Windows, **FlushAsync** devolvía inmediatamente y no garantizaba la que se hayan completado todas las operaciones en la secuencia. En Windows 10, el comportamiento ha cambiado. La devolución de **FlushAsync** está ahora garantizada una vez completadas todas las operaciones del flujo de salida.

Existen algunas limitaciones importantes impuestas mediante el uso de escrituras por lote en el código.

-   No se puede modificar el contenido de las instancias de **IBuffer** que se escriben hasta que se complete la escritura asincrónica.
-   El patrón **FlushAsync** solo funciona en **StreamSocket.OutputStream** y **DatagramSocket.OutputStream**.
-   El patrón **FlushAsync** solo funciona en Windows 10 y en adelante.
-   En otros casos, usa **Task.WaitAll** en lugar del patrón **FlushAsync**.

## Uso compartido de puertos para DatagramSocket

Windows 10 introduce una propiedad [**DatagramSocketControl**](https://msdn.microsoft.com/library/windows/apps/hh701190) nueva, [**MulticastOnly**](https://msdn.microsoft.com/library/windows/apps/dn895368), que permite especificar que la **DatagramSocket** en cuestión sea capaz de coexistir con otros sockets de multidifusión Win32 o WinRT enlazados a la misma dirección y puerto.

## Proporcionar un certificado de cliente con la clase StreamSocket

La clase [**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) admite el uso de SSL/TLS para autenticar el servidor con el que se comunica la aplicación. En algunos casos, la aplicación también debe autenticarse en el servidor mediante un certificado de cliente TLS. En Windows 10, puedes proporcionar un certificado de cliente en el objeto [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) (se debe establecer antes de iniciar el protocolo de enlace TLS). Si el servidor solicita el certificado de cliente, Windows responderá con el certificado proporcionado.

Este es un fragmento de código que muestra cómo puedes implementar esto:

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

## Excepciones en Windows.Networking.Sockets

El constructor de la clase [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) que se usa con los sockets puede generar una excepción si la cadena pasada no es un nombre de host válido (contiene caracteres que no se pueden usar en un nombre de host). Si una aplicación recibe información del usuario relativa a **HostName**, el constructor debe estar en un bloque Try/Catch. Si se genera una excepción, la aplicación podrá notificar al usuario y solicitar otro nombre de host.

El espacio de nombres [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) tiene enumeraciones y métodos auxiliares convenientes para controlar errores al usar sockets y WebSockets. Esto puede ser útil para controlar de un modo diferente excepciones de red específicas en la aplicación.

Un error en una operación [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) o [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) se devuelve como un valor **HRESULT**. El método [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) se usa para convertir un error de red de una operación de socket en un valor de enumeración [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457). La mayoría de los valores de enumeración **SocketErrorStatus** corresponden a un error devuelto por la operación de Windows Sockets nativa. Una aplicación puede filtrar según valores **SocketErrorStatus** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Un error en una operación [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) o [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) se devuelve como un valor **HRESULT**. El método [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) se usa para convertir un error de red de una operación de WebSocket en un valor de enumeración [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818). La mayoría de los valores de enumeración **WebErrorStatus** corresponden a un error devuelto por la operación de cliente HTTP nativa. Una aplicación puede filtrar según valores **WebErrorStatus** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para los errores de validación de parámetros, una aplicación también puede usar el **HRESULT** de la excepción para obtener información más detallada del error que causó la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Para la mayoría de los errores de validación de parámetros, el valor de **HRESULT** devuelto es **E\_INVALIDARG**.

## API Winsock

También puedes usar [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673) en tu aplicación para UWP. La API Winsock compatible se basa en aquella de Microsoft Silverlight de Windows Phone 8.1 y sigue siendo compatible con la mayoría de los tipos, propiedades y métodos (se han quitado algunas API consideradas obsoletas). Puedes encontrar más información sobre la programación de Winsock [aquí](https://msdn.microsoft.com/library/windows/desktop/ms740673).





<!--HONumber=Jun16_HO4-->


