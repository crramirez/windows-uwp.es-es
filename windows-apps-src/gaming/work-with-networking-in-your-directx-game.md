---
title: Conexión en red de juegos
description: Aprende a desarrollar e incorporar características de red en un juego DirectX.
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, redes, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 005dc30bc71d6d9087a3cc15880ffa3936d0a7f7
ms.sourcegitcommit: 22ed0d4edad5e6bab352e641cf86cf455cf83825
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133968"
---
# <a name="networking-for-games"></a>Conexión en red de juegos



Aprende a desarrollar e incorporar características de red en un juego DirectX.

## <a name="concepts-at-a-glance"></a>Conceptos de un vistazo


En tu juego DirectX puedes usar una variedad de características de red, ya sea un juego independiente sencillo o juegos masivos de varios jugadores. El uso más simple de las funciones de red sería almacenar nombres de usuario y puntuaciones del juego en un servidor de red central.

Se necesitan API para redes en los juegos de varios jugadores que usan el modelo de infraestructura (servidor de cliente o punto a punto de Internet) y también para los juegos ad hoc (punto a punto local). Para juegos de varios jugadores basados en servidor, un servidor de juegos central generalmente controla la mayoría de las operaciones de juego y la aplicación de juego cliente se usa para entrada, visualización de gráficos, reproducción de audio y demás características. La velocidad y latencia de las transferencias de red es un factor importante para una experiencia de juego satisfactoria.

Para los juegos de punto a punto, la aplicación de cada jugador controla la entrada y los gráficos. En la mayoría de los casos, los jugadores del juego se encuentran cerca para que la latencia de red sea menor pero siga siendo un factor importante. Cómo descubrir pares y establecer una conexión es un punto importante a tener en cuenta.

Para juegos de un solo jugador, generalmente se usa un servicio o servidor web central para almacenar nombres de usuario, puntuaciones de juego y demás datos varios. En estos juegos, la velocidad y la latencia de las transferencias de red no son tan importantes dado que no afectan directamente la operación de juego.

Las condiciones de la red pueden cambiar en cualquier momento. Por ello, los juegos que usan API de red deben controlar las excepciones de red que puedan producirse. Para obtener más información sobre el control de las excepciones de red, consulta [Conceptos básicos de redes](https://docs.microsoft.com/windows/uwp/networking/networking-basics).

Los firewall y proxy web son comunes y pueden afectar la capacidad de usar las características de red. Un juego que usa la red debe estar preparado para controlar correctamente los firewall y proxy.

Para dispositivos móviles, es importante supervisar los recursos de red disponibles y actuar en consecuencia cuando esté en redes medidas donde los costes de los datos y el roaming pueden ser significativos.

El aislamiento de red forma parte del modelo de seguridad de la aplicación usado por Windows. Windows detecta activamente los límites de la red y aplica las restricciones de acceso a la red para el aislamiento. Las aplicaciones deben declarar funcionalidades de aislamiento de red para definir el ámbito de acceso de red. Sin declarar estas funcionalidades, tu aplicación no tendrá acceso a los recursos de red. Consulta [Cómo configurar las funcionalidades de aislamiento de red](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10)) para obtener más información acerca de cómo Windows aplica el aislamiento de red para las aplicaciones.

## <a name="design-considerations"></a>Consideraciones de diseño


En los juegos DirectX se puede usar una variedad de API de red. Por ello, es importante elegir la API correcta. Windows admite una variedad de API de red que tu aplicación puede usar para comunicarse con otros equipos y dispositivos a través de Internet o de redes privadas. Tu primer paso es determinar qué características de red necesita tu aplicación.

Las API de red más populares para los juegos son:

