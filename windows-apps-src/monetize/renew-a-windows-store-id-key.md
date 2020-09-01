---
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description: Obtenga información sobre cómo renovar una clave de identificador de Microsoft Store expirada mediante el método Renew en la colección Microsoft Store y las API de compra.
title: Renovar una clave de identificador de Microsoft Store
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, API de colección de Microsoft Store, API de compra Microsoft Store, clave de identificador Microsoft Store, renovar
ms.localizationpriority: medium
ms.openlocfilehash: fe19b446f88e16b87ff40288f5d4480f9230469e
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238260"
---
# <a name="renew-a-microsoft-store-id-key"></a>Renovar una clave de identificador de Microsoft Store


Utilice este método para renovar una clave de Microsoft Store. Cuando se [genera una clave de identificador de Microsoft Store](view-and-grant-products-from-a-service.md#step-4), la clave es válida durante 90 días. Cuando la clave expira, puedes usarla para renegociar una nueva clave mediante este método.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, necesitarás:

* Azure AD token de acceso que tiene el valor de URI de audiencia `https://onestore.microsoft.com` .
* Una clave de identificador de Microsoft Store expirada que se [generó a partir del código de cliente en la aplicación](view-and-grant-products-from-a-service.md#step-4).

Para obtener más información, consulte [Administración de derechos de producto desde un servicio](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Tipo de clave    | Método | URI de solicitud                                              |
|-------------|--------|----------------------------------------------------------|
| Colecciones | POST   | ```https://collections.mp.microsoft.com/v6.0/b2b/keys/renew``` |
| Purchase    | POST   | ```https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew```    |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | string | Debe establecerse en el valor **collections.mp.microsoft.com** o **purchase.mp.microsoft.com**.           |
| Content-Length | number | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | string | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |


### <a name="request-body"></a>Cuerpo de la solicitud

| Parámetro     | Tipo   | Descripción                       | Obligatorio |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | string | Token de acceso de Azure AD.        | Sí      |
| key           | string | La clave de identificador de Microsoft Store expirada. | Sí       |


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

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Parámetro | Tipo   | Descripción                                                                                                            |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|
| key       | string | La clave de Microsoft Store actualizada que se puede usar en futuras llamadas a la API de colecciones de Microsoft Store o a la API de compra. |


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
| 401  | No autorizado | InconsistentClientId       | La claim de *clientId* en la clave de identificador de Microsoft Store y la claim de *AppID* en el token de acceso Azure ad no coinciden.                                                                     |


## <a name="related-topics"></a>Temas relacionados


* [Administrar los derechos de producto de un servicio](view-and-grant-products-from-a-service.md)
* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)
