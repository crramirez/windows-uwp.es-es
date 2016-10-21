---
author: mcleanbyron
ms.assetid: E3DF5D11-8791-4CFC-8131-4F59B928A228
description: "Usa este método en la API de envío de la Tienda Windows para obtener datos para un envío de complemento existente."
title: "Obtener un envío de complemento con el uso de la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 699f26e8a73e1777f5966faf346945807d460315

---

# Obtener un envío de complemento con el uso de la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para obtener datos de un envío de complemento (también conocido como producto desde la aplicación o IAP) existente. Para obtener más información sobre el proceso de creación del envío de un complemento mediante la API de envío de la Tienda Windows, consulta [Administrar envíos de complemento](manage-add-on-submissions.md).

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Crea un envío de complemento para una aplicación de tu cuenta del Centro de desarrollo. Puedes hacerlo desde el panel del Centro de desarrollo o mediante el método para [Crear un envío de complemento](create-an-add-on-submission.md).

>**Nota**&nbsp;&nbsp;Este método solo puede usarse en cuentas del Centro de desarrollo de Windows que estén autorizadas para usar la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con el formato **Bearer** &lt;*token*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | cadena | Obligatorio. El Id. de la Tienda del complemento que contiene el envío que quieres obtener. El Id. de la Tienda está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta de las solicitudes para [crear complementos](create-an-add-on.md) u [obtener detalles de complementos](get-all-add-ons.md).  |
| submissionId | cadena | Obligatorio. El identificador del envío que se va a obtener. Esta identificación está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta de las solicitudes de [Creación de un envío de complementos](create-an-add-on-submission.md).  |

<span/>

### Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo obtener un envío de complemento.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680 HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta

En el siguiente ejemplo se muestra el cuerpo de respuesta JSON de una llamada correcta a este método. El cuerpo de la respuesta contiene información sobre el envío especificado. Para obtener más información acerca de los valores del cuerpo de la respuesta, consulta [Recurso de envío de complemento](manage-add-on-submissions.md#add-on-submission-object).

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
    "priceId": "Free"
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

## Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío. |
| 409  | El envío no pertenece al complemento especificado o el complemento usa una característica de panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Crear un envío de complemento](create-an-add-on-submission.md)
* [Confirmar un envío de complemento](commit-an-add-on-submission.md)
* [Actualizar un envío de complemento](update-an-add-on-submission.md)
* [Eliminar un envío de complemento](delete-an-add-on-submission.md)
* [Obtener el estado de un envío de complemento](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


