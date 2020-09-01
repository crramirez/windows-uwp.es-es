---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de estado de Xbox Live.
title: Obtener datos de estado de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, análisis de Xbox Live, estado, errores de cliente
ms.localizationpriority: medium
ms.openlocfilehash: 3cf2432dfa9b4882f6fe878e3a1b832240735cb5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174999"
---
# <a name="get-xbox-live-health-data"></a>Obtener datos de estado de Xbox Live


Use este método en la API de Microsoft Store Analytics para obtener los datos de mantenimiento del [juego habilitado para Xbox Live](/gaming/xbox-live/index.md). Esta información también está disponible en el [Informe de análisis de Xbox](../publish/xbox-analytics-report.md) del centro de Partners.

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
| applicationId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que desea recuperar los datos de estado de Xbox Live.  |  Sí  |
| metricType | string | Cadena que especifica el tipo de datos de análisis de Xbox Live que se va a recuperar. Para este método, especifique el valor **callingpattern**.  |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de los datos de mantenimiento que se va a recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de mantenimiento que se van a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul> | No   |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>date</strong></li><li><strong>deviceType</strong></li><li><strong>packageVersion</strong></li><li><strong>sandboxId</strong></li></ul><p/>Si especifica uno o más campos *GroupBy* , cualquier otro campo *GroupBy* que no especifique tendrá **el valor en** el cuerpo de la respuesta. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener los datos de mantenimiento del juego habilitado para Xbox Live. Reemplace el valor de *ApplicationID* por el identificador de almacén del juego.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=callingpattern&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de estado. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente.                                                                                                                      |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 10000, pero hay más de 10000 filas de datos para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.   |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | string | El identificador de almacén del juego para el que se recuperan los datos de mantenimiento.     |
| date                | string | La primera fecha del intervalo de fechas de los datos de mantenimiento. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se usó el juego:<p/><ul><li><strong>XboxOne</strong></li><li><strong>WindowsOneCore</strong> (este valor indica un equipo)</li><li><strong>Unknown</strong></li></ul>  |
| sandboxId     | string |   El identificador de espacio aislado creado para el juego. Puede ser el valor comercial o el identificador de un espacio aislado privado.   |
| packageVersion     | string |  Versión del paquete de cuatro partes para el juego.  |
| callingPattern     | object |  Un objeto [callingPattern](#callingpattern) que proporciona respuestas de servicio, dispositivos y datos de usuario para cada código de estado devuelto por cada servicio de Xbox Live usado por el juego durante el intervalo de fechas especificado.     |


### <a name="callingpattern"></a>callingPattern

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| service      | string  |   El nombre del servicio Xbox Live con el que se relacionan los datos de mantenimiento.       |
| endpoint      | string  |   El punto de conexión del servicio Xbox Live con el que se relacionan los datos de mantenimiento.        |
| httpStatusCode      | string  |  Código de Estado HTTP para este conjunto de datos de mantenimiento.<p/><p/>**Nota:** &nbsp; &nbsp; El código de estado **429E** indica que la llamada de servicio se realizó correctamente solo debido a que la limitación de la [tasa específica](/gaming/xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md) estaba exenta durante la llamada. En el futuro se podría aplicar una tarifa limitada específica si el servicio experimenta un gran volumen y, en ese caso, la llamada daría como resultado un [código de Estado HTTP 429](/gaming/xbox-live/using-xbox-live/best-practices/fine-grained-rate-limiting.md#http-429-response-object).         |
| serviceResponses      | number  | El número de respuestas de servicio que devolvieron el código de estado especificado.         |
| uniqueDevices      | number  |  El número de dispositivos únicos que llamaron al servicio y recibieron el código de estado especificado.       |
| uniqueUsers      | number  |   Número de usuarios únicos que recibieron el código de estado especificado.       |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud. Los nombres de servicio y los puntos de conexión que se muestran en este ejemplo no son reales y se usan solo con fines de ejemplo.

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

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos del club de Xbox Live](get-xbox-live-club-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)