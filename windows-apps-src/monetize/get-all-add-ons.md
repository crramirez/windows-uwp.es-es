---
author: Xansky
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: Usa este método en la API de envío de Microsoft Store para recuperar los datos de todos los complementos de todas las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows.
title: Obtener todos los complementos
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, add-ons, complementos, in-app products, productos desde la aplicación, IAPs, IAP
ms.localizationpriority: medium
ms.openlocfilehash: c711e2443de4607d2266dcddf513a48ff11522a7
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2018
ms.locfileid: "5168284"
---
# <a name="get-all-add-ons"></a>Obtener todos los complementos

Usa este método en la API de envío de Microsoft Store para recuperar datos para todos los complementos de todas las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos.

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

Todos los parámetros de solicitud son opcionales para este método. Si llamas a este método sin parámetros, la respuesta contiene datos de todos los complementos de todas las aplicaciones registradas en tu cuenta.

|  Parámetro  |  Tipo  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  top  |  entero  |  El número de elementos que se devolverán en la solicitud (es decir, el número de complementos que se devolverán). Si los complementos de la cuenta superan el valor especificado en la consulta, el cuerpo de la respuesta incluye una ruta de acceso al URI relativa que puedes anexar al URI del método para solicitar la siguiente página de datos.  |  No  |
|  skip  |  entero  |  El número de elementos que se omitirán en la consulta antes de devolver los elementos restantes. Usa este parámetro para consultar conjuntos de datos. Por ejemplo, top = 10 y skip = 0 recuperan los elementos del 1 al 10, mientras que top = 10 y skip = 10 recuperan los elementos del 11 al 20, y así sucesivamente.  |  No  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo recuperar los datos de todos los complementos de todas las aplicaciones registradas en tu cuenta.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra cómo recuperar solo los 10 primeros complementos.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de respuesta JSON que devuelve una solicitud correcta para los cinco primeros complementos registrados en una cuenta de desarrollador con un total de 1072 complementos. Para mayor brevedad, en este ejemplo se muestran solo los datos de los dos primeros complementos que devuelve la solicitud. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta la siguiente sección.

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
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
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
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
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene una ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para solicitar la siguiente página de datos. Por ejemplo, si el parámetro *top* del cuerpo de la solicitud inicial se establece en 10, pero hay 100 complementos registrados en la cuenta, el cuerpo de la respuesta incluirá un valor @nextLink de ```inappproducts?skip=10&top=10```, lo que indica que puedes llamar a ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10``` para solicitar los 10 complementos siguientes. |
| value            | matriz  |  Una matriz que contiene objetos que proporcionan información sobre cada complemento. Para obtener más información, consulta [Recurso de complemento](manage-add-ons.md#add-on-object).   |
| totalCount   | entero  | Número de objetos de la aplicación en la matriz *value* del cuerpo de la respuesta.     |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se encontraron complementos. |
| 409  | Las aplicaciones o complementos usan características de panel del Centro de desarrollo que [la API de envío de Microsoft Store no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complemento](manage-add-on-submissions.md)
* [Obtener un complemento](get-an-add-on.md)
* [Crear un complemento](create-an-add-on.md)
* [Eliminar un complemento](delete-an-add-on.md)
