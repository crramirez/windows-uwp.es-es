---
author: mcleanbyron
ms.assetid: 8425F704-8A03-493F-A3D2-8442E85FD835
description: Usa este método en la API de análisis de Microsoft Store para obtener información detallada sobre un error específico de hardware. Este método está previsto solo para OEM.
title: Obtener detalles para un error de hardware OEM
ms.author: mcleans
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis de la Store Windows, errors, errores, details, detalles
ms.localizationpriority: medium
ms.openlocfilehash: 429ebc5237ce35baa6f9c3f31a25d480410d9c86
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/17/2018
ms.locfileid: "1663215"
---
# <a name="get-details-for-an-oem-hardware-error"></a>Obtener detalles para un error de hardware OEM

Usa este método en la API de análisis de Microsoft Store para obtener información detallada sobre un error específico de hardware OEM en formato JSON. Para poder usar este método, tienes que emplear primero el método para [obtener datos de informes de errores de hardware OEM](get-oem-hardware-error-reporting-data.md) con el fin de recuperar el id. del error sobre el que quieres obtener información detallada.

> [!NOTE]
> Solo las cuentas de desarrollador que pertenecen al [programa Centro de desarrollo para hardware de Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) pueden usar este método.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el id. del error sobre el que quieres obtener información detallada. Para obtener este id., usa el método para [obtener datos de informes de errores de hardware OEM](get-oem-hardware-error-reporting-data.md) y usa el valor **failureHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| failureHash | cadena | El id. exclusivo del error sobre el que quieres obtener información detallada. Para obtener este valor respecto al error que te interesa, usa el método para [obtener datos informes de errores de hardware OEM](get-oem-hardware-error-reporting-data.md) y usa el valor **failureHash** en el cuerpo de la respuesta de ese método. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los siguientes campos:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>modelo</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li><li><strong>flightRing</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | No   |
| orderby | cadena | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es <em>orderby=field [order],field [order],....</em>. Puede especificar los siguientes campos desde el cuerpo de la respuesta:<p/><ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>cabIdHash</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>cabType</strong></li><li><strong>deviceType</strong></li><li><strong>moduleName</strong></li><li><strong>moduleVersion</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>osBuild</strong></li><li><strong>mode</strong></li><li><strong>architecture</strong></li><li><strong>model</strong></li><li><strong>baseboard</strong></li><li><strong>modelFamily</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En los siguientes ejemplos se muestran varias solicitudes para obtener datos detallados del error.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/hardware/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción    |
|------------|---------|------------|
| Valor      | matriz   | Una matriz de objetos que contienen los datos detallados del error. Para más información sobre los datos de cada objeto, consulta la tabla siguiente.          |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de errores de la solicitud. |
| TotalCount | número | El número total de filas del resultado de datos de la consulta.        |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción     |
|-----------------|---------|----------------------------|
| fecha            | cadena  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| sellerId   | cadena  | El valor del id. del vendedor asociado con la cuenta de desarrollador (coincide con el valor del **Id. del vendedor** de la configuración de la cuenta del Centro de desarrollo). |
| failureName     | cadena  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen/controlador donde se produjo el error y el nombre de función asociada.             |
| failureHash     | cadena  | El identificador único del error.     |
| osVersion       | cadena  | La versión de compilación de cuatro partes del sistema operativo en el que se produjo el error.    |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.     |
| deviceType      | cadena  | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo el error:<p/><ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>     |
| moduleName     | cadena  | El nombre único del módulo que está asociado al error.      |
| moduleVersion  | cadena  | La versión del paquete del módulo que está asociado al error.   |
| architecture | cadena |  La arquitectura del dispositivo en la que se produjo el error.  |
| model | cadena | El nombre del modelo de dispositivo en el que se produjo el error. |
| baseboard | cadena | El nombre de la placa base del dispositivo en el que se produjo el error. |
| modelFamily | cadena | El nombre de la familia del modelo de dispositivo en el que se produjo el error. |
| mode | cadena | Este valor es siempre *kernel*. |
| cabIdHash         | cadena  | El id. exclusivo del archivo .cab asociado con el error.   |
| cabType         | cadena  | El tipo del archivo .cab.   |
| cabExpirationTime  | cadena  | La fecha y la hora a las que el archivo .cab expira y ya no se puede descargar, en formato ISO 8601.   |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2017-03-14 23:51:11",
      "sellerId": "14313740",
      "mode": "Kernel",
      "failureName": "0x109_18_2_LoadImageNotify",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "osVersion": "10.0.14393.10",
      "market": "US",
      "deviceType": "PC",
      "moduleName": "Unknown_Image",
      "moduleVersion": "Unknown",
      "architecture": "x64",
      "model": "101",
      "baseboard": "750-ABX",
      "modelFamily": "ContosoPad",
      "flightRing": "Windows 10 Retail"
    }]
}
```

## <a name="related-topics"></a>Temas relacionados

* [Obtener datos de informes de errores de hardware OEM](get-oem-hardware-error-reporting-data.md)
* [Descargar el archivo .cab para un error de hardware OEM](download-the-cab-file-for-an-oem-hardware-error.md)
