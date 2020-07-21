---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición agregados en formato JSON para aplicaciones UWP y juegos de Xbox One que se ingeriron a través del portal para desarrolladores de Xbox (XDP) y que están disponibles en el panel de XDP Analytics.
title: Obtener datos de adquisiciones para sus aplicaciones y juegos
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, red de publicidad, metadatos de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: f7db2659bc08ab6da426de94c7dcaf1fb8a7d802
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493230"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>Obtener datos de adquisiciones para sus aplicaciones y juegos 
Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición agregados en formato JSON para aplicaciones UWP y juegos de Xbox One que se ingeriron a través del portal para desarrolladores de Xbox (XDP) y que están disponibles en el panel de XDP Analytics. 

> [!NOTE]
> Esta API no proporciona datos agregados diarios antes del 1 de octubre de 2016. 

## <a name="prerequisites"></a>Requisitos previos
Para usar este método, primero debes hacer lo siguiente: 

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md) de la API de Microsoft Store Analytics. 
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo. 

## <a name="request"></a>Solicitud
### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>Encabezado de solicitud

| Encabezado | Tipo | Descripción |
| --- | --- | --- |
| Authorization | string | Necesario. El token de acceso Azure AD en el **portador** del formulario `<token>` . |

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro | Tipo | Descripción | Requerido |
| --- | --- | --- | --- |
| applicationId | string | El identificador de producto del juego Xbox One para el que se recuperan los datos de adquisición. Para obtener el identificador de producto del juego, navegue hasta el juego en el programa XDP Analytics y recupere el identificador de producto de la dirección URL. Como alternativa, si descarga los datos de adquisiciones del informe del centro de Partners, el ID. del producto se incluye en el archivo. TSV.  | Sí |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de compra que se han de recuperar. La fecha predeterminada es la actual.  | No |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de compra que se han de recuperar. La fecha predeterminada es la actual.  | No |
| top | integer | Número de filas de datos que se van a devolver. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.  | No |
| skip | integer | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, *Top = 10000 y SKIP = 0* recupera las primeras 10000 filas de datos, *Top = 10000 y SKIP = 10000* recupera las siguientes 10000 filas de datos, y así sucesivamente.  | No |
| filter | string | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro filter los valores de la cadena deben estar entre comillas simples. Por ejemplo, *filtro = Market EQ ' EE. UU. ' y sexo EQ ' m '*.  <br/> Puede especificar los siguientes campos del cuerpo de la respuesta: <ul><li>**acquisitionType**</li><li>**antig**</li><li>**storeClient**</li><li>**sexo**</li><li>**datamarket**</li><li>**osVersion**</li><li>**TipoDeDispositivo**</li><li>**sandboxId**</li></ul> | No |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **día**, **semana** o **mes**. Si no se especifica, el valor predeterminado es **día**.  | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada compra. La sintaxis es *OrderBy = Field [order], Field [order],...* El parámetro de *campo* puede ser una de las cadenas siguientes: <ul><li>**date**</li><li>**acquisitionType**</li><li>**antig**</li><li>**storeClient**</li><li>**sexo**</li><li>**datamarket**</li><li>**osVersion**</li><li>**TipoDeDispositivo**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> El parámetro *order*, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **ASC**. Este es un ejemplo de cadena *OrderBy* : *OrderBy = Date, Market*  | No |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos: <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**sexo**</li><li>**datamarket**</li><li>**osVersion**</li><li>**TipoDeDispositivo**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro *groupby* y en los siguientes: <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> Puedes usar el parámetro *groupby* con aggregationLevel. Por ejemplo: *&GroupBy = edad, market&aggregationLevel = Week*  | No |

### <a name="request-example"></a>Ejemplo de solicitud
En el ejemplo siguiente se muestran varias solicitudes para obtener datos de adquisición de juegos de Xbox One. Reemplace el valor de *ApplicationID* por el identificador de producto del juego.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>Response

