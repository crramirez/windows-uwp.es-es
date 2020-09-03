---
description: Usa HttpClient y el resto de la API del espacio de nombres Windows.Web.Http para enviar y recibir información mediante los protocolos HTTP 2.0 y HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 06/05/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb97e58e914da1982066d9cf150f5c8b18ef884a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155449"
---
# <a name="httpclient"></a>HttpClient

**API importantes**

-   [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage)

Usa [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) y el resto de la API del espacio de nombres [**Windows.Web.Http**](/uwp/api/Windows.Web.Http) para enviar y recibir información mediante los protocolos HTTP 2.0 y HTTP 1.1.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Información general de HttpClient y el espacio de nombres Windows.Web.Http

Las clases del espacio de nombres [**Windows.Web.Http**](/uwp/api/Windows.Web.Http) y los espacios de nombres [**Windows.Web.Http.Headers**](/uwp/api/Windows.Web.Http.Headers) y [**Windows.Web.Http.Filters**](/uwp/api/Windows.Web.Http.Filters) relacionados proporcionan una interfaz de programación para aplicaciones para la Plataforma universal de Windows (UWP) que actúan como un cliente HTTP para realizar solicitudes GET básicas o implementar funcionalidades más avanzadas de HTTP que se indican a continuación.

-   Métodos para verbos comunes (**DELETE**, **GET**, **PUT** y **POST**). Cada una de estas solicitudes se envía como una operación asincrónica.

-   Compatibilidad con diseños y configuraciones de autenticación comunes.

-   Acceso a detalles de la Capa de sockets seguros (SSL) en el transporte.

-   Capacidad de incluir filtros personalizados en aplicaciones avanzadas.

-   Capacidad de obtener, establecer y eliminar cookies.

-   Información de progreso de solicitudes HTTP disponible en métodos asincrónicos.

