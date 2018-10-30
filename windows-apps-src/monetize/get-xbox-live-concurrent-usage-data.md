---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de uso simultáneo de Xbox Live.
title: Obtener datos de uso simultáneo de Xbox Live
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store, Xbox Live analytics, análisis de Xbox Live, concurrent usage, uso simultáneo
ms.localizationpriority: medium
ms.openlocfilehash: 3e982d7c5eb1ff8365d2aa527f75d181905784a0
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5764486"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Obtener datos de uso simultáneo de Xbox Live


Usa este método en la API de análisis de Microsoft Store para obtener datos de uso casi en tiempo real (con latencia de 5-15 minutos) sobre el número medio de clientes que juegan tu [juego habilitado para Xbox Live](../xbox-live/index.md) cada minuto, hora o día durante un intervalo de tiempo especificado. Esta información también está disponible en el [informe de análisis de Xbox](../publish/xbox-analytics-report.md) del Panel del Centro de desarrollo de Windows.

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
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que quieres recuperar los datos de uso simultáneo de Xbox Live.  |  Sí  |
| metricType | cadena | Una cadena que especifica el tipo de datos de análisis de Xbox Live que recuperar. En este método, especifica el valor **concurrency**.  |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de uso simultáneo que se han de recuperar. Consulta la descripción de *aggregationLevel* para el comportamiento predeterminado. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos de uso simultáneo que se han de recuperar. Consulta la descripción de *aggregationLevel* para el comportamiento predeterminado. |  No  |
| aggregationLevel | cadena | Especifica el intervalo de tiempo para el que se van a recuperar los datos agregados. Puede ser una de las siguientes cadenas: **minute**, **hour** o **day**. Si no se especifica, el valor predeterminado es **day**. <p/><p/>Si no especificas *startDate* o *endDate*, el cuerpo de respuesta asigna de forma predeterminada los siguientes: <ul><li>**minute**: los últimos 60 registros de datos disponibles.</li><li>**hour**: los últimos 24 registros de datos disponibles.</li><li>**day**: los últimos 7 registros de datos disponibles.</li></ul><p/>Los siguientes niveles de agregación tienen límites de tamaño en el número de registros que se pueden devolver. Si el intervalo de tiempo solicitado es demasiado grande, se truncarán los registros. <ul><li>**minute**: hasta 1440 registros (24 horas de datos).</li><li>**hour**: hasta 720 registros (30 días de datos).</li><li>**day**: hasta 60 registros (60 días de datos).</li></ul>  |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos de uso simultáneo para tu juego habilitado para Xbox Live. Esta solicitud recupera datos cada minuto entre el 1 de febrero de 2018 y el 2 de febrero de 2018. Sustituye el valor *applicationId* por el Id de Store del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

El cuerpo de respuesta contiene una matriz de objetos que contienen un conjunto de datos de uso simultáneo de un minuto, hora o día especificados. Cada objeto contiene los siguientes valores.

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Número      | número  | El promedio de clientes que reproducen tu juego habilitado para Xbox Live durante el minuto, hora o día especificados. <p/><p/>**Nota**&nbsp;&nbsp;Un valor de 0 indica que no hay usuarios simultáneos durante el intervalo especificado, o que se produjo un error durante la recopilación de datos de usuarios simultáneos para el juego durante el intervalo especificado. |
| Date  | cadena | La fecha y la hora que especifica el minuto, hora o día durante el cual se han producido los datos de uso simultáneo.  |
| SeriesName | cadena    | Este tiene siempre el valor **UserConcurrency**. |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud con adición de datos por minuto.

```json
[   {
        "Count": 418.0,
        "Date": "2018-02-02T04:42:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 418.0,
        "Date": "2018-02-02T04:43:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 415.0,
        "Date": "2018-02-02T04:44:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 412.0,
        "Date": "2018-02-02T04:45:13.65Z",
        "SeriesName": "UserConcurrency"
    }, {
        "Count": 414.0,
        "Date": "2018-02-02T04:46:13.65Z",
        "SeriesName": "UserConcurrency"
    }
]
```

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
