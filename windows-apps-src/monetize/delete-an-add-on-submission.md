---
author: mcleanbyron
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: "Usa este método en la API de envío de la Tienda Windows para eliminar un envío de complemento existente."
title: "Eliminar un envío de complemento mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: b0068876803ea62dbf1d5329ddda19912a4f94d7

---

# Eliminar un envío de complemento mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para eliminar un envío de complemento (también conocido como producto desde la aplicación o IAP) existente.

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. Token de acceso de Azure AD con formato **Portador** &lt;*token*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | string | Obligatorio. El Id. de la Tienda del complemento que contiene el envío que se va a eliminar. El Id. de la Tienda está disponible en el panel del Centro de desarrollo.  |
| submissionId | string | Obligatorio. El identificador del envío que se va a eliminar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un envío de complemento](create-an-add-on-submission.md).  |

<span/>

### Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

### Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo eliminar un envío de complemento.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
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
| 409  | Se encontró el envío especificado, pero no se ha podido eliminar en su estado actual o el complemento usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## Temas relacionados

* [Create and manage submissions using Windows Store services (Crear y administrar envíos mediante el uso de servicios de la Tienda Windows)](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener un envío de complemento](get-an-add-on-submission.md)
* [Crear un envío de complemento](create-an-add-on-submission.md)
* [Confirmar un envío de complemento](commit-an-add-on-submission.md)
* [Actualizar un envío de complemento](update-an-add-on-submission.md)
* [Obtener el estado de un envío de complemento](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


