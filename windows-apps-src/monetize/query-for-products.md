---
author: Xansky
ms.assetid: D1F233EC-24B5-4F84-A92F-2030753E608E
description: Usa este método en la API de colecciones de Microsoft Store para obtener todos los productos que posee un cliente para las aplicaciones asociadas a tu identificador de cliente de Azure AD. Puedes definir el ámbito de la consulta para un producto concreto, o bien usar otros filtros.
title: Consultar productos
ms.author: mhopkins
ms.date: 03/16/2018
ms.topic: article
keywords: windows 10, uwp, API de colecciones de Microsoft Store, ver productos, Microsoft Store collection API, view products
ms.localizationpriority: medium
ms.openlocfilehash: 1dded9b66fbae4f65b936335eda406d8773420c4
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5865863"
---
# <a name="query-for-products"></a>Consultar productos


Usa este método en la API de colecciones de Microsoft Store para obtener todos los productos que posee un cliente para las aplicaciones asociadas a tu identificador de cliente de Azure AD. Puedes definir el ámbito de la consulta para un producto concreto, o bien usar otros filtros.

Este método está diseñado para que el servicio lo llame en respuesta a un mensaje de la aplicación. El servicio no debe sondear regularmente a todos los usuarios en una programación.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, necesitarás:

* Un token de acceso de Azure AD que tiene el valor de URI de audiencia `https://onestore.microsoft.com`.
* Una clave de identificador de Microsoft Store que represente la identidad del usuario cuyos productos quieres obtener.

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                 |
|--------|-------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/query``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Authorization  | cadena | Obligatorio. Token de acceso de Azure AD con formato **Portador** &lt;*token*&gt;.                           |
| Host           | string | Debe establecerse en el valor **collections.mp.microsoft.com**.                                            |
| Content-Length | number | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | string | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |


### <a name="request-body"></a>Cuerpo de la solicitud

| Parámetro         | Tipo         | Descripción         | Obligatorio |
|-------------------|--------------|---------------------|----------|
| beneficiaries     | UserIdentity | Objeto UserIdentity que representa al usuario al que se están consultando productos. Para obtener más información, consulta la tabla siguiente.    | Sí      |
| continuationToken | string       | Si hay varios conjuntos de productos, el cuerpo de la respuesta devuelve un token de continuación cuando se alcanza el límite de la página. Proporciona ese token de continuación aquí en llamadas posteriores para recuperar los productos restantes.       | No       |
| maxPageSize       | number       | Número máximo de productos que puede devolver una respuesta. El valor predeterminado y máximo es de 100.                 | No       |
| modifiedAfter     | datetime     | Si se especifica, el servicio devuelve solo los productos modificados después de esta fecha.        | No       |
| parentProductId   | string       | Si se especifica, el servicio devuelve solo los complementos que corresponden a la aplicación especificada.      | No       |
| productSkuIds     | list&lt;ProductSkuId&gt; | Si se especifica, el servicio devuelve solo los productos aplicables a los pares de producto o SKU proporcionados. Para obtener más información, consulta la tabla siguiente.      | No       |
| productTypes      | string       | Si se especifica, el servicio devuelve solo los productos que coinciden con los tipos de producto especificados. Los tipos de producto admitidos son **Application**, **Durable** y **UnmanagedConsumable**.     | No       |
| validityType      | string       | Si se establece en **All**, se devolverán todos los productos de un usuario, incluidos los artículos expirados. Si se establece en **Valid**, solo se devolverán los productos que sean válidos en este momento (es decir, que tengan un estado activo, una fecha de inicio anterior a la actual &lt; y una fecha final posterior &gt; a la actual). | No       |


El objeto UserIdentity contiene los parámetros siguientes.

