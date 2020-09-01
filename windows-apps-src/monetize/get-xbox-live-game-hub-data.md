---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de la central de juegos de Xbox Live.
title: Obtener datos del concentrador de juegos de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, análisis de Xbox Live, centros de juegos
ms.localizationpriority: medium
ms.openlocfilehash: aa714eafbc8357a66128045cf3b5f2effc9f067a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155559"
---
# <a name="get-xbox-live-game-hub-data"></a>Obtener datos del hub de juegos de Xbox Live


Use este método en la API de Microsoft Store Analytics para obtener datos del centro de juegos para el [juego habilitado para Xbox Live](/gaming/xbox-live/index.md). Esta información también está disponible en el [Informe de análisis de Xbox](../publish/xbox-analytics-report.md) del centro de Partners.

> [!IMPORTANT]
> Este método solo admite juegos para Xbox o juegos que usan los servicios de Xbox Live. Estos juegos deben pasar por el [proceso de aprobación del concepto](../gaming/concept-approval.md), que incluye juegos publicados por [asociados de Microsoft](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) y juegos enviados a través del [ ID@Xbox programa](/gaming/xbox-live/developer-program-overview.md#id). Este método no admite actualmente juegos publicados a través del [programa de creadores de Xbox Live](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que desea recuperar los datos de la central de juegos de Xbox Live.  |  Sí  |
| metricType | string | Cadena que especifica el tipo de datos de análisis de Xbox Live que se va a recuperar. Para este método, especifique el valor **communitymanagergamehub**.  |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de los datos del centro de juegos que se va a recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos del centro de juegos que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos del centro de juegos para el juego habilitado para Xbox Live. Reemplace el valor de *ApplicationID* por el identificador de almacén del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagergamehub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contiene los datos del centro de juegos para cada fecha del intervalo de fechas especificado. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente.                                                                                                                      |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 10000, pero hay más de 10000 filas de datos para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.  |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Fecha de los datos del centro de juegos de este objeto. |
| applicationId       | string | El identificador de almacén del juego para el que se recuperan los datos del centro de juegos.     |
| gameHubLikeCount     | number |   El número de like agregados a la página del concentrador de juego en la fecha especificada.   |
| gameHubCommentCount          | number |  El número de comentarios agregados a la página del concentrador de juegos de la aplicación en la fecha especificada.  |
| gameHubShareCount           | number | El número de veces que los clientes han compartido la página del concentrador de juegos de la aplicación en la fecha especificada.   |
| gameHubFollowerCount          | number | El número de seguidores de tiempo para la página del centro de juegos de la aplicación.   |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2018-01-04",
      "applicationId": "9NBLGGGZ5QDR",
      "gameHubLikeCount": 10,
      "gameHubCommentCount": 1,
      "gameHubShareCount": 0,
      "gameHubFollowerCount": 15924
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "gameHubLikeCount": 12,
      "gameHubCommentCount": 1,
      "gameHubShareCount": 0,
      "gameHubFollowerCount": 15931
    }
  ],
  "@nextLink": null,
  "TotalCount": 26
}
```

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)