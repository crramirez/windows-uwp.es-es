---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: Use este método en la API de promociones de Microsoft Store para administrar las líneas de entrega de campañas publicitarias promocionales.
title: Administrar líneas de entrega
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, API de promociones de Microsoft Store, campañas de ad
ms.localizationpriority: medium
ms.openlocfilehash: e7b370d8eea61033092d833cdf751f1dade86c6d
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878576"
---
# <a name="manage-delivery-lines"></a>Administrar líneas de entrega

Use estos métodos en la API de promociones de Microsoft Store para crear una o varias *líneas de entrega* con el fin de comprar inventario y entregar anuncios para una campaña publicitaria de promoción. Para cada línea de entrega, puede establecer destinatarios, establecer el precio de la oferta y decidir cuánto desea gastar si establece un presupuesto y un vínculo a los creativos que quiere usar.

Para obtener más información acerca de la relación entre las líneas de entrega y las campañas de anuncios, los perfiles de destino y los creativos, consulte [ejecución de campañas de ad con Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

>**Nota:** &nbsp; &nbsp; Antes de poder crear correctamente líneas de entrega para campañas de ad mediante esta API, primero debe [crear una campaña de pago de pago mediante la página **campañas de ad** del centro de Partners](./index.md), y debe agregar al menos un instrumento de pago en esta página. Después de hacerlo, podrá crear correctamente líneas de entrega facturable para campañas de ad mediante esta API. Las campañas de ad que cree con la API facturarán automáticamente el instrumento de pago predeterminado elegido en la página de **campañas de ad** del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar estos métodos, primero debe hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](run-ad-campaigns-using-windows-store-services.md#prerequisites) de la API de promociones de Microsoft Store.

  > [!NOTE]
  > Como parte de los requisitos previos, asegúrese de [crear al menos una campaña de anuncios de pago en el centro de Partners](./index.md) y agregar al menos un instrumento de pago para la campaña de AD en el centro de Partners. Las líneas de entrega que cree con esta API facturarán automáticamente el instrumento de pago predeterminado elegido en la página de **campañas de ad** del centro de Partners.

* [Obtenga un token de acceso Azure ad](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de solicitud para estos métodos. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

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
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |
| Tracking ID   | GUID   | Opcional. IDENTIFICADOR que realiza el seguimiento del flujo de llamadas.                                  |


### <a name="request-body"></a>Cuerpo de la solicitud

Los métodos POST y PUT requieren un cuerpo de solicitud JSON con los campos obligatorios de un objeto de [línea de entrega](#line) y cualquier campo adicional que desee establecer o cambiar.


### <a name="request-examples"></a>Ejemplos de solicitud

En el ejemplo siguiente se muestra cómo llamar al método POST para crear una línea de entrega.

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

En el ejemplo siguiente se muestra cómo llamar al método GET para recuperar una línea de entrega.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

Estos métodos devuelven un cuerpo de respuesta JSON con un objeto de [línea de entrega](#line) que contiene información sobre la línea de entrega que se creó, actualizó o recuperó. En el ejemplo siguiente se muestra un cuerpo de respuesta para estos métodos.

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

Los cuerpos de solicitud y respuesta de estos métodos contienen los campos siguientes. En esta tabla se muestran los campos que son de solo lectura (lo que significa que no se pueden cambiar en el método PUT) y qué campos son necesarios en el cuerpo de la solicitud para los métodos POST o PUT.

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Valor predeterminado  | Obligatorio para POST/PUT |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  integer   |  IDENTIFICADOR de la línea de entrega.     |   Sí    |      |  No      |    
|  name   |  string   |   Nombre de la línea de entrega.    |    No   |      |  POST     |     
|  configuredStatus   |  string   |  Uno de los siguientes valores que especifica el estado de la línea de entrega especificada por el desarrollador: <ul><li>**Activo**</li><li>**Inactivo**</li></ul>     |  No     |      |   POST    |       
|  effectiveStatus   |  string   |   Uno de los siguientes valores que especifica el estado efectivo de la línea de entrega en función de la validación del sistema: <ul><li>**Activo**</li><li>**Inactivo**</li><li>**Procesamiento**</li><li>**Erróneo**</li></ul>    |    Sí   |      |  No      |      
|  effectiveStatusReasons   |  array   |  Uno o varios de los siguientes valores que especifican el motivo del Estado efectivo de la línea de entrega: <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  Sí     |     |    No    |           
|  startDatetime   |  string   |  Fecha y hora de inicio de la línea de entrega, en formato ISO 8601. Este valor no se puede cambiar si ya se encuentra en el pasado.     |    No   |      |    POST, PUT     |
|  Fechahorafin   |  string   |  Fecha y hora de finalización de la línea de entrega, en formato ISO 8601. Este valor no se puede cambiar si ya se encuentra en el pasado.     |   No    |      |  POST, PUT     |
|  Fecha   |  string   |  Fecha y hora de creación de la línea de entrega, en formato ISO 8601.      |    Sí   |      |  No      |
|  bidType   |  string   |  Valor que especifica el tipo de apuesta de la línea de entrega. Actualmente, el único valor admitido es **CPM**.      |    No   |  CPM    |  No     |
|  bidAmount   |  Decimal   |  El importe de la puja que se va a usar para la apuesta de cualquier solicitud de anuncio.      |    No   |  Valor de CPM promedio basado en los mercados de destino (este valor se revisa periódicamente).    |    No    |  
|  dailyBudget   |  Decimal   |  Presupuesto diario de la línea de entrega. Se debe establecer *dailyBudget* o *lifetimeBudget* .      |    No   |      |   POST, PUT (si *lifetimeBudget* no está establecido)       |
|  lifetimeBudget   |  Decimal   |   El presupuesto de duración para la línea de entrega. Debe establecerse lifetimeBudget * o *dailyBudget* .      |    No   |      |   POST, PUT (si *dailyBudget* no está establecido)    |
|  targetingProfileId   |  object   |  En el objeto que identifica el [Perfil de destino](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile) que describe los usuarios, las zonas geográficas y los tipos de inventario a los que desea dirigirse para esta línea de entrega. Este objeto está formado por un único campo de *identificador* que especifica el identificador del perfil de destino.     |    No   |      |  No      |  
|  creativos   |  array   |  Uno o más objetos que representan los [creativos](manage-creatives-for-ad-campaigns.md#creative) asociados a la línea de entrega. Cada objeto de este campo se compone de un único campo de *identificador* que especifica el identificador de una creatividad.      |    No   |      |   No     |  
|  campaignId   |  integer   |  IDENTIFICADOR de la campaña de anuncios primaria.      |    No   |      |   No     |  
|  minMinutesPerImp   |  integer   |  Especifica el intervalo de tiempo mínimo (en minutos) entre dos impresiones que se muestran al mismo usuario desde esta línea de entrega.      |    No   |  4000    |  No      |  
|  pacingType   |  string   |  Uno de los siguientes valores que especifican el tipo de ritmo: <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    No   |  SpendEvenly    |  No      |
|  currencyId   |  integer   |  IDENTIFICADOR de la moneda de la campaña.      |    Sí   |  La moneda de la cuenta de desarrollador (no es necesario especificar este campo en llamadas POST o PUT)    |   No     |      |


## <a name="related-topics"></a>Temas relacionados

* [Ejecutar campañas de ad con Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Administrar campañas publicitarias](manage-ad-campaigns.md)
* [Administrar perfiles de destino para campañas de ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos para campañas de ad](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)