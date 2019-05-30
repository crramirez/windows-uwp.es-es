---
description: Utilice este método en la API de análisis de Microsoft Store para obtener datos de insights para su aplicación de escritorio.
title: Obtener datos de información de la aplicación de escritorio
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Store, API, información de análisis de Microsoft Store
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f6f4b2df1cda14bc1f363a1f9100e416f26489b
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372462"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obtener datos de información de la aplicación de escritorio

Use este método en la API de análisis de Microsoft Store para obtener información relacionada con las métricas de mantenimiento para una aplicación de escritorio que se ha agregado a la [programa de aplicación de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program). Estos datos también están disponibles en el [informe de mantenimiento](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report) para aplicaciones de escritorio en el centro de partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Header        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto de la aplicación de escritorio para el que desea obtener datos de insights. Para obtener el identificador de producto de una aplicación de escritorio, abra cualquier [análisis de informes para su aplicación de escritorio en el centro de partners](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (como el **informe de mantenimiento**) y recuperar el identificador de producto de la dirección URL. Si no especifica este parámetro, el cuerpo de respuesta contendrá datos de información para todas las aplicaciones registradas en su cuenta.  |  No  |
| startDate | date | La fecha de inicio del intervalo de fechas de datos de insights para recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de datos de insights para recuperar. El valor predeterminado es la fecha actual. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *Filtro = tipo de datos eq 'adquisición'* . <p/><p/>Actualmente, este método solo admite el filtro **estado**.  | No   |

### <a name="request-example"></a>Ejemplo de solicitud

El ejemplo siguiente muestra una solicitud de obtención de datos de insights. Reemplace el *applicationId* valor con el valor adecuado para su aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
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
| applicationId       | string | El identificador de producto de la aplicación de escritorio para el que se recuperaron datos de insights.     |
| insightDate                | string | La fecha en la que hemos identificado que el cambio en una métrica específica. Esta fecha representa el final de la semana en que se detectó un aumento significativo o disminuir en una métrica en comparación con la semana anterior a éste. |
| dataType     | string | Una cadena que especifica el área de análisis general que le informa de esta información. Actualmente, este método solo admite **estado**.    |
| insightDetail          | array | Uno o varios [InsightDetail valores](#insightdetail-values) que representan los detalles para obtener información actual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Una cadena que indica la métrica que describe la información actual o la dimensión actual. Actualmente, este método solo admite el valor **HitCount**.  |
| SubDimensions         | array |  Uno o más objetos que describen una sola métrica para la perspectiva.   |
| PercentChange            | string |  El porcentaje que ha cambiado la métrica a través de la base de clientes todo.  |
| DimensionName           | string |  El nombre de la métrica que se describe en la dimensión actual. Algunos ejemplos son **EventType**, **mercado**, **DeviceType**, y **PackageVersion**.   |
| DimensionValue              | string | El valor de la métrica que se describe en la dimensión actual. Por ejemplo, si **DimensionName** es **EventType**, **DimensionValue** podría ser **bloqueo** o **bloqueo** .   |
| FactValue     | string | El valor absoluto de la métrica en la fecha en que se ha detectado la recomendación.  |
| Direction | string |  La dirección del cambio (**positivo** o **negativo**).   |
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

* [Programa de aplicación de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)
* [Informe de mantenimiento](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
