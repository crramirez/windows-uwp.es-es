---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de embudo de adquisiciones de una aplicación durante un intervalo de fechas especificado y según otros filtros opcionales.
title: Obtener datos de embudo de adquisiciones de aplicaciones
ms.author: mhopkins
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, servicios de Store, API de Servicios web de Windows, adquisición, embudo
ms.localizationpriority: medium
ms.openlocfilehash: 362bcc956fa5945f9685aac7d6351b9fda7690de
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4616086"
---
# <a name="get-app-acquisition-funnel-data"></a>Obtener datos de embudo de adquisiciones de aplicaciones

Usa este método en la API de análisis de Microsoft Store para obtener datos de embudo de adquisiciones de una aplicación durante un intervalo de fechas especificado y según otros filtros opcionales. Esta información también está disponible en el [informe de adquisiciones](../publish/acquisitions-report.md#acquisition-funnel) del panel del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | cadena | El [identificador de la Store](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que quieres recuperar los datos de embudo de adquisiciones. Un ejemplo de un Id. de la Store sería 9WZDNCRFJ3Q8. |  Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de embudo de adquisiciones que se recuperarán. El valor siempre es la fecha actual. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de los datos de embudo de adquisiciones que se recuperarán. El valor predeterminado es la fecha actual. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |

 
### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**.

Se admiten los siguientes campos de filtros. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples.

| Campos        |  Descripción        |
|---------------|-----------------|
| campaignId | La cadena de identificador para una [campaña de promoción de la aplicación personalizada](../publish/create-a-custom-app-promotion-campaign.md) que está asociada a la adquisición. |
| market | Es una cadena que contiene el código de país ISO 3166 del mercado donde se realizó la compra. |
| deviceType | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo la adquisición:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| ageGroup | Una de las cadenas siguientes que especifica el grupo de edad del usuario que ha completado la adquisición:<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 o más</strong></li><li><strong>Unknown</strong></li></ul> |
| gender | Una de las cadenas siguientes que especifica el sexo del usuario que ha completado la adquisición:<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>Unknown</strong></li></ul> |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener los datos de embudo de adquisiciones de aplicaciones. Sustituye el valor *applicationId* por el id. de la Store de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | array  | Una matriz de objetos que contienen los datos de embudo de adquisiciones de la aplicación. Para más información sobre los datos de cada objeto, consulta la sección [valores de embudo](#funnel-values) que encontrarás a continuación.                  |
| TotalCount | int    | El número total de objetos en la matriz *Value*.        |


### <a name="funnel-values"></a>Valores de embudo

Los objetos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | Una de las siguientes cadenas que especifica el [tipo de datos del embudo](../publish/acquisitions-report.md#acquisition-funnel) que se incluye en este objeto:<ul><li><strong>PageView</strong></li><li><strong>Adquisición</strong></li><li><strong>Install</strong></li><li><strong>Usage</strong></li></ul> |
| UserCount       | string | El número de usuarios que realizó el paso de embudo especificado por el valor *MetricType*.             |


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
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener adquisiciones de aplicaciones](get-app-acquisitions.md)
