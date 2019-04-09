---
description: Utilice este método en la API de análisis de Microsoft Store para obtener datos de adquisición agregado en formato JSON para aplicaciones de UWP y juegos de Xbox One que estaban disponibles en el panel de análisis de XDP e introducidos mediante el Portal para desarrolladores de Xbox (XDP).
title: Obtener datos de adquisiciones para sus aplicaciones y juegos
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, UWP, red de publicidad, metadatos de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: beca5620f25713e8a07e5dbaf64e955d920702a7
ms.sourcegitcommit: df8e4143e81a1c5fe1aa5f14407b8dd5f155a12e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829918"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>Obtener datos de adquisiciones para sus aplicaciones y juegos 
Utilice este método en la API de análisis de Microsoft Store para obtener datos de adquisición agregado en formato JSON para aplicaciones de UWP y juegos de Xbox One que estaban disponibles en el panel de análisis de XDP e introducidos mediante el Portal para desarrolladores de Xbox (XDP). 

> [!NOTE]
> Esta API no proporciona datos agregados diarias antes del 1 de octubre de 2016. 

## <a name="prerequisites"></a>Requisitos previos
Para usar este método, primero debes hacer lo siguiente: 

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md) para la API de análisis de Microsoft Store. 
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo. 

## <a name="request"></a>Solicitud
### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>Encabezado de la solicitud

| Header | Tipo | Descripción |
| --- | --- | --- |
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** `<token>`. |

### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro | Tipo | Descripción | Requerido |
| --- | --- | --- | --- |
| applicationId | string | Identificador de producto del juego para Xbox One para el que recuperas los datos de adquisición. Para obtener el identificador de producto de su juego, vaya a su juego en el programa de análisis XDP y recuperar el identificador de producto de la dirección URL. Como alternativa, si descarga los datos de adquisiciones desde el informe de análisis de centro de partners, el identificador de producto se incluye en el archivo TSV.  | Sí |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de compra que se han de recuperar. El valor predeterminado es la fecha actual.  | No |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos de compra que se han de recuperar. El valor predeterminado es la fecha actual.  | No |
| top | número entero | Número de filas de datos que se van a devolver. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.  | No |
| skip | número entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, *superior = 10000 y omitir = 0* recupera las filas de datos, en primer lugar 10000 *superior = 10000 y omitir = 10000* recupera el siguiente 10000 filas de datos y así sucesivamente.  | No |
| filter | string | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Los valores de cadena deben estar entre comillas simples en el parámetro de filtro. Por ejemplo, *filter=market eq 'US' and gender eq 'm'*.  <br/> Puedes especificar los campos siguientes del cuerpo de respuesta: <ul><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | No |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: **día**, **semana** o **mes**. Si no se especifica, el valor predeterminado es **día**.  | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada compra. La sintaxis es *orderby = campo [order], [order],...* El parámetro *field* puede ser una de las siguientes cadenas: <ul><li>**date**</li><li>**acquisitionType**</li><li>**age**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> El parámetro *order*, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **asc**. Aquí tienes un ejemplo de una cadena *orderby*: *orderby=date,market*  | No |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos: <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**gender**</li><li>**market**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro *groupby* y en los siguientes: <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> El *groupby* parámetro puede usarse con el parámetro aggregationLevel. Por ejemplo: *& groupby = grupo de edad, mercado & aggregationLevel = semana*  | No |

### <a name="request-example"></a>Ejemplo de solicitud
El siguiente ejemplo muestra varias solicitudes para obtener los datos de adquisición de juegos para Xbox One. Reemplace el *applicationId* valor con el identificador de producto para su juego.  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>Respuesta

