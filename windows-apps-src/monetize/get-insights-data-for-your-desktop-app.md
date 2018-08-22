---
author: mcleanbyron
description: Use este método en la API de análisis de Microsoft Store para obtener datos de perspectivas para su aplicación de escritorio.
title: Obtener datos de perspectivas para su aplicación de escritorio
ms.author: mcleans
ms.date: 07/31/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, servicios de almacenamiento, análisis de Microsoft Store API, perspectivas
ms.localizationpriority: medium
ms.openlocfilehash: e7ca6eed40af37276b5b4c98ec7b1b709bdadfb9
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2794456"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obtener datos de perspectivas para su aplicación de escritorio

Use este método en la API de análisis de Microsoft Store para obtener conocimientos datos relacionados con las métricas de mantenimiento para una aplicación de escritorio que haya agregado al [programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Estos datos también están disponibles en el [informe de mantenimiento](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) de aplicaciones de escritorio en el panel del centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

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

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId | cadena | El identificador de producto de la aplicación de escritorio para la que desea obtener datos de perspectivas. Para obtener el id. del producto de una aplicación de escritorio, abra cualquier [informe de análisis del Centro de desarrollo de tu aplicación de escritorio](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como el **Informe de estado**) y recuperara el id. del producto desde la dirección URL. Si no especifica este parámetro, el cuerpo de la respuesta contendrá datos de perspectivas para todas las aplicaciones registradas a su cuenta.  |  No  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de perspectivas para recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos de perspectivas para recuperar. El valor siempre es la fecha actual. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *filtro = dataType eq 'adquisición'*. <p/><p/>Actualmente este método sólo es compatible con el **estado**de filtro.  | No   |

### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos de perspectivas. Reemplace el valor de *applicationId* con el valor adecuado para su aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
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
| applicationId       | cadena | El identificador de producto de la aplicación de escritorio para que recuperan datos de perspectivas.     |
| insightDate                | cadena | La fecha en la que se identificaron el cambio en una métrica específica. Esta fecha representa el final de la semana en el que se detecta un aumento significativo o disminuir en una métrica en comparación con la semana antes de. |
| tipo de datos     | cadena | Una cadena que especifica el área de análisis general que informa este conocimiento. Actualmente, este método sólo es compatible con **Mantenimiento**.    |
| insightDetail          | matriz | Uno o más [valores de InsightDetail](#insightdetail-values) que representan los detalles de insight actual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| FactName           | cadena | Una cadena que indica la métrica que describa la dimensión actual o entendimiento actual. Actualmente, este método sólo es compatible con el valor de **recuento de visitas**.  |
| SubDimensions         | matriz |  Uno o más objetos que describen una métrica único para la perspectiva.   |
| CambioPorcentual            | cadena |  El porcentaje que ha cambiado la métrica a través de su base de clientes completa.  |
| DimensionName           | cadena |  El nombre de la métrica que se describen en la dimensión actual. Algunos ejemplos son **EventType**, **mercado**, **DeviceType**y **PackageVersion**.   |
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

* [Programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Informe de estado](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
