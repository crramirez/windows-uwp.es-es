---
description: Use este método en la API de envío de Microsoft Store para finalizar el lanzamiento de paquetes para un envío de paquetes piloto.
title: Finalizar el lanzamiento de un envío piloto
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, implementación de paquetes, envío de vuelos, finalizar
ms.assetid: e4a645f6-1f00-4af5-80d6-d2ee179acc8a
ms.localizationpriority: medium
ms.openlocfilehash: a60da322ccd76989b43d13c336ecde26671c6873
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171531"
---
# <a name="finalize-the-rollout-for-a-flight-submission"></a>Finalizar el lanzamiento de un envío piloto


Use este método en la API de envío de Microsoft Store para [finalizar el lanzamiento de paquetes](../publish/gradual-package-rollout.md#completing-the-rollout) para un envío de paquetes piloto. Para obtener más información sobre el proceso de creación de un envío de paquetes piloto mediante el Microsoft Store API de envío, vea Administrar envíos de [paquetes de paquetes](manage-flight-submissions.md).


## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Cree un envío para una aplicación en el centro de Partners. Puede hacerlo en el centro de Partners o puede hacerlo mediante el método [crear un envío de aplicación](create-an-app-submission.md) .
* Habilita un lanzamiento de paquete gradual para el envío. Puede hacerlo en el [centro de Partners](../publish/gradual-package-rollout.md)o puede hacerlo mediante [el Microsoft Store API de envío](manage-flight-submissions.md#manage-gradual-package-rollout).

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y de los parámetros de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. El Id. de la Tienda de la aplicación que contiene el envío del paquete piloto con el lanzamiento de paquetes que quieres finalizar. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |
| flightId | string | Necesario. El identificador del paquete piloto que contiene el envío con el lanzamiento de paquetes que quieres finalizar. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). En el caso de un vuelo creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de vuelo del centro de Partners. |
| submissionId | string | Necesario. El identificador del envío con el lanzamiento de paquetes que quieres finalizar. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un envío de paquetes piloto](create-a-flight-submission.md). En el caso de un envío creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de envío del centro de Partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo finalizar el lanzamiento de paquete para el envío del paquete piloto.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243680/finalizepackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información sobre los valores que se encuentran en el cuerpo de respuesta, consulta el recurso [Package rollout object](manage-flight-submissions.md#package-rollout-object) (Objeto de lanzamiento de paquete).

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
| 404  | No se pudo encontrar el envío de paquete piloto. |
| 409  | Este código indica uno de los siguientes errores:<br/><br/><ul><li>El envío no está en un estado válido para la operación de lanzamiento gradual (antes de llamar a este método, se debe publicar el envío y se debe establecer el valor [packageRolloutStatus](manage-flight-submissions.md#package-rollout-object) en **PackageRolloutInProgress**).</li><li>El envío no pertenece a la aplicación especificada.</li><li>La aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).</li></ul> |   


## <a name="related-topics"></a>Temas relacionados

* [Lanzamiento gradual del paquete](../publish/gradual-package-rollout.md)
* [Administrar envíos de paquetes de vuelos mediante la API de envío de Microsoft Store](manage-flight-submissions.md)
* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)