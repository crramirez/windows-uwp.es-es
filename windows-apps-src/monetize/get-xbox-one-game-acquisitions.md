---
author: Xansky
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos de adquisición agregados de un juego de Xbox One durante un intervalo de fechas especificado y otros filtros opcionales.
title: Obtener adquisiciones de juegos de Xbox One
ms.author: mhopkins
ms.date: 10/18/2018
ms.topic: article
keywords: windows 10, uwp, servicios de Microsoft Store, Store services, API de análisis de Microsoft Store, Windows Store analytics API, adquisiciones de juegos de Xbox One, Xbox One game acquisitions
ms.localizationpriority: medium
ms.openlocfilehash: 200f18e443e8a130a7e5c673b03c146b73c9083b
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7291855"
---
# <a name="get-xbox-one-game-acquisitions"></a>Obtener adquisiciones de juegos de Xbox One

Usa este método en la Microsoft Store analytics API para obtener datos de adquisición agregados en formato JSON de una consola Xbox One juego que se ha integrado mediante el Portal de desarrollador de Xbox (XDP) y está disponible en el panel de análisis de XDP.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | cadena | Identificador de producto del juego para Xbox One para el que recuperas los datos de adquisición. Para obtener el identificador de producto de tu juego, ve a tu juego en el programa de análisis de XDP y recuperar el identificador de producto de la dirección URL. Como alternativa, si descargas los datos de adquisición del informe de análisis del centro de partners, el identificador de producto se incluye en el archivo TSV.  |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de compra que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos de compra que se han de recuperar. El valor predeterminado es la fecha actual. |  No  |
| top | int | Número de filas de datos que se van a devolver. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | cadena  | Una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *filter=market eq 'US' and gender eq 'm'*. <p/><p/>Puedes especificar los campos siguientes del cuerpo de respuesta:<p/><ul><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>sandboxId</strong></li></ul> | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | cadena | Instrucción que ordena los valores de datos resultantes de cada compra. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>fecha</strong></li><li><strong>acquisitionType</strong></li><li><strong>age</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>El parámetro <em>order</em> es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>paymentInstrumentType</strong></li><li><strong>sandboxId</strong></li><li><strong>xboxTitleIdHex</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener los datos de adquisición de juegos para Xbox One. Reemplaza el valor de *applicationId* con el identificador de producto para tu juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/xbox/acquisitions?applicationId=BRRT4NJ9B3D1&startDate=8/1/2017&endDate=8/31/2017&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Una matriz de objetos que contienen los datos de adquisición agregados del juego. Para más información sobre los datos de cada objeto, consulta la sección [valores de adquisición](#acquisition-values) que encontrarás a continuación.                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra de la solicitud. |
| TotalCount | entero    | Número total de filas en el resultado de datos de la consulta.              |


### <a name="acquisition-values"></a>Valores de compra

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| date                | cadena | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId       | cadena | Identificador de producto del juego para Xbox One para el que recuperas los datos de adquisición. |
| applicationName     | cadena | El nombre para mostrar del juego.       |
| acquisitionType     | cadena | Una de las siguientes cadenas que indica el tipo de adquisición:<ul><li><strong>Free</strong></li><li><strong>Prueba</strong></li><li><strong>De pago</strong></li><li><strong>Código promocional</strong></li><li><strong>Iap</strong></li><li><strong>Suscripción Iap</strong></li><li><strong>Audiencia privada</strong></li><li><strong>Pedido previo</strong></li><li><strong>Xbox Game Pass</strong> (o <strong>Game Pass</strong> si la consulta de datos es antes del 23 de marzo de 2018)</li><li><strong>Disco</strong></li><li><strong>Código de prepago</strong></li><li><strong>Orden de Pre cargados</strong></li><li><strong>Orden de Pre cancelada</strong></li><li><strong>Orden de Pre con errores</strong></li></ul>    |
| age                 | cadena | Una de las cadenas siguientes que indica el grupo de edad del usuario que ha realizado la adquisición:<ul><li><strong>menor que 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>mayor que 55</strong></li><li><strong>Unknown</strong></li></ul>     |
| deviceType          | cadena | Una de las cadenas siguientes que especifica el tipo de dispositivo que ha completado la adquisición:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Server</strong></li><li><strong>Tablet</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>  |
| gender              | cadena | Una de las cadenas siguientes que especifica el sexo del usuario que ha realizado la adquisición:<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul>     |
| market              | cadena | Código de país ISO 3166 del mercado donde se realizó la adquisición.  |
| osVersion           | cadena | Versión del sistema operativo en el que se realizó la adquisición. Para este método, este valor es siempre **Windows 10**.</li></ul>    |
| paymentInstrumentType           | cadena | Una de las cadenas siguientes que indica la instrucción de pago utilizada para la adquisición:<ul><li><strong>Tarjeta de crédito</strong></li><li><strong>Tarjeta de débito directo</strong></li><li><strong>Compra deducida</strong></li><li><strong>Balance de situación</strong></li><li><strong>Operador móvil</strong></li><li><strong>Transferencia bancaria en línea</strong></li><li><strong>PayPal</strong></li><li><strong>Transacción dividida</strong></li><li><strong>Canje de token</strong></li><li><strong>Importe cero pagado</strong></li><li><strong>eWallet</strong></li><li><strong>Unknown</strong></li></ul>    |
| sandboxId              | cadena | El id. de espacio aislado creado para el juego. Este puede ser el valor **RETAIL** o el Id. de un espacio aislado privado.  |
| storeClient         | cadena | Una de las cadenas siguientes que indica la versión de la Store donde se produce la adquisición:<ul><li>**Tienda de Windows Phone (cliente)**</li><li>**Microsoft Store (cliente)** (o **Tienda de Windows (cliente)** si la consulta de datos es anterior al 23 de marzo de 2018)</li><li>**Microsoft Store (web)** (o **Tienda de Windows (web)** si la consulta de datos es anterior al 23 de marzo de 2018)</li><li>**Compras por volumen de empresas**</li><li>**Otras**</li></ul>                             |
| xboxTitleIdHex              | cadena | Id. de título de Xbox Live (representado en valor hexadecimal) asignado por el Portal de desarrollador de Xbox (XDP) para juegos habilitados para Xbox Live.  |
| acquisitionQuantity | número | Número de adquisiciones que se realizaron durante el nivel de agregación especificado.     |
| purchasePriceUSDAmount | número | El importe pagado por el cliente para la adquisición, convertido en USD, con el tipo de cambio mensual.    |
| taxUSDAmount     | número | El importe de impuestos aplicado a la adquisición,convertido en USD. |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2017-02-01",
      "applicationId": "BRRT4NJ9B3D1 ",
      "applicationName": "Contoso Game",
      "acquisitionType": "Paid",
      "age": "35-49",
      "deviceType": "Console",
      "gender": "m",
      "market": "US",
      "osVersion": "Windows 10",
      "PaymentInstrumentType": "Credit Card ",
      "sandboxId": "RETAIL",
      "storeClient": "Windows Store (web)",
      "xboxTitleIdHex": "",
      "acquisitionQuantity": 1,
      "purchasePriceUSDAmount": 29.99,
      "taxUSDAmount": 2.99
    }
  ],
  "@nextLink": "xbox/acquisitions?applicationId=BRRT4NJ9B3D1&aggregationLevel=day&startDate=2017/02/01&endDate=2017/03/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 39812
}
```

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
