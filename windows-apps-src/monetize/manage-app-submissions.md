---
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Use estos métodos en la API de envío de Microsoft Store para administrar los envíos de aplicaciones que están registradas en su cuenta del centro de Partners.
title: Administración de envíos de aplicaciones
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, envíos de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: de612607da2192af3358c94874e0896557ca6d08
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171369"
---
# <a name="manage-app-submissions"></a>Administración de envíos de aplicaciones

La API de envío de Microsoft Store proporciona métodos que puede usar para administrar los envíos de sus aplicaciones, incluidas las implementaciones de paquetes graduales. Para ver una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para el uso de la API, consulte [crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si usa la API de envío de Microsoft Store para crear un envío para una aplicación, asegúrese de realizar más cambios en el envío solo mediante la API, en lugar del centro de Partners. Si usa el centro de partners para cambiar un envío que creó originalmente con la API, ya no podrá cambiar o confirmar el envío mediante la API. En algunos casos, el envío podría quedar en un estado de error en el que no se puede continuar en el proceso de envío. Si esto ocurre, debe eliminar el envío y crear un nuevo envío.

> [!IMPORTANT]
> No puede usar esta API para publicar envíos de [compras por volumen a través de la Microsoft Store para empresas y Microsoft Store para educación](../publish/organizational-licensing.md) o para publicar envíos de [aplicaciones de LOB](../publish/distribute-lob-apps-to-enterprises.md) directamente en las empresas. En ambos casos, debe usar el centro de partners para publicar el envío.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>Métodos para administrar envíos de aplicaciones

Usa los siguientes métodos para obtener, crear, actualizar, confirmar o eliminar un envío de aplicación. Antes de poder usar estos métodos, la aplicación ya debe existir en la cuenta del centro de Partners y primero debe crear un envío para la aplicación en el centro de Partners. Para más información, consulte la sección los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">Obtener un envío de aplicación existente</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">Obtener el estado de un envío de aplicación existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">Crear un nuevo envío de aplicación</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">Actualizar un envío de aplicación existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">Confirmar un envío de aplicación nuevo o actualizado</a></td>
</tr>
<tr>
<td align="left">Delete</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">Eliminar un envío de aplicación</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>Crear un envío de aplicación

Para crear un envío de aplicación, sigue este proceso.

1. Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
    > [!NOTE]
    > Asegúrese de que la aplicación ya tiene al menos un envío completado con la información de [clasificación de edad](../publish/age-ratings.md) completada.

2. [Obtener un token de acceso Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Debe pasar este token de acceso a los métodos de la API de envío de Microsoft Store. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

3. [Cree un envío de aplicación](create-an-app-submission.md) ejecutando el método siguiente en la API de envío de Microsoft Store. Este método crea un nuevo envío en curso, que es una copia de tu último envío publicado.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    El cuerpo de la respuesta contiene un recurso de envío de la [aplicación](#app-submission-object) que incluye el identificador del nuevo envío, el URI de la firma de acceso compartido (SAS) para cargar los archivos relacionados para el envío al almacenamiento de blobs de Azure (por ejemplo, paquetes de aplicaciones, imágenes de lista y archivos de finalizador) y todos los datos para el nuevo envío (como la información de precios

    > [!NOTE]
    > Un URI de SAS proporciona acceso a un recurso seguro en Azure Storage sin necesidad de claves de cuenta. Para obtener información general sobre los URI de SAS y su uso con Azure Blob Storage, consulta [Shared Access Signatures, Part 1: Understanding the SAS model (Firmas de acceso compartido, parte 1: información sobre el modelo SAS)](/azure/storage/common/storage-sas-overview) y [Firmas de acceso compartido, Parte 2: Creación y uso de una SAS con Almacenamiento de blobs](/azure/storage/common/storage-sas-overview).

4. Si va a agregar nuevos paquetes, mostrar imágenes o archivos de finalizador para el envío, [Prepare los paquetes de la aplicación](../publish/app-package-requirements.md) y [Prepare las capturas de pantallas, las imágenes y los finalizadores](../publish/app-screenshots-and-images.md)de la aplicación. Agrega todos estos archivos a un archivo ZIP.

5. Revise los datos de envío de la [aplicación](#app-submission-object) con los cambios necesarios para el nuevo envío y ejecute el método siguiente para [actualizar el envío de la aplicación](update-an-app-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si va a agregar nuevos archivos para el envío, asegúrese de actualizar los datos de envío para que hagan referencia al nombre y la ruta de acceso relativa de estos archivos en el archivo ZIP.

4. Si va a agregar nuevos paquetes, mostrar imágenes o archivos de finalizador para el envío, cargue el archivo ZIP en el [almacenamiento de blobs de Azure](/azure/storage/storage-introduction#blob-storage) con el URI de SAS que se proporcionó en el cuerpo de respuesta del método post que llamó anteriormente. Hay varias bibliotecas de Azure, que puedes usar para hacer esto en una variedad de plataformas, como:

    * [Biblioteca de cliente de Azure Storage para .NET](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [SDK de Azure Storage para Java](/azure/storage/storage-java-how-to-use-blob-storage)
    * [SDK de Azure Storage para Python](/azure/storage/storage-python-how-to-use-blob-storage)

    En el siguiente ejemplo de código C# se muestra cómo cargar el archivo ZIP en Azure Blob Storage usando la clase [CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) en la biblioteca de cliente de Azure Storage para. NET. En este ejemplo se supone que el archivo ZIP ya se ha escrito en un objeto de secuencia.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Ejecuta el siguiente método para [confirmar el envío de aplicación](commit-an-app-submission.md). Esto le avisará al centro de Partners de que ha terminado con el envío y que las actualizaciones deben aplicarse ahora a su cuenta.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. Comprueba el estado de confirmación ejecutando el siguiente método para [obtener el estado del envío de aplicación](get-status-for-an-app-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    Para confirmar el estado del envío, revisa el valor de *status* en el cuerpo de la respuesta. Este valor debe cambiar de **CommitStarted** a **PreProcessing** si la solicitud se realiza correctamente o a **CommitFailed** si hay errores en la solicitud. Si hay errores, el campo *statusDetails* contendrá más detalles sobre el error.

7. Después de completarse correctamente la confirmación, el envío se remite a la Tienda para su ingesta. Puede seguir supervisando el progreso del envío con el método anterior o visitando el centro de Partners.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>Métodos para administrar un lanzamiento de paquete gradual

Puedes lanzar gradualmente los paquetes actualizados de un envío de aplicación para un porcentaje de los clientes de tu aplicación en Windows 10. Esto te permite supervisar los comentarios y los datos analíticos de los paquetes específicos para asegurarte de que estás seguro sobre la actualización antes de hacer un lanzamiento más amplio. Puedes cambiar el porcentaje de lanzamiento (o detener la actualización) para un envío publicado sin tener que crear un nuevo envío. Para obtener más información, incluidas las instrucciones para habilitar y administrar una implementación de paquetes gradual en el centro de Partners, consulte [este artículo](../publish/gradual-package-rollout.md).

Para habilitar mediante programación una implementación gradual de paquetes para el envío de una aplicación, siga este proceso mediante los métodos de la API de envío de Microsoft Store:

  1. [Crear un envío de aplicación](create-an-app-submission.md) u [obtener un envío de aplicación existente](get-an-app-submission.md).
  2. En los datos de respuesta, busca el recurso [packageRollout](#package-rollout-object), establece el campo *isPackageRollout* en **true** y establece el campo *packageRolloutPercentage* en el porcentaje de los clientes de tu aplicación que deben obtener los paquetes actualizados.
  3. Pasa los datos de envío de aplicación actualizados al método para [actualizar un envío de aplicación](update-an-app-submission.md).

Después de habilitar un lanzamiento de paquete gradual para un envío de aplicación, puedes usar los siguientes métodos para obtener, actualizar, detener o finalizar mediante programación el lanzamiento gradual.

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">Obtener la información de lanzamiento gradual para un envío de aplicación</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">Actualizar el porcentaje de lanzamiento gradual de un envío de aplicación</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">Detener el lanzamiento gradual de un envío de aplicación</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">Finalizar el lanzamiento gradual de un envío de aplicación</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>Ejemplos de código para administrar envíos de aplicaciones

En los siguientes artículos se proporcionan ejemplos de código detallados que muestran cómo crear un envío de aplicación en varios lenguajes de programación:

* [Ejemplo de C#: envíos de aplicaciones, complementos y pilotos](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de C#: envío de aplicación con opciones de juego y tráileres](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Ejemplo de Java: envíos de aplicaciones, complementos y pilotos](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de Java: envío de aplicación con opciones de juego y tráileres](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Ejemplo de Python: envíos de aplicaciones, complementos y pilotos](python-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplo de Python: envío de aplicación con opciones de juego y tráileres](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Módulo de PowerShell de StoreBroker

Como alternativa a llamar a la API de envío de Microsoft Store directamente, también proporcionamos un módulo de PowerShell de código abierto que implementa una interfaz de línea de comandos sobre la API. Este módulo se denomina [StoreBroker](https://github.com/Microsoft/StoreBroker). Puede usar este módulo para administrar los envíos de aplicaciones, vuelos y complementos desde la línea de comandos en lugar de llamar directamente a la API de envío de Microsoft Store, o simplemente puede examinar el origen para ver más ejemplos de cómo llamar a esta API. El módulo StoreBroker se usa activamente en Microsoft como la manera principal en que se envían a la tienda muchas aplicaciones propias.

Para obtener más información, consulte nuestra [Página de StoreBroker en github](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Recursos de datos
Los métodos de la API de envío Microsoft Store para administrar los envíos de aplicaciones usan los siguientes recursos de datos JSON.

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>Recurso de envío de aplicación

Este recurso describe un envío de aplicación.

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
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
            "description": "Main page",
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

Este recurso tiene los siguientes valores.

| Value      | Tipo   | Descripción      |
|------------|--------|-------------------|
| id            | string  | Identificador del envío. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un envío de aplicación](create-an-app-submission.md), [obtener todas las aplicaciones](get-all-apps.md)y [obtener una aplicación](get-an-app.md). En el caso de un envío creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de envío del centro de Partners.  |
| applicationCategory           | string  |   Cadena que especifica la [categoría o subcategoría](../publish/category-and-subcategory-table.md) de la aplicación. Las categorías y subcategorías se combinan en una cadena simple mediante el guion bajo "_" como, por ejemplo, **BooksAndReference_EReader**.      |  
| Precios           |  object  | Un [recurso de precios](#pricing-object) que contiene información sobre precios para la aplicación.        |   
| visibilidad           |  string  |  Visibilidad de la aplicación. Puede ser uno de los siguientes valores: <ul><li>Hidden</li><li>Público</li><li>Privados</li><li>NotSet</li></ul>       |   
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los siguientes valores: <ul><li>Inmediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |  
| listings           |   object  |  Diccionario de pares de clave-valor en el cual cada clave hace referencia al código de un país y cada valor hace referencia a un [recurso de lista](#listing-object) que contiene la descripción de la aplicación.       |   
| hardwarePreferences           |  array  |   Matriz de cadenas que definen las [preferencias de hardware](../publish/enter-app-properties.md) de la aplicación. Puede ser uno de los siguientes valores: <ul><li>Tocar</li><li>Teclado</li><li>Mouse</li><li>Cámara</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonía</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   Indica si Windows puede incluir datos de la aplicación en copias de seguridad automáticas de OneDrive. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](../publish/product-declarations.md).   |   
| canInstallOnRemovableMedia           |  boolean  |   Indica si los clientes pueden instalar la aplicación en el almacenamiento extraíble. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](../publish/product-declarations.md).     |   
| isGameDvrEnabled           |  boolean |   Indica si se habilita game DVR en la aplicación.    |   
| gamingOptions           |  array |   Una matriz que contiene un [recurso de opciones](#gaming-options-object) de juego que define la configuración relacionada con el juego para la aplicación.     |   
| hasExternalInAppProducts           |     boolean          |   Indica si la aplicación permite a los usuarios realizar compras fuera del sistema de comercio Microsoft Store. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](../publish/product-declarations.md).     |   
| meetAccessibilityGuidelines           |    boolean           |  Indica si la aplicación se ha probado para garantizar que cumple las directrices de accesibilidad. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](../publish/product-declarations.md).      |   
| notesForCertification           |  string  |   Contiene [notas para la certificación](../publish/notes-for-certification.md) de la aplicación.    |    
| status           |   string  |  Estado del envío. Puede ser uno de los siguientes valores: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicación</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificación</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   object  | Un [recurso de detalles de estado](#status-details-object) que contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores.       |    
| fileUploadUrl           |   string  | URI de firma de acceso compartido (SAS) para cargar los paquetes para el envío. Si va a agregar nuevos paquetes, mostrar imágenes o archivos de finalizador para el envío, cargue el archivo ZIP que contiene los paquetes e imágenes en este URI. Para obtener más información, consulta [Crear un envío de aplicación](#create-an-app-submission).       |    
| applicationPackages           |   array  | Una matriz de [recursos de paquete de aplicación](#application-package-object) que proporcionan detalles acerca de cada paquete del envío. |    
| packageDeliveryOptions    | object  | Un [recurso de opciones de entrega de paquete](#package-delivery-options-object) que contiene el lanzamiento de paquete gradual y la configuración de actualización obligatoria para el envío.  |
| enterpriseLicensing           |  string  |  Uno de los [valores de licencia de empresa](#enterprise-licensing) indica el comportamiento de la licencia de empresa de la aplicación.  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  Indica si se permite que Microsoft [tenga la aplicación disponible para futuras familias de dispositivos Windows 10](../publish/set-app-pricing-and-availability.md).    |    
| allowTargetFutureDeviceFamilies           | object   |  Un diccionario de pares clave y valor, donde cada clave es una [familia de dispositivos Windows 10](../publish/set-app-pricing-and-availability.md) y cada valor es un booleano que indica si la aplicación puede seleccionar como destino la familia de dispositivos especificada.     |    
| friendlyName           |   string  |  Nombre descriptivo del envío, tal como se muestra en el centro de Partners. Este valor se genera automáticamente cuando se crea el envío.       |  
| clip           |  array |   Una matriz que contiene hasta 15 [recursos de finalizador](#trailer-object) que representan finalizadores de vídeo para la lista de aplicaciones.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>Recurso de precios

Este recurso contiene información sobre precios de la aplicación. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Cadena que especifica el período de prueba de la aplicación. Puede ser uno de los siguientes valores: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados de la aplicación en mercados específicos](../publish/define-market-selection.md). Los elementos de este diccionario reemplazan el precio base especificado por el valor de *priceId* para el mercado especificado.      |     
|  sales (ventas)               |   array      |  En **desuso**. Una matriz de [recursos de venta](#sale-object) que contienen información de ventas de la aplicación.   |     
|  priceId               |   string      |  [Franja de precios](#price-tiers) que especifica el [precio base](../publish/define-market-selection.md) de la aplicación.   |     
|  isAdvancedPricingModel               |   boolean      |  Si **es true**, la cuenta de desarrollador tiene acceso al conjunto expandido de niveles de precio de 99 usd a 1999,99 USD. Si **es false**, la cuenta de desarrollador tiene acceso al conjunto original de niveles de precio de 99 usd a 999,99 USD. Para obtener más información sobre los distintos niveles, consulte [planes de tarifa](#price-tiers).<br/><br/>**Nota:** &nbsp; &nbsp; Este campo es de solo lectura.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Recurso de venta

Este recurso contiene información de venta de una aplicación.

> [!IMPORTANT]
> Ya no se admite el recurso de **venta** y actualmente no se pueden obtener o modificar los datos de ventas de un envío de aplicación mediante el Microsoft Store API de envío. En el futuro, actualizaremos el Microsoft Store API de envío para introducir una nueva forma de acceder mediante programación a la información de ventas de los envíos de aplicaciones.
>    * Después de llamar al [método GET para obtener un envío de aplicación](get-an-app-submission.md), el valor de *sales* estará vacío. Puede seguir usando el centro de partners para obtener los datos de venta para el envío de la aplicación.
>    * Cuando se llama al [método PUT para actualizar un envío de aplicación](update-an-app-submission.md), la información del valor de *sales* se omite. Puede seguir usando el centro de partners para cambiar los datos de venta para el envío de la aplicación.

Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción    |
|-----------------|---------|------|
|  name               |    string     |   Nombre de la venta.    |     
|  basePriceId               |   string      |  [Franja de precios](#price-tiers) que se usará para el precio base de la venta.    |     
|  startDate               |   string      |   Fecha de inicio de la venta en formato ISO 8601.  |     
|  endDate               |   string      |  Fecha de finalización de la venta en formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados de la aplicación en mercados específicos](../publish/define-market-selection.md). Los elementos de este diccionario reemplazan el precio base especificado por el valor *basePriceId* para el mercado especificado.    |


<span id="listing-object" />

### <a name="listing-resource"></a>Recurso de lista

Este recurso contiene la información de descripción de una aplicación. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción                  |
|-----------------|---------|------|
|  baseListing               |   object      |  Información de [descripción de base](#base-listing-object) de la aplicación, que define la información de descripción predeterminada para todas las plataformas.   |     
|  platformOverrides               | object |   Un diccionario de pares clave-valor, donde cada clave es una cadena que identifica una plataforma para la cual se va a reemplazar la información de descripción y cada valor es un recurso de [descripción de base](#base-listing-object) (que contiene solo los valores de la descripción del título) que especifica la información de descripción que se va reemplazar de la plataforma especificada. Las claves pueden tener los siguientes valores: <ul><li>Desconocido</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>Recursos de descripción base

Este recurso contiene la información de descripción de base de una aplicación. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  [Información de copyright o marcas comerciales](../publish/create-app-store-listings.md) opcional.  |
|  keywords                |  array       |  Matriz de [palabras clave](../publish/create-app-store-listings.md) para ayudar a tu aplicación a aparecer en los resultados de búsqueda.    |
|  licenseTerms                |    string     | [Términos de licencia](../publish/create-app-store-listings.md) opcionales para tu aplicación.     |
|  privacyPolicy                |   string      |   Este valor está obsoleto. Para establecer o cambiar la dirección URL de la Directiva de privacidad de la aplicación, debe hacerlo en la página [propiedades](../publish/enter-app-properties.md#privacy-policy-url) del centro de Partners. Puede omitir este valor de las llamadas a la API de envío. Si establece este valor, se omitirá.       |
|  supportContact                |   string      |  Este valor está obsoleto. Para establecer o cambiar la dirección URL de contacto de soporte técnico o la dirección de correo electrónico de la aplicación, debe hacerlo en la página de  [propiedades](../publish/enter-app-properties.md#support-contact-info) del centro de Partners. Puede omitir este valor de las llamadas a la API de envío. Si establece este valor, se omitirá.        |
|  websiteUrl                |   string      |  Este valor está obsoleto. Para establecer o cambiar la dirección URL de la Página Web de la aplicación, debe hacerlo en la página de  [propiedades](../publish/enter-app-properties.md#website) del centro de Partners. Puede omitir este valor de las llamadas a la API de envío. Si establece este valor, se omitirá.      |    
|  description               |    string     |   [Descripción](../publish/create-app-store-listings.md) de la aplicación.   |     
|  features               |    array     |  Matriz de hasta 20 cadenas que enumera las [características](../publish/create-app-store-listings.md) de tu aplicación.     |
|  releaseNotes               |  string       |  [Notas de la versión](../publish/create-app-store-listings.md) de tu aplicación.    |
|  images               |   array      |  Matriz de datos de recursos de [imagen e icono](#image-object) para la descripción de la aplicación.  |
|  recommendedHardware               |   array      |  Matriz de hasta 11 cadenas que enumera las [configuraciones de hardware recomendadas](../publish/create-app-store-listings.md#additional-information) para tu aplicación.     |
|  minimumHardware               |     string    |  Una matriz de hasta 11 cadenas que enumeran las [configuraciones de hardware mínimas](../publish/create-app-store-listings.md#additional-information) para la aplicación.    |  
|  title               |     string    |   Título de la descripción de la aplicación.   |  
|  shortDescription               |     string    |  Solo se usa para juegos. Esta descripción aparece en la sección de **información** de la central de juegos en Xbox One y ayuda a los clientes a comprender mejor su juego.   |  
|  shortTitle               |     string    |  Una versión más corta del nombre del producto. Si se proporciona, este nombre más corto puede aparecer en varios lugares de Xbox One (durante la instalación, en logros, etc.) en lugar del título completo del producto.    |  
|  sortTitle               |     string    |   Si el producto se puede clasificar por orden alfabético de maneras diferentes, puede especificar otra versión aquí. Esto puede ayudar a los clientes a encontrar el producto más rápidamente al buscar.    |  
|  voiceTitle               |     string    |   Un nombre alternativo para el producto que, si se proporciona, se puede usar en la experiencia de audio en Xbox One al usar KINECT o auriculares.    |  
|  devStudio               |     string    |   Especifique este valor si desea incluir un campo **desarrollado por** en la lista. (El campo **publicado por** mostrará el nombre para mostrar del publicador asociado a su cuenta, independientemente de si proporciona o no un valor de *devStudio* ).    |  

<span id="image-object" />

### <a name="image-resource"></a>Recurso de imagen

Este recurso contiene datos de imagen e icono para una descripción de la aplicación. Para obtener más información sobre las imágenes y los iconos de una lista de aplicaciones, consulte [capturas de pantallas e imágenes de aplicaciones](../publish/app-screenshots-and-images.md). Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción           |
|-----------------|---------|------|
|  fileName               |    string     |   Nombre del archivo de imagen en el archivo ZIP que cargaste para el envío.    |     
|  fileStatus               |   string      |  Estado del archivo de imagen. Puede ser uno de los siguientes valores: <ul><li>None</li><li>PendingUpload</li><li>Cargado</li><li>PendingDelete</li></ul>   |
|  id  |  string  | IDENTIFICADOR de la imagen. Este valor lo proporciona el centro de Partners.  |
|  description  |  string  | Descripción de la imagen.  |
|  imageType  |  string  | Indica el tipo de la imagen. Actualmente se admiten las siguientes cadenas. <p/>[Imágenes de captura de pantalla](../publish/app-screenshots-and-images.md#screenshots): <ul><li>Captura de pantalla (Use este valor para la captura de pantalla del escritorio)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[Logotipos](../publish/app-screenshots-and-images.md#store-logos)de la tienda:<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>Icono (Use este valor para el logotipo 1:1 300 x 300 píxeles)</li></ul><p/>[Imágenes promocionales](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Imágenes de Xbox](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[Imágenes promocionales opcionales](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>Recurso opciones de juego

Este recurso contiene la configuración relacionada con el juego de la aplicación. Los valores de este recurso corresponden a la [configuración de juego](../publish/enter-app-properties.md#game-settings) de los envíos del centro de Partners.

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
|  Géneros               |    array     |  Una matriz de una o varias de las siguientes cadenas que describen los géneros del juego: <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    boolean     |  Indica si el juego es compatible con el multijugador local.      |     
|  isLocalCooperative               |   boolean      |  Indica si el juego admite la Co-op local.    |     
|  isOnlineMultiplayer               |   boolean      |  Indica si el juego admite el multijugador en línea.    |     
|  isOnlineCooperative               |   boolean      |  Indica si el juego admite la Co-op en línea.    |     
|  localMultiplayerMinPlayers               |   int      |   Especifica el número mínimo de jugadores que el juego admite para el multijugador local.   |     
|  localMultiplayerMaxPlayers               |   int      |   Especifica el número máximo de jugadores que el juego admite para el multijugador local.  |     
|  localCooperativeMinPlayers               |   int      |   Especifica el número mínimo de jugadores que admite el juego para la Co-op local.  |     
|  localCooperativeMaxPlayers               |   int      |   Especifica el número máximo de jugadores que admite el juego para la Co-op local.  |     
|  isBroadcastingPrivilegeGranted               |   boolean      |  Indica si el juego admite la difusión.   |     
|  isCrossPlayEnabled               |   boolean      |   Indica si el juego admite sesiones de varios jugadores entre reproductores en equipos con Windows 10 y Xbox.  |     
|  kinectDataForExternal               |   string      |  Uno de los siguientes valores de cadena que indica si el juego puede recopilar datos de Kinect y enviarlos a servicios externos: <ul><li>NotSet</li><li>Desconocido</li><li>habilitado</li><li>Disabled</li></ul>   |

> [!NOTE]
> El recurso *gamingOptions* se agregó en el 2017 de mayo, una vez que la API de envío de Microsoft Store se lanzó por primera vez a los desarrolladores. Si creó un envío para una aplicación a través de la API de envío antes de que se introdujera este recurso y este envío todavía está en curso, este recurso será null para los envíos de la aplicación hasta que confirme correctamente el envío o lo elimine. Si el recurso *gamingOptions* no está disponible para los envíos de una aplicación, el campo *hasAdvancedListingPermission* del recurso de la [aplicación](get-app-data.md#application_object) devuelto por el método [obtener una aplicación](get-an-app.md) es false.

<span id="status-details-object" />

### <a name="status-details-resource"></a>Recurso de detalles de estado

Este recurso contiene detalles adicionales sobre el estado de un envío. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción         |
|-----------------|---------|------|
|  errors               |    object     |   Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de errores del envío.    |     
|  warnings               |   object      | Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de advertencias del envío.      |
|  certificationReports               |     object    |   Una matriz de [recursos de informe de certificación](#certification-report-object) que proporcionan acceso a los datos del informe de certificación del envío. Si se produce un error en la certificación, puedes examinar estos informes para obtener más información.   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Recurso de detalle de estado

Este recurso contiene información adicional acerca de las advertencias o los errores relacionados de un envío. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
|  código               |    string     |   Un [código de estado de envío](#submission-status-code) que describe el tipo de error o advertencia.   |     
|  detalles               |     string    |  Mensaje con más detalles sobre el problema.     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>Recurso de paquete de aplicación

Este recurso contiene detalles sobre un paquete de aplicación para el envío.

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

> [!NOTE]
> Cuando se llama al método de [envío de una aplicación de actualización](update-an-app-submission.md) , solo se requieren los valores *filename*, *fileStatus*, *minimumDirectXVersion*y *minimumSystemRam* de este objeto en el cuerpo de la solicitud. Los demás valores se rellenan con el centro de Partners.

| Value           | Tipo    | Descripción                   |
|-----------------|---------|------|
| fileName   |   string      |  Nombre del paquete.    |  
| fileStatus    | string    |  Estado del paquete. Puede ser uno de los siguientes valores: <ul><li>None</li><li>PendingUpload</li><li>Cargado</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Id. que identifica de manera exclusiva el paquete. Este valor lo proporciona el centro de Partners.   |     
| version    |  string   |  Versión del paquete de aplicación. Para obtener más información, consulta [Numeración de la versión del paquete](../publish/package-version-numbering.md).   |   
| arquitectura    |  string   |  Arquitectura del paquete (por ejemplo, ARM).   |     
| languages    | array    |  Matriz de códigos de idioma para los idiomas que admite la aplicación. Para más información, consulte [Lenguajes admitidos](../publish/supported-languages.md).    |     
| capabilities    |  array   |  Matriz de funcionalidades necesarias para el paquete. Para obtener más información acerca de las funcionalidades, consulta [Declaraciones de funcionalidades de las aplicaciones](../packaging/app-capability-declarations.md).   |     
| minimumDirectXVersion    |  string   |  Versión mínima de DirectX que admite el paquete de aplicación. Esto solo se puede establecer para aplicaciones destinadas a Windows 8. x. En el caso de las aplicaciones que tienen como destino otras versiones del sistema operativo, este valor debe estar presente cuando se llama al método de envío de la [actualización de una aplicación](update-an-app-submission.md) , pero se omite el valor especificado. Puede ser uno de los siguientes valores: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  RAM mínima que requiere el paquete de aplicación. Esto solo se puede establecer para aplicaciones destinadas a Windows 8. x. En el caso de las aplicaciones que tienen como destino otras versiones del sistema operativo, este valor debe estar presente cuando se llama al método de envío de la [actualización de una aplicación](update-an-app-submission.md) , pero se omite el valor especificado. Puede ser uno de los siguientes valores: <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Matriz de cadenas que representan las familias de dispositivos a las que está destinado el paquete. Este valor se usa solo para los paquetes destinados a Windows 10. para los paquetes destinados a versiones anteriores, este valor es **None**. Las siguientes cadenas de la familia de dispositivos se admiten actualmente para los paquetes de Windows 10, donde *{0}* es una cadena de la versión de Windows 10 como 10.0.10240.0, 10.0.10586.0 o 10.0.14393.0: <ul><li>Versión mínima de Windows. universal *{0}*</li><li>Versión mínima de Windows. Desktop *{0}*</li><li>Versión mínima de Windows. Mobile *{0}*</li><li>Versión mínima de Windows. Xbox *{0}*</li><li>Versión mínima de Windows. Holographic *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Recurso de informe de certificación

Este recurso proporciona acceso a los datos del informe de certificación de un envío. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción             |
|-----------------|---------|------|
|     date            |    string     |  Fecha y hora en que se generó el informe, en formato ISO 8601.    |
|     reportUrl            |    string     |  Dirección URL en la que puedes obtener acceso al informe.    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Recurso de opciones de entrega de paquete

Este recurso contiene ajustes de lanzamiento de paquete gradual y de actualización obligatoria para el envío.

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

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
| packageRollout   |   object      |  Un [recurso de lanzamiento de paquete](#package-rollout-object) que contiene la configuración del lanzamiento de paquete gradual para el envío.   |  
| isMandatoryUpdate    | boolean    |  Indica si vas a tratar los paquetes de este envío como obligatorios para la instalación automática de actualizaciones de la aplicación. Para obtener más información sobre los paquetes obligatorios para la instalación automática de actualizaciones de la aplicación, consulta [Descargar e instalar actualizaciones de paquete para tu aplicación](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  date   |  La fecha y la hora en que los paquetes de este envío se convierten en obligatorios, en formato ISO 8601 y zona horaria UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Recurso de lanzamiento de paquete

Este recurso contiene la [configuración de lanzamiento de paquete](#manage-gradual-package-rollout) gradual para el envío. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  Indica si el lanzamiento de paquete gradual está habilitado para el envío.    |  
| packageRolloutPercentage    | FLOAT    |  El porcentaje de usuarios que recibirá los paquetes en el lanzamiento gradual.    |  
| packageRolloutStatus    |  string   |  Una de las siguientes cadenas, que indica el estado del lanzamiento de paquete gradual: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  string   |  El identificador del envío que recibirán los clientes que no obtengan los paquetes de lanzamiento gradual.   |          

> [!NOTE]
> Los valores *packageRolloutStatus* y *fallbackSubmissionId* se asignan por el centro de Partners y no están diseñados para que los establezca el desarrollador. Si incluye estos valores en un cuerpo de solicitud, estos valores se omitirán.

<span id="trailer-object" />

### <a name="trailers-resource"></a>Recursos de finalizadores

Este recurso representa un finalizador de vídeo para la lista de aplicaciones. Los valores de este recurso se corresponden con las opciones de [finalizaciones](../publish/app-screenshots-and-images.md#trailers) de envío del centro de Partners.

Puede Agregar hasta 15 recursos de finalizador a la matriz de *finalizadores* en un [recurso de envío](#app-submission-object)de la aplicación. Para cargar archivos de vídeo y imágenes en miniatura para un envío, agregue estos archivos al mismo archivo ZIP que contiene los paquetes y enumere las imágenes para el envío y, a continuación, cargue este archivo ZIP en el URI de la firma de acceso compartido (SAS) para el envío. Para más información sobre la carga del archivo ZIP en el URI de SAS, consulte [crear un envío de aplicación](#create-an-app-submission).

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
|  id               |    string     |   IDENTIFICADOR del finalizador. Este valor lo proporciona el centro de Partners.   |
|  videonombredearchivo               |    string     |    Nombre del archivo de vídeo de finalizador en el archivo ZIP que contiene los archivos para el envío.    |     
|  videoFileId               |   string      |  IDENTIFICADOR del archivo de vídeo de finalizador. Este valor lo proporciona el centro de Partners.   |     
|  trailerAssets               |   object      |  Un diccionario de pares clave-valor, donde cada clave es un código de idioma y cada valor es un [recurso de recursos de finalizador](#trailer-assets-object) que contiene activos adicionales específicos de la configuración regional para el finalizador. Para obtener más información sobre los códigos de idioma admitidos, consulte [idiomas admitidos](../publish/supported-languages.md).    |     

> [!NOTE]
> El recurso de *finalizador* se agregó en el 2017 de mayo, después de que la API de envío de Microsoft Store se lanzó por primera vez a los desarrolladores. Si creó un envío para una aplicación a través de la API de envío antes de que se introdujera este recurso y este envío todavía está en curso, este recurso será null para los envíos de la aplicación hasta que confirme correctamente el envío o lo elimine. Si el recurso de *finalizador* no está disponible para los envíos de una aplicación, el campo *hasAdvancedListingPermission* del [recurso de aplicación](get-app-data.md#application_object) devuelto por el método [obtener una aplicación](get-an-app.md) es false.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>Recurso de recursos de finalizador

Este recurso contiene recursos adicionales específicos de la configuración regional para un finalizador que se define en un [recurso de finalizador](#trailer-object). Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
| title   |   string      |  Título localizado del finalizador. El título se muestra cuando el usuario juega el finalizador en modo de pantalla completa.     |  
| imageList    | array    |   Una matriz que contiene un recurso de [imagen](#image-for-trailer-object) que proporciona la imagen en miniatura para el finalizador. Solo puede incluir un recurso de [imagen](#image-for-trailer-object) en esta matriz.  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>Recurso de imagen (para un finalizador)

Este recurso describe la imagen en miniatura de un finalizador. Este recurso tiene los siguientes valores.

| Value           | Tipo    | Descripción           |
|-----------------|---------|------|
|  fileName               |    string     |   Nombre del archivo de imagen en miniatura del archivo ZIP que cargó para el envío.    |     
|  id  |  string  | IDENTIFICADOR de la imagen en miniatura. Este valor lo proporciona el centro de Partners.  |
|  description  |  string  | La descripción de la imagen en miniatura. Este valor solo es metadatos y no se muestra a los usuarios.   |

<span/>

## <a name="enums"></a>Enumeraciones

Estos métodos usan las enumeraciones siguientes.

<span id="price-tiers" />

### <a name="price-tiers"></a>Franjas de precio

Los siguientes valores representan los niveles de precios disponibles en el recurso de [recursos de precios](#pricing-object) para el envío de una aplicación.

| Value           | Descripción        |
|-----------------|------|
|  Base               |   No se establece la franja de precios; usa el precio base para la aplicación.      |     
|  NotAvailable              |   La aplicación no está disponible en la región especificada.    |     
|  Gratuito              |   La aplicación es gratuita.    |    
|  Nivel*XXX*               |   Una cadena que especifica el nivel de precios de la aplicación, en el formato **nivel<em>xxxx</em>**. Actualmente, se admiten los siguientes intervalos de niveles de precios:<br/><br/><ul><li>Si el valor de *isAdvancedPricingModel* del [recurso de precios](#pricing-object) es **true**, los valores de nivel de precios disponibles para su cuenta son **Tier1012**  -  **Tier1424**.</li><li>Si el valor de *isAdvancedPricingModel* del [recurso de precios](#pricing-object) es **false**, los valores de nivel de precios disponibles para su cuenta son **Tier2**  -  **Tier96**.</li></ul>Para ver la tabla completa de los planes de tarifa que están disponibles para su cuenta de desarrollador, incluidos los precios específicos del mercado que están asociados a cada nivel, vaya a la página de **precios y disponibilidad** para cualquiera de los envíos de la aplicación **en el centro** de Partners y haga clic en el vínculo **ver tabla** en la sección **mercados y precios personalizados** (para algunas cuentas de desarrollador, este vínculo está en    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>Valores de licencia de empresa

Los valores siguientes representan el comportamiento de la licencia de la organización de la aplicación. Para obtener más información acerca de estas opciones, consulta [Opciones de licencia organizativas](../publish/organizational-licensing.md).

> [!NOTE]
> Aunque puede configurar las opciones de licencias de la organización para el envío de una aplicación a través de la API de envío, no puede usar esta API para publicar envíos de [compras por volumen a través del Microsoft Store para empresas y Microsoft Store para educación](../publish/organizational-licensing.md). Para publicar envíos en el Microsoft Store para empresas y Microsoft Store para educación, debe utilizar el centro de Partners.


| Value           |  Descripción      |
|-----------------|---------------|
| None            |     Tu aplicación no se pone a disposición de las empresas con licencias por volumen administradas por la Tienda (en línea).         |     
| En línea        |     Tu aplicación se pone a disposición de las empresas con licencias por volumen administradas por la Tienda (en línea).  |
| OnlineAndOffline | Tu aplicación se pone a disposición de las empresas con licencias por volumen administradas por la Tienda (en línea) y también a través de licencias desconectadas (sin conexión). |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>Código de estado del envío

Los valores siguientes representan el código de estado de un envío.

| Value           |  Descripción      |
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
| Otros  | El envío presenta un estado no reconocido o sin clasificar. |
| PackageValidationWarning | El proceso de validación del paquete genera una advertencia. |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Obtención de datos de la aplicación mediante la API de envío de Microsoft Store](get-app-data.md)
* [Envíos de aplicaciones en el centro de Partners](../publish/app-submissions.md)