---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Usa este método en la API de envío de Microsoft Store para crear un complemento para una aplicación que está registrada en tu cuenta de PartnerCenter.
title: Crear un complemento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, create add-on, crear complemento, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 8465dc7a42961a20fcd33ba8d43c71e2d73727ff
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874122"
---
# <a name="create-an-add-on"></a>Crear un complemento

Usa este método en la API de envío de Microsoft Store para crear un complemento (también conocido como producto la aplicación o IAP) para una aplicación que está registrada en tu cuenta del centro de partners.

> [!NOTE]
> Este método crea un complemento sin envíos. Para crear un envío de un complemento, consulta los métodos en [Administrar envíos de complementos](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Token del** &lt;*portador*&gt;. |


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

|  Parámetro  |  Tipo  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  applicationIds  |  matriz  |  Una matriz que contiene la Id. de Store de la aplicación asociada con este complemento. Solo se admite un elemento en esta matriz.   |  Sí  |
|  productId  |  string  |  La Id. de producto del complemento. Esto es un identificador que puede usarse en el código para hacer referencia al complemento. Para obtener más información, consulta [Establecimiento del tipo de producto y la Id. del producto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Sí  |
|  productType  |  string  |  El tipo de producto del complemento. Se admiten los siguientes valores: **Duradero** y **Consumible**.  |  Sí  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo complemento de consumible para una aplicación.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. Para obtener más información acerca de los valores en el cuerpo de la respuesta, consulta [Recurso de complemento](manage-add-ons.md#add-on-object).

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción                                                                                                                                                                           |
|--------|------------------|
| 400  | La solicitud no es válida. |
| 409  | No se pudo crear el complemento debido a su estado actual o el complemento usa una característica de centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complementos](manage-add-on-submissions.md)
* [Obtención de todos los complementos](get-all-add-ons.md)
* [Obtención de un complemento](get-an-add-on.md)
* [Eliminación de un complemento](delete-an-add-on.md)
