---
author: mcleanbyron
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: "Usa este método en la API de envío de la Tienda Windows para crear un paquete piloto para una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows."
title: "Creación de un paquete piloto mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 35823bd1fd0c059ebc9b2107c31400a7ad788a1e

---

# Creación de un paquete piloto mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para crear un paquete piloto para una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows.

>**Nota**&nbsp;&nbsp;Este método crea un paquete piloto sin envíos. Para crear un envío de paquete piloto, consulta los métodos de [Administración de envíos de paquetes piloto](manage-flight-submissions.md).

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del cuerpo del encabezado y la solicitud.

| Method | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Type   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con formato **Token del** &lt;*portador*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Type   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El Id. de la Tienda de la aplicación sobre la que quieres crear un paquete piloto. Para obtener más información sobre el Id. de la Tienda, consulta [Visualización de los detalles de identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |

<span/>

### Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.
 
|  Parámetro  |  Type  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  friendlyName  |  string  |  El nombre del paquete piloto, según lo especificado por el desarrollador.  |  No  |
|  groupIds  |  matriz  |  Una matriz de cadenas que contengan los identificadores de los grupos piloto asociados con el paquete piloto. Para obtener más información acerca de los grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |  No  |
|  rankHigherThan  |  string  |  El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Si no estableces este parámetro, el nuevo paquete piloto tendrá la puntuación más alta de todos los paquetes piloto. Para obtener más información acerca de la clasificación de grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).    |  No  |

<span/>

### Ejemplo de solicitud

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

## Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. Para obtener más información acerca de los valores en el cuerpo de respuesta, consulta las secciones siguientes.

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

### Cuerpo de la respuesta

| Value      | Type   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | string  | El identificador para el paquete piloto. Este valor está proporcionado por el Centro de desarrollo.  |
| friendlyName           | string  | El nombre del paquete piloto, como se especifica en la solicitud.   |  
| groupIds           | matriz  | Una matriz de cadenas que contengan los identificadores de los grupos piloto asociados con el paquete piloto, como se especifica en la solicitud. Para obtener más información acerca de los grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente menor que el paquete piloto actual, como se especifica en la solicitud. Para obtener más información acerca de la clasificación de grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |

<span/>

## Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | La solicitud no es válida. |
| 409  | No se pudo crear el envío del paquete piloto debido al estado actual de la aplicación o a que esta aplicación usa una característica del panel del Centro de desarrollo que [no admite la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   
<span/>

## Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtención de un paquete piloto](get-a-flight.md)
* [Eliminación de un paquete piloto](delete-a-flight.md)



<!--HONumber=Aug16_HO5-->


