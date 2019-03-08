---
ms.assetid: FA55C65C-584A-4B9B-8451-E9C659882EDE
description: Usa este método en la API de compras de Microsoft Store para conceder de forma gratuita una aplicación o complemento a un usuario determinado.
title: Conceder productos gratuitos
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, API de compra de Microsoft Store, Microsoft Store purchase API, conceder productos, grant products
ms.localizationpriority: medium
ms.openlocfilehash: 957958891b1052be4ac9ae65d90f97ff8a44ef36
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613200"
---
# <a name="grant-free-products"></a>Conceder productos gratuitos

Usa este método en la API de compras de Microsoft Store para conceder de forma gratuita una aplicación o complemento (también conocido como producto desde la aplicación o IAP) a un usuario determinado.

Actualmente, solo puedes conceder productos gratuitos. Si el servicio intenta usar este método para conceder un producto que no es gratuito, este método devolverá un error.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, necesitarás:

* Un token de acceso de Azure AD que tiene el valor de URI de audiencia `https://onestore.microsoft.com`.
* Una clave del identificador de Microsoft Store que representa la identidad del usuario al que quieres conceder un producto gratuito.

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                            |
|--------|--------------------------------------------------------|
| POST   | ```https://purchase.mp.microsoft.com/v6.0/purchases/grant``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorización  | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;.                           |
| Host           | string | Debe establecerse en el valor **purchase.mp.microsoft.com**.                                            |
| Content-Length | número | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | string | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |


### <a name="request-body"></a>Cuerpo de la solicitud

| Parámetro      | Tipo   | Descripción        | Requerido |
|----------------|--------|---------------------|----------|
| availabilityId | string | El identificador de disponibilidad del producto que debe concederse a través del catálogo de Microsoft Store.         | Sí      |
| b2bKey         | string | La [clave de identificador de Microsoft Store](view-and-grant-products-from-a-service.md#step-4) que representa la identidad del usuario al que quieres conceder un producto.    | Sí      |
| devOfferId     | string | Un identificador de la oferta especificado por el desarrollador que aparecerá en el elemento de colección después de la adquisición.        |
| language       | string | Idioma del usuario.  | Sí      |
| market         | string | Mercado del usuario.       | Sí      |
| orderId        | guid   | GUID que se genera para el pedido. Este valor debe ser exclusivo del usuario, pero no es obligatorio que lo sea en todos los pedidos.    | Sí      |
| productId      | string | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para el [producto](in-app-purchases-and-trials.md#products-skus-and-availabilities) del catálogo de Microsoft Store. Un ejemplo de identificador de la Store para un producto es 9NBLGGH42CFD. | Sí      |
| quantity       | entero    | La cantidad que se va a comprar. Actualmente, el único valor admitido es 1. Si no se especifica, el valor predeterminado es 1.   | No       |
| skuId          | string | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para la [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) del producto del catálogo de Microsoft Store. Un ejemplo de identificador de la Store para un SKU es 0010.     | Sí      |


### <a name="request-example"></a>Ejemplo de solicitud

```syntax
POST https://purchase.mp.microsoft.com/v6.0/purchases/grant HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJK……
Content-Length: 1863
Content-Type: application/json

