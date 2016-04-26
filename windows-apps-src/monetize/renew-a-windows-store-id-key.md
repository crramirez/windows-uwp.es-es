---
author: mcleanbyron
ms.assetid: 3569C505-8D8C-4D85-B383-4839F13B2466
description. Usa este método para renovar una clave de la Tienda Windows.
title: Renovar una clave de id. de la Tienda Windows
---

# Renovar una clave de id. de la Tienda Windows


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa este método para renovar una clave de la Tienda Windows. Cuando se genera una clave de id. de la Tienda Windows mediante una llamada al método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) o [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675), la clave es válida durante 90 días. Cuando la clave expira, puedes usarla para renegociar una nueva clave mediante este método.

## Requisitos previos


Para usar este método, necesitarás:

-   Un token de acceso de Azure AD que se creó con la URI de audiencia **https://onestore.microsoft.com**.
-   Una clave de id. de la Tienda Windows expirada que se generó mediante una llamada al método [**GetCustomerCollectionsIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608674) o [**GetCustomerPurchaseIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt608675) desde el código de cliente de la aplicación.

Para obtener más información, consulta [Ver y conceder productos desde un servicio](view-and-grant-products-from-a-service.md).

## Solicitud


### Sintaxis de la solicitud

| Tipo de clave    | Método | URI de solicitud                                              |
|-------------|--------|----------------------------------------------------------|
| Colecciones | POST   | https://collections.mp.microsoft.com/v6.0/b2b/keys/renew |
| Compra    | POST   | https://purchase.mp.microsoft.com/v6.0/b2b/keys/renew    |

 

### Encabezado de la solicitud

| Encabezado         | Tipo   | Descripción                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Host           | cadena | Debe establecerse en el valor **collections.mp.microsoft.com** o **purchase.mp.microsoft.com**.           |
| Content-Length | número | Longitud del cuerpo de la solicitud.                                                                       |
| Content-Type   | cadena | Especifica los tipos de solicitud y respuesta. Actualmente, el único valor admitido es **application/json**. |

 

### Cuerpo de la solicitud

| Parámetro     | Tipo   | Descripción                       | Obligatorio |
|---------------|--------|-----------------------------------|----------|
| serviceTicket | cadena | Token de acceso de Azure AD.        | Sí      |
| key           | cadena | Clave de id. de la Tienda Windows expirada. | No       |

 

### Ejemplo de solicitud

```
POST https://collections.mp.microsoft.com/v6.0/b2b/keys/renew HTTP/1.1
Content-Length: 2774
Content-Type: application/json
Host: collections.mp.microsoft.com

{ 
    "serviceTicket": "eyJ0eXAiOiJKV1QiLCJhb….",
    "Key": "eyJ0eXAiOiJKV1QiLCJhbG…."
}
```

## Respuesta


### Cuerpo de la respuesta

| Parámetro | Tipo   | Descripción                                                                                                            | Obligatorio |
|-----------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| clave       | cadena | Clave de la Tienda Windows actualizada que se puede usar en futuras llamadas a la API de colecciones o la API de compras de la Tienda Windows. | No       |

 

### Ejemplo de respuesta

```
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

## Códigos de error


| Código | Error        | Código de error interno           | Descripción                                                                                                                                                                           |
|------|--------------|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 401  | No autorizado | AuthenticationTokenInvalid | El token de acceso de Azure AD no es válido. En algunos casos, los detalles del código ServiceError contendrán más información, como la fecha de expiración del token o si falta la notificación *appid*. |
| 401  | No autorizado | InconsistentClientId       | La notificación *clientId* de la clave de Id. de la Tienda Windows y la notificación *appid* del token de acceso de Azure AD no coinciden.                                                                     |

 

## Temas relacionados


* [Ver y conceder productos desde un servicio](view-and-grant-products-from-a-service.md)
* [Consultar productos](query-for-products.md)
* [Notificar productos consumibles como completados](report-consumable-products-as-fulfilled.md)
* [Conceder productos gratuitos](grant-free-products.md)



<!--HONumber=Mar16_HO1-->