-   TCP y sockets: proporciona una conexión confiable. Usa TCP para las operaciones de juego que no necesitan seguridad. TCP permite que el servidor escale con facilidad. Por ello, se usa comúnmente en juegos que usan el modelo de infraestructura (servidor cliente o punto a punto de Internet). TCP puede ser usado por juegos ad hoc (punto a punto local) a través de Wi-Fi Direct y Bluetooth. TCP generalmente se usa para movimiento de objetos en juegos, interacción de caracteres, conversaciones de texto y demás operaciones. La clase [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) proporciona un socket TCP que se puede usar en los juegos de Microsoft Store. La clase **StreamSocket** se usa con clases relacionadas en el espacio de nombres [**Windows::Networking::Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets).
-   TCP y sockets que usan SSL: proporciona una conexión confiable que evita interceptaciones. Usa conexiones TCP con SSL para operaciones de juego que requieren de seguridad. El cifrado y la sobrecarga de SSL agrega un coste de latencia y rendimiento. Por ello es el único que se usa cuando se necesita seguridad. TCP con SSL generalmente se usa para iniciar sesión, comprar y vender activos, y crear y administrar personajes. La clase [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) proporciona un socket TCP que admite SSL.
-   UDP y sockets: proporciona transferencias de red no confiables con poca sobrecarga. UDP se usa para operaciones de juego que requieren de poca latencia y pueden tolerar cierto grado de pérdida de paquetes. Generalmente se usa para juegos de lucha, disparos y rastreo, audio de red y conversaciones de voz. La clase [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) proporciona un socket UDP que se puede usar en los juegos de Microsoft Store. La clase **DatagramSocket** se usa con clases relacionadas en el espacio de nombres [**Windows::Networking::Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets).
-   Cliente HTTP: proporciona una conexión confiable a servidores HTTP. El escenario de redes más común es obtener acceso a un sitio web para recuperar o almacenar datos. Un ejemplo simple sería un juego que usa un sitio web para almacenar información del usuario y puntuación de juegos. Cuando se usa con SSL para seguridad, puede usarse un cliente HTTP para iniciar sesión, comercializar activos, administrar y crear caracteres de juego. La clase [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) proporciona una API de cliente http moderna para su uso en Microsoft Store Games. La clase **HttpClient** se usa con clases relacionadas en el espacio de nombres [**Windows::Web::Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http).

## <a name="handling-network-exceptions-in-your-directx-game"></a>Administrar las excepciones de red en tu juego de DirectX


Cuando se produce una excepción de red en tu juego DirectX, significa que se produjo un error o problema importante. Las excepciones pueden producirse por muchas razones cuando se usan las API de red. A menudo, la excepción puede producirse por cambios en la conectividad de red u otros problemas de red con el servidor o host remoto.

Algunos de los motivos de las excepciones al usar API de red pueden ser los siguientes:

-   La entrada del usuario para un nombre de host o un URI contiene errores y no es válida.
-   Errores de resoluciones de nombre al buscar un URI o nombre de host.
-   Pérdida o cambio de la conectividad de red.
-   Errores en la conexión de red al usar sockets o las API de cliente HTTP.
-   Errores del extremo remoto o del servidor de red.
-   Errores de red diversos.

Las excepciones a causa de errores en la red (por ejemplo, por pérdida o cambios de la conectividad, errores de conexión y errores del servidor) pueden producirse en cualquier momento. Estos errores hacen que se arrojen excepciones. Si tu aplicación no controla las excepciones, estas pueden ocasionar que el tiempo de ejecución finalice la aplicación.

Debes escribir código para controlar las excepciones cuando llamas a la mayoría de los métodos de red asincrónicos. Algunas veces, cuando se produce una excepción, se puede reintentar un método de red para intentar solucionar el problema. Otras veces, la aplicación podría necesitar un plan para continuar sin conectividad de red usando los datos almacenados anteriormente en caché.

Las aplicaciones de la Plataforma universal de Windows (UWP) por lo general arrojan una sola excepción. Tu controlador de excepciones puede recuperar información más detallada sobre la causa de la excepción para comprender mejor el error y tomar las decisiones adecuadas.

Cuando se produce una excepción en un juego DirectX que es una aplicación para UWP, se puede recuperar el valor **HRESULT** de la causa del error. El archivo de inclusión *Winerror.h* contiene una lista extensa de posibles valores **HRESULT**, que incluye errores de red.

Las API de red admiten distintos métodos para recuperar esta información detallada sobre la causa de una excepción.

-   Un método para recuperar el valor **HRESULT** del error que provocó la excepción. La lista de posibles valores de **HRESULT** es grande y no especificada. El valor **HRESULT** puede recuperarse al usar cualquiera de las API para redes.
-   Un método auxiliar que convierte el valor **HRESULT** en un valor de enumeración. La lista de posibles valores de enumeración es especificada y relativamente pequeña. Hay un método auxiliar disponible para las clases de socket en [**Windows::Networking::Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets).

### <a name="exceptions-in-windowsnetworkingsockets"></a>Excepciones en Windows.Networking.Sockets

El constructor de la clase [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName) que se usa con los sockets puede generar una excepción si la cadena pasada no es un nombre de host válido (contiene caracteres que no se pueden usar en un nombre de host). Si una aplicación obtiene la intervención del usuario para el **HostName** de una conexión del mismo nivel para juegos, el constructor debe estar en un bloque try/catch. Si se genera una excepción, la aplicación podrá notificar al usuario y solicitar otro nombre de host.

Agregar código para validar una cadena para un nombre de host del usuario

