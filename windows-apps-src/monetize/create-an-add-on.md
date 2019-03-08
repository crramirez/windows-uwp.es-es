---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Use este método en la API de envío de Microsoft Store para crear un complemento para una aplicación que está registrado en su cuenta PartnerCenter.
title: Crear un complemento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, create add-on, crear complemento, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 8465dc7a42961a20fcd33ba8d43c71e2d73727ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651040"
---
# <a name="create-an-add-on"></a>Crear un complemento

Utilice este método en la API de envío de Microsoft Store para crear un complemento (también conocido como aplicación producto o IAP) para una aplicación que está registrado en la cuenta del centro de partners.

> [!NOTE]
> Este método crea un complemento sin envíos. Para crear un envío de un complemento, consulta los métodos en [Administración de envíos de complementos](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

|  Parámetro  |  Tipo  |  Descripción  |  Requerido  |
|------|------|------|------|
|  applicationIds  |  array  |  Una matriz que contiene la Id. de la tienda de la aplicación asociada con este complemento. Solo se admite un elemento en esta matriz.   |  Sí  |
|  productId  |  string  |  Id. del producto del complemento. Esto es un identificador que puede usarse en el código para hacer referencia al complemento. Para obtener más información, consulta [Establecer el tipo de producto y el id. del producto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Sí  |
|  productType  |  string  |  Tipo de producto del complemento. Se admiten los siguientes valores: **Durable** y **consumibles**.  |  Sí  |


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

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información acerca de los valores en el cuerpo de la respuesta, consulta [Recurso de complemento](manage-add-ons.md#add-on-object).

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
| 409  | El complemento no se pudo crear debido a su estado actual, o el complemento utiliza una característica del centro de partners que está [no compatible actualmente con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complemento](manage-add-on-submissions.md)
* [Obtener todos los complementos](get-all-add-ons.md)
* [Obtener un complemento](get-an-add-on.md)
* [Eliminar un complemento](delete-an-add-on.md)
