---
author: mcleanbyron
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: "Usa este método en la API de envío de la Tienda Windows para crear un complemento de una aplicación que esté registrada en tu cuenta del Centro de desarrollo de Windows."
title: "Creación de un complemento mediante la API de envío de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, API de envío de la Tienda Windows, crear complemento, producto desde la aplicación, IAP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0492398872142aabd32d3a4d68d55b4e326f027e
ms.lasthandoff: 02/07/2017

---

# <a name="create-an-add-on-using-the-windows-store-submission-api"></a>Creación de un complemento mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para crear un complemento (también conocido como producto desde la aplicación o IAP) para una aplicación que esté registrada en tu cuenta del Centro de desarrollo de Windows.

>**Nota**&nbsp;&nbsp;Este método crea un complemento sin envíos. Para crear un envío de un complemento, consulta los métodos en [Administración de envíos de complementos](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del cuerpo del encabezado y la solicitud.

| Method | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Type   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obligatorio. Token de acceso de Azure AD con formato **Token del** &lt;*portador*&gt;. |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.
 
|  Parámetro  |  Type  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  applicationIds  |  matriz  |  Una matriz que contiene la Id. de la tienda de la aplicación asociada con este complemento. Solo se admite un elemento en esta matriz.   |  Sí  |
|  productId  |  string  |  La Id. de producto del complemento. Esto es un identificador que puede usarse en el código para hacer referencia al complemento. Para obtener más información, consulta [Establecimiento del tipo de producto y la Id. del producto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id).  |  Sí  |
|  productType  |  string  |  El tipo de producto del complemento. Se admiten los siguientes valores: **Duradero** y **Consumible**.  |  Sí  |

<span/>

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
| 409  | No se pudo crear el complemento debido a su estado actual o a que este complemento usa una función del panel del Centro de desarrollo que [actualmente no admite la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Administración de envíos de complementos](manage-add-on-submissions.md)
* [Obtención de todos los complementos](get-all-add-ons.md)
* [Obtención de un complemento](get-an-add-on.md)
* [Eliminación de un complemento](delete-an-add-on.md)

