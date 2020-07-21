---
description: Use este método en la API de Microsoft Store Analytics para obtener conversiones de agregado por datos de canal para un complemento durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener conversiones de complementos por canal
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, conversiones de complementos, canal
ms.localizationpriority: medium
ms.openlocfilehash: d2458a880d4fb767eb0158cbeaf0041b58237b61
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493030"
---
# <a name="get-add-on-conversions-by-channel"></a>Obtener conversiones de complementos por canal

Use este método en la API de Microsoft Store Analytics para obtener conversiones de agregado por canal para un complemento durante un intervalo de fechas determinado y otros filtros opcionales.

* Una *conversión* significa que un cliente (con sesión iniciada con una cuenta de Microsoft) ha obtenido recientemente una licencia para el complemento (tanto si se le cobra dinero como si lo ha ofrecido gratis).
* El *canal* es el método en el que un cliente llegó a la página de lista de la aplicación (por ejemplo, a través de la tienda o de una [campaña de promoción de aplicaciones personalizada](../publish/create-a-custom-app-promotion-campaign.md)).

Esta información también está disponible en el [Informe de adquisiciones de complementos](../publish/add-on-acquisitions-report.md#add-on-page-views-and-conversions-by-campaign-id) del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de conversión de complementos. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| inAppProductId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del complemento para el que desea recuperar los datos de conversión.  | Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de datos de conversión que se va a recuperar. El valor predeterminado es 1/1/2016. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de datos de conversión que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran el cuerpo de la respuesta. Cada instrucción puede utilizar los operadores **eq** o **ne**, y las instrucciones se pueden combinar mediante **and** u **or**. Puede especificar las siguientes cadenas en las instrucciones de filtro.  Para obtener descripciones, consulte la sección [valores de conversión](#conversion-values) de este artículo. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>datamarket</strong></li></ul><p>Este es un parámetro de *filtro* de ejemplo: <em>Filter = DEVICETYPE EQ ' PC '</em>.</p> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Instrucción que ordena los valores de los datos de resultado de cada conversión. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>datamarket</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<p/><ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>datamarket</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>inAppProductName</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>GroupBy = edad, Market &amp; aggregationLevel = Week</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestran varias solicitudes para obtener datos de conversión de aplicaciones. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de conversión agregados para el complemento. Para obtener más información acerca de los datos de cada objeto, consulte la sección [valores de conversión](#conversion-values) a continuación.                     |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 10, pero hay más de 10 filas de datos de conversión para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.                                                                                                                                                                                                                             |

### <a name="conversion-values"></a>Valores de conversión

Los objetos de la matriz de *valores* contienen los valores siguientes.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Primera fecha del intervalo de fechas de los datos de conversión. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| inAppProductId      | string  | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del complemento para el que se recuperan los datos de conversión.     |
| inAppProductName    | string  | El nombre para mostrar del complemento para el que se recuperan los datos de conversión.   |
| applicationId       | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que se recuperan los datos de conversión.     |
| applicationName     | string | Nombre para mostrar de la aplicación para la que se recuperan los datos de conversión.        |
| appType          | string |  Tipo del producto para el que se recuperan los datos de conversión. Para este método, el único valor admitido es el **complemento**.            |
| customCampaignId           | string |  La cadena de identificador de una [campaña de promoción de aplicaciones personalizada](../publish/create-a-custom-app-promotion-campaign.md) que está asociada a la aplicación.   |
| referrerUriDomain           | string |  Especifica el dominio en el que se ha activado la lista de aplicaciones con el identificador de campaña de promoción de aplicación personalizado.   |
| channelType           | string |  Una de las siguientes cadenas que especifica el canal para la conversión:<ul><li><strong>CustomCampaignId</strong></li><li><strong>Almacenar tráfico</strong></li><li><strong>Otros</strong></li></ul>    |
| storeClient         | string | Versión del almacén donde se produjo la conversión. Actualmente, el único valor admitido es **SFC**.    |
| deviceType          | string | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | string | El código de país ISO 3166 del mercado en el que se produjo la conversión.    |
| clickCount              | number  |     El número de clics del cliente en el vínculo de la lista de aplicaciones.      |           
| conversionCount            | number  |   El número de conversiones de clientes.         |         


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso Add-On",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "Add-On",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "CN",
      "clickCount": 1,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe de adquisiciones de complementos](../publish/add-on-acquisitions-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
