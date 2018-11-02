---
author: Xansky
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Usa este método en la API de envío de Microsoft Store para recuperar información del paquete piloto para una aplicación que está registrada en tu cuenta del centro de partners.
title: Obtener paquetes piloto para una aplicación
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store submission API, API de envío de Microsoft Store, flights, pilotos, package flights, paquetes piloto
ms.localizationpriority: medium
ms.openlocfilehash: f67bb76e1d964dd246be16870a7c76591eb1e7d6
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5974276"
---
# <a name="get-package-flights-for-an-app"></a>Obtener paquetes piloto para una aplicación

Usa este método en la API de envío de Microsoft Store para enumerar los paquetes piloto para una aplicación que está registrada en tu cuenta del centro de partners. Para obtener más información acerca de los paquetes piloto, consulta [Paquetes piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) para la API de envío de Microsoft Store.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

|  Nombre  |  Tipo  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  applicationId  |  cadena  |  El identificador de la Store de la aplicación para la que quieres recuperar paquetes piloto. Para obtener más información sobre el Id. de la Store, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Sí  |
|  top  |  entero  |  Número de elementos que se devolverán en la solicitud (es decir, el número de paquetes piloto que se devolverán). Si los paquetes piloto de tu cuenta superan el valor especificado en la consulta, el cuerpo de la respuesta incluirá una ruta de acceso al URI relativa que puedes anexar al URI del método para solicitar la siguiente página de datos.  |  No  |
|  skip  |  entero  |  El número de elementos que se omitirán en la consulta antes de devolver los elementos restantes. Usa este parámetro para consultar conjuntos de datos. Por ejemplo, top = 10 y skip = 0 recuperan los elementos del 1 al 10, mientras que top = 10 y skip = 10 recuperan los elementos del 11 al 20, y así sucesivamente.  |  No  |


### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo enumerar todos los paquetes piloto de una aplicación.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra cómo enumerar el primer paquete piloto de una aplicación.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de respuesta JSON que devuelve una solicitud correcta para el primer paquete piloto de una aplicación con un total de tres paquetes piloto. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta la siguiente sección.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción       |
|------------|--------|---------------------|
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene una ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para solicitar la siguiente página de datos. Por ejemplo, si el parámetro *top* del cuerpo de la solicitud inicial se establece en 2, pero hay 4 paquetes piloto para la aplicación, el cuerpo de la respuesta incluirá un valor @nextLink de ```applications/{applicationid}/listflights/?skip=2&top=2```, lo que indica que puedes llamar a ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2``` para solicitar los 2 paquetes piloto siguientes. |
| value      | matriz  | Una matriz de objetos que proporcionan información acerca de los paquetes piloto de la aplicación especificada. Para obtener más información sobre los datos de cada objeto, consulta [Recurso de piloto](get-app-data.md#flight-object).               |
| totalCount | entero    | El número total de filas del resultado de datos de la consulta (es decir, el número total de paquetes piloto de la aplicación especificada).   |


## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se encontró ningún paquete piloto. |
| 409  | La aplicación usa una función de centro de partners que [actualmente no es compatible con la API de envío de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener una aplicación](get-an-app.md)
* [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)
