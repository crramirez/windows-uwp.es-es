---
ms.assetid: 87708690-079A-443D-807E-D2BF9F614DDF
description: Use este método en la API de envío de Microsoft Store para obtener datos de un paquete piloto para una aplicación registrada en la cuenta del centro de Partners.
title: Obtener un paquete piloto
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, vuelo, paquete piloto
ms.localizationpriority: medium
ms.openlocfilehash: 3916328f2c48642596c6a0b11401f583957bc973
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172909"
---
# <a name="get-a-package-flight"></a>Obtener un paquete piloto

Use este método en la API de envío de Microsoft Store para obtener datos de un paquete piloto para una aplicación registrada en la cuenta del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. El Id. de la Tienda de la aplicación que contiene el paquete piloto que quieres obtener. El identificador de almacén de la aplicación está disponible en el centro de Partners.  |
| flightId | string | Necesario. El identificador del paquete piloto que se va a obtener. Este identificador está disponible en los datos de respuesta para las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). En el caso de un vuelo creado en el centro de Partners, este identificador también está disponible en la dirección URL de la página de vuelo del centro de Partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra cómo recuperar información sobre un paquete piloto con el identificador 43e448df-97c9-4a43-a0bc-2a445e736bcd para una aplicación con el Id. de la Tienda 9WZDNCRD91MD.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "lastPublishedFlightSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621086517"
  },
  "pendingFlightSubmission": {
    "id": "115292150462124364",
    "resourceLocation": "flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243647"
  },
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | El identificador del paquete piloto. Este valor lo proporciona el centro de Partners.  |
| friendlyName           | string  | El nombre del paquete piloto, según lo especifica el desarrollador.   |  
| lastPublishedFlightSubmission       | object | Un objeto que proporciona información sobre el último envío publicado para el paquete piloto. Para obtener más información, consulta la sección [Objeto de envío](#submission_object) a continuación.  |
| pendingFlightSubmission        | object  |  Un objeto que proporciona información sobre el envío pendiente actualmente para el paquete piloto. Para obtener más información, consulta la sección [Objeto de envío](#submission_object) a continuación.  |   
| groupIds           | array  | Una matriz de cadenas que contienen los identificadores de los grupos de pilotos asociados con el paquete piloto. Para obtener más información sobre los grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).   |
| rankHigherThan           | string  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Para obtener más información sobre la clasificación de grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).  |


<span id="submission_object" />

### <a name="submission-object"></a>Objeto de envío

Los valores *lastPublishedFlightSubmission* y *pendingFlightSubmission* del cuerpo de respuesta contienen objetos que proporcionan información de recursos sobre un envío para el paquete piloto. Estos objetos tienen los siguientes valores.

| Value           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | string  | Identificador del envío.    |
| resourceLocation   | string  | Ruta de acceso relativa que se puede anexar al URI de la solicitud de base `https://manage.devcenter.microsoft.com/v1.0/my/` para recuperar los datos completos para el envío.               |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción     |
|--------|---------------------  |
| 400  | La solicitud no es válida. |
| 404  | No se pudo encontrar el paquete piloto especificado.   |   
| 409  | La aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |                                                                                                 


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Crear un paquete piloto](create-a-flight.md)
* [Eliminar un paquete piloto](delete-a-flight.md)