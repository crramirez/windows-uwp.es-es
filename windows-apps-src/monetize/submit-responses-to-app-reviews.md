---
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: Use este método en la API de Microsoft Store Reviews para enviar respuestas a las revisiones de la aplicación.
title: Enviar respuestas a opiniones
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, API de Microsoft Store Reviews, adquisiciones de complementos
ms.localizationpriority: medium
ms.openlocfilehash: 9cf62f5f619eba0431f1398a391eef03b47bb13d
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984641"
---
# <a name="submit-responses-to-reviews"></a>Enviar respuestas a opiniones


Use este método en la API de Microsoft Store Reviews para responder mediante programación a las revisiones de la aplicación. Cuando llame a este método, debe especificar los identificadores de las revisiones a las que desea responder. Los identificadores de revisión están disponibles en los datos de respuesta del método [Get App Reviews](get-app-reviews.md) de la API de Microsoft Store Analytics y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [Informe de revisiones](../publish/reviews-report.md).

Cuando un cliente envía una revisión, puede optar por no recibir respuestas a su revisión. Si intenta responder a una revisión para la que el cliente decidió no recibir respuestas, el cuerpo de respuesta de este método indicará que el intento de respuesta no se ha realizado correctamente. Antes de llamar a este método, puede determinar opcionalmente si se le permite responder a una revisión determinada mediante el método [Get Response info for App Reviews](get-response-info-for-app-reviews.md) .

> [!NOTE]
> Además de usar este método para responder mediante programación a las revisiones, puede responder de forma alternativa a las revisiones [mediante el centro de Partners](../publish/respond-to-customer-reviews.md).

## <a name="prerequisites"></a>Prerrequisitos

Para usar este método, primero debes hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](respond-to-reviews-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Reviews.
* [Obtén un token de acceso de Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtenga los identificadores de las revisiones a las que desea responder. Los identificadores de revisión están disponibles en los datos de respuesta del método [Get App Reviews](get-app-reviews.md) de la API de Microsoft Store Analytics y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [Informe de revisiones](../publish/reviews-report.md).

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

Este método no tiene parámetros de solicitud.


### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes valores.

| Value        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------|
| Respuestas | array | Matriz de objetos que contiene los datos de respuesta que se van a enviar. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente. |


Cada objeto de la matriz de *respuestas* contiene los siguientes valores.

| Value        | Tipo   | Descripción           |  Obligatorio  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | string |  El identificador de la tienda de la aplicación con la revisión a la que desea responder. El identificador de almacén está disponible en la [Página identidad](../publish/view-app-identity-details.md) de la aplicación del centro de Partners. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8.   |  Sí  |
| ReviewId | string |  IDENTIFICADOR de la revisión a la que se desea responder (es un GUID). Los identificadores de revisión están disponibles en los datos de respuesta del método [Get App Reviews](get-app-reviews.md) de la API de Microsoft Store Analytics y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [Informe de revisiones](../publish/reviews-report.md).   |  Sí  |
| ResponseText | string | Respuesta que se desea enviar. La respuesta debe seguir [estas instrucciones](../publish/respond-to-customer-reviews.md#guidelines-for-responses).   |  Sí  |
| SupportEmail | string | La dirección de correo electrónico de soporte técnico de la aplicación, que el cliente puede usar para ponerse en contacto con usted directamente. Debe ser una dirección de correo electrónico válida.     |  Sí  |
| IsPublic | Boolean |  Si especifica **true**, la respuesta se mostrará en la lista de la tienda de la aplicación, directamente debajo de la revisión del cliente, y será visible para todos los clientes. Si especifica **false** y el usuario no ha optado por recibir respuestas de correo electrónico, la respuesta se enviará al cliente por correo electrónico y no será visible para otros clientes de la lista de tiendas de la aplicación. Si especifica **false** y el usuario ha optado por no recibir respuestas de correo electrónico, se devolverá un error.   |  Sí  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra cómo utilizar este método para enviar respuestas a varias revisiones.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "Responses": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "ResponseText": "Thank you for pointing out this bug. I fixed it and published an update, you should have the fix soon",
      "SupportEmail": "support@contoso.com",
      "IsPublic": true
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": false
    }
  ]
}
```

## <a name="response"></a>Response

### <a name="response-body"></a>Response body

| Value        | Tipo   | Descripción            |
|---------------|--------|---------------------|
| Resultado | array | Matriz de objetos que contienen datos sobre cada respuesta enviada. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente.  |


Cada objeto de la matriz de *resultados* contiene los siguientes valores.

| Value        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | string |  El identificador de la tienda de la aplicación con la revisión a la que ha respondido. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8.   |
| ReviewId | string |  El identificador de la revisión a la que ha respondido. Este identificador es un GUID.   |
| Correcto | string | El valor **true** indica que la respuesta se envió correctamente. El valor **false** indica que la respuesta no se ha realizado correctamente.    |
| FailureReason | string | Si el **resultado** es **false**, este valor contiene una razón para el error. Si es **true**, este **valor está vacío** .      |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Result": [
    {
      "ApplicationId": "9WZDNCRFJ3Q8",
      "ReviewId": "6be543ff-1c9c-4534-aced-af8b4fbe0316",
      "Successful": "true",
      "FailureReason": ""
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "Successful": "false",
      "FailureReason": "No Permission"
    }
  ]
}
```

## <a name="related-topics"></a>Temas relacionados

* [Responder a las opiniones de los clientes mediante el centro de Partners](../publish/respond-to-customer-reviews.md)
* [Respuesta a las revisiones mediante servicios de Microsoft Store](respond-to-reviews-using-windows-store-services.md)
* [Obtención de información de respuesta para las revisiones de la aplicación](get-response-info-for-app-reviews.md)
* [Obtener las opiniones de la aplicación](get-app-reviews.md)
