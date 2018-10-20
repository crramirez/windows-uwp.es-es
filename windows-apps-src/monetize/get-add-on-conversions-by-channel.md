---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener conversiones agregadas por datos de canal de un complemento durante un intervalo de fechas concreto y según otros filtros opcionales.
title: Obtener conversiones de complementos por canal
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, servicios de Microsoft Store, API de análisis de Microsoft Store, conversiones de complementos, canal
ms.localizationpriority: medium
ms.openlocfilehash: af29c790df5508a22c545cdc5a2ca2faac15e134
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "5164344"
---
# <a name="get-add-on-conversions-by-channel"></a>Obtener conversiones de complementos por canal

Usa este método en la API de análisis de Microsoft Store para obtener conversiones agregadas por canal de un complemento durante un intervalo de fechas concreto y según otros filtros opcionales.

* Una *conversión* significa que un cliente (que ha iniciado sesión con una cuenta de Microsoft) acaba de obtener una licencia para tu complemento (tanto si cobras dinero como si la ofreces gratis).
* El *canal* es el método en que un cliente llegó a la página de descripción de tu aplicación (por ejemplo, a través de la Store o una [campaña de promoción de la aplicación personalizada](../publish/create-a-custom-app-promotion-campaign.md)).

Esta información también está disponible en el [informe de adquisiciones de complementos](../publish/add-on-acquisitions-report.md#add-on-page-views-and-conversions-by-campaign-id) del panel del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador de la Store](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que quieres recuperar los datos de conversión de complementos. Un ejemplo de un id. de la Store sería 9WZDNCRFJ3Q8. |  Sí  |
| inAppProductId | string | El [Id. de la Store](in-app-purchases-and-trials.md#store-ids) del complemento para el que quieres recuperar los datos de conversión.  | Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos conversión que se van a recuperar. El valor predeterminado es 1/1/2016. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de conversión que se van a recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran el cuerpo de respuesta. Cada instrucción puede utilizar los operadores **eq** o **ne**, y las instrucciones se pueden combinar mediante **and** u **or**. Puedes especificar las siguientes cadenas en las instrucciones de filtro.  Para obtener descripciones, consulta la sección de [valores de conversión](#conversion-values) de este artículo. <ul><li><strong>applicationName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>Este es un parámetro *filter* de ejemplo: <em>filter=deviceType eq 'PC'</em>.</p> | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada conversión. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>El parámetro <em>order</em> es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<p/><ul><li><strong>fecha</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>appType</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>storeClient</strong></li><li><strong>deviceType</strong></li><li><strong>market</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>fecha</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>inAppProductName</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con el parámetro <em>aggregationLevel</em>. Por ejemplo: <em>groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener los datos de conversión de la aplicación. Sustituye el valor *applicationId* por el id. de la Store de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Matriz de objetos que contienen los datos de conversión agregados del complemento. Para más información sobre los datos de cada objeto, consulta la sección [valores de conversión](#conversion-values) que encontrarás a continuación.                     |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de conversión de la solicitud. |
| TotalCount | entero    | Número total de filas en el resultado de datos de la consulta.                                                                                                                                                                                                                             |

### <a name="conversion-values"></a>Valores de conversión

Los objetos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| fecha                | string | La primera fecha del intervalo de fechas de los datos de conversión. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| inAppProductId      | string  | El [id. de la Store](in-app-purchases-and-trials.md#store-ids) del complemento para el que quieres recuperar datos de conversión.     |
| inAppProductName    | string  | El nombre para mostrar del complemento para el que quieres recuperar datos de conversión.   |
| applicationId       | string | El [Id. de la Store](in-app-purchases-and-trials.md#store-ids) de la aplicación sobre la que estás recuperando los datos de conversión.     |
| applicationName     | string | El nombre para mostrar de la aplicación para la que quieres recuperar datos de conversión.        |
| appType          | string |  Tipo de producto para el que recuperas los datos de conversión. Para este método, el único valor admitido es **Add-On**.            |
| customCampaignId           | string |  La cadena de identificador para una [campaña de promoción de la aplicación personalizada](../publish/create-a-custom-app-promotion-campaign.md) que está asociada a la aplicación.   |
| referrerUriDomain           | string |  Especifica el dominio donde se activó la descripción de la aplicación con el identificador de la campaña de promoción de la aplicación personalizada.   |
| channelType           | string |  Una de las siguientes cadenas que especifica el canal para la conversión:<ul><li><strong>CustomCampaignId</strong></li><li><strong>Tráfico de la Store</strong></li><li><strong>Otros</strong></li></ul>    |
| storeClient         | string | Versión de la Store donde se realizó la conversión. Actualmente, el único valor admitido es **SFC**.    |
| deviceType          | string | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>            |
| market              | string | Código de país ISO 3166 del mercado donde se realizó la conversión.    |
| clickCount              | number  |     Número de clics del cliente en el vínculo descriptivo de la aplicación.      |           
| conversionCount            | number  |   Número de conversiones del cliente.         |         


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
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener adquisiciones de complementos](get-in-app-acquisitions.md)
