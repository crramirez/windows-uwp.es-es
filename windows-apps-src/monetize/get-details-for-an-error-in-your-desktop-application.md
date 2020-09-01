---
description: Use este método en la API de Microsoft Store Analytics para obtener datos detallados de un error específico de la aplicación de escritorio.
title: Obtener los detalles asociados a un error en la aplicación de escritorio
ms.date: 06/05/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, errores, detalles, aplicación de escritorio
ms.localizationpriority: medium
ms.openlocfilehash: 5fb904caf6e7cda0cba43d11b94aeb0811e8a504
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155619"
---
# <a name="get-details-for-an-error-in-your-desktop-application"></a>Obtener los detalles asociados a un error en la aplicación de escritorio

Use este método en la API de Microsoft Store Analytics para obtener datos detallados de un error específico de la aplicación en formato JSON. Este método solo puede recuperar detalles de errores que se hayan producido en los últimos 30 días. Los datos de error detallados también están disponibles en el [Informe de mantenimiento](/windows/desktop/appxpkg/windows-desktop-application-program) de las aplicaciones de escritorio del centro de Partners.

Para poder usar este método, debes usar el método para [obtener los datos del informe de errores](get-error-reporting-data.md) con el fin de recuperar el identificador del error sobre el que quieres obtener información detallada.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del error sobre el que quieres obtener información detallada. Para obtener este identificador, usa el método para [obtener los datos del informe de errores](get-error-reporting-data.md) y utiliza el valor **failureHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El ID. del producto de la aplicación de escritorio para la que desea recuperar los detalles del error. Para obtener el identificador de producto de una aplicación de escritorio, abra el [Informe de análisis de la aplicación de escritorio en el centro de Partners](/windows/desktop/appxpkg/windows-desktop-application-program) (como el **Informe de mantenimiento**) y recupere el identificador de producto de la dirección URL. |  Sí  |
| failureHash | string | El identificador exclusivo del error sobre el que quieres obtener información detallada. Para obtener este valor respecto al error que te interesa, usa el método para [obtener los datos del informe de errores](get-error-reporting-data.md) y utiliza el valor **failureHash** del cuerpo de la respuesta de ese método. |  Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es 30 días antes de la fecha actual.<p/><p/>**Nota:** &nbsp; &nbsp; Este método solo puede recuperar los detalles de los errores que se produjeron en los últimos 30 días. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de los datos detallados del error que se quieren recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente. |  No  |
| filter |string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>datamarket</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul> | No   |
| orderby | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li><strong>datamarket</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener datos detallados del error. Reemplace el valor de *ApplicationID* por el ID. de producto de la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo    | Descripción    |
|------------|---------|------------|
| Value      | array   | Una matriz de objetos que contienen los datos detallados del error. Para obtener más información sobre los datos de cada objeto, consulta la sección [Valores detallados del error](#error-detail-values) que encontrarás a continuación.          |
| @nextLink  | string  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | integer | El número total de filas del resultado de datos de la consulta.        |


<span id="error-detail-values"/>

### <a name="error-detail-values"></a>Valores detallados del error

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value           | Tipo    | Descripción     |
|-----------------|---------|----------------------------|
| applicationId   | string  | El ID. del producto de la aplicación de escritorio para la que ha recuperado los detalles del error.      |
| failureHash     | string  | El identificador único del error.     |
| failureName     | string  | El nombre del error, que se compone de cuatro partes: una o varias clases problemáticas, un código de comprobación de excepción/error, el nombre de la imagen en la que se produjo el error y el nombre de la función asociada.           |
| date            | string  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| cabIdHash           | string  | El hash de identificador único del archivo. CAB que está asociado a este error.   |
| cabExpirationTime  | string  | La fecha y la hora a las que el archivo CAB expira y ya no se puede descargar, en formato ISO 8601.   |
| market          | string  | El código de país ISO 3166 del mercado del dispositivo.     |
| osBuild         | string  | El número de compilación del sistema operativo en el que se ha producido el error.       |
| applicationVersion         | string  |   Versión del archivo ejecutable de la aplicación en el que se produjo el error.     |
| deviceModel           | string  | Una cadena que especifica el modelo del dispositivo en el que se estaba ejecutando la aplicación cuando se produjo el error.   |
| osVersion       | string  | Una de las siguientes cadenas que especifica la versión del sistema operativo en la que se instala la aplicación de escritorio:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>    |
| osRelease       | string  |  Una de las siguientes cadenas que especifica la versión del sistema operativo o el anillo de vuelo (como un rellenado de la versión del sistema operativo) en el que se produjo el error.<p/><p>Para Windows 10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>VERSIÓN</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows 8.1:</p><ul><li><strong>Actualización 1</strong></li></ul><p>Para Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el anillo de vuelo, este campo tiene el valor <strong>desconocido</strong>.</p>    |
| deviceType      | string  | Una de las siguientes cadenas que indica el tipo de dispositivo en el que se produjo el error: <p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Unknown</strong></li></ul>     |
| cabDownloadable           | Boolean  | Indica si el usuario puede descargar el archivo CAB.   |
| fileName           | string  | Nombre del archivo ejecutable de la aplicación de escritorio para la que se han recuperado los detalles del error.  |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "applicationId": "10238467886765136388",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "NULL_CLASS_PTR_WRITE_c0000005_contoso.exe!unknown_error_in_process",
      "date": "2018-01-28 23:55:29",
      "cabIdHash": "54ffb83a-e159-41d2-8158-f36f306cc01e",
      "cabExpirationTime": "2018-02-27 23:55:29",
      "market": "US",
      "osBuild": "10.0.10240",
      "applicationVersion": "2.2.2.0",
      "deviceModel": "Contoso All-in-one",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "deviceType": "PC",
      "cabDownloadable": false,
      "fileName": "contosodemo.exe"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe de estado](../publish/health-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores de la aplicación de escritorio](get-desktop-application-error-reporting-data.md)
* [Obtener el seguimiento de pila asociado a un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Descargar el archivo CAB asociado a un error en la aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)