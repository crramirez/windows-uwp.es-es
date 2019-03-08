---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Usa este método en la API de promociones de Microsoft Store para administrar las líneas de entrega para las campañas de anuncios promocionales.
title: Administrar las líneas de entrega
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store promotions API, API de promociones de Microsoft Store, ad campaigns, campañas de anuncios
ms.localizationpriority: medium
ms.openlocfilehash: 363f7034d7e353d9ee110637971e7b848dbca1bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625640"
---
# <a name="manage-delivery-lines"></a>Administrar las líneas de entrega

Usa estos métodos en la API de promociones de Microsoft Store para crear una o varias *líneas de entrega* para comprar inventario y entregar tus anuncios para una campaña de anuncios promocionales. Para cada línea de entrega, puedes establecer la selección del destino, el precio de oferta y decidir cuánto quieres gastar. Para ello, establece un presupuesto y vincular los creativos que quieres usar.

Para obtener más información sobre la relación entre las líneas de entrega y las campañas de anuncios, los perfiles objetivo y los creativos, consulta [Ejecutar campañas de anuncios con los servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Tenga en cuenta**&nbsp;&nbsp;antes de poder crear correctamente las líneas de entrega para campañas de anuncios mediante esta API, primero debe [crear una campaña de ad de pago mediante la **campañas publicitarias** página Centro de partners](../publish/create-an-ad-campaign-for-your-app.md), y debe agregar al menos un instrumento de pago de esta página. Después de hacer esto, podrás crear correctamente líneas de entrega facturables para campañas de anuncios con esta API. Crear mediante la API de campañas de anuncios automáticamente le facturará el instrumento de pago predeterminado seleccionado en el **campañas publicitarias** página en el centro de partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar estos métodos, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](run-ad-campaigns-using-windows-store-services.md#prerequisites) de la API de promociones de Microsoft Store.

  > [!NOTE]
  > Como parte de los requisitos previos, asegúrese de que usted [Crear campaña de publicidad de pago al menos un centro de partners](../publish/create-an-ad-campaign-for-your-app.md) y agregar al menos un instrumento de pago de la campaña de anuncio en el centro de partners. Las líneas de entrega que se crea mediante esta API le facturará automáticamente el instrumento de pago predeterminado seleccionado en el **campañas publicitarias** página en el centro de partners.

* [Obtén un token de acceso de Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para estos métodos. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Estos métodos tienen los siguientes URI.

| Tipo de método | URI de la solicitud         |  Descripción  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  Crea una nueva línea de entrega.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Edita la línea de entrega especificada por *lineId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  Obtiene la línea de entrega especificada por *lineId*.  |


### <a name="header"></a>Encabezado

| Encabezado        | Tipo   | Descripción         |
|---------------|--------|---------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |
| Id. de seguimiento   | GUID   | Opcional. Un id. que realiza un seguimiento del flujo de llamadas.                                  |


### <a name="request-body"></a>Cuerpo de la solicitud

Los métodos POST y PUT necesitan un cuerpo de la solicitud JSON con los campos obligatorios de un objeto de [línea de entrega](#line) y los campos adicionales que quieras establecer o cambiar.


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

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Predeterminado  | Obligatorio para POST o PUT |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número entero   |  El id. de la línea de entrega.     |   Sí    |      |  No      |    
|  name   |  string   |   El nombre de la línea de entrega.    |    No   |      |  POST     |     
|  configuredStatus   |  string   |  Uno de los valores siguientes que especifica el estado de la línea de entrega que especifica el desarrollador: <ul><li>**Active**</li><li>**inactivo**</li></ul>     |  No     |      |   POST    |       
|  effectiveStatus   |  string   |   Uno de los siguientes valores que especifica el estado efectivo de la línea de entrega en función de la validación del sistema: <ul><li>**Active**</li><li>**inactivo**</li><li>**Procesamiento**</li><li>**Error**</li></ul>    |    Sí   |      |  No      |      
|  effectiveStatusReasons   |  array   |  Uno o varios de los valores siguientes que especifican el motivo del estado efectivo de la línea de entrega: <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Sí     |     |    No    |           
|  startDatetime   |  string   |  La fecha y hora de inicio de la línea de entrega, en formato ISO 8601. No se puede cambiar este valor si ya es pasado.     |    No   |      |    POST, PUT     |
|  endDatetime   |  string   |  La fecha y hora de finalización de la línea de entrega, en formato ISO 8601. No se puede cambiar este valor si ya es pasado.     |   No    |      |  POST, PUT     |
|  createdDatetime   |  string   |  La fecha y hora en la que se creó la línea de entrega, en formato ISO 8601.      |    Sí   |      |  No      |
|  bidType   |  string   |  Un valor que especifica el tipo de oferta de la línea de entrega. Actualmente, el único valor admitido es **CPM**.      |    No   |  CPM    |  No     |
|  bidAmount   |  decimal   |  La cantidad de oferta que se usará para realizar ofertas en cualquier solicitud de anuncio.      |    No   |  El valor medio de CPM en función de los mercados de destino (este valor se revisa periódicamente).    |    No    |  
|  dailyBudget   |  decimal   |  El presupuesto diario de la línea de entrega. Se debe establecer *dailyBudget* o *lifetimeBudget*.      |    No   |      |   POST, PUT (si no se estableció *lifetimeBudget*)       |
|  lifetimeBudget   |  decimal   |   El presupuesto de duración de la línea de entrega. Se debe establecer lifetimeBudget* o *dailyBudget*.      |    No   |      |   POST, PUT (si no se estableció *dailyBudget*)    |
|  targetingProfileId   |  object   |  Un objeto que identifica el [perfil de destino](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) que describe los usuarios, las regiones y los tipos de inventario que quieres como destino para esta línea de entrega. Este objeto consiste en un solo campo *id* que especifica el id. del perfil de destino.     |    No   |      |  No      |  
|  creatives   |  array   |  Uno o varios objetos que representan los [creativos](manage-creatives-for-ad-campaigns.md#creative) asociados a la línea de entrega. Cada objeto de este campo consiste en un solo campo *id* que especifica el id. de un creativo.      |    No   |      |   No     |  
|  campaignId   |  número entero   |  El id. de la campaña de anuncios principal.      |    No   |      |   No     |  
|  minMinutesPerImp   |  número entero   |  Especifica el intervalo de tiempo mínimo (en minutos) entre dos impresiones que se muestran al mismo usuario desde esta línea de entrega.      |    No   |  4000    |  No      |  
|  pacingType   |  string   |  Uno de los valores siguientes que especifican el tipo de ritmo: <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    No   |  SpendEvenly    |  No      |
|  currencyId   |  número entero   |  El id. de la moneda de la campaña.      |    Sí   |  La moneda de la cuenta de desarrollador (no es necesario especificar este campo en las llamadas POST o PUT)    |   No     |      |


## <a name="related-topics"></a>Temas relacionados

* [Ejecutar campañas de anuncios mediante servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Administrar campañas de anuncios](manage-ad-campaigns.md)
* [Administrar perfiles de destinatarios para campañas de anuncios](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar la creatividad para campañas de anuncios](manage-creatives-for-ad-campaigns.md)
* [Obtener datos de rendimiento de campaña de anuncio](get-ad-campaign-performance-data.md)
