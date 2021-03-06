---
description: Obtenga información sobre cómo habilitar la aplicación para redes agregando elementos de capacidad de red, proporcionando autenticación y proporcionando una conexión segura.
title: Conceptos básicos de redes
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 42c82b8cfdfca3287b8943760ef0b47180425af4
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304597"
---
# <a name="networking-basics"></a>Conceptos básicos de redes
Cosas que debes hacer para cualquier aplicación habilitada para la red

## <a name="capabilities"></a>Funcionalidades
Para usar la funciones de red, debes agregar elementos de la funcionalidad apropiada al manifiesto de la aplicación. Si no se especifica ninguna funcionalidad de red en el manifiesto de la aplicación, la aplicación no tendrá ninguna funcionalidad de red y se producirá un error de cualquier intento de conexión a la red.

Definamos las funcionalidades de red más usadas:

| Capacidad | Descripción |
|------------|-------------|
| **internetClient** | Proporciona acceso saliente a Internet y a redes de lugares públicos, como aeropuertos y cafeterías. La mayoría de las aplicaciones que requieren acceso a Internet deben usar esta funcionalidad. |
| **internetClientServer** | Proporciona a la aplicación acceso entrante y saliente a Internet de redes en lugares públicos, como aeropuertos y cafeterías. |
| **privateNetworkClientServer** | Otorga a la aplicación acceso entrante y saliente a la Red en lugares de confianza del usuario, como su hogar o el trabajo. |

En determinadas circunstancias, existen otras funcionalidades que pueden ser necesarias para la aplicación.

