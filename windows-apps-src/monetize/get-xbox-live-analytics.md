---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de análisis de Xbox Live.
title: Obtener datos de análisis de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, análisis de Xbox Live
ms.localizationpriority: medium
ms.openlocfilehash: 5649a81e1c6e869e9e010841cb31432350bb33d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172889"
---
# <a name="get-xbox-live-analytics-data"></a>Obtener datos de análisis de Xbox Live

Use este método en la API de Microsoft Store Analytics para obtener los últimos 30 días de datos de análisis general para los clientes que juegan su [juego habilitado para Xbox Live](/gaming/xbox-live/index.md), incluido el uso de accesorios de dispositivo, el tipo de conexión a Internet, la distribución de Gamerscore, las estadísticas de juegos y los datos de amigos y seguidores. Esta información también está disponible en el [Informe de análisis de Xbox](../publish/xbox-analytics-report.md) del centro de Partners.

> [!IMPORTANT]
> Este método solo admite juegos para Xbox o juegos que usan los servicios de Xbox Live. Estos juegos deben pasar por el [proceso de aprobación del concepto](../gaming/concept-approval.md), que incluye juegos publicados por [asociados de Microsoft](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) y juegos enviados a través del [ ID@Xbox programa](/gaming/xbox-live/developer-program-overview.md#id). Este método no admite actualmente juegos publicados a través del [programa de creadores de Xbox Live](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

Los datos de análisis adicionales de los juegos habilitados para Xbox Live están disponibles a través de los métodos siguientes:
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtener datos de uso simultáneo de Xbox Live](get-xbox-live-concurrent-usage-data.md)

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
| applicationId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que desea recuperar los datos generales de análisis de Xbox Live.  |  Sí  |
| metricType | string | Cadena que especifica el tipo de datos de análisis de Xbox Live que se va a recuperar. Para este método, especifique el valor **productvalues**.  |  Sí  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos de análisis general para los clientes que juegan su juego habilitado para Xbox Live. Reemplace el valor de *ApplicationID* por el identificador de almacén del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

Este método devuelve una matriz de *valores* que contiene los siguientes objetos.

