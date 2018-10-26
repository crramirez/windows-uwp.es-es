---
author: Xansky
description: Usa este método en la API de envío de Microsoft Store para finalizar el lanzamiento de paquete en un envío de aplicación.
title: Finalizar el lanzamiento de un envío de aplicación
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, package rollout, lanzamiento de paquete, app submission, envío de aplicación, finalize, finalizar
ms.assetid: c7dd39e6-5162-455a-b03b-1ed76bffcf6e
ms.localizationpriority: medium
ms.openlocfilehash: 462fba2cea3a9d3056ae63eec491025d613f57f4
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5593199"
---
# <a name="finalize-the-rollout-for-an-app-submission"></a>Finalizar el lanzamiento de un envío de aplicación


Usa este método en la API de envío de Microsoft Store para [finalizar el lanzamiento de paquete](../publish/gradual-package-rollout.md#completing-the-rollout) en un envío de aplicación. Para obtener más información sobre el proceso de creación de un envío de aplicación mediante la API de envío de Microsoft Store, consulta [Administrar envíos de aplicación](manage-app-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un envío para una aplicación de tu cuenta del Centro de desarrollo. Puedes hacer esto en el panel del Centro de desarrollo o con el método de [creación de un envío de aplicación](create-an-app-submission.md).
* Habilita un lanzamiento de paquete gradual para el envío. Puedes hacerlo en el [panel del Centro de desarrollo](../publish/gradual-package-rollout.md) o mediante el [uso de la API de envío de Microsoft Store](manage-app-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El Id. de la Store de la aplicación que contiene el envío con el lanzamiento de paquetes que quieres finalizar. Para obtener más información sobre el Id. de la Store, consulta [Ver los detalles de identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | cadena | Obligatorio. El identificador del envío con el lanzamiento de paquetes que quieres finalizar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de aplicación](create-an-app-submission.md). Para un envío creado en el panel del Centro de desarrollo, este id. también está disponible en la URL de la página de envío del panel.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo finalizar el lanzamiento de paquete para el envío del paquete piloto.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/finalizepackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información sobre los valores que se encuentran en el cuerpo de respuesta, consulta el recurso [Package rollout object](manage-app-submissions.md#package-rollout-object) (Objeto de lanzamiento de paquete).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 100.0,
    "packageRolloutStatus": "PackageRolloutComplete",
    "fallbackSubmissionId": "1212922684621243058"
}
```


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío. |
| 409  | Este código indica uno de los siguientes errores:<br/><br/><ul><li>El envío no está en un estado válido para la operación de lanzamiento gradual (antes de llamar a este método, se debe publicar el envío y se debe establecer el valor [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) en **PackageRolloutInProgress**).</li><li>El envío no pertenece a la aplicación especificada.</li><li>La aplicación usa una característica del panel del Centro de desarrollo que [la API de envío de Microsoft Store no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administrar envíos de aplicaciones con la API de envío de Microsoft Store](manage-app-submissions.md)
* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
