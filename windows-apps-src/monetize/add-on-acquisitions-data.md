---
description: Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición de complementos de agregado en formato JSON para aplicaciones UWP y juegos de Xbox One que se ingeriron a través del portal para desarrolladores de Xbox (XDP) y que están disponibles en el panel del centro de Partners de XDP Analytics.
title: Obtener datos de adquisiciones de complementos para sus aplicaciones y juegos
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, red de publicidad, metadatos de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 3e1349582515ce66232ea8266efc588610faae98
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493570"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>Obtener datos de adquisiciones de complementos para sus aplicaciones y juegos 
Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición de complementos de agregado en formato JSON para aplicaciones UWP y juegos de Xbox One que se ingeriron a través del portal para desarrolladores de Xbox (XDP) y que están disponibles en el panel del centro de Partners de XDP Analytics. 

## <a name="prerequisites"></a>Requisitos previos
Para usar este método, primero debes hacer lo siguiente: 

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md) de la API de Microsoft Store Analytics. 
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo. 

> [!NOTE]
> Esta API no proporciona datos agregados diarios antes del 1 de octubre de 2016. 

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud
| Método | URI de solicitud |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>Encabezado de solicitud 
| Encabezado | Tipo | Descripción | 
| --- | --- | --- |
| Authorization | string | Necesario. El token de acceso Azure AD en el **portador** del formulario `<token>` . |

### <a name="request-parameters"></a>Parámetros de solicitud
El parámetro *ApplicationID* o *addonProductId* es obligatorio. Para recuperar los datos de compra de todos los complementos registrados en la aplicación, especifica el parámetro *applicationId*. Para recuperar los datos de adquisición para un único complemento, especifique el parámetro *addonProductId* . Si especificas ambos, el parámetro *applicationId* se ignorará. 

| Parámetro | Tipo | Descripción | Requerido | 
| --- | --- | --- | --- |
| applicationId | string | *ProductId* del juego Xbox One para el que se recuperan los datos de adquisición. Para obtener el *ProductID* del juego, navegue hasta el juego en el programa XDP Analytics y recupere el *ProductID* de la dirección URL. Como alternativa, si descarga los datos de adquisiciones del informe del centro de Partners, el *productId* se incluye en el archivo. TSV. | Sí |
| addonProductId | string | *ProductId* del complemento para el que desea recuperar los datos de adquisición. | Sí |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de compra del complemento que se recuperarán. La fecha predeterminada es la actual. | No |
| endDate | date | La fecha de finalización del intervalo de fechas de los datos de compra del complemento que recuperarán. La fecha predeterminada es la actual. | No |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. | No |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. | No |
| filter | string | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores EQ o NE, y las instrucciones se pueden combinar con and u or. Ten en cuenta que en el parámetro filter los valores de la cadena deben estar entre comillas simples. Por ejemplo, filtro = Market EQ ' EE. UU. ' y sexo EQ ' m '. <br/> Puede especificar los siguientes campos del cuerpo de la respuesta: <ul><li>**acquisitionType**</li><li>**antig**</li><li>**storeClient**</li><li>**sexo**</li><li>**datamarket**</li><li>**osVersion**</li><li>**TipoDeDispositivo**</li><li>**sandboxId**</li></ul> | No |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **día**, **semana** o **mes**. Si no se especifica, el valor predeterminado es **día**. | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada compra de complemento. La sintaxis es *OrderBy = Field [order], Field [order],...* El parámetro de *campo* puede ser una de las cadenas siguientes: <ul><li>**date**</li><li>**acquisitionType**</li><li>**antig**</li><li>**storeClient**</li><li>**sexo**</li><li>**datamarket**</li><li>**osVersion**</li><li>**TipoDeDispositivo**</li><li>**orderName**</li></ul> El parámetro order, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **ASC**. <br/> Este es un ejemplo de cadena *OrderBy* : *OrderBy = Date, Market* | No |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos: <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**antig**</li> <li>**storeClient**</li><li>**sexo**</li> <li>**datamarket**</li> <li>**osVersion**</li><li>**TipoDeDispositivo**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro *groupby* y en los siguientes: <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> Puedes usar el parámetro groupby con *aggregationLevel*. Por ejemplo: *&GroupBy = Age, market&aggregationLevel = Week* | No |

### <a name="request-example"></a>Ejemplo de solicitud
En los ejemplos siguientes se muestran varias solicitudes para obtener datos de compra de complementos. Reemplace los valores *addonProductId* y *APPLICATIONID* por el identificador de almacén adecuado para el complemento o la aplicación. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

### <a name="response-body"></a>Cuerpo de la respuesta

