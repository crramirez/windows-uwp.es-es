---
author: mcleanbyron
ms.assetid: 934F2DBF-2C7E-4B77-997D-17B9B0535D51
description: "Usa este método en la API de envío de la Tienda Windows para confirmar un envío de aplicaciones nuevo o actualizado al Centro de desarrollo de Windows."
title: "Confirmación de un envío de aplicaciones con el uso de la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 14698cfcd57d71682c40a8820a8a3d2c43dd3567

---

# Confirmación de un envío de aplicaciones con el uso de la API de envío de la Tienda Windows


Usa este método en la API de envío de la Tienda Windows para confirmar un envío de aplicaciones nuevo o actualizado al Centro de desarrollo de Windows. La acción de confirmación alerta al Centro de desarrollo de que los datos de envío se han cargado (incluidos los paquetes e imágenes relacionados). En respuesta, el Centro de desarrollo confirma los cambios en los datos de envío para la recopilación y la publicación. Después de que la operación de confirmación se realice correctamente, los cambios en el envío se muestran en el panel del Centro de desarrollo.

Para obtener más información sobre cómo se ajusta la operación de confirmación en el proceso de envío de una aplicación mediante la API de envío de la Tienda Windows, consulta [Administración de envíos de aplicaciones](manage-app-submissions.md).

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* [Crea un envío de aplicación](create-an-app-submission.md) y posteriormente [actualiza el envío](update-an-app-submission.md) con cualquier cambio necesario en los datos de envío.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del cuerpo del encabezado y la solicitud.

| Method | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. Token de acceso de Azure AD con formato **Token del** &lt;*portador*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El Id. de la Tienda de la aplicación que contiene el envío que deseas confirmar. Para obtener más información sobre el Id. de la Tienda, consulta [Visualización de los detalles de identidad de la aplicación](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| submissionId | string | Obligatorio. El identificador del envío que deseas confirmar. Esta identificación está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta para las solicitudes de [Creación de un envío de aplicaciones](create-an-app-submission.md).  |

<span/>

### Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### Ejemplo de solicitud

El siguiente ejemplo muestra cómo confirmar un envío de aplicaciones.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta

El siguiente ejemplo muestra el cuerpo de respuesta JSON para una llamada satisfactoria a este método. Para obtener más información acerca de los valores en el cuerpo de respuesta, consulta las secciones siguientes.

```json
{
  "status": "CommitStarted"
}
```

### Cuerpo de la respuesta

| Value      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| estado           | string  | El estado del envío. Este puede ser uno de los valores siguientes: <ul><li>Ninguno</li><li>Cancelado</li><li>Conf. pendiente</li><li>Conf. iniciada</li><li>Error de conf.</li><li>Publicación pendiente</li><li>Publicación</li><li>Publicado</li><li>Error de publ.</li><li>Preprocesamiento</li><li>Error de preproc.</li><li>Certificación</li><li>Error de certif.</li><li>Lanzamiento</li><li>Error de lanz.</li></ul>  |

<span/>

## Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 400  | Los parámetros de la solicitud no son válidos. |
| 404  | No se pudo encontrar el envío especificado. |
| 409  | Se encontró el envío especificado, pero no se ha podido confirmar en su estado actual o la aplicación usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>


## Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtención de un envío de aplicación](get-an-app-submission.md)
* [Creación de un envío de aplicación](create-an-app-submission.md)
* [Actualización de un envío de aplicación](update-an-app-submission.md)
* [Eliminación de un envío de aplicación](delete-an-app-submission.md)
* [Obtención del estado de un envío de aplicación](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