```cpp

    // Define some variables at the class level.
    Windows::Networking::HostName^ remoteHost;

    bool isHostnameFromUser = false;
    bool isHostnameValid = false;

    ///...

    // If the value of 'remoteHostname' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^hostString = remoteHostname;

    try 
    {
        remoteHost = ref new Windows::Networking:Host(hostString);
        isHostnameValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad hostname, please re-enter a valid hostname.";
        return;
    }

    isHostnameFromUser = true;


    // ... Continue with code to execute with a valid hostname.
```

El espacio de nombres [**Windows.Networking.Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets) tiene enumeraciones y métodos auxiliares convenientes para controlar errores al usar sockets. Esto puede ser útil para controlar de un modo diferente excepciones de red específicas en la aplicación.

Encontrar un error en [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket), [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) o una operación [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) hace que se arroje una excepción. La causa de la excepción es un valor de error representado como un valor **HRESULT**. El método [**SocketError.GetStatus**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.socketerror.getstatus) se usa para convertir un error de red de una operación de socket en un valor de enumeración [**SocketErrorStatus**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.SocketErrorStatus). La mayoría de los valores de enumeración **SocketErrorStatus** corresponden a un error devuelto por la operación de Windows Sockets nativa. Una aplicación puede filtrar según valores **SocketErrorStatus** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para los errores de validación de parámetros, una aplicación también puede usar el **HRESULT** de la excepción para obtener información más detallada del error que causó la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Para la mayoría de los errores de validación de parámetros, el **HRESULT** devuelto es **E \_ INVALIDARG**.

Agregar código para controlar excepciones cuando se intenta hacer una conexión de socket de secuencias

```cpp
using namespace Windows::Networking;
using namespace Windows::Networking::Sockets;

    
    // Define some more variables at the class level.

    bool isSocketConnected = false
    bool retrySocketConnect = false;

    // The number of times we have tried to connect the socket.
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid remoteHost and serviceName parameter.
    // The hostname can contain a name or an IP address.
    // The servicename can contain a string or a TCP port number.

    StreamSocket ^ socket = ref new StreamSocket();
    SocketErrorStatus errorStatus; 
    HResult hr;

    // Save the socket, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("clientSocket", socket);

    // Connect to the remote server. 
    create_task(socket->ConnectAsync(
            remoteHost,
            serviceName,
            SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isSocketConnected = true;
            // Mark the socket as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
            errorStatus = SocketStatus::GetStatus(hr); 
            if (errorStatus != Unknown)
            {
                                                                switch (errorStatus) 
                   {
                    case HostNotFound:
                        // If the hostname is from the user, this may indicate a bad input.
                        // Set a flag to ask the user to re-enter the hostname.
                        isHostnameValid = false;
                        return;
                        break;
                    case ConnectionRefused:
                        // The server might be temporarily busy.
                        retrySocketConnect = true;
                        return;
                        break; 
                    case NetworkIsUnreachable: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case UnreachableHost: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case NetworkIsDown: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    // Handle other errors. 
                    default: 
                        // The connection failed and no options are available.
                        // Try to use cached data if it is available. 
                        // You may want to tell the user that the connect failed.
                        break;
                }
                }
                else 
                {
                    // Received an Hresult that is not mapped to an enum.
                    // This could be a connectivity issue.
                    retrySocketConnect = true;
                }
            }
        });
    }

```

### <a name="exceptions-in-windowswebhttp"></a>Excepciones en Windows.Web.Http

El constructor de la clase [**Windows::Foundation::Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) usado con [**Windows::Web::Http::HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) puede arrojar una excepción si se pasa una cadena con un URI no válido (contiene caracteres no permitidos en un URI). En C++, no hay ningún método para probar y analizar una cadena en un URI. Si una aplicación obtiene la entrada del usuario para **Windows:: Foundation:: URI**, el constructor debe estar en un bloque try/catch. Si se arroja una excepción, la aplicación puede notificar al usuario y solicitar un nuevo URI.

La aplicación también debe comprobar que el esquema en el URI sea HTTP o HTTPS, dado que estos son los únicos esquemas admitidos por [**Windows::Web::Http::HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient).

Agregar código para validar una cadena para un URI del usuario

```cpp

    // Define some variables at the class level.
    Windows::Foundation::Uri^ resourceUri;

    bool isUriFromUser = false;
    bool isUriValid = false;

    ///...

    // If the value of 'inputUri' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^uriString = inputUri;

    try 
    {
        isUriValid = false;
        resourceUri = ref new Windows::Foundation:Uri(uriString);

        if (resourceUri->SchemeName != "http" && resourceUri->SchemeName != "https")
        {
            statusText->Text = "Only 'http' and 'https' schemes supported. Please re-enter URI";
            return;
        }
        isUriValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad URI, please re-enter Uri to continue.";
        return;
    }

    isUriFromUser = true;


    // ... Continue with code to execute with a valid URI.
```

