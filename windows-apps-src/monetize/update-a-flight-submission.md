---
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: Use este método en la API de envío de Microsoft Store para actualizar un envío de paquete piloto existente.
title: Actualización de un envío de paquete piloto
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, envío de vuelos, actualización
ms.localizationpriority: medium
ms.openlocfilehash: d2603319e2a1fc242210f79ef38c2ba5362c9c3e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171339"
---
# <a name="update-a-package-flight-submission"></a>Actualización de un envío de paquete piloto


Use este método en la API de envío de Microsoft Store para actualizar un envío de paquete piloto existente. Después de actualizar correctamente un envío mediante este método, debes [confirmar el envío](commit-a-flight-submission.md) para su ingesta y publicación.

Para obtener más información sobre cómo encaja este método en el proceso de creación de un envío de paquetes piloto mediante el Microsoft Store API de envío, consulte administrar envíos de [paquetes de paquetes](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Cree un envío de paquetes piloto para una de sus aplicaciones. Puede hacerlo en el centro de Partners, o bien puede hacerlo mediante el método [crear un envío de paquetes piloto](create-a-flight-submission.md) .

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. Id. de la Tienda de la aplicación para la cual quieres actualizar un envío de paquete piloto. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |
| flightId | string | Necesario. Id. del paquete piloto para el cual quieres actualizar un envío. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). En el caso de un vuelo creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de vuelo del centro de Partners.  |
| submissionId | string | Necesario. Identificador del envío que se debe actualizar. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un envío de paquetes piloto](create-a-flight-submission.md). En el caso de un envío creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de envío del centro de Partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

| Value      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | array  | Contiene los objetos que proporcionan detalles acerca de cada paquete del envío. Para obtener más información acerca de los valores que se encuentran en el cuerpo de respuesta, consulta [Flight package resource (Recurso del paquete piloto)](manage-flight-submissions.md#flight-package-object). Al llamar a este método para actualizar un envío de aplicación, solo los valores *fileName*, *fileStatus*, *minimumDirectXVersion* y *minimumSystemRam* de esos objetos son necesarios en el cuerpo de la solicitud. Los demás valores se rellenan con el centro de Partners. |
| packageDeliveryOptions    | object  | Contiene el lanzamiento de paquete gradual y la configuración de actualización obligatoria del envío. Para obtener más información, consulta [Package delivery options object](manage-flight-submissions.md#package-delivery-options-object) (Objeto de opciones de entrega de paquete).  |
| targetPublishMode           | string  | Modo de publicación del envío. Puede ser uno de los siguientes valores: <ul><li>Inmediata</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |
| notesForCertification           | string  |  Proporciona información adicional de los evaluadores de certificación como, por ejemplo, las credenciales de la cuenta de prueba y los pasos para obtener acceso y comprobar las características. Para obtener más información, consulta [Notas para la certificación](../publish/notes-for-certification.md). |


### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo actualizar el envío de paquete piloto de una aplicación.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
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
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el envío actualizado. Para obtener más información acerca de los valores que se encuentran en el cuerpo de la respuesta, consulta [Package flight submission resource (Recurso de envío del paquete piloto)](manage-flight-submissions.md#flight-submission-object).

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

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | No se pudo actualizar el envío del paquete piloto porque la solicitud no es válida. |
| 409  | No se pudo actualizar el envío de paquetes piloto debido al estado actual de la aplicación o la aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md)
* [Get a package flight submission (Obtener un envío de paquete piloto)](get-a-flight-submission.md)
* [Crear un envío de paquete piloto](create-a-flight-submission.md)
* [Commit a package flight submission (Confirmar un envío de paquete piloto)](commit-a-flight-submission.md)
* [Eliminar un envío de paquete piloto](delete-a-flight-submission.md)
* [Get the status of a package flight submission (Obtener el estado de un envío de paquete piloto)](get-status-for-a-flight-submission.md)