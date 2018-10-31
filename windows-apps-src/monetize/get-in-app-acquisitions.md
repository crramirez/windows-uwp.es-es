---
author: Xansky
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos de compra agregados de un complemento de un intervalo de fechas especificado y otros filtros opcionales.
title: Obtener los datos de las adquisiciones de complementos
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, servicios de Microsoft Store, Store services, API de análisis de Microsoft Store, Microsoft Store analytics API, adquisiciones de complementos, add-on acquisitions
ms.localizationpriority: medium
ms.openlocfilehash: c1fb44457b8589c9e1525de9c8dca2801c0c47bf
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5823933"
---
# <a name="get-add-on-acquisitions"></a>Obtener los datos de las adquisiciones de complementos

Usa este método en la API de análisis de Microsoft Store para obtener los datos de adquisición agregados en formato JSON para complementos de tu aplicación pertenecientes a un intervalo de fechas dado y según otros filtros opcionales. Esta información también está disponible en el [informe de adquisiciones de complementos](../publish/add-on-acquisitions-report.md) del panel del Centro de desarrollo de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción          |
|---------------|--------|--------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de la solicitud

El parámetro *applicationId* o *inAppProductId* es obligatorio. Para recuperar los datos de compra de todos los complementos registrados en la aplicación, especifica el parámetro *applicationId*. Para recuperar los datos de compra de un solo complemento, especifica el parámetro *inAppProductId*. Si especificas ambos, el parámetro *applicationId* se ignorará.

| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que quieres recuperar los datos de compra de complementos.  |  Sí  |
| inAppProductId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del complemento para el que quieres recuperar los datos de compra.  | Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de compra del complemento que se recuperarán. El valor predeterminado es la fecha actual. |  No  |
| endDate | fecha | La fecha de finalización del intervalo de fechas de los datos de compra del complemento que recuperarán. El valor predeterminado es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |cadena  | Una o más instrucciones que filtran las filas en la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |
| aggregationLevel | cadena | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | cadena | Instrucción que ordena los valores de datos resultantes de cada compra de complemento. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>fecha</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No  |
| groupby | cadena | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>fecha</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>gender</strong></li><li><strong>market</strong></li><li><strong>osVersion</strong></li><li><strong>deviceType</strong></li><li><strong>orderName</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>fecha</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  No  |


### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Estos son algunos ejemplos de parámetros *filter*:

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Unknown') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'mayor que 55' or ageGroup ne ‘menor que 13’)*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que los valores de la cadena deben estar entre comillas simples en el parámetro *filter*.

| Campos        |  Descripción        |
|---------------|-----------------|
| acquisitionType | Una de las cadenas siguientes:<ul><li><strong>gratuita</strong></li><li><strong>versión de prueba</strong></li><li><strong>de pago</strong></li><li><strong>código promocional</strong></li><li><strong>IAP</strong></li></ul> |
| ageGroup | Una de las cadenas siguientes:<ul><li><strong>menor que 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>mayor que 55</strong></li><li><strong>Unknown</strong></li></ul> |
| storeClient | Una de las cadenas siguientes:<ul><li><strong>Tienda de Windows Phone (cliente)</strong></li><li><strong>Microsoft Store (cliente)</strong></li><li><strong>Microsoft Store (web)</strong></li><li><strong>Compras por volumen de empresas</strong></li><li><strong>Otros</strong></li></ul> |
| gender | Una de las cadenas siguientes:<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul> |
| market | Es una cadena que contiene el código de país ISO 3166 del mercado donde se realizó la compra. |
| osVersion | Una de las cadenas siguientes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Phone</strong></li><li><strong>Console</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| orderName | Una cadena que especifica el nombre del pedido del código promocional que se usó para comprar el complemento (esto solo es aplicable si el usuario adquirió el complemento canjeando un código promocional). |


### <a name="request-example"></a>Ejemplo de solicitud

En los ejemplos siguientes se muestran varias solicitudes para obtener datos de compra de complementos. Reemplaza los valores de *inAppProductId* y *applicationId* con los Id. de la Store correspondientes al complemento o aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción         |
|------------|--------|------------------|
| Valor      | matriz  | Matriz de objetos que contienen los datos de compra agregados del complemento. Para más información sobre los datos de cada objeto, consulta la sección [valores de compra del complemento](#add-on-acquisition-values) que encontrarás a continuación.                                                                                                              |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero hay más de 10000 filas de datos de compra de complementos para la solicitud. |
| TotalCount | entero    | Número total de filas del resultado de datos de la consulta.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valores de compra del complemento

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo    | Descripción        |
|---------------------|---------|---------------------|
| fecha                | cadena  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| inAppProductId      | cadena  | El identificador de la Store del complemento para el que quieres recuperar datos de compra.                                                                                                                                                                 |
| inAppProductName    | cadena  | Nombre del complemento que quieres que se muestre. Este valor solo aparece en los datos de respuesta si el parámetro *aggregationLevel* se establece en **day**, a menos que especifiques el campo **inAppProductName** en el parámetro *groupby*.                                                                                                                                                                                                            |
| applicationId       | cadena  | El identificador de la Store de la aplicación para la que quieres recuperar los datos de compra de complementos.                                                                                                                                                           |
| applicationName     | cadena  | Nombre para mostrar de la aplicación.                                                                                                                                                                                                             |
| deviceType          | cadena  | Tipo de dispositivo que completó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                  |
| orderName           | cadena  | Nombre del pedido.                                                                                                                                                                                                                   |
| storeClient         | cadena  | Versión de la Store donde se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                            |
| osVersion           | cadena  | Versión del sistema operativo en el que se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                   |
| market              | cadena  | Código de país ISO 3166 del mercado donde se realizó la compra.                                                                                                                                                                  |
| gender              | cadena  | Género del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                    |
| ageGroup            | cadena  | Grupo de edad del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                 |
| acquisitionType     | cadena  | Tipo de compra (gratuita, de pago, etc.). Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                    |
| acquisitionQuantity | entero | Número de compras que se realizaron.                        |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso add-on 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe Adquisiciones de complementos](../publish/add-on-acquisitions-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener conversiones de complementos por canal](get-add-on-conversions-by-channel.md)
* [Obtener los datos de compra de la aplicación](get-app-acquisitions.md)
* [Obtener datos de embudo de adquisiciones de aplicaciones](get-acquisition-funnel-data.md)
* [Obtener conversiones de aplicaciones por canal](get-app-conversions-by-channel.md)

 

 
