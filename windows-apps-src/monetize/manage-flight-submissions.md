---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: Use estos métodos en la API de envío de Microsoft Store para administrar los envíos de paquetes para las aplicaciones registradas en su cuenta del centro de Partners.
title: Administrar envíos de paquetes de vuelos
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, envíos de vuelos
ms.localizationpriority: medium
ms.openlocfilehash: 50596fdadae2ac4a0625687e7c8acaf985ccfaa7
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690374"
---
# <a name="manage-package-flight-submissions"></a>Administrar envíos de paquetes de vuelos

La API de envío de Microsoft Store proporciona métodos que puede usar para administrar los envíos de paquetes de vuelos para las aplicaciones, incluidas las implementaciones de paquetes graduales. Para ver una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para el uso de la API, consulte [crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si usa la API de envío de Microsoft Store para crear un envío para un paquete piloto, asegúrese de realizar más cambios en el envío solo mediante la API, en lugar del centro de Partners. Si usa el panel para cambiar un envío que creó originalmente con la API, ya no podrá cambiar o confirmar el envío mediante la API... En algunos casos, el envío podría quedar en un estado de error en el que no se puede continuar en el proceso de envío. Si esto ocurre, debe eliminar el envío y crear un nuevo envío.

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>Métodos para administrar envíos de paquetes de vuelos

Use los métodos siguientes para obtener, crear, actualizar, confirmar o eliminar un envío de paquetes piloto. Antes de poder usar estos métodos, el paquete piloto debe existir en el centro de Partners. Puede crear un paquete piloto [en el centro de Partners](https://docs.microsoft.com/windows/uwp/publish/package-flights) o mediante el Microsoft Store métodos de API de envío descritos en [administrar paquetes piloto](manage-flights.md).

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
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">Obtener un envío de paquete piloto existente</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">Obtener el estado de un envío de paquete piloto existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">Creación de un nuevo envío de paquetes piloto</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">Actualización de un envío de paquete piloto existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">Confirmar un envío de paquete de paquetes nuevo o actualizado</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">Eliminación de un envío de paquetes piloto</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

Para crear un envío para un paquete piloto, siga este proceso.

1. Si todavía no lo ha hecho, complete los requisitos previos descritos en [creación y administración de envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md), incluida la Asociación de una aplicación Azure ad con la cuenta del centro de Partners y la obtención del identificador de cliente y la clave. Solo tiene que hacerlo una vez; una vez que tenga el identificador de cliente y la clave, puede volver a usarlos cada vez que necesite crear un nuevo token de acceso de Azure AD.  

2. [Obtener un token de acceso Azure ad](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Debe pasar este token de acceso a los métodos de la API de envío de Microsoft Store. Después de obtener un token de acceso, tiene 60 minutos para usarlo antes de que expire. Una vez que expire el token, puede obtener uno nuevo.

3. [Cree un envío de paquetes piloto](create-a-flight-submission.md) ejecutando el método siguiente en la API de envío de Microsoft Store. Este método crea un nuevo envío en curso, que es una copia del último envío publicado.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    El cuerpo de la respuesta contiene un recurso de [envío de vuelo](#flight-submission-object) que incluye el identificador del nuevo envío, el URI de la firma de acceso compartido (SAS) para cargar los paquetes para el envío en el almacenamiento de blobs de Azure y los datos para el nuevo envío (incluido todo las listas e información de precios).

    > [!NOTE]
    > Un URI de SAS proporciona acceso a un recurso seguro en Azure Storage sin necesidad de claves de cuenta. Para obtener información general sobre los URI de SAS y su uso con Azure BLOB Storage, consulte [firmas de acceso compartido, parte 1: Descripción del modelo SAS y las](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) [firmas de acceso compartido, parte 2: creación y uso de una SAS con almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si va a agregar nuevos paquetes para el envío, [Prepare los paquetes](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) y agréguelos a un archivo zip.

5. Revise los datos de [envío de vuelos](#flight-submission-object) con los cambios necesarios para el nuevo envío y ejecute el método siguiente para [actualizar el envío de paquetes piloto](update-a-flight-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si va a agregar nuevos paquetes para el envío, asegúrese de actualizar los datos de envío para que hagan referencia al nombre y la ruta de acceso relativa de estos archivos en el archivo ZIP.

4. Si va a agregar nuevos paquetes para el envío, cargue el archivo ZIP en el [almacenamiento de blobs de Azure](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) con el URI de SAS que se proporcionó en el cuerpo de respuesta del método post que llamó anteriormente. Hay diferentes bibliotecas de Azure que puede usar para hacer esto en una gran variedad de plataformas, entre las que se incluyen:

    * [Biblioteca de cliente de Azure Storage para .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [SDK de Azure Storage para Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [SDK de Azure Storage para Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    En el C# ejemplo de código siguiente se muestra cómo cargar un archivo zip en el almacenamiento de blobs de Azure mediante la clase [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) de la biblioteca de cliente de Azure Storage para .net. En este ejemplo se da por supuesto que el archivo ZIP ya se ha escrito en un objeto de secuencia.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. [Confirme el envío del paquete piloto](commit-a-flight-submission.md) ejecutando el método siguiente. Esto le avisará al centro de Partners de que ha terminado con el envío y que las actualizaciones deben aplicarse ahora a su cuenta.

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. Compruebe el estado de confirmación ejecutando el método siguiente para [obtener el estado del envío de paquetes piloto](get-status-for-a-flight-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    Para confirmar el estado de envío, revise el valor de *Estado* en el cuerpo de la respuesta. Este valor debe cambiar de **CommitStarted** a **preprocesamiento** si la solicitud se realiza correctamente o a **CommitFailed** si hay errores en la solicitud. Si hay errores, el campo *statusDetails* contiene más detalles sobre el error.

7. Una vez que la confirmación se ha completado correctamente, el envío se envía al almacén para su ingesta. Puede seguir supervisando el progreso del envío con el método anterior o visitando el centro de Partners.

<span/>

## <a name="code-examples"></a>Ejemplos de código

En los artículos siguientes se proporcionan ejemplos de código detallados que muestran cómo crear un envío de paquetes piloto en varios lenguajes de programación diferentes:

* [C#ejemplos de código](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Módulo de PowerShell de StoreBroker

Como alternativa a llamar a la API de envío de Microsoft Store directamente, también proporcionamos un módulo de PowerShell de código abierto que implementa una interfaz de línea de comandos sobre la API. Este módulo se denomina [StoreBroker](https://aka.ms/storebroker). Puede usar este módulo para administrar los envíos de aplicaciones, vuelos y complementos desde la línea de comandos en lugar de llamar directamente a la API de envío de Microsoft Store, o simplemente puede examinar el origen para ver más ejemplos de cómo llamar a esta API. El módulo StoreBroker se usa activamente en Microsoft como la manera principal en que se envían a la tienda muchas aplicaciones propias.

Para obtener más información, consulte nuestra [Página de StoreBroker en github](https://aka.ms/storebroker).

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>Administrar un lanzamiento de paquetes gradual para un envío de paquetes piloto

Puede implementar gradualmente los paquetes actualizados en un envío de paquetes piloto a un porcentaje de los clientes de la aplicación en Windows 10. Esto le permite supervisar los datos analíticos y los comentarios de los paquetes específicos para asegurarse de que está seguro de la actualización antes de implementarla más ampliamente. Puede cambiar el porcentaje de lanzamiento (o detener la actualización) para un envío publicado sin tener que crear un nuevo envío. Para obtener más información, incluidas las instrucciones para habilitar y administrar una implementación de paquetes gradual en el centro de Partners, consulte [este artículo](../publish/gradual-package-rollout.md).

Para habilitar mediante programación una implementación gradual de paquetes para el envío de paquetes piloto, siga este proceso mediante los métodos de la API de envío de Microsoft Store:

  1. [Cree un envío de paquetes piloto](create-a-flight-submission.md) u [obtenga un envío de paquetes piloto](get-a-flight-submission.md).
  2. En los datos de respuesta, busque el recurso [packageRollout](#package-rollout-object) , establezca el campo *isPackageRollout* en true y establezca el campo *packageRolloutPercentage* en el porcentaje de los clientes de la aplicación que deben obtener los paquetes actualizados.
  3. Pase los datos del envío de paquetes actualizados al método de [envío actualizar paquete piloto](update-a-flight-submission.md) .

Una vez habilitada la implementación gradual de paquetes para el envío de paquetes piloto, puede usar los métodos siguientes para obtener, actualizar, detener o finalizar la implementación gradual mediante programación.

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
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">Obtención de la información de implementación gradual para un envío de paquetes piloto</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">Actualización del porcentaje de lanzamiento gradual para un envío de paquetes piloto</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">Detener la implementación gradual para un envío de paquetes piloto</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">Finalización de la implementación gradual para un envío de paquetes piloto</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>Recursos de datos

Los métodos de la API de envío Microsoft Store para administrar los envíos de paquetes de vuelos usan los siguientes recursos de datos JSON.

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>Recurso de envío de vuelo

Este recurso describe un envío de paquetes piloto.

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
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
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

Este recurso tiene los siguientes valores.

| Valor      | Tipo   | Description              |
|------------|--------|------------------------------|
| id            | cadena  | IDENTIFICADOR del envío.  |
| flightId           | cadena  |  IDENTIFICADOR del paquete piloto al que está asociado el envío.  |  
| status           | cadena  | Estado del envío. Puede ser uno de los siguientes valores: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicación</li><li>Publicado</li><li>PublishFailed</li><li>Preprocesamiento</li><li>PreProcessingFailed</li><li>Certificación</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objeto  |  Un [recurso de detalles de estado](#status-details-object) que contiene detalles adicionales sobre el estado del envío, incluida información sobre los errores.  |
| flightPackages           | array  | Contiene [recursos de paquetes de vuelos](#flight-package-object) que proporcionan detalles acerca de cada paquete en el envío.   |
| packageDeliveryOptions    | objeto  | Un [recurso de opciones de entrega de paquetes](#package-delivery-options-object) que contiene la implementación gradual de paquetes y la configuración de actualización obligatoria para el envío.   |
| fileUploadUrl           | cadena  | El URI de la firma de acceso compartido (SAS) para cargar los paquetes para el envío. Si va a agregar nuevos paquetes para el envío, cargue el archivo ZIP que contiene los paquetes en este URI. Para obtener más información, consulte [crear un envío de paquetes piloto](#create-a-package-flight-submission).  |
| targetPublishMode           | cadena  | El modo de publicación para el envío. Puede ser uno de los siguientes valores: <ul><li>Inmediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | cadena  | Fecha de publicación para el envío en formato ISO 8601, si el valor de *targetPublishMode* se establece en SpecificDate.  |
| notesForCertification           | cadena  |  Proporciona información adicional para los evaluadores de certificación, como las credenciales de la cuenta de prueba y los pasos para obtener acceso y comprobar las características. Para obtener más información, vea [notas para la certificación](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />

### <a name="status-details-resource"></a>Recurso de detalles de estado

Este recurso contiene detalles adicionales sobre el estado de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Description                   |
|-----------------|---------|------|
|  errors               |    objeto     |   Una matriz de [recursos de detalle de estado](#status-detail-object) que contienen los detalles del error para el envío.   |     
|  ADVERTENCIAS               |   objeto      | Una matriz de [recursos de detalle de estado](#status-detail-object) que contienen detalles de advertencia para el envío.     |
|  certificationReports               |     objeto    |   Una matriz de [recursos de informes de certificación](#certification-report-object) que proporcionan acceso a los datos del informe de certificación para el envío. Puede examinar estos informes para obtener más información si se produce un error en la certificación.    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>Recurso de detalles de estado

Este recurso contiene información adicional sobre cualquier error o ADVERTENCIA relacionado para un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Description       |
|-----------------|---------|------|
|  código               |    cadena     |   [Código de estado de envío](#submission-status-code) que describe el tipo de error o advertencia. |  
|  detalles               |     cadena    |  Un mensaje con más detalles sobre el problema.     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>Recurso de informe de certificación

Este recurso proporciona acceso a los datos del informe de certificación para un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Description         |
|-----------------|---------|------|
|     fecha            |    cadena     |  Fecha y hora en que se generó el informe, en formato ISO 8601.    |
|     reportUrl            |    cadena     |  Dirección URL en la que se puede tener acceso al informe.    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>Recurso de paquete de vuelo

Este recurso proporciona detalles sobre un paquete en un envío.

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

Este recurso tiene los siguientes valores.

> [!NOTE]
> Al llamar al método de [envío actualizar un paquete piloto](update-a-flight-submission.md) , solo se requieren los valores *filename*, *fileStatus*, *minimumDirectXVersion*y *minimumSystemRam* de este objeto en el cuerpo de la solicitud. Los demás valores se rellenan con el centro de Partners.

| Valor           | Tipo    | Description              |
|-----------------|---------|------|
| fileName   |   cadena      |  Nombre del paquete.    |  
| fileStatus    | cadena    |  Estado del paquete. Puede ser uno de los siguientes valores: <ul><li>None</li><li>PendingUpload</li><li>Cargado</li><li>PendingDelete</li></ul>    |  
| id    |  cadena   |  IDENTIFICADOR que identifica de forma única el paquete. Este valor lo utiliza el centro de Partners.   |     
| version    |  cadena   |  Versión del paquete de la aplicación. Para obtener más información, vea [numeración](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering)de la versión del paquete.   |   
| arquitectura    |  cadena   |  La arquitectura del paquete de la aplicación (por ejemplo, ARM).   |     
| languages    | array    |  Una matriz de códigos de idioma para los idiomas admitidos por la aplicación. Para obtener más información, consulte [idiomas admitidos](https://docs.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Matriz de funciones que requiere el paquete. Para obtener más información sobre las funcionalidades, consulte declaraciones de funcionalidades de [aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  cadena   |  La versión mínima de DirectX que es compatible con el paquete de la aplicación. Esto solo se puede establecer para aplicaciones destinadas a Windows 8. x; se omite para las aplicaciones destinadas a otras versiones. Puede ser uno de los siguientes valores: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | cadena    |  La memoria RAM mínima que requiere el paquete de la aplicación. Esto solo se puede establecer para aplicaciones destinadas a Windows 8. x; se omite para las aplicaciones destinadas a otras versiones. Puede ser uno de los siguientes valores: <ul><li>None</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>Recurso opciones de entrega de paquetes

Este recurso contiene la implementación gradual de paquetes y la configuración de actualización obligatoria para el envío.

```json
{
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
}
```

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Description        |
|-----------------|---------|------|
| packageRollout   |   objeto      |   Un [recurso de implementación de paquete](#package-rollout-object) que contiene la configuración de lanzamiento de paquetes gradual para el envío.    |  
| isMandatoryUpdate    | boolean    |  Indica si desea tratar los paquetes de este envío como obligatorios para las actualizaciones de aplicaciones que se instalan automáticamente. Para más información sobre los paquetes obligatorios para las actualizaciones de aplicaciones que se instalan automáticamente, consulte [descarga e instalación de actualizaciones de paquetes para la aplicación](../packaging/self-install-package-updates.md).    |  
| mandatoryUpdateEffectiveDate    |  fecha   |  Fecha y hora en que se convierten en obligatorias los paquetes de este envío, en formato ISO 8601 y zona horaria UTC.   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>Recurso de implementación de paquetes

Este recurso contiene la [configuración de lanzamiento](#manage-gradual-package-rollout) gradual de paquetes para el envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Description        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  Indica si la implementación gradual de paquetes está habilitada para el envío.    |  
| packageRolloutPercentage    | float    |  El porcentaje de usuarios que recibirán los paquetes en el lanzamiento gradual.    |  
| packageRolloutStatus    |  cadena   |  Una de las siguientes cadenas que indica el estado del lanzamiento del paquete gradual: <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  cadena   |  El ID. del envío que recibirán los clientes que no obtienen los paquetes de implementación gradual.   |          

> [!NOTE]
> Los valores *packageRolloutStatus* y *fallbackSubmissionId* se asignan por el centro de Partners y no están diseñados para que los establezca el desarrollador. Si incluye estos valores en un cuerpo de solicitud, estos valores se omitirán.

<span/>

## <a name="enums"></a>Enumeraciones

Estos métodos utilizan las siguientes enumeraciones.

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Código de estado del envío

Los siguientes códigos representan el estado de un envío.

| Código           |  Description      |
|-----------------|---------------|
|  None            |     No se especificó ningún código.         |     
|      InvalidArchive        |     El archivo ZIP que contiene el paquete no es válido o tiene un formato de archivo no reconocido.  |
| MissingFiles | El archivo ZIP no contiene todos los archivos que se enumeraron en los datos de envío o están en una ubicación incorrecta en el archivo. |
| PackageValidationFailed | No se pudieron validar uno o más paquetes en el envío. |
| InvalidParameterValue | Uno de los parámetros del cuerpo de la solicitud no es válido. |
| InvalidOperation | La operación que ha intentado no es válida. |
| InvalidState | La operación que ha intentado no es válida para el estado actual del paquete piloto. |
| ResourceNotFound | No se encontró el paquete piloto especificado. |
| ServiceError | Un error de servicio interno impidió que la solicitud se realizara correctamente. Vuelva a intentar la solicitud. |
| ListingOptOutWarning | El desarrollador quitó una lista de un envío anterior o no incluía información de lista que es compatible con el paquete. |
| ListingOptInWarning  | El desarrollador ha agregado una lista. |
| UpdateOnlyWarning | El desarrollador está intentando insertar algo que solo tiene compatibilidad con actualizaciones. |
| Otros  | El envío se encuentra en un estado desconocido o sin clasificar. |
| PackageValidationWarning | El proceso de validación del paquete produjo una advertencia. |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Administración de paquetes piloto mediante el Microsoft Store API de envío](manage-flights.md)
* [Obtener un envío de paquete piloto](get-a-flight-submission.md)
* [Crear un envío de paquete piloto](create-a-flight-submission.md)
* [Actualización de un envío de paquetes piloto](update-a-flight-submission.md)
* [Confirmar el envío de un paquete piloto](commit-a-flight-submission.md)
* [Eliminación de un envío de paquetes piloto](delete-a-flight-submission.md)
* [Obtención del estado de un envío de paquete piloto](get-status-for-a-flight-submission.md)
