---
author: mcleanbyron
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: "Usa este método en la API de envío de la Tienda Windows para actualizar un envío ya existente de un paquete piloto."
title: "Actualizar un envío de paquete piloto mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 9dfad9c0cc6b6e03a2196946ac578ca3170fccc5

---

# Actualizar un envío de paquete piloto mediante la API de envío de la Tienda Windows


Usa este método en la API de envío de la Tienda Windows para actualizar un envío ya existente de un paquete piloto. Después de actualizar correctamente un envío mediante este método, debes [confirmar el envío](commit-a-flight-submission.md) para su ingesta y publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación de un envío de paquete piloto mediante la API de envío de la Tienda Windows, consulta [Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md).

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un envío de paquete piloto para una aplicación de tu cuenta del Centro de desarrollo. Para hacer esto, puedes usar el panel del Centro de desarrollo o el método [crear un envío de paquete piloto](create-a-flight-submission.md).

>**Nota**&nbsp;&nbsp;Este método solo puede usarse en cuentas del Centro de desarrollo de Windows que estén autorizadas para usar la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. Id. de la Tienda de la aplicación para la cual quieres actualizar un envío de paquete piloto. Para obtener más información sobre la Id. de la Tienda, consulta [View app identity details (Ver los detalles de identidad de la aplicación)](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Obligatorio. Id. del paquete piloto para el cual quieres actualizar un envío. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un paquete piloto](create-a-flight.md) y [obtención de paquetes piloto de una aplicación](get-flights-for-an-app.md).  |
| submissionId | string | Obligatorio. Identificador del envío que se debe actualizar. Esta identificación está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta de las solicitudes de [creación de un envío de paquete piloto](create-a-flight-submission.md).  |

<span/>

### Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | array  | Contiene los objetos que proporcionan detalles acerca de cada paquete del envío. Para obtener más información acerca de los valores que se encuentran en el cuerpo de respuesta, consulta [Flight package resource (Recurso del paquete piloto)](manage-flight-submissions.md#flight-package-object). Al llamar a este método para actualizar un envío de aplicación, solo los valores *fileName*, *fileStatus*, *minimumDirectXVersion* y *minimumSystemRam* de esos objetos son necesarios en el cuerpo de la solicitud. El Centro de desarrollo se encarga de rellenar el resto de valores. |
| targetPublishMode           | string  | Modo de publicación del envío. Esta puede ser uno de los valores siguientes: <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | string  | Fecha de publicación del envío en formato ISO 8601, si el valor *targetPublishMode* se establece en SpecificDate.  |
| notesForCertification           | string  |  Proporciona información adicional de los evaluadores de certificación como, por ejemplo, las credenciales de la cuenta de prueba y los pasos para obtener acceso y comprobar las características. Para obtener más información, consulta [Notes for certification (Notas de certificación)](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification). |

<span/>

### Ejemplo de solicitud

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
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Respuesta

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
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | No se pudo actualizar el envío del paquete piloto porque la solicitud no es válida. |
| 409  | No se pudo actualizar el envío del paquete piloto debido al estado actual de la aplicación o a que esta aplicación usa una característica del panel del Centro de desarrollo que [no admite la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Temas relacionados

* [Create and manage submissions using Windows Store services (Crear y administrar envíos mediante el uso de servicios de la Tienda Windows)](create-and-manage-submissions-using-windows-store-services.md)
* [Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md)
* [Get a package flight submission (Obtener un envío de paquete piloto)](get-a-flight-submission.md)
* [Create a package flight submission (Crear un envío de paquete piloto)](create-a-flight-submission.md)
* [Commit a package flight submission (Confirmar un envío de paquete piloto)](commit-a-flight-submission.md)
* [Delete a package flight submission (Eliminar un envío de paquete piloto)](delete-a-flight-submission.md)
* [Get the status of a package flight submission (Obtener el estado de un envío de paquete piloto)](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