| Object      | Descripción                  |
|-------------|---------------------------------------------------|
| ProductData   |   Contiene un objeto [DeviceProperties](#deviceproperties) y un objeto [UserProperties](#userproperties) que contienen los últimos 30 días de datos de análisis de dispositivo y usuario para el juego.    |  
| XboxwideData   |  Contiene un objeto [DeviceProperties](#deviceproperties) y un objeto [UserProperties](#userproperties) que contienen los últimos 30 días de datos de análisis de dispositivo y usuario promedio para todos los clientes de Xbox Live, como porcentajes. Estos datos se incluyen con fines de comparación con los datos del juego.   |                                           


### <a name="deviceproperties"></a>DeviceProperties

Este recurso contiene los datos de uso de los dispositivos para los datos de uso de dispositivos del juego o promedio para todos los clientes de Xbox Live durante los últimos 30 días.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
|  applicationId               |    string     |  El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que recuperó los datos de análisis.   |
|  connectionTypeDistribution               |    array     |   Contiene objetos que indican cuántos clientes usan una conexión a Internet cableada en lugar de una conexión inalámbrica a Internet en Xbox. Cada objeto tiene dos campos de cadena: <ul><li>**contype**: especifica el tipo de conexión.</li><li>**deviceCount**: en el objeto **ProductData** , este campo especifica el número de clientes del juego que usan el tipo de conexión. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que usan el tipo de conexión.</li></ul>   |     
|  deviceCount               |   string      |  En el objeto **ProductData** , este campo especifica el número de dispositivos de cliente en los que se ha jugado el juego durante los últimos 30 días. En el objeto **XboxwideData** , este campo es siempre 1, lo que indica un porcentaje inicial del 100% para los datos de todos los clientes de Xbox Live.   |     
|  eliteControllerPresentDeviceCount               |   string      |  En el objeto **ProductData** , este campo especifica el número de clientes del juego que usan el controlador inalámbrico de Xbox Elite. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que usan el controlador inalámbrico de Xbox Elite.  |     
|  externalDrivePresentDeviceCount               |   string      |  En el objeto **ProductData** , este campo especifica el número de clientes del juego que usan una unidad de disco duro externa en Xbox. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que usan una unidad de disco duro externa en Xbox.  |


### <a name="userproperties"></a>UserProperties

Este recurso contiene los datos de usuario del juego o el promedio de datos de usuario de todos los clientes de Xbox Live durante los últimos 30 días.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
|  applicationId               |    string     |   El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que recuperó los datos de análisis.  |
|  userCount               |    string     |   En el objeto **ProductData** , este campo especifica el número de clientes que han jugado el juego durante los últimos 30 días. En el objeto **XboxwideData** , este campo es siempre 1, lo que indica un porcentaje inicial del 100% para los datos de todos los clientes de Xbox Live.   |     
|  dvrUsageCounts               |   array      |  Contiene objetos que indican cuántos clientes han usado Game DVR para grabar y ver el juego. Cada objeto tiene dos campos de cadena: <ul><li>**dvrName**: especifica la característica de DVR utilizada. Los valores posibles son **gameClipUploads**, **gameClipViews**, **screenshotUploads**y **screenshotViews**.</li><li>**userCount**: en el objeto **ProductData** , este campo especifica el número de clientes del juego que USARon la característica DVR de juego especificada. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que usaron la característica de DVR de juego especificada.</li></ul>   |     
|  followerCountPercentiles               |   array      |  Contiene objetos que proporcionan detalles sobre el número de seguidores para los clientes. Cada objeto tiene dos campos de cadena: <ul><li>**porcentaje**: Actualmente, este valor siempre es 50, lo que indica que los datos del seguimiento se proporcionan como un valor medio.</li><li>**valor**: en el objeto **ProductData** , este campo especifica el número medio de seguidores para los clientes del juego. En el objeto **XboxwideData** , este campo especifica el número medio de seguidores para todos los clientes de Xbox Live.</li></ul>  |   
|  friendCountPercentiles               |   array      |  Contiene objetos que proporcionan detalles sobre el número de amigos para los clientes. Cada objeto tiene dos campos de cadena: <ul><li>**porcentaje**: Actualmente, este valor siempre es 50, lo que indica que los datos de los amigos se proporcionan como un valor medio.</li><li>**valor**: en el objeto **ProductData** , este campo especifica el número medio de amigos para los clientes del juego. En el objeto **XboxwideData** , este campo especifica el número medio de amigos para todos los clientes de Xbox Live.</li></ul>  |     
|  gamerScoreRangeDistribution               |   array      |  Contiene objetos que proporcionan detalles sobre la distribución de Gamerscore para los clientes. Cada objeto tiene dos campos de cadena: <ul><li>**scoreRange**: el intervalo de Gamerscore para el que el siguiente campo proporciona datos de uso. Por ejemplo, **10k-25.000**.</li><li>**userCount**: en el objeto **ProductData** , este campo especifica el número de clientes del juego que tienen un Gamerscore en el intervalo especificado para todos los juegos que han jugado. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que tienen un Gamerscore en el intervalo especificado para todos los juegos que han jugado.</li></ul>  |
|  titleGamerScoreRangeDistribution               |   array      |  Contiene objetos que proporcionan detalles sobre la distribución de Gamerscore para el juego. Cada objeto tiene dos campos de cadena: <ul><li>**scoreRange**: el intervalo de Gamerscore para el que el siguiente campo proporciona datos de uso. Por ejemplo, **100-200**.</li><li>**userCount**: en el objeto **ProductData** , este campo especifica el número de clientes del juego que tienen un Gamerscore en el intervalo especificado para el juego. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que tienen un Gamerscore en el intervalo especificado para el juego.</li></ul>   |
|  socialUsageCounts               |   array      |  Contiene objetos que proporcionan detalles sobre el uso social de los clientes. Cada objeto tiene dos campos de cadena: <ul><li>**scName**: el tipo de uso social. Por ejemplo, **gameInvites** y **textMessages**.</li><li>**userCount**: en el objeto **ProductData** , este campo especifica el número de clientes del juego que han participado en el tipo de uso social especificado. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que han participado en el tipo de uso social especificado.</li></ul>   |
|  streamingUsageCounts               |   array      |  Contiene objetos que proporcionan detalles sobre el uso de streaming para los clientes. Cada objeto tiene dos campos de cadena: <ul><li>**stName**: el tipo de plataforma de streaming. Por ejemplo, **youtubeUsage**, **twitchUsage**y **mixerUsage**.</li><li>**userCount**: en el objeto **ProductData** , este campo especifica el número de clientes del juego que han usado la plataforma de streaming especificada. En el objeto **XboxwideData** , este campo especifica el porcentaje de todos los clientes de Xbox Live que han usado la plataforma de streaming especificada.</li></ul>  |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del concentrador de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtener datos de uso simultáneo de Xbox Live](get-xbox-live-concurrent-usage-data.md)