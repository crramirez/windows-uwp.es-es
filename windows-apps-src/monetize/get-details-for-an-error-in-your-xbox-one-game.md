---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener información detallada sobre un error específico de la Xbox One juego.
title: Obtener los detalles de un error en tu Xbox One juego
ms.author: mhopkins
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis de la Store Windows, errors, errores, details, detalles
ms.localizationpriority: medium
ms.openlocfilehash: 33733af7f323817bc82d49800c2dc17c5f7b9887
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6832897"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>Obtener los detalles de un error en tu Xbox One juego

Usa este método en la Microsoft Store analytics API para obtener información detallada sobre un error específico de la Xbox One juego que se ha integrado mediante el Portal de desarrollador de Xbox (XDP) y está disponible en el panel del centro de desarrollo de análisis de XDP. Este método solo puede recuperar detalles de errores que se hayan producido en los últimos 30 días.

Antes de que puedes usar este método, primero debes usar el método [get para tu juego de Xbox One los datos de informes de errores](get-error-reporting-data-for-your-xbox-one-game.md) para recuperar el identificador del error para el que quieres obtener información detallada.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el id. del error sobre el que quieres obtener información detallada. Para obtener este Id., usa el método [obtener datos informes de errores de la Xbox One en juegos](get-error-reporting-data-for-your-xbox-one-game.md) y usa el valor de **failureHash** en el cuerpo de respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto del juego de Xbox One para el que quieres recuperar los detalles de errores. Para obtener el id. del producto de tu juego, ve a tu juego en el Portal de desarrollador de Xbox (XDP) y recupera el id. del producto desde la dirección URL. Como alternativa, si descargas los datos de estado del informe de análisis del centro de desarrollo de Windows, el identificador de producto se incluye en el archivo TSV. |  Sí  |
| failureHash | cadena | El id. exclusivo del error sobre el que quieres obtener información detallada. Para obtener este valor para el error que te interesa, usa el método [obtener datos informes de errores de la Xbox One en juegos](get-error-reporting-data-for-your-xbox-one-game.md) y usa el valor de **failureHash** en el cuerpo de respuesta de ese método. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | No   |
| orderby | cadena | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos detallados del error para una consola Xbox One juego. Reemplaza el valor de *applicationId* con el identificador de producto para tu juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails?applicationId=BRRT4NJ9B3D1&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción    |
|------------|---------|------------|
| Valor      | matriz   | Una matriz de objetos que contienen los datos detallados del error. Para más información sobre los datos de cada objeto, consulta la sección sobre los [valores detallados del error](#error-detail-values) que encontrarás a continuación.          |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de errores de la solicitud. |
| TotalCount | entero | El número total de filas en el resultado de datos de la consulta.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valores detallados del error

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción     |
|-----------------|---------|----------------------------|
| applicationId   | string  | El identificador de producto del juego de Xbox One cual recuperaste los datos detallados del error.      |
| failureHash     | cadena  | El identificador único del error.     |
| failureName     | cadena  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen donde se produjo el error y el nombre de función asociada.           |
| date            | cadena  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| cabId           | cadena  | El identificador exclusivo del archivo CAB asociado con el error.   |
| cabExpirationTime  | cadena  | La fecha y la hora a las que el archivo .cab expira y ya no se puede descargar, en formato ISO 8601.   |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.     |
| osBuild         | cadena  | El número de compilación del sistema operativo en el que se ha producido el error.       |
| packageVersion  | cadena  | La versión del paquete de juego que está asociado al error.    |
| deviceModel           | cadena  | Una de las cadenas siguientes que especifica la consola Xbox One en el que se estaba ejecutando el juego cuando se produjo el error.<p/><ul><li><strong>Microsoft Xbox uno</strong></li><li><strong>Microsoft Xbox One S</strong></li><li><strong>Microsoft Xbox One X</strong></li></ul>  |
| osVersion       | cadena  | Versión del sistema operativo en el que sucedió el error. Este es siempre el valor **de Windows 10**.    |
| osRelease       | cadena  |  Una de las cadenas siguientes que especifica la versión del sistema operativo de Windows 10 o (como una subpoblación dentro de la versión del sistema operativo) en el que se produjo el error.<p/><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Vista previa de versión</strong></li><li><strong>Modo anticipado de Insider</strong></li><li><strong>Modo aplazado de Insider</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p>    |
| deviceType      | cadena  | El tipo de dispositivo en el que se produjo el error. Este es siempre el valor de la **consola**.     |
| cabDownloadable           | Booleano  | Indica si el usuario puede descargar el archivo CAB.   |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "applicationId": "BRRT4NJ9B3D1",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoSports.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2018-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.17134",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Microsoft-Xbox One",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "deviceType": "Console",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos del informe de la Xbox One de errores de juego](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtener el seguimiento de la pila de un error en tu Xbox One juego](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Descargar el archivo .cab para un error en tu juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
