---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: Usa este método para renovar una clave de Microsoft Store.
title: Renovar una clave de id. de Microsoft Store
ms.date: 03/16/2018
ms.topic: article
keywords: windows 10, uwp, API de colecciones de Microsoft Store, API de compras de Microsoft Store, clave de identificador de Microsoft Store, renovar, Microsoft Store collection API, Microsoft Store purchase API, Microsoft Store ID key, renew
ms.localizationpriority: medium
ms.openlocfilehash: 0f1c6248b2d87a68b77cad6f1bdc7cce0fae587e
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8193197"
---
# <a name="renew-a-microsoft-store-id-key"></a>Renovar una clave de id. de Microsoft Store


Usa este método para renovar una clave de Microsoft Store. Al [generar una clave de id. de Microsoft Store](view-and-grant-products-from-a-service.md#step-4), la clave es válida durante 90 días. Cuando la clave expira, puedes usarla para renegociar una nueva clave mediante este método.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, necesitarás:

* Un token de acceso de Azure AD que tiene el valor de URI de audiencia `https://onestore.microsoft.com`.
* La clave de id. de Microsoft Store expirada que se haya [generado a partir del código de cliente en la aplicación](view-and-grant-products-from-a-service.md#step-4).

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Tipo de clave    | Método | URI de la solicitud                                              |
|-------------|--------|----------------------------------------------------------|
| Colecciones | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Compra    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | string | Debe establecerse en el valor **collections.mp.microsoft.com** o **purchase.mp.microsoft.com**.           |
| Content-Length | number | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | string | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |


### <a name="request-body"></a>Cuerpo de la solicitud

| Parámetro     | Tipo   | Descripción                       | Obligatorio |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | string | Token de acceso de Azure AD.        | Sí      |
| key           | string | Clave de id. de Microsoft Store expirada. | Sí       |


### <a name="request-example"></a>Ejemplo de solicitud

```syntax
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Parámetro | Tipo   | Descripción                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| key       | string | Clave de Microsoft Store actualizada que se puede usar en futuras llamadas a la API de colecciones o la API de compras de Microsoft Store. |


### <a name="response-example"></a>Ejemplo de respuesta

```syntax
HTTP/1.1 200 OK
Content-Length: 1646
Content-Type: application/json
MS-CorrelationId: bfebe80c-ff89-4c4b-8897-67b45b916e47
MS-RequestId: 1b5fa630-d672-4971-b2c0-3713f4ea6c85
MS-CV: xu2HW6SrSkyfHyFh.0.0
MS-ServerId: 030011428
Date: Tue, 13 Sep 2015 07:31:12 GMT

{
    "key":"eyJ0eXAi….."
}
```

## <a name="error-codes"></a>Códigos de error


| Código | Error        | Código de error interno           | Descripción   |
|------|--------------|----------------------------|---------------|
| 401  | No autorizado | AuthenticationTokenInvalid | El token de acceso de Azure AD no es válido. En algunos casos, los detalles del código ServiceError contendrán más información, como la fecha de expiración del token o si falta la notificación *appid*. |
| 401  | No autorizado | InconsistentClientId       | La notificación *clientId* de la clave de Id. de Microsoft Store y la notificación *appid* del token de acceso de Azure AD no coinciden.                                                                     |


## <a name="related-topics"></a>Temas relacionados


* [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md)
* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
