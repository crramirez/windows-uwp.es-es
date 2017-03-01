---
author: mcleanbyron
ms.assetid: 038903d6-efab-4da6-96b5-046c7431e6e7
description: "Usa este método en la API de opiniones de la Tienda Windows para enviar respuestas a las opiniones sobre tu aplicación."
title: "Envío de respuestas a las opiniones sobre la aplicación"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, servicios de la Tienda, API de opiniones de la Tienda Windows, adquisiciones de complementos
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 1531059831b4c20d11661eb87fceda7b8dcb7f02
ms.lasthandoff: 02/08/2017

---

# <a name="submit-responses-to-app-reviews"></a>Envío de respuestas a las opiniones sobre la aplicación


Usa este método en la API de opiniones de la Tienda Windows para responder mediante programación a las opiniones sobre tu aplicación. Cuando se llama a este método, es necesario especificar los identificadores de las opiniones a las que quieres responder. Los identificadores de las opiniones están disponibles en los datos de respuesta del método de [obtención de las opiniones de la aplicación](get-app-reviews.md) en la API de análisis de la tienda Windows y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [informe de opiniones](../publish/reviews-report.md).

Cuando un cliente envía una opinión, puede elegir no recibir respuestas a dicha opinión. Si se intenta responder a una opinión para la que el cliente ha elegido no recibir respuestas, el cuerpo de la respuesta de este método indicará que el intento de respuesta no ha tenido éxito. Antes de llamar a este método, tienes la opción de determinar si puedes responder a una opinión determinada mediante el método [obtener información sobre la respuesta a las opiniones sobre la aplicación](get-response-info-for-app-reviews.md).

>**Nota**&nbsp;&nbsp;Además de usar este método para responder mediante programación a las opiniones, tienes la alternativa de responder a las opiniones [mediante el panel del Centro de desarrollo de Windows](../publish/respond-to-customer-reviews.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](respond-to-reviews-using-windows-store-services.md#prerequisites) para la API de opiniones de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](respond-to-reviews-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Tras obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.
* Obtén los identificadores de las opiniones a las que quieres responder. Los identificadores de las opiniones están disponibles en los datos de respuesta del método de [obtención de las opiniones de la aplicación](get-app-reviews.md) en la API de análisis de la tienda Windows y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [informe de opiniones](../publish/reviews-report.md).

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/reviews/responses``` |

<span/> 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Obligatorio. El token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Parámetros de la solicitud

Este método no tiene parámetros de la solicitud.

<span/> 

### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de la solicitud tiene los siguientes valores.

| Valor        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------|
| Responses | Matriz | Una matriz de objetos que contiene los datos de la respuesta que quieras enviar. Para obtener más información sobre los datos de cada objeto, consulta la tabla siguiente. |

Cada objeto de la matriz *Responses* contiene los siguientes valores.

| Valor        | Tipo   | Descripción           |  Obligatorio  |
|---------------|--------|-----------------------------|-----|
| ApplicationId | Cadena |  El id. de la Tienda de la aplicación que contiene la opinión a la que deseas responder. El id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8.   |  Sí  |
| ReviewId | Cadena |  El id. de la opinión a la que deseas responder (es un GUID). Los identificadores de las opiniones están disponibles en los datos de respuesta del método de [obtención de las opiniones de la aplicación](get-app-reviews.md) en la API de análisis de la tienda Windows y en la [descarga sin conexión](../publish/download-analytic-reports.md) del [informe de opiniones](../publish/reviews-report.md).   |  Sí  |
| ResponseText | Cadena | La respuesta que quieres enviar. La respuesta debe seguir [estas directrices](../publish/respond-to-customer-reviews.md#guidelines-for-responses).   |  Sí  |
| SupportEmail | Cadena | La dirección de correo electrónico del soporte técnico de la aplicación, que el cliente puede usar para ponerse en contacto contigo directamente. Debe ser una dirección de correo electrónico válida.     |  Sí  |
| IsPublic | Booleano |  El valor **true** indica que la respuesta se mostrará en descripción de la Tienda de la aplicación, justo debajo de la opinión del cliente, y será visible para todos los clientes. El valor **false** indica que la respuesta se enviará al cliente por correo electrónico y no será visible para los demás clientes en descripción de la Tienda de la aplicación.     |  Sí  |

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra cómo usar este método para enviar respuestas a diversas opiniones.

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
      "IsPublic": "true"
    },
    {
      "ApplicationId": "9NBLGGH1RP08",
      "ReviewId": "80c9671a-96c2-4278-bcbc-be0ce5a32a7c",
      "ResponseText": "Thank you for submitting your review. Can you tell more about what you were doing in the app when it froze? Thanks very much for your help.",
      "SupportEmail": "support@contoso.com",
      "IsPublic": "false"
    }
  ]
}
```

## <a name="response"></a>Respuesta

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor        | Tipo   | Descripción            |
|---------------|--------|---------------------|
| Result | Matriz | Una matriz de objetos que contienen datos sobre cada respuesta que has enviado. Para obtener más información sobre los datos de cada objeto, consulta la tabla siguiente.  |

Cada objeto de la matriz *Result* contiene los siguientes valores.

| Valor        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------|
| ApplicationId | Cadena |  El id. de la Tienda de la aplicación que contiene la opinión a la que has respondido. Un ejemplo de un id. de la Tienda sería 9WZDNCRFJ3Q8.   |
| ReviewId | Cadena |  El identificador de la opinión a la que has respondido. Se trata de un GUID.   |
| Successful | Cadena | El valor **true** indica que la respuesta se envió correctamente. El valor **false** indica que la respuesta no tuvo éxito.    |
| FailureReason | Cadena | Si **Successful** es **false**, este valor contiene un motivo del error. Si **Successful** es **true**, este valor está vacío.      |

### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo para esta solicitud.

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

* [Responder a las opiniones de los clientes mediante el panel del Centro de desarrollo](../publish/respond-to-customer-reviews.md)
* [Responder a las opiniones mediante servicios de la Tienda de Windows](respond-to-reviews-using-windows-store-services.md)
* [Obtención de información acerca de las respuesta a las opiniones sobre la aplicación](get-response-info-for-app-reviews.md)
* [Obtener las opiniones de la aplicación](get-app-reviews.md)

