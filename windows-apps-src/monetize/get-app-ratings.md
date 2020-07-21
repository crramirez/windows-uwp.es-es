---
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: Use este método en la API de Microsoft Store Analytics para obtener los datos de clasificación agregados para un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener las valoraciones de la aplicación
ms.date: 11/29/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, API de Microsoft Store Analytics, clasificaciones
ms.localizationpriority: medium
ms.openlocfilehash: 228f567ecb0d89f2e5af0b53c7105aa9a9fca213
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492930"
---
# <a name="get-app-ratings"></a>Obtener las valoraciones de la aplicación

Use este método en la API de Microsoft Store Analytics para obtener datos de clasificación agregados en formato JSON para un intervalo de fechas determinado y otros filtros opcionales. Esta información también está disponible en el [Informe de revisiones](../publish/reviews-report.md) del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.


## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de clasificación.  |  Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de clasificación que se han de recuperar. La fecha predeterminada es la actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de clasificación que se han de recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada clasificación. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li><strong>date</strong></li><li><strong>osVersion</strong></li><li><strong>datamarket</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>isRevised</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>isRevised</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>fiveStars</strong></li><li><strong>fourStars</strong></li><li><strong>threeStars</strong></li><li><strong>twoStars</strong></li><li><strong>oneStar</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em> &amp; GroupBy = osVersion, Market &amp; aggregationLevel = Week</em></p> |  No  |

 
### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**.

Este es un ejemplo de la cadena *filter*: *filter=market eq 'US' and deviceType eq 'teléfono' and isRevised eq true*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples.

| Campos        |  Descripción        |
|---------------|-----------------|
| market | Una cadena que contiene el código de país ISO 3166 del mercado donde se ha valorado la aplicación. |
| osVersion | Una de las cadenas siguientes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| isRevised | Especifica <strong>true</strong> para filtrar las clasificaciones que hayan sido revisadas; de lo contrario, especifica <strong>false</strong>. |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de clasificación. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de clasificación agregados. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de clasificación](#rating-values) que encontrarás a continuación.                                                                                                                           |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de clasificación de la solicitud. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.                               |


### <a name="rating-values"></a>Valores de clasificación

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value           | Tipo    | Descripción       |
|-----------------|---------|-------------------|
| date            | string  | Es la primera fecha del intervalo de fechas de los datos de clasificación. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | string  | El Id. de la Tienda de la aplicación sobre la que estás recuperando los datos de clasificación.         |
| applicationName | string  | El nombre para mostrar de la aplicación.    |
| market          | string  | Código de país ISO 3166 del mercado desde el cual se envió la clasificación.        |
| osVersion       | string  | Versión del sistema operativo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).            |
| deviceType      | string  | Tipo de dispositivo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).            |
| isRevised       | Boolean | El valor **true** indica que la clasificación fue revisada; en caso contrario, será **false**.   |
| oneStar         | number  | Número de clasificaciones de una estrella.        |
| twoStars        | number  | Número de clasificaciones de dos estrellas.    |
| threeStars      | number  | Número de clasificaciones de tres estrellas.   |
| fourStars       | number  | Número de clasificaciones de cuatro estrellas.    |
| fiveStars       | number  | Número de clasificaciones de cinco estrellas.    |


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

* [Informe de clasificación](../publish/reviews-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener las opiniones de la aplicación](get-app-reviews.md)
