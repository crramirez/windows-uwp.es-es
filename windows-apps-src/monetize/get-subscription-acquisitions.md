---
description: Utilice este método en la API de análisis de Microsoft Store para obtener datos de adquisición de una suscripción durante un intervalo de fechas determinado y los demás filtros opcionales del complemento.
title: Obtener adquisiciones de complementos de suscripción
ms.date: 01/25/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Store, API, las suscripciones de análisis de Microsoft Store
ms.openlocfilehash: c77cab76aa070e21288e93e5264e70325bb4d616
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714096"
---
# <a name="get-subscription-add-on-acquisitions"></a>Obtener adquisiciones de complementos de suscripción

Utilice este método en la API de análisis de Microsoft Store para obtener datos de adquisición agregado para las suscripciones del complemento para la aplicación durante un intervalo de fechas determinado y los demás filtros opcionales.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Header        | Tipo   | Descripción          |
|---------------|--------|--------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [Store ID](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que desea recuperar datos de adquisición de complemento de suscripción. |  Sí  |
| subscriptionProductId  | string | El [Store ID](in-app-purchases-and-trials.md#store-ids) del complemento de suscripción para la que desea recuperar datos de adquisición. Si no especifica este valor, este método devuelve datos de adquisición para todos los complementos de suscripción para la aplicación especificada.  | No  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de adquisición de complemento de suscripción para recuperar. El valor predeterminado es la fecha actual. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de los datos de adquisición de complemento de suscripción para recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado si no se especifica es 100. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=100 y skip=0 recuperan las primeras 100 filas de datos, los valores top=100 y skip=100 recuperan las siguientes 100 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran el cuerpo de respuesta. Cada instrucción puede utilizar los operadores **eq** o **ne**, y las instrucciones se pueden combinar mediante **and** u **or**. Puede especificar las siguientes cadenas en las instrucciones de filtro (estos se corresponden con [valores en el cuerpo de respuesta](#subscription-acquisition-values)): <ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Este es un ejemplo *filtro* parámetro: <em>Filtro = fecha eq "2017-07-08'</em>.</p> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Una instrucción que ordena el resultado de los valores de datos para cada adquisición de complemento de la suscripción. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>subscriptionProductName</strong></li><li><strong>applicationName</strong></li><li><strong>skuId</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>groupby = mercado&amp;aggregationLevel = semana</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes se muestra cómo obtener datos de adquisición de complemento de suscripción. Reemplace el *applicationId* valor con el identificador de Store adecuado para la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/subscriptions?applicationId=9NBLGGGZ5QDR&startDate=2017-07-07&endDate=2017-07-08 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción         |
|------------|--------|------------------|
| Valor      | array  | Una matriz de objetos que contienen datos de adquisición de complemento de suscripción agregada. Para obtener más información acerca de los datos de cada objeto, vea el [valores de adquisición de suscripción](#subscription-acquisition-values) sección más adelante.             |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el **superior** parámetro de la solicitud se establece en 100, pero hay más de 100 filas de datos de adquisición de complemento de suscripción para la consulta. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.       |


<span id="subscription-acquisition-values" />

### <a name="subscription-acquisition-values"></a>Valores de adquisición de suscripción

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo    | Descripción        |
|---------------------|---------|---------------------|
| date                | string  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| subscriptionProductId      | string  | El [Store ID](in-app-purchases-and-trials.md#store-ids) del complemento de suscripción para el que va a recuperar datos de adquisición.    |
| subscriptionProductName    | string  | El nombre para mostrar del complemento de suscripción.         |
| applicationId       | string  | El [Store ID](in-app-purchases-and-trials.md#store-ids) de la aplicación para el que va a recuperar datos de adquisición de complemento de suscripción.   |
| applicationName     | string  | El nombre para mostrar de la aplicación.     |
| skuId     | string  | El identificador de la [SKU](in-app-purchases-and-trials.md#products-skus) del complemento de suscripción para el que va a recuperar datos de adquisición.     |
| deviceType          | string  |  Una de las cadenas siguientes que especifica el tipo de dispositivo que ha completado la adquisición:<ul><li><strong>PC</strong></li><li><strong>Teléfono</strong></li><li><strong>Consola de</strong></li><li><strong>IoT</strong></li><li><strong>Holográfica</strong></li><li><strong>Unknown</strong></li></ul>       |
| market           | string  | Código de país ISO 3166 del mercado donde se realizó la compra.     |
| currencyCode         | string  | El código de divisa en formato ISO 4217 para ventas brutas antes de impuestos.       |
| grossSalesBeforeTax           | número entero  | Las ventas brutas en la moneda local especificada por el *currencyCode* valor.     |
| totalActiveCount             | número entero  | El número de suscripciones activas total durante el período de tiempo especificado. Esto es equivalente a la suma de los *goodStandingActiveCount*, *pendingGraceActiveCount*, *graceActiveCount*, y *lockedActiveCount* valores.  |
| totalChurnCount              | número entero  | El recuento total de las suscripciones que se han desactivado durante el período de tiempo especificado. Esto es equivalente a la suma de los *billingChurnCount*, *nonRenewalChurnCount*, *refundChurnCount*, *chargebackChurnCount*, *earlyChurnCount*, y *otherChurnCount* valores.   |
| newCount            | número entero  | El número de nuevas adquisiciones de suscripción durante el período de tiempo especificado, incluidas las evaluaciones.    |
| renewCount     | número entero  | El número de renovaciones de suscripción durante el período de tiempo especificado, incluidas las renovaciones iniciadas por el usuario y la renovación automática.        |
| goodStandingActiveCount | número entero | El número de suscripciones que estaban activas durante el período de tiempo especificado y que la fecha de expiración es > = la *endDate* valor para la consulta.    |
| pendingGraceActiveCount | número entero | El número de suscripciones que estaban activas durante el período de tiempo especificado, pero tenían un error de facturación y la fecha de expiración de suscripción es > = la *endDate* valor para la consulta.     |
| graceActiveCount | número entero | El número de suscripciones que estaban activas durante el período de tiempo especificado, pero tenían un error de facturación, y donde:<ul><li>Es la fecha de expiración de suscripción < la *endDate* valor para la consulta.</li><li>El final del período de gracia es > = la *endDate* valor.</li></ul>        |
| lockedActiveCount | número entero | El número de suscripciones que se encontraban en *recordándoles* (es decir, es expirar la suscripción y está intentando adquirir fondos para renovar automáticamente la suscripción de Microsoft) durante especificado del tiempo período y donde:<ul><li>Es la fecha de expiración de suscripción < la *endDate* valor para la consulta.</li><li>El final del período de gracia es < = el *endDate* valor.</li></ul>        |
| billingChurnCount | número entero | El número de suscripciones que se han desactivado durante el período de tiempo especificado por un error al procesar un cargo de facturación y donde las suscripciones se estaban previamente en recordándoles.        |
| nonRenewalChurnCount | número entero | El número de suscripciones que se han desactivado durante el período de tiempo especificado porque no se renuevan.        |
| refundChurnCount | número entero | El número de suscripciones que se han desactivado durante el período de tiempo especificado porque estaban reembolsarán.        |
| chargebackChurnCount | número entero | El número de suscripciones que se han desactivado durante el período de tiempo especificado debido a una anulación.        |
| earlyChurnCount | número entero | El número de suscripciones que se han desactivado durante el período de tiempo especificado mientras eran de buena reputación.        |
| otherChurnCount | número entero | El número de suscripciones que se han desactivado durante el período de tiempo especificado por otras razones.        |


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

* [Informe Adquisiciones de complementos](../publish/add-on-acquisitions-report.md)
* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)

 

 
