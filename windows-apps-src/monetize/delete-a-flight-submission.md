---
author: mcleanbyron
ms.assetid: 1A69A388-B1CC-4D2C-886B-EA07E6E60252
description: "Usa este método en la API de envío de la Tienda Windows para eliminar el envío de un paquete piloto existente."
title: "Eliminar un envío de paquete piloto mediante la API de envío de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envío de la Tienda Windows, Windows Store submission API, envío de paquete piloto, flight submission, eliminar, delete, paquete piloto, package flight"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e720b8551949335faa0407cbc159c217d4b91343
ms.lasthandoff: 02/07/2017

---

# <a name="delete-a-package-flight-submission-using-the-windows-store-submission-api"></a>Eliminar un envío de paquete piloto mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para eliminar el envío de un paquete piloto existente.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones del encabezado y del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/flights/{flightId}/submissions/{submissionId}``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Obligatorio. El Id. de la Tienda de la aplicación que contiene el envío de paquete piloto que quieres eliminar. Para obtener más información sobre el Id. de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | string | Obligatorio. El identificador del paquete piloto que contiene el envío que se va a eliminar. Este identificador está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta a las solicitudes de [creación de un paquete piloto](create-a-flight.md) y [obtención de paquetes piloto para una aplicación](get-flights-for-an-app.md).  |
| submissionId | string | Obligatorio. El identificador del envío que se va a eliminar. Esta identificación está disponible en el panel del Centro de desarrollo y se incluye en los datos de respuesta de las solicitudes de [creación de un envío de paquete piloto](create-a-flight-submission.md).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo eliminar un envío de un paquete piloto.

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
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
| 409  | Se encontró el envío especificado, pero no se ha podido eliminar en su estado actual o la aplicación usa una función de panel del Centro de desarrollo que [actualmente no es compatible con la API de envío de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Creación y administración de envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Manage package flight submissions (Administrar envíos de paquetes piloto)](manage-flight-submissions.md)
* [Get a package flight submission (Obtener un envío de paquete piloto)](get-a-flight-submission.md)
* [Crear un envío de paquete piloto](create-a-flight-submission.md)
* [Confirmar un envío de paquete piloto](commit-a-flight-submission.md)
* [Actualizar un envío de paquete piloto](update-a-flight-submission.md)
* [Obtener el estado de un envío de paquete piloto](get-status-for-a-flight-submission.md)

