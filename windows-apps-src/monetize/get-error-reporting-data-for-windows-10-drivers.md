---
author: mcleanbyron
ms.assetid: BAC79A64-445D-4702-8E96-7727FF180245
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados del informe de errores de controladores de Windows 10 para un intervalo de fechas y otros filtros opcionales. Este método está previsto solo para IHV.
title: Obtener datos de informes de errores para controladores de Windows 10
ms.author: mcleans
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis de la Store Windows, errors, errores
ms.localizationpriority: medium
ms.openlocfilehash: e87a836c08f29e1e7279c19566ead8a8d7e36453
ms.sourcegitcommit: cd91724c9b81c836af4773df8cd78e9f808a0bb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "1989409"
---
# <a name="get-error-reporting-data-for-windows-10-drivers"></a>Obtener datos de informes de errores para controladores de Windows 10

Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados del informe de los errores de controladores de Windows 10 para un intervalo de fechas y otros filtros opcionales. Puedes recuperar información adicional de los errores con el método para [Obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md).

> [!NOTE]
> Solo las cuentas de desarrollador que pertenecen al [programa Centro de desarrollo de hardware de Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) pueden usar este método.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId   | cadena  | Valor del id. del producto del controlador del cual quieres recuperar los datos de errores. | Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor siempre es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>date</strong></li><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul> | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. Si especificas los valores <strong>semana</strong> o <strong>mes</strong>, los valores <em>failureName</em> y <em>failureHash</em> se limitarán a 1000 depósitos. | No |
| orderby | cadena | Una instrucción que ordena los valores de datos de resultados. La sintaxis es <em>orderby=field [order],field [order],....</em>. Puede especificar los siguientes campos desde el cuerpo de la respuesta:<p/><ul><li><strong>date</strong></li><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | cadena | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>submissionId</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>Las filas de datos que se devuelvan, contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>fecha</strong></li><li><strong>applicationId</strong></li><li><strong>eventCount</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En los siguientes ejemplos se muestran varias solicitudes para obtener los datos del informe de errores de hardware OEM.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits?applicationId=18430592857500421&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits?applicationId=18430592857500421&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción     |
|------------|---------|--------------|
| Valor      | matriz   | Una matriz de objetos que contienen los datos agregados del informe de errores. Para más información sobre los datos de cada objeto, consulta la tabla siguiente.     |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | entero | El número total de filas en el resultado de datos de la consulta.     |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|---------------------|
| fecha            | cadena  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | cadena  | Valor del id. del producto del controlador del cual recuperaste los datos de errores. |
| submissionId   | cadena  | El id. de envío que está asociado con el controlador. |
| failureName     | cadena  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de controlador donde se produjo el error y el nombre de función asociada.  |
| failureHash     | cadena  | Identificador único del error.   |
| symbol          | cadena  | Símbolo que se asigna al error. |
| osVersion       | cadena  | La versión de compilación de cuatro partes del sistema operativo en el que se produjo el error.  |
| osName       | cadena  | El nombre del sistema operativo en el que se produjo el error.  |
| eventType       | cadena  | El tipo del error que se produjo.      |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | cadena  | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo el error:<p/><ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>    |
| driverName     | cadena  | Nombre único del controlador que está asociado al error.      |
| driverVersion  | cadena  | Versión del controlador que está asociado al error.   |
| architecture | cadena |  La arquitectura del dispositivo en la que se produjo el error.  |
| oemName | cadena | El nombre de OEM del dispositivo en el que se produjo el error. |
| oemModel | cadena | El nombre del modelo de dispositivo en el que se produjo el error. |
| flightRing | cadena | El nombre del piloto de SO en el que se produjo el error. |
| eventCount      | entero | El número de eventos atribuidos a este error del nivel de agregación especificado.      |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2017-02-26",
      "submissionId":"29989500_13736592797500435_1152921504626321174",
      "applicationId":"18430592857500421",
      "driverName": "fastboot.sys",
      "osVersion": "10.0.14393.693",
      "osName": "Windows 10",
      "flightRing": "Windows 10 Retail",
      "oemName": "Contoso",
      "oemModel": "C-122",
      "market": "US",
      "symbol": "fastboot",
      "failureName": "AV_fastboot!unknown_function",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "architecture": "x64",
      "driverVersion": "2.1.1.0",
      "deviceType": "Unknown",
      "eventType": "System crash",
      "deviceCount": 1.0,
      "eventCount": 1.0
    }]
}
```

## <a name="related-topics"></a>Temas relacionados

* [Obtener los detalles de un error de controlador de Windows 10](get-details-for-a-windows-10-driver-error.md)
* [Descargar el archivo .cab para un error de controlador de Windows 10](download-the-cab-file-for-a-windows-10-driver-error.md)