| Capacidad | Descripción |
|------------|-------------|
| **enterpriseAuthentication** | Permite que una aplicación se conecte a los recursos de red que precisan credenciales de dominio. Por ejemplo, una aplicación que recupera datos de servidores SharePoint en una Intranet privada. Con esta funcionalidad puedes usar tus credenciales para acceder a recursos de la red en una red que exija credenciales. Una aplicación con esta funcionalidad puede suplantarte en la red. No necesitas esta funcionalidad para que la aplicación pueda obtener acceso a Internet a través de un proxy de autenticación.<br/><br/>Para obtener más información, consulta la documentación para el escenario de la funcionalidad de *Enterprise* en [Funcionalidades restringidas](../packaging/app-capability-declarations.md#restricted-capabilities). |
| **proximity** | Es necesaria para la comunicación de datos en proximidad con dispositivos que se encuentran cerca del equipo. La comunicación de datos en proximidad puede usarse para realizar envíos o para conectar con una aplicación de un dispositivo cercano. <br/><br/> Esta funcionalidad permite que la aplicación acceda a la red para conectarse a un dispositivo en proximidad, con el consentimiento del usuario para enviar una invitación o aceptarla. |
| **sharedUserCertificates** | Esta funcionalidad permite que la aplicación obtenga acceso a los certificados de software y hardware como, por ejemplo, los certificados de una tarjeta inteligente. Cuando se invoca esta funcionalidad en tiempo de ejecución, el usuario debe realizar ciertas acciones, como insertar una tarjeta o seleccionar un certificado. <br/><br/> Con esta funcionalidad, se usan los certificados de software y hardware o una tarjeta inteligente para la identificación en la aplicación. Esta funcionalidad la pueden usar para la identificación el empleador, el banco o los servicios gubernamentales. |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>Comunicación cuando la aplicación no está en primer plano
El artículo [Support your app with background tasks (Consigue que tu aplicación sea compatible con tareas en segundo plano)](../launch-resume/support-your-app-with-background-tasks.md) contiene información general sobre el uso de tareas en segundo plano para que realicen trabajos cuando la aplicación no esté en primer plano. Más concretamente, tu código debe realizar unos pasos especiales para recibir notificaciones cuando no es la aplicación en primer plano actual y llegan datos a través de la red para esta. Usaste desencadenadores de canal de control para este propósito en Windows 8 y aún se admiten en Windows 10. Encontrarás información completa acerca del uso de desencadenadores de canal de control [**aquí**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger). Esta nueva tecnología de Windows 10 proporciona una mejor funcionalidad con menos sobrecarga en algunos escenarios, como los siguientes sockets de secuencia habilitados para la inserción: el agente de sockets y los desencadenadores de actividad de socket.

Si la aplicación usa [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) o [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener), podrá transferir la propiedad de un socket abierto a un agente de sockets proporcionado por el sistema y, a continuación, salir del primer plano o incluso cerrarse. Cuando se establece una conexión en el socket transferido o si llega tráfico a ese socket, se activa la aplicación o su tarea en segundo plano designada. Si la aplicación no se está ejecutando, se iniciará. A continuación, el agente de sockets notifica a la aplicación mediante la clase [**SocketActivityTrigger**](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger), que hay tráfico nuevo. Es entonces cuando la aplicación recupera el socket desde el agente de sockets y procesa el tráfico en el mismo socket. Esto significa que la aplicación consume menos recursos del sistema cuando no se está procesando activamente el tráfico de red.

El agente de socket está pensado para reemplazar los desencadenadores de canal de control cuando sea aplicable, ya que proporciona la misma funcionalidad, pero con menos restricciones y una menor superficie de memoria. El agente de socket puede usarse en aplicaciones que no son aplicaciones con pantalla de bloqueo y se usa la misma manera en los teléfonos como en otros dispositivos. Las aplicaciones no necesitan estar ejecutándose cuando llegue el tráfico para que sean activadas por el agente de socket. Además, el agente de socket admite la capacidad de escuchar en los sockets TCP, que no son compatibles con los desencadenadores de canal de control.

### <a name="choosing-a-network-trigger"></a>Elección de un desencadenador de red
Hay algunos escenarios donde cualquier tipo de desencadenador sería adecuado. Cuando elijas qué tipo de desencadenador usar en tu aplicación, ten en cuenta lo siguiente.

-   SI estás usando [**IXMLHTTPRequest2**](/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2), [**System.Net.Http.HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) o [System.Net.Http.HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler), debes usar [**ControlChannelTrigger**](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger).
-   Si estás usando elementos **StreamSockets** habilitados para la inserción, puedes usar desencadenadores de canal de control, pero deberías tener como preferencia la clase [**SocketActivityTrigger**](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger). La última opción permite que el sistema tenga más espacio en la memoria y reduzca los requisitos de energía cuando la conexión no se use activamente.
-   Si deseas minimizar la superficie de memoria de la aplicación cuando no está dando servicio activamente a las solicitudes de red, ten como preferencia [**SocketActivityTrigger**](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger), cuando te sea posible.
-   Si quieres que tu aplicación pueda recibir datos mientras el sistema está en modo de espera conectado, usa [**SocketActivityTrigger**](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger).

Para ver detalles y ejemplos acerca de cómo usar el agente de sockets, consulta [Comunicaciones de red en segundo plano](network-communications-in-the-background.md).

## <a name="secured-connections"></a>Conexiones seguras
Los protocolos Capa de sockets seguros (SSL) y Seguridad de la capa de transporte (TLS) son protocolos criptográficos diseñados para proporcionar autenticación y cifrado para comunicaciones de red. Estos protocolos están diseñados para evitar interceptaciones y alteraciones cuando se envían y reciben datos de red. Usan un modelo de cliente-servidor para los intercambios de protocolos. También usan certificados digitales y entidades de certificación para comprobar que el servidor sea realmente el que dice ser.

### <a name="creating-secure-socket-connections"></a>Crear conexiones de sockets seguras
Un objeto [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) se puede configurar para usar SSL/TLS en comunicaciones entre el cliente y el servidor. Esta compatibilidad con SSL/TLS se limita al uso del objeto **StreamSocket** como cliente en la negociación de SSL/TLS. No se puede usar SSL/TLS con el objeto **StreamSocket** que haya creado la clase [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener) cuando se reciben comunicaciones entrantes, porque la clase **StreamSocket** no implementa la negociación de SSL/TLS como servidor.

Hay dos formas de asegurar una conexión de [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) con SSL/TLS:

-   [**ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync): establece la conexión inicial a un servicio de red y negocia inmediatamente el uso de SSL/TLS de todas las comunicaciones.
-   [**UpgradeToSslAsync**](/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync): conecta inicialmente con un servicio de red sin cifrado. La aplicación puede enviar o recibir datos. A continuación, actualiza la conexión para usar SSL/TLS en todas las comunicaciones.

El SocketProtectionLevel especifica el nivel de protección de socket deseado con el que la aplicación quiere establecer o actualizar la conexión. Sin embargo, el nivel de protección definitivo de la conexión establecida se determina en un proceso de negociación entre ambos puntos de la conexión. El resultado puede ser un nivel de protección inferior que el especificado, si el otro punto de conexión solicita un nivel inferior. 

 Una vez finalizada correctamente la operación asincrónica, puedes recuperar el nivel de protección solicitado que se usó en la llamada [**ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync) o [**UpgradeToSslAsync**](/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) a través de la propiedad [**StreamSocketinformation.ProtectionLevel**](/uwp/api/windows.networking.sockets.streamsocketinformation.protectionlevel). Sin embargo, esto no refleja el nivel de protección real que la conexión está usando.

> [!NOTE]
> El código no debe depender implícitamente del uso de un nivel de protección concreto, ni de la suposición de que se usa un nivel de seguridad específico de manera predeterminada. El panorama de seguridad cambia constantemente, y los protocolos y niveles de protección predeterminados cambian con el tiempo para evitar el uso de protocolos con puntos débiles conocidos. Los valores predeterminados pueden variar según la configuración del equipo individual o según el software instalado y las revisiones aplicadas. Si la aplicación depende del uso de un nivel de seguridad concreto, debes especificar explícitamente ese nivel y, a continuación, comprobar que realmente se usa en la conexión establecida.

### <a name="use-connectasync"></a>Usar ConnectAsync
[**ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync) se puede usar para establecer la conexión inicial a un servicio de red y, a continuación, negociar inmediatamente el uso de SSL/TLS de todas las comunicaciones. Hay dos métodos **ConnectAsync** que admiten pasar un parámetro *protectionLevel*:

-   [**ConnectAsync(EndpointPair, SocketProtectionLevel)** ](/uwp/api/windows.networking.sockets.streamsocket.connectasync): inicia una operación asincrónica en un objeto [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) para establecer la conexión con un destino de red remoto especificado como un objeto [**EndpointPair**](/uwp/api/Windows.Networking.EndpointPair) y una enumeración [**SocketProtectionLevel**](/uwp/api/Windows.Networking.Sockets.SocketProtectionLevel).
-   [**ConnectAsync(HostName, String, SocketProtectionLevel)** ](/uwp/api/windows.networking.sockets.streamsocket.connectasync): inicia una operación asincrónica en un objeto [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) para establecer la conexión a un destino remoto especificado por un nombre de host remoto, un nombre de servicio remoto y una enumeración [**SocketProtectionLevel**](/uwp/api/Windows.Networking.Sockets.SocketProtectionLevel).

Si el parámetro *protectionLevel* se establece en **Windows.Networking.Sockets.SocketProtectionLevel.Ssl** cuando se llama a cualquiera de los métodos [**ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync) arriba mencionados, la clase [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) deberá establecerse para usar SSL/TLS en el cifrado. Este valor requiere cifrado y nunca permite el uso de un cifrado NULL.

La secuencia normal que puedes usar con uno de estos métodos [**ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync) es la misma.

-   Crea una clase [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket).
-   Si necesitas una opción avanzada en el socket, usa la propiedad [**StreamSocket.Control**](/uwp/api/windows.networking.sockets.streamsocket.control) para obtener la instancia [**StreamSocketControl**](/uwp/api/Windows.Networking.Sockets.StreamSocketControl) asociada a un objeto [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket). Establece una propiedad en **StreamSocketControl**.
-   Llama a uno de los métodos [**ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync) arriba mencionados, para iniciar una operación de conexión a un destino remoto y negociar inmediatamente el uso de SSL/TLS.
-   La intensidad de SSL negociada con el método [**ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync) puede determinarse al obtener la propiedad [**StreamSocketinformation.ProtectionLevel**](/uwp/api/windows.networking.sockets.streamsocketinformation.protectionlevel) después de que la operación asincrónica se haya completado correctamente.

El siguiente ejemplo crea un objeto [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) e intenta establecer una conexión al servicio de red y negociar de inmediato el uso de SSL/TLS. Si la negociación se realiza correctamente, toda comunicación de red que use el objeto **StreamSocket** entre el cliente y el servidor de red será cifrada.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
     
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "https";
    
    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 
    
    // Try to connect to contoso using HTTPS (port 443)
    try {

        // Call ConnectAsync method with SSL
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.Ssl);

        NotifyUser("Connected");
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }
        
        NotifyUser("Connect failed with error: " + exception.Message);
        // Could retry the connection, but for this simple example
        // just close the socket.
        
        clientSocket.Dispose();
        clientSocket = null; 
    }
           
    // Add code to send and receive data using the clientSocket
    // and then close the clientSocket
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>

