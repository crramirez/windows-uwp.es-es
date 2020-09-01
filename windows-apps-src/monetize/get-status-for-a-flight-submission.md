---
ms.assetid: C78176D6-47BB-4C63-92F8-426719A70F04
description: Use este método en la API de envío de Microsoft Store para obtener el estado de un envío de paquetes piloto.
title: Get the status of a package flight submission (Obtener el estado de un envío de paquete piloto)
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, envío de vuelos, estado
ms.localizationpriority: medium
ms.openlocfilehash: 28eb08ff5f1dd6a5c7887c100edfe2f38780f6fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175019"
---
# <a name="get-the-status-of-a-package-flight-submission"></a>Get the status of a package flight submission (Obtener el estado de un envío de paquete piloto)

Use este método en la API de envío de Microsoft Store para obtener el estado de un envío de paquetes piloto. Para obtener más información sobre el proceso de creación de un envío de paquetes piloto mediante el Microsoft Store API de envío, vea Administrar envíos de [paquetes de paquetes](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Cree un envío de paquetes piloto para una de sus aplicaciones. Puede hacerlo en el centro de Partners, o bien puede hacerlo mediante el método [crear un envío de paquetes piloto](create-a-flight-submission.md) .

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. El Id. de la Tienda de la aplicación que contiene el envío del paquete piloto cuyo estado quieres obtener. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |
| flightId | string | Necesario. El Id. del paquete piloto que contiene el envío cuyo estado quieres obtener. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). En el caso de un vuelo creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de vuelo del centro de Partners.  |
| submissionId | string | Necesario. El identificador del envío para el que quieres obtener el estado. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un envío de paquetes piloto](create-a-flight-submission.md). En el caso de un envío creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de envío del centro de Partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo obtener el estado de un envío de paquete piloto.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. El cuerpo de la respuesta contiene información sobre el envío especificado. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta la siguiente sección.

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

| Value      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Estado del envío. Puede ser uno de los siguientes valores: <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publicación</li><li>Publicado</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certificación</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | object  |  Contiene detalles adicionales sobre el estado del envío, incluida la información sobre los errores. Para obtener más información, consulta [Recurso de detalles de estado](manage-flight-submissions.md#status-details-object). |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío. |
| 409  | La aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md)
* [Get an app submission (Obtener un envío de aplicación)](get-an-app-submission.md)
* [Crear un envío de aplicación](create-an-app-submission.md)
* [Confirmar el envío de aplicación](commit-an-app-submission.md)
* [Actualización de un envío de aplicación](update-an-app-submission.md)
* [Eliminar un envío de aplicación](delete-an-app-submission.md)