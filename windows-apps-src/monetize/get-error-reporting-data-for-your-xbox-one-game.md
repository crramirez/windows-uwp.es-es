---
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados del informe de errores de un intervalo de fechas y otros filtros opcionales.
title: Obtener datos de informes de errores en tu juego de Xbox One
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store analytics API, API de análisis de la Store Windows, errors, errores
ms.localizationpriority: medium
ms.openlocfilehash: 22dff391e787e1763cb730272ba9cea029758c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662080"
---
# <a name="get-error-reporting-data-for-your-xbox-one-game"></a>Obtener datos de informes de errores en tu juego de Xbox One

Utilice este método en la API de análisis de Microsoft Store para obtener datos de informes de errores agregados para Xbox One juegos que estaba introducidos mediante el Portal para desarrolladores de Xbox (XDP) y está disponible en el panel del centro de partners XDP Analytics.

Puede recuperar información de error adicional mediante la [obtener los detalles de un error en Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md), [obtener el seguimiento de pila para un error en Xbox One juego](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md), y [descargar el archivo .cab para un error en el juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md) métodos.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El **Store ID** del juego de Xbox One que va a recuperar el error de datos de informes. El **Store ID** está disponible en la página de la identidad de aplicación en el centro de partners. Un ejemplo **Store ID** es 9WZDNCRFJ3Q8. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. Si *aggregationLevel* es **day**, **week** o **month**, este parámetro debe especificar una fecha con el formato ```mm/dd/yyyy```. Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha con el formato ```mm/dd/yyyy``` o una fecha y hora con el formato ```yyyy-mm-dd hh:mm:ss```.  |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. Si *aggregationLevel* es **day**, **week** o **month**, este parámetro debe especificar una fecha con el formato ```mm/dd/yyyy```. Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha con el formato ```mm/dd/yyyy``` o una fecha y hora con el formato ```yyyy-mm-dd hh:mm:ss```. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **hour**, **day**, **week** o **month**. Si no se especifica, el valor predeterminado es **día**. Si especificas los valores **semana** o **mes**, los valores *failureName* y *failureHash* se limitarán a 1000 depósitos.<p/><p/>**Nota:**&nbsp;&nbsp;Si especificas **hour**, puedes recuperar datos de error solo de las últimas 72 horas. Para recuperar datos de error anteriores a 72 horas, especifica **day** o uno de los otros niveles de agregación.  | No |
| orderby | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es *orderby=field [order],field [order],...*. El parámetro *field* puede ser una de las siguientes cadenas:<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>El parámetro *order*, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **asc**.</p><p>Aquí tienes un ejemplo de una cadena *orderby*: *orderby=date,market*</p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro *groupby* y en los siguientes:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>Puedes usar el parámetro *groupby* con *aggregationLevel*. Por ejemplo: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de informes de error de juegos Xbox One. Reemplace el *applicationId* valor con el **Store ID** para su juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits?applicationId=BRRT4NJ9B3D1&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'Console' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo    | Descripción     |
|------------|---------|--------------|
| Valor      | array   | Matriz de objetos que contienen los datos agregados del informe de errores. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de error](#error-values) que encontrarás a continuación.     |
| @nextLink  | string  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | número entero | El número total de filas del resultado de datos de la consulta.     |


### <a name="error-values"></a>Valores de error

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|---------------------|
| fecha            | string  | La primera fecha del intervalo de fechas de los datos del error con el formato ```yyyy-mm-dd```. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica un intervalo de fechas más largo, este valor será la primera fecha de ese intervalo de fechas. Para las solicitudes que especifican un *aggregationLevel* valor **hora**, este valor también incluye un valor de hora con el formato ```hh:mm:ss``` en la zona horaria local en el que se produjo el error.  |
| applicationId   | string  | El **Store ID** del juego de Xbox One que desea recuperar los datos de error.   |
| applicationName | string  | El nombre para mostrar del juego.   |
| failureName     | string  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen donde se produjo el error y el nombre de función asociada.  |
| failureHash     | string  | El identificador único del error.   |
| symbol          | string  | Símbolo que se asigna al error. |
| osVersion       | string  | La versión del sistema operativo en el que se ha producido el error. Esto es siempre el valor **Windows 10**.  |
| osRelease       | string  |  Una de las siguientes cadenas que especifica la versión de sistema operativo de Windows 10 o anillo de distribución de paquetes piloto (como un subpoblación dentro de la versión del sistema operativo) en que se ha producido el error.<p/><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Versión preliminar</strong></li><li><strong>Insider rápido</strong></li><li><strong>Insider lenta</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p>    |
| eventType       | string  | Una de las cadenas siguientes:<ul><li>**crash**</li><li>**hang**</li><li>**Error de memoria**</li></ul>      |
| market          | string  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | string  | El tipo de dispositivo en el que se ha producido el error. Esto es siempre el valor **consola**.    |
| packageName     | string  | El paquete único nombre del juego que está asociado a este error.      |
| packageVersion  | string  | La versión del paquete de juego que está asociado a este error.   |
| deviceCount     | número entero | Número de dispositivos únicos que corresponden a este error del nivel de agregación especificado.  |
| eventCount      | número entero | Número de eventos atribuidos a este error del nivel de agregación especificado.      |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2017-03-09",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Sports",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows 10",
      "osRelease": "Version 1803",
      "eventType": "crash",
      "market": "US",
      "deviceType": "Console",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=BRRT4NJ9B3D1&aggregationLevel=week&startDate=2017/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 21
}

```

## <a name="related-topics"></a>Temas relacionados

* [Obtener los detalles de un error en Xbox One juegos](get-details-for-an-error-in-your-xbox-one-game.md)
* [Obtener el seguimiento de pila para un error en Xbox One juegos](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Descargue el archivo CAB para un error en el juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
