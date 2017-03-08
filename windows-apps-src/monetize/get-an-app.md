---
author: mcleanbyron
ms.assetid: DAF92881-6AF6-44C7-B466-215F5226AE04
description: "Usa este método en la API de envío de la Tienda Windows para recuperar información sobre una aplicación específica registrada en tu cuenta del Centro de desarrollo de Windows."
title: "Obtener una aplicación mediante la API de envío de la Tienda Windows"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, UWP, API de envío de la Tienda Windows, Windows Store submission API, aplicación, app"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 176e3b584137e503c4adfe2eb612d9329226baac
ms.lasthandoff: 02/07/2017

---

# <a name="get-an-app-using-the-windows-store-submission-api"></a>Obtener una aplicación mediante la API de envío de la Tienda Windows




Usa este método en la API de envío de la Tienda Windows para recuperar información sobre una aplicación específica registrada en tu cuenta del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | cadena | Obligatorio. El Id. de la Tienda de la aplicación que se recuperará. Para obtener más información sobre el Id. de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

<span/>

### <a name="request-example"></a>Ejemplo de solicitud

En el siguiente ejemplo se muestra cómo recuperar información acerca de una aplicación con el valor de Id. de la Tienda 9WZDNCRD91MD.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de respuesta JSON de una llamada correcta a este método. Para obtener más información acerca de los valores del cuerpo de la respuesta, consulta [Recurso de aplicación](get-app-data.md#application_object).

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  }
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar la aplicación especificada. |
| 409  | La aplicación usa una característica del panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md)
* [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)

