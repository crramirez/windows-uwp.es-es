---
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: Usa este método en la API de colecciones de la Tienda Windows para obtener todos los productos que posee un cliente para las aplicaciones asociadas a tu identificador de cliente de Azure AD. Puedes definir el ámbito de la consulta para un producto concreto, o bien usar otros filtros.
title: Consultar productos
---

# Consultar productos


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa este método en la API de colecciones de la Tienda Windows para obtener todos los productos que posee un cliente para las aplicaciones asociadas a tu identificador de cliente de Azure AD. Puedes definir el ámbito de la consulta para un producto concreto, o bien usar otros filtros.

Este método está diseñado para que el servicio lo llame en respuesta a un mensaje de la aplicación. El servicio no debe sondear regularmente a todos los usuarios en una programación.

## Requisitos previos


Para usar este método, necesitarás:

-   Un token de acceso de Azure AD que se creó con la URI de audiencia **https://onestore.microsoft.com**.
-   Una clave de id. de la Tienda Windows que se generó mediante la llamada del método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) desde el código de cliente de la aplicación.

Para obtener más información, consulta [Ver y conceder productos desde un servicio](view-and-grant-products-from-a-service.md).

## Solicitud

### Sintaxis de la solicitud

| Método | URI de solicitud                                                 |
|--------|-------------------------------------------------------------|
| POST   | https://collections.mp.microsoft.com/v6.0/collections/query |

 
### Encabezado de la solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | cadena | Obligatorio. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;.                           |
| Host           | cadena | Debe establecerse en el valor **collections.mp.microsoft.com**.                                            |
| Content-Length | número | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | cadena | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |

 

### Cuerpo de la solicitud

| Parámetro         | Tipo         | Descripción                                                                                                                                                                                                                                                          | Obligatorio |
|-------------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| beneficiaries     | UserIdentity | Objeto UserIdentity que representa al usuario al que se están consultando productos.                                                                                                                                                                                           | Sí      |
| continuationToken | Cadena       | Si hay varios conjuntos de productos, el cuerpo de la respuesta devuelve un token de continuación cuando se alcanza el límite de la página. Proporciona ese token de continuación aquí en llamadas posteriores para recuperar los productos restantes.                                                      | No       |
| maxPageSize       | Número       | Número máximo de productos que puede devolver una respuesta. El valor predeterminado y máximo es de 100.                                                                                                                                                                      | No       |
| modifiedAfter     | Fecha y hora     | Si se especifica, el servicio devuelve solo los productos modificados después de esta fecha.                                                                                                                                                                             | No       |
| parentProductId   | Cadena       | Si se especifica, el servicio devuelve solo los productos IAP que corresponden a la aplicación especificada.                                                                                                                                                                                    | No       |
| productSkuIds     | Identificadores de SKU de producto | Si se especifica, el servicio devuelve solo los productos aplicables a los pares de producto o SKU proporcionados.                                                                                                                                                                        | No       |
| productTypes      | Cadena       | Si se especifica, el servicio devuelve solo los productos que coinciden con los tipos de producto especificados. Los tipos de producto admitidos son **Application**, **Durable** y **UnmanagedConsumable**.                                                                                       | No       |
| validityType      | cadena       | Si se establece en **All**, se devolverán todos los productos de un usuario, incluidos los artículos expirados. Si se establece en **Valid**, solo se devolverán los productos que sean válidos en este momento (es decir, que tengan un estado activo, una fecha de inicio anterior a la actual y una fecha final posterior a la actual). | No       |

 

El objeto UserIdentity contiene los parámetros siguientes.

| Parámetro            | Tipo   | Descripción                                                                                                                                                                                                                  | Obligatorio |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| identityType         | cadena | Especifique el valor de cadena **b2b**.                                                                                                                                                                                            | Sí      |
| identityValue        | cadena | Valor de cadena de la clave de id. de la Tienda Windows.                                                                                                                                                                                    | Sí      |
| localTicketReference | Cadena | Identificador solicitado para los productos devueltos. Los artículos devueltos en el cuerpo de la respuesta tendrán una *localTicketReference* coincidente. Se recomienda usar el mismo valor que la notificación *userId* de la clave de id. de la Tienda Windows. | Sí      |

 

