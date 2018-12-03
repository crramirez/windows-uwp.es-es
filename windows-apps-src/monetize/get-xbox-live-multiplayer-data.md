---
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de multijugador de Xbox Live.
title: Obtener datos de multijugador de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store, Xbox Live analytics, análisis de Xbox Live, multiplayer, multijugador
ms.localizationpriority: medium
ms.openlocfilehash: 74f1a64bde32fe68a51527527a0b049d811d0853
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8459271"
---
# <a name="get-xbox-live-multiplayer-data"></a>Obtener datos de multijugador de Xbox Live


Usa este método en la API de análisis de Microsoft Store para obtener datos de multijugador de tu [juego habilitado para Xbox Live](../xbox-live/index.md) diaria o mensualmente. Esta información también está disponible en el [informe de análisis de Xbox](../publish/xbox-analytics-report.md) en el centro de partners.

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
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para el que quieres recuperar los datos de multijugador de Xbox Live.  |  Sí  |
| metricType | cadena | Una cadena que especifica el tipo de datos de análisis de Xbox Live que se han de recuperar. En este método, especifica el valor **multiplayerdaily** para obtener datos de multijugador diarios o **multiplayermonthly** para obtener datos de multijugador mensuales.  |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de multijugador que se han de recuperar. Para **multiplayerdaily**, el valor predeterminado es 3 meses antes de la fecha actual. Para **multiplayermonthly**, el valor predeterminado es 1 año antes de la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos de multijugador que se han de recuperar. El valor siempre es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul> | No   |
| groupby | cadena | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>fecha</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>market</strong></li><li><strong>subscriptionName</strong></li></ul><p/>Si especificas uno o más campos *groupby*, cualquier otro campo *groupby* que no especifiques tendrá el valor **All** en el cuerpo de respuesta. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos de multijugador para tu juego habilitado para Xbox Live. Sustituye el valor *applicationId* por el Id. de Store del juego.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Una matriz de objetos que contiene datos de multijugador, donde cada objeto representa un conjunto de datos del periodo de tiempo diario o mensual especificado y organizado por los valores de **filter** y **groupby** especificados. Para obtener más información sobre los datos de cada objeto, consulta las secciones [Análisis diario de multijugador](#daily-multiplayer-analytics) y [Análisis mensual de multijugador](#monthly-multiplayer-analytics).     |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de datos de la consulta. |
| TotalCount | entero    | El número total de filas en el resultado de datos de la consulta. |


### <a name="daily-multiplayer-analytics"></a>Análisis diario de multijugador

Los elementos de la matriz *Value* contienen los siguientes valores cuando solicites los datos de análisis diario de multijugador (es decir, cuando especifiques **multiplayerdaily** para el parámetro **metricType**).

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| fecha                | cadena | La fecha de los datos de multijugador. |
| applicationId       | cadena | El Id de Store del juego sobre la que estás recuperando los datos de multijugador.     |
| applicationName       | cadena |  El nombre del juego sobre la que estás recuperando los datos de multijugador.     |
| market       | cadena | El código de país ISO 3166 de dos letras del mercado de donde proceden los datos de multijugador.       |
| packageVersion     | cadena |  La versión del paquete de cuatro partes del juego.  |
| deviceType          | cadena | Una de las siguientes cadenas que especifica el tipo de dispositivo del que proceden los datos de multijugador:<p/><ul><li><strong>Consola</strong></li><li><strong>PC</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | cadena |  El nombre de la suscripción usada para los datos de multijugador. Algunos valores incluyen **Xbox Game Pass** y **""** (para no suscripción).  |
| dailySessionCount     | número |  El número de sesiones multijugador del juego en la fecha especificada.  |
| engagementDurationMinutes     | número |  El número total de minutos que los clientes se conectaron en sesiones multijugador en el juego en la fecha especificada.  |
| dailyActiveUsers     | número |  El número total de usuarios multijugador activos del juego en la fecha especificada.  |
| dailyActiveDevices     | número |  El número total de dispositivos activos que participaron en sesiones multijugador del juego en la fecha especificada.  |
| dailyNewUsers     | número |  El número total de nuevos usuarios multijugador del juego en la fecha especificada.  |
| monthlyActiveUsers     | número |  El número total de usuarios multijugador activos durante el mes de la fecha especificada.  |
| monthlyActiveDevices     | número | El número total de dispositivos activos que participaron en sesiones multijugador del juego durante el mes de la fecha especificada.   |
| monthlyNewUsers     | número |  El número total de nuevos usuarios multijugador del juego durante el mes de la fecha especificada.  |


### <a name="monthly-multiplayer-analytics"></a>Análisis mensual de multijugador

Los elementos de la matriz *Value* contienen los siguientes valores cuando solicites los datos de análisis mensual de multijugador (es decir, cuando especifiques **multiplayermonthly** para el parámetro **metricType**).

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| fecha                | cadena | La primera fecha del mes de los datos de multijugador. |
| applicationId       | cadena | El Id de Store del juego sobre la que estás recuperando los datos de multijugador.     |
| applicationName       | cadena |  El nombre del juego sobre la que estás recuperando los datos de multijugador.     |
| market       | cadena | El código de país ISO 3166 de dos letras del mercado de donde proceden los datos de multijugador.       |
| packageVersion     | cadena |  La versión del paquete de cuatro partes del juego.  |
| deviceType          | cadena | Una de las siguientes cadenas que especifica el tipo de dispositivo del que proceden los datos de multijugador:<p/><ul><li><strong>Consola</strong></li><li><strong>PC</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | cadena |  El nombre de la suscripción usada para los datos de multijugador. Algunos valores incluyen **Xbox Game Pass** y **""** (para no suscripción).  |
| monthlySessionCount     | número |  El número de sesiones multijugador del juego durante el mes especificado.   |
| engagementDurationMinutes     | número |  El número total de minutos que los clientes se conectaron en sesiones multijugador en el juego durante el mes especificado.  |
| monthlyActiveUsers     | número | El número total de usuarios multijugador activos durante el mes especificado.   |
| monthlyActiveDevices     | número | El número total de dispositivos activos que participaron en sesiones multijugador del juego durante el mes especificado.   |
| monthlyNewUsers     | número |  El número total de nuevos usuarios multijugador del juego durante el mes especificado.  |
| averageDailyActiveUsers     | número |  El número medio de usuarios multijugador activos diarios del juego durante el mes especificado.  |
| averageDailyActiveDevices     | número |  El número medio de dispositivos activos que participaron en sesiones multijugador del juego durante el mes especificado.  |


### <a name="response-example"></a>Ejemplo de respuesta

El siguiente ejemplo muestra un ejemplo del cuerpo de respuesta JSON de la variante métrica diaria de esta solicitud (es decir, cuando especifiques **multiplayerdaily** para el parámetro **metricType**).

```json
{
  "Value": [
    {
      "date": "2018-01-07",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 976711,
      "engagementDurationMinutes": 16836064.5,
      "dailyActiveUsers": 180377,
      "dailyActiveDevices": 153359,
      "dailyNewUsers": 8638,
      "monthlyActiveUsers": 779984,
      "monthlyActiveDevices": 606495,
      "monthlyNewUsers": 212093
    },
    {
      "date": "2018-01-05",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Sports",
      "market": "All",
      "packageVersion": "",
      "deviceType": "All",
      "subscriptionName": "All",
      "dailySessionCount": 857433,
      "engagementDurationMinutes": 14087724.5,
      "dailyActiveUsers": 166630,
      "dailyActiveDevices": 143065,
      "dailyNewUsers": 9646,
      "monthlyActiveUsers": 764947,
      "monthlyActiveDevices": 595368,
      "monthlyNewUsers": 204248
    },
  ],
  "@nextLink": null,
  "TotalCount":2
}
```

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
