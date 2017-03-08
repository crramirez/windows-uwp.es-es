---
author: mcleanbyron
ms.assetid: f0c0325e-ad61-4238-a096-c37802db3d3b
description: "Usa este método en la API de análisis de la Tienda Windows para obtener información detallada acerca de un error específico de tu aplicación."
title: "Obtener los detalles de un error en la aplicación"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, UWP, servicios de la Tienda, Store services, API de análisis de la Tienda Windows, Windows Store analytics API, errores, errors, detalles, details"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1aaf5b6864c678d26289270be6a9ad96f5b5e004
ms.lasthandoff: 02/08/2017

---

# <a name="get-details-for-an-error-in-your-app"></a>Obtener los detalles de un error en la aplicación

Usa este método en la API de análisis de la Tienda Windows para obtener información detallada acerca de un error específico de tu aplicación en formato JSON. Este método solo puede recuperar detalles de errores que se hayan producido en los últimos 30 días. Los datos de error detallados también están disponibles en la sección **Errores** del [Informe de estado](../publish/health-report.md) en el panel del Centro de desarrollo de Windows.

Para poder usar este método, debes usar el método para [obtener los datos del informe de errores](get-error-reporting-data.md) con el fin de recuperar el identificador del error sobre el que quieres obtener información detallada.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el identificador del error sobre el que quieres obtener información detallada. Para obtener este identificador, usa el método para [obtener los datos del informe de errores](get-error-reporting-data.md) y utiliza el valor **failureHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails``` |

<span/> 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con el formato **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | cadena | El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos detallados del error. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| failureHash | cadena | El identificador exclusivo del error sobre el que quieres obtener información detallada. Para obtener este valor respecto al error que te interesa, usa el método para [obtener los datos del informe de errores](get-error-reporting-data.md) y utiliza el valor **failureHash** del cuerpo de la respuesta de ese método. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas de la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |
| orderby | cadena | Una instrucción que ordena los valores de datos resultantes. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabId</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>deviceType</strong></li><li><strong>deviceModel</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |

<span/>
 
### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Estos son algunos ejemplos de parámetros *filter*:

-   *filter=market eq 'US' and osVersion eq 'Windows 10'*
-   *filter=market ne 'US' and osVersion ne 'Windows 8'*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples.

| Campos        |  Descripción        |
|---------------|-----------------|
| osVersion | Una de las cadenas siguientes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| osBuild | El número de compilación del sistema operativo en el que se estaba ejecutando la aplicación cuando se produjo el error. |
| market | Una cadena que contiene el código de país ISO 3166 del mercado del dispositivo en el que se estaba ejecutando la aplicación cuando se produjo el error. |
| deviceType | Una de las siguientes cadenas, que especifica el tipo del dispositivo en el que se estaba ejecutando la aplicación cuando se produjo el error:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceModel | Una cadena que especifica el modelo del dispositivo en el que se estaba ejecutando la aplicación cuando se produjo el error. |
| cabId | El identificador exclusivo del archivo CAB asociado con el error. |
| cabExpirationTime | La fecha y la hora a las que el archivo CAB expira y ya no se puede descargar, en formato ISO 8601. |
| packageVersion | La versión del paquete de la aplicación asociado con el error. |

<span/> 

### <a name="request-example"></a>Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener datos detallados del error. Sustituye el valor *applicationId* por el identificador de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failuredetails?applicationId=9NBLGGGZ5QDR&failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'Windows.Desktop' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción    |
|------------|---------|------------|
| Valor      | matriz   | Una matriz de objetos que contienen los datos detallados del error. Para obtener más información sobre los datos de cada objeto, consulta la sección [Valores detallados del error](#error-detail-values) que encontrarás a continuación.          |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | número | El número total de filas del resultado de datos de la consulta.        |

<span id="error-detail-values"/>
### <a name="error-detail-values"></a>Valores detallados del error

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción     |
|-----------------|---------|----------------------------|
| date            | cadena  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | cadena  | El Id. de la Tienda de la aplicación sobre la que has recuperado los datos detallados del error.      |
| failureName     | cadena  | El nombre del error. Este es el mismo nombre que aparece en la sección **Errores** del [informe de estado](../publish/health-report.md) en el panel del Centro de desarrollo de Windows.            |
| failureHash     | cadena  | El identificador único del error.     |
| osVersion       | cadena  | La versión del sistema operativo en el que se ha producido el error.    |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.     |
| deviceType      | cadena  | El tipo de dispositivo en el que se ha producido el error.     |
| packageVersion  | cadena  | La versión del paquete de la aplicación asociado con el error.    |
| osBuild         | cadena  | El número de compilación del sistema operativo en el que se ha producido el error.       |
| cabId           | cadena  | El identificador exclusivo del archivo CAB asociado con el error.   |
| cabExpirationTime  | cadena  | La fecha y la hora a las que el archivo CAB expira y ya no se puede descargar, en formato ISO 8601.   |
| deviceModel           | cadena  | Una cadena que especifica el modelo del dispositivo en el que se estaba ejecutando la aplicación cuando se produjo el error.   |
| cabDownloadable           | Booleano  | Indica si el usuario puede descargar el archivo CAB.   |

<span/> 

### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR ",
      "failureHash": "012345-5dbc9-b12f-c124-9d9810f05d8b",
      "failureName": "STOWED_EXCEPTION_System.UriFormatException_exe!ContosoGame.GroupedItems+_ItemView_ItemClick_d__9.MoveNext",
      "date": "2015-02-05 09:11:25",
      "cabId": "133637331323",
      "cabExpirationTime": "2016-12-05 09:11:25",
      "market": "US",
      "osBuild": "10.0.10240",
      "packageVersion": "1.0.2.6",
      "deviceModel": "Contoso Computer",
      "osVersion": "Windows 10",
      "deviceType": "Windows.Desktop",
      "cabDownloadable": false
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe de estado](../publish/health-report.md)
* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener el seguimiento de la pila de un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)

