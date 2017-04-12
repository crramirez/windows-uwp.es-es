---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "Usa estos métodos en la API de envío de la Tienda Windows para administrar complementos para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Administración de complementos"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store submission API, API de envío de la Tienda Windows, add-ons, complementos, in-app product, producto desde la aplicación, IAP, IAP"
ms.openlocfilehash: b442f48b7d03f0f972882ec240dbc1f37f018fd9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-add-ons"></a>Administración de complementos

Usa los métodos siguientes en la API de envío de la Tienda Windows para administrar complementos (también conocidos como productos desde la aplicación o IAP) para tus aplicaciones. Para obtener una introducción a la API de envío de la Tienda Windows, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota**&nbsp;&nbsp;Estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. Este permiso se habilita para cuentas de desarrollador en fases y no todas las cuentas tienen este permiso habilitado en este momento. Para solicitar acceso anterior, inicia sesión en el panel del Centro de desarrollo, haz clic en **Comentarios** en la parte inferior del panel, selecciona **API de envío** para el área de comentarios y envía la solicitud. Recibirás un correo electrónico cuando se habilite este permiso para tu cuenta.

Estos métodos solo pueden usarse para obtener, crear o eliminar complementos. Para crear envíos de complementos, consulta los métodos de [Administrar envíos de complemento](manage-add-on-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Método</th>
<th align="left">URI</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[Obtener todos los complementos de tus aplicaciones](get-all-add-ons.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[Obtener un complemento específico](get-an-add-on.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts```</td>
<td align="left">[Crear un complemento](create-an-add-on.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}```</td>
<td align="left">[Eliminar un complemento](delete-an-add-on.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Requisitos previos

Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows antes de intentar usar cualquiera de estos métodos.

## <a name="data-resources"></a>Recursos de datos

Los métodos de las API de envío de la Tienda Windows para administrar los complementos usan los siguientes recursos de datos JSON.

<span id="add-on-object" />
### <a name="add-on-resource"></a>Recurso de complemento

Este recurso describe un complemento.

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

| Valor      | Tipo   | Descripción        |
|------------|--------|--------------|
| applications      | array  | Matriz que contiene un [recurso de aplicación](#application-object) que representa la aplicación a la que está asociado este complemento. Solo se admite un elemento en esta matriz.  |
| id | cadena  | Id. de la Tienda del complemento. Este valor lo proporciona la Tienda. Un ejemplo de un Id. de la Tienda sería 9NBLGGH4TNMP.  |
| productId | string  | Id. del producto del complemento. Identificador que proporcionó el desarrollador cuando se creó el complemento. Para obtener más información, consulta [Establecer el tipo de producto y el id. del producto](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id). |
| productType | string  | El tipo de producto del complemento. Se admiten los siguientes valores: **Duradero** y **Consumible**.  |
| lastPublishedInAppProductSubmission       | object | Un [recurso de envío](#submission-object) que proporciona información sobre el último envío publicado para el complemento.         |
| pendingInAppProductSubmission        | object  |  Un [recurso de envío](#submission-object) que proporciona información sobre el envío pendiente actual para el complemento.  |   |

<span id="application-object" />
### <a name="application-resource"></a>Recurso de aplicación

Este recurso describe la aplicación a la que está asociado un complemento. En el siguiente ejemplo se muestra el formato de este recurso.

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

| Valor           | Tipo    | Descripción        |
|-----------------|---------|-----------|
| value            | object  |  Objeto que contiene los siguientes valores: <br/><br/> <ul><li>*id*. Id. de la Tienda de la aplicación. Para obtener más información sobre el Id. de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).</li><li>*resourceLocation*. Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para la aplicación.</li></ul>   |
| totalCount   | entero  | Número de objetos de la aplicación en la matriz *applications* del cuerpo de la respuesta.                                                                                                                                                 |

<span id="submission-object" />
### <a name="submission-resource"></a>Recurso de envío

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

| Valor           | Tipo    | Descripción     |
|-----------------|---------|------------------|
| id            | cadena  | Identificador del envío.    |
| resourceLocation   | cadena  | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para recuperar los datos completos para el envío.     |
 
<span/>
## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complemento con la API de envío de la Tienda Windows](manage-add-on-submissions.md)
* [Obtener todos los complementos](get-all-add-ons.md)
* [Obtención de un complemento](get-an-add-on.md)
* [Crear un complemento](create-an-add-on.md)
* [Eliminar un complemento](delete-an-add-on.md)
