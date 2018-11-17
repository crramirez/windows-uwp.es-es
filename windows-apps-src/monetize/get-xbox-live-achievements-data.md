---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de logros de Xbox Live.
title: Obtener datos de logros de Xbox Live
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store, Xbox Live analytics, análisis de Xbox Live, achievements, logros
ms.localizationpriority: medium
ms.openlocfilehash: 6b635a659a8516184998b5f0b05d2d7692a42af1
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7162634"
---
# <a name="get-xbox-live-achievements-data"></a>Obtener datos de logros de Xbox Live

Usa este método en la API de análisis de Microsoft Store para obtener el número de clientes que han desbloqueado cada logros para tu [juego habilitado para Xbox Live](../xbox-live/index.md) durante el día más reciente para el que están disponibles los datos de logros, los 30 días anteriores hasta ese día y la duración total de tu juego hasta ese día. Esta información también está disponible en el [informe de análisis de Xbox](../publish/xbox-analytics-report.md) en el centro de partners.

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
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que quieres recuperar los datos de logros de Xbox Live.  |  Sí  |
| metricType | cadena | Una cadena que especifica el tipo de datos de análisis de Xbox Live que recuperar. En este método, especifica el valor **achievements**.  |  Sí  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener los datos de logros para clientes que han desbloqueado los primeros 10 logros para tu juego habilitada para Xbox Live. Sustituye el valor *applicationId* por el Id. de Store del juego.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Una matriz de objetos que contiene datos para cada logro del juego. Para más información sobre los datos de cada objeto, consulta la tabla siguiente.                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 100, pero resulta que hay más de 100 filas de datos de la consulta. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.  |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | cadena | El Id de Store del juego sobre la que estás recuperando los datos de logros.     |
| reportDateTime     | cadena |  La fecha de los datos de logros.    |
| achievementId          | número |  El id. del logro. |
| achievementName           | cadena | El nombre del logro.  |
| gamerscore           | número |  El premio de puntuación del jugador del logro.  |
| dailyUnlocks           | número |  El número de clientes que desbloquearon el logro el día especificado por *reportDateTime*.  |
| monthlyUnlocks              | número |  El número de clientes que desbloquearon el logro 30 días antes del día especificado por *reportDateTime*.   |
| totalUnlocks | número |  El número de clientes que han desbloqueado el logro durante el ciclo de vida del juego, hasta el día especificado por *reportDateTime*.   |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
