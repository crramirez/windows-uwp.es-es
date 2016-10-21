---
author: mcleanbyron
ms.assetid: 96C090C1-88F8-42E7-AED1-AFA9031E952B
description: "Usa este método en la API de envío de la Tienda Windows para eliminar un envío de aplicación existente."
title: "Eliminar un envío de aplicación mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 4109414e684fc38f461f9e97af38839cb83330fc

---

# Eliminar un envío de aplicación mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para eliminar un envío de aplicación existente.

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. Token de acceso de Azure AD con formato **Portador** &lt;*token*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El Id. de la Tienda de la aplicación que contiene el envío que se va a eliminar. Para obtener más información sobre el Id. de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Obligatorio. El identificador del envío que se va a eliminar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un envío de aplicación](create-an-app-submission.md).  |

<span/>

### Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

### Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo eliminar un envío de aplicación.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610 HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta

Si se realiza correctamente, este método devuelve un cuerpo de respuesta vacía.

## Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | Los parámetros de la solicitud no son válidos. |
| 404  | No se pudo encontrar el envío especificado. |
| 409  | Se encontró el envío especificado, pero no se ha podido eliminar en su estado actual o la aplicación usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## Temas relacionados

* [Create and manage submissions using Windows Store services (Crear y administrar envíos mediante el uso de servicios de la Tienda Windows)](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener un envío de aplicación](get-an-app-submission.md)
* [Crear un envío de aplicación](create-an-app-submission.md)
* [Confirmar el envío de aplicación](commit-an-app-submission.md)
* [Actualizar un envío de aplicación](update-an-app-submission.md)
* [Obtener el estado de un envío de aplicación](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


