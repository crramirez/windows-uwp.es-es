---
description: Utilice este método en la API de análisis de Microsoft Store para que obtenga datos detallados de un error específico de Xbox One juegos.
title: Obtener detalles de un error en tu juego de Xbox One
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis de la Store Windows, errors, errores, details, detalles
ms.localizationpriority: medium
ms.openlocfilehash: da3252c42a0c2e2bd02465985737125cc053a616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589970"
---
# <a name="get-details-for-an-error-in-your-xbox-one-game"></a>Obtener detalles de un error en tu juego de Xbox One

Use este método en la Microsoft Store analytics API para obtener datos detallados para un error específico para Xbox One juegos que estaba introducidos mediante el Portal para desarrolladores de Xbox (XDP) y está disponible en el panel del centro de partners XDP Analytics. Este método solo puede recuperar detalles de errores que se hayan producido en los últimos 30 días.

Para poder usar este método, primero debe usar el [obtener informes de errores datos para Xbox One juego](get-error-reporting-data-for-your-xbox-one-game.md) método para recuperar el identificador del error para el que desea obtener información detallada.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del error sobre el que quieres obtener información detallada. Para obtener este identificador, utilice el [obtener informes de errores datos para Xbox One juego](get-error-reporting-data-for-your-xbox-one-game.md) método y el uso la **failureHash** valor en el cuerpo de respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failuredetails``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El **Store ID** del juego de Xbox One que va a recuperar los detalles del error. El **Store ID** está disponible en la página de la identidad de aplicación en el centro de partners. Un ejemplo **Store ID** es 9WZDNCRFJ3Q8. |  Sí  |
| failureHash | string | El identificador exclusivo del error sobre el que quieres obtener información detallada. Para obtener este valor para el error que le interese, use el [obtener informes de errores datos para Xbox One juego](get-error-reporting-data-for-your-xbox-one-game.md) método y el uso la **failureHash** valor en el cuerpo de respuesta de ese método. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente. |  No  |
| filter |string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul> | No   |
| orderby | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de error detallado para una Xbox One juegos. Reemplace el *applicationId* valor con el **Store ID** para su juego.

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
| Valor      | array   | Una matriz de objetos que contienen los datos detallados del error. Para obtener más información sobre los datos de cada objeto, consulta la sección [Valores detallados del error](#error-detail-values) que encontrarás a continuación.          |
| @nextLink  | string  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | número entero | El número total de filas del resultado de datos de la consulta.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valores detallados del error

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción     |
|-----------------|---------|----------------------------|
| applicationId   | string  | El **Store ID** del juego de Xbox One que recuperan datos de error detallado.      |
| failureHash     | string  | El identificador único del error.     |
| failureName     | string  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen donde se produjo el error y el nombre de función asociada.           |
| fecha            | string  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| cabId           | string  | El identificador exclusivo del archivo CAB asociado con el error.   |
| cabExpirationTime  | string  | La fecha y la hora a las que el archivo CAB expira y ya no se puede descargar, en formato ISO 8601.   |
| market          | string  | El código de país ISO 3166 del mercado del dispositivo.     |
| osBuild         | string  | El número de compilación del sistema operativo en el que se ha producido el error.       |
| packageVersion  | string  | La versión del paquete de juego que está asociado a este error.    |
| deviceModel           | string  | Una de las siguientes cadenas que especifica la consola Xbox One en el que el juego se estaba ejecutando cuando se produjo el error.<p/><ul><li><strong>Microsoft-Xbox One</strong></li><li><strong>Microsoft-Xbox One S</strong></li><li><strong>Microsoft-Xbox One X</strong></li></ul>  |
| osVersion       | string  | La versión del sistema operativo en el que se ha producido el error. Esto es siempre el valor **Windows 10**.    |
| osRelease       | string  |  Una de las siguientes cadenas que especifica la versión de sistema operativo de Windows 10 o anillo de distribución de paquetes piloto (como un subpoblación dentro de la versión del sistema operativo) en que se ha producido el error.<p/><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lenta</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p>    |
| deviceType      | string  | El tipo de dispositivo en el que se ha producido el error. Esto es siempre el valor **consola**.     |
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

## <a name="related-topics"></a>Temas relacionados

* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener juegos para Xbox One datos de informe de errores](get-error-reporting-data-for-your-xbox-one-game.md)
* [Obtener el seguimiento de pila para un error en Xbox One juegos](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Descargue el archivo CAB para un error en el juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
