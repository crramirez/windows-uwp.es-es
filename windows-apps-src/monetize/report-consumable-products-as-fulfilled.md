---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Use este método en la API de colección de Microsoft Store para notificar un producto consumible como se cumple para un cliente determinado. Para que un usuario pueda volver a comprar un producto consumible, la aplicación o el servicio debe notificar el producto consumible como completado para dicho usuario.
title: Notificar productos consumibles como completados
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, API de colección Microsoft Store, satisfacer, consumir
ms.localizationpriority: medium
ms.openlocfilehash: 88ff4f9bd2c490c8fae4deb2cfa4cbf5c74956c8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174949"
---
# <a name="report-consumable-products-as-fulfilled"></a>Notificar productos consumibles como completados

Use este método en la API de colección de Microsoft Store para notificar un producto consumible como se cumple para un cliente determinado. Para que un usuario pueda volver a comprar un producto consumible, la aplicación o el servicio debe notificar el producto consumible como completado para dicho usuario.

Existen dos formas de usar este método para notificar un producto consumible como completado:

* Proporcionar el identificador de elemento del consumible (tal como se devuelve en el parámetro **itemId** de una [consulta de productos](query-for-products.md)) y un identificador de seguimiento único que debe indicar el usuario. Si se usa el mismo identificador de seguimiento para varios intentos, se devolverá el mismo resultado, aunque ya se haya consumido el artículo. Si no estás seguro sobre si una solicitud de consumo se llevó a cabo correctamente, el servicio debería volver a enviar las solicitudes de consumo con el mismo identificador de seguimiento. El id. de seguimiento siempre estará vinculado a esa solicitud de consumo y se podrá volver a enviar indefinidamente.
* Proporcionar el id. del producto (tal y como se devuelve en el parámetro **productId** de una [consulta de productos](query-for-products.md)) y un id. de transacción, que se obtiene de uno de los orígenes enumerados en la descripción del parámetro **transactionId** en la sección del cuerpo de la solicitud a continuación.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, necesitarás:

* Azure AD token de acceso que tiene el valor de URI de audiencia `https://onestore.microsoft.com` .
* Una clave de identificador de Microsoft Store que representa la identidad del usuario para el que desea notificar un producto consumible como satisfecho.

Para obtener más información, consulte [Administración de derechos de producto desde un servicio](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;.                           |
| administrador de flujos de trabajo           | string | Debe establecerse en el valor **collections.mp.microsoft.com**.                                            |
| Content-Length | number | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | string | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |


### <a name="request-body"></a>Cuerpo de la solicitud

| Parámetro     | Tipo         | Descripción         | Obligatorio |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | El usuario para el que se está consumiendo este artículo. Para más información, vea la tabla siguiente.        | Sí      |
| itemId        | string       | Valor de *Itemid* devuelto por una [consulta de productos](query-for-products.md). Use este parámetro con *trackingId*      | No       |
| trackingId    | guid         | Identificador de seguimiento único proporcionado por el desarrollador. Use este parámetro con *Itemid*.         | No       |
| productId     | string       | Valor *productId* devuelto por una [consulta de productos](query-for-products.md). Use este parámetro con *transactionId*   | No       |
| transactionId | guid         | El valor de identificador de transacción que se obtiene de uno de los siguientes orígenes. Use este parámetro con *productId*.<ul><li>La propiedad [TransactionID](/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid) de la clase [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults).</li><li>El recibo de la aplicación o del producto devuelto por [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), [RequestAppPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) o [GetAppReceiptAsync](/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync).</li><li>El parámetro *transactionId* devuelto por una [consulta de productos](query-for-products.md).</li></ul>   | No       |


El objeto UserIdentity contiene los parámetros siguientes.

| Parámetro            | Tipo   | Descripción       | Obligatorio |
|----------------------|--------|-------------------|----------|
| identityType         | string | Especifica el valor de cadena **b2b**.    | Sí      |
| identityValue        | string | La [clave de identificador de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa la identidad del usuario para el que desea notificar un producto consumible como satisfecho.      | Sí      |
| localTicketReference | string | El identificador solicitado para la respuesta devuelta. Se recomienda usar el mismo valor que la de la *userId*[demanda](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key) de userId en la clave de identificador de Microsoft Store.   | Sí      |


### <a name="request-examples"></a>Ejemplos de solicitud

El siguiente ejemplo usa *itemId* y *trackingId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

El siguiente ejemplo usa *productId* y *transactionId*.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>Response

Si el consumo se ejecutó correctamente, no se devolverá ningún contenido.

### <a name="response-example"></a>Ejemplo de respuesta

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>Códigos de error


| Código | Error        | Código de error interno           | Descripción           |
|------|--------------|----------------------------|-----------------------|
| 401  | No autorizado | AuthenticationTokenInvalid | El token de acceso de Azure AD no es válido. En algunos casos, los detalles del código ServiceError contendrán más información, como la fecha de expiración del token o si falta la notificación *appid*. |
| 401  | No autorizado | PartnerAadTicketRequired   | No se pasó un token de acceso de Azure AD al servicio en el encabezado de autorización.                                                                                                   |
| 401  | No autorizado | InconsistentClientId       | La solicitud *clientId* en la clave de identificador de Microsoft Store en el cuerpo de la solicitud y la solicitud *AppID* en el token de acceso Azure ad en el encabezado de autorización no coinciden.                     |

<span/> 

## <a name="related-topics"></a>Temas relacionados

* [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md)
* [Consultar productos](query-for-products.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Renovar una clave de identificador de Microsoft Store](renew-a-windows-store-id-key.md)