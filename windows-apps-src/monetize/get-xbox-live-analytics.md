---
author: mcleanbyron
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de análisis de Xbox Live.
title: Obtener datos de análisis de Xbox Live
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store, Xbox Live analytics, análisis de Xbox Live,
ms.localizationpriority: medium
ms.openlocfilehash: 4ba81f88b583a2d13785b3efe8d7008bac759a2d
ms.sourcegitcommit: 633dd07c3a9a4d1c2421b43c612774c760b4ee58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "1976573"
---
# <a name="get-xbox-live-analytics-data"></a>Obtener datos de análisis de Xbox Live

Usa este método en la API de análisis de Microsoft Store para obtener los últimos 30 días de datos de análisis general de los clientes al reproducir tu [juego habilitado para Xbox Live](../xbox-live/index.md), incluido el uso de datos del accesorio para dispositivo, tipo de conexión a Internet, distribución de la puntuación del jugador, estadísticas del juego y amigos y seguidores. Esta información también está disponible en el [informe de análisis de Xbox](../publish/xbox-analytics-report.md) del Panel del Centro de desarrollo de Windows.

> [!IMPORTANT]
> Este método solo admite juegos para Xbox o juegos que usan servicios de Xbox Live. Estos juegos debe pasar por el [proceso de aprobación de concepto](../gaming/concept-approval.md), que incluye juegos publicados por [partners de Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) y juegos enviados a través del [programa ID@Xbox](../xbox-live/developer-program-overview.md#id). Este método no admite actualmente juegos publicados mediante el [Programa de creadores de Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

Los datos de análisis adicionales para juegos habilitados para Xbox Live están disponibles a través de los siguientes métodos:
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtener datos de uso simultáneo de Xbox Live](get-xbox-live-concurrent-usage-data.md)

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
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que quieres recuperar los datos generales de análisis de Xbox Live.  |  Sí  |
| metricType | cadena | Una cadena que especifica el tipo de datos de análisis de Xbox Live que recuperar. En este método, especifica el valor **productvalues**.  |  Sí  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos de análisis generales para los clientes que juegan tu juego habilitado para Xbox Live. Sustituye el valor *applicationId* por el Id de Store del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Este método devuelve una matriz *Value* que contiene los siguientes objetos.

