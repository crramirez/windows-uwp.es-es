---
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: Use este método en la API de Microsoft Store Analytics para obtener datos de revisión para un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener las opiniones de la aplicación
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, revisiones
ms.localizationpriority: medium
ms.openlocfilehash: 01a22d22245882454fce6eb53b67c4fec0f8072b
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493110"
---
# <a name="get-app-reviews"></a>Obtener las opiniones de la aplicación


Use este método en la API de Microsoft Store Analytics para obtener datos de revisión en formato JSON para un intervalo de fechas determinado y otros filtros opcionales. Esta información también está disponible en el [Informe de revisiones](../publish/reviews-report.md) del centro de Partners.

Después de recuperar las revisiones, puede usar los métodos [Get Response info for App](get-response-info-for-app-reviews.md) Reviews y Submit Response [to App Reviews](submit-responses-to-app-reviews.md) en la API de Microsoft Store Reviews para responder a las revisiones mediante programación.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|---------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de revisión.  |  Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de revisión que se han de recuperar. La fecha predeterminada es la actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de revisión que se han de recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |string  | Una o más instrucciones que filtran las filas de la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |
| orderby | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>datamarket</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>isRevised</strong></li><li><strong>packageVersion</strong></li><li><strong>deviceModel</strong></li><li><strong>productFamily</strong></li><li><strong>deviceScreenResolution</strong></li><li><strong>isTouchEnabled</strong></li><li><strong>reviewerName</strong></li><li><strong>reviewTitle</strong></li><li><strong>reviewText</strong></li><li><strong>helpfulCount</strong></li><li><strong>notHelpfulCount</strong></li><li><strong>responseDate</strong></li><li><strong>responseText</strong></li><li><strong>deviceRAM</strong></li><li><strong>deviceStorageCapacity</strong></li><li><strong>clasific</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |


### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un elemento que se asocian a los operadores **eq** o **ne**; asimismo, algunos campos también son compatibles con los operadores **contains**, **gt**, **lt**, **ge**, y **le**. Las instrucciones se pueden combinar mediante **and** u **or**.

Este es un ejemplo de una cadena *filter*: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

Para obtener una lista de los campos y operadores compatibles de cada campo, consulta la siguiente tabla. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples.

| Campos        | Operadores admitidos   |  Descripción        |
|---------------|--------|-----------------|
| market | eq, ne | Cadena que contiene el código de país ISO 3166 del mercado del dispositivo. |
| osVersion  | eq, ne  | Una de las cadenas siguientes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>  |
| deviceType  | eq, ne  | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>  |
| isRevised  | eq, ne  | Especifica <strong>true</strong> para filtrar las opiniones que hayan sido revisadas; de lo contrario, especifica <strong>false</strong>.  |
| packageVersion  | eq, ne  | Versión del paquete de aplicaciones que se revisó.  |
| deviceModel  | eq, ne  | Tipo de dispositivo en el cual se revisó la aplicación.  |
| productFamily  | eq, ne  | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Tableta</strong></li><li><strong>Número</strong></li><li><strong>Wearable</strong></li><li><strong>Server</strong></li><li><strong>Collaborative</strong></li><li><strong>Otros</strong></li></ul>  |
| deviceRAM  | eq, ne, gt, lt, ge, le  | La RAM física, en MB.  |
| deviceScreenResolution  | eq, ne  | La resolución de pantalla del dispositivo en el formato &quot; <em>ancho</em> x <em>alto</em> &quot; .   |
| deviceStorageCapacity  | eq, ne, gt, lt, ge, le   | La capacidad del disco de almacenamiento principal, en GB.  |
| isTouchEnabled  | eq, ne  | Especifica <strong>true</strong> para filtrar según los dispositivos táctiles; en caso contrario, especifica <strong>false</strong>.   |
| reviewerName  | eq, ne  |  El nombre de la persona que ha dado la opinión. |
| rating  | eq, ne, gt, lt, ge, le  | La clasificación de la aplicación, en estrellas.  |
| reviewTitle  | eq, ne, contains  | El título de la opinión.  |
| reviewText  | eq, ne, contains  |  El contenido de texto de la opinión. |
| helpfulCount  | eq, ne  |  El número de veces que la opinión se ha marcado como útil. |
| notHelpfulCount  | eq, ne  | El número de veces que la opinión se ha marcado como no útil.  |
| responseDate  | eq, ne  | La fecha en la que se envió la respuesta.  |
| responseText  | eq, ne, contains  | El contenido de texto de la respuesta.  |
| id  | eq, ne  | IDENTIFICADOR de la revisión (es un GUID).        |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de opiniones. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción      |
|------------|--------|------------------|
| Value      | array  | Matriz de objetos que contienen los datos de revisión. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de revisión](#review-values) que encontrarás a continuación.       |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de opiniones de la solicitud. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.  |

 
### <a name="review-values"></a>Valores de revisión

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value           | Tipo    | Descripción       |
|-----------------|---------|-------------------|
| date            | string  | La primera fecha del intervalo de fechas de los datos de opiniones. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | string  | El Id. de la Tienda de la aplicación sobre la que estás recuperando los datos de opiniones.         |
| applicationName | string  | El nombre para mostrar de la aplicación.    |
| market          | string  | El código de país ISO 3166 del mercado desde el cual se ha enviado la opinión.        |
| osVersion       | string  | La versión del sistema operativo desde el cual se ha enviado la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).            |
| deviceType      | string  | El tipo de dispositivo desde el cual se ha enviado la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).            |
| isRevised       | Boolean | El valor **true** indica que la opinión fue revisada; en caso contrario, será **false**.   |
| packageVersion  | string  | Versión del paquete de aplicaciones que se revisó.        |
| deviceModel        | string  |Tipo de dispositivo en el cual se revisó la aplicación.     |
| productFamily      | string  | Nombre de la familia de dispositivos. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).   |
| deviceRAM       | number  | La RAM física, en MB.    |
| deviceScreenResolution       | string  | La resolución de pantalla del dispositivo en el formato "*ancho* x *alto*".    |
| deviceStorageCapacity | number | La capacidad del disco de almacenamiento principal, en GB. |
| isTouchEnabled | Boolean | El valor **true** indica que la función táctil está habilitada; de lo contrario, el valor será **false**. |
| reviewerName | string | El nombre de la persona que ha dado la opinión. |
| rating | number | La clasificación de la aplicación, en estrellas. |
| reviewTitle | string | El título de la opinión. |
| reviewText | string | El contenido de texto de la opinión. |
| helpfulCount | number | El número de veces que la opinión se ha marcado como útil. |
| notHelpfulCount | number | El número de veces que la opinión se ha marcado como no útil. |
| responseDate | string | La fecha en la que se envió una respuesta. |
| responseText | string | El contenido de texto de la respuesta. |
| id | string | IDENTIFICADOR de la revisión (es un GUID). Puede usar este identificador en los métodos [obtener información de respuesta para las revisiones de la aplicación](get-response-info-for-app-reviews.md) y [enviar respuestas a las revisiones](submit-responses-to-app-reviews.md) de la aplicación. |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1",
      "id": "6be543ff-1c9c-4534-aced-af8b4fbe0316"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe Críticas](../publish/reviews-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtención de información de respuesta para las revisiones de la aplicación](get-response-info-for-app-reviews.md)
* [Enviar respuestas a las revisiones de la aplicación](submit-responses-to-app-reviews.md)
* [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener las valoraciones de la aplicación](get-app-ratings.md)
