---
author: mcleanbyron
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: "Usa este método en la API de envío de la Tienda Windows para confirmar un envío de paquete piloto nuevo o actualizado al Centro de desarrollo de Windows."
title: "Confirmación de un envío de paquete piloto con la API de envío de la Tienda Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: a9ea8de7b92b254c7bb8d63a5a3ea41afdd2d966

---

# Confirmación de un envío de paquete piloto con la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para confirmar un envío de paquetes piloto nuevo o actualizado al Centro de desarrollo de Windows. El centro de desarrollo alerta al Centro de desarrollo de que los datos de envío se han cargado (incluidos los paquetes relacionados). En respuesta, el Centro de desarrollo confirma los cambios en los datos de envío para la recopilación y la publicación. Después de que la operación de confirmación se realice correctamente, los cambios en el envío se muestran en el panel del Centro de desarrollo.

Para obtener más información sobre cómo se ajusta la operación de confirmación en el proceso de creación de un envío de paquete piloto mediante la API de envío de la Tienda Windows, consulta [Administración de envíos de paquetes piloto](manage-flight-submissions.md).

## Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.
* [Crea un envío de paquete piloto](create-a-flight-submission.md) y después [actualiza el envío](update-a-flight-submission.md) con cualquier cambio necesario en los datos de envío.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del cuerpo del encabezado y la solicitud.

| Method | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` |

<span/>
 

### Encabezado de la solicitud

| Encabezado        | Type   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. Token de acceso de Azure AD con formato **Token del** &lt;*portador*&gt;. |

<span/>

### Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El Id. de la Tienda de la aplicación que contiene el envío de paquete piloto que quieres confirmar. El Id. de la Tienda para la aplicación está disponible en el panel del Centro de desarrollo.  |
| flightId | string | Obligatorio. El identificador del paquete piloto que contiene el envío que se va a confirmar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta para las solicitudes de [creación de un paquete piloto](create-a-flight.md) y [obtención de paquetes piloto para una aplicación](get-flights-for-an-app.md).  |
| submissionId | string | Obligatorio. El identificador del envío que se va a confirmar. Esta identificación está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta para las solicitudes de [creación de un envío de paquete piloto](create-a-flight-submission.md).  |

<span/>

### Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### Ejemplo de solicitud

El siguiente ejemplo muestra cómo confirmar un envío de paquete piloto.

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
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

| Value      | Type   | Descripción                                                                                                                                                                                                                                                                         |
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
* [Administración de envíos de paquetes piloto](manage-flight-submissions.md)
* [Obtención de un envío de paquete piloto](get-a-flight-submission.md)
* [Creación de un envío de paquete piloto](create-a-flight-submission.md)
* [Actualización de un envío de paquete piloto](update-a-flight-submission.md)
* [Eliminación de un envío de paquete piloto](delete-a-flight-submission.md)
* [Obtención del estado de un envío de paquete piloto](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


