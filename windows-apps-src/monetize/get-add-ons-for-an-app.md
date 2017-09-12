---
author: mcleanbyron
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: "Usa este método en la API de envío de la Tienda Windows para recuperar información sobre las compras desde la aplicación para una aplicación registrada en tu cuenta del Centro de desarrollo de Windows."
title: "Obtener complementos para una aplicación"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, Windows Store submission API, API de envío de la Tienda Windows, add-ons, complementos, in-app products, productos desde la aplicación, IAPs, IAP"
ms.openlocfilehash: 9c39bfe9754805dccf5ccb82c9834d6d4deea385
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2017
---
# <a name="get-add-ons-for-an-app"></a>Obtener complementos para una aplicación




Usa este método en la API de envío de la Tienda Windows para enumerar complementos de una aplicación que está registrada en tu cuenta del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) de la API de envío de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Este método tiene la siguiente sintaxis. Consulta las siguientes secciones para ver ejemplos de uso y descripciones tanto del encabezado como del cuerpo de la solicitud.

| Método | URI de solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` |

<span/>
 
### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span/>

### <a name="request-parameters"></a>Parámetros de solicitud


|  Nombre  |  Tipo  |  Descripción  |  Obligatorio  |
|------|------|------|------|
|  applicationId  |  cadena  |  El identificador de la Tienda de la aplicación para la que quieres recuperar los complementos. Para obtener más información sobre el Id. de la Tienda, consulta [Ver detalles de identidad de las aplicaciones](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Sí  |
|  top  |  entero  |  El número de elementos que se devolverán en la solicitud (es decir, el número de complementos que se devolverán). Si los complementos de la aplicación superan el valor especificado en la consulta, el cuerpo de la respuesta incluye una ruta de acceso al URI relativa que puedes anexar al URI del método para solicitar la siguiente página de datos.  |  No  |
|  skip |  entero  | El número de elementos que se omitirán en la consulta antes de devolver los elementos restantes. Usa este parámetro para consultar conjuntos de datos. Por ejemplo, top = 10 y skip = 0 recuperan los elementos del 1 al 10, mientras que top = 10 y skip = 10 recuperan los elementos del 11 al 20, y así sucesivamente.   |  No  |

<span/>

### <a name="request-body"></a>Cuerpo de la solicitud

No incluyas un cuerpo de la solicitud para este método.

### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo enumerar todos los complementos de una aplicación.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra cómo enumerar los 10 primeros complementos de una aplicación.

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

En el ejemplo siguiente se muestra el cuerpo de respuesta de JSON que devuelve una solicitud correcta para los 10 primeros complementos para una aplicación con un total de 53 complementos. Para mayor brevedad, en este ejemplo se muestran solo los datos de los tres primeros complementos que devuelve la solicitud. Para obtener más información acerca de los valores del cuerpo de respuesta, consulta la siguiente sección.

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene una ruta de acceso relativa que se puede anexar al URI de la solicitud de base ```https://manage.devcenter.microsoft.com/v1.0/my/``` para solicitar la siguiente página de datos. Por ejemplo, si el parámetro *top* del cuerpo de la solicitud inicial se establece en 10, pero hay 50 complementos para la aplicación, el cuerpo de la respuesta incluirá un valor @nextLink de ```applications/{applicationid}/listinappproducts/?skip=10&top=10```, lo que indica que puedes llamar a ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10``` para solicitar los 10 complementos siguientes. |
| value      | matriz  | Una matriz de objetos que enumera el Id. de la Tienda de cada complemento de la aplicación especificada. Para obtener más información sobre los datos de cada objeto, consulta [Recurso de complemento](get-app-data.md#add-on-object).                                                                                                                           |
| totalCount | entero    | El número total de filas del resultado de datos de la consulta (es decir, el número total de complementos de la aplicación especificada).                                                                                                                                                                                                                             |

<span/>

## <a name="error-codes"></a>Códigos de error

Si la solicitud no se puede completar correctamente, la respuesta contendrá uno de los siguientes códigos de error HTTP.

| Código de error |  Descripción   |
|--------|------------------|
| 404  | No se encontraron complementos. |
| 409  | Los complementos usan características de panel del Centro de desarrollo que [la API de envío de la Tienda Windows no admite actualmente](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |

<span/>

## <a name="related-topics"></a>Temas relacionados

* [Crear y administrar envíos mediante el uso de servicios de la Tienda Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Obtener todas las aplicaciones](get-all-apps.md)
* [Obtener una aplicación](get-an-app.md)
* [Obtener paquetes piloto para una aplicación](get-flights-for-an-app.md)