using namespace winrt;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages.

    // Try to connect to the server using HTTPS and SSL (port 443).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }
    // Add code to send and receive data using the clientSocket,
    // then set the clientSocket to nullptr when done to close it.
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    HostName^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "https";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to the server using HTTPS and SSL (port 443)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::SSL)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
            
            clientSocket.Close();
            clientSocket = null;
        }
    });
    // Add code to send and receive data using the clientSocket
    // Then close the clientSocket when done
```

### <a name="use-upgradetosslasync"></a>Usar UpgradeToSslAsync
Si tu código usa [**UpgradeToSslAsync**](/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync), establece primero una conexión a un servicio de red sin cifrado. La aplicación puede enviar o recibir algunos datos. A continuación, actualiza la conexión para usar SSL/TLS en todas las comunicaciones.

El método [**UpgradeToSslAsync**](/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) usa dos parámetros. El parámetro *protectionLevel* indica el nivel de protección que quieres. El parámetro *validationHostName* es el nombre de host del destino de red remota que se usa para la validación, cuando se actualiza a SSL. Por lo general, el parámetro *validationHostName* es el mismo nombre de host que usó la aplicación para establecer inicialmente la conexión. Si el parámetro *protectionLevel* se establece como **Windows.System.Socket.SocketProtectionLevel.Ssl** al llamar a **UpgradeToSslAsync**, la clase [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) deberá usar el SSL/TLS para cifrar las posibles comunicaciones con el socket. Este valor requiere cifrado y nunca permite el uso de un cifrado NULL.

La secuencia normal que puedes usar con el método [**UpgradeToSslAsync**](/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) es la siguiente:

-   Crea una clase [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket).
-   Si necesitas una opción avanzada en el socket, usa la propiedad [**StreamSocket.Control**](/uwp/api/windows.networking.sockets.streamsocket.control) para obtener la instancia [**StreamSocketControl**](/uwp/api/Windows.Networking.Sockets.StreamSocketControl) asociada a un objeto [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket). Establece una propiedad en **StreamSocketControl**.
-   Si necesitas enviar o recibir algún dato sin cifrar, envíalo ahora.
-   Llama al método [**UpgradeToSslAsync**](/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) para iniciar una operación de actualización que te permita usar SSL/TLS en la conexión.
-   Puedes determinar la intensidad de SSL negociada con [**UpgradeToSslAsync**](/uwp/api/windows.networking.sockets.streamsocket.upgradetosslasync) al obtener la propiedad [**StreamSocketinformation.ProtectionLevel**](/uwp/api/windows.networking.sockets.streamsocketinformation.protectionlevel), una vez que la operación asincrónica se complete correctamente.

El siguiente ejemplo crea una clase [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) e intenta establecer una conexión al servicio de red, enviar algunos datos iniciales y, a continuación, negociar el uso de SSL/TLS. Si la negociación se realiza correctamente, toda comunicación de red que use la clase **StreamSocket** entre el cliente y el servidor de red estará cifrada.

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
 
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    try {
        // Call ConnectAsync method with a plain socket
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.PlainSocket);

        NotifyUser("Connected");

    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Connect failed with error: " + exception.Message, NotifyType.ErrorMessage);
        // Could retry the connection, but for this simple example
        // just close the socket.

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }

    // Now try to send some data
    DataWriter writer = new DataWriter(clientSocket.OutputStream);
    string hello = "Hello, World! ☺ ";
    Int32 len = (int) writer.MeasureString(hello); // Gets the UTF-8 string length.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser("Client: sending hello");

    try {
        // Call StoreAsync method to store the hello message
        await writer.StoreAsync();

        NotifyUser("Client: sent data");

        writer.DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
    }
    catch (Exception exception) {
        NotifyUser("Store failed with error: " + exception.Message);
        // Could retry the store, but for this simple example
            // just close the socket.

            clientSocket.Dispose();
            clientSocket = null; 
            return;
    }

    // Now upgrade the client to use SSL
    try {
        // Try to upgrade to SSL
        await clientSocket.UpgradeToSslAsync(SocketProtectionLevel.Ssl, serverHost);

        NotifyUser("Client: upgrade to SSL completed");
           
        // Add code to send and receive data 
        // The close clientSocket when done
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt;
using namespace Windows::Storage::Streams;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages. 

    // Try to connect to the server using HTTP (port 80).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }

    // Now, try to send some data.
    DataWriter writer{ clientSocket.OutputStream() };
    winrt::hstring hello{ L"Hello, World! ☺ " };
    uint32_t len{ writer.MeasureString(hello) }; // Gets the size of the string, in bytes.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser(L"Client: sending hello");

    try
    {
        co_await writer.StoreAsync();
        NotifyUser(L"Client: sent hello");

        writer.DetachStream(); // Detach the stream when you want to continue using it; otherwise, the DataWriter destructor closes it.
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Store failed with error: " + exception.message());
        // We could retry the store operation. But, for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }

    // Now, upgrade the client to use SSL.
    try
    {
        co_await clientSocket.UpgradeToSslAsync(Windows::Networking::Sockets::SocketProtectionLevel::Tls12, serverHost);
        NotifyUser(L"Client: upgrade to SSL completed");

        // Add code to send and receive data using the clientSocket,
        // then set the clientSocket to nullptr when done to close it.
    }
    catch (winrt::hresult_error const& exception)
    {
        // If this is an unknown status, then the error is fatal and retry will likely fail.
        Windows::Networking::Sockets::SocketErrorStatus socketErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(exception.to_abi()) };
        if (socketErrorStatus == Windows::Networking::Sockets::SocketErrorStatus::Unknown)
        {
            throw;
        }

        NotifyUser(L"Upgrade to SSL failed with error: " + exception.message());
        // We could retry the store operation. But for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;
using Windows::Storage::Streams;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    Hostname^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
 
            clientSocket->Close();
            clientSocket = null;
        }
    });
       
    // Now try to send some data
    DataWriter^ writer = new ref DataWriter(clientSocket.OutputStream);
    String hello = "Hello, World! ☺ ";
    Int32 len = (int) writer->MeasureString(hello); // Gets the UTF-8 string length.
    writer->writeInt32(len);
    writer->writeString(hello);
    NotifyUser("Client: sending hello");

    task<void>(writer->StoreAsync()).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

            NotifyUser("Client: sent hello");

            writer->DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
       }
       catch (Exception^ exception) {
               NotifyUser("Store failed with error: " + exception->Message);
               // Could retry the store, but for this simple example
               // just close the socket.
 
               clientSocket->Close();
               clientSocket = null;
               return
       }
    });

    // Now upgrade the client to use SSL
    task<void>(clientSocket->UpgradeToSslAsync(clientSocket.SocketProtectionLevel.Ssl, serverHost)).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

           NotifyUser("Client: upgrade to SSL completed");
           
           // Add code to send and receive data 
           // Then close clientSocket when done
        }
        catch (Exception^ exception) {
            // If this is an unknown status it means that the error is fatal and retry will likely fail.
            if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
                throw;
            }

            NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

            clientSocket->Close();
            clientSocket = null; 
            return;
        }
    });
```