### <a name="response-body"></a>Cuerpo de la respuesta
| Valor | Tipo | Descripción |
| --- | --- | --- |
| Valor | array | Una matriz de objetos que contienen los datos de adquisición agregados del juego. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra](#acquisition-values) que encontrarás a continuación. |
| @nextLink | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra de la solicitud. |
| TotalCount | número entero | El número total de filas del resultado de datos de la consulta. |

### <a name="acquisition-values"></a>Valores de compra 
Los elementos de la matriz *Value* contienen los siguientes valores. 

| Valor | Tipo | Descripción |
| --- | --- | --- |
| fecha | string | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId | string | Identificador de producto del juego para Xbox One para el que recuperas los datos de adquisición. |
| applicationName | string | El nombre para mostrar del juego. |
| acquisitionType | string | Una de las siguientes cadenas que indica el tipo de adquisición:  <ul><li>**Free**</li><li>**Versión de prueba**</li><li>**Paid**</li><li>**Código de promoción**</li><li>**IAP**</li><li>**Suscripción Iap**</li><li>**Audiencia privada**</li><li>**Pre Order**</li><li>**Xbox Game Pass** (o **Game Pass** si la consulta de datos es antes del 23 de marzo de 2018)</li><li>**Disco**</li><li>**Código de prepago**</li><li>**Orden de Pre cargada**</li><li>**Cancelación de pedido anterior a**</li><li>**Orden de Pre con errores**</li></ul> |
| age | string | Una de las cadenas siguientes que indica el grupo de edad del usuario que ha realizado la adquisición: <ul><li>**menos de 13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**superior a 55**</li><li>**Unknown**</li></ul> |
| deviceType | string | Una de las cadenas siguientes que especifica el tipo de dispositivo que ha completado la adquisición: <ul><li>**PC**</li><li>**Teléfono**</li><li>**Consola de**</li><li>**IoT**</li><li>**Server**</li><li>**Tablet**</li><li>**Holográfica**</li><li>**Unknown**</li></ul> |
| gender | string | Una de las cadenas siguientes que especifica el sexo del usuario que ha realizado la adquisición: <ul><li>**m**</li><li>**f**</li><li>**Unknown**</li></ul> |
| market | string | Código de país ISO 3166 del mercado donde se realizó la compra. |
| osVersion | string | Versión del sistema operativo en el que se realizó la compra. Para este método, este valor es siempre **Windows 10**. |
| paymentInstrumentType | string | Una de las cadenas siguientes que indica la instrucción de pago utilizada para la adquisición: <ul><li>**Tarjeta de crédito**</li><li>**Tarjeta de débito directo**</li><li>**Compra deducido**</li><li>**Saldo de MS**</li><li>**Operador de telefonía móvil**</li><li>**Transferencia bancaria en línea**</li><li>**PayPal**</li><li>**Dividir la transacción**</li><li>**Canje de token**</li><li>**Cero importe pagado**</li><li>**eWallet**</li><li>**Unknown**</li></ul> |
| sandboxId | string | El id. de espacio aislado creado para el juego. Esto puede ser el valor **RETAIL** o un identificador de espacio aislado privada. |
| storeClient | string | Una de las cadenas siguientes que indica la versión de la Store donde se produce la adquisición: <ul><li>**Windows Phone Store (cliente)**</li><li>**Microsoft Store (cliente)** (o **Tienda de Windows (cliente)** si la consulta de datos es anterior al 23 de marzo de 2018) </li><li>**Microsoft Store (web)** (o **Tienda de Windows (web)** si la consulta de datos es anterior al 23 de marzo de 2018) </li><li>**Compras por volumen de organizaciones**</li><li>**Otros**</li></ul> |
| xboxTitleId | string | Id. de título de Xbox Live (representado en valor hexadecimal) asignado por el Portal de desarrollador de Xbox (XDP) para juegos habilitados para Xbox Live. |
| acquisitionQuantity | número | Número de compras que se realizaron durante el nivel de agregación especificado. |
| purchasePriceUSDAmount | número | El importe pagado por el cliente para la adquisición, convertido en USD, con el tipo de cambio mensual. |
| purchaseTaxUSDAmount | número | El importe de impuestos aplicado a la adquisición,convertido en USD. |
| localCurrencyCode | string | Código de moneda local según el país de la cuenta del centro de partners.  |
| xboxProductId | string | Xbox Id. de producto del producto de XDP, si procede.  |
| availabilityId | string | Identificador de la disponibilidad del producto de XDP, si procede.  |
| skuId | string | Identificador de SKU del producto de XDP, si procede.  |
| skuDisplayName  | string | SKU Mostrar nombre del producto de XDP, si procede.  |
| xboxParentProductId | string | Identificador de producto de Xbox principal del producto de XDP, si procede.  |
| parentProductName | string | Nombre de producto principal del producto de XDP, si procede.  |
| productTypeName | string | Nombre de tipo de producto del producto de XDP, si procede.  |
| purchaseTaxType | string | Tipo de impuesto de compra del producto de XDP, si procede.  |
| purchasePriceLocalAmount | número | Comprar cantidad Local del precio del producto de XDP, si procede.  |
| purchaseTaxLocalAmount | número | Comprar Tax Amount Local del producto de XDP, si procede.  |

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

 
