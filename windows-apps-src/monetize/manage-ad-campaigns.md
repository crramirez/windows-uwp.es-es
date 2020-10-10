---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: Use este método en la API de promociones de Microsoft Store para crear, editar y obtener campañas publicitarias promocionales.
title: Administrar campañas publicitarias
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, API de promociones de Microsoft Store, campañas de ad
ms.localizationpriority: medium
ms.openlocfilehash: bf5945ca68a4ea060943cb2cc89ba907f597e4f5
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878408"
---
# <a name="manage-ad-campaigns"></a>Administrar campañas publicitarias

Use estos métodos en la [API de promociones de Microsoft Store](run-ad-campaigns-using-windows-store-services.md) para crear, editar y obtener campañas publicitarias promocionales para su aplicación. Cada campaña que se crea mediante este método solo se puede asociar a una aplicación.

>**Nota:** &nbsp; &nbsp; También puede crear y administrar campañas de ad con el centro de Partners y se puede tener acceso a las campañas creadas mediante programación en el centro de Partners. Para obtener más información acerca de la administración de campañas de AD en el centro de Partners, consulte [creación de una campaña de AD para la aplicación](./index.md).

Cuando se usan estos métodos para crear o actualizar una campaña, normalmente también se llama a uno o varios de los métodos siguientes para administrar las *líneas de entrega*, los perfiles de *destino*y los *creativos* asociados a la campaña. Para obtener más información sobre la relación entre las campañas, las líneas de entrega, los perfiles de destino y los creativos, consulte [ejecución de campañas de ad con Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

* [Administrar líneas de entrega para campañas de ad](manage-delivery-lines-for-ad-campaigns.md)
* [Administrar perfiles de destino para campañas de ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos para campañas de ad](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>Requisitos previos

Para usar estos métodos, primero debe hacer lo siguiente:

* Si todavía no lo ha hecho, complete todos los [requisitos previos](run-ad-campaigns-using-windows-store-services.md#prerequisites) de la API de promociones de Microsoft Store.

  >**Nota:** &nbsp; &nbsp; Como parte de los requisitos previos, asegúrese de [crear al menos una campaña de anuncios de pago en el centro de Partners](./index.md) y agregar al menos un instrumento de pago para la campaña de AD en el centro de Partners. Las líneas de entrega de las campañas de ad que cree con esta API facturarán automáticamente el instrumento de pago predeterminado elegido en la página **campañas de ad** del centro de Partners.

* [Obtenga un token de acceso Azure ad](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de solicitud para estos métodos. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.


## <a name="request"></a>Solicitud

Estos métodos tienen los siguientes URI.

| Tipo de método | URI de la solicitud                                                      |  Descripción  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Crea una nueva campaña de ad.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Edita la campaña de ad especificada por *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  Obtiene la campaña de ad especificada por *campaignId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  Consultas para campañas de ad. Vea la sección [parámetros](#parameters) para obtener los parámetros de consulta admitidos.  |


### <a name="header"></a>Encabezado

| Encabezado        | Tipo   | Descripción         |
|---------------|--------|---------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |
| Tracking ID   | GUID   | Opcional. IDENTIFICADOR que realiza el seguimiento del flujo de llamadas.                                  |


<span id="parameters"/> 

### <a name="parameters"></a>Parameters

El método GET para consultar las campañas de ad admite los siguientes parámetros de consulta opcionales.

| Nombre        | Tipo   |  Descripción      |    
|-------------|--------|---------------|------|
| skip  |  int   | Número de filas que se omiten en la consulta. Usa este parámetro para consultar conjuntos de datos. Por ejemplo, fetch = 10 y SKIP = 0 recupera las 10 primeras filas de datos, Top = 10 y SKIP = 10 recupera las 10 filas de datos siguientes, etc.    |       
| fetch  |  int   | Número de filas de datos que se devuelven en la solicitud.    |       
| campaignSetSortColumn  |  string   | Ordena los objetos de [campaña](#campaign) en el cuerpo de la respuesta por el campo especificado. La sintaxis es <em>CampaignSetSortColumn = Field</em>, donde el parámetro de <em>campo</em> puede ser una de las cadenas siguientes:</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>El valor predeterminado es **fecha**.     |     
| isDescending  |  Boolean   | Ordena los objetos de [campaña](#campaign) en el cuerpo de la respuesta en orden descendente o ascendente.   |         
| storeProductId  |  string   | Use este valor para devolver solo las campañas de ad asociadas a la aplicación con el identificador de [almacén](in-app-purchases-and-trials.md#store-ids)especificado. Un ejemplo de un identificador de almacén para un producto es 9nblggh42cfd.   |         
| etiqueta  |  string   | Use este valor para devolver solo las campañas de ad que incluyen la *etiqueta* especificada en el objeto de [campaña](#campaign) .    |       |    


### <a name="request-body"></a>Cuerpo de la solicitud

Los métodos POST y PUT requieren un cuerpo de solicitud JSON con los campos obligatorios de un objeto de [campaña](#campaign) y cualquier campo adicional que desee establecer o cambiar.


### <a name="request-examples"></a>Ejemplos de solicitud

En el ejemplo siguiente se muestra cómo llamar al método POST para crear una campaña de ad.

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

En el ejemplo siguiente se muestra cómo llamar al método GET para recuperar una campaña de ad específica.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

En el ejemplo siguiente se muestra cómo llamar al método GET para consultar un conjunto de campañas de ad, ordenados por la fecha de creación.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Response

Estos métodos devuelven un cuerpo de respuesta JSON con uno o más objetos de [campaña](#campaign) , dependiendo del método al que llamó. En el ejemplo siguiente se muestra un cuerpo de respuesta para el método GET de una campaña específica.

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

Los cuerpos de solicitud y respuesta de estos métodos contienen los campos siguientes. En esta tabla se muestran los campos que son de solo lectura (lo que significa que no se pueden cambiar en el método PUT) y qué campos son necesarios en el cuerpo de la solicitud para el método POST.

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Valor predeterminado  | Requerido para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  integer   |  El identificador de la campaña de anuncios.     |   Sí    |      |  No     |       
|  name   |  string   |   El nombre de la campaña de anuncios.    |    No   |      |  Sí     |       
|  configuredStatus   |  string   |  Uno de los siguientes valores que especifica el estado de la campaña de ad especificada por el desarrollador: <ul><li>**Activo**</li><li>**Inactivo**</li></ul>     |  No     |  Active    |   Sí    |       
|  effectiveStatus   |  string   |   Uno de los siguientes valores que especifica el estado efectivo de la campaña de AD en función de la validación del sistema: <ul><li>**Activo**</li><li>**Inactivo**</li><li>**Procesamiento**</li></ul>    |    Sí   |      |   No      |       
|  effectiveStatusReasons   |  array   |  Uno o varios de los siguientes valores que especifican el motivo del Estado efectivo de la campaña de ad: <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**Erróneo**</li></ul>      |  Sí     |     |    No     |       
|  storeProductId   |  string   |  El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) de la aplicación a la que está asociada esta campaña de ad. Un ejemplo de un identificador de almacén para un producto es 9nblggh42cfd.     |   Sí    |      |  Sí     |       
|  labels   |  array   |   Una o más cadenas que representan etiquetas personalizadas para la campaña. Estas etiquetas se usan para buscar y etiquetar campañas.    |   No    |  null    |    No     |       
|  type   | string    |  Uno de los siguientes valores que especifica el tipo de campaña: <ul><li>**Pagado**</li><li>**Casa**</li><li>**Comunidad**</li></ul>      |   Sí    |      |   Sí    |       
|  objetivo   |  string   |  Uno de los siguientes valores que especifica el objetivo de la campaña: <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   No    |  DriveInstall    |   Sí    |       
|  lines   |  array   |   Uno o más objetos que identifican las [líneas de entrega](manage-delivery-lines-for-ad-campaigns.md#line) asociadas a la campaña de ad. Cada objeto de este campo consta de un *identificador* y un campo de *nombre* que especifica el identificador y el nombre de la línea de entrega.     |   No    |      |    No     |       
|  createdDate   |  string   |  Fecha y hora en que se creó la campaña de ad, en formato ISO 8601.     |  Sí     |      |     No    |       |


## <a name="related-topics"></a>Temas relacionados

* [Ejecutar campañas de ad con Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Administrar líneas de entrega para campañas de ad](manage-delivery-lines-for-ad-campaigns.md)
* [Administrar perfiles de destino para campañas de ad](manage-targeting-profiles-for-ad-campaigns.md)
* [Administrar creativos para campañas de ad](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)