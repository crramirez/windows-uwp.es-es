---
description: Use este método en la API de envío de Microsoft Store para obtener información sobre el lanzamiento de paquetes para el envío de paquetes piloto.
title: Obtener la información de lanzamiento para un envío piloto
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, implementación de paquetes, envío de vuelos
ms.assetid: 397f1b99-2be7-4f65-bcf1-9433a3d496ad
ms.localizationpriority: medium
ms.openlocfilehash: 5f8e39650c5b0d9e9124ed671613e7a2f8e77ed2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171419"
---
# <a name="get-rollout-info-for-a-flight-submission"></a>Obtener la información de lanzamiento para un envío piloto


Use este método en la API de envío de Microsoft Store para obtener información sobre el [lanzamiento de paquetes](../publish/gradual-package-rollout.md) para el envío de paquetes piloto. Para obtener más información sobre el proceso de creación de un envío de paquetes piloto mediante el Microsoft Store API de envío, vea Administrar envíos de [paquetes de paquetes](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Cree un envío de paquetes piloto para una de sus aplicaciones. Puede hacerlo en el centro de Partners, o bien puede hacerlo mediante el método [crear un envío de paquetes piloto](create-a-flight-submission.md) .

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout   ` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. El identificador de la Tienda de la aplicación que contiene el envío del paquete piloto con la información acerca del lanzamiento de paquetes cuyo estado quieres obtener. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |
| flightId | string | Necesario. El identificador del paquete piloto que contiene el envío con la información acerca del lanzamiento de paquetes cuyo estado quieres obtener. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). En el caso de un vuelo creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de vuelo del centro de Partners.    |
| submissionId | string | Necesario. El identificador del envío con la información acerca del lanzamiento de paquetes que quieres obtener. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un envío de paquetes piloto](create-a-flight-submission.md). En el caso de un envío creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de envío del centro de Partners.   |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo obtener la información acerca del lanzamiento de paquetes para el envío de paquetes piloto.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

El siguiente ejemplo muestra el cuerpo de la respuesta JSON a una llamada correcta a este método para el envío de un paquete piloto con el lanzamiento de paquetes gradual habilitado. Para obtener más información sobre los valores que se encuentran en el cuerpo de respuesta, consulta el recurso [Package rollout object](manage-flight-submissions.md#package-rollout-object) (Objeto de lanzamiento de paquete).

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
| 409  | El envío de paquetes piloto no pertenece al paquete piloto especificado o la aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administrar envíos de paquetes de vuelos mediante la API de envío de Microsoft Store](manage-flight-submissions.md)
* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)