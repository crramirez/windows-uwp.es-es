---
author: mcleanbyron
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: "Usa este método en la API de análisis de la Tienda Windows para obtener los datos agregados del informe de errores de un intervalo de fechas y otros filtros opcionales."
title: "Obtener datos de informes de errores para la aplicación"
ms.author: mcleans
ms.date: 06/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Store services, servicios de la Tienda, Windows Store analytics API, API de análisis de la Tienda Windows, errors, errores"
ms.openlocfilehash: 68e54c955d865669907c68d7cf1ef5a0f8986d8d
ms.sourcegitcommit: 7aabd2e59d45bbc5512dd4ddd9110ae62b79d552
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2017
---
# <a name="get-error-reporting-data-for-your-app"></a>Obtener datos de informes de errores para la aplicación

Usa este método en la API de análisis de la Tienda Windows para obtener los datos agregados de informes de errores relativos a tu aplicación en formato JSON de un intervalo de fechas dado y según otros filtros opcionales. Esta información también está disponible en la sección **Errores** del [informe de estado](../publish/health-report.md) en el panel del Centro de desarrollo de Windows.

Puedes recuperar información adicional de los errores con los métodos para [obtener los detalles de un error](get-details-for-an-error-in-your-app.md), [obtener el seguimiento de la pila](get-the-stack-trace-for-an-error-in-your-app.md) y [descargar archivo CAB](download-the-cab-file-for-an-error-in-your-app.md).

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |

<span/> 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | cadena | El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos del informe de errores. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. Si especificas los valores <strong>semana</strong> o <strong>mes</strong>, los valores <em>failureName</em> y <em>failureHash</em> se limitarán a 1000 depósitos. | No |
| orderby | cadena | Una instrucción que ordena los valores de datos resultantes. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>packageName</strong></li><li><strong>packageVersion</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>packageName</strong></li><li><strong>packageVersion</strong></li></ul><p>Las filas de datos que se devuelvan, contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>applicationName</strong></li><li><strong>deviceCount</strong></li><li><strong>eventCount</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  No  |

<span/>
 
### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Estos son algunos ejemplos de parámetros *filter*:

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Desconocido') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'mayor que 55' or ageGroup ne ‘menor que 13’)*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que los valores de la cadena deben estar entre comillas simples en el parámetro *filter*.

| Campos        |  Descripción        |
|---------------|-----------------|
| failureName | El nombre del error. |
| failureHash | Identificador único del error. |
| symbol | Símbolo que se asigna al error. |
| osVersion | Una de las cadenas siguientes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Desconocido</strong></li></ul> |
| eventType | Una de las cadenas siguientes:<ul><li><strong>bloquear</strong></li><li><strong>colgar</strong></li><li><strong>memoria</strong></li><li><strong>jse</strong></li></ul> |
| market | Una cadena que contiene el código de país ISO 3166 del mercado donde se ha producido el error. |
| deviceType | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| packageName | El nombre exclusivo del paquete de la aplicación asociado con el error. |
| packageVersion | Versión del paquete de la aplicación que está asociado al error. |

<span/> 

### <a name="request-example"></a>Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener los datos del informe de errores. Reemplaza el valor *applicationId* por el Id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción     |
|------------|---------|--------------|
| Valor      | matriz   | Matriz de objetos que contienen los datos agregados del informe de errores. Para más información sobre los datos de cada objeto, consulta la sección [valores de error](#error-values) que encontrarás a continuación.     |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | número | Número total de filas en el resultado de datos de la consulta.     |

<span/>

### <a name="error-values"></a>Valores de error

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|---------------------|
| fecha            | cadena  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | cadena  | El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos del error.   |
| applicationName | cadena  | El nombre para mostrar de la aplicación.   |
| failureName     | cadena  | El nombre del error.  |
| failureHash     | cadena  | Identificador único del error.   |
| symbol          | cadena  | Símbolo que se asigna al error. |
| osVersion       | cadena  | Versión del sistema operativo en el que sucedió el error. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).  |
| eventType       | cadena  | Tipo de evento de error. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).      |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | cadena  | El tipo de dispositivo en el que se ha producido el error. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).    |
| packageName     | cadena  | Nombre único del paquete de la aplicación que está asociado al error.      |
| packageVersion  | cadena  | Versión del paquete de la aplicación que está asociado al error.   |
| eventCount      | número | Número de eventos atribuidos a este error del nivel de agregación especificado.      |
| deviceCount     | número | Número de dispositivos únicos que corresponden a este error del nivel de agregación especificado.  |

<span/> 

### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows Phone 8",
      "eventType": "crash",
      "market": "US",
      "deviceType": "mobile",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>Temas relacionados

* [Informe de estado](../publish/health-report.md)
* [Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)
* [Obtener el seguimiento de la pila de un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)
* [Descargar el archivo .CAB para un error de la aplicación](download-the-cab-file-for-an-error-in-your-app.md)
* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos de compra de la aplicación](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener la clasificación de la aplicación](get-app-ratings.md)
* [Obtener opiniones de la aplicación](get-app-reviews.md)
