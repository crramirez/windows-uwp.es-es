---
author: Xansky
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Usa este método en la API de promociones de Microsoft Store para crear, editar y obtener campañas de anuncios promocionales.
title: Administrar campañas de anuncios
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store promotions API, API de promociones de Microsoft Store, ad campaigns, campañas de anuncios
ms.localizationpriority: medium
ms.openlocfilehash: 6c86c0d5d1a10442c7addeed11cdbfc37846f337
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7288161"
---
# <a name="manage-ad-campaigns"></a>Administrar campañas de anuncios

Usa estos métodos en la [API de promociones de Microsoft Store](run-ad-campaigns-using-windows-store-services.md) para crear, editar y obtener campañas de anuncios promocionales para tu aplicación. Cada campaña que crees con este método puede asociarse con solo una aplicación.

>**Nota**&nbsp;&nbsp;también puede crear y administrar las campañas publicitarias mediante el centro de partners y se pueden tener acceso a las campañas que crees mediante programación en el centro de partners. Para obtener más información acerca de cómo administrar las campañas de anuncios del centro de partners, consulta [crear una campaña publicitaria para tu aplicación](../publish/create-an-ad-campaign-for-your-app.md).

Cuando usas estos métodos para crear o actualizar una campaña, normalmente también llamas a uno o varios de los siguientes métodos para administrar las *líneas de entrega*, los *perfiles de destino* y los *creativos* asociados a la campaña. Para obtener más información sobre la relación entre las campañas, las líneas de entrega, los perfiles de destino y los creativos, consulta [Ejecutar campañas de anuncios con servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

* [Administrar líneas de entrega de campañas de anuncios](manage-delivery-lines-for-ad-campaigns.md)
* [Administración de los perfiles de selección de destino de las campañas publicitarias](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos de campañas de anuncios](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>Requisitos previos

Para usar estos métodos, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](run-ad-campaigns-using-windows-store-services.md#prerequisites) de la API de promociones de Microsoft Store.

  >**Nota**&nbsp;&nbsp;como parte de los requisitos previos, asegúrate de que puedes [crear al menos una campaña de anuncios de pago del centro de partners](../publish/create-an-ad-campaign-for-your-app.md) y que agregar al menos un instrumento de pago para la campaña publicitaria en el centro de partners. Las líneas de entrega para campañas de anuncios que creas con esta API facturarán siempre automáticamente al instrumento de pago elegido en la página de **las campañas de anuncios** en el centro de partners.

* [Obtén un token de acceso de Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para estos métodos. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.


## <a name="request"></a>Solicitud

Estos métodos tienen los siguientes URI.

| Tipo de método | URI de la solicitud                                                      |  Descripción  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Crea una campaña de anuncios nueva.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Edita la campaña de anuncios especificada por *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Obtiene la campaña de anuncios especificada por *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Consultas de campañas de anuncios. Consulta la sección [Parámetros](#parameters) para obtener los parámetros de consulta admitidos.  |


### <a name="header"></a>Encabezado

| Encabezado        | Tipo   | Descripción         |
|---------------|--------|---------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |
| Id. de seguimiento   | GUID   | Opcional. Un id. que realiza un seguimiento del flujo de llamadas.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>Parámetros

El método GET para consultar campañas de anuncios admite los siguientes parámetros de consulta opcionales.

| Nombre        | Tipo   |  Descripción      |    
|-------------|--------|---------------|------|
| skip  |  entero   | Número de filas que se omiten en la consulta. Usa este parámetro para consultar conjuntos de datos. Por ejemplo, los valores fetch=10 y skip=0 recuperan las primeras 10 filas de datos, los valores top=10 y skip=10 recuperan las siguientes 10 filas de datos, y así sucesivamente.    |       
| fetch  |  entero   | Número de filas de datos que se devuelven en la solicitud.    |       
| campaignSetSortColumn  |  cadena   | Ordena los objetos de [campaña](#campaign) en el cuerpo de la respuesta por el campo especificado. La sintaxis es <em>CampaignSetSortColumn=field</em>, donde el parámetro <em>field</em> puede ser una de las siguientes cadenas:</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>El valor predeterminado es **createdDateTime**.     |     
| isDescending  |  Booleano   | Ordena los objetos de [campaña](#campaign) en el cuerpo de la respuesta en orden ascendente o descendente.   |         
| storeProductId  |  cadena   | Usa este valor para devolver solo las campañas de anuncios que están asociadas a la aplicación con el [id. de la Store](in-app-purchases-and-trials.md#store-ids) especificado. Un ejemplo de id. de la Store para un producto es 9nblggh42cfd.   |         
| label  |  cadena   | Usa este valor para devolver solo las campañas de anuncios que incluyen el campo *label* especificado en el objeto de [campaña](#campaign).    |       |    


### <a name="request-body"></a>Cuerpo de la solicitud

Los métodos POST y PUT necesitan un cuerpo de la solicitud JSON con los campos obligatorios de un objeto de [campaña](#campaign) y los campos adicionales que quieras establecer o cambiar.


### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo llamar al método POST para crear una campaña de anuncios.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

En el siguiente ejemplo se muestra cómo llamar al método GET para recuperar una campaña de anuncios específica.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra cómo llamar al método GET para consultar un conjunto de campañas de anuncios, ordenado por la fecha de creación.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Respuesta

Estos métodos devuelven un cuerpo de respuesta JSON con uno o varios objetos de [campaña](#campaign), en función del método al que llamaste. En el siguiente ejemplo se muestra un cuerpo de la respuesta para el método GET de una campaña específica.

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>Objeto de campañas

Los cuerpos de solicitud y respuesta para estos métodos contienen los siguientes campos. En esta tabla se muestran los campos que son de solo lectura (es decir, no se pueden cambiar en el método PUT) y los campos que son obligatorios en el cuerpo de la solicitud para el método POST.

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Valor predeterminado  | Obligatorio para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número entero   |  El id. de la campaña de anuncios.     |   Sí    |      |  No     |       
|  name   |  cadena   |   El nombre de la campaña de anuncios.    |    No   |      |  Sí     |       
|  configuredStatus   |  cadena   |  Uno de los valores siguientes que especifica el estado de la campaña de anuncios que especifica el desarrollador: <ul><li>**Activo**</li><li>**Inactivo**</li></ul>     |  No     |  Activo    |   Sí    |       
|  effectiveStatus   |  cadena   |   Uno de los siguientes valores que especifica el estado efectivo de la campaña de anuncios en función de la validación del sistema: <ul><li>**Activo**</li><li>**Inactivo**</li><li>**En proceso**</li></ul>    |    Sí   |      |   No      |       
|  effectiveStatusReasons   |  matriz   |  Uno o varios de los valores siguientes que especifican el motivo del estado efectivo de la campaña de anuncios: <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**Failed**</li></ul>      |  Sí     |     |    No     |       
|  storeProductId   |  cadena   |  El [id. de la Store](in-app-purchases-and-trials.md#store-ids) de la aplicación a la que está asociada esta campaña de anuncios. Un ejemplo de id. de la Store para un producto es 9nblggh42cfd.     |   Sí    |      |  Sí     |       
|  labels   |  matriz   |   Una o varias cadenas que representan etiquetas personalizadas para la campaña. Estas etiquetas pueden usarse para buscar y etiquetar campañas.    |   No    |  nulo    |    No     |       
|  type   | cadena    |  Uno de los siguientes valores que especifica el tipo de campaña: <ul><li>**De pago**</li><li>**Interna**</li><li>**Comunidad**</li></ul>      |   Sí    |      |   Sí    |       
|  objective   |  cadena   |  Uno de los siguientes valores que especifica el objetivo de la campaña: <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   No    |  DriveInstall    |   Sí    |       
|  lines   |  matriz   |   Uno o varios objetos que identifican las [líneas de entrega](manage-delivery-lines-for-ad-campaigns.md#line) asociados a la campaña de anuncios. Cada objeto de este campo consiste en un campo *id* y *name* que especifica el id. y el nombre de la línea de entrega.     |   No    |      |    No     |       
|  createdDate   |  cadena   |  La fecha y hora en que se creó la campaña de anuncios, en formato ISO 8601.     |  Sí     |      |     No    |       |


## <a name="related-topics"></a>Temas relacionados

* [Ejecutar campañas de anuncios con los servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Administrar líneas de entrega de campañas de anuncios](manage-delivery-lines-for-ad-campaigns.md)
* [Administración de los perfiles de selección de destino de las campañas publicitarias](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos de campañas de anuncios](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)