| Objeto      | Descripción                  |
|-------------|---------------------------------------------------|
| ProductData   |   Contiene un objeto [DeviceProperties](#deviceproperties) y un objeto [UserProperties](#userproperties) que contiene los últimos 30 días de datos de análisis de dispositivos y usuarios de tu juego.    |  
| XboxwideData   |  Contiene un objeto [DeviceProperties](#deviceproperties) y un objeto [UserProperties](#userproperties) que contiene los últimos 30 días de datos medios de análisis de dispositivos y usuarios todos los clientes Xbox Live, en porcentajes. Estos datos se incluyen para compararlos con los datos de tu juego.   |                                           


### <a name="deviceproperties"></a>DeviceProperties

Este recurso contiene datos de uso del dispositivo de tu juego o los datos de uso medio del dispositivo de todos los clientes de Xbox Live durante los últimos 30 días.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
|  applicationId               |    cadena     |  El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que has recuperado los datos de análisis.   |
|  connectionTypeDistribution               |    matriz     |   Contiene objetos que indican cuántos clientes usan una conexión a Internet con cable en comparación con una conexión a Internet inalámbrica en Xbox. Cada objeto tiene dos campos de cadena: <ul><li>**conType**: especifica el tipo de conexión.</li><li>**deviceCount**: en el objeto **ProductData**, este campo especifica el número de clientes de tu juego que usa el tipo de conexión. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que usan el tipo de conexión.</li></ul>   |     
|  deviceCount               |   cadena      |  En el objeto **ProductData**, este campo especifica el número de dispositivos de clientes en los que se ha jugado el juego durante los últimos 30 días. En el objeto **XboxwideData**, este campo es siempre 1 e indica un porcentaje inicial del 100% para los datos de todos los clientes de Xbox Live.   |     
|  eliteControllerPresentDeviceCount               |   cadena      |  En el objeto **ProductData**, este campo especifica el número de clientes de tu juego que usa el Mando Inalámbrico Xbox Elite. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que usan el Mando Inalámbrico Xbox Elite.  |     
|  externalDrivePresentDeviceCount               |   cadena      |  En el objeto **ProductData**, este campo especifica el número de clientes de tu juego que usa una unidad de disco externa en Xbox. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que usan una unidad de disco externa en Xbox.  |


### <a name="userproperties"></a>UserProperties

Este recurso contiene datos de usuario de tu juego o datos de usuario de todos los clientes de Xbox Live durante los últimos 30 días.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
|  applicationId               |    cadena     |   El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que has recuperado los datos de análisis.  |
|  userCount               |    cadena     |   En el objeto **ProductData**, este campo especifica el número de clientes que han jugado el juego durante los últimos 30 días. En el objeto **XboxwideData**, este campo es siempre 1 e indica un porcentaje inicial del 100% para los datos de todos los clientes de Xbox Live.   |     
|  dvrUsageCounts               |   matriz      |  Contiene objetos que indican cuántos clientes han usado DVR de juegos para grabar y ver el juego. Cada objeto tiene dos campos de cadena: <ul><li>**dvrName**: especifica la característica de DVR de juego usada. Los valores posibles son **gameClipUploads**, **gameClipViews**, **screenshotUploads** y **screenshotViews**.</li><li>**userCount**: en el objeto **ProductData**, este campo especifica el número de clientes de tu juego que usaron la función de DVR de juego especificada. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que usaron la función de DVR de juego especificada.</li></ul>   |     
|  followerCountPercentiles               |   matriz      |  Contiene objetos que proporcionan información sobre el número de seguidores de clientes. Cada objeto tiene dos campos de cadena: <ul><li>**porcentaje**: actualmente, este valor es siempre 50 e indica que los datos de seguidores se proporcionen como un valor medio.</li><li>**value**: en el objeto **ProductData**, este campo especifica el número medio de seguidores de clientes de tu juego. En el objeto **XboxwideData**, este campo especifica el número medio de seguidores de todos los clientes de Xbox Live.</li></ul>  |   
|  friendCountPercentiles               |   matriz      |  Contiene objetos que proporcionan información sobre el número de amigos de clientes. Cada objeto tiene dos campos de cadena: <ul><li>**porcentaje**: actualmente, este valor es siempre 50 e indica que los datos de amigos se proporcionen como un valor medio.</li><li>**value**: en el objeto **ProductData**, este campo especifica el número medio de amigos de clientes de tu juego. En el objeto **XboxwideData**, este campo especifica el número medio de amigos de todos los clientes de Xbox Live.</li></ul>  |     
|  gamerScoreRangeDistribution               |   matriz      |  Contiene objetos que proporcionan información sobre la distribución de puntuaciones de jugador de clientes. Cada objeto tiene dos campos de cadena: <ul><li>**scoreRange**: el intervalo de puntuación de jugador para el que se el siguiente campo proporciona datos de uso. Por ejemplo, **10K-25K**.</li><li>**userCount**: en el objeto **ProductData**, este campo especifica el número de clientes de tu juego que tienen una puntuación de jugador en el intervalo especificado para todos los juegos que han jugado. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que tienen una puntuación de jugador en el intervalo especificado para todos los juegos que han jugado.</li></ul>  |
|  titleGamerScoreRangeDistribution               |   matriz      |  Contiene objetos que proporcionan información sobre la distribución de puntuaciones de jugador de tu juego. Cada objeto tiene dos campos de cadena: <ul><li>**scoreRange**: el intervalo de puntuación de jugador para el que se el siguiente campo proporciona datos de uso. Por ejemplo, **100-200**.</li><li>**userCount**: en el objeto **ProductData**, este campo especifica el número de clientes de tu juego que tienen una puntuación de jugador en el intervalo especificado para tu juego. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que tienen una puntuación de jugador en el intervalo especificado para tu juego.</li></ul>   |
|  socialUsageCounts               |   matriz      |  Contiene objetos que proporcionan información sobre el uso social de clientes. Cada objeto tiene dos campos de cadena: <ul><li>**scName**: el tipo de uso social. Por ejemplo, **gameInvites** y **textMessages**.</li><li>**userCount**: en el objeto **ProductData**, este campo especifica el número de clientes de tu juego que han participado en el tipo de uso social especificado. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que han participado en el tipo de uso social especificado.</li></ul>   |
|  streamingUsageCounts               |   matriz      |  Contiene objetos que proporcionan información sobre el uso de streaming de clientes. Cada objeto tiene dos campos de cadena: <ul><li>**stName**: el tipo de plataforma de streaming. Por ejemplo, **youtubeUsage**, **twitchUsage** y **mixerUsage**.</li><li>**userCount**: en el objeto **ProductData**, este campo especifica el número de clientes de tu juego que han usado la plataforma de streaming. En el objeto **XboxwideData**, este campo especifica el porcentaje de todos los clientes de Xbox Live que han usado la plataforma de streaming especificada.</li></ul>  |


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

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
* [Obtener datos de uso simultáneo de Xbox Live](get-xbox-live-concurrent-usage-data.md)
