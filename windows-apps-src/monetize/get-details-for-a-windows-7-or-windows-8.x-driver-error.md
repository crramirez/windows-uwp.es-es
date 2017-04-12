---
author: mcleanbyron
ms.assetid: 2FBA0B73-17C6-4F25-A79D-63F2F262491A
description: "Usa este método en la API de análisis de la Tienda Windows para obtener información detallada para un error de controlador de Windows 7 o Windows 8.x. Este método está previsto solo para IHV."
title: Obtener los detalles para un error de controlador de Windows 7 o Windows 8.x
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Store services, servicios de la Tienda, Windows Store analytics API, API de análisis de la Tienda Windows, errors, errores, details, detalles"
ms.openlocfilehash: fbf1ee456080ff228abde061dbfef859baf79465
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-details-for-a-windows-7-or-windows-8x-driver-error"></a>Obtener los detalles para un error de controlador de Windows 7 o Windows 8.x

Usa este método en la API de análisis de la Tienda Windows para obtener información detallada sobre un error específico de controlador de Windows 7 o Windows 8.x en formato JSON. Para poder usar este método, tienes que emplear primero el método para [obtener datos de informes de errores para controladores de Windows 7 y Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) con el fin de recuperar el id. del error para el que quieres obtener información detallada.

> [!NOTE]
> Solo las cuentas de desarrollador que pertenecen al [programa Centro de desarrollo para hardware de Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard) pueden usar este método.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el id. del error sobre el que quieres obtener información detallada. Para obtener este id., usa el método para [obtener datos de informes de errores para controladores de Windows 7 y Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md) y usa el valor **failureHash** en el cuerpo de la respuesta de ese método.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails``` |

<span/> 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| failureHash | cadena | El id. exclusivo del error sobre el que quieres obtener información detallada. Para obtener este valor respecto al error que te interesa, usa el método para [obtener datos informes de errores de hardware OEM](get-oem-hardware-error-reporting-data.md) y usa el valor **failureHash** en el cuerpo de la respuesta de ese método. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos detallados del error que se quieren recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>date</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li></ul> | No   |
| orderby | cadena | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es <em>orderby=field [order],field [order],...</em>. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>cabType</strong></li><li><strong>cabExpirationTime</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osName</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>driverName</strong></li><li><strong>driverVersion</strong></li><li><strong>oemName</strong></li><li><strong>oemModel</strong></li><li><strong>flightRing</strong></li><li><strong>architecture</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |

<span/> 

### <a name="request-example"></a>Ejemplo de solicitud

En los siguientes ejemplos se muestran varias solicitudes para obtener datos detallados del error.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ihvdriver/failuredetails?failureHash=012e33e3-dbc9-b12f-c124-9d9810f05d8b&startDate=2016-11-05&endDate=2016-11-06&top=10&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción    |
|------------|---------|------------|
| Valor      | matriz   | Una matriz de objetos que contienen los datos detallados del error. Para más información sobre los datos de cada objeto, consulta la tabla siguiente.          |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de errores de la solicitud. |
| TotalCount | número | El número total de filas del resultado de datos de la consulta.        |

<span/>

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción     |
|-----------------|---------|----------------------------|
| fecha            | cadena  | La primera fecha del intervalo de fechas de los datos del error. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| sellerId   | cadena  | El valor del id. del vendedor asociado con la cuenta de desarrollador (coincide con el valor del **Id. del vendedor** de la configuración de la cuenta del Centro de desarrollo). |
| failureName     | cadena  | El nombre del error. Este es el mismo nombre que aparece en la sección **Errores** del [informe de estado](../publish/health-report.md) en el panel del Centro de desarrollo de Windows.            |
| failureHash     | cadena  | Identificador único del error.     |
| symbol     | cadena  | Símbolo que se asigna al error.     |
| osVersion       | cadena  | La versión de compilación de cuatro partes del sistema operativo en el que se produjo el error.    |
| osName       | cadena  | El nombre del sistema operativo en el que se produjo el error.  |
| eventType       | cadena  | El tipo del error que se produjo.      |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.     |
| deviceType      | cadena  | El tipo de dispositivo en el que se produjo el error.     |
| driverName     | cadena  | Nombre único del controlador que está asociado al error.      |
| driverVersion  | cadena  | Versión del controlador que está asociado al error.   |
| architecture | cadena |  La arquitectura del dispositivo en la que se produjo el error.  |
| oemName | cadena | El nombre de OEM del dispositivo en el que se produjo el error. |
| oemModel | cadena | El nombre del modelo de dispositivo en el que se produjo el error. |
| flightRing | cadena | El nombre del piloto de SO en el que se produjo el error. |
| clientDeviceId | cadena | El id. del dispositivo en el que se produjo el error. |
| cabIdHash         | cadena  | El id. exclusivo del archivo .cab asociado con el error.   |
| cabType         | cadena  | El tipo del archivo .cab.   |
| cabExpirationTime  | cadena  | La fecha y la hora a las que el archivo .cab expira y ya no se puede descargar, en formato ISO 8601.   |

<span/> 

### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "sellerId": "14313740",
      "architecture": "x64",
      "cabExpirationTime": "2017-06-12 23:51:11",
      "cabIdHash": "cc1797f9-b783-44d4-b0e5-46ae7ca668bc",
      "cabType": "MINI",
      "clientDeviceId": null,
      "date": "2017-03-14 23:51:11",
      "deviceType": "PC",
      "driverName": "fastboot.sys",
      "driverVersion": "2.1.1.0",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "failureName": "0x109_18_2_LoadImageNotify",
      "market": "US",
      "oemModel": "C-122",
      "oemName": "Contoso",
      "osName": "Windows 8.1",
      "osVersion": "6.3.9600.9600"
    }]
}
```

## <a name="related-topics"></a>Temas relacionados

* [Obtener datos de informes de errores para controladores de Windows 7 y Windows 8.x](get-error-reporting-data-for-windows-7-and-windows-8.x-drivers.md)
* [Descargar el archivo .cab para un error de controlador de Windows 7 o Windows 8.x](download-the-cab-file-for-a-windows-7-or-windows-8.x-driver-error.md)
