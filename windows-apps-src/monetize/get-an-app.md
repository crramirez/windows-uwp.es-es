---
ms.assetid: DAF92881-6AF6-44C7-B466-215F5226AE04
description: Use este método en la API de envío de Microsoft Store para recuperar información acerca de una aplicación específica registrada en su cuenta del centro de Partners.
title: Obtener una aplicación
ms.date: 02/28/2018
ms.topic: article
keywords: Windows 10, UWP, API de envío de Microsoft Store, aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 29f4b7fdb196e1463f4e738618694363094933f0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171479"
---
# <a name="get-an-app"></a>Obtener una aplicación

Use este método en la API de envío de Microsoft Store para recuperar información acerca de una aplicación específica registrada en su cuenta del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Nombre        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | string | Necesario. El Id. de la Tienda de la aplicación que se recuperará. Para obtener más información sobre el identificador de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](../publish/view-app-identity-details.md).  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra cómo recuperar información sobre una aplicación con el valor de ID. de almacén 9NBLGGH4R315.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

En el siguiente ejemplo se muestra el cuerpo de la respuesta JSON de una llamada satisfactoria a este método. Para obtener más información acerca de los valores del cuerpo de la respuesta, consulta [Recurso de aplicación](get-app-data.md#application_object).

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
  },
  "hasAdvancedListingPermission": false
}
```

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se pudo encontrar la aplicación especificada. |
| 409  | La aplicación usa una característica del centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos con Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md)
* [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)