---
description: Use este método en la API de Microsoft Store Analytics para obtener los datos de los logros de Xbox Live.
title: Obtener datos de logros de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, análisis de Xbox Live, logros
ms.localizationpriority: medium
ms.openlocfilehash: 7d4c031d967c48c65f0f9be7b386c11c12b7366e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162379"
---
# <a name="get-xbox-live-achievements-data"></a>Obtener datos de logros de Xbox Live

Use este método en la API de Microsoft Store Analytics para obtener el número de clientes que han desbloqueado cada uno de los logros del [juego habilitado para Xbox Live](/gaming/xbox-live/index.md) durante el último día en que estén disponibles los datos de logros, los 30 días anteriores a ese día y la duración total del juego hasta ese día. Esta información también está disponible en el [Informe de análisis de Xbox](../publish/xbox-analytics-report.md) del centro de Partners.

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
| applicationId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que desea recuperar los datos de los logros de Xbox Live.  |  Sí  |
| metricType | string | Cadena que especifica el tipo de datos de análisis de Xbox Live que se va a recuperar. Para este método, especifique los **logros**del valor.  |  Sí  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener los datos de logros de los clientes que han desbloqueado los primeros 10 Logros del juego habilitado para Xbox Live. Reemplace el valor de *ApplicationID* por el identificador de almacén del juego.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos para cada consecución en el juego. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente.                                                                                                                      |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 100, pero hay más de 100 filas de datos para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.  |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | El identificador de almacén del juego para el que se recuperan los datos de los logros.     |
| reportDateTime     | string |  La fecha de los datos de logros.    |
| achievementId          | number |  IDENTIFICADOR del logro. |
| achievementName           | string | Nombre del logro.  |
| gamerscore           | number |  El premio Gamerscore para el logro.  |
| dailyUnlocks           | number |  El número de clientes que han desbloqueado el logro el día especificado por *reportDateTime*.  |
| monthlyUnlocks              | number |  El número de clientes que han desbloqueado el logro en los 30 días anteriores al día especificado por *reportDateTime*.   |
| totalUnlocks | number |  El número de clientes que han desbloqueado el logro durante la vigencia del juego, hasta el día especificado por *reportDateTime*.   |


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

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)