El objeto ProductSkuId contiene los parámetros siguientes.

| Parámetro | Tipo   | Descripción                                                                                                                                                                                                                                                                                                            | Obligatorio |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| productId | Cadena | Identificador del producto del catálogo de la Tienda Windows. Para obtener el identificador del producto, navega hasta la aplicación en el panel del Centro de desarrollo de Windows, ve a la página **Administración de aplicaciones** &gt; **Identidad de aplicación** y recupera el sufijo de la cadena que se muestra en el campo **URL para Windows 10**. Un identificador de producto de ejemplo es "9WZDNCRFJ3Q8". | Sí      |
| skuID     | Cadena | Identificador de SKU del catálogo de la Tienda Windows. Un ejemplo de identificador de SKU es "0010".                                                                                                                                                                                                                                                | Sí      |

 

### Ejemplo de solicitud

```
POST https://collections.mp.microsoft.com/v6.0/collections/query HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q…….
Host: collections.mp.microsoft.com
Content-Length: 2531
Content-Type: application/json

{
    "maxPageSize": 100,
    "beneficiaries": [
            {
                "localTicketReference": "1055521810674918",
                "identityValue": "eyJ0eXAiOiJ……",
                "identityType": "b2b"
            }
        ],
    "modifiedAfter": "\/Date(-62135568000000)\/",
    "productSkuIds": [
            {
                "productId": "9NBLGGH5WVP6",
                "skuId": "0010"
            }
        ],
    "productTypes": [
            "UnmanagedConsumable"
        ],
    "validityType": "All"
}
```

## Respuesta


### Cuerpo de la respuesta

| Parámetro         | Tipo                     | Descripción                                                                                                                                                                                | Obligatorio |
|-------------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| continuationToken | Cadena                   | Si hay varios conjuntos de productos, este token se devuelve cuando se alcanza el límite de la página. Puedes especificar este token de continuación en llamadas posteriores para recuperar los productos restantes. | No       |
| Elementos             | CollectionItemContractV6 | Matriz de productos para el usuario especificado.                                                                                                                                               | No       |

 

El objeto CollectionItemContractV6 contiene los parámetros siguientes.

