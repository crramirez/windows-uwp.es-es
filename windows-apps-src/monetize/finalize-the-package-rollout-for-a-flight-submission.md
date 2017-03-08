---
author: mcleanbyron
description: "Usa este método en la API de envío de la Tienda Windows para finalizar el lanzamiento de paquete en el envío de un paquete piloto."
title: "Finalizar el lanzamiento de paquete de un envío de paquete piloto mediante la API de envío de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envío de la Tienda Windows, Windows Store submission API, lanzamiento de paquete, package rollout, envío de paquete piloto, flight submission, finalizar, finalize"
ms.assetid: e4a645f6-1f00-4af5-80d6-d2ee179acc8a
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 163ca2144f460a3ff992a19c366b3cf6d9654978
ms.lasthandoff: 02/08/2017

---

# <a name="finalize-the-package-rollout-for-a-package-flight-submission-using-the-windows-store-submission-api"></a>Finalizar el lanzamiento de paquete de un envío de paquete piloto mediante la API de envío de la Tienda Windows


Usa este método en la API de envío de la Tienda Windows para [finalizar el lanzamiento de un paquete](../publish/gradual-package-rollout.md#completing-the-rollout) en el envío de un paquete piloto. Para obtener más información sobre el proceso de creación de un envío de paquete piloto mediante la API de envío de la Tienda Windows, consulta [Administrar envíos de paquetes piloto](manage-flight-submissions.md).


## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un envío para una aplicación de tu cuenta del Centro de desarrollo. Puedes hacer esto en el panel del Centro de desarrollo o con el método de [creación de un envío de aplicación](create-an-app-submission.md).
* Habilita un lanzamiento de paquete gradual para el envío. Puedes hacerlo en el [panel del Centro de desarrollo](../publish/gradual-package-rollout.md) o mediante el [uso de la API de envío de la Tienda Windows](manage-flight-submissions.md#manage-gradual-package-rollout).

>**Nota**&nbsp;&nbsp;Este método solo puede usarse en cuentas del Centro de desarrollo de Windows que estén autorizadas para usar la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El Id. de la Tienda de la aplicación que contiene el envío del paquete piloto con el lanzamiento de paquetes que quieres finalizar. Para obtener más información sobre el Id. de la Tienda, consulta [Ver los detalles de identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Obligatorio. El identificador del paquete piloto que contiene el envío con el lanzamiento de paquetes que quieres finalizar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta para las solicitudes de [creación de un paquete piloto](create-a-flight.md) y [obtención de paquetes piloto de una aplicación](get-flights-for-an-app.md).  |
| submissionId | cadena | Obligatorio. El identificador del envío con el lanzamiento de paquetes que quieres finalizar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta para las solicitudes de [creación de un envío de paquete piloto](create-a-flight-submission.md).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo finalizar el lanzamiento de paquete para el envío del paquete piloto.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/finalizepackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON a una llamada satisfactoria a este método. Para obtener más información sobre los valores que se encuentran en el cuerpo de respuesta, consulta el recurso [Package rollout object](manage-flight-submissions.md#package-rollout-object) (Objeto de lanzamiento de paquete).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 100,
    "packageRolloutStatus": "PackageRolloutComplete",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío de paquete piloto. |
| 409  | Este código indica uno de los siguientes errores:<br/><br/><ul><li>El envío no está en un estado válido para la operación de lanzamiento gradual (antes de llamar a este método, se debe publicar el envío y se debe establecer el valor [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) en **PackageRolloutInProgress**).</li><li>El envío no pertenece a la aplicación especificada.</li><li>La aplicación usa una característica del panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   

<span/>


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administrar envíos de paquete piloto mediante la API de envío de la Tienda Windows](manage-flight-submissions.md)
* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)

