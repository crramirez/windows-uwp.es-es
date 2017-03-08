---
author: mcleanbyron
description: "Usa este método en la API de envío de la Tienda Windows para obtener información acerca del lanzamiento de paquetes para el envío de paquetes piloto."
title: "Obtener información acerca del lanzamiento de paquete para el envío de paquetes piloto con la API de envío de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envío de la Tienda Windows, Windows Store submission API, lanzamiento de paquete, package rollout, envío de paquete piloto, flight submission"
ms.assetid: 397f1b99-2be7-4f65-bcf1-9433a3d496ad
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: d47bd0a5df654ba723c1c7650ea9779ee5962993
ms.lasthandoff: 02/08/2017

---

# <a name="get-package-rollout-info-for-a-package-flight-submission-using-the-windows-store-submission-api"></a>Obtener información acerca del lanzamiento de paquete para el envío de paquetes piloto con la API de envío de la Tienda Windows


Usa este método en la API de envío de la Tienda Windows para obtener información acerca del [lanzamiento de paquetes](../publish/gradual-package-rollout.md) para el envío de paquetes piloto. Para obtener más información acerca del proceso de creación de un envío de paquete piloto mediante la API de envío de la Tienda Windows, consulta [Administrar envíos de paquetes piloto](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* Crea un envío de paquete piloto para una aplicación de tu cuenta del Centro de desarrollo. Para hacer esto, puedes usar el panel del Centro de desarrollo o el método [crear un envío de paquete piloto](create-a-flight-submission.md).

>**Nota**&nbsp;&nbsp;Este método solo puede usarse en cuentas del Centro de desarrollo de Windows que estén autorizadas para usar la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de utilización y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El identificador de la Tienda de la aplicación que contiene el envío del paquete piloto con la información acerca del lanzamiento de paquetes cuyo estado quieres obtener. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Obligatorio. El identificador del paquete piloto que contiene el envío con la información acerca del lanzamiento de paquetes cuyo estado quieres obtener. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un paquete piloto](create-a-flight.md) y [obtención de paquetes piloto de una aplicación](get-flights-for-an-app.md).  |
| submissionId | cadena | Obligatorio. El identificador del envío con la información acerca del lanzamiento de paquetes que quieres obtener. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un envío de paquete piloto](create-a-flight-submission.md).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo obtener la información acerca del lanzamiento de paquetes para el envío de paquetes piloto.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

El siguiente ejemplo muestra el cuerpo de la respuesta JSON a una llamada correcta a este método para el envío de un paquete piloto con el lanzamiento de paquetes gradual habilitado. Para obtener más información acerca de los valores que se encuentran en el cuerpo de respuesta, consulta el [recurso de lanzamiento de paquete](manage-flight-submissions.md#package-rollout-object).

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

Si el lanzamiento de paquetes gradual no está habilitado en el envío del paquete piloto, se devolverá el siguiente cuerpo de respuesta.

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío de paquete piloto. |
| 409  | El envío de paquete piloto no pertenece al paquete piloto especificado o la aplicación usa una característica del panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administrar envíos de paquete piloto mediante la API de envío de la Tienda Windows](manage-flight-submissions.md)
* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)

