---
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados del informe de errores de un intervalo de fechas y otros filtros opcionales.
title: Obtener los datos del informe de la Xbox One de errores de juego
ms.date: 11/06/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Windows Store analytics API, API de análisis de Microsoft Store, errors, errores
ms.localizationpriority: medium
ms.openlocfilehash: f9ae7c75fb332e910aa1b63712cf0d230172afd3
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476377"
---
# <a name="get-error-reporting-data-for-your-xbox-one-game"></a>Obtener los datos del informe de la Xbox One de errores de juego

Usa este método en la API de análisis de Microsoft Store para obtener los datos de informes de error agregados para Xbox One juego integrado mediante el Portal de desarrollador de Xbox (XDP) y disponible en el panel del centro de partners de análisis de XDP.

Puedes recuperar información adicional sobre errores mediante los métodos de [obtener los detalles de un error en tu Xbox One, juego](get-details-for-an-error-in-your-xbox-one-game.md), [obtener el seguimiento de la pila de un error en tu Xbox One en juegos](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)y [descargar el archivo .cab para un error en tu juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md) .

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/failurehits``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El identificador de producto del juego de Xbox One para el que estás recuperando los datos de informes de errores. Para obtener el id. del producto de tu juego, ve a tu juego en el Portal de desarrollador de Xbox (XDP) y recupera el id. del producto desde la dirección URL. Como alternativa, si descargas los datos de estado desde el informe de análisis del centro de partners de Windows, el identificador de producto se incluye en el archivo TSV. |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. Si *aggregationLevel* es **day**, **week** o **month**, este parámetro debe especificar una fecha con el formato ```mm/dd/yyyy```. Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha con el formato ```mm/dd/yyyy``` o una fecha y hora con el formato ```yyyy-mm-dd hh:mm:ss```.  |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual. Si *aggregationLevel* es **day**, **week** o **month**, este parámetro debe especificar una fecha con el formato ```mm/dd/yyyy```. Si *aggregationLevel* es **hour**, este parámetro puede especificar una fecha con el formato ```mm/dd/yyyy``` o una fecha y hora con el formato ```yyyy-mm-dd hh:mm:ss```. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul> | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo para el que se han de recuperar los datos agregados. Puede ser una de las siguientes cadenas: **hour**, **day**, **week** o **month**. Si no se especifica, el valor predeterminado es **day**. Si especificas los valores **week** o **month**, los valores *failureName* y *failureHash* se limitarán a 1000 depósitos.<p/><p/>**Nota:**&nbsp;&nbsp;Si especificas **hour**, puedes recuperar datos de error solo de las últimas 72 horas. Para recuperar datos de error anteriores a 72 horas, especifica **day** o uno de los otros niveles de agregación.  | No |
| orderby | cadena | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es *orderby=field [order],field [order],...*. El parámetro *field* puede ser una de las siguientes cadenas:<ul><li>**applicationName**</li><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**osRelease**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li><li>**date**</li></ul><p>El parámetro *order* es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **asc**.</p><p>Aquí tienes un ejemplo de una cadena *orderby*: *orderby=date,market*</p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li>**failureName**</li><li>**failureHash**</li><li>**symbol**</li><li>**osVersion**</li><li>**eventType**</li><li>**market**</li><li>**deviceType**</li><li>**packageName**</li><li>**packageVersion**</li></ul><p>Las filas de datos que se devuelvan, contendrán los campos especificados en el parámetro *groupby* y en los siguientes:</p><ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**deviceCount**</li><li>**eventCount**</li></ul><p>Puedes usar el parámetro *groupby* con *aggregationLevel*. Por ejemplo: *&amp;groupby=failureName,market&amp;aggregationLevel=week*</p></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de informes de error en el juego Xbox One. Reemplaza el valor de *applicationId* con el identificador de producto para tu juego.

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
| Valor      | matriz   | Matriz de objetos que contienen los datos agregados del informe de errores. Para más información sobre los datos de cada objeto, consulta la sección [valores de error](#error-values) que encontrarás a continuación.     |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | entero | El número total de filas en el resultado de datos de la consulta.     |


### <a name="error-values"></a>Valores de error

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|---------------------|
| date            | cadena  | La primera fecha del intervalo de fechas de los datos del error con el formato ```yyyy-mm-dd```. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica un intervalo de fechas más largo, este valor será la primera fecha de ese intervalo de fechas. Para las solicitudes que especifican un valor de *aggregationLevel* de **hora**, este valor también incluye un valor de hora en el formato ```hh:mm:ss``` en la zona horaria local en el que se produjo el error.  |
| applicationId   | string  | El identificador de producto del juego de Xbox One para el que quieres recuperar los datos de errores.   |
| applicationName | cadena  | El nombre para mostrar del juego.   |
| failureName     | cadena  | El nombre del error, que se compone de cuatro partes: una o varias clases de problemas, un código de comprobación de errores o excepciones, el nombre de la imagen donde se produjo el error y el nombre de función asociada.  |
| failureHash     | cadena  | Identificador único del error.   |
| symbol          | cadena  | Símbolo que se asigna al error. |
| osVersion       | cadena  | Versión del sistema operativo en el que sucedió el error. Este es siempre el valor **de Windows 10**.  |
| osRelease       | cadena  |  Una de las siguientes cadenas que especifica la versión del sistema operativo de Windows 10 o (como una subpoblación dentro de la versión de sistema operativo) en el que se produjo el error.<p/><ul><li><strong>Versión 1507</strong></li><li><strong>Versión 1511</strong></li><li><strong>Versión 1607</strong></li><li><strong>Versión 1703</strong></li><li><strong>Versión 1709</strong></li><li><strong>Versión 1803</strong></li><li><strong>Vista previa de versión</strong></li><li><strong>Modo anticipado de Insider</strong></li><li><strong>Modo aplazado de Insider</strong></li></ul><p>Si se desconoce la versión del sistema operativo o el canal de actualizaciones, este campo tiene el valor <strong>Unknown</strong>.</p>    |
| eventType       | cadena  | Una de las cadenas siguientes:<ul><li>**bloquear**</li><li>**colgar**</li><li>**Error de memoria**</li></ul>      |
| market          | cadena  | El código de país ISO 3166 del mercado del dispositivo.   |
| deviceType      | cadena  | El tipo de dispositivo en el que se produjo el error. Este es siempre el valor de la **consola**.    |
| packageName     | cadena  | El paquete único nombre del juego que está asociado al error.      |
| packageVersion  | cadena  | La versión del paquete de juego que está asociado al error.   |
| deviceCount     | entero | Número de dispositivos únicos que corresponden a este error del nivel de agregación especificado.  |
| eventCount      | entero | El número de eventos atribuidos a este error del nivel de agregación especificado.      |


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

* [Obtener los detalles de un error en tu Xbox One juego](get-details-for-an-error-in-your-xbox-one-game.md)
* [Obtener el seguimiento de la pila de un error en tu Xbox One juego](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)
* [Descargar el archivo .cab para un error en tu juego de Xbox One](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
