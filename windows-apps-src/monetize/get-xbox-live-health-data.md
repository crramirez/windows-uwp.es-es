---
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de estado de Xbox Live.
title: Obtener datos de estado de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store, Xbox Live analytics, análisis de Xbox Live, health, estado, client errors, errores de clientes
ms.localizationpriority: medium
ms.openlocfilehash: 3b996d85776cb49d45cc5b699709b4eb107e7086
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7848729"
---
# <a name="get-xbox-live-health-data"></a>Obtener datos de estado de Xbox Live


Usa este método en la API de análisis de Microsoft Store para obtener datos de estado de tu [juego habilitado para Xbox Live](../xbox-live/index.md). Esta información también está disponible en el [informe de análisis de Xbox](../publish/xbox-analytics-report.md) en el centro de partners.

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
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que quieres recuperar los datos de estado de Xbox Live.  |  Sí  |
| metricType | cadena | Una cadena que especifica el tipo de datos de análisis de Xbox Live que recuperar. En este método, especifica el valor **callingpattern**.  |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de estado que se han de recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos de estado que se han de recuperar. El valor siempre es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | No   |
| groupby | cadena | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>fecha</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>Si especificas uno o más campos *groupby*, cualquier otro campo *groupby* que no especifiques tendrá el valor **All** en el cuerpo de respuesta. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos de estado para tu juego habilitado para Xbox Live. Sustituye el valor *applicationId* por el Id de Store del juego.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Matriz de objetos que contienen los datos de estado. Para más información sobre los datos de cada objeto, consulta la tabla siguiente.                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de datos de la consulta. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.   |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | cadena | El Id. de Store del juego para la que estás recuperando los datos de estado.     |
| fecha                | cadena | La primera fecha del intervalo de fechas de los datos de estado. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, mes u otro intervalo de fechas, este valor será la primera fecha de dicho intervalo de fechas. |
| deviceType          | cadena | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que utilizó el juego:<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong> (este valor indica un PC)</li><li><strong>Unknown</strong></li></ul>  |
| sandboxId     | cadena |   El id. de espacio aislado creado para el juego. Este puede ser el valor RETAIL o el Id. de un espacio aislado privado.   |
| packageVersion     | cadena |  La versión del paquete de cuatro partes del juego.  |
| callingPattern     | objeto |  Un objeto [callingPattern](#callingpattern) que proporciona las respuestas de servicio, dispositivos y datos de usuario para cada código de estado devuelto por cada servicio de Xbox Live usado por el juego durante el intervalo de fechas especificado.     |


### <a name="callingpattern"></a>callingPattern

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| servicio      | cadena  |   El nombre del servicio de Xbox Live relacionado con los datos de estado.       |
| endpoint      | cadena  |   El punto de conexión del servicio de Xbox Live relacionado con los datos de estado.        |
| httpStatusCode      | cadena  |  El código de estado HTTP de este conjunto de datos de estado.<p/><p/>**Nota**&nbsp;&nbsp;El código de estado **429E** indica que se realizó correctamente la llamada al servicio solo porque la [limitación de velocidad detallada](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md) estaba exenta durante la llamada. La velocidad detallada limitada podría aplicarse en el futuro si el gran volumen de experiencias de servicio, y en ese caso la llamada daría como resultado un [código de estado HTTP 429](../xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object).         |
| serviceResponses      | número  | El número de respuestas de servicio que devolvieron el código de estado especificado.         |
| uniqueDevices      | número  |  El número de dispositivos únicos que llamaron al servicio y recibieron el código de estado especificado.       |
| uniqueUsers      | número  |   El número de usuarios únicos que recibieron el código de estado especificado.       |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud. Los nombres de servicio y puntos de conexión mostrados en este ejemplo no son reales y se usan solo con fines de ejemplo.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "date": "2018-01-30",
      "deviceType": "All",
      "sandboxId": "RETAIL",
      "packageVersion": "Unknown",
      "callingpattern": [
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchReadHandler.POST",
          "httpStatusCode": "200",
          "serviceResponses": 160891,
          "uniqueDevices": 410,
          "uniqueUsers": 410
        },
        {
          "service": "userstats",
          "endpoint": "UserStats.BatchStatReadHandler.GET",
          "httpStatusCode": "200",
          "serviceResponses": 422,
          "uniqueDevices": 0,
          "uniqueUsers": 30
        }
      ],
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
