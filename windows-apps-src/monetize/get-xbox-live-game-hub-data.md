---
author: mcleanbyron
description: Usa este método en la API de análisis de Microsoft Store para obtener datos del hub de juegos de Xbox Live.
title: Obtener datos del hub de juegos de Xbox Live
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store, Xbox Live analytics, análisis de Xbox Live, Game Hubs, hubs de juegos
ms.localizationpriority: medium
ms.openlocfilehash: 70a76f82c37a4cfba6e0a562c8a6295da38976db
ms.sourcegitcommit: 633dd07c3a9a4d1c2421b43c612774c760b4ee58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "1976200"
---
# <a name="get-xbox-live-game-hub-data"></a>Obtener datos del hub de juegos de Xbox Live


Usa este método en la API de análisis de Microsoft Store para obtener datos de hub de juegos de tu [juego habilitado para Xbox Live](../xbox-live/index.md). Esta información también está disponible en el [informe de análisis de Xbox](../publish/xbox-analytics-report.md) del Panel del Centro de desarrollo de Windows.

> [!IMPORTANT]
> Este método solo admite juegos para Xbox o juegos que usan servicios de Xbox Live. Estos juegos debe pasar por el [proceso de aprobación de concepto](../gaming/concept-approval.md), que incluye juegos publicados por [partners de Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) y juegos enviados a través del [programa ID@Xbox](../xbox-live/developer-program-overview.md#id). Este método no admite actualmente juegos publicados mediante el [Programa de creadores de Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que quieres recuperar los datos de hub de juegos de Xbox Live.  |  Sí  |
| metricType | cadena | Una cadena que especifica el tipo de datos de análisis de Xbox Live que recuperar. En este método, especifica el valor **communitymanagergamehub**.  |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos del hub de juegos que se van a recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos del hub de juegos que se van a recuperar. El valor siempre es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos del hub de juegos para tu juego habilitado para Xbox Live. Sustituye el valor *applicationId* por el Id de Store del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagergamehub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Una matriz de objetos que contienen datos del hub de juegos para cada fecha del intervalo de fechas especificado. Para más información sobre los datos de cada objeto, consulta la tabla siguiente.                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de datos de la consulta. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.  |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| fecha                | cadena | La fecha de los datos del hub de juegos de este objeto. |
| applicationId       | cadena | El Id de Store del juego sobre la que estás recuperando los datos del hub de juegos.     |
| gameHubLikeCount     | número |   El número de "Me gusta" agregados a la página del hub de juegos en la fecha especificada.   |
| gameHubCommentCount          | número |  El número de comentarios agregados a la página del hub de juegos en la fecha especificada.  |
| gameHubShareCount           | número | El número de veces que la página del hub de juegos de tu aplicación fue compartida por los clientes en la fecha especificada.   |
| gameHubFollowerCount          | number | El número de seguidores de siempre para la página del hub de juegos para la aplicación.   |


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

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)