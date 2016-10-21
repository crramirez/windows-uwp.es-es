---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "Usa estos métodos en la API de envío de la Tienda Windows para administrar complementos para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Administrar complementos mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 75548ee4689fd31d734c570f8e3eca5d33a6181f

---

# Administrar complementos mediante la API de envío de la Tienda Windows




Usa los métodos siguientes en la API de envío de la Tienda Windows para administrar complementos (también conocidos como productos desde la aplicación o IAP) para aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows. Para obtener una introducción a la API de envío de la Tienda Windows, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota**&nbsp;&nbsp;Estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado. Estos métodos solo pueden usarse para obtener, crear o eliminar complementos. Para crear envíos de complementos, consulta los métodos de [Administrar envíos de complemento](manage-add-on-submissions.md).

| Método        | URI    | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Obtiene los datos de todos los complementos de todas las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows. Para obtener más información, consulta [Obtener todos los complementos](get-all-add-ons.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId} ``` | Obtiene los datos de un complemento específico. Para obtener más información, consulta [Obtener un complemento](get-an-add-on.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | Crea un complemento nuevo. Para obtener más información, consulta [Crear un complemento](create-an-add-on.md).  |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` | Elimina un complemento. Para obtener más información, consulta [Eliminar un complemento](delete-an-add-on.md). |

## Requisitos previos

Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows antes de intentar usar cualquiera de estos métodos.

## Recursos

Estos métodos usan los siguientes recursos para dar formato a los datos.

<span id="add-on-object" />
### Complemento

Este recurso representa un complemento. En el siguiente ejemplo se muestra el formato de este recurso.

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
  "productId": "TestAddOn",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

Este recurso tiene los siguientes valores.

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applications      | array  | Matriz que contiene un objeto que representa la aplicación a la que está asociado este complemento. Para obtener más información sobre los datos de este objeto, consulta la sección del [objeto Aplicación](#application-object) a continuación. Solo se admite un elemento en esta matriz.  |
| id | string  | Id. de la Tienda del complemento. Este valor lo proporciona la Tienda. Un ejemplo de un Id. de la Tienda sería 9NBLGGH4TNMP.  |
| productId | string  | Id. del producto del complemento. Identificador que proporcionó el desarrollador cuando se creó el complemento. Para obtener más información, consulta [Establecer el tipo de producto y el id. del producto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | string  | Tipo de producto del complemento. Se admiten los siguientes valores: **Duradero** y **Consumible**.  |
| lastPublishedInAppProductSubmission       | object | Objeto que proporciona información sobre el último envío publicado para el complemento. Si quieres obtener más información, consulta la sección [Envío](#submission-object) a continuación.                                                                                                                                                          |
| pendingInAppProductSubmission        | object  |  Objeto que proporciona información sobre el envío pendiente actualmente para el complemento. Si quieres obtener más información, consulta la sección [Envío](#submission-object) a continuación.  |   |

<span id="application-object" />
### Aplicación

Este recurso representa una aplicación a la que está asociado un complemento. En el siguiente ejemplo se muestra el formato de este recurso.

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
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|-----------|
| value            | object  |  Objeto que contiene los siguientes valores: <br/><br/> <ul><li>*id*. Id. de la Tienda de la aplicación. Para obtener más información sobre el Id. de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para la aplicación.</li></ul>   |
| totalCount   | entero  | Número de objetos de la aplicación en la matriz *applications* del cuerpo de la respuesta.                                                                                                                                                 |

<span id="submission-object" />
### Envío

Este recurso proporciona información sobre un envío de un complemento. En el siguiente ejemplo se muestra el formato de este recurso.

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Identificador del envío.    |
| resourceLocation   | string  | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para el envío.                                                                                                                                               |
 
<span/>

## Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complemento con la API de envío de la Tienda Windows](manage-add-on-submissions.md)
* [Obtener todos los complementos](get-all-add-ons.md)
* [Obtener un complemento](get-an-add-on.md)
* [Crear un complemento](create-an-add-on.md)
* [Eliminar un complemento](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


