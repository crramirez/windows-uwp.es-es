---
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: Use estos métodos en la API de envío de Microsoft Store para administrar los envíos de complementos de las aplicaciones que están registradas en su cuenta del centro de Partners.
title: Manage add-on submissions (Administrar envíos de complemento)
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, add-on submissions, envíos de complementos, in-app product, producto desde la aplicación, IAP, IAP
ms.localizationpriority: medium
ms.openlocfilehash: c8204382a4e341083ce825a9424181cdd75771e1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260236"
---
# <a name="manage-add-on-submissions"></a>Manage add-on submissions (Administrar envíos de complemento)

La API de envío de Microsoft Store proporciona métodos que puedes usar para administrar envíos de complementos (también llamados productos en aplicación o IAP) para tus aplicaciones. Para obtener una introducción a la API de envío de Microsoft Store, incluidos los requisitos previos para usar la API, consulta [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

> [!IMPORTANT]
> Si usa la API de envío de Microsoft Store para crear un envío de un complemento, asegúrese de realizar más cambios en el envío solo mediante la API, en lugar de realizar cambios en el centro de Partners. Si usa el centro de partners para cambiar un envío que creó originalmente con la API, ya no podrá cambiar o confirmar el envío mediante la API. En algunos casos, el envío podría quedar en un estado de error en el que no se puede continuar con el proceso de envío. Si esto ocurre, debes eliminar el envío y crear uno nuevo.

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>Métodos para administrar envíos de complementos

Usa los siguientes métodos para obtener, crear, actualizar, confirmar o eliminar un envío de complemento. Para poder usar estos métodos, el complemento ya debe existir en la cuenta del centro de Partners. Puede crear un complemento en el centro de partners mediante la [definición de su tipo de producto e identificador de producto](../publish/set-your-add-on-product-id.md) , o mediante los métodos de API de envío Microsoft Store descritos en [Administrar complementos](manage-add-ons.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">Obtener un envío de complemento existente</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">Obtener el estado de un envío de complemento existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">Crear un nuevo envío de complementos</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">Actualizar un envío de complemento existente</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">Confirmar un envío de complementos nuevo o actualizado</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">Eliminación de un envío de complemento</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>Crear un envío de complemento

Para crear un envío de un complemento, sigue este proceso.

1. Si todavía no lo ha hecho, complete los requisitos previos descritos en [creación y administración de envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md), incluida la Asociación de una aplicación Azure ad con la cuenta del centro de Partners y la obtención del identificador de cliente y la clave. Solo tienes que hacerlo una vez; cuando tengas el identificador y la clave de cliente, puedes volver a usarlos siempre que necesites crear un nuevo token de acceso de Azure AD.  

2. [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token). Debes pasar este token de acceso a los métodos de la API de envío de Microsoft Store. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

3. Ejecuta el siguiente método en la API de envío de Microsoft Store. Este método crea un nuevo envío en curso, que es una copia de tu último envío publicado. Para obtener más información, consulta [Crear un envío de complemento](create-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    El cuerpo de la respuesta contiene un recurso [envío de complemento](#add-on-submission-object) que incluye el identificador del nuevo envío, el URI de firma de acceso compartido (SAS) para cargar los iconos de complemento para el envío a Azure Blob Storage y todos los datos del nuevo envío (como las descripciones y la información sobre precios).

    > [!NOTE]
    > Un URI de SAS proporciona acceso a un recurso seguro en el almacenamiento de Azure sin necesidad de claves de cuenta. Para obtener información general sobre los URI de SAS y su uso con Azure Blob Storage, consulta [Shared Access Signatures, Part 1: Understanding the SAS model (Firmas de acceso compartido, parte 1: información sobre el modelo SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) y [Firmas de acceso compartido, Parte 2: Creación y uso de una SAS con Almacenamiento de blobs](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/).

4. Si estás agregando nuevos iconos para el envío, [prepara los iconos](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions) y agrégalos a un archivo ZIP.

5. Actualiza los [datos de envío](#add-on-submission-object) con todos los cambios necesarios para el nuevo envío y ejecuta el siguiente método para actualizar el envío. Para obtener más información, consulta [Actualizar un envío de complemento](update-an-add-on-submission.md).

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > Si estás agregando nuevos iconos para el envío, asegúrate de actualizar los datos de envío para que hagan referencia al nombre y a la ruta de acceso relativa de estos archivos en el archivo ZIP.

4. Si estás agregando nuevos iconos para el envío, carga el archivo ZIP en [Azure Blob Storage](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage) usando el URI de SAS que se proporcionó en el cuerpo de la respuesta del método POST que llamaste anteriormente. Hay varias bibliotecas de Azure, que puedes usar para hacer esto en una variedad de plataformas, como:

    * [Biblioteca de cliente de Azure Storage para .NET](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [SDK de Azure Storage para Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [SDK de Azure Storage para Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    En el siguiente ejemplo de código C# se muestra cómo cargar el archivo ZIP en Azure Blob Storage usando la clase [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) en la biblioteca de cliente de Azure Storage para. NET. En este ejemplo se supone que el archivo ZIP ya se ha escrito en un objeto de secuencia.

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. Ejecuta el siguiente método para confirmar el envío. Esto le avisará al centro de Partners de que ha terminado con el envío y que las actualizaciones deben aplicarse ahora a su cuenta. Para obtener más información, consulta [Confirmar un envío de complemento](commit-an-add-on-submission.md).

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. Ejecuta el siguiente método para comprobar el estado de confirmación. Para obtener más información, consulta [Obtener el estado de un envío de complemento](get-status-for-an-add-on-submission.md).

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    Para confirmar el estado del envío, revisa el valor de *status* en el cuerpo de la respuesta. Este valor debe cambiar de **CommitStarted** a **PreProcessing** si la solicitud se realiza correctamente o a **CommitFailed** si hay errores en la solicitud. Si hay errores, el campo *statusDetails* contendrá más detalles sobre el error.

7. Después de completarse correctamente la confirmación, el envío se remite a la Tienda para su ingesta. Puede seguir supervisando el progreso del envío con el método anterior o visitando el centro de Partners.

<span/>

## <a name="code-examples"></a>Ejemplos de código

En los siguientes artículos se proporcionan ejemplos de código detallados que muestran cómo crear un envío de complemento en varios lenguajes de programación:

* [C#ejemplos de código](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Ejemplos de código de Python](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>Módulo de StoreBroker PowerShell

Como alternativa a llamar a la API de envío de Microsoft Store directamente, también proporcionamos un módulo de PowerShell de código abierto que implementa una interfaz de línea de comandos en la parte superior de la API. Este módulo se denomina [StoreBroker](https://github.com/Microsoft/StoreBroker). Puedes usar este módulo para administrar tus envíos de aplicaciones, paquetes piloto y complementos desde la línea de comandos en lugar de llamar directamente a la API de envío de Microsoft Store o, simplemente, puedes examinar el origen para ver más ejemplos de cómo llamar a esta API. El módulo StoreBroker se usa activamente dentro de Microsoft como método principal para enviar muchas aplicaciones propias a la Store.

Para obtener más información, consulta nuestra [página de StoreBroker en GitHub](https://github.com/Microsoft/StoreBroker).

<span/>

## <a name="data-resources"></a>Recursos de datos

Los métodos de las API de envío de Microsoft Store para administrar los envíos de complemento usan los siguientes recursos de datos JSON.

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

Este recurso tiene los siguientes valores.

| Valor      | Tipo   | Descripción        |
|------------|--------|----------------------|
| id            | string  | Identificador del envío. Este identificador está disponible en los datos de respuesta de las solicitudes para [crear un envío de complemento](create-an-add-on-submission.md), [obtener todos los complementos](get-all-add-ons.md) y [obtener un complemento](get-an-add-on.md). En el caso de un envío creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de envío del centro de Partners.  |
| contentType           | string  |  [Tipo de contenido](../publish/enter-add-on-properties.md#content-type) que se proporciona en el complemento. Puede ser uno de los valores siguientes: <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | Matriz de cadenas que contienen hasta 10 [palabras clave](../publish/enter-add-on-properties.md#keywords) para el complemento. Gracias a estas palabras clave, la aplicación puede consultar complementos.   |
| lifetime           | string  |  Duración del complemento. Puede ser uno de los valores siguientes: <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | object  |  Diccionario de pares clave-valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es un [recurso de lista](#listing-object) que contiene la información de lista del complemento.  |
| pricing           | object  | Un [recurso de precios](#pricing-object) que contiene información sobre precios para el complemento.   |
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |
| tag           | string  |  Los [datos del desarrollador personalizados](../publish/enter-add-on-properties.md#custom-developer-data) para el complemento (esta información se denominaba anteriormente *tag*).   |
| visibility  | string  |  Visibilidad del complemento. Puede ser uno de los valores siguientes: <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | string  |  Estado del envío. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Publicación</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Un [recurso de detalles de estado](#status-details-object) que contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores. |
| fileUploadUrl           | string  | URI de firma de acceso compartido (SAS) para cargar los paquetes para el envío. Si estás agregando nuevos paquetes para el envío, carga el archivo ZIP que contiene los paquetes en este URI. Para obtener más información, consulta [Crear un envío de complemento](#create-an-add-on-submission).  |
| friendlyName  | string  |  Nombre descriptivo del envío, tal como se muestra en el centro de Partners. Generas este valor cuando creas el envío.  |

<span id="listing-object" />

### <a name="listing-resource"></a>Recurso de lista

Este recurso contiene la [información de lista de un complemento](../publish/create-add-on-store-listings.md). Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción       |
|-----------------|---------|------|
|  description               |    string     |   Descripción de la lista de complementos.   |     
|  icono               |   object      |Un [recurso de icono](#icon-object) que contiene los datos para el icono de la lista de complementos.    |
|  title               |     string    |   Título de la lista de complementos.   |  

<span id="icon-object" />

### <a name="icon-resource"></a>Recurso de icono

Este recurso contiene los datos de iconos de una lista de complementos. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción     |
|-----------------|---------|------|
|  fileName               |    string     |   Nombre del archivo de icono en el archivo ZIP que cargaste para el envío. El icono debe ser un archivo .png que mida exactamente 300 x 300 píxeles.   |     
|  fileStatus               |   string      |  Estado del archivo de icono. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>Recurso de precios

Este recurso contiene información sobre precios del complemento. Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción    |
|-----------------|---------|------|
|  marketSpecificPricings               |    object     |  Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados del complemento en mercados específicos](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability). Los elementos de este diccionario reemplazan el precio base especificado por el valor de *priceId* para el mercado especificado.     |     
|  sales               |   array      |  **En desuso**. Una matriz de [recursos de venta](#sale-object) que contienen información de ventas del complemento.     |     
|  priceId               |   string      |  [Franja de precios](#price-tiers) que especifica el [precio base](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability) del complemento.    |    
|  isAdvancedPricingModel               |   boolean      |  Si está en **true**, tu cuenta de desarrollador tiene acceso al conjunto expandido de franjas de precios de 0,99 USD a 1999,99 USD. Si está en **false**, tu cuenta de desarrollador tiene acceso al conjunto original de franjas de precios de 0,99 USD a 999,99 USD. Para obtener más información sobre las diferentes franjas de precios, consulta [franjas de precios](#price-tiers).<br/><br/>**Nota**&nbsp;&nbsp;este campo es de solo lectura.   |


<span id="sale-object" />

### <a name="sale-resource"></a>Recurso de venta

Este recurso contiene información de venta de un complemento.

> [!IMPORTANT]
> El recurso **Sale** ya no se admite y actualmente no se pueden obtener ni modificar los datos de ventas del envío de un complemento mediante la API de envío de Microsoft Store. En el futuro, actualizaremos la API de envío de Microsoft Store para incorporar una nueva forma de acceder mediante programación a la información de ventas de los envíos de complementos.
>    * Después de llamar al [método GET para obtener un envío de complemento](get-an-add-on-submission.md), el valor de *sales* estará vacío. Puede seguir usando el centro de partners para obtener los datos de venta para el envío del complemento.
>    * Cuando se llama al [método PUT para actualizar un envío de complemento](update-an-add-on-submission.md), la información del valor de *sales* se omite. Puede seguir usando el centro de partners para cambiar los datos de venta del envío del complemento.

Este recurso tiene los siguientes valores.

| Valor           | Tipo    | Descripción           |
|-----------------|---------|------|
|  name               |    string     |   Nombre de la venta.    |     
|  basePriceId               |   string      |  [Franja de precios](#price-tiers) que se usará para el precio base de la venta.    |     
|  startDate               |   string      |   Fecha de inicio de la venta en formato ISO 8601.  |     
|  endDate               |   string      |  Fecha de finalización de la venta en formato ISO 8601.      |     
|  marketSpecificPricings               |   object      |   Diccionario de pares de clave y valor, donde cada clave es un código de país de dos letras ISO 3166-1 alpha-2 y cada valor es una [franja de precios](#price-tiers). Estos elementos representan los [precios personalizados del complemento en mercados específicos](https://docs.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability). Los elementos de este diccionario reemplazan el precio base especificado por el valor *basePriceId* para el mercado especificado.    |

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
|     fecha            |    string     |  Fecha y hora en que se generó el informe, en formato ISO 8601.    |
|     reportUrl            |    string     |  Dirección URL en la que puedes obtener acceso al informe.    |

## <a name="enums"></a>Enumeraciones

Estos métodos usan las enumeraciones siguientes.

<span id="price-tiers" />

### <a name="price-tiers"></a>Franjas de precios

Los siguientes valores representan las franjas de precios disponibles en el [recurso de precios](#pricing-object) de un envío de complemento.

| Valor           | Descripción       |
|-----------------|------|
|  Base               |   No se establece la franja de precios; usa el precio base para el complemento.      |     
|  NotAvailable              |   El complemento no está disponible en la región especificada.    |     
|  Free              |   El complemento es gratuito.    |    
|  Tier*xxxx*               |   Una cadena que especifica la franja de precios del complemento, con formato **Tier<em>xxxx</em>** . Actualmente, se admiten los siguientes intervalos de franjas de precios:<br/><br/><ul><li>Si el valor *isAdvancedPricingModel* del [recurso de precios](#pricing-object) es **true**, los valores disponibles de la franja de precios para tu cuenta son **Tier1012** - **Tier1424**.</li><li>Si el valor *isAdvancedPricingModel* del [recurso de precios](#pricing-object) es **false**, los valores disponibles de la franja de precios para tu cuenta son **Tier2** - **Tier96**.</li></ul>Para ver la tabla completa de los planes de tarifa que están disponibles para su cuenta de desarrollador, incluidos los precios específicos del mercado que están asociados a cada nivel, vaya a la página de **precios y disponibilidad** para cualquiera de los envíos de la aplicación **en el centro** de Partners y haga clic en el vínculo **ver tabla** en la sección **mercados y precios personalizados** (para algunas cuentas de desarrollador, este vínculo está en     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>Código de estado del envío

Los valores siguientes representan el código de estado de un envío.

| Valor           |  Descripción      |
|-----------------|---------------|
|  Ninguno            |     No se especificó ningún código.         |     
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
| Otras  | El envío presenta un estado no reconocido o sin clasificar. |
| PackageValidationWarning | El proceso de validación del paquete genera una advertencia. |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Administración de complementos mediante la API de envío de Microsoft Store](manage-add-ons.md)
* [Envíos de complementos en el centro de Partners](https://docs.microsoft.com/windows/uwp/publish/iap-submissions)
