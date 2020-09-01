---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de multijugador de Xbox Live.
title: Obtener datos de multijugador de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, análisis de Xbox Live, varios jugadores
ms.localizationpriority: medium
ms.openlocfilehash: 546151e1cf95adc8ecbb64513a66671067869143
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171389"
---
# <a name="get-xbox-live-multiplayer-data"></a>Obtener datos de multijugador de Xbox Live


Use este método en la API de Microsoft Store Analytics para obtener datos de varios jugadores para el [juego habilitado para Xbox Live](/gaming/xbox-live/index.md) diaria o mensualmente. Esta información también está disponible en el [Informe de análisis de Xbox](../publish/xbox-analytics-report.md) del centro de Partners.

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
| applicationId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que desea recuperar los datos de varios jugadores de Xbox Live.  |  Sí  |
| metricType | string | Cadena que especifica el tipo de datos de análisis de Xbox Live que se va a recuperar. Para este método, especifique el valor **multiplayerdaily** para obtener los datos de varios jugadores diarios o **multiplayermonthly** para obtener los datos de varios jugadores mensuales.  |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de los datos de varios jugadores que se van a recuperar. En el caso de **multiplayerdaily**, el valor predeterminado es 3 meses antes de la fecha actual. En el caso de **multiplayermonthly**, el valor predeterminado es 1 año antes de la fecha actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de varios jugadores que se van a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>datamarket</strong></li><li><strong>subscriptionName</strong></li></ul> | No   |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>datamarket</strong></li><li><strong>subscriptionName</strong></li></ul><p/>Si especifica uno o más campos *GroupBy* , cualquier otro campo *GroupBy* que no especifique tendrá **el valor en** el cuerpo de la respuesta. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos de varios jugadores para el juego habilitado para Xbox Live. Reemplace el valor de *ApplicationID* por el identificador de almacén del juego.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=multiplayerdaily&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contiene datos de varios jugadores, donde cada objeto representa un conjunto de datos para el período de tiempo diario o mensual especificado y organizado por el **filtro** y los valores **GroupBy** especificados. Para obtener más información sobre los datos de cada objeto, consulte las secciones de análisis multijugador [diario](#daily-multiplayer-analytics) y de análisis multijugador [mensual](#monthly-multiplayer-analytics) .     |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 10000, pero hay más de 10000 filas de datos para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta. |


### <a name="daily-multiplayer-analytics"></a>Análisis multijugador diario

Los elementos de la matriz de *valores* contienen los valores siguientes cuando se solicitan datos de análisis multijugador diarios (es decir, cuando se especifica **multiplayerdaily** para el parámetro **metricType** ).

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| date                | string | La fecha de los datos de varios jugadores. |
| applicationId       | string | El identificador de almacén del juego para el que se recuperan los datos de varios jugadores.     |
| applicationName       | string |  Nombre del juego para el que se recuperan los datos de varios jugadores.     |
| market       | string | Código de país de dos letras ISO 3166 del mercado del que proceden los datos de varios jugadores.       |
| packageVersion     | string |  Versión del paquete de cuatro partes para el juego.  |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo del que proceden los datos de varios jugadores:<p/><ul><li><strong>Consola</strong></li><li><strong>PC</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | string |  El nombre de la suscripción que se usa para los datos de varios jugadores. Entre los valores posibles se incluyen la **fase de juego de Xbox** y **""** (para ninguna suscripción).  |
| dailySessionCount     | number |  El número de sesiones de varios jugadores para el juego en la fecha especificada.  |
| engagementDurationMinutes     | number |  El número total de minutos durante los que los clientes se encontraban con sesiones de varios jugadores para el juego en la fecha especificada.  |
| dailyActiveUsers     | number |  Número total de usuarios activos de varios jugadores para el juego en la fecha especificada.  |
| dailyActiveDevices     | number |  El número total de dispositivos activos que reproducen sesiones multijugador para el juego en la fecha especificada.  |
| dailyNewUsers     | number |  Número total de nuevos usuarios de varios jugadores del juego en la fecha especificada.  |
| monthlyActiveUsers     | number |  Número total de usuarios activos de varios jugadores para el mes en el que se produjo la fecha especificada.  |
| monthlyActiveDevices     | number | El número total de dispositivos activos que reproducen sesiones multijugador para el juego durante el mes en el que se produjo la fecha especificada.   |
| monthlyNewUsers     | number |  El número total de nuevos usuarios de varios jugadores del juego durante el mes en el que se produjo la fecha especificada.  |


### <a name="monthly-multiplayer-analytics"></a>Análisis multijugador mensual

Los elementos de la matriz de *valores* contienen los valores siguientes cuando se solicitan datos de análisis multijugador mensuales (es decir, cuando se especifica **multiplayermonthly** para el parámetro **metricType** ).

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| date                | string | La primera fecha del mes para los datos de varios jugadores. |
| applicationId       | string | El identificador de almacén del juego para el que se recuperan los datos de varios jugadores.     |
| applicationName       | string |  Nombre del juego para el que se recuperan los datos de varios jugadores.     |
| market       | string | Código de país de dos letras ISO 3166 del mercado del que proceden los datos de varios jugadores.       |
| packageVersion     | string |  Versión del paquete de cuatro partes para el juego.  |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo del que proceden los datos de varios jugadores:<p/><ul><li><strong>Consola</strong></li><li><strong>PC</strong></li><li>**Unknown**</li></ul>  |
| subscriptionName     | string |  El nombre de la suscripción que se usa para los datos de varios jugadores. Entre los valores posibles se incluyen la **fase de juego de Xbox** y **""** (para ninguna suscripción).  |
| monthlySessionCount     | number |  El número de sesiones de varios jugadores para el juego durante el mes especificado.   |
| engagementDurationMinutes     | number |  El número total de minutos durante los que los clientes se encontraban con sesiones multijugador para el juego durante el mes especificado.  |
| monthlyActiveUsers     | number | Número total de usuarios activos de varios jugadores durante el mes especificado.   |
| monthlyActiveDevices     | number | El número total de dispositivos activos que reproducen sesiones multijugador para el juego durante el mes especificado.   |
| monthlyNewUsers     | number |  El número total de nuevos usuarios de varios jugadores del juego durante el mes especificado.  |
| averageDailyActiveUsers     | number |  Número medio de usuarios de varios jugadores activos de forma diaria durante el mes especificado.  |
| averageDailyActiveDevices     | number |  El número promedio de dispositivos activos que reproducen sesiones multijugador para el juego durante el mes especificado.  |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra un cuerpo de respuesta JSON de ejemplo para la variante de métricas diarias de esta solicitud (es decir, cuando se especifica **multiplayerdaily** para el parámetro **metricType** ).

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

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)