---
author: mcleanbyron
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: "Usa este método en la API de envío de la Tienda Windows para actualizar un envío de aplicación ya existente."
title: "Actualizar un envío de aplicaciones mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 178b70db1583790c174d65e060c8bce6e4f69243
ms.openlocfilehash: ad1c565f1ec84127b2ac689cc7cb23d2b39764ef

---

# Actualizar un envío de aplicaciones mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para actualizar un envío de aplicación ya existente. Después de actualizar correctamente un envío mediante este método, debes [confirmar el envío](commit-an-app-submission.md) para su ingesta y publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación de un envío de aplicación mediante la API de envío de la Tienda Windows, consulta [Manage app submissions (Administrar envíos de aplicación)](manage-app-submissions.md).

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un envío para una aplicación de tu cuenta del Centro de desarrollo. Puedes hacer esto en el panel del Centro de desarrollo o puedes hacerlo mediante el método [Create an app submission (Crear un envío de aplicación)](create-an-app-submission.md).

>**Nota**&nbsp;&nbsp;Este método solo puede usarse en cuentas del Centro de desarrollo de Windows que estén autorizadas para usar la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. Id. de la Tienda de la aplicación para la cual deseas actualizar un envío. Para obtener más información sobre la Id. de la Tienda, consulta [View app identity details (Ver los detalles de identidad de la aplicación)](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Obligatorio. Identificador del envío que se debe actualizar. Esta identificación está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta de las solicitudes de [Creación de un envío de aplicaciones](create-an-app-submission.md).  |

<span/>

### Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | string  |   Cadena que especifica la [categoría o subcategoría](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) de la aplicación. Las categorías y subcategorías se combinan en una cadena simple mediante el guion bajo "_" como, por ejemplo, **BooksAndReference_EReader**.      |  
| pricing           |  object  | Objeto que contiene la información del precio de la aplicación. Para obtener más información, consulta la sección [Pricing resource (Recurso de precio)](manage-app-submissions.md#pricing-object).       |   
| visibility           |  string  |  Visibilidad de la app. Esta puede ser uno de los valores siguientes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | Modo de publicación del envío. Este puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |  
| listings           |   object  |  Diccionario de pares de claves y valores en el cual cada clave hace referencia al código de un país y cada valor hace referencia a un objeto de [recurso de descripción](manage-app-submissions.md#listing-object) que contiene la descripción de la aplicación.       |   
| hardwarePreferences           |  array  |   Matriz de cadenas que definen la [las preferencias de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) de la aplicación. Estas preferencias pueden ser uno de los valores siguientes: <ul><li>Función táctil</li><li>Teclado</li><li>Mouse</li><li>Cámara</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonía</li></ul>     |   
| automaticBackupEnabled           |  booleano  |   Indica si Windows puede incluir datos de la aplicación en copias de seguridad automáticas de OneDrive. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booleano  |   Indica si los clientes pueden instalar la aplicación en el almacenamiento extraíble. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booleano |   Indica si se habilita game DVR en la aplicación.    |   
| hasExternalInAppProducts           |     booleano          |   Indica si la aplicación permite a los usuarios realizar compras fuera del sistema de comercio de la Tienda Windows. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booleano           |  Indica si la aplicación se ha probado para garantizar que cumple las directrices de accesibilidad. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contiene [notas para la certificación](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) de la aplicación.    |    
| applicationPackages           |   array  | Contiene los objetos que proporcionan detalles acerca de cada paquete del envío. Para obtener más información, consulta la sección [Application package (Paquete de aplicación)](manage-app-submissions.md#application-package-object). Al llamar a este método para actualizar un envío de aplicación, solo los valores *fileName*, *fileStatus*, *minimumDirectXVersion* y *minimumSystemRam* de esos objetos son necesarios en el cuerpo de la solicitud. El Centro de desarrollo se encarga de rellenar el resto de valores.   |    
| enterpriseLicensing           |  string  |  Uno de los [valores de licencia de empresa](#enterprise-licensing) indica el comportamiento de la licencia de empresa de la aplicación  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  booleano   |  Indica si se permite que Microsoft [tenga la aplicación disponible para futuras familias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | booleano   |  Indica si se permite que la aplicación [se enfoque a futuras familias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).     |    

<span/>

### Ejemplo de solicitud

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
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
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
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  }
}
```

## Respuesta

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
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
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
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2"
}
```

## Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | No se pudo actualizar el envío porque la solicitud no es válida. |
| 409  | No se pudo actualizar el envío debido al estado actual de la aplicación o a que esta aplicación usa una función del panel del Centro de desarrollo que [actualmente no admite la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Temas relacionados

* [Create and manage submissions using Windows Store services (Crear y administrar envíos mediante el uso de servicios de la Tienda Windows)](create-and-manage-submissions-using-windows-store-services.md)
* [Get an app submission (Obtener un envío de aplicación)](get-an-app-submission.md)
* [Create an app submission (Crear un envío de aplicación)](create-an-app-submission.md)
* [Commit an app submission (Confirmar el envío de aplicación)](commit-an-app-submission.md)
* [Delete an app submission (Eliminar un envío de aplicación)](delete-an-app-submission.md)
* [Get the status of an app submission (Obtener el estado de un envío de aplicación)](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


