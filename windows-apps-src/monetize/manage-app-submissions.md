---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "Usa estos métodos en la API de envío de la Tienda Windows para administrar envíos para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Administrar envíos de aplicaciones con la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: f52059a37194b78db2f9bb29a5e8959b2df435b4
ms.openlocfilehash: 5c19a05f51a14d9df38e64aac3b741e916fc0524

---

# <a name="manage-app-submissions-using-the-windows-store-submission-api"></a>Administrar envíos de aplicaciones con la API de envío de la Tienda Windows


Usa los métodos siguientes en la API de envío de la Tienda Windows para administrar envíos para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows. Para obtener una introducción a la API de envío de la Tienda Windows, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota**&nbsp;&nbsp;Estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.


| Método        | URI    | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Obtiene los datos de un envío de aplicación existente. Para obtener más información, consulta [este artículo](get-an-app-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status``` | Obtiene el estado de un envío de aplicación existente. Para obtener más información, consulta [este artículo](get-status-for-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions``` | Crea un nuevo envío para una aplicación que esté registrada en tu cuenta del Centro de desarrollo de Windows. Para obtener más información, consulta [este artículo](create-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` | Confirma un envío de aplicación nuevo o actualizado en el Centro de desarrollo de Windows. Para obtener más información, consulta [este artículo](commit-an-app-submission.md). |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Actualiza un envío de aplicación existente. Para obtener más información, consulta [este artículo](update-an-app-submission.md). |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | Elimina un envío de aplicación. Para obtener más información, consulta [este artículo](delete-an-app-submission.md). |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout``` | Obtiene información de lanzamiento gradual para un envío de aplicación. Para obtener más información, consulta [este artículo](get-package-rollout-info-for-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage``` | Actualiza el porcentaje de lanzamiento gradual de un envío de aplicación. Para obtener más información, consulta [este artículo](update-the-package-rollout-percentage-for-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout``` | Detiene el lanzamiento gradual de un envío de aplicación. Para obtener más información, consulta [este artículo](halt-the-package-rollout-for-an-app-submission.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout``` | Finaliza el lanzamiento gradual de un envío de aplicación. Para obtener más información, consulta [este artículo](finalize-the-package-rollout-for-an-app-submission.md). |

<span id="create-an-app-submission">
## <a name="create-an-app-submission"></a>Crear un envío de aplicación

Para crear un envío de aplicación, sigue este proceso.

1. Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows.

  >**Nota**&nbsp;&nbsp;Asegúrate de que la aplicación ya tiene al menos un envío completado con la información de [clasificación por edades](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) rellenada.

2. [Obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Debes pasar este token de acceso a los métodos de la API de envío de la Tienda Windows. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

3. [Crea un envío de aplicación](create-an-app-submission.md) ejecutando el siguiente método en la API de envío de la Tienda Windows. Este método crea un nuevo envío en curso, que es una copia de tu último envío publicado.

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  El cuerpo de la respuesta contiene tres elementos: el identificador del nuevo envío, los datos del nuevo envío (incluidas todas las listas y la información sobre precios) y el URI de firma de acceso compartido (SAS) para cargar los paquetes de aplicación y enumerar las imágenes del envío. Para obtener más información sobre SAS, consulta [Firmas de acceso compartido, Parte 1: Descripción del modelo de firmas de acceso compartido](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

4. Si estás agregando nuevos paquetes o imágenes para el envío, [prepara los paquetes de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) y [las imágenes y capturas de pantalla de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Agrega todos estos archivos a un archivo ZIP.

5. Revisa los datos de envío con todos los cambios necesarios para el nuevo envío y ejecuta el siguiente método para [actualizar el envío de aplicación](update-an-app-submission.md).

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  >**Nota:**&nbsp;&nbsp;Si estás agregando nuevos paquetes o imágenes para el envío, asegúrate de actualizar los datos de envío para que hagan referencia al nombre y a la ruta de acceso relativa de estos archivos en el archivo ZIP.

4. Si estás agregando nuevos paquetes o imágenes para el envío, carga el archivo ZIP en el URI de SAS que se proporcionó en el cuerpo de la respuesta del método POST que llamaste en el paso 2. Para obtener más información, consulta [Firmas de acceso compartido, Parte 2: Creación y uso de una SAS con Almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

  El siguiente fragmento de código muestra cómo cargar el archivo usando la clase [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) en la biblioteca de cliente de Azure Storage para. NET.

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Ejecuta el siguiente método para [confirmar el envío de aplicación](commit-an-app-submission.md). Esta acción avisará al Centro de desarrollo que completaste el envío y que las actualizaciones deberían haberse aplicado a tu cuenta.

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. Comprueba el estado de confirmación ejecutando el siguiente método para [obtener el estado del envío de aplicación](get-status-for-an-app-submission.md).

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Para confirmar el estado del envío, revisa el valor de *status* en el cuerpo de la respuesta. Este valor debe cambiar de **CommitStarted** a **PreProcessing** si la solicitud se realiza correctamente o a **CommitFailed** si hay errores en la solicitud. Si hay errores, el campo *statusDetails* contendrá más detalles sobre el error.

7. Después de completarse correctamente la confirmación, el envío se remite a la Tienda para su ingesta. Para continuar con la supervisión del progreso del envío, puedes usar el método anterior o visitar el panel del Centro de desarrollo.

<span id="manage-gradual-package-rollout">
## <a name="manage-a-gradual-package-rollout-for-an-app-submission"></a>Administrar un lanzamiento de paquete gradual para un envío de aplicación

Puedes lanzar gradualmente los paquetes actualizados de un envío de aplicación para un porcentaje de los clientes de tu aplicación en Windows 10. Esto te permite supervisar los comentarios y los datos analíticos de los paquetes específicos para asegurarte de que estás seguro sobre la actualización antes de hacer un lanzamiento más amplio. Puedes cambiar el porcentaje de lanzamiento (o detener la actualización) para un envío publicado sin tener que crear un nuevo envío. Para obtener más detalles, incluidas instrucciones sobre cómo habilitar y administrar un lanzamiento de paquete gradual en el panel del Centro de desarrollo, consulta [este artículo](../publish/gradual-package-rollout.md).

También puedes habilitar y administrar un lanzamiento de paquete gradual mediante programación para un envío de aplicación mediante los siguientes métodos en la API de envío de la Tienda Windows.

* Para activar un lanzamiento de paquete gradual para un envío de aplicación:

  1. [Crea un envío de aplicación](create-an-app-submission.md) u [obtén un envío de aplicación](get-an-app-submission.md).
  2. En los datos de respuesta, busca el recurso [packageRollout](#package-rollout-object), establece el campo *isPackageRollout* en true y establece el campo *packageRolloutPercentage* en el porcentaje de los clientes de tu aplicación que podrán obtener acceso a los paquetes actualizados.
  3. Pasa los datos de envío de aplicación actualizados al método para [actualizar un envío de aplicación](update-an-app-submission.md).

<span/>

* Para [obtener la información de lanzamiento de paquete de un envío de aplicación](get-package-rollout-info-for-an-app-submission.md), ejecuta el siguiente método.

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout
  ```

<span/>

* Para [actualizar el porcentaje de lanzamiento de paquete de un envío de aplicación](update-the-package-rollout-percentage-for-an-app-submission.md), ejecuta el siguiente método.

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage  
  ```

<span/>

* Para [detener el lanzamiento de paquete de un envío de aplicación](halt-the-package-rollout-for-an-app-submission.md), ejecuta el siguiente método.

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout   
  ```  

<span/>

* Para [finalizar el lanzamiento de paquete de un envío de aplicación](finalize-the-package-rollout-for-an-app-submission.md), ejecuta el siguiente método.

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout
  ```

## <a name="resources"></a>Recursos

Estos métodos usan los siguientes recursos para dar formato a los datos.

<span id="app-submission-object" />
### <a name="app-submission"></a>Envío de aplicación

Este recurso representa un envío de una aplicación. En el siguiente ejemplo se muestra el formato de este recurso.

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
      "fileStatus": "Uploaded",
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
        "packageRolloutPercentage": 0,
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
  "friendlyName": "Submission 2"
}
```

Este recurso tiene los siguientes valores.

| Valor      | Tipo   | Descripción      |
|------------|--------|-------------------|
| id            | string  | Identificador del envío.  |
| applicationCategory           | string  |   Cadena que especifica la [categoría o subcategoría](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) de la aplicación. Las categorías y subcategorías se combinan en una cadena simple mediante el guion bajo "_" como, por ejemplo, **BooksAndReference_EReader**.      |  
| pricing           |  object  | Objeto que contiene la información del precio de la aplicación. Para obtener más información, consulta la sección [Recurso Precios](#pricing-object) a continuación.       |   
| visibility           |  string  |  Visibilidad de la app. Puede ser uno de los valores siguientes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |  
| listings           |   object  |  Diccionario de pares de claves y valores en el cual cada clave hace referencia al código de un país y cada valor hace referencia a un objeto de [recurso Descripción](#listing-object) que contiene la descripción de la aplicación.       |   
| hardwarePreferences           |  array  |   Matriz de cadenas que definen las [preferencias de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) de la aplicación. Puede ser uno de los valores siguientes: <ul><li>Función táctil</li><li>Teclado</li><li>Mouse</li><li>Cámara</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonía</li></ul>     |   
| automaticBackupEnabled           |  booleano  |   Indica si Windows puede incluir datos de la aplicación en copias de seguridad automáticas de OneDrive. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booleano  |   Indica si los clientes pueden instalar la aplicación en el almacenamiento extraíble. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booleano |   Indica si se habilita game DVR en la aplicación.    |   
| hasExternalInAppProducts           |     booleano          |   Indica si la aplicación permite a los usuarios realizar compras fuera del sistema de comercio de la Tienda Windows. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booleano           |  Indica si la aplicación se ha probado para garantizar que cumple las directrices de accesibilidad. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contiene [notas para la certificación](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) de la aplicación.    |    
| status           |   string  |  Estado del envío. Puede ser uno de los valores siguientes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   object  |  Contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores. Para obtener más información, consulta la sección [Detalles de estado](#status-details-object) a continuación.       |    
| fileUploadUrl           |   string  | URI de firma de acceso compartido (SAS) para cargar los paquetes para el envío. Si estás agregando nuevos paquetes o imágenes para el envío, carga el archivo ZIP que contiene los paquetes y las imágenes en este URI. Para obtener más información, consulta [Crear un envío de aplicación](#create-an-app-submission).       |    
| applicationPackages           |   array  | Contiene los objetos que proporcionan detalles acerca de cada paquete del envío. Para obtener más información, consulta la sección [Paquetes de la aplicación](#application-package-object) a continuación. |    
| packageDeliveryOptions    | objeto  | Contiene el lanzamiento de paquete gradual y la configuración de actualización obligatoria del envío. Para obtener más información, consulta la sección [Objeto de opciones de entrega de paquete](#package-delivery-options-object), a continuación.  |
| enterpriseLicensing           |  cadena  |  Uno de los [valores de licencia de empresa](#enterprise-licensing) indica el comportamiento de la licencia de empresa de la aplicación.  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  booleano   |  Indica si se permite que Microsoft [tenga la aplicación disponible para futuras familias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | object   |  Un diccionario de pares clave y valor, donde cada clave es una [familia de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) y cada valor es un booleano que indica si la aplicación puede seleccionar como destino la familia de dispositivos especificada.     |    
| friendlyName           |   string  |  Nombre descriptivo de la aplicación, que se usa con fines de visualización.       |  


<span id="listing-object" />
### <a name="listing"></a>Descripción

Este recurso contiene la información de descripción de una aplicación. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  baseListing               |   object      |  Información de [descripción de base](#base-listing-object) de la aplicación, que define la información de descripción predeterminada para todas las plataformas.   |     
|  platformOverrides               | object |   Un diccionario de pares de clave y valor, donde cada clave es una cadena que identifica una plataforma para la cual se va a reemplazar la información de descripción y cada valor es un objeto de [descripción de base](#base-listing-object) (que contiene solo los valores de la descripción del título) que especifica la información de descripción que se va reemplazar de la plataforma especificada. Las claves pueden tener los siguientes valores: <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### <a name="base-listing"></a>Descripción de base

Este recurso contiene la información de descripción de base de una aplicación. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  [Información de copyright o marcas comerciales](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info) opcional.  |
|  keywords                |  array       |  Matriz de [palabras clave](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords) para ayudar a tu aplicación a aparecer en los resultados de búsqueda.    |
|  licenseTerms                |    string     | [Términos de licencia](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms) opcionales para tu aplicación.     |
|  privacyPolicy                |   string      |   Dirección URL de la [directiva de privacidad](https://msdn.microsoft.com/windows/uwp/publish/privacy-policy) de tu aplicación.    |
|  supportContact                |   string      |  Dirección de correo electrónico o URL de [información de contacto de soporte técnico](https://msdn.microsoft.com/windows/uwp/publish/support-contact-info) de tu aplicación.     |
|  websiteUrl                |   string      |  Dirección URL de la [página web](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) de tu aplicación.    |    
|  description               |    string     |   [Descripción](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) de la aplicación.   |     
|  features               |    array     |  Matriz de hasta 20 cadenas que enumera las [características](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) de tu aplicación.     |
|  releaseNotes               |  string       |  [Notas de la versión](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) de tu aplicación.    |
|  images               |   array      |  Matriz de datos de [imagen e icono](#image-object) para la descripción de la aplicación.  |
|  recommendedHardware               |   array      |  Matriz de hasta 11 cadenas que enumera las [configuraciones de hardware recomendadas](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware) para tu aplicación.     |
|  title               |     string    |   Título de la descripción de la aplicación.   |  


<span id="image-object" />
### <a name="image"></a>Image

Este recurso contiene datos de imagen e icono para una descripción de la aplicación. Para obtener más información acerca de las imágenes y los iconos para la descripción, consulta [Imágenes y capturas de pantalla de aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción           |
|-----------------|---------|------|
|  fileName               |    string     |   Nombre del archivo de imagen en el archivo ZIP que cargaste para el envío.    |     
|  fileStatus               |   string      |  Estado del archivo de imagen. Puede ser uno de los valores siguientes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  string  | Identificador de la imagen, como se especifica en el Centro de desarrollo.  |
|  description  |  string  | Descripción de la imagen.  |
|  imageType  |  string  | Una de las siguientes cadenas que indica el tipo de la imagen: <ul><li>Unknown</li><li>Screenshot</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>Icon</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### <a name="pricing"></a>Precios

Este recurso contiene información sobre precios de la aplicación. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Cadena que especifica el período de prueba de la aplicación. Puede ser uno de los valores siguientes: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados de la aplicación en mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Los elementos de este diccionario reemplazan el precio base especificado por el valor de *priceId* para el mercado especificado.      |     
|  sales               |   matriz      |  **En desuso**. Una matriz de objetos que contienen información de ventas de la aplicación. Para obtener más información, consulta la sección [Venta](#sale-object) a continuación.    |     
|  priceId               |   string      |  [Franja de precios](#price-tiers) que especifica el [precio base](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) de la aplicación.   |


<span id="sale-object" />
### <a name="sale"></a>Sale

Este recurso contiene información de venta de una aplicación.

>**Importante:**&nbsp;&nbsp;El recurso **Sale** ya no se admite, y actualmente no se puede obtener ni modificar los datos de ventas del envío de una aplicación mediante la API de envío de la Tienda Windows:

   > * Después de llamar al [método GET para obtener un envío de aplicación](get-an-app-submission.md), el valor de *sales* estará vacío. Puedes seguir utilizando el panel del Centro de desarrollo para obtener los datos de ventas referentes al envío de la aplicación.
   > * Cuando se llama al [método PUT para actualizar un envío de aplicación](update-an-app-submission.md), la información del valor de *sales* se omite. Puedes seguir utilizando el panel del Centro de desarrollo para modificar los datos de ventas referentes al envío de la aplicación.

> En el futuro, actualizaremos la API de envío de la Tienda Windows para incorporar una nueva forma de acceder mediante programación a la información de ventas de los envíos de aplicaciones.

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    string     |   Nombre de la venta.    |     
|  basePriceId               |   string      |  [Franja de precios](#price-tiers) que se usará para el precio base de la venta.    |     
|  startDate               |   string      |   Fecha de inicio de la venta en formato ISO 8601.  |     
|  endDate               |   string      |  Fecha de finalización de la venta en formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados de la aplicación en mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Los elementos de este diccionario reemplazan el precio base especificado por el valor *basePriceId* para el mercado especificado.    |


<span id="status-details-object" />
### <a name="status-details"></a>Detalles de estado

Este recurso contiene detalles adicionales sobre el estado de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    object     |   Matriz de objetos que contienen detalles de error del envío. Para obtener más información, consulta la sección [Detalle de estado](#status-detail-object) a continuación.   |     
|  warnings               |   object      | Matriz de objetos que contienen detalles de advertencia del envío. Para obtener más información, consulta la sección [Detalle de estado](#status-detail-object) a continuación.     |
|  certificationReports               |     object    |   Matriz de objetos que proporcionan acceso a los datos del informe de certificación del envío. Si se produce un error en la certificación, puedes examinar estos informes para obtener más información. Para obtener más información, consulta la sección [Informe de certificación](#certification-report-object) a continuación.   |  


<span id="status-detail-object" />
### <a name="status-detail"></a>Detalle de estado

Este recurso contiene información adicional acerca de las advertencias o los errores relacionados de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    string     |   Cadena que describe el tipo de error o advertencia. Para obtener más información, consulta la sección [Código de estado del envío](#submission-status-code) a continuación.   |     
|  details               |     string    |  Mensaje con más detalles sobre el problema.     |


<span id="application-package-object" />
### <a name="application-package"></a>Paquetes de la aplicación

Este recurso contiene detalles sobre un paquete de aplicación para el envío. En el siguiente ejemplo se muestra el formato de este recurso.

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
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
}
```

Este recurso tiene los siguientes valores.  

>**Nota**&nbsp;&nbsp;Al llamar al método para [actualizar un envío de aplicación](update-an-app-submission.md), solo los valores *fileName*, *fileStatus*, *minimumDirectXVersion* y *minimumSystemRam* de este objeto son necesarios en el cuerpo de la solicitud. El Centro de desarrollo se encarga de rellenar el resto de valores.

| Valor           | Tipo    | Descripción                   |
|-----------------|---------|------|
| fileName   |   string      |  Nombre del paquete.    |  
| fileStatus    | string    |  Estado del paquete. Puede ser uno de los valores siguientes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Id. que identifica de manera exclusiva el paquete. Este valor lo usa el Centro de desarrollo.   |     
| version    |  string   |  Versión del paquete de aplicación. Para obtener más información, consulta [Numeración de la versión del paquete](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  Arquitectura del paquete (por ejemplo, ARM).   |     
| languages    | array    |  Matriz de códigos de idioma para los idiomas que admite la aplicación. Para obtener más información, consulta [Idiomas admitidos](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Matriz de funcionalidades necesarias para el paquete. Para obtener más información acerca de las funcionalidades, consulta [Declaraciones de funcionalidades de las aplicaciones](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  Versión mínima de DirectX que admite el paquete de aplicación. Solo se puede establecer para las aplicaciones diseñadas para Windows 8.x; se ignora para la aplicaciones diseñadas para otras versiones. Puede ser uno de los valores siguientes: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  RAM mínima que requiere el paquete de aplicación. Solo se puede establecer para las aplicaciones diseñadas para Windows 8.x; se ignora para la aplicaciones diseñadas para otras versiones. Puede ser uno de los valores siguientes: <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Matriz de cadenas que representan las familias de dispositivos a las que está destinado el paquete. Este valor se usa solo para los paquetes destinados a Windows 10. para los paquetes destinados a versiones anteriores, este valor es **None**. Las siguientes cadenas de familia de dispositivos se admiten actualmente para paquetes de Windows 10, donde *{0}* es una cadena de versión de Windows 10 como 10.0.10240.0, 10.0.10586.0 o 10.0.14393.0: <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />
### <a name="certification-report"></a>Informe de certificación

Este recurso proporciona acceso a los datos del informe de certificación de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     date            |    string     |  Fecha y hora en que se generó el informe, en formato ISO 8601.    |
|     reportUrl            |    cadena     |  Dirección URL en la que puedes obtener acceso al informe.    |


<span id="package-delivery-options-object" />
### <a name="package-delivery-options-object"></a>Objeto de opciones de entrega de paquete

Este recurso contiene ajustes de lanzamiento de paquete gradual y de actualización obligatoria para el envío. En el siguiente ejemplo se muestra el formato de este recurso.

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
| packageRollout   |   objeto      |  Contiene los ajustes de lanzamiento de paquete gradual para el envío. Para obtener más información, consulta la sección [Objeto de lanzamiento de paquete](#package-rollout-object) a continuación.    |  
| isMandatoryUpdate    | booleano    |  Indica si vas a tratar los paquetes de este envío como obligatorios para la instalación automática de actualizaciones de la aplicación. Para obtener más información sobre los paquetes obligatorios para la instalación automática de actualizaciones de la aplicación, consulta [Descargar e instalar actualizaciones de paquete para tu aplicación](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  fecha   |  La fecha y la hora en que los paquetes de este envío se convierten en obligatorios, en formato ISO 8601 y zona horaria UTC.   |        

<span id="package-rollout-object" />
### <a name="package-rollout-object"></a>Objeto de lanzamiento de paquete

Este recurso contiene la [configuración de lanzamiento de paquete](#manage-gradual-package-rollout) gradual para el envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
| isPackageRollout   |   booleano      |  Indica si el lanzamiento de paquete gradual está habilitado para el envío.    |  
| packageRolloutPercentage    | flotante    |  El porcentaje de usuarios que recibirá los paquetes en el lanzamiento gradual.    |  
| packageRolloutStatus    |  cadena   |  Una de las siguientes cadenas, que indica el estado del lanzamiento de paquete gradual: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  cadena   |  El identificador del envío que recibirán los clientes que no obtengan los paquetes de lanzamiento gradual.   |          

<span/>

## <a name="enums"></a>Enumeraciones

Estos métodos usan las enumeraciones siguientes.


<span id="price-tiers" />
### <a name="price-tiers"></a>Franjas de precios

Los valores siguientes representan las franjas de precios disponibles para un envío de aplicación.

| Valor           | Descripción                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   No se establece la franja de precios; usa el precio base para la aplicación.      |     
|  NotAvailable              |   La aplicación no está disponible en la región especificada.    |     
|  Free              |   La aplicación es gratuita.    |    
|  Tier2 a Tier194               |   Tier2 representa la franja de precios de 0,99 USD. Cada franja adicional representa incrementos adicionales (1,29 USD, 1,49 USD, 1,99 USD, etc.).    |


<span id="enterprise-licensing" />
### <a name="enterprise-licensing-values"></a>Valores de licencia de empresa

Los valores siguientes representan el comportamiento de las licencias de empresa para la aplicación. Para obtener más información acerca de estas opciones, consulta [Opciones de licencia organizativas](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

| Valor           |  Descripción      |
|-----------------|---------------|
| None            |     Tu aplicación no se pone a disposición de las empresas con licencias por volumen administradas por la Tienda (en línea).         |     
| Online        |     Tu aplicación se pone a disposición de las empresas con licencias por volumen administradas por la Tienda (en línea).  |
| OnlineAndOffline | Tu aplicación se pone a disposición de las empresas con licencias por volumen administradas por la Tienda (en línea) y también a través de licencias desconectadas (sin conexión). |


<span id="submission-status-code" />
### <a name="submission-status-code"></a>Código de estado del envío

Los valores siguientes representan el código de estado de un envío.

| Valor           |  Descripción      |
|-----------------|---------------|
| None            |     No se especificó ningún código.         |     
| InvalidArchive        |     El archivo ZIP que contiene el paquete no es válido o tiene un formato de archivo no reconocido.  |
| MissingFiles | El archivo ZIP no tiene todos los archivos que se enumeran en los datos del envío o estos se encuentran en una ubicación incorrecta en el archivo. |
| PackageValidationFailed | Uno o varios paquetes de tu envío no se pudieron validar. |
| InvalidParameterValue | Uno de los parámetros del cuerpo de la solicitud no es válido. |
| InvalidOperation | La operación que intentaste realizar no es válida. |
| InvalidState | La operación que intentaste realizar no es válida para el estado actual del paquete piloto. |
| ResourceNotFound | No se pudo encontrar el paquete piloto especificado. |
| ServiceError | Un error de servicio interno impidió que la solicitud se realizase correctamente. Vuelve a intentar realizar la solicitud. |
| ListingOptOutWarning | El desarrollador quitó una descripción de un envío anterior o no incluyó la información de descripción que admitía el paquete. |
| ListingOptInWarning  | El desarrollador agregó una lista. |
| UpdateOnlyWarning | El desarrollador está intentando insertar algo que solo presenta soporte técnico de actualizaciones. |
| Other  | El envío presenta un estado no reconocido o sin clasificar. |
| PackageValidationWarning | El proceso de validación del paquete genera una advertencia. |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener datos de aplicación mediante la API de envío de la Tienda Windows](get-app-data.md)
* [Envíos de aplicaciones en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)



<!--HONumber=Dec16_HO1-->


