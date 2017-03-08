---
author: mcleanbyron
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: "Usa este método en la API de envío de la Tienda Windows para crear un paquete piloto para una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows."
title: "Creación de un paquete piloto mediante la API de envío de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, API de envío de la Tienda Windows, crear piloto"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2b2b8acb1cfa2a1eaa0ce586cace250cedb5cf71
ms.lasthandoff: 02/07/2017

---

# <a name="create-a-package-flight-using-the-windows-store-submission-api"></a>Creación de un paquete piloto mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para crear un paquete piloto para una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows.

>**Nota**&nbsp;&nbsp;Este método crea un paquete piloto sin envíos. Para crear un envío de paquete piloto, consulta los métodos de [Administración de envíos de paquetes piloto](manage-flight-submissions.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del cuerpo del encabezado y la solicitud.

| Method | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Type   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | Cadena | Obligatorio. El Id. de la Tienda de la aplicación sobre la que quieres crear un paquete piloto. Para obtener más información sobre el Id. de la Tienda, consulta [Visualización de los detalles de identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes parámetros.
 
|  Parámetro  |  Type  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  friendlyName  |  string  |  El nombre del paquete piloto, según lo especificado por el desarrollador.  |  No  |
|  groupIds  |  matriz  |  Una matriz de cadenas que contengan los identificadores de los grupos piloto asociados con el paquete piloto. Para obtener más información acerca de los grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |  No  |
|  rankHigherThan  |  string  |  El nombre descriptivo del paquete piloto que está clasificado inmediatamente por debajo del paquete piloto actual. Si no estableces este parámetro, el nuevo paquete piloto tendrá la puntuación más alta de todos los paquetes piloto. Para obtener más información acerca de la clasificación de grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).    |  No  |

<span/>

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

## <a name="response"></a>Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta las secciones siguientes.

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

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | cadena  | El identificador para el paquete piloto. Proporciona este valor el Centro de desarrollo.  |
| friendlyName           | string  | El nombre del paquete piloto, como se especifica en la solicitud.   |  
| groupIds           | matriz  | Una matriz de cadenas que contengan los identificadores de los grupos piloto asociados con el paquete piloto, como se especifica en la solicitud. Para obtener más información acerca de los grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | cadena  | El nombre descriptivo del paquete piloto que está clasificado inmediatamente menor que el paquete piloto actual, como se especifica en la solicitud. Para obtener más información acerca de la clasificación de grupos piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).  |

<span/>

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | La solicitud no es válida. |
| 409  | No se pudo crear el envío del paquete piloto debido al estado actual de la aplicación o a que esta aplicación usa una característica del panel del Centro de desarrollo que [no admite la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   
<span/>

## <a name="related-topics"></a>Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtención de un paquete piloto](get-a-flight.md)
* [Eliminación de un paquete piloto](delete-a-flight.md)

