---
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos de clasificación agregados de un intervalo de fechas y otros filtros opcionales.
title: Obtener las valoraciones de la aplicación
ms.date: 11/29/2017
ms.topic: article
keywords: windows 10, UWP, servicios de Microsoft Store, Store services, API de análisis de Microsoft Store, Microsoft Store analytics API, valoraciones, ratings
ms.localizationpriority: medium
ms.openlocfilehash: 32f32d3389a340c25d99ec0f0a68e0c20af89c2b
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7980432"
---
# <a name="get-app-ratings"></a>Obtener las valoraciones de la aplicación

Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados de clasificación en formato JSON pertenecientes a un intervalo de fechas dado y según otros filtros opcionales. Esta información también está disponible en el [informe de críticas](../publish/reviews-report.md) en el centro de partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.


## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) de la aplicación sobre la que quieres recuperar los datos de clasificación.  |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de clasificación que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos de clasificación que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | cadena | Instrucción que ordena los valores de datos resultantes de cada clasificación. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>isRevised</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>fiveStars</strong></li><li><strong>fourStars</strong></li><li><strong>threeStars</strong></li><li><strong>twoStars</strong></li><li><strong>oneStar</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=osVersion,market&amp;aggregationLevel=week</em></p> |  No  |

 
### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**.

Este es un ejemplo de la cadena *filter*: *filter=market eq 'US' and deviceType eq 'teléfono' and isRevised eq true*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que los valores de la cadena deben estar entre comillas simples en el parámetro *filter*.

| Campos        |  Descripción        |
|---------------|-----------------|
| market | Una cadena que contiene el código de país ISO 3166 del mercado donde se ha valorado la aplicación. |
| osVersion | Una de las cadenas siguientes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| isRevised | Especifica <strong>true</strong> para filtrar las clasificaciones que hayan sido revisadas; de lo contrario, especifica <strong>false</strong>. |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de clasificación. Reemplaza el valor *applicationId* por el Id. de la Store de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | matriz  | Matriz de objetos que contienen datos de clasificación agregados. Para más información sobre los datos de cada objeto, consulta la sección [valores de clasificación](#rating-values) que encontrarás a continuación.                                                                                                                           |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de datos de clasificación de la solicitud. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.                               |


### <a name="rating-values"></a>Valores de clasificación

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción       |
|-----------------|---------|-------------------|
| date            | cadena  | Es la primera fecha del intervalo de fechas de los datos de clasificación. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | cadena  | El Id. de la Store de la aplicación sobre la que estás recuperando los datos de clasificación.         |
| applicationName | cadena  | Nombre para mostrar de la aplicación.    |
| market          | cadena  | Código de país ISO 3166 del mercado desde el cual se envió la clasificación.        |
| osVersion       | cadena  | Versión del sistema operativo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).            |
| deviceType      | cadena  | Tipo de dispositivo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).            |
| isRevised       | Booleano | El valor **true** indica que la clasificación fue revisada; en caso contrario, será **false**.   |
| oneStar         | número  | Número de clasificaciones de una estrella.        |
| twoStars        | número  | Número de clasificaciones de dos estrellas.    |
| threeStars      | número  | Número de clasificaciones de tres estrellas.   |
| fourStars       | número  | Número de clasificaciones de cuatro estrellas.    |
| fiveStars       | número  | Número de clasificaciones de cinco estrellas.    |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-02-13",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "CN",
      "osVersion": "8.0.10517.0",
      "deviceType": "Phone",
      "isRevised": false,
      "oneStar": 0,
      "twoStars": 0,
      "threeStars": 0,
      "fourStars": 0,
      "fiveStars": 2
    }
  ],
  "@nextLink": "ratings?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 15242
}

```

## <a name="related-topics"></a>Temas relacionados

* [Informe Clasificaciones](../publish/reviews-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener adquisiciones de aplicaciones](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener opiniones de la aplicación](get-app-reviews.md)
