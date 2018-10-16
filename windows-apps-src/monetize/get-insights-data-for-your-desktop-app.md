---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de información de la aplicación de escritorio.
title: Obtener datos de información de la aplicación de escritorio
ms.author: mhopkins
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, servicios de la tienda, API de información de análisis de Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 0b4390fba26922372a74de76d09844a7243bce73
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4689600"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obtener datos de información de la aplicación de escritorio

Usa este método en la API de análisis de Microsoft Store para obtener información datos relacionados con las métricas de mantenimiento para una aplicación de escritorio que agregaste al [programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Estos datos también están disponibles en el [informe de estado](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) para aplicaciones de escritorio en el panel del centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto de la aplicación de escritorio para la que quieres obtener datos de información. Para obtener el id. del producto de una aplicación de escritorio, abra cualquier [informe de análisis del Centro de desarrollo de tu aplicación de escritorio](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como el **Informe de estado**) y recuperara el id. del producto desde la dirección URL. Si no se especifica este parámetro, el cuerpo de la respuesta contendrá los datos de información de todas las aplicaciones registradas en tu cuenta.  |  No  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de información para recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de los datos de información para recuperar. El valor predeterminado es la fecha actual. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *filter = dataType eq 'adquisición'*. <p/><p/>Actualmente, este método solo admite el **estado**de filtro.  | No   |

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos de información. Reemplaza el valor de *applicationId* por el valor adecuado para la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
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
| applicationId       | string | El identificador de producto de la aplicación de escritorio para la que has recuperado los datos de información.     |
| insightDate                | string | La fecha en la que hemos identificado el cambio en una métrica específica. Esta fecha representa el final de la semana en el que hemos detectado un aumento significativo o reducir en una métrica en comparación con la semana anterior. |
| dataType     | string | Una cadena que especifica el área de análisis general que informa esta información. Actualmente, este método solo admite el **estado**.    |
| insightDetail          | array | Uno o más [valores de InsightDetail](#insightdetail-values) que representan los detalles de insight actual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Una cadena que indica la métrica que describe la información actual o la dimensión actual. Actualmente, este método solo admite el valor de **recuento de visitas**.  |
| SubDimensions         | array |  Uno o varios objetos que describen una métrica única para la perspectiva.   |
| CambioPorcentual            | string |  El porcentaje de la métrica ha cambiado a través de la base de clientes completa.  |
| DimensionName           | string |  El nombre de la métrica que se describe en la dimensión actual. Algunos ejemplos son **EventType**, **mercado**, **DeviceType**y **PackageVersion**.   |
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

* [Programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Informe Estado](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
