---
author: Xansky
description: Usa este método en la API de envío de Microsoft Store para obtener información acerca del lanzamiento de paquetes para el envío de paquete piloto.
title: Obtener la información de lanzamiento para un envío piloto
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, package rollout, lanzamiento de paquete, flight submission, envío piloto
ms.assetid: 397f1b99-2be7-4f65-bcf1-9433a3d496ad
ms.localizationpriority: medium
ms.openlocfilehash: 97051f3953b215dcf2ae2a2af00b55b435b726c8
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6971957"
---
# <a name="get-rollout-info-for-a-flight-submission"></a>Obtener la información de lanzamiento para un envío piloto


Usa este método en la API de envío de Microsoft Store para obtener información acerca del [lanzamiento de paquetes](../publish/gradual-package-rollout.md) para el envío de paquete piloto. Para obtener más información acerca del proceso de creación de un envío de paquete piloto mediante la API de envío de Microsoft Store, consulta [Administrar envíos de paquetes piloto](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.
* Crear un envío de paquete piloto para una de las aplicaciones. Puedes hacerlo en el centro de partners, o puedes hacerlo mediante el método [crea un envío de paquete piloto](create-a-flight-submission.md) .

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El identificador de la Store de la aplicación que contiene el envío del paquete piloto con la información acerca del lanzamiento de paquetes cuyo estado quieres obtener. Para obtener más información sobre el identificador de la Store, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | cadena | Obligatorio. El identificador del paquete piloto que contiene el envío con la información acerca del lanzamiento de paquetes cuyo estado quieres obtener. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). Para un piloto creado en el centro de partners, este Id. también está disponible en la dirección URL de la página de piloto del centro de partners.    |
| submissionId | cadena | Obligatorio. El identificador del envío con la información acerca del lanzamiento de paquetes que quieres obtener. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un envío de paquete piloto](create-a-flight-submission.md). Para un envío que se creó en el centro de partners, este Id. también está disponible en la dirección URL de la página de envío del centro de partners.   |


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
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

Si el lanzamiento de paquetes gradual no está habilitado en el envío del paquete piloto, se devolverá el siguiente cuerpo de respuesta.

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar el envío de paquete piloto. |
| 409  | El envío de paquete piloto no pertenece del paquete piloto especificado o la aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administrar envíos de paquete piloto mediante la API de envío de Microsoft Store](manage-flight-submissions.md)
* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
