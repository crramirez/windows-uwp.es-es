---
author: mcleanbyron
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados del informe de errores de una aplicación de escritorio para un intervalo de fechas y otros filtros opcionales.
title: Obtener datos de informes de errores para la aplicación de escritorio
ms.author: mcleans
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis Microsoft Store, errors, errores, desktop application, aplicación de escritorio
ms.localizationpriority: medium
ms.openlocfilehash: 71c566ff375f36108d724f3c550570b3332f4c6b
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867694"
---
# <a name="get-error-reporting-data-for-your-desktop-application"></a>Obtener datos de informes de errores para la aplicación de escritorio

Usa este método en la API de análisis de Microsoft Store para obtener datos de informes de error adicionales de una aplicación de escritorio que agregaste al [Programa de aplicaciones de escritorio de Windows](https://msdn.microsoft.com/library/windows/desktop/mt826504). Este método sólo puede recuperar los errores que se produjeron en los últimos 30 días. Esta información también está disponible en el [informe Mantenimiento](https://msdn.microsoft.com/library/windows/desktop/mt826504) para aplicaciones de escritorio del panel del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId | cadena | El id. del producto de la aplicación de escritorio de la cual quieres recuperar los datos del informe de errores. Para obtener el id. del producto de una aplicación de escritorio, abra cualquier [informe de análisis del Centro de desarrollo de tu aplicación de escritorio](https://msdn.microsoft.com/library/windows/desktop/mt826504) (como el **Informe de estado**) y recuperara el id. del producto desde la dirección URL. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar con el formato ```mm/dd/yyyy```. El valor siempre es la fecha actual.<p/><p/>**Nota:**&nbsp;&nbsp;este método sólo puede recuperar los errores que se produjeron en los últimos 30 días.  |  No  |
| endDate | fecha | La fecha final del intervalo de fechas de los datos del informe de errores que se han de recuperar con el formato ```mm/dd/yyyy```. El valor siempre es la fecha actual.   |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>fecha</strong></li></ul> | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo para el que se han de recuperar los datos agregados. Puede ser una de las siguientes cadenas: **día**, **semana** o **mes**. Si no se especifica, el valor predeterminado es **día**. Si especificas los valores **semana** o **mes**, los valores *failureName* y *failureHash* se limitarán a 1000 depósitos.<p/>  | No |
| orderby | cadena | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es *orderby=field [order],field [order],...*. El parámetro *field* puede ser una de las siguientes cadenas:<ul><li><strong>fileName</strong></li><li><strong>applicationVersion</strong></li><li><strong>failureName</strong></li><li><strong>failureHash</strong></li><li><strong>symbol</strong></li><li><strong>osVersion</strong></li><li><strong>osBuild</strong></li><li><strong>osRelease</strong></li><li><strong>eventType</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>productName</strong></li><li><strong>fecha</strong></li></ul>El parámetro *order* es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **asc**.</p><p>Aquí tienes un ejemplo de una cadena *orderby*: *orderby=date,market*</p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li></ul><p>Las filas de datos devueltas contendrán los campos especificados en el parámetro *groupby* y en los siguientes:</p><ul><li>**fecha**</li><li>**applicationId**</li><li>**applicationName**</li><li>**eventCount**</li></ul><p>Puedes usar el parámetro *groupby* con *aggregationLevel*. Por ejemplo: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener los datos del informe de errores. Reemplaza el valor *applicationId* por el identificador de producto de la aplicación de escritorio.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=1/1/2018&endDate=2/1/2018&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/failurehits?applicationId=10238467886765136388&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
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
| fecha            | cadena  | La primera fecha del intervalo de fechas de los datos del error con el formato ```yyyy-mm-dd```. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica un intervalo de fechas más largo, este valor será la primera fecha de ese intervalo de fechas. Para las solicitudes que especifican un valor *aggregationLevel* de **hora**, este valor también incluye un valor de hora con el formato ```hh:mm:ss```.  |
| applicationId   | cadena  | El identificador de producto de la aplicación de escritorio de la cual recuperaste los datos de errores.    |
| productName | cadena  | El nombre de la aplicación de escritorio derivado de los metadatos de su(s) ejecutable(s) asociado(s).   |
| appName | cadena  |  TBD  |
| fileName | cadena  | El nombre del archivo ejecutable de la aplicación de escritorio.   |
| failureName     | cadena  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen donde se produjo el error y el nombre de función asociada.  |
| failureHash     | cadena  | Identificador único del error.   |
| symbol          | cadena  | Símbolo que se asigna al error. |
| osBuild       | cadena  | El número de compilación de cuatro partes del sistema operativo en el que se produjo el error.  |
| osVersion       | cadena  | Una de las cadenas siguientes que especifica la versión del sistema operativo en la que se realizó la instalación de la aplicación de escritorio:<p/><ul><li><strong>Windows7</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>WindowsServer2016</strong></li><li><strong>WindowsServer1709</strong></li><li><strong>Unknown</strong></li></ul>   |   
| osRelease | cadena  | Una de las siguientes cadenas que especifica la versión del sistema operativo o el canal de actualizaciones (como una subpoblación dentro de la versión del sistema operativo) en el que se ha producido el error.<p/><p>Para Windows10:</p><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Vista previa de versión</strong></li><li><strong>Modo anticipado de Insider</strong></li><li><strong>Modo aplazado de Insider</strong></li></ul><p/><p>Para Windows Server 1709:</p><ul><li><strong>RTM</strong></li></ul><p>Para Windows Server 2016:</p><ul><li><strong>Versión 1607</strong></li></ul><p>Para Windows8.1:</p><ul><li><strong>Update 1</strong></li></ul><p>Para Windows7:</p><ul><li><strong>Service Pack 1</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p> |
| eventType       | cadena  | Una de las siguientes cadenas que indica el tipo de evento de error:<ul><li>**crash**</li><li>**colgar**</li><li>**memoria**</li><li>**jse**</li></ul>       |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | cadena  | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo el error:<p/><ul><li><strong>PC</strong></li><li><strong>Servidor</strong></li><li><strong>Tableta</strong></li><li><strong>Unknown</strong></li></ul>    |
| applicationVersion     | cadena  |   La versión de la aplicación ejecutable en la que se produjo el error.    |
| eventCount      | entero | El número de eventos atribuidos a este error del nivel de agregación especificado.      |


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
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener los detalles de un error en la aplicación de escritorio](get-details-for-an-error-in-your-desktop-application.md)
* [Obtener el seguimiento de la pila de un error en la aplicación de escritorio](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
* [Descargar el archivo .cab para un error en tu aplicación de escritorio](download-the-cab-file-for-an-error-in-your-desktop-application.md)
