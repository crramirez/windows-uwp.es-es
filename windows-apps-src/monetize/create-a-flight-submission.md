---
author: mcleanbyron
ms.assetid: CD866083-EB7F-4389-A907-FC43DC2FCB5E
description: "Usa este método en la API de envío de la Tienda Windows para crear un nuevo envío de paquetes piloto para una aplicación que esté registrada en tu cuenta del Centro de desarrollo de Windows."
title: "Creación de un envío de paquete piloto mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 33f008c7b2bd32aacaf5a31d0b3201a00d145276

---

# Creación de un envío de paquete piloto mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para crear un nuevo envío de un paquete piloto para una aplicación. Después de crear correctamente un nuevo envío mediante este método, [actualiza el envío](update-a-flight-submission.md) para realizar los cambios necesarios a los datos de envío y luego [confirma el envío](commit-a-flight-submission.md) para la recopilación y la publicación.

Para obtener más información sobre cómo se ajusta este método en el proceso de creación de un envío de paquete piloto mediante la API de envío de la Tienda Windows, consulta [Administración de envíos de paquetes piloto](manage-flight-submissions.md).

>**Nota**&nbsp;&nbsp;Este método crea un envío para un paquete piloto existente. Para crear un paquete piloto, usa el método de [creación de un paquete piloto](create-a-flight.md).

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un paquete piloto para una aplicación de tu cuenta del Centro de desarrollo. Para hacer esto, puedes usar el panel del Centro de desarrollo o el método de [Creación de un paquete piloto](create-a-flight.md).

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del cuerpo del encabezado y la solicitud.

| Method | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Type   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con formato **Token del** &lt;*portador*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Type   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. Id. de la Tienda de la aplicación para la cual quieres crear un envío de paquete piloto. Para obtener más información sobre el Id. de la Tienda, consulta [Visualización de los detalles de identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Obligatorio. La Id. del paquete piloto para el cual quieres añadir el envío. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta para las solicitudes de [creación de un paquete piloto](create-a-flight.md) y [obtención de paquetes piloto para una aplicación](get-flights-for-an-app.md).  |

<span/>

### Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo envío de paquete piloto para una aplicación que tiene la Id. de la Tienda 9WZDNCRD91MD.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta

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
| 400  | No se pudo crear el envío del paquete piloto porque la solicitud no es válida. |
| 409  | No se pudo crear el envío del paquete piloto debido al estado actual de la aplicación o a que esta aplicación usa una característica del panel del Centro de desarrollo que [no admite la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Administración de envíos de paquetes piloto](manage-flight-submissions.md)
* [Obtención de un envío de paquete piloto](get-a-flight-submission.md)
* [Confirmación de un envío de paquete piloto](commit-a-flight-submission.md)
* [Actualización de un envío de paquete piloto](update-a-flight-submission.md)
* [Eliminación de un envío de paquete piloto](delete-a-flight-submission.md)
* [Obtención del estado de un envío de paquete piloto](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