| Value | Tipo | Descripción |
| --- | --- | --- |
| Value | array | Matriz de objetos que contienen los datos de compra agregados del complemento. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra del complemento](#add-on-acquisition-values) que encontrarás a continuación. |
| @nextLink | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero hay más de 10000 filas de datos de compra de complementos para la solicitud. |
| TotalCount | int | El número total de filas del resultado de datos de la consulta. |

### <a name="add-on-acquisition-values"></a>Valores de compra del complemento
Los elementos de la matriz Value contienen los siguientes valores.

| Value | Tipo | Descripción | 
| --- | --- | --- |
| date | string | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| addonProductId | string | *ProductId* del complemento para el que se recuperan los datos de adquisición. |
| addonProductName | string | Nombre del complemento que quieres que se muestre. Este valor solo aparece en los datos de respuesta si el parámetro *aggregationLevel* está establecido en **Day**, a menos que especifique el campo **addonProductName** en el parámetro *GroupBy* . |
| applicationId | string | El *productId* de la aplicación para la que desea recuperar los datos de adquisición del complemento. |
| applicationName | string | El nombre para mostrar del juego. |
| deviceType | string | Una de las siguientes cadenas que especifica el tipo de dispositivo que completó la adquisición: <ul><li>PC</li><li>Número</li><li>"Consola: Xbox One"</li><li>"Consola: serie Xbox X"</li><li>IOT</li><li>Servidor</li><li>Tableta</li><li>Holográfica</li><li>Unknown</li></ul> |
| storeClient | string | Una de las siguientes cadenas que indica la versión del almacén donde se produjo la adquisición: <ul><li>"Almacén de Windows Phone (cliente)"</li><li>"Microsoft Store (cliente)" (o "tienda Windows (cliente)" si se consultan los datos antes del 23 de marzo de 2018)</li><li>"Microsoft Store (Web)" (o "tienda Windows (Web)" si se consultan los datos antes del 23 de marzo de 2018)</li><li>"Compras por volumen por organizaciones"</li><li>Distinta</li></ul> |
| osVersion | string | Versión del sistema operativo en el que se realizó la compra. Para este método, este valor siempre es "Windows 10". |
| market | string | Código de país ISO 3166 del mercado donde se realizó la compra. |
| gender | string | Una de las siguientes cadenas que especifica el sexo del usuario que realizó la adquisición: <ul><li>"m"</li><li>"f"</li><li>Unknown</li></ul> |
| age | string | Una de las siguientes cadenas que indica el grupo de edad del usuario que realizó la adquisición: <ul><li>"menor que 13"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"mayor que 55"</li><li>Unknown</li></ul> |
| acquisitionType | string | Una de las siguientes cadenas que indica el tipo de adquisición: <ul><li>Ningún </li><li>Periodo </li><li>Abonen</li><li>"Código promocional" </li><li>IAP</li><li>"Suscripción IAP"</li><li>"Público privado"</li><li>"Orden previo"</li><li>"Paso de juego de Xbox" (o "paso de juego" si se consultan los datos antes del 23 de marzo de 2018)</li><li>Discos</li><li>"Código de prepago"</li><li>"Pedido anterior cargado"</li><li>"Se canceló el pedido anterior"</li><li>"Error de orden anterior"</li></ul> |
| acquisitionQuantity | integer | Número de compras que se realizaron. |
| inAppProductId | string | IDENTIFICADOR del producto en el que se usa este complemento.  |
| inAppProductName | string | Nombre del producto en el que se usa este complemento.  |
| paymentInstrumentType | string | Tipo de instrumento de pago usado para la adquisición.  |
| sandboxId | string | El identificador de espacio aislado creado para el juego. Puede ser el valor **comercial** o un identificador de espacio aislado privado.  |
| xboxTitleId | string | IDENTIFICADOR de título de Xbox del producto de XDP, si procede.  |
| localCurrencyCode | string | Código de moneda local basado en el país de la cuenta del centro de Partners.  |
| xboxProductId | string | ID. de producto de Xbox del producto de XDP, si procede.  |
| availabilityId | string | IDENTIFICADOR de disponibilidad del producto de XDP, si procede.  |
| skuId | string | IDENTIFICADOR de SKU del producto de XDP, si procede.  |
| skuDisplayName | string | Nombre para mostrar de SKU del producto de XDP, si procede.  |
| xboxParentProductId | string | IDENTIFICADOR del producto primario de Xbox del producto de XDP, si procede.  |
| parentProductName | string | Nombre del producto primario del producto de XDP, si procede.  |
| productTypeName | string | Nombre del tipo de producto del producto de XDP, si procede.  |
| purchaseTaxType | string | Tipo de impuesto de compra del producto de XDP, si procede.  |
| purchasePriceUSDAmount | number | La cantidad pagada por el cliente para el complemento, convertido a USD.  |
| purchasePriceLocalAmount | number | Importe de impuestos aplicado al complemento.  |
| purchaseTaxUSDAmount | number | Importe de impuestos que se aplica al complemento, convertido a USD.  |
| purchaseTaxLocalAmount | number | Importe local del impuesto de compra del producto de XDP, si procede.  |

### <a name="response-example"></a>Ejemplo de respuesta
En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud. 

```JSON
{ 
  "Value": [ 
    { 
            "inAppProductId": "9NBLGGH1864K", 
            "inAppProductName": "866879", 
            "addonProductId": "9NBLGGH1864K", 
            "addonProductName": "866879", 
            "date": "2017-11-05", 
            "applicationId": "9WZDNCRFJ314", 
            "applicationName": "Tetris Blitz", 
            "acquisitionType": "Iap", 
            "age": "35-49", 
            "deviceType": "Phone", 
            "gender": "m", 
            "market": "US", 
            "osVersion": "Windows Phone 8.1", 
            "paymentInstrumentType": "Credit Card", 
            "sandboxId": "RETAIL", 
            "storeClient": "Windows Phone Store (client)", 
            "xboxTitleId": "", 
            "localCurrencyCode": "USD", 
            "xboxProductId": "00000000-0000-0000-0000-000000000000", 
            "availabilityId": "", 
            "skuId": "", 
            "skuDisplayName": "Full", 
            "xboxParentProductId": "", 
            "parentProductName": "Tetris Blitz", 
            "productTypeName": "Add-On", 
            "purchaseTaxType": "", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 1.08, 
            "purchasePriceLocalAmount": 0.09, 
            "purchaseTaxUSDAmount": 1.08, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null, 
    
    "TotalCount": 7601 
} 
```