### <a name="response-body"></a>Cuerpo de la respuesta
| Value | Tipo | Descripción |
| --- | --- | --- |
| Value | array | Matriz de objetos que contienen datos de adquisición agregados para el juego. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra](#acquisition-values) que encontrarás a continuación. |
| @nextLink | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra de la solicitud. |
| TotalCount | integer | El número total de filas del resultado de datos de la consulta. |

### <a name="acquisition-values"></a>Valores de compra 
Los elementos de la matriz *Value* contienen los siguientes valores. 

| Value | Tipo | Descripción |
| --- | --- | --- |
| date | string | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId | string | El identificador de producto del juego Xbox One para el que se recuperan los datos de adquisición. |
| applicationName | string | El nombre para mostrar del juego. |
| acquisitionType | string | Una de las siguientes cadenas que indica el tipo de adquisición:  <ul><li>**Gratis**</li><li>**Versión de prueba**</li><li>**Pagado**</li><li>**Código promocional**</li><li>**IAP**</li><li>**Suscripción IAP**</li><li>**Audiencia privada**</li><li>**Pedido previo**</li><li>**Juego de Xbox Pass** (o **Pass Pass** si consulta los datos antes del 23 de marzo de 2018)</li><li>**Disco**</li><li>**Código de prepago**</li><li>**Pedido previo aplicado**</li><li>**Pedido anterior cancelado**</li><li>**Error de orden previo**</li></ul> |
| age | string | Una de las siguientes cadenas que indica el grupo de edad del usuario que realizó la adquisición: <ul><li>**Menor que 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**Mayor que 55**</li><li>**Unknown**</li></ul> |
| deviceType | string | Una de las siguientes cadenas que especifica el tipo de dispositivo que completó la adquisición: <ul><li>**PC**</li><li>**Número**</li><li>**Consola: Xbox One**</li><li>**Consola: serie Xbox X**</li><li>**IoT**</li><li>**Server**</li><li>**Tableta**</li><li>**Holographic**</li><li>**Unknown**</li></ul> |
| gender | string | Una de las siguientes cadenas que especifica el sexo del usuario que realizó la adquisición: <ul><li>**m**</li><li>**f**</li><li>**Unknown**</li></ul> |
| market | string | Código de país ISO 3166 del mercado donde se realizó la compra. |
| osVersion | string | Versión del sistema operativo en el que se realizó la compra. Para este método, este valor siempre es **Windows 10**. |
| paymentInstrumentType | string | Una de las siguientes cadenas que indica la instrucción de pago utilizada para la adquisición: <ul><li>**Tarjeta de crédito**</li><li>**Tarjeta de domiciliación directa**</li><li>**Compra inferido**</li><li>**Saldo de MS**</li><li>**Operador móvil**</li><li>**Transferencia bancaria en línea**</li><li>**PayPal**</li><li>**Dividir transacción**</li><li>**Canje de tokens**</li><li>**Importe pagado de cero**</li><li>**eWallet**</li><li>**Unknown**</li></ul> |
| sandboxId | string | El identificador de espacio aislado creado para el juego. Puede ser el valor **comercial** o un identificador de espacio aislado privado. |
| storeClient | string | Una de las siguientes cadenas que indica la versión del almacén donde se produjo la adquisición: <ul><li>**Tienda de Windows Phone (cliente)**</li><li>**Microsoft Store (cliente)** (o la **tienda Windows (cliente)** si consulta los datos antes del 23 de marzo de 2018) </li><li>**Microsoft Store (Web)** (o la **tienda Windows (Web)** si consulta los datos antes del 23 de marzo de 2018) </li><li>**Compras por volumen de empresas**</li><li>**Otros**</li></ul> |
| xboxTitleId | string | El identificador de título de Xbox Live (representado en valor hexadecimal) asignado por el portal para desarrolladores de Xbox (XDP) para juegos habilitados para Xbox Live. |
| acquisitionQuantity | number | Número de compras que se realizaron durante el nivel de agregación especificado. |
| purchasePriceUSDAmount | number | La cantidad pagada por el cliente para la adquisición, convertida en USD, con la tasa de cambio mensual. |
| purchaseTaxUSDAmount | number | Importe de impuestos aplicado a la adquisición, convertido a USD. |
| localCurrencyCode | string | Código de moneda local basado en el país de la cuenta del centro de Partners.  |
| xboxProductId | string | ID. de producto de Xbox del producto de XDP, si procede.  |
| availabilityId | string | IDENTIFICADOR de disponibilidad del producto de XDP, si procede.  |
| skuId | string | IDENTIFICADOR de SKU del producto de XDP, si procede.  |
| skuDisplayName  | string | Nombre para mostrar de SKU del producto de XDP, si procede.  |
| xboxParentProductId | string | IDENTIFICADOR del producto primario de Xbox del producto de XDP, si procede.  |
| parentProductName | string | Nombre del producto primario del producto de XDP, si procede.  |
| productTypeName | string | Nombre del tipo de producto del producto de XDP, si procede.  |
| purchaseTaxType | string | Tipo de impuesto de compra del producto de XDP, si procede.  |
| purchasePriceLocalAmount | number | Importe local del precio de compra del producto de XDP, si procede.  |
| purchaseTaxLocalAmount | number | Importe local del impuesto de compra del producto de XDP, si procede.  |

### <a name="response-example"></a>Ejemplo de respuesta
En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud. 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": "acquisitions?applicationId=9WZDNCRFHXHT&aggregationLevel=day&startDate=2017-01-01T08:00:00.0000000Z&endDate=2019-01-16T08:44:15.6045249Z&top=1&skip=1", 
    
    "TotalCount": 12221 
} 
```

 
