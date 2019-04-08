---
description: Utilice este método en la API de análisis de Microsoft Store para obtener información para la aplicación.
title: Obtener datos de insights
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Store, API, información de análisis de Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1847f22f52eb066115b5681e745e74ec74f77f7d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662850"
---
# <a name="get-insights-data"></a>Obtener datos de insights

Use este método en la API de análisis de Microsoft Store para obtener información relacionada con las adquisiciones, mantenimiento y métricas de uso para una aplicación durante un intervalo de fechas determinado y los demás filtros opcionales. Esta información también está disponible en el [informe Insights](../publish/insights-report.md) en el centro de partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [Store ID](in-app-purchases-and-trials.md#store-ids) de la aplicación para el que desea recuperar datos de insights. Si no especifica este parámetro, el cuerpo de respuesta contendrá datos de información para todas las aplicaciones registradas en su cuenta.  |  No  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de datos de insights para recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de datos de insights para recuperar. El valor predeterminado es la fecha actual. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *Filtro = tipo de datos eq 'adquisición'*. <p/><p/>Puede especificar los campos de filtro siguientes:<p/><ul><li><strong>Adquisición</strong></li><li><strong>Estado</strong></li><li><strong>Uso de</strong></li></ul> | No   |

### <a name="request-example"></a>Ejemplo de solicitud

El ejemplo siguiente muestra una solicitud de obtención de datos de insights. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Una matriz de objetos que contienen datos de información de la aplicación. Para obtener más información acerca de los datos de cada objeto, vea el [valores Insight](#insight-values) sección más adelante.                                                                                                                      |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.                 |


### <a name="insight-values"></a>Valores de información

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | El identificador de Store de la aplicación para el que va a recuperar datos de insights.     |
| insightDate                | string | La fecha en la que hemos identificado que el cambio en una métrica específica. Esta fecha representa el final de la semana en que se detectó un aumento significativo o disminuir en una métrica en comparación con la semana anterior a éste. |
| Tipo de datos     | string | Una de las siguientes cadenas que especifica el área de análisis general que describe esta información:<p/><ul><li><strong>Adquisición</strong></li><li><strong>Estado</strong></li><li><strong>Uso de</strong></li></ul>   |
| insightDetail          | array | Uno o varios [InsightDetail valores](#insightdetail-values) que representan los detalles para obtener información actual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Uno de los siguientes valores que indica la métrica que describe la información actual o la dimensión actual, según la **dataType** valor.<ul><li>Para **mantenimiento**, este valor es siempre **HitCount**.</li><li>Para **adquisición**, este valor es siempre **cantidad de adquisición**.</li><li>Para **uso**, este valor puede ser una de las siguientes cadenas:<ul><li><strong>dailyActiveUsers</strong></li><li><strong>engagementDurationMinutes</strong></li><li><strong>dailyActiveDevices</strong></li><li><strong>dailyNewUsers</strong></li><li><strong>dailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | array |  Uno o más objetos que describen una sola métrica para la perspectiva.   |
| CambioPorcentual            | string |  El porcentaje que ha cambiado la métrica a través de la base de clientes todo.  |
| DimensionName           | string |  El nombre de la métrica que se describe en la dimensión actual. Algunos ejemplos son **EventType**, **mercado**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **Grupo de edad** y **sexo**.   |
| DimensionValue              | string | El valor de la métrica que se describe en la dimensión actual. Por ejemplo, si **DimensionName** es **EventType**, **DimensionValue** podría ser **bloqueo** o **bloqueo** .   |
| FactValue     | string | El valor absoluto de la métrica en la fecha en que se ha detectado la recomendación.  |
| Dirección | string |  La dirección del cambio (**positivo** o **negativo**).   |
| Fecha              | string |  La fecha en la que hemos identificado que el cambio relacionado con la información actual o la dimensión actual.   |

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

* [Informe de perspectivas](../publish/insights-report.md)
* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