| Parámetro            | Tipo               | Descripción                                                                                                                                        | Obligatorio |
|----------------------|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| acquiredDate         | datetime           | Fecha en que el usuario compró el artículo.                                                                                                      | Sí      |
| campaignId           | Cadena             | Identificador de campaña que se proporcionó al realizar la compra del artículo.                                                                                  | No       |
| devOfferId           | Cadena             | Identificador de la oferta de una compra desde la aplicación.                                                                                                              | No       |
| endDate              | datetime           | Fecha de finalización del artículo.                                                                                                                          | Sí      |
| fulfillmentData      | Cadena             | No disponible                                                                                                                                                | No       |
| inAppOfferToken      | Cadena             | Identificador del producto especificado por el desarrollador que se asignó al artículo en el panel del Centro de desarrollo de Windows. Un ejemplo de identificador del producto es "product123". | No       |
| itemId               | Cadena             | Id. que identifica este artículo de colección de otros artículos que posee el usuario. Este identificador es único para cada producto.                                          | Sí      |
| localTicketReference | Cadena             | Identificador de la `localTicketReference` suministrada anteriormente en el cuerpo de la solicitud.                                                                      | Sí      |
| modifiedDate         | datetime           | Fecha de la última modificación de este artículo.                                                                                                              | Sí      |
| orderId              | Cadena             | Si está presente, el identificador del objeto del que se obtuvo este artículo.                                                                                          | No       |
| orderLineItemId      | Cadena             | Si está presente, el artículo de línea de un pedido concreto para el que se obtuvo el artículo.                                                                | No       |
| ownershipType        | Cadena             | Cadena "OwnedByBeneficiary".                                                                                                                   | Sí      |
| productId            | Cadena             | Identificador del producto del catálogo de la Tienda Windows. Un identificador de producto de ejemplo es "9WZDNCRFJ3Q8".                                                            | Sí      |
| productType          | cadena             | Uno de los siguientes tipos de producto: **Application**, **Durable** y **UnmanagedConsumable**.                                                     | Sí      |
| purchasedCountry     | cadena             | N disponible.                                                                                                                                               | No       |
| purchaser            | IdentityContractV6 | Si está presente, representa la identidad del comprador del artículo. Consulta los detalles de este objeto a continuación.                                      | No       |
| Quantity             | Número             | Cantidad del artículo. Actualmente, el valor siempre será 1.                                                                                        | No       |
| skuId                | Cadena             | Identificador de SKU del catálogo de la Tienda Windows. Un ejemplo de identificador de SKU es "0010".                                                                            | Sí      |
| skuType              | cadena             | Tipo de la SKU. Entre los valores posibles se incluyen **Trial**, **Full** y **Rental**.                                                                      | Sí      |
| startDate            | datetime           | Fecha en que el artículo comienza a ser válido.                                                                                                         | Sí      |
| Estado               | cadena             | Estado del artículo. Entre los valores posibles se incluyen **Active**, **Expired**, **Revoked** y **Banned**.                                              | Sí      |
| Tags                 | cadena             | No disponible                                                                                                                                                | Sí      |
| transactionId        | guid               | El identificador de transacción como resultado de la compra de este artículo. Se puede usar para notificar la cumplimentación de un artículo.                                       | Sí      |

 

El objeto IdentityContractV6 contiene los parámetros siguientes.

| Parámetro     | Tipo   | Descripción                                                                        | Obligatorio |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | cadena | Contiene el valor **"pub"**.                                                      | Sí      |
| identityValue | cadena | El valor de cadena del elemento *publisherUserId* de la clave de id. de la Tienda Windows especificada. | Sí      |

 

### Ejemplo de respuesta

```
HTTP/1.1 200 OK
Content-Length: 7241
Content-Type: application/json
MS-CorrelationId: 699681ce-662c-4841-920a-f2269b2b4e6c
MS-RequestId: a9988cf9-652b-4791-beba-b0e732121a12
MS-CV: xu2HW6SrSkyfHyFh.0.1
MS-ServerId: 020022359
Date: Tue, 22 Sep 2015 20:28:18 GMT

{
    "items" : [
        {
            "acquiredDate" : "2015-09-22T19:22:51.2068724+00:00",
            "devOfferId" : "f9587c53-540a-498b-a281-8a349491ed47",
            "endDate" : "9999-12-31T23:59:59.9999999+00:00",
            "fulfillmentData" : [],
            "inAppOfferToken" : "consumable2",
            "itemId" : "4b8fbb13127a41f299270ea668681c1d",
            "localTicketReference" : "1055521810674918",
            "modifiedDate" : "2015-09-22T19:22:51.2513155+00:00",
            "orderId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31",
            "ownershipType" : "OwnedByBeneficiary",
            "productId" : "9NBLGGH5WVP6",
            "productType" : "UnmanagedConsumable",
            "purchaser" : {
                "identityType" : "pub",
                "identityValue" : "user123"
            },
            "skuId" : "0010",
            "skuType" : "Full",
            "startDate" : "2015-09-22T19:22:51.2068724+00:00",
            "status" : "Active",
            "tags" : [],
            "transactionId" : "4ba5960d-4ec6-4a81-ac20-aafce02ddf31"
        }
    ]
}
```

## Temas relacionados

* [Ver y conceder productos desde un servicio](view-and-grant-products-from-a-service.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Renovar una clave de id. de la Tienda Windows](renew-a-windows-store-id-key.md)


<!--HONumber=Mar16_HO1-->


