---
author: stevewhims
description: Usa HttpClient y el resto de la API del espacio de nombres Windows.Web.Http para enviar y recibir información mediante los protocolos HTTP 2.0 y HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c874c690826dfa74b8dcb2312204cd549db3db2b
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035690"
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

La clase [**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) representa un mensaje de solicitud HTTP enviado por [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639). La clase [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) representa un mensaje de respuesta HTTP recibido de una solicitud HTTP. IETF define los mensajes HTTP en la especificación [RFC 2616](http://go.microsoft.com/fwlink/p/?linkid=241642).

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

## <a name="exceptions-in-windowswebhttp"></a>Excepciones en Windows.Web.Http

Se inicia una excepción cuando se pasa una cadena no válida del identificador uniforme de recursos (URI) al constructor del objeto [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**. NET:** el tipo de [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) aparece como [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en C# y VB.

En C# y Visual Basic, este error puede evitarse si se usa la clase [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) en .NET 4.5 y uno de los métodos [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) para probar la cadena recibida del usuario antes de que se cree el URI.

En C++, no hay ningún método para probar y analizar una cadena en un URI. Si una aplicación recibe información del usuario relativa a [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), el constructor debe estar en un bloque Try/Catch. Si se genera una excepción, la aplicación podrá notificar al usuario y solicitar otro nombre de host.

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) no tiene una función conveniente. Por este motivo, una aplicación que use [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) y otras clases de este espacio de nombres debe usar el valor **HRESULT**.

En las aplicaciones usando el Framework4.5 de .NET en C#, VB.NET, la [System.Exception](http://msdn.microsoft.com/library/system.exception.aspx) representa un error durante la ejecución de la aplicación cuando se produce una excepción. La propiedad [System.Exception.HResult](http://msdn.microsoft.com/library/system.exception.hresult.aspx) devuelve el valor **HRESULT** asignado a la excepción específica. La propiedad [System.Exception.Message](http://msdn.microsoft.com/library/system.exception.message.aspx) devuelve el mensaje que describe la excepción. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Una aplicación puede filtrar según valores **HRESULT** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

En las aplicaciones que usan C++ administrado, [Platform::Exception](http://msdn.microsoft.com/library/windows/apps/hh755825.aspx) representa un error durante la ejecución de la aplicación cuando se produce una excepción. La propiedad [Platform::Exception::HResult](http://msdn.microsoft.com/library/windows/apps/hh763371.aspx) devuelve el valor **HRESULT** asignado a la excepción concreta. La propiedad [Platform::Exception::Message](http://msdn.microsoft.com/library/windows/apps/hh763375.aspx) devuelve la cadena proporcionada por el sistema asociada con el valor **HRESULT**. Los valores posibles de **HRESULT** se enumeran en el archivo de encabezado *Winerror.h*. Una aplicación puede filtrar según valores **HRESULT** concretos para modificar el comportamiento de la aplicación en función del motivo de la excepción.

Para la mayoría de los errores de validación de parámetros, el valor de **HRESULT** devuelto es **E\_INVALIDARG**. Para algunas llamadas a métodos no válidas, el valor de **HRESULT** devuelto es **E\_ILLEGAL\_METHOD\_CALL**.

