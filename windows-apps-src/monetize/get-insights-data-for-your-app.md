---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de información de la aplicación.
title: Obtener datos de información
ms.author: mhopkins
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, servicios de la tienda, API de información de análisis de Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 30b9303fc44f557210c9ba80a2a135f77909dc10
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "4950230"
---
# <a name="get-insights-data"></a>Obtener datos de información

Usa este método en la API de análisis de Microsoft Store para obtener datos de información relacionada con adquisiciones, estado y métricas de uso de una aplicación durante un intervalo de fechas dado y según otros filtros opcionales. Esta información también está disponible en el [informe de información](../publish/insights-report.md) en el panel del centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El [Identificador de la tienda](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que quieres recuperar datos de información. Si no se especifica este parámetro, el cuerpo de la respuesta contendrá los datos de información de todas las aplicaciones registradas en tu cuenta.  |  No  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de información para recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de los datos de información para recuperar. El valor predeterminado es la fecha actual. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *filter = dataType eq 'adquisición'*. <p/><p/>Puedes especificar los siguientes campos de filtro:<p/><ul><li><strong>adquisición</strong></li><li><strong>salud</strong></li><li><strong>uso</strong></li></ul> | No   |

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos de información. Sustituye el valor *applicationId* por el id. de la Store de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Una matriz de objetos que contienen datos de información de la aplicación. Para obtener más información sobre los datos de cada objeto, consulta la sección de [valores de Insight](#insight-values) a continuación.                                                                                                                      |
| TotalCount | entero    | Número total de filas en el resultado de datos de la consulta.                 |


### <a name="insight-values"></a>Valores de detalles de valoración

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | El identificador de la aplicación para la que estás recuperando los datos de información de la tienda.     |
| insightDate                | string | La fecha en la que hemos identificado el cambio en una métrica específica. Esta fecha representa el final de la semana en el que hemos detectado un aumento significativo o reducir en una métrica en comparación con la semana anterior. |
| dataType     | string | Una de las siguientes cadenas que especifica el área de análisis general que se describe en esta información:<p/><ul><li><strong>adquisición</strong></li><li><strong>salud</strong></li><li><strong>uso</strong></li></ul>   |
| insightDetail          | array | Uno o más [valores de InsightDetail](#insightdetail-values) que representan los detalles de insight actual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Uno de los valores siguientes que indica la métrica que describe la información actual o la dimensión actual, según el valor de **tipo de datos** .<ul><li>**Salud**, este valor es siempre **recuento de visitas**.</li><li>Para la **adquisición**, este valor es siempre **AcquisitionQuantity**.</li><li>Para el **uso**, este valor puede ser una de las siguientes cadenas:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  Uno o varios objetos que describen una métrica única para la perspectiva.   |
| CambioPorcentual            | string |  El porcentaje de la métrica ha cambiado a través de la base de clientes completa.  |
| DimensionName           | string |  El nombre de la métrica que se describe en la dimensión actual. Algunos ejemplos son **EventType**, **mercado**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **AgeGroup** y **sexo**.   |
| DimensionValue              | string | El valor de la métrica que se describe en la dimensión actual. Por ejemplo, si **DimensionName** es **EventType**, podría ser **DimensionValue** **bloqueo** o **falta de respuesta**.   |
| FactValue     | string | El valor absoluto de la métrica en la fecha en que se detectó la perspectiva.  |
| Direction | string |  La dirección del cambio (**positivo** o **negativo**).   |
| Date              | cadena |  La fecha en la que hemos identificado el cambio relacionadas con la información actual o la dimensión actual.   |

### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe de información](../publish/insights-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
