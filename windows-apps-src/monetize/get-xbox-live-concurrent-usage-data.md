---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de uso simultáneo de Xbox Live.
title: Obtener datos de uso simultáneo de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, análisis de Xbox Live, uso simultáneo
ms.localizationpriority: medium
ms.openlocfilehash: 9e8d36ce9336d5fc65a73a19233b8a1f0a05a8bf
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167589"
---
# <a name="get-xbox-live-concurrent-usage-data"></a>Obtener datos de uso simultáneo de Xbox Live


Use este método en la API de Microsoft Store Analytics para obtener datos de uso casi en tiempo real (con una latencia de 5-15 minutos) sobre el número promedio de clientes que juegan su [juego habilitado para Xbox Live](/gaming/xbox-live/index.md) cada minuto, hora o día durante un intervalo de tiempo especificado. Esta información también está disponible en el [Informe de análisis de Xbox](../publish/xbox-analytics-report.md) del centro de Partners.

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
| applicationId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que desea recuperar los datos de uso simultáneo de Xbox Live.  |  Sí  |
| metricType | string | Cadena que especifica el tipo de datos de análisis de Xbox Live que se va a recuperar. Para este método, especifique la **simultaneidad**del valor.  |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de datos de uso simultáneos que se va a recuperar. Vea la descripción de *aggregationLevel* para ver el comportamiento predeterminado. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de datos de uso simultáneos que se va a recuperar. Vea la descripción de *aggregationLevel* para ver el comportamiento predeterminado. |  No  |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **minute**, **hour**o **Day**. Si no se especifica, el valor predeterminado es **día**. <p/><p/>Si no especifica *startDate* ni *EndDate*, el cuerpo de respuesta tiene como valor predeterminado el siguiente: <ul><li>**minute**: los últimos 60 registros de datos disponibles.</li><li>**hour**: los últimos 24 registros de datos disponibles.</li><li>**Day**: los 7 últimos registros de datos disponibles.</li></ul><p/>Los siguientes niveles de agregación tienen límites de tamaño en el número de registros que se pueden devolver. Los registros se truncarán si el intervalo de tiempo solicitado es demasiado grande. <ul><li>**minuto**: hasta 1440 registros (24 horas de datos).</li><li>**hora**: hasta 720 registros (30 días de datos).</li><li>**Day**: hasta 60 registros (60 días de datos).</li></ul>  |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos de uso simultáneos para el juego habilitado para Xbox Live. Esta solicitud recupera los datos de cada minuto entre el 1 2018 de febrero y el 2 2018 de febrero. Reemplace el valor de *ApplicationID* por el identificador de almacén del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=concurrency&aggregationLevel=hour&startDate=2018-02-01&endData=2018-02-02 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

El cuerpo de la respuesta contiene una matriz de objetos que contienen un conjunto de datos de uso simultáneo para un minuto, hora o día especificados. Cada objeto contiene los valores siguientes.

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Count      | number  | El número promedio de clientes que juegan su Xbox Live habilitada para el minuto, hora o día especificados. <p/><p/>**Nota:** &nbsp; &nbsp; Un valor de 0 indica que no había usuarios simultáneos durante el intervalo especificado o que se produjo un error al recopilar datos de usuario simultáneos para el juego durante el intervalo especificado. |
| Date  | string | Fecha y hora que especifican el minuto, la hora o el día en que se produjeron los datos de uso simultáneos.  |
| SeriesName | string    | Siempre tiene el valor **UserConcurrency**. |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra un cuerpo de respuesta JSON de ejemplo para esta solicitud con la agregación de datos por minuto.

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

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del concentrador de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)