---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición de una suscripción de complementos durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener adquisiciones de complementos de suscripción
ms.date: 01/25/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, API de análisis de Microsoft Store, suscripciones
ms.openlocfilehash: 4c307dbf7d17251e3d3c5f8792d8ea3d049924d9
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493550"
---
# <a name="get-subscription-add-on-acquisitions"></a>Obtener adquisiciones de complementos de suscripción

Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición agregados para las suscripciones de complementos de la aplicación durante un intervalo de fechas determinado y otros filtros opcionales.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción          |
|---------------|--------|--------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de adquisición del complemento de suscripción. |  Sí  |
| subscriptionProductId  | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del complemento de suscripción para el que desea recuperar los datos de adquisición. Si no especifica este valor, este método devuelve los datos de adquisición de todos los complementos de suscripción de la aplicación especificada.  | No  |
| startDate | date | Fecha de inicio del intervalo de fechas de los datos de adquisición del complemento de suscripción que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de adquisición del complemento de suscripción que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado si no se especifica es 100. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=100 y skip=0 recuperan las primeras 100 filas de datos, los valores top=100 y skip=100 recuperan las siguientes 100 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran el cuerpo de la respuesta. Cada instrucción puede utilizar los operadores **eq** o **ne**, y las instrucciones se pueden combinar mediante **and** u **or**. Puede especificar las siguientes cadenas en las instrucciones de filtro (que se corresponden con [los valores del cuerpo de respuesta](#subscription-acquisition-values)): <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>datamarket</strong></li><li><strong>TipoDeDispositivo</strong></li></ul><p>Este es un parámetro de *filtro* de ejemplo: <em>Filter = date EQ ' 2017-07-08 '</em>.</p> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Una instrucción que ordena los valores de los datos de resultado de cada adquisición del complemento de suscripción. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>datamarket</strong></li><li><strong>TipoDeDispositivo</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>datamarket</strong></li><li><strong>TipoDeDispositivo</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>GroupBy = Market &amp; aggregationLevel = Week</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En los siguientes ejemplos se muestra cómo obtener los datos de adquisición del complemento de suscripción. Reemplace el valor de *ApplicationID* por el identificador de almacén adecuado para su aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción         |
|------------|--------|------------------|
| Value      | array  | Matriz de objetos que contienen datos agregados de adquisición de complementos de suscripción. Para obtener más información sobre los datos de cada objeto, consulte la sección [valores de adquisición de suscripción](#subscription-acquisition-values) a continuación.             |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 100, pero hay más de 100 filas de datos de adquisición del complemento de suscripción para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>Valores de adquisición de suscripción

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo    | Descripción        |
|---------------------|---------|---------------------|
| date                | string  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| subscriptionProductId      | string  | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del complemento de suscripción para el que se recuperan los datos de adquisición.    |
| subscriptionProductName    | string  | El nombre para mostrar del complemento de suscripción.         |
| applicationId       | string  | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que se recuperan los datos de adquisición del complemento de suscripción.   |
| applicationName     | string  | El nombre para mostrar de la aplicación.     |
| skuId     | string  | IDENTIFICADOR de la [SKU](in-app-purchases-and-trials.md#products-skus) del complemento de suscripción para el que se recuperan los datos de adquisición.     |
| deviceType          | string  |  Una de las siguientes cadenas que especifica el tipo de dispositivo que completó la adquisición:<ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>       |
| market           | string  | Código de país ISO 3166 del mercado donde se realizó la compra.     |
| currencyCode         | string  | El código de moneda en formato ISO 4217 para ventas brutas antes de los impuestos.       |
| grossSalesBeforeTax           | integer  | Ventas brutas en la moneda local especificada por el valor de *currencyCode* .     |
| totalActiveCount             | integer  | Número total de suscripciones activas durante el período de tiempo especificado. Esto es equivalente a la suma de los valores *goodStandingActiveCount*, *pendingGraceActiveCount*, *graceActiveCount*y *lockedActiveCount* .  |
| totalChurnCount              | integer  | Número total de suscripciones que se han desactivado durante el período de tiempo especificado. Esto es equivalente a la suma de los valores *billingChurnCount*, *nonRenewalChurnCount*, *refundChurnCount*, *chargebackChurnCount*, *earlyChurnCount*y *otherChurnCount* .   |
| newCount            | integer  | El número de nuevas adquisiciones de suscripciones durante el período de tiempo especificado, incluidas las pruebas.    |
| renewCount     | integer  | El número de renovaciones de suscripción durante el período de tiempo especificado, incluidas las renovaciones iniciadas por el usuario y las renovaciones automáticas.        |
| goodStandingActiveCount | integer | El número de suscripciones que estaban activas durante el período de tiempo especificado y donde la fecha de expiración es >= el valor de *EndDate* de la consulta.    |
| pendingGraceActiveCount | integer | El número de suscripciones que estaban activas durante el período de tiempo especificado pero tuvieron un error de facturación y donde la fecha de expiración de la suscripción es >= el valor de *EndDate* de la consulta.     |
| graceActiveCount | integer | El número de suscripciones que estaban activas durante el período de tiempo especificado pero tuvieron un error de facturación y donde:<ul><li>La fecha de expiración de la suscripción es < el valor de *EndDate* de la consulta.</li><li>El final del período de gracia es >= el valor de *EndDate* .</li></ul>        |
| lockedActiveCount | integer | El número de suscripciones que *estaban en las* demás (es decir, la suscripción está a punto de expirar y Microsoft está intentando adquirir fondos para renovar automáticamente la suscripción) durante el período de tiempo especificado y:<ul><li>La fecha de expiración de la suscripción es < el valor de *EndDate* de la consulta.</li><li>El final del período de gracia es <= el valor de *EndDate* .</li></ul>        |
| billingChurnCount | integer | El número de suscripciones que se han desactivado durante el período de tiempo especificado debido a un error en el procesamiento de un cargo de facturación y en el que las suscripciones estaban anteriormente en las demás.        |
| nonRenewalChurnCount | integer | Número de suscripciones que se han desactivado durante el período de tiempo especificado porque no se han renovado.        |
| refundChurnCount | integer | Número de suscripciones que se han desactivado durante el período de tiempo especificado porque se volvieron a reembolsar.        |
| chargebackChurnCount | integer | Número de suscripciones que se han desactivado durante el período de tiempo especificado debido a un recargo.        |
| earlyChurnCount | integer | Número de suscripciones que se han desactivado durante el período de tiempo especificado mientras estaban en buen estado.        |
| otherChurnCount | integer | Número de suscripciones que se han desactivado durante el período de tiempo especificado por otros motivos.        |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9KDLGHH6R365",
      "subscriptionProductName": "Contoso App Subscription with One Month Free Trial",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "Unknown",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    },
    {
      "date": "2017-07-08",
      "subscriptionProductId": "9JJFDHG4R478",
      "subscriptionProductName": "Contoso App Monthly Subscription",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso App",
      "skuId": "0020",
      "market": "US",
      "deviceType": "PC",
      "currencyCode": "USD",
      "grossSalesBeforeTax": 0.0,
      "totalActiveCount": 1,
      "totalChurnCount": 0,
      "newCount": 0,
      "renewCount": 0,
      "goodStandingActiveCount": 1,
      "pendingGraceActiveCount": 0,
      "graceActiveCount": 0,
      "lockedActiveCount": 0,
      "billingChurnCount": 0,
      "nonRenewalChurnCount": 0,
      "refundChurnCount": 0,
      "chargebackChurnCount": 0,
      "earlyChurnCount": 0,
      "otherChurnCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe de adquisiciones de complementos](../publish/add-on-acquisitions-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)

 

 
