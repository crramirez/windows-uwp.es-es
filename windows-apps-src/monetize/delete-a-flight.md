---
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: Usa este método en la API de envío de Microsoft Store para eliminar un paquete piloto para una aplicación que está registrada en tu cuenta del centro de partners.
title: Eliminar un paquete piloto
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, delete flight, eliminar piloto
ms.localizationpriority: medium
ms.openlocfilehash: fa3fa78c695538ec13dbd20d38a24224c560463e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7851688"
---
# <a name="delete-a-package-flight"></a>Eliminar un paquete piloto

Usa este método en la API de envío de Microsoft Store para eliminar un paquete piloto para una aplicación que está registrada en tu cuenta del centro de partners.


## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El Id. de la Store de la aplicación que contiene el paquete piloto que quieres eliminar. El Id. de la tienda de la aplicación está disponible en el centro de partners.  |
| flightId | cadena | Obligatorio. El identificador del paquete piloto que se va a eliminar. Este identificador está disponible en los datos de respuesta a las solicitudes para [crear un paquete piloto](create-a-flight.md) y [obtener paquetes piloto para una aplicación](get-flights-for-an-app.md). Para un piloto creado en el centro de partners, este Id. también está disponible en la dirección URL de la página de piloto del centro de partners.  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.


### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo eliminar un paquete piloto.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Si se realiza correctamente, este método devuelve un cuerpo de respuesta vacía.

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción                                                                                                                                                                           |
|--------|------------------|
| 400  | Los parámetros de la solicitud no son válidos. |
| 404  | No se pudo encontrar el paquete piloto especificado.  |
| 409  | Se encontró el paquete piloto especificado, pero no se ha podido en su estado actual o la aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Crear un paquete piloto](create-a-flight.md)
* [Obtener un paquete piloto](get-a-flight.md)
