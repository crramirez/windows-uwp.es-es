---
author: mcleanbyron
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: "Usa estos métodos en la API de envío de la Tienda Windows para administrar envíos de complemento para las aplicaciones que están registradas en tu cuenta del Centro de desarrollo de Windows."
title: "Administrar envíos de complemento con la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 020c8b3f4d9785842bbe127dd391d92af0962117
ms.openlocfilehash: 1a1ace9d456089d4bed2dd4ac4f39479dc8faa52

---

# <a name="manage-add-on-submissions-using-the-windows-store-submission-api"></a>Administrar envíos de complemento con la API de envío de la Tienda Windows

La API de envío de la Tienda Windows proporciona métodos que puedes usar para administrar envíos de complementos (también llamados productos en aplicación o IAP) para tus aplicaciones. Para obtener una introducción a la API de envío de la Tienda Windows, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md).

>**Nota**&nbsp;&nbsp;Estos métodos solo pueden usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado. Para poder usar estos métodos para crear o administrar envíos para un complemento, el complemento ya debe existir en tu cuenta del Centro de desarrollo. Para crear un complemento, puedes [usar el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) o los métodos de la API de envío de la Tienda Windows que se describen en [Administrar complementos](manage-add-ons.md).

Usa los siguientes métodos para obtener, crear, actualizar, confirmar o eliminar un envío de complemento.

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}```</td>
<td align="left">[Obtener un envío de complemento existente](get-an-add-on-submission.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status```</td>
<td align="left">[Obtener el estado de un envío de complemento existente](get-status-for-an-add-on-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions```</td>
<td align="left">[Crear un nuevo envío de complemento](create-an-add-on-submission.md)</td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}```</td>
<td align="left">[Actualizar un envío de complemento existente](update-an-add-on-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit```</td>
<td align="left">[Confirmar un envío de complemento nuevo o actualizado](commit-an-add-on-submission.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}```</td>
<td align="left">[Eliminar un envío de complemento](delete-an-add-on-submission.md)</td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">
## Crear un envío de complemento

Para crear un envío de un complemento, sigue este proceso.

1. Si no lo has hecho aún, completa los requisitos previos se describen en [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md), incluida la asociación de una aplicación de Azure AD con tu cuenta del Centro de desarrollo de Windows y la obtención del identificador y la clave de cliente. Solo tienes que hacerlo una vez; cuando tengas el identificador y la clave de cliente, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.  

2. [Obtener un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Debes pasar este token de acceso a los métodos de la API de envío de la Tienda Windows. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

3. Ejecuta el siguiente método en la API de envío de la Tienda Windows. Este método crea un nuevo envío en curso, que es una copia de tu último envío publicado. Para obtener más información, consulta [Crear un envío de complemento](create-an-add-on-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
  ```

  El cuerpo de la respuesta contiene tres elementos: el identificador del nuevo envío, los datos del nuevo envío (incluidas todas las listas y la información sobre precios) y el URI de firma de acceso compartido (SAS) para cargar los iconos de complemento para el envío a Azure Blob Storage.

  >**Nota**&nbsp;&nbsp;Un URI de SAS proporciona acceso a un recurso seguro en el almacenamiento de Azure sin necesidad de claves de cuenta. Para obtener información general sobre los URI de SAS y su uso con Azure Blob Storage, consulta [Shared Access Signatures, Part 1: Understanding the SAS model (Firmas de acceso compartido, parte 1: información sobre el modelo SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1) y [Firmas de acceso compartido, Parte 2: Creación y uso de una SAS con Almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si estás agregando nuevos iconos para el envío, [prepara los iconos](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon) y agrégalos a un archivo ZIP.

5. Actualiza los datos de envío con todos los cambios necesarios para el nuevo envío y ejecuta el siguiente método para actualizar el envío. Para obtener más información, consulta [Actualizar un envío de complemento](update-an-add-on-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
  ```

  <span/>
  >**Nota**&nbsp;&nbsp;Si estás agregando nuevos iconos para el envío, asegúrate de actualizar los datos de envío para que hagan referencia al nombre y a la ruta de acceso relativa de estos archivos en el archivo ZIP.

4. Si estás agregando nuevos iconos para el envío, carga el archivo ZIP en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando el URI de SAS que se proporcionó en el cuerpo de la respuesta del método POST que llamaste anteriormente. Hay varias bibliotecas de Azure, que puedes usar para hacer esto en una variedad de plataformas, como:

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

5. Ejecuta el siguiente método para confirmar el envío. Esta acción avisará al Centro de desarrollo que completaste el envío y que las actualizaciones deberían haberse aplicado a tu cuenta. Para obtener más información, consulta [Confirmar un envío de complemento](commit-an-add-on-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
  ```

6. Ejecuta el siguiente método para comprobar el estado de confirmación. Para obtener más información, consulta [Obtener el estado de un envío de complemento](get-status-for-an-add-on-submission.md).

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
  ```

  Para confirmar el estado del envío, revisa el valor de *status* en el cuerpo de la respuesta. Este valor debe cambiar de **CommitStarted** a **PreProcessing** si la solicitud se realiza correctamente o a **CommitFailed** si hay errores en la solicitud. Si hay errores, el campo *statusDetails* contendrá más detalles sobre el error.

7. Después de completarse correctamente la confirmación, el envío se remite a la Tienda para su ingesta. Para continuar con la supervisión del progreso del envío, puedes usar el método anterior o visitar el panel del Centro de desarrollo.

<span/>
### <a name="code-examples"></a>Ejemplos de código

En los siguientes artículos se proporcionan ejemplos de código detallados que muestran cómo crear un envío de complemento en varios lenguajes de programación:

* [Ejemplos de código de C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Python](python-code-examples-for-the-windows-store-submission-api.md)

<span/>
## <a name="data-resources"></a>Recursos de datos

Los métodos de las API de envío de la Tienda Windows para administrar los envíos de complemento usan los siguientes recursos de datos JSON.

<span id="add-on-submission-object" />
### <a name="add-on-submission-resource"></a>Recurso de envío de complemento

Este recurso describe un envío de complemento.

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

Este recurso tiene los siguientes valores.

| Valor      | Tipo   | Descripción        |
|------------|--------|----------------------|
| id            | string  | Identificador del envío.  |
| contentType           | string  |  [Tipo de contenido](../publish/enter-add-on-properties.md#content-type) que se proporciona en el complemento. Puede ser uno de los valores siguientes: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Matriz de cadenas que contienen hasta 10 [palabras clave](../publish/enter-add-on-properties.md#keywords) para el complemento. Gracias a estas palabras clave, la aplicación puede consultar complementos.   |
| lifetime           | string  |  Duración del complemento. Puede ser uno de los valores siguientes: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  |  Diccionario de pares clave-valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es un [recurso de lista](#listing-object) que contiene la información de lista del complemento.  |
| pricing           | object  | Un [recurso de precios](#pricing-object) que contiene información sobre precios para el complemento.   |
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |
| tag           | cadena  |  Los [datos del desarrollador personalizados](../publish/enter-add-on-properties.md#custom-developer-data) para el complemento (esta información se denominaba anteriormente *tag*).   |
| visibility  | cadena  |  Visibilidad del complemento. Puede ser uno de los valores siguientes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | string  |  Estado del envío. Puede ser uno de los valores siguientes: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Un [recurso de detalles de estado](#status-details-object) que contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores. |
| fileUploadUrl           | string  | URI de firma de acceso compartido (SAS) para cargar los paquetes para el envío. Si estás agregando nuevos paquetes para el envío, carga el archivo ZIP que contiene los paquetes en este URI. Para obtener más información, consulta [Crear un envío de complemento](#create-an-add-on-submission).  |
| friendlyName  | string  |  Nombre descriptivo del complemento, que se usa con fines de visualización.  |

<span id="listing-object" />
### <a name="listing-resource"></a>Recurso de lista

Este recurso contiene la información de lista de un complemento. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción       |
|-----------------|---------|------|
|  description               |    string     |   Descripción de la lista de complementos.   |     
|  icon               |   object      |Un [recurso de icono](#icon-object) que contiene los datos para el icono de la lista de complementos.    |
|  title               |     string    |   Título de la lista de complementos.   |  

<span id="icon-object" />
### <a name="icon-resource"></a>Recurso de icono

Este recurso contiene los datos de iconos de una lista de complementos. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción     |
|-----------------|---------|------|
|  fileName               |    string     |   Nombre del archivo de icono en el archivo ZIP que cargaste para el envío.    |     
|  fileStatus               |   string      |  Estado del archivo de icono. Puede ser uno de los valores siguientes: <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />
### <a name="pricing-resource"></a>Recurso de precios

Este recurso contiene información sobre precios del complemento. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción    |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados del complemento en mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices). Los elementos de este diccionario reemplazan el precio base especificado por el valor de *priceId* para el mercado especificado.     |     
|  sales               |   array      |  **En desuso**. Una matriz de [recursos de venta](#sale-object) que contienen información de ventas del complemento.     |     
|  priceId               |   string      |  [Franja de precios](#price-tiers) que especifica el [precio base](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price) del complemento.    |


<span id="sale-object" />
### <a name="sale-resource"></a>Recurso de venta

Este recurso contiene información de venta de un complemento.

>**Importante:**&nbsp;&nbsp;El recurso **Sale** ya no se admite, y actualmente no se puede obtener ni modificar los datos de ventas del envío de un complemento mediante la API de envío de la Tienda Windows:

   > * Después de llamar al [método GET para obtener un envío de complemento](get-an-add-on-submission.md), el valor de *sales* estará vacío. Puedes seguir utilizando el panel del Centro de desarrollo para obtener los datos de ventas referentes al envío del complemento.
   > * Cuando se llama al [método PUT para actualizar un envío de complemento](update-an-add-on-submission.md), la información del valor de *sales* se omite. Puedes seguir utilizando el panel del Centro de desarrollo para modificar los datos de ventas referentes al envío del complemento.

> En el futuro, actualizaremos la API de envío de la Tienda Windows para incorporar una nueva forma de acceder mediante programación a la información de ventas de los envíos de complementos.

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción           |
|-----------------|---------|------|
|  name               |    string     |   Nombre de la venta.    |     
|  basePriceId               |   string      |  [Franja de precios](#price-tiers) que se usará para el precio base de la venta.    |     
|  startDate               |   string      |   Fecha de inicio de la venta en formato ISO 8601.  |     
|  endDate               |   string      |  Fecha de finalización de la venta en formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados del complemento en mercados específicos](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess). Los elementos de este diccionario reemplazan el precio base especificado por el valor *basePriceId* para el mercado especificado.    |

<span id="status-details-object" />
### <a name="status-details-resource"></a>Recurso de detalles de estado

Este recurso contiene detalles adicionales sobre el estado de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción       |
|-----------------|---------|------|
|  errors               |    object     |   Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de errores del envío.   |     
|  warnings               |   object      | Una matriz de [recursos de detalles de estado](#status-detail-object) que contienen los detalles de advertencias del envío.     |
|  certificationReports               |     object    |   Una matriz de [recursos de informe de certificación](#certification-report-object) que proporcionan acceso a los datos del informe de certificación del envío. Si se produce un error en la certificación, puedes examinar estos informes para obtener más información.    |  

<span id="status-detail-object" />
### <a name="status-detail-resource"></a>Recurso de detalle de estado

Este recurso contiene información adicional acerca de las advertencias o los errores relacionados de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción    |
|-----------------|---------|------|
|  code               |    string     |   Un [código de estado de envío](#submission-status-code) que describe el tipo de error o advertencia.   |     
|  details               |     string    |  Mensaje con más detalles sobre el problema.     |

<span id="certification-report-object" />
### <a name="certification-report-resource"></a>Recurso de informe de certificación

Este recurso proporciona acceso a los datos del informe de certificación de un envío. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción               |
|-----------------|---------|------|
|     date            |    string     |  Fecha y hora en que se generó el informe, en formato ISO 8601.    |
|     reportUrl            |    string     |  Dirección URL en la que puedes acceder al informe.    |

## <a name="enums"></a>Enumeraciones

Estos métodos usan las enumeraciones siguientes.

<span id="price-tiers" />
### <a name="price-tiers"></a>Franjas de precios

Los valores siguientes representan las franjas de precios disponibles para un envío de complemento.

| Valor           | Descripción       |
|-----------------|------|
|  Base               |   No se establece la franja de precios; usa el precio base para el complemento.      |     
|  NotAvailable              |   El complemento no está disponible en la región especificada.    |     
|  Free              |   El complemento es gratuito.    |    
|  Tier2 a Tier194               |   Tier2 representa la franja de precios de 0,99 USD. Cada franja adicional representa incrementos adicionales (1,29 USD, 1,49 USD, 1,99 USD, etc.).    |


<span id="submission-status-code" />
### <a name="submission-status-code"></a>Código de estado del envío

Los valores siguientes representan el código de estado de un envío.

| Valor           |  Descripción      |
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
* [Administrar complementos mediante la API de envío de la Tienda Windows](manage-add-ons.md)
* [Envíos de complementos en el panel del Centro de desarrollo](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)



<!--HONumber=Dec16_HO3-->


