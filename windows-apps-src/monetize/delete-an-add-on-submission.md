---
author: mcleanbyron
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: "Usa este método en la API de envío de la Tienda Windows para eliminar un envío de complemento existente."
title: "Eliminar un envío de complemento"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store submission API, API de envío de la Tienda Windows, add-on submission, envío de complemento, delete, eliminar, delete, in-app product, producto desde la aplicación, IAP, IAP"
ms.openlocfilehash: a1f2a545b75ac4416ac016d36836328b24433917
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="delete-an-add-on-submission"></a>Eliminar un envío de complemento




Usa este método en la API de envío de la Tienda Windows para eliminar un envío de complemento (también conocido como producto desde la aplicación o IAP) existente.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | cadena | Obligatorio. El Id. de la Tienda del complemento que contiene el envío que se va a eliminar. El Id. de la Tienda está disponible en el panel del Centro de desarrollo.  |
| submissionId | cadena | Obligatorio. El identificador del envío que se va a eliminar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un envío de complemento](create-an-add-on-submission.md).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo eliminar un envío de complemento.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

Si se realiza correctamente, este método devuelve un cuerpo de respuesta vacía.

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | Los parámetros de la solicitud no son válidos. |
| 404  | No se pudo encontrar el envío especificado. |
| 409  | Se encontró el envío especificado, pero no se ha podido eliminar en su estado actual o el complemento usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtención de un envío de complemento](get-an-add-on-submission.md)
* [Create an add-on submission (Crear un envío de complemento)](create-an-add-on-submission.md)
* [Confirmar un envío de complemento](commit-an-add-on-submission.md)
* [Actualizar un envío de complemento](update-an-add-on-submission.md)
* [Obtener el estado de un envío de complemento](get-status-for-an-add-on-submission.md)
