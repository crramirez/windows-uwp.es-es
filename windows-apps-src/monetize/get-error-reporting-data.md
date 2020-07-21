---
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: Use este método en la API de Microsoft Store Analytics para obtener datos de informes de errores agregados para un intervalo de fechas determinado y otros filtros opcionales.
title: Obtención de datos de informes de errores para la aplicación
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, errores
ms.localizationpriority: medium
ms.openlocfilehash: b1ecfad64598bb74ead0a912fa67b9e1c5c5d0c2
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492800"
---
# <a name="get-error-reporting-data-for-your-app"></a>Obtención de datos de informes de errores para la aplicación

Use este método en la API de Microsoft Store Analytics para obtener datos de informes de errores agregados para su aplicación en formato JSON para un intervalo de fechas determinado y otros filtros opcionales. Este método solo puede recuperar los errores que se produjeron en los últimos 30 días. Esta información también está disponible en la sección **errores** del [Informe de mantenimiento](../publish/health-report.md) del centro de Partners.

Puede recuperar información de error adicional mediante los métodos [Get error details](get-details-for-an-error-in-your-app.md), [Get stack trace](get-the-stack-trace-for-an-error-in-your-app.md)y [download CAB File](download-the-cab-file-for-an-error-in-your-app.md) .

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos del informe de errores. El identificador de almacén está disponible en la [Página identidad](../publish/view-app-identity-details.md) de la aplicación en el centro de Partners. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8. |  Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar. La fecha predeterminada es la actual. Si *aggregationLevel* es **día**, **semana**o **mes**, este parámetro debe especificar una fecha en el formato ```mm/dd/yyyy``` . Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha en el formato ```mm/dd/yyyy``` o una fecha y hora en el formato ```yyyy-mm-dd hh:mm:ss``` .<p/><p/>**Nota:** &nbsp; &nbsp; Este método solo puede recuperar los errores que se produjeron en los últimos 30 días.  |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos del informe de errores que se han de recuperar. La fecha predeterminada es la actual. Si *aggregationLevel* es **día**, **semana**o **mes**, este parámetro debe especificar una fecha en el formato ```mm/dd/yyyy``` . Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha en el formato ```mm/dd/yyyy``` o una fecha y hora en el formato ```yyyy-mm-dd hh:mm:ss``` . |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**símbolo**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**datamarket**</li><li>**TipoDeDispositivo**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **hour**, **Day**, **Week**o **Month**. Si no se especifica, el valor predeterminado es **día**. Si especificas los valores **semana** o **mes**, los valores *failureName* y *failureHash* se limitarán a 1000 depósitos.<p/><p/>**Nota:** &nbsp; &nbsp; Si especifica **hora**, solo puede recuperar datos de error de las 72 horas anteriores. Para recuperar los datos de error anteriores a 72 horas, especifique **Day** o uno de los otros niveles de agregación.  | No |
| orderby | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es *OrderBy = Field [order], Field [order],...*. El parámetro de *campo* puede ser una de las cadenas siguientes:<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**símbolo**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**datamarket**</li><li>**TipoDeDispositivo**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>El parámetro *order*, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **ASC**.</p><p>Este es un ejemplo de cadena *OrderBy* : *OrderBy = Date, Market*</p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li>**failureName**</li><li>**failureHash**</li><li>**símbolo**</li><li>**osVersion**</li><li>**eventType**</li><li>**datamarket**</li><li>**TipoDeDispositivo**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro *groupby* y en los siguientes:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>Puedes usar el parámetro *groupby* con *aggregationLevel*. Por ejemplo: * &amp; GroupBy = failureName, Market &amp; aggregationLevel = Week*</p></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener los datos del informe de errores. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo    | Descripción     |
|------------|---------|--------------|
| Value      | array   | Matriz de objetos que contienen los datos agregados del informe de errores. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de error](#error-values) que encontrarás a continuación.     |
| @nextLink  | string  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | integer | El número total de filas del resultado de datos de la consulta.     |


### <a name="error-values"></a>Valores de error

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value           | Tipo    | Descripción        |
|-----------------|---------|---------------------|
| date            | string  | La primera fecha del intervalo de fechas de los datos de error, en el formato ```yyyy-mm-dd``` . Si la solicitud especifica un solo día, este valor es esa fecha. Si la solicitud especifica un intervalo de fechas más largo, este valor es la primera fecha del intervalo de fechas. En el caso de las solicitudes que especifican un valor de *aggregationLevel* de **hora**, este valor también incluye un valor de hora en el formato ```hh:mm:ss``` .  |
| applicationId   | string  | El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos del error.   |
| applicationName | string  | El nombre para mostrar de la aplicación.   |
| failureName     | string  | El nombre del error, que se compone de cuatro partes: una o varias clases problemáticas, un código de comprobación de excepción/error, el nombre de la imagen en la que se produjo el error y el nombre de la función asociada.  |
| failureHash     | string  | El identificador único del error.   |
| símbolo          | string  | Símbolo que se asigna al error. |
| osVersion       | string  | Una de las siguientes cadenas que especifica la versión del sistema operativo en la que se produjo el error:<ul><li>**Windows Phone 7.5**</li><li>**Windows Phone 8**</li><li>**Windows Phone 8.1**</li><li>**Windows Phone 10**</li><li>**Windows 8**</li><li>**Windows 8.1**</li><li>**Windows 10**</li><li>**Unknown**</li></ul>  |
| osRelease       | string  |  Una de las siguientes cadenas que especifica la versión del sistema operativo o el anillo de vuelo (como un rellenado de la versión del sistema operativo) en el que se produjo el error.<p/><p>Para Windows 10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>VERSIÓN</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows 8.1:</p><ul><li><strong>Actualización 1</strong></li></ul><p>Para Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el anillo de vuelo, este campo tiene el valor <strong>desconocido</strong>.</p>    |
| eventType       | string  | Una de las cadenas siguientes:<ul><li>**experimentó**</li><li>**aplicaci**</li><li>**memoria**</li><li>**JSE**</li></ul>      |
| market          | string  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | string  | Una de las siguientes cadenas que indica el tipo de dispositivo en el que se produjo el error:<ul><li>**PC**</li><li>**Número**</li><li>**Consola: Xbox One**</li><li>**Consola: serie Xbox X**</li><li>**IoT**</li><li>**Holographic**</li><li>**Unknown**</li></ul>    |
| packageName     | string  | El nombre exclusivo del paquete de la aplicación asociado con el error.      |
| packageVersion  | string  | La versión del paquete de la aplicación asociado con el error.   |
| deviceCount     | number | Número de dispositivos únicos que corresponden a este error del nivel de agregación especificado.  |
| eventCount      | number | Número de eventos atribuidos a este error del nivel de agregación especificado.      |


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
* [Descargar el archivo CAB asociado a un error en la aplicación](download-the-cab-file-for-an-error-in-your-app.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener las valoraciones de la aplicación](get-app-ratings.md)
* [Obtener las opiniones de la aplicación](get-app-reviews.md)
