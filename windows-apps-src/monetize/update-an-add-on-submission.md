---
ms.assetid: 8C63D33B-557D-436E-9DDA-11F7A5BFA2D7
description: Usa este método en la API de envío de Microsoft Store para actualizar un envío de complemento ya existente.
title: Actualización de un envío de complemento
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, add-on submission, envío de complemento, update, actualizar, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: fd0bb8df9b9fc36216da72e4ad01ebd2e650ad1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620620"
---
# <a name="update-an-add-on-submission"></a>Actualización de un envío de complemento


Usa este método en la API de envío de Microsoft Store para actualizar un envío de complemento (también conocido como producto desde la aplicación o IAP) ya existente. Después de actualizar correctamente un envío mediante este método, debes [confirmar el envío](commit-an-add-on-submission.md) para su ingesta y publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación del envío de un complemento mediante la API de envío de Microsoft Store, consulta [Administrar envíos de complementos)](manage-add-on-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Creación de un envío de complemento de uno de sus aplicaciones. Puede hacerlo en el centro de partners, o puede hacerlo mediante el uso de la [crear una presentación de complemento](create-an-add-on-submission.md) método.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obligatorio. Id. de la Tienda del complemento para el cual deseas actualizar un envío. El identificador de Store está disponible en el centro de partners, y se incluye en los datos de respuesta para las solicitudes a [crear un complemento](create-an-add-on.md) o [obtener los detalles del complemento](get-all-add-ons.md).  |
| submissionId | string | Obligatorio. Identificador del envío que se debe actualizar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de complemento](create-an-add-on-submission.md). Para un envío que se creó en el centro de partners, este identificador también está disponible en la dirección URL de la página de envío en el centro de partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| contentType           | string  |  [Tipo de contenido](../publish/enter-add-on-properties.md#content-type) que se proporciona en el complemento. Puede ser uno de los valores siguientes: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Matriz de cadenas que contienen hasta 10 [palabras clave](../publish/enter-add-on-properties.md#keywords) para el complemento. Gracias a estas palabras clave, la aplicación puede consultar complementos.   |
| lifetime           | string  |  Duración del complemento. Puede ser uno de los valores siguientes: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  | Objeto que contiene la información de la descripción del complemento. Para obtener más información, consulta [Listing resource (Recurso de descripción)](manage-add-on-submissions.md#listing-object).  |
| pricing           | object  | Objeto que contiene la información del precio del complemento. Para obtener más información, consulta [Pricing resource (Recurso de precio)](manage-add-on-submissions.md#pricing-object).  |
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |
| tag           | string  |  Los [datos del desarrollador personalizados](../publish/enter-add-on-properties.md#custom-developer-data) para el complemento (esta información se denominaba anteriormente *tag*).   |
| visibility  | string  |  Visibilidad del complemento. Puede ser uno de los valores siguientes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |


### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo actualizar un envío de complemento.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
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
    "sales": [],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
}
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el envío actualizado. Para obtener más información acerca de los valores en el cuerpo de la respuesta, consulta [Add-on submission resource (Recurso de envío de complemento)](manage-add-on-submissions.md#add-on-submission-object).

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
    "sales": [],
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

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | No se pudo actualizar el envío porque la solicitud no es válida. |
| 409  | No se pudo actualizar la presentación debido al estado actual del complemento o el complemento utiliza una característica de centro de partners que es [no compatible actualmente con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de complemento](manage-add-on-submissions.md)
* [Obtener la presentación de un complemento](get-an-add-on-submission.md)
* [Crear una presentación de complemento](create-an-add-on-submission.md)
* [Confirmar la presentación de un complemento](commit-an-add-on-submission.md)
* [Eliminar un envío de complemento](delete-an-add-on-submission.md)
* [Obtener el estado del envío de un complemento](get-status-for-an-add-on-submission.md)
