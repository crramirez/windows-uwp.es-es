---
author: mcleanbyron
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: "Usa este método en la API de envío de la Tienda Windows para eliminar un complemento de una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows."
title: "Eliminar un complemento mediante la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 8c28205ca004bd63b3f6a3fea2971cf927f7e2e4

---

# Eliminar un complemento mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para eliminar un complemento (también conocido como producto desde la aplicación o IAP) de una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows.

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. Token de acceso de Azure AD con formato **Portador** &lt;*token*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | string | Obligatorio. El Id. de la Tienda del complemento que se va a eliminar. El Id. de la Tienda está disponible en el panel del Centro de desarrollo.  |

<span/>

### Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

### Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo eliminar un complemento.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta

Si se realiza correctamente, este método devuelve un cuerpo de respuesta vacía.

## Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción                                                                                                                                                                           |
|--------|------------------|
| 400  | La solicitud no es válida. |
| 404  | No se pudo encontrar el complemento especificado.  |
| 409  | Se encontró el complemento especificado, pero no se ha podido eliminar en su estado actual o el complemento usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>

## Temas relacionados

* [Create and manage submissions using Windows Store services (Crear y administrar envíos mediante el uso de servicios de la Tienda Windows)](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener todos los complementos](get-all-add-ons.md)
* [Obtener un complemento](get-an-add-on.md)
* [Crear un complemento](create-an-add-on.md)



<!--HONumber=Aug16_HO5-->


