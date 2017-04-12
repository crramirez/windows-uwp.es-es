---
author: mcleanbyron
description: "Usa este método en la API de envío de la Tienda Windows para detener el lanzamiento de paquete para un envío de aplicación."
title: "Detener el lanzamiento de un envío de aplicación"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store submission API, API de envío de la Tienda Windows, package rollout, lanzamiento de paquete, app submission, envío de aplicación, halt, detener"
ms.assetid: 4ce79fe3-deda-4d31-b938-d672c3869051
ms.openlocfilehash: 7bc86db250f27f0785bf505a15975bcf65cb5eff
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="halt-the-rollout-for-an-app-submission"></a>Detener el lanzamiento de un envío de aplicación


Usa este método en la API de envío de la Tienda Windows para [detener el lanzamiento de paquete](../publish/gradual-package-rollout.md#completing-the-rollout) para un envío de aplicación. Para obtener más información sobre el proceso de creación de un envío de aplicación mediante la API de envío de la Tienda Windows, consulta [Administrar envíos de aplicación](manage-app-submissions.md).

>**Nota**&nbsp;&nbsp;Si se detiene el lanzamiento de un envío de aplicación y, después, se [crea un nuevo envío de aplicación](create-an-app-submission.md), el nuevo envío es un clon del envío detenido.


## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un envío para una aplicación de tu cuenta del Centro de desarrollo. Puedes hacer esto en el panel del Centro de desarrollo o con el método de [creación de un envío de aplicación](create-an-app-submission.md).
* Habilita un lanzamiento de paquete gradual para el envío. Puedes hacerlo en el [panel del Centro de desarrollo](../publish/gradual-package-rollout.md) o mediante el [uso de la API de envío de la Tienda Windows](manage-app-submissions.md#manage-gradual-package-rollout).

>**Nota**&nbsp;&nbsp;Este método solo puede usarse en cuentas del Centro de desarrollo de Windows que estén autorizadas para usar la API de envío de la TiendaWindows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El identificador de la Tienda de la aplicación que contiene el envío con el lanzamiento de paquete que quieres detener. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | cadena | Obligatorio. El identificador del envío con el lanzamiento de paquete que quieres detener. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta de las solicitudes de [creación de un envío de aplicaciones](create-an-app-submission.md).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo detener el lanzamiento de paquete en el envío de una aplicación.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

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
| 409  | Este código indica uno de los siguientes errores:<br/><br/><ul><li>El envío no está en un estado válido para la operación de lanzamiento gradual (antes de llamar a este método, se debe publicar el envío y se debe establecer el valor [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) en **PackageRolloutInProgress**).</li><li>El envío no pertenece a la aplicación especificada.</li><li>La aplicación usa una característica del panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   

<span/>


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administrar envíos de aplicaciones con la API de envío de la Tienda Windows](manage-app-submissions.md)
* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
