---
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: Use este método en el Microsoft Store API de envío para crear un complemento para una aplicación que esté registrada en su cuenta de PartnerCenter.
title: Crear un complemento
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de envío, crear complemento, producto en la aplicación, IAP
ms.localizationpriority: medium
ms.openlocfilehash: a54ad4bcdb0c6fd652d56c767aa8362073b07875
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167729"
---
# <a name="create-an-add-on"></a>Crear un complemento

Use este método en el Microsoft Store API de envío para crear un complemento (también conocido como producto de la aplicación o IAP) para una aplicación registrada en la cuenta del centro de Partners.

> [!NOTE]
> Este método crea un complemento sin envíos. Para crear un envío de un complemento, consulta los métodos en [Administración de envíos de complementos](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

|  Parámetro  |  Tipo  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  applicationIds  |  array  |  Una matriz que contiene la Id. de la tienda de la aplicación asociada con este complemento. Solo se admite un elemento en esta matriz.   |  Sí  |
|  productId  |  string  |  Id. del producto del complemento. Esto es un identificador que puede usarse en el código para hacer referencia al complemento. Para obtener más información, consulta [Establecer el tipo de producto y el id. del producto](../publish/set-your-add-on-product-id.md).  |  Sí  |
|  productType  |  string  |  Tipo de producto del complemento. Se admiten los siguientes valores: **Duradero** y **Consumible**.  |  Sí  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo complemento de consumible para una aplicación.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>Response

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
| 409  | No se pudo crear el complemento debido a su estado actual, o el complemento usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Manage add-on submissions (Administrar envíos de complemento)](manage-add-on-submissions.md)
* [Obtención de todos los complementos](get-all-add-ons.md)
* [Obtención de un complemento](get-an-add-on.md)
* [Eliminar un complemento](delete-an-add-on.md)