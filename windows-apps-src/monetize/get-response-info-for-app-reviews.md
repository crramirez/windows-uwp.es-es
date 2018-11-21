---
author: Xansky
ms.assetid: fb6bb856-7a1b-4312-a602-f500646a3119
description: Usa este método en la API de opiniones de Microsoft Store para determinar si puedes responder a una opinión particular, o si puedes responder a cualquier opinión de una aplicación determinada.
title: Obtener información de respuesta de opiniones
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Microsoft Store, Microsoft Store reviews API, API de opiniones de Microsoft Store, response info, información de respuesta
ms.localizationpriority: medium
ms.openlocfilehash: 466455a5e8da9364206245f1e0ac10acfed07ee7
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7440569"
---
# <a name="get-response-info-for-reviews"></a>Obtener información de respuesta de opiniones

Si quieres responder mediante programación a una opinión del cliente sobre la aplicación, puedes usar este método en la API de opiniones de Microsoft Store para determinar primero si tienes permiso para responder. No puedes responder a opiniones enviadas por clientes que han elegido no recibir respuestas a opiniones. Después de confirmar que puedes responder a la opinión, puedes usar el método de [Submit responses to app reviews (Enviar respuestas a las opiniones de la aplicación](submit-responses-to-app-reviews.md) para responderla mediante programación.


## <a name="prerequisites"></a>Requisitos previos.

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](respond-to-reviews-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén el ID de la opinión que quieras comprobar para determinar si puedes responderla. Los identificadores de opinión están disponibles en los datos de respuesta del método [obtener opiniones de la aplicación](get-app-reviews.md) en la API de análisis de Microsoft Store y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [informe de opiniones](../publish/reviews-report.md).

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/{reviewId}/apps/{applicationId}/responses/info``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   | Descripción                                     |  Obligatorio  |
|---------------|--------|--------------------------------------------------|--------------|
| applicationId | string | El identificador de la aplicación que contiene la opinión que quieres determinar si puedes responder. El identificador de la tienda está disponible en la [página de identidad de la aplicación](../publish/view-app-identity-details.md) en el centro de partners. Un ejemplo de un Id. de la Store sería 9WZDNCRFJ3Q8. |  Sí  |
| reviewId | string | El id. de la opinión a la que deseas responder (es un GUID). Los identificadores de opinión están disponibles en los datos de respuesta del método [obtener opiniones de la aplicación](get-app-reviews.md) en la API de análisis de Microsoft Store y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [informe de opiniones](../publish/reviews-report.md). <br/>Si omites este parámetro, el cuerpo de respuesta de este método indicará si dispones de permisos para responder a opiniones de la aplicación especificada. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En los siguientes ejemplos se muestra cómo usar este método para determinar si puedes responder a una opinión determinada.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/reviews/6be543ff-1c9c-4534-aced-af8b4fbe0316/apps/9WZDNCRFJ3Q8/responses/info HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción    |  
|------------|--------|-----------------------|
| CanRespond      | Boolean  | El valor **true** indica que puedes responder a la opinión especificada o que tienes permisos para responder a cualquier opinión de la aplicación especificada. De lo contrario, este valor es **false**.       |
| DefaultSupportEmail  | string |  La [dirección de correo electrónico de soporte técnico](../publish/enter-app-properties.md#support-contact-info) de tu aplicación según se indica en la lista de aplicaciones de la Store. Si no especificaste ninguna dirección de correo electrónico de soporte técnico, este campo estará vacío.    |

 
### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "CanRespond": true,
  "DefaultSupportEmail": "support@contoso.com"
}
```

## <a name="related-topics"></a>Temas relacionados

* [Enviar respuestas a opiniones con la API de análisis de Microsoft Store](submit-responses-to-app-reviews.md)
* [Responder a las opiniones de cliente mediante el centro de partners](../publish/respond-to-customer-reviews.md)
* [Responder a las opiniones con servicios de Microsoft Store](respond-to-reviews-using-windows-store-services.md)
* [Obtener opiniones de la aplicación](get-app-reviews.md)
