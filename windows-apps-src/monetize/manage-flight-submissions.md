---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: "Usa estos métodos en la API de envío de la Tienda Windows para administrar envíos de paquete piloto para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Administrar envíos de paquete piloto mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 41203bffb65de3d4d6cc0fec2c7436e0b9d5821b
ms.openlocfilehash: 9fc42f3abf1d876f271d18c1747ed7462dd20bd6

---

# <a name="manage-package-flight-submissions-using-the-windows-store-submission-api"></a>Administrar envíos de paquete piloto mediante la API de envío de la Tienda Windows

La API de envío de la Tienda Windows proporciona métodos que puedes usar para administrar envíos de paquete piloto para tus aplicaciones, incluidos los lanzamientos de paquetes graduales. Para obtener una introducción a la API de envío de la Tienda Windows, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota**&nbsp;&nbsp;Estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado. Para poder usar estos métodos para crear o administrar envíos para un paquete piloto, el paquete piloto ya debe existir en tu cuenta del Centro de desarrollo. Puedes crear un paquete piloto [mediante el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/package-flights) o mediante los métodos de la API de envío de la Tienda Windows que se describen en [Administrar paquetes piloto](manage-flights.md).

<span id="methods-for-package-flight-submissions" />
## <a name="methods-for-managing-package-flight-submissions"></a>Métodos para administrar envíos de paquetes piloto

