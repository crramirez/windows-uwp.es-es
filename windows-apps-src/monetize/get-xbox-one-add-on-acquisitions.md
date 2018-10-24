---
author: Xansky
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos de compra agregados del complemento.
title: Obtener adquisiciones de complementos de Xbox One
ms.author: mhopkins
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, servicios de Store, Microsoft Store analytics API, adquisiciones de complementos de Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 931cd7b351a122c22a59a3a0bc2975c61dc38aaa
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5478101"
---
# <a name="get-xbox-one-add-on-acquisitions"></a>Obtener adquisiciones de complementos de Xbox One

Usa este método en la Microsoft Store analytics API para obtener los datos de compra agregados del complemento en formato JSON de una consola Xbox One juego que se ha integrado mediante el Portal de desarrollador de Xbox (XDP) y está disponible en el panel del centro de partners de análisis de XDP.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción          |
|---------------|--------|--------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

El parámetro *applicationId* o *addonProductId* es necesario. Para recuperar los datos de compra de todos los complementos registrados en la aplicación, especifica el parámetro *applicationId*. Para recuperar los datos de compra de un solo complemento, especifica el parámetro *addonProductId* . Si especificas ambos, el parámetro *applicationId* se ignorará.

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  |
|---------------|--------|---------------|------|
| applicationId | string | El *valor de productId* del juego de Xbox One para el que estás recuperando los datos de compra. Para obtener el *valor de productId* de tu juego, ve a tu juego en el programa de análisis de XDP y recuperar el *valor de productId* desde la dirección URL. Como alternativa, si descargas los datos de adquisiciones desde el informe de análisis del centro de partners, el *valor de productId* se incluye en el archivo TSV. |  Sí  |
| addonProductId | string | El *valor de productId* del complemento para el que quieres recuperar los datos de compra.  | Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de compra del complemento que se recuperarán. El valor predeterminado es la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos de compra del complemento que recuperarán. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | <p>Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores eq o ne; asimismo, puedes combinar las instrucciones mediante and u or. Ten en cuenta que en el parámetro filter los valores de la cadena deben estar entre comillas simples. Por ejemplo, filter=market eq 'US' and gender eq 'm'.</p> <p>Puedes especificar los campos siguientes del cuerpo de respuesta:</p> <ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul>| No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | cadena | Instrucción que ordena los valores de datos resultantes de cada compra de complemento. La sintaxis es <em>orderby = field [order], [order], campo …</em> El parámetro de <em>campo</em> puede ser una de las siguientes cadenas:<ul><li><strong>fecha</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>addonProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>addonProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em> &amp;groupby = edad, mercado&amp;aggregationLevel = week</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En los ejemplos siguientes se muestran varias solicitudes para obtener datos de compra de complementos. Reemplaza los valores *addonProductId* y *applicationId* con el identificador de la tienda adecuado para tu complemento o aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/addonacquisitions?addonProductId=BRRT4NJ9B3D2&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and age ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción         |
|------------|--------|------------------|
| Valor      | matriz  | Matriz de objetos que contienen los datos de compra agregados del complemento. Para más información sobre los datos de cada objeto, consulta la sección [valores de compra del complemento](#add-on-acquisition-values) que encontrarás a continuación.                                                                                                              |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero hay más de 10000 filas de datos de compra de complementos para la solicitud. |
| TotalCount | entero    | Número total de filas del resultado de datos de la consulta.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valores de compra del complemento

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo    | Descripción        |
|---------------------|---------|---------------------|
| date                | cadena  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| addonProductId      | string  | El *valor de productId* del complemento para el que estás recuperando los datos de compra.                                                                                                                                                                 |
| addonProductName    | cadena  | Nombre del complemento que quieres que se muestre. Este valor solo aparece en los datos de respuesta si el parámetro *aggregationLevel* se establece en **día**, a menos que especifiques el campo **addonProductName** en el parámetro *groupby* .                                                                                                                                                                                                            |
| applicationId       | string  | El *valor de productId* de la aplicación para la que quieres recuperar los datos de compra de complementos.                                                                                                                                                           |
| applicationName     | cadena  | El nombre para mostrar del juego.                                                                                                                                                                                                             |
| deviceType          | cadena  | <p>Una de las cadenas siguientes que especifica el tipo de dispositivo que ha completado la adquisición:</p> <ul><li>"EQUIPO"</li><li>"Teléfono"</li><li>"Consola"</li><li>"IoT"</li><li>"Server"</li><li>"Tableta"</li><li>"Holographic"</li><li>"Desconocido"</li></ul>                                                                                                  |
| storeClient         | cadena  | <p>Una de las cadenas siguientes que indica la versión de la Store donde se produce la adquisición:</p> <ul><li>"Windows Phone Store (cliente)"</li><li>"Microsoft Store (cliente)" (o "Windows Store (cliente)" si la consulta de datos del 23 de marzo de 2018)</li><li>"Microsoft Store (web)" (o "Windows Store (web)" si la consulta de datos del 23 de marzo de 2018)</li><li>"Compras por volumen de las organizaciones"</li><li>"Otros"</li></ul>                                                                                            |
| osVersion           | cadena  | Versión del sistema operativo en el que se realizó la compra. Este método, este valor es siempre "Windows 10".                                                                                                   |
| market              | cadena  | Código de país ISO 3166 del mercado donde se realizó la compra.                                                                                                                                                                  |
| gender              | cadena  | <p>Una de las cadenas siguientes que especifica el sexo del usuario que ha realizado la adquisición:</p> <ul><li>"m"</li><li>"f"</li><li>"Desconocido"</li></ul>                                                                                                    |
| age            | cadena  | <p>Una de las cadenas siguientes que indica el grupo de edad del usuario que ha realizado la adquisición:</p> <ul><li>"menor que 13"</li><li>"13-17"</li><li>"18 a 24"</li><li>"25-34"</li><li>"35-44"</li><li>"44: 55"</li><li>"mayor que 55"</li><li>"Desconocido"</li></ul>                                                                                                 |
| acquisitionType     | cadena  | <p>Una de las siguientes cadenas que indica el tipo de adquisición:</p> <ul><li>"Gratuito"</li><li>"Evaluación"</li><li>"De pago"</li><li>"Código promocional"</li><li>"Iap"</li><li>"Suscripción Iap"</li><li>"Audiencia privada"</li><li>"Pre orden"</li><li>"Xbox Game Pass" (o "Game Pass" si la consulta de datos del 23 de marzo de 2018)</li><li>"Disco"</li><li>"Código de prepago"</li><li>"El cargo Pre orden"</li><li>"Cancelado Pre orden"</li><li>"No se pudo Pre orden"</li></ul>                                                                                                    |
| acquisitionQuantity | entero | Número de compras que se realizaron.                        |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2018-10-18",
      "addonProductId": " BRRT4NJ9B3D2",
      "addonProductName": "Contoso add-on 7",
      "applicationId": "BRRT4NJ9B3D1",
      "applicationName": "Contoso Demo",
      "deviceType": "Console",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows 10",
      "market": "GB",
      "gender": "m",
      "age": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "addonacquisitions?applicationId=BRRT4NJ9B3D1&addonProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```