{
    "b2bKey" : "eyJ0eXAiOiJK……",
    "availabilityId" : "9RT7C09D5J3W",
    "productId" : "9NBLGGH5WVP6",
    "skuId" : "0010",
    "language" : "en-us",
    "market" : "us",
    "orderId" : "3eea1529-611e-4aee-915c-345494e4ee76",
}
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Parámetro                 | Tipo                        | Descripción             | Requerido |
|---------------------------|-----------------------------|-----------------------|----------|
| clientContext             | ClientContextV6             | Información contextual de cliente para este pedido. Se asignará al valor *clientID* del token de Azure AD.    | Sí      |
| createdtime               | datetimeoffset              | Hora de creación del pedido.         | Sí      |
| currencyCode              | string                      | Código de moneda para *totalAmount* y *totalTaxAmount*. No disponible para los artículos gratuitos.     | Sí      |
| friendlyName              | string                      | Nombre descriptivo del pedido. No disponible para los pedidos realizados a través de la API de compras de Microsoft Store. | Sí      |
| isPIRequired              | boolean                     | Indica si se requiere un instrumento de pago (PI) como parte del pedido de compra.  | Sí      |
| language                  | string                      | Identificador de idioma del pedido (por ejemplo, "en").       | Sí      |
| market                    | string                      | Identificador de mercado del pedido (por ejemplo, "EE. UU").  | Sí      |
| orderId                   | string                      | Id. que identifica el pedido para un usuario determinado.                | Sí      |
| orderLineItems            | list&lt;OrderLineItemV6&gt; | Lista de artículos de línea del pedido. Normalmente hay 1 artículo de línea por pedido.       | Sí      |
| orderState                | string                      | Estado del pedido. Los estados válidos son **Editing**, **CheckingOut**, **Pending**, **Purchased**, **Refunded**, **ChargedBack** y **Cancelled**. | Sí      |
| orderValidityEndTime      | string                      | Última vez que el precio del pedido es válido antes de su envío. No disponible para aplicaciones gratuitas.      | Sí      |
| orderValidityStartTime    | string                      | Primera vez que el precio del pedido es válido antes de su envío. No disponible para aplicaciones gratuitas.          | Sí      |
| purchaser                 | IdentityV6                  | Objeto que describe la identidad del comprador.       | Sí      |
| totalAmount               | decimal                     | Importe total de la compra de todos los artículos del pedido incluidos los impuestos.       | Sí      |
| totalAmountBeforeTax      | decimal                     | El importe total de la compra de todos los artículos del pedido antes de impuestos.      | Sí      |
| totalChargedToCsvTopOffPI | decimal                     | En caso de usar un instrumento de pago y un valor almacenado (CSV) independientes, el importe cobrado al CSV.            | Sí      |
| totalTaxAmount            | decimal                     | Importe total de los impuestos para todos los artículos de línea.    | Sí      |


El objeto ClientContext contiene los parámetros siguientes.

| Parámetro | Tipo   | Descripción                           | Requerido |
|-----------|--------|---------------------------------------|----------|
| cliente    | string | Identificador de cliente que creó el pedido. | No       |


El objeto OrderLineItemV6 contiene los parámetros siguientes.

