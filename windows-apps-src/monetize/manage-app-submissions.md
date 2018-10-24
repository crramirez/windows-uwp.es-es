---
author: Xansky
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: Usa estos métodos en la API de envío de Microsoft Store para administrar envíos para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows.
title: Administración de envíos de aplicaciones
ms.author: mhopkins
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, app submissions, envíos de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: f0edcde4916311a629d248b800320f6e1c596600
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5475340"
---
# <a name="manage-app-submissions"></a>Administración de envíos de aplicaciones

La API de envío de Microsoft Store proporciona métodos que puedes usar para administrar envíos para tus aplicaciones, incluidos los lanzamientos de paquetes graduales. Para obtener una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si usas la API de envío de Microsoft Store para crear un envío de una aplicación, asegúrate de realizar más cambios en el envío solo mediante la API, en lugar de en el panel del Centro de desarrollo. Si usas el panel para cambiar un envío que creaste originalmente mediante la API, ya no podrás cambiar o confirmar el envío con la API. En algunos casos, el envío podría quedar en un estado de error en el que no se puede continuar con el proceso de envío. Si esto ocurre, debes eliminar el envío y crear uno nuevo.

> [!IMPORTANT]
> No puedes usar esta API para publicar envíos para [compras por volumen a través de Microsoft Store para Empresas y Microsoft Store para Educación](../publish/organizational-licensing.md) o publicar envíos para [aplicaciones LOB](../publish/distribute-lob-apps-to-enterprises.md) directamente para empresas. Para estos dos escenarios, debes usar el panel del Centro de desarrollo de Windows para publicar el envío.


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>Métodos para administrar envíos de aplicaciones

Usa los siguientes métodos para obtener, crear, actualizar, confirmar o eliminar un envío de aplicación. Antes de poder usar estos métodos, la aplicación debe existir en tu cuenta del Centro de desarrollo y debes haber creado un envío para la aplicación del panel. Para obtener más información, consulta [Requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites).

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
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">Eliminar un envío de aplicación</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>Crear un envío de aplicación

Para crear un envío de aplicación, sigue este proceso.

1. Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
    > [!NOTE]
    > Asegúrate de que la aplicación ya tenga al menos un envío completado con la información de [clasificación por edades](https://msdn.microsoft.com/windows/uwp/publish/age-ratings) rellenada.

2. [Obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Debes pasar este token de acceso a los métodos de la API de envío de Microsoft Store. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

3. [Crea un envío de aplicación](create-an-app-submission.md) ejecutando el siguiente método en la API de envío de Microsoft Store. Este método crea un nuevo envío en curso, que es una copia de tu último envío publicado.

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    El cuerpo de la respuesta contiene un recurso [envío de aplicación](#app-submission-object) que incluye el identificador del nuevo envío, el URI de firma de acceso compartido (SAS) para cargar los archivos relacionados para el envío a Azure Blob Storage (como paquetes de aplicaciones, imágenes de descripciones y archivos de tráileres) y todos los datos del nuevo envío (como las descripciones y la información sobre precios).

    > [!NOTE]
    > Un URI de SAS proporciona acceso a un recurso seguro en el almacenamiento de Azure sin necesidad de claves de cuenta. Para obtener información general sobre los URI de SAS y su uso con Azure Blob Storage, consulta [Shared Access Signatures, Part 1: Understanding the SAS model (Firmas de acceso compartido, parte 1: información sobre el modelo SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) y [Firmas de acceso compartido, Parte 2: Creación y uso de una SAS con Almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si estás agregando nuevos paquetes, imágenes de descripciones o archivos de tráileres para el envío, [prepara los paquetes de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) y [prepara las imágenes y capturas de pantalla de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images). Agrega todos estos archivos a un archivo ZIP.

5. Revisa los datos de [envío de aplicación](#app-submission-object) con todos los cambios necesarios para el nuevo envío y ejecuta el siguiente método para [actualizar el envío de aplicación](update-an-app-submission.md).

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si estás agregando nuevos archivos para el envío, asegúrate de actualizar los datos de envío para que hagan referencia al nombre y a la ruta de acceso relativa de estos archivos en el archivo ZIP.

4. Si estás agregando nuevos paquetes, imágenes de descripciones o archivos de tráileres para el envío, carga el archivo ZIP en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando el URI de SAS que se proporcionó en el cuerpo de la respuesta del método POST que llamaste antes. Hay varias bibliotecas de Azure, que puedes usar para hacer esto en una variedad de plataformas, como:

    * [Biblioteca de cliente de Azure Storage para .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage SDK para Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK para Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    En el siguiente ejemplo de código C# se muestra cómo cargar el archivo ZIP en Azure Blob Storage usando la clase [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) en la biblioteca de cliente de Azure Storage para. NET. En este ejemplo se supone que el archivo ZIP ya se ha escrito en un objeto de secuencia.

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

7. Después de completarse correctamente la confirmación, el envío se remite a la Store para su ingesta. Para continuar con la supervisión del progreso del envío, puedes usar el método anterior o visitar el panel del Centro de desarrollo.

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>Métodos para administrar un lanzamiento de paquete gradual

Puedes lanzar gradualmente los paquetes actualizados de un envío de aplicación para un porcentaje de los clientes de tu aplicación en Windows 10. Esto te permite supervisar los comentarios y los datos analíticos de los paquetes específicos para asegurarte de que estás seguro sobre la actualización antes de hacer un lanzamiento más amplio. Puedes cambiar el porcentaje de lanzamiento (o detener la actualización) para un envío publicado sin tener que crear un nuevo envío. Para obtener más detalles, incluidas instrucciones sobre cómo habilitar y administrar un lanzamiento de paquete gradual en el panel del Centro de desarrollo, consulta [este artículo](../publish/gradual-package-rollout.md).

Para habilitar un lanzamiento de paquete gradual mediante programación para un envío de aplicación, sigue este proceso usando los siguientes métodos en la API de envío de Microsoft Store:

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

* [Muestra de C#: envíos de aplicaciones, complementos y pilotos](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Muestra de C#: envío de aplicación con opciones de juego y tráileres](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Muestra de Java: envíos de aplicaciones, complementos y pilotos](java-code-examples-for-the-windows-store-submission-api.md)
* [Muestra de Java: envío de aplicación con opciones de juego y tráileres](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Muestra de Python: envíos de aplicaciones, complementos y pilotos](python-code-examples-for-the-windows-store-submission-api.md)
* [Muestra de Python: envío de aplicación con opciones de juego y tráileres](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Módulo de StoreBroker PowerShell

Como alternativa a llamar a la API de envío de Microsoft Store directamente, también proporcionamos un módulo de PowerShell de código abierto que implementa una interfaz de línea de comandos en la parte superior de la API. Este módulo se denomina [StoreBroker](https://aka.ms/storebroker). Puedes usar este módulo para administrar tus envíos de aplicaciones, paquetes piloto y complementos desde la línea de comandos en lugar de llamar directamente a la API de envío de Microsoft Store o, simplemente, puedes examinar el origen para ver más ejemplos de cómo llamar a esta API. El módulo StoreBroker se usa activamente dentro de Microsoft como método principal para enviar muchas aplicaciones propias a la Store.

Para obtener más información, consulta nuestra [página de StoreBroker en GitHub](https://aka.ms/storebroker).

<span/>

## <a name="data-resources"></a>Recursos de datos
Los métodos de las API de envío de Microsoft Store para administrar los envíos de aplicaciones usan los siguientes recursos de datos JSON.

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

| Valor      | Tipo   | Descripción      |
|------------|--------|-------------------|
| id            | string  | Identificador del envío. Este identificador está disponible en los datos de respuesta de las solicitudes para [crear un envío de aplicación](create-an-app-submission.md), [obtener todas las aplicaciones](get-all-apps.md) y [obtener una aplicación](get-an-app.md). Para un envío creado en el panel del Centro de desarrollo, este id. también está disponible en la dirección URL de la página de envío del panel.  |
| applicationCategory           | string  |   Cadena que especifica la [categoría o subcategoría](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table) de la aplicación. Las categorías y subcategorías se combinan en una cadena simple mediante el guion bajo "_" como, por ejemplo, **BooksAndReference_EReader**.      |  
| pricing           |  object  | Un [recurso de precios](#pricing-object) que contiene información sobre precios para la aplicación.        |   
| visibility           |  string  |  Visibilidad de la app. Puede ser uno de los valores siguientes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | cadena  | Modo de publicación del envío. Puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |  
| listings           |   object  |  Diccionario de pares de clave-valor en el cual cada clave hace referencia al código de un país y cada valor hace referencia a un [recurso de lista](#listing-object) que contiene la descripción de la aplicación.       |   
| hardwarePreferences           |  array  |   Matriz de cadenas que definen las [preferencias de hardware](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences) de la aplicación. Puede ser uno de los valores siguientes: <ul><li>Función táctil</li><li>Teclado</li><li>Mouse</li><li>Cámara</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telefonía</li></ul>     |   
| automaticBackupEnabled           |  booleano  |   Indica si Windows puede incluir datos de la aplicación en copias de seguridad automáticas de OneDrive. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).   |   
| canInstallOnRemovableMedia           |  booleano  |   Indica si los clientes pueden instalar la aplicación en el almacenamiento extraíble. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| isGameDvrEnabled           |  booleano |   Indica si se habilita game DVR en la aplicación.    |   
| gamingOptions           |  matriz |   Matriz que contiene un [recurso de opciones de juegos](#gaming-options-object) que definir la configuración relacionada con juegos para la aplicación.     |   
| hasExternalInAppProducts           |     booleano          |   Indica si la aplicación permite a los usuarios realizar compras fuera del sistema de comercio de Microsoft Store. Para obtener más información, consulta [Declaraciones de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).     |   
| meetAccessibilityGuidelines           |    booleano           |  Indica si la aplicación se ha probado para garantizar que cumple las directrices de accesibilidad. Para obtener más información, consulta [App declarations (Declaraciones de las aplicaciones)](https://msdn.microsoft.com/windows/uwp/publish/app-declarations).      |   
| notesForCertification           |  string  |   Contiene [notas para la certificación](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification) de la aplicación.    |    
| status           |   string  |  Estado del envío. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicación</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   object  | Un [recurso de detalles de estado](#status-details-object) que contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores.       |    
| fileUploadUrl           |   string  | URI de firma de acceso compartido (SAS) para cargar los paquetes para el envío. Si estás agregando nuevos paquetes, imágenes de descripciones o archivos de tráileres para el envío, carga el archivo ZIP que contiene los paquetes y las imágenes en este URI. Para obtener más información, consulta [Crear un envío de aplicación](#create-an-app-submission).       |    
| applicationPackages           |   array  | Una matriz de [recursos de paquete de aplicación](#application-package-object) que proporcionan detalles acerca de cada paquete del envío. |    
| packageDeliveryOptions    | object  | Un [recurso de opciones de entrega de paquete](#package-delivery-options-object) que contiene el lanzamiento de paquete gradual y la configuración de actualización obligatoria para el envío.  |
| enterpriseLicensing           |  cadena  |  Uno de los [valores de licencia de empresa](#enterprise-licensing) indica el comportamiento de la licencia de empresa de la aplicación.  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  booleano   |  Indica si se permite que Microsoft [tenga la aplicación disponible para futuras familias de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families).    |    
| allowTargetFutureDeviceFamilies           | object   |  Un diccionario de pares clave y valor, donde cada clave es una [familia de dispositivos Windows 10](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families) y cada valor es un booleano que indica si la aplicación puede seleccionar como destino la familia de dispositivos especificada.     |    
| friendlyName           |   cadena  |  El nombre descriptivo del envío, como se muestra en el panel del Centro de desarrollo. Generas este valor cuando creas el envío.       |  
| tráileres           |  matriz |   Matriz que contiene hasta 15 [recursos de tráileres](#trailer-object) que representan tráileres de vídeo para la descripción de la aplicación.<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>Recurso de precios

Este recurso contiene información sobre precios de la aplicación. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
|  trialPeriod               |    string     |  Cadena que especifica el período de prueba de la aplicación. Puede ser uno de los valores siguientes: <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    object     |  Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados de la aplicación en mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Los elementos de este diccionario reemplazan el precio base especificado por el valor de *priceId* para el mercado especificado.      |     
|  sales               |   array      |  **En desuso**. Una matriz de [recursos de venta](#sale-object) que contienen información de ventas de la aplicación.   |     
|  priceId               |   string      |  Una [franja de precios](#price-tiers) que especifica el [precio base](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price) de la aplicación.   |     
|  isAdvancedPricingModel               |   booleano      |  Si está en **true**, tu cuenta de desarrollador tiene acceso al conjunto expandido de franjas de precios de 0,99USD a 1999,99USD. Si está en **false**, tu cuenta de desarrollador tiene acceso al conjunto original de franjas de precios de 0,99USD a 999,99USD. Para obtener más información sobre las diferentes franjas de precios, consulta [franjas de precios](#price-tiers).<br/><br/>**Nota**&nbsp;&nbsp;este campo es de solo lectura.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Recurso de venta

Este recurso contiene información de venta de una aplicación.

> [!IMPORTANT]
> El recurso **Sale** ya no se admite y actualmente no se pueden obtener ni modificar los datos de ventas del envío de una aplicación mediante la API de envío de Microsoft Store. En el futuro, actualizaremos la API de envío de Microsoft Store para incorporar una nueva forma de acceder mediante programación a la información de ventas de los envíos de aplicaciones.
>    * Después de llamar al [método GET para obtener un envío de aplicación](get-an-app-submission.md), el valor de *sales* estará vacío. Puedes seguir utilizando el panel del Centro de desarrollo para obtener los datos de ventas referentes al envío de la aplicación.
>    * Cuando se llama al [método PUT para actualizar un envío de aplicación](update-an-app-submission.md), la información del valor de *sales* se omite. Puedes seguir utilizando el panel del Centro de desarrollo para modificar los datos de ventas referentes al envío de la aplicación.

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción    |
|-----------------|---------|------|
|  name               |    string     |   Nombre de la venta.    |     
|  basePriceId               |   string      |  [Franja de precios](#price-tiers) que se usará para el precio base de la venta.    |     
|  startDate               |   string      |   Fecha de inicio de la venta en formato ISO 8601.  |     
|  endDate               |   string      |  Fecha de finalización de la venta en formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados de la aplicación en mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices). Los elementos de este diccionario reemplazan el precio base especificado por el valor *basePriceId* para el mercado especificado.    |


<span id="listing-object" />

### <a name="listing-resource"></a>Recurso de lista

Este recurso contiene la información de descripción de una aplicación. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                  |
|-----------------|---------|------|
|  baseListing               |   object      |  Información de [descripción de base](#base-listing-object) de la aplicación, que define la información de descripción predeterminada para todas las plataformas.   |     
|  platformOverrides               | object |   Un diccionario de pares clave-valor, donde cada clave es una cadena que identifica una plataforma para la cual se va a reemplazar la información de descripción y cada valor es un recurso de [descripción de base](#base-listing-object) (que contiene solo los valores de la descripción del título) que especifica la información de descripción que se va reemplazar de la plataforma especificada. Las claves pueden tener los siguientes valores: <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>Recursos de descripción base

Este recurso contiene la información de descripción de base de una aplicación. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   string      |  [Información de copyright o marcas comerciales](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info) opcional.  |
|  keywords                |  array       |  Matriz de [palabras clave](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords) para ayudar a tu aplicación a aparecer en los resultados de búsqueda.    |
|  licenseTerms                |    string     | [Términos de licencia](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms) opcionales para tu aplicación.     |
|  privacyPolicy                |   string      |   Esta valor está obsoleta. Para establecer o cambiar la dirección URL de directiva de privacidad de la aplicación, debes hacer esto en la página [Propiedades](../publish/enter-app-properties.md#privacy-policy-url) en el panel del Centro de desarrollo. Puedes omitir este valor de las llamadas a la API de envío. Si estableces este valor, se omitirá.       |
|  supportContact                |   string      |  Esta valor está obsoleta. Para establecer o cambiar la URL de contacto de soporte o la dirección de correo electrónico, debes hacer esto en la página [Propiedades](../publish/enter-app-properties.md#support-contact-info) en el panel del Centro de desarrollo. Puedes omitir este valor de las llamadas a la API de envío. Si estableces este valor, se ignorará.        |
|  websiteUrl                |   string      |  Esta valor está obsoleta. Para establecer o cambiar la dirección URL de la página web de la aplicación, debes hacer esto en la página [Propiedades](../publish/enter-app-properties.md#website) en el panel del Centro de desarrollo. Puedes omitir este valor de las llamadas a la API de envío. Si estableces este valor, se omitirá.      |    
|  description               |    string     |   [Descripción](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description) de la aplicación.   |     
|  features               |    array     |  Matriz de hasta 20 cadenas que enumera las [características](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features) de tu aplicación.     |
|  releaseNotes               |  string       |  [Notas de la versión](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes) de tu aplicación.    |
|  images               |   array      |  Matriz de datos de recursos de [imagen e icono](#image-object) para la descripción de la aplicación.  |
|  recommendedHardware               |   array      |  Matriz de hasta 11 cadenas que enumera las [configuraciones de hardware recomendadas](../publish/create-app-store-listings.md#additional-information) para tu aplicación.     |
|  minimumHardware               |     string    |  Matriz de hasta 11 cadenas que enumera las [configuraciones de hardware mínimas](../publish/create-app-store-listings.md#additional-information) para tu aplicación.    |  
|  title               |     string    |   Título de la descripción de la aplicación.   |  
|  shortDescription               |     cadena    |  Solo se usa para juegos Esta descripción aparece en la sección **Información** del hub de juegos en Xbox One y ayuda a los clientes a saber más acerca de tu juego.   |  
|  shortTitle               |     cadena    |  Se trata de una versión más corta del nombre de tu producto. Si se facilita, este nombre más corto puede aparecer en varios lugares de Xbox One (durante la instalación, en los logros, etc.) en lugar del título completo del producto.    |  
|  sortTitle               |     string    |   Si tu producto pudiera escribirse de diferentes maneras, puedes introducir aquí otra versión. Esto puede ayudar a los clientes a buscar más rápidamente el producto al realizar búsquedas.    |  
|  voiceTitle               |     cadena    |   Se trata de un nombre alternativo para el producto que, si se facilita, puede usarse en la experiencia de audio en Xbox One cuando se usa Kinect o un casco.    |  
|  devStudio               |     string    |   Especifica este valor si quieres incluir un campo **Desarrollado por** en la descripción. (El valor **Publicado por** indicará el nombre para mostrar del publicador asociado a tu cuenta, independientemente de si proporcionas un valor *devStudio*).    |  

<span id="image-object" />

### <a name="image-resource"></a>Recurso de imagen

Este recurso contiene datos de imagen e icono para una descripción de la aplicación. Para obtener más información acerca de las imágenes y los iconos para la descripción de una aplicación, consulta [Imágenes y capturas de pantalla de aplicación](../publish/app-screenshots-and-images.md). Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción           |
|-----------------|---------|------|
|  fileName               |    string     |   Nombre del archivo de imagen en el archivo ZIP que cargaste para el envío.    |     
|  fileStatus               |   string      |  Estado del archivo de imagen. Puede ser uno de los valores siguientes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  string  | Identificador de la imagen. Este valor está proporcionado por el Centro de desarrollo.  |
|  description  |  string  | Descripción de la imagen.  |
|  imageType  |  string  | Indica el tipo de la imagen. Actualmente se admiten las siguientes cadenas. <p/>[Imágenes de captura de pantalla](../publish/app-screenshots-and-images.md#screenshots): <ul><li>Captura de pantalla (usa este valor para la captura de pantalla de escritorio)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[Logotipos de la tienda](../publish/app-screenshots-and-images.md#store-logos):<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>Icono (usa este valor para el logotipo de 1:1 300 x 300 píxeles)</li></ul><p/>[Imágenes promocionales](../publish/app-screenshots-and-images.md#promotional-images): <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Imágenes de Xbox](../publish/app-screenshots-and-images.md#xbox-images): <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[Imágenes promocionales opcionales](../publish/app-screenshots-and-images.md#optional-promotional-images): <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>Recurso de opciones de juegos

Este recurso contiene configuración relacionada con juegos para la aplicación. Los valores de este recurso corresponden con la [configuración de juegos](../publish/enter-app-properties.md#game-settings) para los envíos en el panel del Centro de desarrollo.

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

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
|  genres               |    array     |  Matriz de una o varias de las siguientes cadenas que describen los géneros del juego: <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    boolean     |  Indica si el juego admite multijugador local.      |     
|  isLocalCooperative               |   boolean      |  Indica si el juego admite cooperación local.    |     
|  isOnlineMultiplayer               |   boolean      |  Indica si el juego admite multijugador en línea.    |     
|  isOnlineCooperative               |   boolean      |  Indica si el juego admite cooperación en línea.    |     
|  localMultiplayerMinPlayers               |   int      |   Especifica el número mínimo de jugadores que el juego admite para multijugador local.   |     
|  localMultiplayerMaxPlayers               |   int      |   Especifica el número máximo de jugadores que el juego admite para multijugador local.  |     
|  localCooperativeMinPlayers               |   int      |   Especifica el número mínimo de jugadores que el juego admite para cooperación local.  |     
|  localCooperativeMaxPlayers               |   int      |   Especifica el número máximo de jugadores que el juego admite para cooperación local.  |     
|  isBroadcastingPrivilegeGranted               |   boolean      |  Indica si el juego admite difusión.   |     
|  isCrossPlayEnabled               |   boolean      |   Indica si el juego admite sesiones de varios jugadores entre jugadores en equipos con Windows 10 y Xbox.  |     
|  kinectDataForExternal               |   string      |  Uno de los siguientes valores de cadena que indica si el juego puede recopilar datos de Kinect y enviarlos a los servicios externos: <ul><li>NotSet</li><li>Unknown</li><li>Habilitada</li><li>Deshabilitada</li></ul>   |

> [!NOTE]
> El recurso *gamingOptions* se agregó en mayo de 2017, después del lanzamiento inicial de la API de envío de Microsoft Store para los desarrolladores. Si has creado un envío para una aplicación a través de la API de envío antes de que se introdujera este recurso y este envío todavía está en curso, el recurso no podrá quedar vacío para envíos para la aplicación hasta que confirmes correctamente el envío o lo elimines. SI el recurso *gamingOptions* no está disponible para envíos de una aplicación, el campo *hasAdvancedListingPermission* del [recurso de aplicación](get-app-data.md#application_object) devuelto por el método [obtener una aplicación](get-an-app.md) es false.

<span id="status-details-object" />

### <a name="status-details-resource"></a>Recurso de detalles de estado

Este recurso contiene detalles adicionales sobre el estado de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción         |
|-----------------|---------|------|
|  errors               |    object     |   Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de errores del envío.    |     
|  warnings               |   object      | Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de advertencias del envío.      |
|  certificationReports               |     object    |   Una matriz de [recursos de informe de certificación](#certification-report-object) que proporcionan acceso a los datos del informe de certificación del envío. Si se produce un error en la certificación, puedes examinar estos informes para obtener más información.   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Recurso de detalle de estado

Este recurso contiene información adicional acerca de las advertencias o los errores relacionados de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
|  code               |    string     |   Un [código de estado de envío](#submission-status-code) que describe el tipo de error o advertencia.   |     
|  details               |     string    |  Mensaje con más detalles sobre el problema.     |


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
> Al llamar al método para [actualizar un envío de aplicación](update-an-app-submission.md), solo los valores *fileName*, *fileStatus*, *minimumDirectXVersion* y *minimumSystemRam* de este objeto son necesarios en el cuerpo de la solicitud. El Centro de desarrollo se encarga de rellenar el resto de valores.

| Valor           | Tipo    | Descripción                   |
|-----------------|---------|------|
| fileName   |   string      |  Nombre del paquete.    |  
| fileStatus    | string    |  Estado del paquete. Puede ser uno de los valores siguientes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Id. que identifica de manera exclusiva el paquete. Este valor lo proporciona el Centro de desarrollo.   |     
| versión    |  string   |  Versión del paquete de aplicación. Para obtener más información, consulta [Numeración de la versión del paquete](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  Arquitectura del paquete (por ejemplo, ARM).   |     
| languages    | array    |  Matriz de códigos de idioma para los idiomas que admite la aplicación. Para obtener más información, consulta [Idiomas admitidos](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Matriz de funcionalidades necesarias para el paquete. Para obtener más información acerca de las funcionalidades, consulta [Declaraciones de funcionalidades de las aplicaciones](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  Versión mínima de DirectX que admite el paquete de aplicación. Esto puede establecerse solo para aplicaciones destinadas a Windows 8. x. Para aplicaciones destinadas a otras versiones de sistema operativo, este valor debe estar presente cuando se llama al método [actualizar el envío de una aplicación](update-an-app-submission.md), pero el valor especificado se ignora. Este puede ser uno de los valores siguientes: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  RAM mínima que requiere el paquete de aplicación. Esto puede establecerse solo para aplicaciones destinadas a Windows 8. x. Para aplicaciones destinadas a otras versiones de sistema operativo, este valor debe estar presente cuando se llama al método [actualizar el envío de una aplicación](update-an-app-submission.md), pero el valor especificado se ignora. Este puede ser uno de los valores siguientes: <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  Matriz de cadenas que representan las familias de dispositivos a las que está destinado el paquete. Este valor se usa solo para los paquetes destinados a Windows 10. para los paquetes destinados a versiones anteriores, este valor es **None**. Las siguientes cadenas de familia de dispositivos se admiten actualmente para paquetes de Windows 10, donde *{0}* es una cadena de versión de Windows 10 como 10.0.10240.0, 10.0.10586.0 o 10.0.14393.0: <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Recurso de informe de certificación

Este recurso proporciona acceso a los datos del informe de certificación de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción             |
|-----------------|---------|------|
|     date            |    string     |  Fecha y hora en que se generó el informe, en formato ISO 8601.    |
|     reportUrl            |    cadena     |  Dirección URL en la que puedes obtener acceso al informe.    |


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

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
| packageRollout   |   object      |  Un [recurso de lanzamiento de paquete](#package-rollout-object) que contiene la configuración del lanzamiento de paquete gradual para el envío.   |  
| isMandatoryUpdate    | boolean    |  Indica si vas a tratar los paquetes de este envío como obligatorios para la instalación automática de actualizaciones de la aplicación. Para obtener más información sobre los paquetes obligatorios para la instalación automática de actualizaciones de la aplicación, consulta [Descargar e instalar actualizaciones de paquete para tu aplicación](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  fecha   |  La fecha y la hora en que los paquetes de este envío se convierten en obligatorios, en formato ISO 8601 y zona horaria UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Recurso de lanzamiento de paquete

Este recurso contiene la [configuración de lanzamiento de paquete](#manage-gradual-package-rollout) gradual para el envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
| isPackageRollout   |   booleano      |  Indica si el lanzamiento de paquete gradual está habilitado para el envío.    |  
| packageRolloutPercentage    | flotante    |  El porcentaje de usuarios que recibirá los paquetes en el lanzamiento gradual.    |  
| packageRolloutStatus    |  cadena   |  Una de las siguientes cadenas, que indica el estado del lanzamiento de paquete gradual: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  cadena   |  El id. del envío que recibirán los clientes que no obtengan los paquetes de lanzamiento gradual.   |          

> [!NOTE]
> El Centro de desarrollo asigna los valores *packageRolloutStatus* y *fallbackSubmissionId* y no está previsto que los establezca el desarrollador. Si incluyes estos valores en el cuerpo de una solicitud, se ignorarán.

<span id="trailer-object" />

### <a name="trailers-resource"></a>Recurso de tráileres

Este recurso representa un tráiler para la descripción de la aplicación. Los valores de este recurso corresponden con las opciones de [tráileres](../publish/app-screenshots-and-images.md#trailers) para los envíos en el panel del Centro de desarrollo.

Puedes agregar hasta 15 recursos de tráileres a la matriz de *tráileres* en un [recurso de envío de aplicación](#app-submission-object). Para cargar archivos de vídeo de tráiler e imágenes en miniatura para un envío, agrega estos archivos al mismo archivo ZIP que contiene los paquetes y las imágenes de descripciones para el envío y, a continuación, carga este archivo ZIP en el URI de firma de acceso compartido (SAS) para el envío. Para obtener más información acerca de la carga del archivo ZIP en el URI de SAS, consulta [Crear un envío de aplicación](#create-an-app-submission).

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

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
|  id               |    string     |   Identificador del tráiler. Este valor lo proporciona el Centro de desarrollo.   |
|  videoFileName               |    string     |    Nombre del archivo de vídeo de tráiler en el archivo ZIP que contiene archivos para el envío.    |     
|  videoFileId               |   string      |  Identificador del archivo de vídeo de tráiler. Este valor lo proporciona el Centro de desarrollo.   |     
|  trailerAssets               |   object      |  Diccionario de pares de claves y valores en el cual cada clave hace referencia al código de un idioma y cada valor hace referencia a un [recurso de activos de tráiler](#trailer-assets-object) que contiene los activos específicos de configuración regional adicional para el tráiler. Para obtener más información acerca de los códigos de idioma admitidos, consulta [Idiomas admitidos](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     

> [!NOTE]
> El recurso *trailers* se agregó en mayo de 2017, después del lanzamiento inicial de la API de envío de Microsoft Store para los desarrolladores. Si has creado un envío para una aplicación a través de la API de envío antes de que se introdujera este recurso y este envío todavía está en curso, el recurso no podrá quedar vacío para envíos para la aplicación hasta que confirmes correctamente el envío o lo elimines. Si el recurso *trailers* no está disponible para envíos de una aplicación, el campo *hasAdvancedListingPermission* del [recurso de aplicación](get-app-data.md#application_object) devuelto por el método [obtener una aplicación](get-an-app.md) es false.

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>Recurso de activos de tráiler

Este recurso contiene activos específicos de configuración regional adicionales para un final que se define en un [recurso de tráiler](#trailer-object). Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
| title   |   string      |  Título localizado del tráiler. El título se muestra cuando el usuario reproduce el tráiler en modo de pantalla completa.     |  
| imageList    | array    |   Matriz que contiene un recurso de [imagen](#image-for-trailer-object) que proporciona la imagen en miniatura para el tráiler. Solo puede incluir un recurso de [imagen](#image-for-trailer-object) en esta matriz.  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>Recurso de imagen (para un tráiler)

Este recurso describe la imagen en miniatura para un tráiler. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción           |
|-----------------|---------|------|
|  fileName               |    string     |   Nombre del archivo de imagen en miniatura en el archivo ZIP que cargaste para el envío.    |     
|  id  |  string  | Identificador de la imagen en miniatura. Este valor lo proporciona el Centro de desarrollo.  |
|  description  |  string  | Descripción de la imagen en miniatura. Este valor son solo metadatos y no se muestra a los usuarios.   |

<span/>

## <a name="enums"></a>Enumeraciones

Estos métodos usan las enumeraciones siguientes.

<span id="price-tiers" />

### <a name="price-tiers"></a>Franjas de precios

Los siguientes valores representan las franjas de precios disponibles en el [recurso de precios](#pricing-object) de un envío de aplicación.

| Valor           | Descripción        |
|-----------------|------|
|  Base               |   No se establece la franja de precios; usa el precio base para la aplicación.      |     
|  NotAvailable              |   La aplicación no está disponible en la región especificada.    |     
|  Free              |   La aplicación es gratuita.    |    
|  Tier*xxx*               |   Una cadena que especifica la franja de precios de la aplicación, con formato **Tier<em>xxxx</em>**. Actualmente, se admiten los siguientes intervalos de franjas de precios:<br/><br/><ul><li>Si el valor *isAdvancedPricingModel* del [recurso de precios](#pricing-object) es **true**, los valores disponibles de la franja de precios para tu cuenta son **Tier1012** - **Tier1424**.</li><li>Si el valor *isAdvancedPricingModel* del [recurso de precios](#pricing-object) es **false**, los valores disponibles de la franja de precios para tu cuenta son **Tier2** - **Tier96**.</li></ul>Para ver la tabla completa de franjas de precios que están disponibles para tu cuenta de desarrollador, incluidos los precios específicos del mercado asociados a cada franja, ve a la página **Precios y disponibilidad** para ver cualquiera de los envíos de aplicaciones del panel del Centro de desarrollo y haz clic en el vínculo **Ver tabla** en la sección **Mercados y precios personalizados** (en algunas cuentas de desarrollador, este vínculo está en la sección **Precios**).    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>Valores de licencia de empresa

Los valores siguientes representan el comportamiento de las licencias organizativas para la aplicación. Para obtener más información acerca de estas opciones, consulta [Opciones de licencias organizativas](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing).

> [!NOTE]
> Aunque puedes configurar las opciones de licencias organizativas para el envío de aplicaciones a través de la API de envío, no puedes usar esta API para publicar envíos para [compras por volumen a través de la Microsoft Store para Empresas y Microsoft Store para Educación](../publish/organizational-licensing.md). Para publicar envíos a Microsoft Store para Empresas y Microsoft Store para Educación, debes usar el panel del Centro de desarrollo de Windows.


| Valor           |  Descripción      |
|-----------------|---------------|
| None            |     Tu aplicación no se pone a disposición de las empresas con licencias por volumen administradas por la Store (en línea).         |     
| Online        |     Tu aplicación se pone a disposición de las empresas con licencias por volumen administradas por la Store (en línea).  |
| OnlineAndOffline | Tu aplicación se pone a disposición de las empresas con licencias por volumen administradas por la Store (en línea) y también a través de licencias desconectadas (sin conexión). |


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

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener datos de aplicación mediante la API de envío de Microsoft Store](get-app-data.md)
* [Envíos de aplicaciones en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)