La clase [**Windows.Web.Http.HttpRequestMessage**](/uwp/api/Windows.Web.Http.HttpRequestMessage) representa un mensaje de solicitud HTTP enviado por [**Windows.Web.Http.HttpClient**](/uwp/api/Windows.Web.Http.HttpClient). La clase [**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage) representa un mensaje de respuesta HTTP recibido de una solicitud HTTP. IETF define los mensajes HTTP en la especificación [RFC 2616](https://tools.ietf.org/html/rfc2616).

El espacio de nombres [**Windows.Web.Http**](/uwp/api/Windows.Web.Http) representa el contenido HTTP como el cuerpo de entidad HTTP y los encabezados con cookies. El contenido HTTP puede asociarse a una solicitud HTTP o a una respuesta HTTP. El espacio de nombres **Windows.Web.Http** proporciona distintas clases para representar el contenido HTTP.

-   [**HttpBufferContent**](/uwp/api/Windows.Web.Http.HttpBufferContent). Contenido como un búfer
-   [**HttpFormUrlEncodedContent**](/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent). Contenido como tuplas de nombre y valor codificadas con el tipo MIME **application/x-www-form-urlencoded**.
-   [**HttpMultipartContent**](/uwp/api/Windows.Web.Http.HttpMultipartContent). Contenido con el formato del tipo MIME **multipart/\*** .
-   [**HttpMultipartFormDataContent**](/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent). Contenido que se codifica como el tipo MIME **multipart/form-data**.
-   [**HttpStreamContent**](/uwp/api/Windows.Web.Http.HttpStreamContent). Contenido como un flujo (el tipo interno lo usan el método HTTP GET para recibir datos y el método HTTP POST para cargar datos).
-   [**HttpStringContent**](/uwp/api/Windows.Web.Http.HttpStringContent). Contenido como una cadena.
-   [**IHttpContent**](/uwp/api/Windows.Web.Http.IHttpContent): una interfaz base para que los desarrolladores creen sus propios objetos de contenido

El fragmento de código de la sección "Enviar una solicitud GET sencilla a través de HTTP" usa la clase [**HttpStringContent**](/uwp/api/Windows.Web.Http.HttpStringContent) para representar la respuesta HTTP de una solicitud HTTP GET como una cadena.

El espacio de nombres [**Windows.Web.Http.Headers**](/uwp/api/Windows.Web.Http.Headers) admite la creación de cookies y encabezados HTTP, que se asocian como propiedades a objetos [**HttpRequestMessage**](/uwp/api/Windows.Web.Http.HttpRequestMessage) y [**HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage).

## <a name="send-a-simple-get-request-over-http"></a>Enviar una solicitud GET sencilla a través de HTTP

Como se mencionó anteriormente en este artículo, el espacio de nombres [**Windows.Web.Http**](/uwp/api/Windows.Web.Http) permite que las aplicaciones para UWP envíen solicitudes GET. En el siguiente fragmento de código se muestra cómo enviar una solicitud GET a http:\//www.contoso.com mediante la clase [**Windows.Web.Http.Http.Client**](/uwp/api/Windows.Web.Http.HttpClient) y la clase [**Windows.Web.Http.HttpResponseMessage**](/uwp/api/Windows.Web.Http.HttpResponseMessage) para leer la respuesta de la solicitud GET.

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

## <a name="post-binary-data-over-http"></a>Datos POST binarios a través de HTTP

El ejemplo de código [ C++/WinRT](../cpp-and-winrt-apis/index.md) siguiente ilustra el uso de datos del formulario y una solicitud POST para enviar una pequeña cantidad de datos binarios como una carga de archivos a un servidor web. El código usa la clase [**HttpBufferContent**](/uwp/api/windows.web.http.httpbuffercontent) para representar los datos binarios y la clase [**HttpMultipartFormDataContent**](/uwp/api/windows.web.http.httpmultipartformdatacontent) para representar los datos del formulario de varias partes.

> [!NOTE]
> Una llamada **obtener** (como se muestra en el ejemplo de código siguiente), no es adecuada para un subproceso de interfaz de usuario. Para la técnica correcta para usar en ese caso, consulte [simultaneidad y operaciones asincrónicas con C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

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

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
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

Para publicar el contenido de un archivo binario real (en lugar de los datos binarios explícitos usados anteriormente), le resultará más fácil de usar un objeto [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent). Construya uno y, como argumento a su constructor, pase el valor devuelto de una llamada a [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync). Este método devuelve una secuencia para los datos en el archivo binario.

Además, si está cargando un archivo grande (mayor que 10MB aproximadamente), a continuación, se recomienda que use la [transferencia en segundo plano](/uwp/api/windows.networking.backgroundtransfer) de API de Windows Runtime.

## <a name="post-json-data-over-http"></a>Datos POST JSON a través de HTTP

El ejemplo siguiente registra algunos JSON a un punto de conexión y luego escribe el cuerpo de respuesta.

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

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

    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        Windows::Web::Http::HttpClient httpClient;
        Uri requestUri{ L"https://www.contoso.com/post" };

        // Construct the JSON to post.
        Windows::Web::Http::HttpStringContent jsonContent(
            L"{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding::Utf8,
            L"application/json");

        // Post the JSON, and wait for a response.
        httpResponseMessage = httpClient.PostAsync(
            requestUri,
            jsonContent).get();

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
        std::wcout << httpResponseBody.c_str();
    }
    catch (winrt::hresult_error const& ex)
    {
        std::wcout << ex.message().c_str();
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Excepciones en Windows.Web.Http

Se inicia una excepción cuando se pasa una cadena no válida del identificador uniforme de recursos (URI) al constructor del objeto [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri).

**.NET:**   el tipo[**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri) aparece como [**System.Uri**](/dotnet/api/system.uri) en C# y VB.

En C# y Visual Basic, este error puede evitarse si se usa la clase [**System.Uri**](/dotnet/api/system.uri) en .NET 4.5 y uno de los métodos [**System.Uri.TryCreate**](/dotnet/api/system.uri.trycreate#overloads) para probar la cadena recibida del usuario antes de que se cree el URI.

En C++, no hay ningún método para probar y analizar una cadena en un URI. Si una aplicación recibe información del usuario relativa a [**Windows.Foundation.Uri**](/uwp/api/Windows.Foundation.Uri), el constructor debe estar en un bloque Try/Catch. Si se genera una excepción, la aplicación podrá notificar al usuario y solicitar otro nombre de host.

[  **Windows.Web.Http**](/uwp/api/Windows.Web.Http) no tiene una función conveniente. Por este motivo, una aplicación que use [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) y otras clases de este espacio de nombres debe usar el valor **HRESULT**.

En las aplicaciones que usan .NET Framework 4.5 en C#, VB.NET, [System.Exception](/dotnet/api/system.exception) representa un error durante la ejecución de la aplicación cuando se produce una excepción. La propiedad [System.Exception.HResult](/dotnet/api/system.exception.hresult#System_Exception_HResult) devuelve el valor **HRESULT** asignado a la excepción específica. La propiedad [System.Exception.Message](/dotnet/api/system.exception.message#System_Exception_Message) devuelve el mensaje que describe la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Una aplicación puede filtrar según valores **HRESULT** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

En las aplicaciones que usan C++ administrado, [Platform::Exception](/cpp/cppcx/platform-exception-class) representa un error durante la ejecución de la aplicación cuando se produce una excepción. La propiedad [Platform::Exception::HResult](/cpp/cppcx/platform-exception-class#hresult) devuelve el valor **HRESULT** asignado a la excepción concreta. La propiedad [Platform::Exception::Message](/cpp/cppcx/platform-exception-class#message) devuelve la cadena proporcionada por el sistema asociada con el valor **HRESULT**. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Una aplicación puede filtrar según valores **HRESULT** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para la mayoría de los errores de validación de parámetros, el valor de **HRESULT** devuelto es **E\_INVALIDARG**. Para algunas llamadas a métodos no válidas, el valor de **HRESULT** devuelto es **E\_ILLEGAL\_METHOD\_CALL**.