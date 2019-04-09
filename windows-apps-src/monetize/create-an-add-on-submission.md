---
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: Use este método en la API de envío de Microsoft Store para crear un nuevo envío de complemento para una aplicación que está registrado en el centro de partners.
title: Crear un envío de complemento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, create add-on submission, crear envío de complemento, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: cbb093576badf5cd84b132cfb139db9da7d31991
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334933"
---
# <a name="create-an-add-on-submission"></a>Crear un envío de complemento

Use este método en la API de envío de Microsoft Store para crear un nuevo envío de complementos (también conocido como aplicación producto o IAP) para una aplicación que está registrado en la cuenta del centro de partners. Después de crear correctamente un nuevo envío mediante este método, [actualiza el envío](update-an-add-on-submission.md) para realizar los cambios necesarios a los datos de envío y luego [confirma el envío](commit-an-add-on-submission.md) para la recopilación y la publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación del envío de un complemento mediante la API de envío de Microsoft Store, consulta [Administrar envíos de complementos)](manage-add-on-submissions.md).

> [!NOTE]
> Este método crea un envío para un complemento existente. Para crear un complemento, usa el método de [Creación de un complemento](create-an-add-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Crear un complemento para una de las aplicaciones. Puede hacerlo en el centro de partners, o puede hacerlo mediante el uso de la [crear un complemento](create-an-add-on.md) método.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| EXPONER    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions` |

### <a name="request-header"></a>Encabezado de la solicitud

| Header        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |

### <a name="request-parameters"></a>Parámetros de solicitud

| Name        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obligatorio. Id. de la Tienda del complemento para el cual deseas crear un envío. El identificador de Store está disponible en el centro de partners, y se incluye en los datos de respuesta para las solicitudes a [crear un complemento](create-an-add-on.md) o [obtener los detalles del complemento](get-all-add-ons.md).  |

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo envío para un complemento.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el nuevo envío. Para obtener más información acerca de los valores del cuerpo de la respuesta, consulta [Recurso de envío de complemento](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free",
    "isAdvancedPricingModel": true
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | No se pudo crear el envío porque la solicitud no es válida. |
| 409  | El envío no se pudo crear debido al estado actual de la aplicación o la aplicación usa una característica del centro de partners que está [no compatible actualmente con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complemento](manage-add-on-submissions.md)
* [Obtener la presentación de un complemento](get-an-add-on-submission.md)
* [Confirmar la presentación de un complemento](commit-an-add-on-submission.md)
* [Actualizar un complemento de envío](update-an-add-on-submission.md)
* [Eliminar un envío de complemento](delete-an-add-on-submission.md)
* [Obtener el estado del envío de un complemento](get-status-for-an-add-on-submission.md)
