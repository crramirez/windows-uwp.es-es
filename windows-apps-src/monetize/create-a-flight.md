---
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: Use este método en el Microsoft Store API de envío para crear un paquete piloto para una aplicación registrada en su cuenta del centro de Partners.
title: Crear un paquete piloto
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store API de envío, creación de vuelos
ms.localizationpriority: medium
ms.openlocfilehash: ec087d590c0286eb99c3ad2524036d9fd4230508
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172949"
---
# <a name="create-a-package-flight"></a>Crear un paquete piloto

Use este método en el Microsoft Store API de envío para crear un paquete piloto para una aplicación registrada en su cuenta del centro de Partners.

> [!NOTE]
> Este método crea un paquete piloto sin envíos. Para crear un envío de paquete piloto, consulta los métodos de [Administración de envíos de paquetes piloto](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. El Id. de la Tienda de la aplicación sobre la que quieres crear un paquete piloto. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.

|  Parámetro  |  Tipo  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  friendlyName  |  string  |  El nombre del paquete piloto, según lo especifica el desarrollador.  |  No  |
|  groupIds  |  array  |  Una matriz de cadenas que contienen los identificadores de los grupos de pilotos asociados con el paquete piloto. Para obtener más información sobre los grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).  |  No  |
|  rankHigherThan  |  string  |  El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Si no estableces este parámetro, el nuevo paquete piloto tendrá la puntuación más alta de todos los paquetes piloto. Para obtener más información sobre la clasificación de grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).    |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo crear un nuevo paquete piloto para una aplicación que tiene el Id. de la Tienda 9WZDNCRD911W.

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
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
| friendlyName           | string  | El nombre del paquete piloto, como se especifica en la solicitud.   |  
| groupIds           | array  | Una matriz de cadenas que contengan los identificadores de los grupos piloto asociados con el paquete piloto, como se especifica en la solicitud. Para obtener más información sobre los grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).   |
| rankHigherThan           | string  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente menor que el paquete piloto actual, como se especifica en la solicitud. Para obtener más información sobre la clasificación de grupos de pilotos, consulta [Paquetes piloto](../publish/package-flights.md).  |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | La solicitud no es válida. |
| 409  | No se pudo crear el paquete piloto debido a su estado actual o la aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener un paquete piloto](get-a-flight.md)
* [Eliminar un paquete piloto](delete-a-flight.md)