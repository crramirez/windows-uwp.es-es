---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de embudo de adquisición para una aplicación durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener datos de embudo de adquisiciones de aplicaciones
ms.date: 08/04/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, adquisición, embudo
ms.localizationpriority: medium
ms.openlocfilehash: 817ad85bae642d6b09cf77bb5e7b4bf71b08f594
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493560"
---
# <a name="get-app-acquisition-funnel-data"></a>Obtener datos de embudo de adquisiciones de aplicaciones

Use este método en la API de Microsoft Store Analytics para obtener datos de embudo de adquisición para una aplicación durante un intervalo de fechas determinado y otros filtros opcionales. Esta información también está disponible en el [Informe de adquisiciones](../publish/acquisitions-report.md#acquisition-funnel) del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de embudo de adquisición. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de datos de embudo de adquisición que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de datos de embudo de adquisición que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |

 
### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**.

Se admiten los siguientes campos de filtro. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples.

| Campos        |  Descripción        |
|---------------|-----------------|
| campaignId | La cadena de identificador de una [campaña de promoción de aplicaciones personalizada](../publish/create-a-custom-app-promotion-campaign.md) que está asociada a la adquisición. |
| market | Es una cadena que contiene el código de país ISO 3166 del mercado donde se realizó la compra. |
| deviceType | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo la adquisición:<ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| ageGroup | Una de las siguientes cadenas que especifica el grupo de edad del usuario que completó la adquisición:<ul><li><strong>de 0 a 17</strong></li><li><strong>de 18 a 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 o superior</strong></li><li><strong>Unknown</strong></li></ul> |
| gender | Una de las siguientes cadenas que especifica el sexo del usuario que completó la adquisición:<ul><li><strong>M</strong></li><li><strong>Formato</strong></li><li><strong>Unknown</strong></li></ul> |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestran varias solicitudes para obtener datos de embudo de adquisición para una aplicación. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen los datos de embudo de adquisición de la aplicación. Para obtener más información acerca de los datos de cada objeto, consulte la sección [valores de embudo](#funnel-values) más abajo.                  |
| TotalCount | int    | Número total de objetos de la matriz de *valores* .        |


### <a name="funnel-values"></a>Valores de embudo

Los objetos de la matriz de *valores* contienen los valores siguientes.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | Una de las siguientes cadenas que especifica el [tipo de datos de embudo](../publish/acquisitions-report.md#acquisition-funnel) que se incluye en este objeto:<ul><li><strong>PageView</strong></li><li><strong>Adquisición</strong></li><li><strong>Instalación</strong></li><li><strong>Uso</strong></li></ul> |
| UserCount       | string | El número de usuarios que realizaron el paso de embudo especificado por el valor de *MetricType* .             |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe Adquisiciones](../publish/acquisitions-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