Usa los siguientes métodos para obtener, crear, actualizar, confirmar o eliminar un envío de paquete piloto.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}```</td>
<td align="left">[Obtener un envío de paquete piloto existente](get-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status```</td>
<td align="left">[Obtener el estado de un envío de paquete piloto existente](get-status-for-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions```</td>
<td align="left">[Crear un nuevo envío de paquete piloto](create-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}```</td>
<td align="left">[Actualizar un envío de paquete piloto existente](update-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit```</td>
<td align="left">[Confirmar un envío de paquete piloto nuevo o actualizado](commit-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}```</td>
<td align="left">[Eliminar un envío de paquete piloto](delete-a-flight-submission.md)</td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">
### Crear un envío de paquete piloto

Para crear un envío de un paquete piloto, sigue este proceso.

1. Si no lo has hecho aún, completa los requisitos previos se describen en [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md), incluida la asociación de una aplicación de Azure AD con tu cuenta del Centro de desarrollo de Windows y la obtención del identificador y la clave de cliente. Solo tienes que hacerlo una vez; cuando tengas el identificador y la clave de cliente, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.  

2. [Obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Debes pasar este token de acceso a los métodos de la API de envío de la Tienda Windows. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

3. [Crea un envío de paquete piloto](create-a-flight-submission.md) ejecutando el siguiente método en la API de envío de la Tienda Windows. Este método crea un nuevo envío en curso, que es una copia de tu último envío publicado.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
  ```

  El cuerpo de la respuesta contiene tres elementos: el identificador del nuevo envío, los datos del nuevo envío (incluidas todas las listas y la información sobre precios) y el URI de firma de acceso compartido (SAS) para cargar los paquetes para el envío a Azure Blob Storage.

  >**Nota**&nbsp;&nbsp;Un URI de SAS proporciona acceso a un recurso seguro en el almacenamiento de Azure sin necesidad de claves de cuenta. Para obtener información general sobre los URI de SAS y su uso con Azure Blob Storage, consulta [Shared Access Signatures, Part 1: Understanding the SAS model (Firmas de acceso compartido, parte 1: información sobre el modelo SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) y [Firmas de acceso compartido, Parte 2: Creación y uso de una SAS con Almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si estás agregando nuevos paquetes para el envío, [prepara los paquetes](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements) y agrégalos a un archivo ZIP.

5. Revisa los datos de envío con todos los cambios necesarios para el nuevo envío y ejecuta el siguiente método para [actualizar el envío de paquete piloto](update-a-flight-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
  ```

  <span/>
  >**Nota:**&nbsp;&nbsp;Si estás agregando nuevos paquetes para el envío, asegúrate de actualizar los datos de envío para que hagan referencia al nombre y a la ruta de acceso relativa de estos archivos en el archivo ZIP.

4. Si estás agregando nuevos paquetes para el envío, carga el archivo ZIP en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando el URI de SAS que se proporcionó en el cuerpo de la respuesta del método POST que llamaste antes. Hay varias bibliotecas de Azure, que puedes usar para hacer esto en una variedad de plataformas, como:

  * [Biblioteca de cliente de Azure Storage para .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
  * [Azure Storage SDK para Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
  * [Azure Storage SDK para Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

  <span/>

  En el siguiente ejemplo de código C# se muestra cómo cargar el archivo ZIP en Azure Blob Storage usando la clase [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) en la biblioteca de cliente de Azure Storage para. NET. En este ejemplo se supone que el archivo ZIP ya se ha escrito en un objeto de secuencia.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. Ejecuta el siguiente método para [confirmar el envío de paquete piloto](commit-a-flight-submission.md). Esta acción avisará al Centro de desarrollo que completaste el envío y que las actualizaciones deberían haberse aplicado a tu cuenta.

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
  ```

6. Comprueba el estado de confirmación ejecutando el siguiente método para [obtener el estado del envío de paquete piloto](get-status-for-a-flight-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
  ```

  Para confirmar el estado del envío, revisa el valor de *status* en el cuerpo de la respuesta. Este valor debe cambiar de **CommitStarted** a **PreProcessing** si la solicitud se realiza correctamente o a **CommitFailed** si hay errores en la solicitud. Si hay errores, el campo *statusDetails* contendrá más detalles sobre el error.

7. Después de completarse correctamente la confirmación, el envío se remite a la Tienda para su ingesta. Para continuar con la supervisión del progreso del envío, puedes usar el método anterior o visitar el panel del Centro de desarrollo.

<span/>
### <a name="code-examples"></a>Ejemplos de código

Los siguientes artículos proporcionan ejemplos de código detallados que muestran cómo crear un envío de paquete piloto en varios lenguajes de programación:

* [Ejemplos de código de C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Python](python-code-examples-for-the-windows-store-submission-api.md)

<span id="manage-gradual-package-rollout">
## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>Administrar un lanzamiento de paquete gradual para un envío de paquete piloto

Puedes lanzar gradualmente los paquetes actualizados de un envío de paquete piloto para un porcentaje de los clientes de tu aplicación en Windows 10. Esto te permite supervisar los comentarios y los datos analíticos de los paquetes específicos para asegurarte de que estás seguro sobre la actualización antes de hacer un lanzamiento más amplio. Puedes cambiar el porcentaje de lanzamiento (o detener la actualización) para un envío publicado sin tener que crear un nuevo envío. Para obtener más detalles, incluidas instrucciones sobre cómo habilitar y administrar un lanzamiento de paquete gradual en el panel del Centro de desarrollo, consulta [este artículo](../publish/gradual-package-rollout.md).

Para habilitar un lanzamiento de paquete gradual mediante programación para un envío de paquete piloto, sigue este proceso usando los siguientes métodos en la API de envío de la Tienda Windows:

  1. [Crea un envío de paquete piloto](create-a-flight-submission.md) u [obtén un envío de paquete piloto](get-a-flight-submission.md).
  2. En los datos de respuesta, busca el recurso [packageRollout](#package-rollout-object), establece el campo *isPackageRollout* en true y establece el campo *packageRolloutPercentage* en el porcentaje de los clientes de tu aplicación que podrán obtener acceso a los paquetes actualizados.
  3. Pasa los datos de envío de paquete piloto actualizados al método de [actualización de un envío de paquete piloto](update-a-flight-submission.md).

Después de habilitar un lanzamiento de paquete gradual para un envío de paquete piloto, puedes usar los siguientes métodos para obtener, actualizar, detener o finalizar mediante programación el lanzamiento gradual.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout```</td>
<td align="left">[Obtener la información de lanzamiento gradual de un envío de paquete piloto](get-package-rollout-info-for-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage```</td>
<td align="left">[Actualizar el porcentaje de lanzamiento gradual de un envío de paquete piloto](update-the-package-rollout-percentage-for-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout```</td>
<td align="left">[Detener el lanzamiento gradual de un envío de paquete piloto](halt-the-package-rollout-for-a-flight-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout```</td>
<td align="left">[Finalizar el lanzamiento gradual de un envío de paquete piloto](finalize-the-package-rollout-for-a-flight-submission.md)</td>
</tr>
</tbody>
</table>

<span/>
## Recursos de datos

Los métodos de API de envío de la Tienda Windows para administrar envíos de paquetes piloto usan los siguientes recursos de datos JSON.

<span id="flight-submission-object" />
### <a name="flight-submission-resource"></a>Recursos de envío de paquete piloto

Este recurso describe un envío de paquete piloto.

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
        "packageRolloutPercentage": 0,
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

| Valor      | Tipo   | Descripción              |
|------------|--------|------------------------------|
| id            | string  | Identificador del envío.  |
| flightId           | string  |  Identificador del paquete piloto al que está asociado el envío.  |  
| status           | string  | Estado del envío. Puede ser uno de los valores siguientes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Un [recurso de detalles de estado](#status-details-object) que contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores.  |
| flightPackages           | array  | Contiene [recursos de paquete piloto](#flight-package-object) que proporcionan detalles acerca de cada paquete del envío.   |
| packageDeliveryOptions    | object  | Un [recurso de opciones de entrega de paquete](#package-delivery-options-object) que contiene el lanzamiento de paquete gradual y la configuración de actualización obligatoria para el envío.   |
| fileUploadUrl           | string  | URI de firma de acceso compartido (SAS) para cargar los paquetes para el envío. Si estás agregando nuevos paquetes para el envío, carga el archivo ZIP que contiene los paquetes en este URI. Para obtener más información, consulta [Crear un envío de paquete piloto](#create-a-package-flight-submission).  |
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |
| notesForCertification           | string  |  Proporciona información adicional de los evaluadores de certificación como, por ejemplo, las credenciales de la cuenta de prueba y los pasos para obtener acceso y comprobar las características. Para obtener más información, consulta [Notas para la certificación](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span id="status-details-object" />
### <a name="status-details-resource"></a>Recurso de detalles de estado

Este recurso contiene detalles adicionales sobre el estado de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción                   |
|-----------------|---------|------|
|  errors               |    object     |   Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de errores del envío.   |     
|  warnings               |   object      | Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de advertencias del envío.     |
|  certificationReports               |     object    |   Una matriz de [recursos de informe de certificación](#certification-report-object) que proporcionan acceso a los datos del informe de certificación del envío. Si se produce un error en la certificación, puedes examinar estos informes para obtener más información.    |  


<span id="status-detail-object" />
### <a name="status-detail-resource"></a>Recurso de detalle de estado

Este recurso contiene información adicional acerca de las advertencias o los errores relacionados de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción       |
|-----------------|---------|------|
|  code               |    string     |   Un [código de estado de envío](#submission-status-code) que describe el tipo de error o advertencia. |  
|  details               |     string    |  Mensaje con más detalles sobre el problema.     |


<span id="certification-report-object" />
### <a name="certification-report-resource"></a>Recurso de informe de certificación

Este recurso proporciona acceso a los datos del informe de certificación de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción         |
|-----------------|---------|------|
|     date            |    string     |  Fecha y hora en que se generó el informe, en formato ISO 8601.    |
|     reportUrl            |    string     |  Dirección URL en la que puedes obtener acceso al informe.    |


<span id="flight-package-object" />
### <a name="flight-package-resource"></a>Recurso de paquete piloto

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

>**Nota**&nbsp;&nbsp;Al llamar al método para [actualizar un envío de paquete piloto](update-a-flight-submission.md), solo los valores *fileName*, *fileStatus*, *minimumDirectXVersion* y *minimumSystemRam* de este objeto son necesarios en el cuerpo de la solicitud. El Centro de desarrollo se encarga de rellenar el resto de valores.

| Valor           | Tipo    | Descripción              |
|-----------------|---------|------|
| fileName   |   string      |  Nombre del paquete.    |  
| fileStatus    | string    |  Estado del paquete. Puede ser uno de los valores siguientes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  string   |  Id. que identifica de manera exclusiva el paquete. Este valor lo usa el Centro de desarrollo.   |     
| version    |  string   |  Versión del paquete de aplicación. Para obtener más información, consulta [Numeración de la versión del paquete](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering).   |   
| architecture    |  string   |  Arquitectura del paquete de aplicación (por ejemplo, ARM).   |     
| languages    | array    |  Matriz de códigos de idioma para los idiomas que admite la aplicación. Para obtener más información, consulta [Idiomas admitidos](https://msdn.microsoft.com/windows/uwp/publish/supported-languages).    |     
| capabilities    |  array   |  Matriz de funcionalidades necesarias para el paquete. Para obtener más información acerca de las funcionalidades, consulta [Declaraciones de funcionalidades de las aplicaciones](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations).   |     
| minimumDirectXVersion    |  string   |  Versión mínima de DirectX que admite el paquete de aplicación. Solo se puede establecer para las aplicaciones diseñadas para Windows 8.x; se ignora para la aplicaciones diseñadas para otras versiones. Puede ser uno de los valores siguientes: <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | string    |  RAM mínima que requiere el paquete de aplicación. Solo se puede establecer para las aplicaciones diseñadas para Windows 8.x; se omite para las aplicaciones diseñadas para otras versiones. Puede ser uno de los valores siguientes: <ul><li>None</li><li>Memory2GB</li></ul>   |    


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
| packageRollout   |   object      |   Un [recurso de lanzamiento de paquete](#package-rollout-object) que contiene la configuración del lanzamiento de paquete gradual para el envío.    |  
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
| fallbackSubmissionId    |  cadena   |  El identificador del envío que recibirán los clientes que no obtengan los paquetes de lanzamiento gradual.   |          

<span/>

## <a name="enums"></a>Enumeraciones

Estos métodos usan las enumeraciones siguientes.

<span id="submission-status-code" />
### <a name="submission-status-code"></a>Código de estado del envío

Los siguientes códigos de representan el estado de un envío.

| Código           |  Descripción      |
|-----------------|---------------|
|  None            |     No se especificó ningún código.         |     
|      InvalidArchive        |     El archivo ZIP que contiene el paquete no es válido o tiene un formato de archivo no reconocido.  |
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
* [Administrar paquetes piloto mediante la API de envío de la Tienda Windows](manage-flights.md)
* [Obtener un envío de paquete piloto](get-a-flight-submission.md)
* [Crear un envío de paquete piloto](create-a-flight-submission.md)
* [Actualizar un envío de paquete piloto](update-a-flight-submission.md)
* [Confirmar un envío de paquete piloto](commit-a-flight-submission.md)
* [Delete a package flight submission (Eliminar un envío de paquete piloto)](delete-a-flight-submission.md)
* [Get the status of a package flight submission (Obtener el estado de un envío de paquete piloto)](get-status-for-a-flight-submission.md)



<!--HONumber=Dec16_HO3-->


