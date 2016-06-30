---
author: mcleanbyron
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: "Usa este método en la API de colecciones de la Tienda Windows para notificar un producto consumible como completado para un cliente determinado. Para que un usuario pueda volver a comprar un producto consumible, la aplicación o el servicio debe notificar el producto consumible como completado para dicho usuario."
title: Notificar productos consumibles como completados
ms.sourcegitcommit: 2f4351d6f9bdc0b9a131ad5ead10ffba7e76c437
ms.openlocfilehash: b099bdc26565ef218eaf1f73c5bb3ec9c24065c3

---

# Notificar productos consumibles como completados


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa este método en la API de colecciones de la Tienda Windows para notificar un producto consumible como completado para un cliente determinado. Para que un usuario pueda volver a comprar un producto consumible, la aplicación o el servicio debe notificar el producto consumible como completado para dicho usuario.

Existen dos formas de usar este método para notificar un producto consumible como completado:

-   Proporcionar el identificador de elemento del consumible (tal como se devuelve en el parámetro **itemId** de una [consulta de productos](query-for-products.md)) y un identificador de seguimiento único que debe indicar el usuario. Si se usa el mismo identificador de seguimiento para varios intentos, se devolverá el mismo resultado, aunque ya se haya consumido el artículo. Si no estás seguro sobre si una solicitud de consumo se llevó a cabo correctamente, el servicio debería volver a enviar las solicitudes de consumo con el mismo identificador de seguimiento. El id. de seguimiento siempre estará vinculado a esa solicitud de consumo y se podrá volver a enviar indefinidamente.
-   Proporcionar el id. del producto (tal y como se devuelve en el parámetro **productId** de una [consulta de productos](query-for-products.md)) y un id. de transacción, que se obtiene de uno de los orígenes enumerados en la descripción del parámetro **transactionId** en la sección del cuerpo de la solicitud a continuación.

## Requisitos previos


Para usar este método, necesitarás:

-   Un token de acceso de Azure AD que se haya creado con el URI de público `https://onestore.microsoft.com`.
-   Una clave de id. de la Tienda Windows que se haya generado mediante la llamada al método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) desde el código del cliente de la aplicación.

Para obtener más información, consulta [Ver y conceder productos desde un servicio](view-and-grant-products-from-a-service.md).

## Solicitud


### Sintaxis de la solicitud

| Método | URI de la solicitud                                                   |
|--------|---------------------------------------------------------------|
| POST   | `https://collections.mp.microsoft.com/v6.0/collections/consume` |

<br/> 

### Encabezado de la solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorización  | cadena | Obligatorio. El token de acceso de Azure AD del formulario **Bearer**&lt;*token*&gt;.                           |
| Host           | cadena | Debe establecerse en el valor **collections.mp.microsoft.com**.                                            |
| Content-Length | número | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | cadena | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |

<br/> 

### Cuerpo de la solicitud

| Parámetro     | Tipo         | Descripción         | Obligatorio |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | El usuario para el que se está consumiendo este artículo.                                                                                                                                                                                                                                                                 | Sí      |
| itemId        | Cadena       | Valor itemId devuelto por una [consulta de productos](query-for-products.md). Usa este parámetro con trackingId                                                                                                                                                                                                  | No       |
| trackingId    | Guid         | Identificador de seguimiento único proporcionado por el desarrollador. Usa este parámetro con itemId.                                                                                                                                                                                                                                     | No       |
| productId     | Cadena       | Valor productId devuelto por una [consulta de productos](query-for-products.md). Usa este parámetro con transactionId                                                                                                                                                                                            | No       |
| transactionId | Guid         | Valor de identificador de transacción que se obtiene de uno de los siguientes orígenes:                                                                                                                                                                                                                                      | No       |
|               |              | * Propiedad [TransactionID](https://msdn.microsoft.com/library/windows/apps/dn263396) de la clase [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392).   |        |
|               |              | * Recibo de aplicación o de producto devuelto por [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381), [RequestAppPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/hh967813) o [GetAppReceiptAsync](https://msdn.microsoft.com/library/windows/apps/hh967811).   |        |
|               |              | * Parámetro transactionId devuelto por una [consulta de productos](query-for-products.md).   |        |        
|               |              | Usa este parámetro con productId.   |        |
 
<br/>

El objeto UserIdentity contiene los parámetros siguientes.

| Parámetro            | Tipo   | Descripción                                                                                                                                 | Obligatorio |
|----------------------|--------|---------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | cadena | Especifique el valor de cadena **b2b**.                                                                                                           | Sí      |
| identityValue        | cadena | Valor de cadena de la clave de id. de la Tienda Windows.                                                                                                   | Sí      |
| localTicketReference | Cadena | Identificador solicitado para la respuesta devuelta. Se recomienda usar el mismo valor que la notificación *userId* de la clave de id. de la Tienda Windows. | Sí      |

<br/> 

### Ejemplos de solicitud

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

## Respuesta


Si el consumo se ejecutó correctamente, no se devolverá ningún contenido.

### Ejemplo de respuesta

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## Códigos de error


| Código | Error        | Código de error interno           | Descripción                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | No autorizado | AuthenticationTokenInvalid | El token de acceso de Azure AD no es válido. En algunos casos, los detalles del código ServiceError contendrán más información, como la fecha de expiración del token o si falta la notificación *appid*. |
| 401  | No autorizado | PartnerAadTicketRequired   | No se pasó un token de acceso de Azure AD al servicio en el encabezado de autorización.                                                                                                   |
| 401  | No autorizado | InconsistentClientId       | La notificación *clientId* de la clave de id. de la Tienda Windows del cuerpo de la solicitud y la notificación *appid* del token de acceso de Azure AD del encabezado de autorización no coinciden.                     |

<br/> 

## Temas relacionados

* [Ver y conceder productos desde un servicio](view-and-grant-products-from-a-service.md)
* [Consultar productos](query-for-products.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Renovar una clave de id. de la Tienda Windows](renew-a-windows-store-id-key.md)
 

 



<!--HONumber=Jun16_HO4-->


