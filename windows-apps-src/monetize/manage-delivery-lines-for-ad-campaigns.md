---
author: mcleanbyron
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: "Usa este método en la API de promociones de la Tienda Windows para administrar las líneas de entrega para las campañas de anuncios promocionales."
title: "Administrar líneas de entrega de campañas de anuncios"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, API de promociones de la Tienda Windows, campañas de anuncios, Windows Store promotions API, ad campaigns"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 5f9ede6a2e645a644e4650f3af7e5476bd52dd53
ms.lasthandoff: 02/08/2017

---

# <a name="manage-delivery-lines-for-ad-campaigns"></a>Administrar líneas de entrega de campañas de anuncios

Usa estos métodos en la API de promociones de la Tienda Windows para crear una o varias *líneas de entrega* para comprar inventario y entregar tus anuncios para una campaña de anuncios promocionales. Para cada línea de entrega, puedes establecer la selección del destino, el precio de oferta y decidir cuánto quieres gastar. Para ello, establece un presupuesto y vincular los creativos que quieres usar.

Para obtener más información sobre la relación entre las líneas de entrega y las campañas de anuncios, los perfiles de destino y los creativos, consulta [Run ad campaigns using Windows Store services (Ejecutar campañas de anuncios con servicios de la Tienda Windows)](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Requisitos previos

Para usar estos métodos, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](run-ad-campaigns-using-windows-store-services.md#prerequisites) de la API de promociones de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para estos métodos. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Estos métodos tienen los siguientes URI.

| Tipo de método | URI de la solicitud         |  Descripción  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Crea una nueva línea de entrega.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Edita la línea de entrega especificada por *lineId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Obtiene la línea de entrega especificada por *lineId*.  |

<span/> 
### <a name="header"></a>Encabezado

| Encabezado        | Tipo   | Descripción         |
|---------------|--------|---------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |
| Id. de seguimiento   | GUID   | Opcional. Un id. que realiza un seguimiento del flujo de llamadas.                                  |

<span/>
### <a name="request-body"></a>Cuerpo de la solicitud

Los métodos POST y PUT necesitan un cuerpo de la solicitud JSON con los campos obligatorios de un objeto de [línea de entrega](#line) y los campos adicionales que quieras establecer o cambiar.
 
<span/>
### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo llamar al método POST para crear una línea de entrega.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

En el siguiente ejemplo se muestra cómo llamar al método GET para recuperar una línea de entrega.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/> 
## <a name="response"></a>Respuesta

Estos métodos devuelven un cuerpo de respuesta JSON con un objeto de [línea de entrega](#line) que contiene información sobre la línea de entrega que se creó, actualizó o recuperó. En el siguiente ejemplo se muestra un cuerpo de respuesta para estos métodos.

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```

<span id="line"/>
## <a name="delivery-line-object"></a>Objeto de línea de entrega

Los cuerpos de solicitud y respuesta para estos métodos contienen los siguientes campos. En esta tabla se muestran los campos que son de solo lectura (es decir, no se pueden cambiar en el método PUT) y los campos que son obligatorios en el cuerpo de la solicitud para los métodos POST o PUT.

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Valor predeterminado  | Obligatorio para POST o PUT |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número entero   |  El id. de la línea de entrega.     |   Sí    |      |  No      |    
|  name   |  cadena   |   El nombre de la línea de entrega.    |    No   |      |  POST     |     
|  configuredStatus   |  cadena   |  Uno de los valores siguientes que especifica el estado de la línea de entrega que especifica el desarrollador: <ul><li>**Activo**</li><li>**Inactivo**</li></ul>     |  No     |      |   POST    |       
|  effectiveStatus   |  cadena   |   Uno de los siguientes valores que especifica el estado efectivo de la línea de entrega en función de la validación del sistema: <ul><li>**Activo**</li><li>**Inactivo**</li><li>**En proceso**</li><li>**Error**</li></ul>    |    Sí   |      |  No      |      
|  effectiveStatusReasons   |  matriz   |  Uno o varios de los valores siguientes que especifican el motivo del estado efectivo de la línea de entrega: <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Sí     |     |    No    |           
|  startDatetime   |  cadena   |  La fecha y hora de inicio de la línea de entrega, en formato ISO 8601. No se puede cambiar este valor si ya es pasado.     |    No   |      |    POST, PUT     |
|  endDatetime   |  cadena   |  La fecha y hora de finalización de la línea de entrega, en formato ISO 8601. No se puede cambiar este valor si ya es pasado.     |   No    |      |  POST, PUT     |
|  createdDatetime   |  cadena   |  La fecha y hora en la que se creó la línea de entrega, en formato ISO 8601.      |    Sí   |      |  No      |
|  bidType   |  cadena   |  Un valor que especifica el tipo de oferta de la línea de entrega. Actualmente, el único valor admitido es **CPM**.      |    No   |  CPM    |  No     |
|  bidAmount   |  decimal   |  La cantidad de oferta que se usará para realizar ofertas en cualquier solicitud de anuncio.      |    No   |  El valor medio de CPM en función de los mercados de destino (este valor se revisa periódicamente).    |    No    |  
|  dailyBudget   |  decimal   |  El presupuesto diario de la línea de entrega. Se debe establecer *dailyBudget* o *lifetimeBudget*.      |    No   |      |   POST, PUT (si no se estableció *lifetimeBudget*)       |
|  lifetimeBudget   |  decimal   |   El presupuesto de duración de la línea de entrega. Se debe establecer lifetimeBudget* o *dailyBudget*.      |    No   |      |   POST, PUT (si no se estableció *dailyBudget*)    |
|  targetingProfileId   |  objeto   |  Un objeto que identifica el [perfil de destino](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) que describe los usuarios, las regiones y los tipos de inventario que quieres como destino para esta línea de entrega. Este objeto consiste en un solo campo *id* que especifica el id. del perfil de destino.     |    No   |      |  No      |  
|  creatives   |  matriz   |  Uno o varios objetos que representan los [creativos](manage-creatives-for-ad-campaigns.md#creative) asociados a la línea de entrega. Cada objeto de este campo consiste en un solo campo *id* que especifica el id. de un creativo.      |    No   |      |   No     |  
|  campaignId   |  número entero   |  El id. de la campaña de anuncios principal.      |    No   |      |   No     |  
|  minMinutesPerImp   |  número entero   |  Especifica el intervalo de tiempo mínimo (en minutos) entre dos impresiones que se muestran al mismo usuario desde esta línea de entrega.      |    No   |  4000    |  No      |  
|  pacingType   |  cadena   |  Uno de los valores siguientes que especifican el tipo de ritmo: <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    No   |  SpendEvenly    |  No      |
|  currencyId   |  número entero   |  El id. de la moneda de la campaña.      |    Sí   |  La moneda de la cuenta de desarrollador (no es necesario especificar este campo en las llamadas POST o PUT)    |   No     |      |


## <a name="related-topics"></a>Temas relacionados

* [Ejecutar campañas de anuncios con los servicios de la Tienda Windows](run-ad-campaigns-using-windows-store-services.md)
* [Administrar campañas de anuncios](manage-ad-campaigns.md)
* [Administrar perfiles de destino de las campañas de anuncios](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos de campañas de anuncios](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)

