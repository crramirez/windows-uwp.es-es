---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de informes de errores agregados para una aplicación de escritorio para un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener datos de informes de errores de la aplicación de escritorio
ms.date: 09/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, errores, aplicación de escritorio
ms.localizationpriority: medium
ms.openlocfilehash: bb9c0bd3162f603bd9965a98c5e75bad5aae9c54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167599"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>Obtener datos de informes de errores de la aplicación de escritorio

Use este método en la API de Microsoft Store Analytics para obtener datos de informes de errores agregados para una aplicación de escritorio que haya agregado al [programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program). Este método solo puede recuperar los errores que se produjeron en los últimos 30 días. Esta información también está disponible en el [Informe de mantenimiento](/windows/desktop/appxpkg/windows-desktop-application-program) de las aplicaciones de escritorio del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El ID. de producto de la aplicación de escritorio para la que desea recuperar datos de informes de errores. Para obtener el identificador de producto de una aplicación de escritorio, abra el [Informe de análisis de la aplicación de escritorio en el centro de Partners](/windows/desktop/appxpkg/windows-desktop-application-program) (como el **Informe de mantenimiento**) y recupere el identificador de producto de la dirección URL. |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de los datos de informes de errores que se van a recuperar, en el formato ```mm/dd/yyyy``` . La fecha predeterminada es la actual.<p/><p/>**Nota:** &nbsp; &nbsp; Este método solo puede recuperar los errores que se produjeron en los últimos 30 días.  |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de informes de errores que se van a recuperar, en el formato ```mm/dd/yyyy``` . La fecha predeterminada es la actual.   |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>símbolo</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>datamarket</strong></li><li><strong>deviceType</strong></li><li><strong>NombreDeProducto</strong></li><li><strong>date</strong></li></ul> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **día**, **semana** o **mes**. Si no se especifica, el valor predeterminado es **día**. Si especificas los valores **semana** o **mes**, los valores *failureName* y *failureHash* se limitarán a 1000 depósitos.<p/>  | No |
| orderby | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es *OrderBy = Field [order], Field [order],...*. El parámetro de *campo* puede ser una de las cadenas siguientes:<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>símbolo</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>datamarket</strong></li><li><strong>deviceType</strong></li><li><strong>NombreDeProducto</strong></li><li><strong>date</strong></li></ul>El parámetro *order*, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **ASC**.</p><p>Este es un ejemplo de cadena *OrderBy* : *OrderBy = Date, Market*</p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li>**failureName**</li><li>**failureHash**</li><li>**símbolo**</li><li>**osVersion**</li><li>**eventType**</li><li>**datamarket**</li><li>**deviceType**</li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro *groupby* y en los siguientes:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>Puedes usar el parámetro *groupby* con *aggregationLevel*. Por ejemplo: * &amp; GroupBy = failureName, Market &amp; aggregationLevel = Week*</p></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener los datos del informe de errores. Reemplace el valor de *ApplicationID* por el ID. de producto de la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
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
| applicationId   | string  | El ID. del producto de la aplicación de escritorio para la que recuperó los datos de error.    |
| ProductName | string  | Nombre para mostrar de la aplicación de escritorio que se deriva de los metadatos de los archivos ejecutables asociados.   |
| appName | string  |  TBD  |
| fileName | string  | Nombre del archivo ejecutable de la aplicación de escritorio.   |
| failureName     | string  | El nombre del error, que se compone de cuatro partes: una o varias clases problemáticas, un código de comprobación de excepción/error, el nombre de la imagen en la que se produjo el error y el nombre de la función asociada.  |
| failureHash     | string  | El identificador único del error.   |
| símbolo          | string  | Símbolo que se asigna al error. |
| osBuild       | string  | Número de compilación de cuatro partes del sistema operativo en el que se produjo el error.  |
| osVersion       | string  | Una de las siguientes cadenas que especifica la versión del sistema operativo en la que se instala la aplicación de escritorio:<p/><ul><li><strong>Windows 7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Windows Server 2016</strong></li><li><strong>Windows Server 1709</strong></li><li><strong>Unknown</strong></li></ul>   |   
| osRelease | string  | Una de las siguientes cadenas que especifica la versión del sistema operativo o el anillo de vuelo (como un rellenado de la versión del sistema operativo) en el que se produjo el error.<p/><p>Para Windows 10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lento</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>VERSIÓN</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows 8.1:</p><ul><li><strong>Actualización 1</strong></li></ul><p>Para Windows 7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el anillo de vuelo, este campo tiene el valor <strong>desconocido</strong>.</p> |
| eventType       | string  | Una de las siguientes cadenas que indica el tipo de evento de error:<ul><li>**experimentó**</li><li>**aplicaci**</li><li>**memoria**</li><li>**JSE**</li></ul>       |
| market          | string  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | string  | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo el error:<p/><ul><li><strong>PC</strong></li><li><strong>Server</strong></li><li><strong>Tableta</strong></li><li><strong>Unknown</strong></li></ul>    |
| applicationVersion     | string  |   Versión del archivo ejecutable de la aplicación en el que se produjo el error.    |
| eventCount      | number | Número de eventos atribuidos a este error del nivel de agregación especificado.      |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2018-02-01",
      "applicationId": "10238467886765136388",
      "productName": "Contoso Demo",
      "appName": "Contoso Demo",
      "fileName": "contosodemo.exe",
      "failureName": "SVCHOSTGROUP_localservice_IN_PAGE_ERROR_c0000006_hardware_disk!Unknown",
      "failureHash": "11242ef3-ebd8-d525-838d-b5497b225695",
      "symbol": "hardware_disk!Unknown",
      "osBuild": "10.0.15063.850",
      "osVersion": "Windows 10",
      "osRelease": "Version 1703",
      "eventType": "crash",
      "market": "US",
      "deviceType": "PC",
      "applicationVersion": "2.2.2.0",
      "eventCount": 0.0012422360248447205
    }
  ],
  "@nextLink": "desktop/failurehits?applicationId=10238467886765136388&aggregationLevel=week&startDate=2018/02/01&endDate2018/02/08&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Temas relacionados

* [Informe de estado](../publish/health-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los detalles asociados a un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)
* [Obtener el seguimiento de pila asociado a un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Descargar el archivo CAB asociado a un error en la aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)