---
description: Utilice este método en la API de análisis de Microsoft Store para obtener datos de adquisición de complemento agregado en formato JSON para aplicaciones de UWP y juegos de Xbox One que estaban introducidos mediante el Portal para desarrolladores de Xbox (XDP) y está disponible en el panel del centro de partners XDP Analytics.
title: Obtener datos de adquisiciones de complemento para las aplicaciones y juegos
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, red de publicidad, metadatos de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 518648d52c613a3dd5f1bca0d34a7f533b59733f
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829909"
---
# <a name="get-add-on-acquisitions-data-for-your-games-and-apps"></a>Obtener datos de adquisiciones de complemento para las aplicaciones y juegos 
Utilice este método en la API de análisis de Microsoft Store para obtener datos de adquisición de complemento agregado en formato JSON para aplicaciones de UWP y juegos de Xbox One que estaban introducidos mediante el Portal para desarrolladores de Xbox (XDP) y está disponible en el panel del centro de partners XDP Analytics. 

## <a name="prerequisites"></a>Requisitos previos
Para usar este método, primero debes hacer lo siguiente: 

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md) para la API de análisis de Microsoft Store. 
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo. 

> [!NOTE]
> Esta API no proporciona datos agregados diarias antes del 1 de octubre de 2016. 

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud
| Método | URI de la solicitud |
| --- | --- | 
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions` |

### <a name="request-header"></a>Encabezado de la solicitud 
| Header | Tipo | Descripción | 
| --- | --- | --- |
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** `<token>`. |

### <a name="request-parameters"></a>Parámetros de solicitud
El *applicationId* o *addonProductId* el parámetro es obligatorio. Para recuperar los datos de compra de todos los complementos registrados en la aplicación, especifica el parámetro *applicationId*. Para recuperar datos de adquisición de un único complemento, especifique el *addonProductId* parámetro. Si especificas ambos, el parámetro *applicationId* se ignorará. 

| Parámetro | Tipo | Descripción | Requerido | 
| --- | --- | --- | --- |
| applicationId | string | El *productId* del juego de Xbox One que va a recuperar datos de adquisición. Para obtener el *productId* de su juego, vaya a su juego en el programa de análisis XDP y recuperar el *productId* desde la dirección URL. Como alternativa, si descarga los datos de adquisiciones desde el informe de análisis de centro de partners, el *productId* se incluye en el archivo TSV. | Sí |
| addonProductId | string | El *productId* del complemento para el que desea recuperar datos de adquisición. | Sí |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de compra del complemento que se recuperarán. El valor predeterminado es la fecha actual. | No |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos de compra del complemento que recuperarán. El valor predeterminado es la fecha actual. | No |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. | No |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. | No |
| filter | string | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo desde el cuerpo de respuesta y el valor que están asociados con los operadores eq o ne, y las instrucciones se pueden combinar con y o. Los valores de cadena deben estar entre comillas simples en el parámetro de filtro. Por ejemplo, filtrar = mercado eq eq 'US' y el sexo estoy '. <br/> Puedes especificar los campos siguientes del cuerpo de respuesta: <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | No |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **día**, **semana** o **mes**. Si no se especifica, el valor predeterminado es **día**. | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada compra de complemento. La sintaxis es *orderby = campo [order], [order],...* El parámetro *field* puede ser una de las siguientes cadenas: <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**orderName**</li></ul> El parámetro order es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente para cada campo. El valor predeterminado es **asc**. <br/> Aquí tienes un ejemplo de una cadena *orderby*: *orderby=date,market* | No |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos: <ul><li>**date**</li><li>**applicationName**</li><li>**addonProductName**</li> <li>**acquisitionType**</li><li>**age**</li> <li>**storeClient**</li><li>**gender**</li> <li>**market**</li> <li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro *groupby* y en los siguientes: <ul><li>**date**</li><li>**applicationId**</li><li>**addonProductId**</li><li>**acquisitionQuantity**</li></ul> El parámetro groupby puede usarse con el *aggregationLevel* parámetro. Por ejemplo: *& groupby = edad, mercado & aggregationLevel = semana* | No |

### <a name="request-example"></a>Ejemplo de solicitud
En los ejemplos siguientes se muestran varias solicitudes para obtener datos de compra de complementos. Reemplace el *addonProductId* y *applicationId* valores con el identificador de Store apropiado para su aplicación o complemento. 

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0 HTTP/1.1 

Authorization: Bearer <your access token> 

 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/addonacquisitions?applicationId=9WZDNCRFJ314&startDate=1/1/2015&endDate=2/1/2015&skip=0&filter=market eq 'GB' and gender eq 'm' HTTP/1.1 

Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor | Tipo | Descripción |
| --- | --- | --- |
| Valor | array | Matriz de objetos que contienen los datos de compra agregados del complemento. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra del complemento](#add-on-acquisition-values) que encontrarás a continuación. |
| @nextLink | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero hay más de 10000 filas de datos de compra de complementos para la solicitud. |
| TotalCount | entero | El número total de filas del resultado de datos de la consulta. |

### <a name="add-on-acquisition-values"></a>Valores de compra del complemento
Elementos de la matriz de valores contienen los siguientes valores.

| Valor | Tipo | Descripción | 
| --- | --- | --- |
| fecha | string | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| addonProductId | string | El *productId* del complemento para el que va a recuperar datos de adquisición. |
| addonProductName | string | Nombre del complemento que quieres que se muestre. Este valor solo aparece en los datos de respuesta si la *aggregationLevel* parámetro está establecido en **día**, a menos que especifique el **addonProductName** campo el *groupby* parámetro. |
| applicationId | string | El *productId* de la aplicación para la que desea recuperar los datos de adquisición de complemento. |
| applicationName | string | El nombre para mostrar del juego. |
| deviceType | string | Una de las cadenas siguientes que especifica el tipo de dispositivo que ha completado la adquisición: <ul><li>"PC"</li><li>"Teléfono"</li><li>"Consola"</li><li>"IoT"</li><li>"Server"</li><li>"Tablet PC"</li><li>"Holographic"</li><li>"Desconocido"</li></ul> |
| storeClient | string | Una de las cadenas siguientes que indica la versión de la Store donde se produce la adquisición: <ul><li>"Windows Phone Store (cliente)"</li><li>"Microsoft Store (cliente)" (o "Windows Store (cliente)" si la consulta de datos antes del 23 de marzo de 2018)</li><li>"Microsoft Store (web)" (o "Windows Store (web)" si la consulta de datos antes del 23 de marzo de 2018)</li><li>"Compras de volumen por organizaciones"</li><li>"Otros"</li></ul> |
| osVersion | string | Versión del sistema operativo en el que se realizó la compra. Este método, este valor siempre es "Windows 10". |
| market | string | Código de país ISO 3166 del mercado donde se realizó la compra. |
| gender | string | Una de las cadenas siguientes que especifica el sexo del usuario que ha realizado la adquisición: <ul><li>"m"</li><li>"f"</li><li>"Desconocido"</li></ul> |
| age | string | Una de las cadenas siguientes que indica el grupo de edad del usuario que ha realizado la adquisición: <ul><li>"menos de 13"</li><li>"13-17"</li><li>"18-24"</li><li>"25-34"</li><li>"35-44"</li><li>"44-55"</li><li>"mayor que 55"</li><li>"Desconocido"</li></ul> |
| acquisitionType | string | Una de las siguientes cadenas que indica el tipo de adquisición: <ul><li>"Gratuito" </li><li>"Trial" </li><li>"Pagado"</li><li>"Código de promoción" </li><li>"Iap"</li><li>"Suscripción Iap"</li><li>"Audience privada"</li><li>"Pre Order"</li><li>"Xbox Game Pass" (o "Juego pasar" si la consulta de datos antes del 23 de marzo de 2018)</li><li>"Disco"</li><li>"Código de prepago"</li><li>"Cobra orden anterior a"</li><li>"Cancelar pedido Pre"</li><li>"Error de orden anterior a"</li></ul> |
| acquisitionQuantity | número entero | Número de compras que se realizaron. |
| inAppProductId | string | Id. de producto del producto que se usa este complemento.  |
| inAppProductName | string | Nombre de producto del producto que se usa este complemento.  |
| paymentInstrumentType | string | Tipo de instrumento de pago utilizado para la adquisición.  |
| sandboxId | string | El identificador de espacio aislado creado para el juego. Esto puede ser el valor **RETAIL** o un identificador de espacio aislado privada.  |
| xboxTitleId | string | Xbox título Id. del producto de XDP, si procede.  |
| localCurrencyCode | string | Código de moneda local según el país de la cuenta del centro de partners.  |
| xboxProductId | string | Xbox Id. de producto del producto de XDP, si procede.  |
| availabilityId | string | Identificador de la disponibilidad del producto de XDP, si procede.  |
| skuId | string | Identificador de SKU del producto de XDP, si procede.  |
| skuDisplayName | string | Nombre para mostrar SKU del producto de XDP, si procede.  |
| xboxParentProductId | string | Identificador de producto de Xbox principal del producto de XDP, si procede.  |
| parentProductName | string | Nombre de producto principal del producto de XDP, si procede.  |
| productTypeName | string | Nombre de tipo de producto del producto de XDP, si procede.  |
| purchaseTaxType | string | Tipo de impuesto de compra del producto de XDP, si procede.  |
| purchasePriceUSDAmount | número | El importe pagado por el cliente para el complemento, se convierten en USD.  |
| purchasePriceLocalAmount | número | El importe de impuestos que se aplica al complemento.  |
| purchaseTaxUSDAmount | número | El importe de impuesto aplicado al complemento, se convierten en USD.  |
| purchaseTaxLocalAmount | número | Comprar Tax Amount Local del producto de XDP, si procede.  |

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