| Parámetro               | Tipo           | Descripción                                                                                                  | Requerido |
|-------------------------|----------------|--------------------------------------------------------------------------------------------------------------|----------|
| agente                   | IdentityV6     | Agente que editó por última vez el artículo de línea. Para obtener más información acerca de este objeto, consulte la tabla siguiente.       | No       |
| availabilityId          | string         | Identificador de disponibilidad del producto que debe adquirirse a través del catálogo de Microsoft Store.                           | Sí      |
| beneficiary             | IdentityV6     | La identidad del beneficiario del pedido.                                                                | No       |
| billingState            | string         | Estado de facturación del pedido. Se establece en **Charged** cuando se completa.                                   | No       |
| campaignId              | string         | Identificador de campaña de este pedido.                                                                              | No       |
| currencyCode            | string         | El código de moneda que se usa para los detalles del precio.                                                                    | Sí      |
| description             | string         | Descripción localizada del artículo de línea.                                                                    | Sí      |
| devofferId              | string         | Identificador de la oferta del pedido concreto, si está presente.                                                           | No       |
| fulfillmentDate         | datetimeoffset | Fecha de cumplimentación.                                                                           | No       |
| fulfillmentState        | string         | Estado de cumplimentación de este artículo. Se establece en **Fulfilled** cuando se completa.                      | No       |
| isPIRequired            | boolean        | Indica si se requiere un instrumento de pago para este artículo de línea.                                       | Sí      |
| isTaxIncluded           | boolean        | Indica si se incluyen los impuestos en los detalles de precio del artículo.                                        | Sí      |
| legacyBillingOrderId    | string         | Identificador de facturación heredado.                                                                                       | No       |
| lineItemId              | string         | Identificador de artículo de línea para el artículo de este pedido.                                                                 | Sí      |
| listPrice               | decimal        | Precio de lista del artículo de este pedido.                                                                    | Sí      |
| productId               | string         | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para el [producto](in-app-purchases-and-trials.md#products-skus-and-availabilities) que representa el elemento de la línea en el catálogo de Microsoft Store. Un ejemplo de identificador de la Store para un producto es 9NBLGGH42CFD.   | Sí      |
| productType             | string         | Tipo de producto. Los valores admitidos son **Durable**, **Application** y **UnmanagedConsumable**. | Sí      |
| quantity                | entero            | Cantidad del artículo pedido.                                                                            | Sí      |
| retailPrice             | decimal        | Precio de venta del artículo pedido.                                                                        | Sí      |
| revenueRecognitionState | string         | Estado de reconocimiento de ingresos.                                                                               | Sí      |
| skuId                   | string         | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) para la [SKU](in-app-purchases-and-trials.md#products-skus-and-availabilities) del artículo de línea en el catálogo de Microsoft Store. Un ejemplo de identificador de la Store para un SKU es 0010.                                                                   | Sí      |
| taxAmount               | decimal        | Importe de impuestos del artículo de línea.                                                                            | Sí      |
| taxType                 | string         | Tipo de impuesto para los impuestos aplicables.                                                                       | Sí      |
| Título                   | string         | Título localizado del artículo de línea.                                                                        | Sí      |
| totalAmount             | decimal        | Importe total de compra del artículo de línea incluidos los impuestos.                                                    | Sí      |


El objeto IdentityV6 contiene los parámetros siguientes.

| Parámetro     | Tipo   | Descripción                                                                        | Requerido |
|---------------|--------|------------------------------------------------------------------------------------|----------|
| identityType  | string | Contiene el valor **"pub"**.                                                      | Sí      |
| identityValue | string | El valor de cadena del elemento *publisherUserId* de la clave de Id. de Microsoft Store especificada. | Sí      |


### <a name="response-example"></a>Ejemplo de respuesta

```syntax
Content-Length: 1203
Content-Type: application/json
MS-CorrelationId: fb2e69bc-f26a-4aab-a823-7586c19f5762
MS-RequestId: c1bc832c-f742-47e4-a76c-cf061402f698
MS-CV: XjfuNWLQlEuxj6Mt.8
MS-ServerId: 030032362
Date: Tue, 13 Oct 2015 21:21:51 GMT

{
    "clientContext": {
        "client": "86b78998-d05a-487b-b380-6c738f6553ea"
    },
    "createdTime": "2015-10-13T21:21:51.1863494+00:00",
    "currencyCode": "USD",
    "isPIRequired": false,
    "language": "en-us",
    "market": "us",
    "orderId": "3eea1529-611e-4aee-915c-345494e4ee76",
    "orderLineItems": [{
        "availabilityId": "9RT7C09D5J3W",
        "beneficiary": {
            "identityType": "pub",
            "identityValue": "user1"
        },
        "billingState": "Charged",
        "currencyCode": "USD",
        "description": "Jewels, Jewels, Jewels - Consumable 2",
        "fulfillmentDate": "2015-10-13T21:21:51.639478+00:00",
        "fulfillmentState": "Fulfilled",
        "isPIRequired": false,
        "isTaxIncluded": true,
        "lineItemId": "2814d758-3ee3-46b3-9671-4fb3bdae9ffe",
        "listPrice": 0.0,
        "payments": [],
        "productId": "9NBLGGH5WVP6",
        "productType": "UnmanagedConsumable",
        "quantity": 1,
        "retailPrice": 0.0,
        "revenueRecognitionState": "None",
        "skuId": "0010",
        "taxAmount": 0.0,
        "taxType": "NoApplicableTaxes",
        "title": "Jewels, Jewels, Jewels - Consumable 2",
        "totalAmount": 0.0
    }],
    "orderState": "Purchased",
    "orderValidityEndTime": "2015-10-14T21:21:51.1863494+00:00",
    "orderValidityStartTime": "2015-10-13T21:21:51.1863494+00:00",
    "purchaser": {
        "identityType": "pub",
        "identityValue": "user1"
    },
    "testScenarios": "None",
    "totalAmount": 0.0,
    "totalTaxAmount": 0.0
}
```

## <a name="error-codes"></a>Códigos de error


| Código | Error        | Código de error interno           | Descripción   |
|------|--------------|----------------------------|----------------|
| 401  | Sin autorización | AuthenticationTokenInvalid | El token de acceso de Azure AD no es válido. En algunos casos, los detalles del código ServiceError contendrán más información, como la fecha de expiración del token o si falta la notificación *appid*. |
| 401  | Sin autorización | PartnerAadTicketRequired   | No se pasó un token de acceso de Azure AD al servicio en el encabezado de autorización.   |
| 401  | Sin autorización | InconsistentClientId       | La notificación *clientId* de la clave de id. de Microsoft Store del cuerpo de la solicitud y la notificación *appid* del token de acceso de Azure AD del encabezado de autorización no coinciden.       |
| 400  | BadRequest   | InvalidParameter           | Los detalles contienen información sobre el cuerpo de la solicitud y los campos que tienen un valor no válido.           |

<span/> 

## <a name="related-topics"></a>Temas relacionados

* [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md)
* [Consultar productos](query-for-products.md)
* [Informe de productos consumibles que cumpla](report-consumable-products-as-fulfilled.md)
* [Renovar una clave de Id. de Microsoft Store](renew-a-windows-store-id-key.md)
