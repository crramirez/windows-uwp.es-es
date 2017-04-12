---
author: mcleanbyron
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: "Usa este método en la API de envío de la Tienda Windows para recuperar información sobre todas las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows."
title: Obtener todas las aplicaciones
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store submission API, API de envío de la Tienda Windows, apps, aplicaciones"
ms.openlocfilehash: 93e973eb20835160e6b580bc932ce559b47e0ce1
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="get-all-apps"></a>Obtener todas las aplicaciones




Usa este método en la API de envío de la Tienda Windows para recuperar los datos de todas las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos.

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

>**Nota**&nbsp;&nbsp;Este método solo puede usarse para cuentas del Centro de desarrollo de Windows autorizadas para el uso de la API de envío de la Tienda Windows. No todas las cuentas tienen este permiso habilitado.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` |

<span/>
 

### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud

Todos los parámetros de solicitud son opcionales para este método. Si llamas a este método sin parámetros, la respuesta contiene datos de todas las aplicaciones registradas en tu cuenta.
 
|  Parámetro  |  Tipo  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  top  |  entero  |  El número de elementos que se devolverán en la solicitud (es decir, el número de aplicaciones que se devolverán). Si las aplicaciones de la cuenta superan el valor especificado en la consulta, el cuerpo de la respuesta incluye una ruta de acceso al URI relativa que puedes anexar al URI del método para solicitar la siguiente página de datos.  |  No  |
|  skip  |  entero  |  El número de elementos que se omitirán en la consulta antes de devolver los elementos restantes. Usa este parámetro para consultar conjuntos de datos. Por ejemplo, top = 10 y skip = 0 recuperan los elementos del 1 al 10, mientras que top = 10 y skip = 10 recuperan los elementos del 11 al 20, y así sucesivamente.  |  No  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo recuperar información sobre todas las aplicaciones registradas en tu cuenta.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra cómo recuperar las 10 primeras aplicaciones registradas en tu cuenta.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el siguiente ejemplo se muestra el cuerpo de respuesta JSON que devuelve una solicitud correcta para las 10 primeras aplicaciones registradas en una cuenta de desarrollador con un total de 21 aplicaciones. Para mayor brevedad, en este ejemplo se muestran solo los datos de las dos primeras aplicaciones que devuelve la solicitud. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta la siguiente sección.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | matriz  | Una matriz de objetos que contienen información acerca de cada aplicación registrada en tu cuenta. Para más información sobre los datos de cada objeto, consulta [Recurso de aplicación](get-app-data.md#application_object).                                                                                                                           |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene una ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para solicitar la siguiente página de datos. Por ejemplo, si el parámetro *top* del cuerpo de la solicitud inicial se establece en 10, pero hay 20 aplicaciones registradas en la cuenta, el cuerpo de la respuesta incluirá un valor @nextLink de ```applications?skip=10&top=10```, lo que indica que puedes llamar a ```https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10``` para solicitar las 10 aplicaciones siguientes. |
| totalCount | entero    | El número total de filas del resultado de datos de la consulta (es decir, el número total de aplicaciones registradas en tu cuenta).                                                                                                                                                                                                                             |

<span/>

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se encontró ninguna aplicación. |
| 409  | Las aplicaciones usan características de panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener una aplicación](get-an-app.md)
* [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md)
* [Obtener complementos para una aplicación](get-add-ons-for-an-app.md)