El espacio de nombres [**Windows::Web::Http**](https://docs.microsoft.com/uwp/api/windows.web.http) no tiene una función de conveniencia. Por lo tanto, una aplicación que usa [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) y otras clases de este espacio de nombres necesita usar el valor **HRESULT** .

En las aplicaciones con C++, [**Platform::Exception**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) representa un error durante la ejecución de la aplicación cuando se produce una excepción. La propiedad [**Platform::Exception::HResult**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) devuelve el valor **HRESULT** asignado a la excepción concreta. La propiedad [**Platform::Exception::Message**](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) devuelve la cadena proporcionada por el sistema asociada con el valor **HRESULT**. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Una aplicación puede filtrar según valores **HRESULT** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para la mayoría de los errores de validación de parámetros, el **HRESULT** devuelto es **E \_ INVALIDARG**. Para algunas llamadas a métodos no válidas, el valor de **HRESULT** devuelto es **E\_ILLEGAL\_METHOD\_CALL**.

Agregar código para controlar excepciones cuando se intenta usar [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) para conectarse a un servidor HTTP

```cpp
using namespace Windows::Foundation;
using namespace Windows::Web::Http;
    
    // Define some more variables at the class level.

    bool isHttpClientConnected = false
    bool retryHttpClient = false;

    // The number of times we have tried to connect the socket
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid resourceUri parameter.
    // The URI must contain a scheme and a name or an IP address.

    HttpClient ^ httpClient = ref new HttpClient();
    HResult hr;

    // Save the httpClient, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("httpClient", httpClient);

    // Send a GET request to the HTTP server. 
    create_task(httpClient->GetAsync(resourceUri)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isHttpClientConnected = true;
            // Mark the HttClient as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
                                                switch (errorStatus) 
               {
                case WININET_E_NAME_NOT_RESOLVED:
                    // If the Uri is from the user, this may indicate a bad input.
                    // Set a flag to ask user to re-enter the Uri.
                    isUriValid = false;
                    return;
                    break;
                case WININET_E_CANNOT_CONNECT:
                    // The server might be temporarily busy.
                    retryHttpClientConnect = true;
                    return;
                    break; 
                case WININET_E_CONNECTION_ABORTED: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case WININET_E_CONNECTION_RESET: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case INET_E_RESOURCE_NOT_FOUND: 
                    // The server cannot locate the resource specified in the uri.
                    // If the Uri is from user, this may indicate a bad input.
                    // Set a flag to ask the user to re-enter the Uri
                    isUriValid = false;
                    return;
                    break;
                // Handle other errors. 
                default: 
                    // The connection failed and no options are available.
                    // Try to use cached data if it is available. 
                    // You may want to tell the user that the connect failed.
                    break;
            }
            else 
            {
                // Received an Hresult that is not mapped to an enum.
                // This could be a connectivity issue.
                retrySocketConnect = true;
            }
        }
    });
    

```

## <a name="related-topics"></a>Temas relacionados


**Otros recursos**

* [Conectar con un socket de datagramas](https://docs.microsoft.com/previous-versions/windows/apps/jj635238(v=win.10))
* [Conectar a un recurso de red con un socket de secuencias](https://docs.microsoft.com/previous-versions/windows/apps/jj150599(v=win.10))
* [Conexión a servicios de red](https://docs.microsoft.com/previous-versions/windows/apps/hh452976(v=win.10))
* [Conexión a servicios web](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10))
* [Conceptos básicos de redes](https://docs.microsoft.com/windows/uwp/networking/networking-basics)
* [Procedimiento para configurar las funcionalidades de aislamiento de red](https://docs.microsoft.com/previous-versions/windows/apps/hh770532(v=win.10))
* [Cómo habilitar el aislamiento de red de bucle invertido y de depuración](https://docs.microsoft.com/previous-versions/windows/apps/hh780593(v=win.10))

**Referencia**

* [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)
* [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
* [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket)
* [**Windows:: Web:: http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
* [**Windows:: Networking:: Sockets**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets)

**Ejemplos**

* [Muestra de DatagramSocket](https://github.com/microsoft/VCSamples/tree/master/VC2012Samples/Windows%208%20samples/C%2B%2B/Windows%208%20app%20samples/ControlChannelTrigger%20StreamSocket%20sample%20(Windows%208))
* [Ejemplo de HttpClient](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/HttpClient%20sample)
* [Muestra de proximidad](https://github.com/microsoft/VCSamples/tree/master/VC2012Samples/Windows%208%20samples/C%2B%2B/Windows%208%20app%20samples/Proximity%20sample%20(Windows%208))
* [Muestra StreamSocket](https://code.msdn.microsoft.com/windowsapps/StreamSocket-Sample-8c573931)
