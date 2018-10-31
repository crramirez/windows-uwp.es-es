---
author: Xansky
ms.assetid: C78176D6-47BB-4C63-92F8-426719A70F04
description: Usa este método en la API de envío de Microsoft Store para obtener el estado de un envío de paquete piloto.
title: Obtener el estado de un envío de paquete piloto
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, flight submission, envío piloto, status, estado
ms.localizationpriority: medium
ms.openlocfilehash: e3db0b4823b4bc6d8ac50e40026b21f288f9e64c
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5814555"
---
# <a name="get-the-status-of-a-package-flight-submission"></a>Obtener el estado de un envío de paquete piloto

Usa este método en la API de envío de Microsoft Store para obtener el estado de un envío de paquete piloto. Para obtener más información acerca del proceso de creación de un envío de paquete piloto mediante la API de envío de Microsoft Store, consulta [Administrar envíos de paquetes piloto](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un envío de paquete piloto para una aplicación de tu cuenta del Centro de desarrollo. Para hacer esto, puedes usar el panel del Centro de desarrollo o el método [crear un envío de paquete piloto](create-a-flight-submission.md).

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El Id. de la Store de la aplicación que contiene el envío del paquete piloto cuyo estado quieres obtener. Para obtener más información sobre el Id. de la Store, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | cadena | Obligatorio. El Id. del paquete piloto que contiene el envío cuyo estado quieres obtener. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). Para un piloto creado en el panel del Centro de desarrollo, este id. también está disponible en la URL de la página de piloto del panel.  |
| submissionId | cadena | Obligatorio. El identificador del envío para el que quieres obtener el estado. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de paquete piloto](create-a-flight-submission.md). Para un envío creado en el panel del Centro de desarrollo, este id. también está disponible en la URL de la página de envío del panel.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo obtener el estado de un envío de paquete piloto.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de respuesta JSON de una llamada correcta a este método. El cuerpo de la respuesta contiene información sobre el envío especificado. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta la siguiente sección.

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Estado del envío. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicación</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | objeto  |  Contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores. Para obtener más información, consulta [Recurso de detalles de estado](manage-flight-submissions.md#status-details-object). |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío. |
| 409  | La aplicación usa una característica del panel del Centro de desarrollo que [la API de envío de Microsoft Store no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de paquetes piloto](manage-flight-submissions.md)
* [Obtener un envío de aplicación](get-an-app-submission.md)
* [Crear un envío de aplicación](create-an-app-submission.md)
* [Confirmar un envío de aplicación](commit-an-app-submission.md)
* [Actualizar un envío de aplicación](update-an-app-submission.md)
* [Eliminar un envío de aplicación](delete-an-app-submission.md)
