---
author: Xansky
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: Usa este método en la API de promociones de Microsoft Store para administrar los creativos para las campañas de anuncios promocionales.
title: Administrar creativos
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store promotions API, API de promociones de Microsoft Store, ad campaigns, campañas de anuncios
ms.localizationpriority: medium
ms.openlocfilehash: 838329101695c21abfb7ac89dd9c83330b7bd26b
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5470531"
---
# <a name="manage-creatives"></a>Administrar creativos

Usa estos métodos en la API de promociones de Microsoft Store para cargar tus propios creativos personalizados y usarlos en las campañas de anuncios promocionales u obtener un creativo existente. Un creativo puede estar asociado a una o varias líneas de entrega, incluso entre campañas de anuncios, siempre que represente siempre a la misma aplicación.

Para obtener más información sobre la relación entre los creativos y las campañas de anuncios, las líneas de entrega y los perfiles objetivo, consulta [Ejecutar campañas de anuncios con los servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

> [!NOTE]
> Al usar esta API para cargar tu propio creativo, el tamaño máximo permitido para el creativo es 40 KB. Si envías un archivo creativo más grande, esta API no emitirá un error, pero la campaña no se creará correctamente.

## <a name="prerequisites"></a>Requisitos previos

Para usar estos métodos, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](run-ad-campaigns-using-windows-store-services.md#prerequisites) de la API de promociones de Microsoft Store.
* [Obtén un token de acceso de Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para estos métodos. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.


## <a name="request"></a>Solicitud

Estos métodos tienen los siguientes URI.

| Tipo de método | URI de la solicitud     |  Descripción  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  Crea a un nuevo creativo.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  Obtiene el creativo especificado por *creativeId*.  |

> [!NOTE]
> Esta API no admite actualmente un método PUT.


### <a name="header"></a>Encabezado

| Encabezado        | Tipo   | Descripción         |
|---------------|--------|---------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |
| Id. de seguimiento   | GUID   | Opcional. Un id. que realiza un seguimiento del flujo de llamadas.                                  |


### <a name="request-body"></a>Cuerpo de la solicitud

El método POST necesita un cuerpo de la solicitud JSON con los campos obligatorios de un objeto de [creativo](#creative).


### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo llamar al método POST para crear un creativo. En este ejemplo, el valor *content* se acortó por razones de brevedad.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

En el siguiente ejemplo se muestra cómo llamar al método GET para recuperar un creativo.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Respuesta

Estos métodos devuelven un cuerpo de respuesta JSON con un objeto de [creativo](#creative) que contiene información sobre el creativo que se creó o recuperó. En el siguiente ejemplo se muestra un cuerpo de respuesta para estos métodos. En este ejemplo, el valor *content* se acortó por razones de brevedad.

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>Objeto de creativo

Los cuerpos de solicitud y respuesta para estos métodos contienen los siguientes campos. En esta tabla se muestran los campos que son de solo lectura (es decir, no se pueden cambiar en el método PUT) y los campos que son obligatorios en el cuerpo de la solicitud para el método POST.

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Valor predeterminado  |  Obligatorio para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número entero   |  El id. del creativo.     |   Sí    |      |    No   |       
|  name   |  cadena   |   El nombre del creativo.    |    No   |      |  Sí     |       
|  content   |  cadena   |  El contenido de la imagen de creativo, en formato codificado en Base64.<br/><br/>**Nota**&nbsp;&nbsp;El tamaño máximo permitido para el creativo es 40 KB. Si envías un archivo creativo más grande, esta API no emitirá un error, pero la campaña no se creará correctamente.     |  No     |      |   Sí    |       
|  height   |  entero   |   La altura del creativo.    |    No    |      |   Sí    |       
|  width   |  entero   |  El ancho del creativo.     |  No    |     |    Sí   |       
|  landingUrl   |  cadena   |  Si usas un servicio de seguimiento de campañas como Kochava, AppsFlyer o Tune para medir el análisis de instalación de la aplicación, asigna la dirección URL de seguimiento en este campo al llamar el método POST (si se especifica, este valor debe ser un URI válido). Si no usas un servicio de seguimiento de campañas, omite este valor al llamar al método (en este caso, esta dirección URL se creará automáticamente).   |  No    |     |   Sí    |       
|  format   |  cadena   |   El formato del anuncio. Actualmente, el único valor admitido es **Banner**.    |   No    |  Pancarta   |  No     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   Proporciona los atributos del creativo.     |   No    |      |   Sí    |       
|  storeProductId   |  cadena   |   El [id. de la Store](in-app-purchases-and-trials.md#store-ids) de la aplicación a la que está asociada esta campaña de anuncios. Un ejemplo de id. de la Store para un producto es 9nblggh42cfd.    |   No    |    |  No     |   |  


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>Objeto ImageAttributes

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Valor predeterminado  | Obligatorio para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   cadena  |   Uno de los siguientes valores: **PNG** o **JPG**.    |    No   |      |   Sí    |       |


## <a name="related-topics"></a>Temas relacionados

* [Ejecutar campañas de anuncios con los servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Administrar campañas de anuncios](manage-ad-campaigns.md)
* [Administración de las líneas de entrega de las campañas publicitarias](manage-delivery-lines-for-ad-campaigns.md)
* [Administrar perfiles de destino de las campañas de anuncios](manage-targeting-profiles-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)
