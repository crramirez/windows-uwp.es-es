---
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Use este método en el Microsoft Store API de envío para crear un nuevo envío de paquetes piloto para una aplicación registrada en la cuenta del centro de Partners.
title: Crear un envío de paquete piloto
ms.date: 08/03/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de envío, crear un envío de vuelos
ms.localizationpriority: medium
ms.openlocfilehash: 5b00439656bfd4bcfaa90d976d28c4c71ab81e27
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175079"
---
# <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

Use este método en el Microsoft Store API de envío para crear un nuevo envío de un paquete piloto para una aplicación. Después de crear correctamente un nuevo envío mediante este método, [actualiza el envío](update-a-flight-submission.md) para realizar los cambios necesarios a los datos de envío y luego [confirma el envío](commit-a-flight-submission.md) para la recopilación y la publicación.

Para obtener más información sobre cómo encaja este método en el proceso de creación de un envío de paquetes piloto mediante el Microsoft Store API de envío, consulte administrar envíos de [paquetes de paquetes](manage-flight-submissions.md).

> [!NOTE]
> Este método crea un envío para un paquete piloto existente. Para crear un paquete piloto, usa el método de [creación de un paquete piloto](create-a-flight.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Cree un paquete piloto para una aplicación. Puede hacerlo en el centro de Partners o puede hacerlo mediante el método [crear un paquete piloto](create-a-flight.md) .

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. Id. de la Tienda de la aplicación para la cual quieres crear un envío de paquete piloto. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |
| flightId | string | Necesario. La Id. del paquete piloto para el cual quieres añadir el envío. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md).  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo envío de paquete piloto para una aplicación que tiene la Id. de la Tienda 9WZDNCRD91MD.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el nuevo envío. Para obtener más información acerca de los valores que se encuentran en el cuerpo de la respuesta, consulta [Package flight submission resource (Recurso de envío del paquete piloto)](manage-flight-submissions.md#flight-submission-object).

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
| 400  | No se pudo crear el envío del paquete piloto porque la solicitud no es válida. |
| 409  | No se pudo crear el envío de paquetes piloto debido al estado actual de la aplicación o la aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md)
* [Get a package flight submission (Obtener un envío de paquete piloto)](get-a-flight-submission.md)
* [Commit a package flight submission (Confirmar un envío de paquete piloto)](commit-a-flight-submission.md)
* [Actualización de un envío de paquete piloto](update-a-flight-submission.md)
* [Eliminar un envío de paquete piloto](delete-a-flight-submission.md)
* [Get the status of a package flight submission (Obtener el estado de un envío de paquete piloto)](get-status-for-a-flight-submission.md)