| Parámetro            | Tipo   |  Descripción      | Obligatorio |
|----------------------|--------|----------------|----------|
| identityType         | cadena | Especifica el valor de cadena **b2b**.    | Sí      |
| identityValue        | string | La [clave de identificador de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa la identidad del usuario cuyos productos quieres consultar.  | Sí      |
| localTicketReference | cadena | El identificador solicitado para los productos devueltos. Los artículos devueltos en el cuerpo de la respuesta tendrán un parámetro *localTicketReference* coincidente. Se recomienda usar el mismo valor que la notificación *userId* de la clave de id. de Microsoft Store. | Sí      |


El objeto ProductSkuId contiene los parámetros siguientes.

| Parámetro | Tipo   | Descripción          | Obligatorio |
|-----------|--------|----------------------|----------|
| productId | string | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para un [producto](in-app-purchases-and-trials.md#products-skus-and-availabilities) del catálogo de Microsoft Store. Un ejemplo de identificador de la Store para un producto es 9NBLGGH42CFD. | Sí      |
| skuID     | string | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para la [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) de un producto del catálogo de Microsoft Store. Un ejemplo de identificador de la Store para un SKU es 0010.       | Sí      |


### <a name="request-example"></a>Ejemplo de solicitud

```syntax
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

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Parámetro         | Tipo                     | Descripción          | Obligatorio |
|-------------------|--------------------------|-----------------------|----------|
| continuationToken | string                   | Si hay varios conjuntos de productos, este token se devuelve cuando se alcanza el límite de la página. Puedes especificar este token de continuación en llamadas posteriores para recuperar los productos restantes. | No       |
| elementos             | CollectionItemContractV6 | Matriz de productos para el usuario especificado. Para obtener más información, consulta la tabla siguiente.        | No       |


El objeto CollectionItemContractV6 contiene los parámetros siguientes.

| Parámetro            | Tipo               | Descripción            | Obligatorio |
|----------------------|--------------------|-------------------------|----------|
| acquiredDate         | datetime           | Fecha en que el usuario compró el artículo.                  | Sí      |
| campaignId           | string             | Identificador de campaña que se proporcionó al realizar la compra del artículo.                  | No       |
| devOfferId           | string             | El identificador de la oferta de una compra desde la aplicación.              | No       |
| endDate              | datetime           | La fecha de finalización del artículo.              | Sí      |
| fulfillmentData      | string             | N/D         | No       |
| inAppOfferToken      | string             | Identificador del producto especificado por el desarrollador que se asignó al artículo en el panel del Centro de desarrollo de Windows. Un ejemplo de identificador de producto es *product123*. | No       |
| itemId               | string             | Id. que identifica este artículo de colección de otros artículos que posee el usuario. Este identificador es único para cada producto.   | Sí      |
| localTicketReference | string             | Id. de *localTicketReference* que se suministró previamente en el cuerpo de la solicitud.                  | Sí      |
| modifiedDate         | datetime           | Fecha de la última modificación de este artículo.              | Sí      |
| orderId              | string             | Si está presente, el identificador del objeto del que se obtuvo este artículo.              | No       |
| orderLineItemId      | string             | Si está presente, el artículo de línea de un pedido concreto para el que se obtuvo el artículo.              | No       |
| ownershipType        | string             | La cadena *OwnedByBeneficiary*.   | Sí      |
| productId            | string             | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para el [producto](in-app-purchases-and-trials.md#products-skus-and-availabilities) del catálogo de Microsoft Store. Un ejemplo de Id. de Store para un producto es 9NBLGGH42CFD.          | Sí      |
| productType          | string             | Uno de los siguientes tipos de producto: **Application**, **Durable** y **UnmanagedConsumable**.        | Sí      |
| purchasedCountry     | string             | N/D   | No       |
| purchaser            | IdentityContractV6 | Si está presente, representa la identidad del comprador del artículo. Consulta los detalles de este objeto a continuación.        | No       |
| quantity             | number             | Cantidad del artículo. Actualmente, el valor siempre será 1.      | No       |
| skuId                | string             | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para la [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) del producto del catálogo de Microsoft Store. Un ejemplo de Id. de Store para una SKU es 0010.     | Sí      |
| skuType              | string             | Tipo de la SKU. Entre los valores posibles se incluyen **Trial**, **Full** y **Rental**.        | Sí      |
| startDate            | datetime           | Fecha en que el artículo comienza a ser válido.       | Sí      |
| status               | string             | Estado del artículo. Entre los valores posibles se incluyen **Active**, **Expired**, **Revoked** y **Banned**.    | Sí      |
| tags                 | string             | N/D    | Sí      |
| transactionId        | guid               | El identificador de transacción como resultado de la compra de este artículo. Se puede usar para notificar la cumplimentación de un artículo.      | Sí      |


El objeto IdentityContractV6 contiene los parámetros siguientes.

| Parámetro     | Tipo   | Descripción                                                                        | Obligatorio |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | Contiene el valor *pub*.                                                      | Sí      |
| identityValue | string | El valor de cadena del elemento *publisherUserId* de la clave de Id. de Microsoft Store especificada. | Sí      |


### <a name="response-example"></a>Ejemplo de respuesta

```syntax
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

## <a name="related-topics"></a>Temas relacionados

* [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
* [Renovar una clave de id. de Microsoft Store](renew-a-windows-store-id-key.md)
