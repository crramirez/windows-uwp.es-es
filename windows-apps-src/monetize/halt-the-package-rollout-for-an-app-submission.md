---
description: Use este método en la API de envío de Microsoft Store para detener el lanzamiento de paquetes para un envío de aplicación.
title: Detener el lanzamiento del envío de una aplicación
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, implementación de paquetes, envío de aplicaciones, detener
ms.assetid: 4ce79fe3-deda-4d31-b938-d672c3869051
ms.localizationpriority: medium
ms.openlocfilehash: ba6c32bcf61f16c5b734ad58c591a9575a3440a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158549"
---
# <a name="halt-the-rollout-for-an-app-submission"></a>Detener el lanzamiento del envío de una aplicación


Use este método en la API de envío de Microsoft Store para [detener el lanzamiento de paquetes](../publish/gradual-package-rollout.md#completing-the-rollout) para un envío de aplicación. Para obtener más información sobre el proceso de creación de un envío de aplicación mediante el Microsoft Store API de envío, consulte Administración de envíos de [aplicaciones](manage-app-submissions.md).

> [!NOTE]
> Si detiene el lanzamiento de un envío de aplicación y, a continuación, [crea un nuevo envío de aplicación](create-an-app-submission.md), el nuevo envío es un clon del envío detenido.


## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Cree un envío para una de sus aplicaciones. Puede hacerlo en el centro de Partners o puede hacerlo mediante el método [crear un envío de aplicación](create-an-app-submission.md) .
* Habilita un lanzamiento de paquete gradual para el envío. Puede hacerlo en el [centro de Partners](../publish/gradual-package-rollout.md)o puede hacerlo mediante [el Microsoft Store API de envío](manage-app-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. El identificador de la Tienda de la aplicación que contiene el envío con el lanzamiento de paquete que quieres detener. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |
| submissionId | string | Necesario. El identificador del envío con el lanzamiento de paquete que quieres detener. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un envío de aplicación](create-an-app-submission.md). En el caso de un envío creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de envío del centro de Partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo detener el lanzamiento de paquete en el envío de una aplicación.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información sobre los valores que se encuentran en el cuerpo de respuesta, consulta el recurso [Package rollout object](manage-app-submissions.md#package-rollout-object) (Objeto de lanzamiento de paquete).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío. |
| 409  | Este código indica uno de los siguientes errores:<br/><br/><ul><li>El envío no está en un estado válido para la operación de lanzamiento gradual (antes de llamar a este método, se debe publicar el envío y se debe establecer el valor [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) en **PackageRolloutInProgress**).</li><li>El envío no pertenece a la aplicación especificada.</li><li>La aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administración de envíos de aplicaciones mediante la API de envío de Microsoft Store](manage-app-submissions.md)
* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)