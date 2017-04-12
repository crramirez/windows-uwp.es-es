---
author: mcleanbyron
ms.assetid: AD80F9B3-CED0-40BD-A199-AB81CDAE466C
description: "Usa este método en la API de envío de la Tienda Windows para eliminar un paquete piloto de una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows."
title: Eliminar un paquete piloto
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store submission API, API de envío de la Tienda Windows, delete flight, eliminar piloto"
ms.openlocfilehash: e20661cef4ac7cad17ea5a62d37e9b217061809c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="delete-a-package-flight"></a>Eliminar un paquete piloto




Usa este método en la API de envío de la Tienda Windows para eliminar un paquete piloto de una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows.


## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El Id. de la Tienda de la aplicación que contiene el paquete piloto que quieres eliminar. El Id. de la Tienda para la aplicación está disponible en la página del panel del Centro de desarrollo.  |
| flightId | cadena | Obligatorio. El identificador del paquete piloto que se va a eliminar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un paquete piloto](create-a-flight.md) y [obtención de paquetes piloto para una aplicación](get-flights-for-an-app.md).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

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
| 409  | Se encontró el paquete piloto especificado, pero no se ha podido eliminar en su estado actual o la aplicación usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Crear un paquete piloto](create-a-flight.md)
* [Obtener un paquete piloto](get-a-flight.md)
