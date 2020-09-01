---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de información para su aplicación de escritorio.
title: Obtener información detallada de la aplicación de escritorio
ms.date: 07/31/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, API de análisis de Microsoft Store, información
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bd60425a5ec3c040417aded818c766db80eb59eb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172899"
---
# <a name="get-insights-data-for-your-desktop-application"></a>Obtener información detallada de la aplicación de escritorio

Use este método en la API de Microsoft Store Analytics para obtener datos de información relacionada con las métricas de estado de una aplicación de escritorio que ha agregado al [programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Estos datos también están disponibles en el [Informe de mantenimiento](/windows/desktop/appxpkg/windows-desktop-application-program#health-report) de las aplicaciones de escritorio del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El ID. del producto de la aplicación de escritorio para la que desea obtener datos de información. Para obtener el identificador de producto de una aplicación de escritorio, abra el [Informe de análisis de la aplicación de escritorio en el centro de Partners](/windows/desktop/appxpkg/windows-desktop-application-program) (como el **Informe de mantenimiento**) y recupere el identificador de producto de la dirección URL. Si no especifica este parámetro, el cuerpo de la respuesta contendrá datos de información para todas las aplicaciones registradas en su cuenta.  |  No  |
| startDate | date | Fecha de inicio del intervalo de fechas de datos de información que se va a recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de datos de información que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *Filter = DataType EQ ' Acquisition '*. <p/><p/>Actualmente, este método solo admite el **Estado**del filtro.  | No   |

### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos de información. Reemplace el valor de *ApplicationID* por el valor adecuado para su aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/insights?applicationId=10238467886765136388&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de información de la aplicación. Para obtener más información acerca de los datos de cada objeto, consulte la sección [valores](#insight-values) de la información más adelante.                                                                                                                      |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.                 |


### <a name="insight-values"></a>Valores de Insight

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | El ID. del producto de la aplicación de escritorio para la que ha recuperado los datos de información.     |
| insightDate                | string | Fecha en la que se identificó el cambio en una métrica específica. Esta fecha representa el final de la semana en el que se ha detectado un aumento o una disminución significativos en una métrica en comparación con la semana anterior. |
| dataType     | string | Una cadena que especifica el área de análisis general A la que informa esta información. Actualmente, este método solo admite el **Estado**.    |
| insightDetail          | array | Uno o más [valores de InsightDetail](#insightdetail-values) que representan los detalles de la información actual.    |


### <a name="insightdetail-values"></a>Valores de InsightDetail

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| FactName           | string | Cadena que indica la métrica que describe la actual información o la dimensión actual. Actualmente, este método solo admite el valor **HitCount**.  |
| Subdimensións         | array |  Uno o más objetos que describen una sola métrica para la información.   |
| PercentChange            | string |  Porcentaje de cambio de la métrica en toda la base de clientes.  |
| DimensionName           | string |  Nombre de la métrica descrita en la dimensión actual. Entre los ejemplos se incluyen **EventType**, **Market**, **DeviceType**y **PackageVersion**.   |
| DimensionValue              | string | El valor de la métrica que se describe en la dimensión actual. Por ejemplo, si **DimensionName** es **EventType**, **DimensionValue** podría **bloquearse** o **bloquearse**.   |
| FactValue     | string | Valor absoluto de la métrica en la fecha en que se detectó la información.  |
| Dirección | string |  Dirección del cambio (**positivo** o **negativo**).   |
| Date              | string |  Fecha en la que se identificó el cambio relacionado con la información actual o la dimensión actual.   |

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

* [Programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program)
* [Informe de estado](/windows/desktop/appxpkg/windows-desktop-application-program#health-report)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)