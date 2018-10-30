---
author: Xansky
ms.assetid: C09F4B7C-6324-4973-980A-A60035792EFC
description: Usa este método en la API de envío de Microsoft Store para crear un nuevo complemento para una aplicación que esté registrada en tu cuenta del Centro de desarrollo de Windows.
title: Crear un envío de complemento
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, create add-on submission, crear envío de complemento, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 32e5c803600d0b56e421ae87a5514f6c70cc8d11
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5757437"
---
# <a name="create-an-add-on-submission"></a>Crear un envío de complemento


Usa este método en la API de envío de Microsoft Store para crear un nuevo envío de complemento (también conocido como producto desde la aplicación o IAP) para una aplicación que esté registrada en tu cuenta del Centro de desarrollo de Windows. Después de crear correctamente un nuevo envío mediante este método, [actualiza el envío](update-an-add-on-submission.md) para realizar los cambios necesarios a los datos de envío y luego [confirma el envío](commit-an-add-on-submission.md) para la recopilación y la publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación del envío de un complemento mediante la API de envío de Microsoft Store, consulta [Administrar envíos de complementos)](manage-add-on-submissions.md).

> [!NOTE]
> Este método crea un envío para un complemento existente. Para crear un complemento, usa el método de [Creación de un complemento](create-an-add-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un complemento para una aplicación de tu cuenta del Centro de desarrollo. Puedes hacer esto en el panel del Centro de desarrollo o puedes hacerlo mediante el método de [Creación de un envío de complemento](create-an-add-on.md).

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | cadena | Obligatorio. Id. de la Store del complemento para el cual deseas crear un envío. El Id. de la Store está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta de las solicitudes de [Creación de un complemento](create-an-add-on.md) u [Obtención de los detalles de los complementos](get-all-add-ons.md).  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo envío para un complemento.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el nuevo envío. Para obtener más información acerca de los valores en el cuerpo de la respuesta, consulta [Recurso de envío de complemento](manage-add-on-submissions.md#add-on-submission-object).

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
| 409  | No se pudo crear el envío debido al estado actual de la aplicación o a que esta aplicación usa una función del panel del Centro de desarrollo que [actualmente no admite la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complemento](manage-add-on-submissions.md)
* [Obtener un envío de complemento](get-an-add-on-submission.md)
* [Confirmación de un envío de complemento](commit-an-add-on-submission.md)
* [Actualizar un envío de complemento](update-an-add-on-submission.md)
* [Eliminar un envío de complemento](delete-an-add-on-submission.md)
* [Get the status of an add-on submission (Obtener el estado de un envío de complemento)](get-status-for-an-add-on-submission.md)
