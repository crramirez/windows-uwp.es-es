---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados del informe de errores de un intervalo de fechas y otros filtros opcionales.
title: Obtener datos de informes de errores para la aplicación
ms.date: 09/04/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Windows Store analytics API, API de análisis de Microsoft Store, errors, errores
ms.localizationpriority: medium
ms.openlocfilehash: 69504fd8d670158a35e5afd5dcd035acd296f084
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8738286"
---
# <a name="get-error-reporting-data-for-your-app"></a>Obtener datos de informes de errores para la aplicación

Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados de informes de errores relativos a tu aplicación en formato JSON de un intervalo de fechas dado y según otros filtros opcionales. Este método solo puede recuperar los errores que se han producido en los últimos 30 días. Esta información también está disponible en la sección de **errores** del [informe de estado](../publish/health-report.md) en el centro de partners.

Puedes recuperar información adicional de los errores con los métodos para [obtener los detalles de un error](get-details-for-an-error-in-your-app.md), [obtener el seguimiento de la pila](get-the-stack-trace-for-an-error-in-your-app.md) y [descargar archivo CAB](download-the-cab-file-for-an-error-in-your-app.md).

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | cadena | El Id. de la Store de la aplicación sobre la que quieres recuperar los datos del informe de errores. El identificador de la tienda está disponible en la [página de identidad de la aplicación](../publish/view-app-identity-details.md) en el centro de partners. Un ejemplo de un Id. de la Store sería 9WZDNCRFJ3Q8. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. Si *aggregationLevel* es **day**, **week** o **month**, este parámetro debe especificar una fecha con el formato ```mm/dd/yyyy```. Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha con el formato ```mm/dd/yyyy``` o una fecha y hora con el formato ```yyyy-mm-dd hh:mm:ss```.<p/><p/>**Nota:**&nbsp;&nbsp;este método solo puede recuperar los errores que se han producido en los últimos 30 días.  |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. Si *aggregationLevel* es **day**, **week** o **month**, este parámetro debe especificar una fecha con el formato ```mm/dd/yyyy```. Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha con el formato ```mm/dd/yyyy``` o una fecha y hora con el formato ```yyyy-mm-dd hh:mm:ss```. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo para el que se han de recuperar los datos agregados. Puede ser una de las siguientes cadenas: **hour**, **day**, **week** o **month**. Si no se especifica, el valor predeterminado es **day**. Si especificas los valores **week** o **month**, los valores *failureName* y *failureHash* se limitarán a 1000 depósitos.<p/><p/>**Nota:**&nbsp;&nbsp;Si especificas **hour**, puedes recuperar datos de error solo de las últimas 72 horas. Para recuperar datos de error anteriores a 72 horas, especifica **day** o uno de los otros niveles de agregación.  | No |
| orderby | cadena | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es *orderby=field [order],field [order],...*. El parámetro *field* puede ser una de las siguientes cadenas:<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>El parámetro *order* es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **asc**.</p><p>Aquí tienes un ejemplo de una cadena *orderby*: *orderby=date,market*</p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>Las filas de datos que se devuelvan, contendrán los campos especificados en el parámetro *groupby* y en los siguientes:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>Puedes usar el parámetro *groupby* con *aggregationLevel*. Por ejemplo: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener los datos del informe de errores. Reemplaza el valor *applicationId* por el Id. de la Store de la aplicación.

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
| TotalCount | entero | El número total de filas en el resultado de datos de la consulta.     |


### <a name="error-values"></a>Valores de error

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|---------------------|
| date            | cadena  | La primera fecha del intervalo de fechas de los datos del error con el formato ```yyyy-mm-dd```. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica un intervalo de fechas más largo, este valor será la primera fecha de ese intervalo de fechas. Para las solicitudes que especifican un valor *aggregationLevel* de **hora**, este valor también incluye un valor de hora con el formato ```hh:mm:ss```.  |
| applicationId   | cadena  | El Id. de la Store de la aplicación sobre la que quieres recuperar los datos del error.   |
| applicationName | cadena  | El nombre para mostrar de la aplicación.   |
| failureName     | cadena  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen donde se produjo el error y el nombre de función asociada.  |
| failureHash     | cadena  | Identificador único del error.   |
| symbol          | cadena  | Símbolo que se asigna al error. |
| osVersion       | cadena  | Una de las cadenas siguientes que especifica la versión del sistema operativo en la que se produjo el error:<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**Unknown**</li></ul>  |
| osRelease       | cadena  |  Una de las siguientes cadenas que especifica la versión del sistema operativo o el canal de actualizaciones (como una subpoblación dentro de la versión del sistema operativo) en el que se ha producido el error.<p/><p>Para Windows10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Vista previa de versión</strong></li><li><strong>Modo anticipado de Insider</strong></li><li><strong>Modo aplazado de Insider</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Para Windows7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p>    |
| eventType       | cadena  | Una de las cadenas siguientes:<ul><li>**bloquear**</li><li>**colgar**</li><li>**memoria**</li><li>**jse**</li></ul>      |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | cadena  | Una de las siguientes cadenas que indica el tipo de dispositivo en el que se produjo el error:<ul><li>**PC**</li><li>**Phone**</li><li>**Console**</li><li>**IoT**</li><li>**Holographic**</li><li>**Unknown**</li></ul>    |
| packageName     | cadena  | Nombre único del paquete de la aplicación que está asociado al error.      |
| packageVersion  | cadena  | Versión del paquete de la aplicación que está asociado al error.   |
| deviceCount     | number | Número de dispositivos únicos que corresponden a este error del nivel de agregación especificado.  |
| eventCount      | number | El número de eventos atribuidos a este error del nivel de agregación especificado.      |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1507",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Temas relacionados

* [Informe de estado](../publish/health-report.md)
* [Obtener los detalles de un error en la aplicación](get-details-for-an-error-in-your-app.md)
* [Obtener el seguimiento de la pila de un error en la aplicación](get-the-stack-trace-for-an-error-in-your-app.md)
* [Descargar el archivo .cab para un error en tu aplicación](download-the-cab-file-for-an-error-in-your-app.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener adquisiciones de aplicaciones](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener la clasificación de la aplicación](get-app-ratings.md)
* [Obtener opiniones de la aplicación](get-app-reviews.md)
