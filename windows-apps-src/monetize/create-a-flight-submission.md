---
author: Xansky
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: Usa este método en la API de envío de Microsoft Store para crear un nuevo envío de paquete piloto para una aplicación que está registrada en tu cuenta del centro de partners.
title: Crear un envío de paquete piloto
ms.author: mhopkins
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, create flight submission, crear envío piloto
ms.localizationpriority: medium
ms.openlocfilehash: 4cdcc0f06820600523be111d67d3cad5e38b6ceb
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6651541"
---
# <a name="create-a-package-flight-submission"></a>Crear un envío de paquete piloto

Usa este método en la API de envío de Microsoft Store para crear un nuevo envío de paquete piloto para una aplicación. Después de crear correctamente un nuevo envío mediante este método, [actualiza el envío](update-a-flight-submission.md) para realizar los cambios necesarios a los datos de envío y luego [confirma el envío](commit-a-flight-submission.md) para la recopilación y la publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación de un envío de paquete piloto mediante la API de envío de Microsoft Store, consulta [Administrar envíos de paquetes piloto](manage-flight-submissions.md).

> [!NOTE]
> Este método crea un envío para un paquete piloto existente. Para crear un paquete piloto, usa el método de [creación de un paquete piloto](create-a-flight.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Crear un paquete piloto para una aplicación. Puedes hacerlo en el centro de partners, o puedes hacerlo mediante el método para [crear un paquete piloto](create-a-flight.md) .

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. Id. de la Store de la aplicación para la cual quieres crear un envío de paquete piloto. Para obtener más información sobre el Id. de la Store, consulta [Visualización de los detalles de identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | cadena | Obligatorio. El id. del paquete piloto para el cual quieres añadir el envío. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md).  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo envío de paquete piloto para una aplicación que tiene la Id. de la Store 9WZDNCRD91MD.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el nuevo envío. Para obtener más información acerca de los valores que se encuentran en el cuerpo de la respuesta, consulta [Recurso de envío del paquete piloto](manage-flight-submissions.md#flight-submission-object).

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
| 409  | No se pudo crear el envío del paquete piloto debido al estado actual de la aplicación o la aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de paquetes piloto](manage-flight-submissions.md)
* [Obtener un envío de paquete piloto](get-a-flight-submission.md)
* [Confirmación de un envío de paquete piloto](commit-a-flight-submission.md)
* [Actualizar un envío de paquete piloto](update-a-flight-submission.md)
* [Eliminación de un envío de paquete piloto](delete-a-flight-submission.md)
* [Get the status of a package flight submission (Obtener el estado de un envío de paquete piloto)](get-status-for-a-flight-submission.md)