### <a name="creating-secure-websocket-connections"></a>Crear conexiones WebSocket seguras
Al igual que las conexiones de sockets tradicionales, las conexiones WebSocket también pueden cifrarse mediante el cifrado Seguridad de la capa de transporte (TLS) o Capa de sockets seguros (SSL), cuando se usan las características [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) y [**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket) para una aplicación para UWP. En la mayoría de los casos, querrás usar una conexión WebSocket segura. Esto aumentará las posibilidades de que tu conexión se realice correctamente, ya que muchos servidores proxy rechazarán las conexiones WebSocket no cifradas.

Para obtener ejemplos sobre cómo crear o actualizar una conexión de sockets segura a un servicio de red, consulta [Cómo proteger conexiones WebSocket con TLS/SSL](/previous-versions/windows/apps/hh994399(v=win.10)).

Además del cifrado con TLS/SSL, es posible que un servidor necesite un valor de encabezado **Sec-WebSocket-Protocol** para completar el protocolo de enlace inicial. Este valor, representado por las propiedades [**StreamWebSocketInformation.Protocol**](/uwp/api/windows.networking.sockets.streamwebsocketinformation.protocol) y [**MessageWebSocketInformation.Protocol**](/uwp/api/windows.networking.sockets.messagewebsocketinformation.protocol), indica la versión del protocolo de la conexión y permite que el servidor interprete correctamente el protocolo de enlace de apertura y los datos que se intercambian después. Mediante esta información del protocolo, si en algún momento el servidor no puede interpretar los datos entrantes de manera segura, se puede cerrar la conexión.

