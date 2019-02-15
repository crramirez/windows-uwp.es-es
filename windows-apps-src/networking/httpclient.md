---
description: Usa HttpClient y el resto de la API del espacio de nombres Windows.Web.Http para enviar y recibir información mediante los protocolos HTTP 2.0 y HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dd4b8c137d65339701b40027bb3230162e2c2456
ms.sourcegitcommit: fde2d41ef4b5658785723359a8c4b856beae8f95
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "9079213"
---
# <a name="httpclient"></a>HttpClient

**API importantes**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

Usa [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) y el resto de la API del espacio de nombres [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) para enviar y recibir información mediante los protocolos HTTP 2.0 y HTTP 1.1.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Información general de HttpClient y el espacio de nombres Windows.Web.Http

Las clases del espacio de nombres [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) y los espacios de nombres [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) y [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623) relacionados proporcionan una interfaz de programación para aplicaciones para la Plataforma universal de Windows (UWP) que actúan como un cliente HTTP para realizar solicitudes GET básicas o implementar funcionalidades más avanzadas de HTTP que se indican a continuación.

-   Métodos para verbos comunes (**DELETE**, **GET**, **PUT** y **POST**). Cada una de estas solicitudes se envía como una operación asincrónica.

-   Compatibilidad con diseños y configuraciones de autenticación comunes.

-   Acceso a detalles de la Capa de sockets seguros (SSL) en el transporte.

-   Capacidad de incluir filtros personalizados en aplicaciones avanzadas.

-   Capacidad de obtener, establecer y eliminar cookies.

-   Información de progreso de solicitudes HTTP disponible en métodos asincrónicos.

La clase [**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) representa un mensaje de solicitud HTTP enviado por [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639). La clase [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) representa un mensaje de respuesta HTTP recibido de una solicitud HTTP. IETF define los mensajes HTTP en la especificación [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642).

El espacio de nombres [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) representa el contenido HTTP como el cuerpo de entidad HTTP y los encabezados con cookies. El contenido HTTP puede asociarse a una solicitud HTTP o a una respuesta HTTP. El espacio de nombres **Windows.Web.Http** proporciona distintas clases para representar el contenido HTTP.

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625). Contenido como un búfer
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685). Contenido como tuplas de nombre y valor codificadas con el tipo MIME **application/x-www-form-urlencoded**.
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708). Contenido con el formato del tipo MIME **multipart/\***.
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596). Contenido que se codifica como el tipo MIME **multipart/form-data**.
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649). Contenido como un flujo (el tipo interno lo usan el método HTTP GET para recibir datos y el método HTTP POST para cargar datos).
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661). Contenido como una cadena.
-   [**IHttpContent**](https://msdn.microsoft.com/library/windows/apps/dn279684): una interfaz base para que los desarrolladores creen sus propios objetos de contenido.

El fragmento de código de la sección "Enviar una solicitud GET sencilla a través de HTTP" usa la clase [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) para representar la respuesta HTTP de una solicitud HTTP GET como una cadena.

El espacio de nombres [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) admite la creación de cookies y encabezados HTTP, que se asocian como propiedades a objetos [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) y [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631).

## <a name="send-a-simple-get-request-over-http"></a>Enviar una solicitud GET sencilla a través de HTTP

Como se mencionó anteriormente en este artículo, el espacio de nombres [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) permite que las aplicaciones para UWP envíen solicitudes GET. El siguiente fragmento de código muestra cómo enviar una solicitud GET para http://www.contoso.com mediante la clase [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) y la clase de [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) para leer la respuesta de la solicitud GET.

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>Datos binarios POST a través de HTTP

El [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis) siguiente ejemplo de código ilustra el uso de datos de un formulario y una solicitud POST para enviar una pequeña cantidad de datos binarios como una carga de archivos a un servidor web. El código usa la clase [**HttpBufferContent**](/uwp/api/windows.web.http.httpbuffercontent) para representar los datos binarios y la clase [**HttpMultipartFormDataContent**](/uwp/api/windows.web.http.httpmultipartformdatacontent) para representar los datos de varias partes del formulario.

> [!NOTE]
> Una llamada a **obtener** (tal como se muestra en el siguiente ejemplo de código) no es apropiado para un subproceso de interfaz de usuario. Para que la correcta de la técnica usar en ese caso, consulta [operaciones simultáneas y asincrónicas con C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    Windows::Web::Http::HttpClient httpClient;

    Uri requestUri{ L"https://www.contoso.com/post" };

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    postContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

Para registrar el contenido de un archivo binario real (en lugar de los datos binarios explícitos utilizados anteriormente), le resultará más fácil de usar un objeto [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent) . Construye uno y, como el argumento de su constructor, pasa el valor devuelto desde una llamada a [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync). Este método devuelve una secuencia de los datos en el archivo binario.

Además, si estás cargando un archivo grande (más de aproximadamente 10MB), a continuación, te recomendamos que puedes usar el tiempo de ejecución de Windows [Transferencia en segundo plano](/uwp/api/windows.networking.backgroundtransfer) API.

## <a name="exceptions-in-windowswebhttp"></a>Excepciones en Windows.Web.Http

Se inicia una excepción cuando se pasa una cadena no válida del identificador uniforme de recursos (URI) al constructor del objeto [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**. NET:** el tipo de [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) aparece como [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en C# y VB.

En C# y Visual Basic, este error puede evitarse si se usa la clase [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en .NET 4.5 y uno de los métodos [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) para probar la cadena recibida del usuario antes de que se cree el URI.

En C++, no hay ningún método para probar y analizar una cadena en un URI. Si una aplicación recibe información del usuario relativa a [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), el constructor debe estar en un bloque Try/Catch. Si se genera una excepción, la aplicación podrá notificar al usuario y solicitar otro nombre de host.

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) no tiene una función conveniente. Por este motivo, una aplicación que use [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) y otras clases de este espacio de nombres debe usar el valor **HRESULT**.

En las aplicaciones usando el Framework4.5 de .NET en C#, VB.NET, la [System.Exception](https://msdn.microsoft.com/library/system.exception.aspx) representa un error durante la ejecución de la aplicación cuando se produce una excepción. La propiedad [System.Exception.HResult](https://msdn.microsoft.com/library/system.exception.hresult.aspx) devuelve el valor **HRESULT** asignado a la excepción específica. La propiedad [System.Exception.Message](https://msdn.microsoft.com/library/system.exception.message.aspx) devuelve el mensaje que describe la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Una aplicación puede filtrar según valores **HRESULT** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

En las aplicaciones que usan C++ administrado, [Platform::Exception](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx) representa un error durante la ejecución de la aplicación cuando se produce una excepción. La propiedad [Platform::Exception::HResult](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) devuelve el valor **HRESULT** asignado a la excepción concreta. La propiedad [Platform::Exception::Message](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) devuelve la cadena proporcionada por el sistema asociada con el valor **HRESULT**. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Una aplicación puede filtrar según valores **HRESULT** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para la mayoría de los errores de validación de parámetros, el valor de **HRESULT** devuelto es **E\_INVALIDARG**. Para algunas llamadas a métodos no válidas, el valor de **HRESULT** devuelto es **E\_ILLEGAL\_METHOD\_CALL**.

