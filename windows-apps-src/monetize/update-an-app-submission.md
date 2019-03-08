---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: Usa este método en la API de envío de Microsoft Store para actualizar un envío de aplicación ya existente.
title: Actualización de un envío de aplicación
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, app submission, envío de aplicación, update, actualizar
ms.localizationpriority: medium
ms.openlocfilehash: b61508edf2ebc2ab155110189fe67df63e2bab30
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634620"
---
# <a name="update-an-app-submission"></a>Actualización de un envío de aplicación

Usa este método en la API de envío de Microsoft Store para actualizar un envío de aplicación ya existente. Después de actualizar correctamente un envío mediante este método, debes [confirmar el envío](commit-an-app-submission.md) para su ingesta y publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación de un envío de aplicación mediante la API de envío de Microsoft Store, consulta [Administración de envíos de aplicación](manage-app-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Crear una presentación para una de las aplicaciones. Puede hacerlo en el centro de partners, o puede hacerlo mediante el uso de la [crear un envío de la aplicación](create-an-app-submission.md) método.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. Id. de la Tienda de la aplicación para la cual deseas actualizar un envío. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Obligatorio. Identificador del envío que se debe actualizar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de aplicación](create-an-app-submission.md). Para un envío que se creó en el centro de partners, este identificador también está disponible en la dirección URL de la página de envío en el centro de partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | string  |   Cadena que especifica la [categoría o subcategoría](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) de la aplicación. Las categorías y subcategorías se combinan en una cadena simple mediante el guion bajo "_" como, por ejemplo, **BooksAndReference_EReader**.      |  
| pricing           |  object  | Objeto que contiene la información del precio de la aplicación. Para obtener más información, consulta la sección [Recurso de precios](manage-app-submissions.md#pricing-object).       |   
| visibility           |  string  |  Visibilidad de la aplicación. Puede ser uno de los valores siguientes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |  
| listings           |   object  |  Diccionario de pares de claves y valores en el cual cada clave hace referencia al código de un país y cada valor hace referencia a un objeto de [recurso de descripción](manage-app-submissions.md#listing-object) que contiene la descripción de la aplicación.       |   
| hardwarePreferences           |  array  |   Matriz de cadenas que definen las [preferencias de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) de la aplicación. Puede ser uno de los valores siguientes: <ul><li>Función táctil</li><li>Teclado</li><li>Mouse</li><li>Cámara</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonía</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Indica si Windows puede incluir datos de la aplicación en copias de seguridad automáticas de OneDrive. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  boolean  |   Indica si los clientes pueden instalar la aplicación en el almacenamiento extraíble. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  boolean |   Indica si se habilita game DVR en la aplicación.    |   
| gamingOptions           |  object |   Matriz que contiene un [recurso de opciones de juegos](manage-app-submissions.md#gaming-options-object) que definir la configuración relacionada con juegos para la aplicación.     |   
| hasExternalInAppProducts           |     boolean          |   Indica si la aplicación permite a los usuarios realizar compras fuera del sistema de comercio de Microsoft Store. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    boolean           |  Indica si la aplicación se ha probado para garantizar que cumple las directrices de accesibilidad. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contiene [notas para la certificación](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) de la aplicación.    |    
| applicationPackages           |   array  | Contiene los objetos que proporcionan detalles acerca de cada paquete del envío. Para obtener más información, consulta la sección [Application package (Paquete de aplicación)](manage-app-submissions.md#application-package-object). Al llamar a este método para actualizar un envío de aplicación, solo los valores *fileName*, *fileStatus*, *minimumDirectXVersion* y *minimumSystemRam* de esos objetos son necesarios en el cuerpo de la solicitud. Los demás valores se rellenan con el centro de partners.   |    
| packageDeliveryOptions    | object  | Contiene el lanzamiento de paquete gradual y la configuración de actualización obligatoria del envío. Para obtener más información, consulta [Package delivery options object](manage-app-submissions.md#package-delivery-options-object) (Objeto de opciones de entrega de paquete).  |
| enterpriseLicensing           |  string  |  Uno de los [valores de licencia de empresa](manage-app-submissions.md#enterprise-licensing) indica el comportamiento de la licencia de empresa de la aplicación.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Indica si se permite que Microsoft [tenga la aplicación disponible para futuras familias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | boolean   |  Indica si se permite que la aplicación [se enfoque a futuras familias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).     |   
| tráileres           |  array |   Matriz que contiene hasta [recursos de tráileres](manage-app-submissions.md#trailer-object) que representan tráileres de vídeo para la descripción de la aplicación.   |   


### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo actualizar un envío de aplicación.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
              "epub"
            ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
              "Free ebook reader"
            ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el envío actualizado. Para obtener más información acerca de los valores del cuerpo de la respuesta, consulta [App submission resource (Recurso de envío de aplicación)](manage-app-submissions.md#app-submission-object).

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
           "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | No se pudo actualizar el envío porque la solicitud no es válida. |
| 409  | No se pudo actualizar la presentación debido al estado actual de la aplicación o la aplicación usa una característica del centro de partners que está [no compatible actualmente con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener un envío de la aplicación](get-an-app-submission.md)
* [Creación de un envío de la aplicación](create-an-app-submission.md)
* [Confirmación de envío de la aplicación](commit-an-app-submission.md)
* [Eliminar un envío de la aplicación](delete-an-app-submission.md)
* [Obtener el estado de un envío de la aplicación](get-status-for-an-app-submission.md)
