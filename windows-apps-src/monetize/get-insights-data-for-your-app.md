---
author: mcleanbyron
description: Use este método en la API de análisis de Microsoft Store para obtener datos de perspectivas para su aplicación.
title: Obtener datos de perspectivas
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, servicios de almacenamiento, análisis de Microsoft Store API, perspectivas
ms.localizationpriority: medium
ms.openlocfilehash: 53fbd91437e5dc702f8672c6cbadeea32a8a96bf
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867344"
---
# <a name="get-insights-data"></a>Obtener datos de perspectivas

Utilice este método en la API de análisis de Microsoft Store para obtener datos de perspectivas relacionadas con adquisiciones, mantenimiento y métricas de uso de una aplicación durante un intervalo de fechas determinado y otros filtros opcionales. Esta información también está disponible en el [informe de conocimientos](../publish/insights-report.md) en el panel del centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

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

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId | cadena | El [Identificador de almacenamiento](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que desea recuperar datos de perspectivas. Si no especifica este parámetro, el cuerpo de la respuesta contendrá datos de perspectivas para todas las aplicaciones registradas a su cuenta.  |  No  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de perspectivas para recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos de perspectivas para recuperar. El valor siempre es la fecha actual. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *filtro = dataType eq 'adquisición'*. <p/><p/>Puede especificar los campos de filtro siguientes:<p/><ul><li><strong>adquisición</strong></li><li><strong>mantenimiento</strong></li><li><strong>uso</strong></li></ul> | No   |

### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos de perspectivas. Reemplaza el valor *applicationId* por el Id. de la Store de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Una matriz de objetos que contienen datos de perspectivas para la aplicación. Para obtener más información acerca de los datos de cada objeto, vea la sección [valores de Insight](#insight-values) más adelante.                                                                                                                      |
| TotalCount | entero    | El número total de filas en el resultado de datos de la consulta.                 |


### <a name="insight-values"></a>Valores de Insight

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | cadena | El identificador de la aplicación para la que va a recuperar datos de perspectivas de almacenamiento.     |
| insightDate                | cadena | La fecha en la que se identificaron el cambio en una métrica específica. Esta fecha representa el final de la semana en el que se detecta un aumento significativo o disminuir en una métrica en comparación con la semana antes de. |
| tipo de datos     | cadena | Una de las siguientes cadenas que especifica el área de análisis general que describe este entendimiento:<p/><ul><li><strong>adquisición</strong></li><li><strong>mantenimiento</strong></li><li><strong>uso</strong></li></ul>   |
| insightDetail          | matriz | Uno o más [valores de InsightDetail](#insightdetail-values) que representan los detalles de insight actual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| FactName           | cadena | Uno de los siguientes valores que indica la métrica que describa la dimensión actual o entendimiento actual, según el valor de **tipo de datos** .<ul><li>Para el **Mantenimiento**, este valor siempre es **recuento de visitas**.</li><li>Para la **adquisición**, este valor es siempre **AcquisitionQuantity**.</li><li>Para el **uso**, este valor puede ser una de las siguientes cadenas:<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | matriz |  Uno o más objetos que describen una métrica único para la perspectiva.   |
| CambioPorcentual            | cadena |  El porcentaje que ha cambiado la métrica a través de su base de clientes completa.  |
| DimensionName           | cadena |  El nombre de la métrica que se describen en la dimensión actual. Algunos ejemplos son **EventType**, **mercado**, **DeviceType**, **PackageVersion**, **AcquisitionType**, **AgeGroup** y **género**.   |
| DimensionValue              | cadena | El valor de la métrica que se describe en la dimensión actual. Por ejemplo, si **DimensionName** es **EventType**, **DimensionValue** podría ser **intensivo** o se **bloquee**.   |
| FactValue     | cadena | El valor absoluto de la métrica en la fecha en que se detectó el entendimiento.  |
| Direction | cadena |  La dirección del cambio (**positivos** o **negativos**).   |
| Date              | cadena |  La fecha en la que se identificaron el cambio relacionado con el entendimiento actual o la dimensión actual.   |

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

* [Informe de conocimientos](../publish/insights-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