Si la solicitud inicial del cliente no contiene este valor o proporciona un valor que no coincide con el que espera el servidor, el valor esperado se envía desde el servidor al cliente a modo de error de protocolo de enlace de WebSocket.

## <a name="authentication"></a>Autenticación
Cómo proporcionar credenciales de autenticación al conectarse a través de la red.

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>Aprovisionamiento de un certificado de cliente con la clase StreamSocket
La clase [**Windows.Networking.Sockets.StreamSocket**](/uwp/api/windows.networking.sockets.streamsocket) admite el uso de SSL/TLS para autenticar el servidor con el que se comunica la aplicación. En algunos casos, la aplicación también debe autenticarse en el servidor mediante un certificado de cliente TLS. En Windows 10, puedes proporcionar un certificado de cliente en el objeto [**StreamSocket.Control**](/uwp/api/Windows.Networking.Sockets.StreamSocketControl) (se debe establecer antes de iniciar el protocolo de enlace TLS). Si el servidor solicita el certificado de cliente, Windows responderá con el certificado proporcionado.

Este es un fragmento de código que muestra cómo puedes implementar esto:

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>Proporcionar credenciales de autenticación a un servicio web
Las API para redes que permiten que las aplicaciones interactúen con servicios web seguros, proporcionan sus propios métodos para inicializar un cliente o establecer un encabezado de solicitud con credenciales de autenticación de proxy y servidor. Cada método se establece con un objeto [**PasswordCredential**](/uwp/api/Windows.Security.Credentials.PasswordCredential) que indica un nombre de usuario, una contraseña y el recurso para el cual se usan estas credenciales. En la siguiente tabla, se proporciona una relación de estas API:

| **WebSockets** | [**MessageWebSocketControl.ServerCredential**](/uwp/api/windows.networking.sockets.messagewebsocketcontrol.servercredential) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](/uwp/api/windows.networking.sockets.messagewebsocketcontrol.proxycredential) |
|  | [**StreamWebSocketControl.ServerCredential**](/uwp/api/windows.networking.sockets.streamwebsocketcontrol.servercredential) |
|  | [**StreamWebSocketControl.ProxyCredential**](/uwp/api/windows.networking.sockets.streamwebsocketcontrol.proxycredential) |
| **Background Transfer** | [**BackgroundDownloader.ServerCredential**](/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.servercredential) |
|  | [**BackgroundDownloader.ProxyCredential**](/uwp/api/windows.networking.backgroundtransfer.backgrounddownloader.proxycredential) |
|  | [**BackgroundUploader.ServerCredential**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.servercredential) |
|  | [**BackgroundUploader.ProxyCredential**](/uwp/api/windows.networking.backgroundtransfer.backgrounduploader.proxycredential) |
| **Syndication** | [**SyndicationClient(PasswordCredential)** ](/uwp/api/windows.web.syndication.syndicationclient.-ctor#Windows_Web_Syndication_SyndicationClient__ctor_Windows_Security_Credentials_PasswordCredential_) |
|  | [**SyndicationClient.ServerCredential**](/uwp/api/windows.web.syndication.syndicationclient.servercredential) |
|  | [**SyndicationClient.ProxyCredential**](/uwp/api/windows.web.syndication.syndicationclient.proxycredential) |
| **AtomPub** | [**AtomPubClient(PasswordCredential)** ](/uwp/api/windows.web.atompub.atompubclient.-ctor#Windows_Web_AtomPub_AtomPubClient__ctor_Windows_Security_Credentials_PasswordCredential_) |
|  | [**AtomPubClient.ServerCredential**](/uwp/api/windows.web.atompub.atompubclient.servercredential) |
|  | [**AtomPubClient.ProxyCredential**](/uwp/api/windows.web.atompub.atompubclient.proxycredential) |

## <a name="handling-network-exceptions"></a>Controlar excepciones de red
En la mayoría de las áreas de programación, una excepción indica un error o problema significativo, causado por algún error en el programa. En la programación de red, existe un origen adicional para excepciones: la propia red y la naturaleza de las comunicaciones de red. Las comunicaciones de red son inherentemente no confiables y están propensas a errores inesperados. Para cada una de las formas en que la aplicación usa la conexión de red, debes mantener alguna información de estado; y el código de la aplicación debe controlar las excepciones de red actualizando esa información de estado e iniciando la lógica adecuada para tu aplicación para restablecer o reintentar en caso de errores de comunicación.

Cuando las aplicaciones universales de Windows arrojan una excepción, el controlador de excepciones puede recuperar información más detallada sobre la causa de la excepción para que así puedas comprender mejor el error y tomar las decisiones adecuadas.

Cada proyección de lenguaje admite un método para obtener acceso a esta información detallada. Una excepción se proyecta como un valor **HRESULT** en las aplicaciones universales de Windows. El archivo de inclusión *Winerror.h* contiene una lista muy extensa de posibles valores **HRESULT**, que incluye errores de red.

Las API para redes admiten distintos métodos para recuperar la información que detalla la causa de una excepción.

-   Algunas API proporcionan un método auxiliar que convierte el valor **HRESULT** de la excepción en un valor de enumeración.
-   Otras API proporcionan un método para recuperar el valor **HRESULT** real.

## <a name="related-topics"></a>Temas relacionados
* [Networking API Improvements in Windows 10](https://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/) (Mejoras de la API para redes en Windows 10)