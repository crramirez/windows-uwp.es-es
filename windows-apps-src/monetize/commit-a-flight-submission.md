---
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: Utilice este método en la API de envío de Microsoft Store para confirmar el envío de vuelo de un paquete nuevo o actualizado al centro de partners.
title: Commit a package flight submission (Confirmar un envío de paquete piloto)
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, API de envío de Microsoft Store, confirmar envío piloto
ms.localizationpriority: medium
ms.openlocfilehash: 820e10695cce2d6242a51b0017d2fe3981cf77b1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603480"
---
# <a name="commit-a-package-flight-submission"></a>Commit a package flight submission (Confirmar un envío de paquete piloto)

Utilice este método en la API de envío de Microsoft Store para confirmar el envío de vuelo de un paquete nuevo o actualizado al centro de partners. El centro de partners alertas de confirmación acción se que se ha cargado los datos de envío (incluidos todos los paquetes relacionados). En respuesta, el centro de partners confirma los cambios a los datos de envío para ingesta y la publicación. Una vez finalizada correctamente la operación de confirmación, los cambios realizados en el envío se muestran en el centro de partners.

Para obtener más información sobre cómo se ajusta la operación de confirmación en el proceso de creación de un envío de paquete piloto mediante la API de envío de Microsoft Store, consulta [Administrar envíos de paquetes piloto](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* [Crea un envío de paquete piloto](create-a-flight-submission.md) y después [actualiza el envío](update-a-flight-submission.md) con cualquier cambio necesario en los datos de envío.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El Id. de la Tienda de la aplicación que contiene el envío de paquete piloto que quieres confirmar. El identificador de Store para la aplicación está disponible en el centro de partners.  |
| flightId | string | Obligatorio. El identificador del paquete piloto que contiene el envío que se va a confirmar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). Para un vuelo en el que se creó en el centro de partners, este identificador también está disponible en la dirección URL de la página de vuelos en el centro de partners.  |
| submissionId | string | Obligatorio. El identificador del envío que se va a confirmar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de paquete piloto](create-a-flight-submission.md). Para un envío que se creó en el centro de partners, este identificador también está disponible en la dirección URL de la página de envío en el centro de partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo confirmar un envío de paquete piloto.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | string  | Estado del envío. Puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Publicación</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | Los parámetros de la solicitud no son válidos. |
| 404  | No se pudo encontrar el envío especificado. |
| 409  | Se encontró el envío especificado, pero no pudo confirmarse en su estado actual o la aplicación usa una característica del centro de partners que está [no compatible actualmente con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos de uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Administrar envíos de vuelos de paquete](manage-flight-submissions.md)
* [Obtener el envío de un vuelo de paquetes](get-a-flight-submission.md)
* [Crear una presentación de vuelos de paquete](create-a-flight-submission.md)
* [Envío de vuelo de un paquete de actualización](update-a-flight-submission.md)
* [Eliminar el envío de un vuelo de paquetes](delete-a-flight-submission.md)
* [Obtener el estado de envío de vuelo de un paquete](get-status-for-a-flight-submission.md)
