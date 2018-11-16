---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener información detallada acerca de un error específico de tu aplicación de escritorio.
title: Obtener los detalles de un error en la aplicación de escritorio
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis Microsoft Store, errors, errores, details, detalles
ms.localizationpriority: medium
ms.openlocfilehash: 922ab18bfebfbe539788ade3caa7626919d6b19a
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995066"
---
# <a name="get-details-for-an-error-in-your-desktop-application"></a>Obtener los detalles de un error en la aplicación de escritorio

Usa este método en la API de análisis de Microsoft Store para obtener información detallada acerca de un error específico de tu aplicación en formato JSON. Este método solo puede recuperar detalles de errores que se hayan producido en los últimos 30 días. Datos de error detallados también están disponibles en el [informe de estado](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicaciones de escritorio en el centro de partners.

Para poder usar este método, debes usar el método para [obtener los datos del informe de errores](get-error-reporting-data.md) con el fin de recuperar el identificador del error sobre el que quieres obtener información detallada.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del error sobre el que quieres obtener información detallada. Para obtener este identificador, usa el método para [obtener los datos del informe de errores](get-error-reporting-data.md) y utiliza el valor **failureHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId | cadena | El identificador de producto de la aplicación de escritorio de la cual quieres recuperar los detalles de errores. Para obtener el identificador de producto de una aplicación de escritorio, abra cualquier [informe de análisis de la aplicación de escritorio en el centro de partners](https://msdn.microsoft.com/library/windows/desktop/mt826504) (por ejemplo, el **informe de estado**) y recuperar el identificador de producto de la dirección URL. |  Sí  |
| failureHash | cadena | El identificador exclusivo del error sobre el que quieres obtener información detallada. Para obtener este valor respecto al error que te interesa, usa el método para [obtener los datos del informe de errores](get-error-reporting-data.md) y utiliza el valor **failureHash** del cuerpo de la respuesta de ese método. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es 30 días antes de la fecha actual.<p/><p/>**Nota:**&nbsp;&nbsp;este método solo puede recuperar los detalles de errores que se han producido en los últimos 30 días. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul> | No   |
| orderby | cadena | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>market</strong></li><li><strong>date</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>osRelease</strong></li><li><strong>applicationVersion</strong></li><li><strong>osBuild</strong></li><li><strong>fileName</strong></li></ul><p>El parámetro <em>order</em> es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En los siguientes ejemplos se muestran varias solicitudes para obtener datos detallados del error. Reemplaza el valor *applicationId* por el identificador de producto de la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failuredetails?applicationId=10238467886765136388&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
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
| applicationId   | cadena  | El identificador de producto de la aplicación de escritorio de la cual recuperaste los detalles de errores.      |
| failureHash     | cadena  | El identificador único del error.     |
| failureName     | cadena  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen donde se produjo el error y el nombre de función asociada.           |
| date            | cadena  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| cabIdHash           | cadena  | El hash de id. exclusivo del archivo .cab asociado con el error.   |
| cabExpirationTime  | cadena  | La fecha y la hora a las que el archivo .cab expira y ya no se puede descargar, en formato ISO 8601.   |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.     |
| osBuild         | cadena  | El número de compilación del sistema operativo en el que se ha producido el error.       |
| applicationVersion         | cadena  |   La versión de la aplicación ejecutable en la que se produjo el error.     |
| deviceModel           | cadena  | Una cadena que especifica el modelo del dispositivo en el que se estaba ejecutando la aplicación cuando se produjo el error.   |
| osVersion       | cadena  | Una de las cadenas siguientes que especifica la versión del sistema operativo en la que se realizó la instalación de la aplicación de escritorio:<p/><ul><li><strong>Windows7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>WindowsServer2016</strong></li><li><strong>WindowsServer1709</strong></li><li><strong>Unknown</strong></li></ul>    |
| osRelease       | cadena  |  Una de las siguientes cadenas que especifica la versión del sistema operativo o el canal de actualizaciones (como una subpoblación dentro de la versión del sistema operativo) en el que se ha producido el error.<p/><p>Para Windows10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Vista previa de versión</strong></li><li><strong>Modo anticipado de Insider</strong></li><li><strong>Modo aplazado de Insider</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Para Windows7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p>    |
| deviceType      | cadena  | Una de las siguientes cadenas que indica el tipo de dispositivo en el que se produjo el error: <p/><ul><li><strong>PC</strong></li><li><strong>Servidor</strong></li><li><strong>Unknown</strong></li></ul>     |
| cabDownloadable           | Booleano  | Indica si el usuario puede descargar el archivo .cab.   |
| fileName           | cadena  | El nombre del archivo ejecutable de la aplicación de escritorio de la cual recuperaste los detalles de errores.  |


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

* [Informe Estado](../publish/health-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de informes de errores para la aplicación de escritorio](get-desktop-application-error-reporting-data.md)
* [Obtener el seguimiento de la pila de un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Descargar el archivo .cab para un error en tu aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)
