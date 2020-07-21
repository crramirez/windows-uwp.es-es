---
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición agregados para un complemento durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener los datos de las adquisiciones de complementos
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, adquisiciones de complementos
ms.localizationpriority: medium
ms.openlocfilehash: 64605b044c93ee4744d09fe4f44d07bca39285ad
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493220"
---
# <a name="get-add-on-acquisitions"></a>Obtener los datos de las adquisiciones de complementos

Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición agregados para los complementos de la aplicación en formato JSON durante un intervalo de fechas determinado y otros filtros opcionales. Esta información también está disponible en el [Informe de adquisiciones de complementos](../publish/add-on-acquisitions-report.md) del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción          |
|---------------|--------|--------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

El parámetro *applicationId* o *inAppProductId* es obligatorio. Para recuperar los datos de compra de todos los complementos registrados en la aplicación, especifica el parámetro *applicationId*. Para recuperar los datos de compra de un solo complemento, especifica el parámetro *inAppProductId*. Si especificas ambos, el parámetro *applicationId* se ignorará.

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de adquisición del complemento.  |  Sí  |
| inAppProductId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del complemento para el que desea recuperar los datos de adquisición.  | Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de compra del complemento que se recuperarán. La fecha predeterminada es la actual. |  No  |
| endDate | date | La fecha de finalización del intervalo de fechas de los datos de compra del complemento que recuperarán. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter |string  | Una o más instrucciones que filtran las filas de la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada compra de complemento. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>orderName</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>inAppProductName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>orderName</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>inAppProductId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em> &amp; GroupBy = edad, Market &amp; aggregationLevel = Week</em></p> |  No  |


### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Estos son algunos ejemplos de parámetros *filter*:

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Desconocido') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'mayor que 55' or ageGroup ne ‘menor que 13’)*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples.

| Campos        |  Descripción        |
|---------------|-----------------|
| acquisitionType | Una de las cadenas siguientes:<ul><li><strong>ningún</strong></li><li><strong>periodo</strong></li><li><strong>abonen</strong></li><li><strong>código promocional</strong></li><li><strong>IAP</strong></li></ul> |
| ageGroup | Una de las cadenas siguientes:<ul><li><strong>menor que 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>mayor que 55</strong></li><li><strong>Unknown</strong></li></ul> |
| storeClient | Una de las cadenas siguientes:<ul><li><strong>Tienda de Windows Phone (cliente)</strong></li><li><strong>Microsoft Store (cliente)</strong></li><li><strong>Microsoft Store (Web)</strong></li><li><strong>Compras por volumen de empresas</strong></li><li><strong>Otros</strong></li></ul> |
| gender | Una de las cadenas siguientes:<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul> |
| market | Es una cadena que contiene el código de país ISO 3166 del mercado donde se realizó la compra. |
| osVersion | Una de las cadenas siguientes:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul> |
| deviceType | Una de las cadenas siguientes:<ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul> |
| orderName | Una cadena que especifica el nombre del pedido del código promocional que se usó para comprar el complemento (esto solo es aplicable si el usuario adquirió el complemento canjeando un código promocional). |


### <a name="request-example"></a>Ejemplo de solicitud

En los ejemplos siguientes se muestran varias solicitudes para obtener datos de compra de complementos. Reemplaza los valores de *inAppProductId* y *applicationId* con los Id. de la Tienda correspondientes al complemento o aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción         |
|------------|--------|------------------|
| Value      | array  | Matriz de objetos que contienen los datos de compra agregados del complemento. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra del complemento](#add-on-acquisition-values) que encontrarás a continuación.                                                                                                              |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero hay más de 10000 filas de datos de compra de complementos para la solicitud. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.    |


<span id="add-on-acquisition-values" />

### <a name="add-on-acquisition-values"></a>Valores de compra del complemento

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo    | Descripción        |
|---------------------|---------|---------------------|
| date                | string  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| inAppProductId      | string  | El identificador de la Tienda del complemento para el que quieres recuperar datos de compra.                                                                                                                                                                 |
| inAppProductName    | string  | Nombre del complemento que quieres que se muestre. Este valor solo aparece en los datos de respuesta si el parámetro *aggregationLevel* se establece en **day**, a menos que especifiques el campo **inAppProductName** en el parámetro *groupby*.                                                                                                                                                                                                            |
| applicationId       | string  | El identificador de la Tienda de la aplicación para la que quieres recuperar los datos de compra de complementos.                                                                                                                                                           |
| applicationName     | string  | El nombre para mostrar de la aplicación.                                                                                                                                                                                                             |
| deviceType          | string  | Tipo de dispositivo que completó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).                                                                                                  |
| orderName           | string  | Nombre del pedido.                                                                                                                                                                                                                   |
| storeClient         | string  | Versión de la Tienda donde se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).                                                                                            |
| osVersion           | string  | Versión del sistema operativo en el que se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).                                                                                                   |
| market              | string  | Código de país ISO 3166 del mercado donde se realizó la compra.                                                                                                                                                                  |
| gender              | string  | Género del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).                                                                                                    |
| ageGroup            | string  | Grupo de edad del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).                                                                                                 |
| acquisitionType     | string  | Tipo de compra (gratuita, de pago, etc.). Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).                                                                                                    |
| acquisitionQuantity | integer | Número de compras que se realizaron.                        |


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

* [Informe de adquisiciones de complementos](../publish/add-on-acquisitions-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener conversiones de complementos por canal](get-add-on-conversions-by-channel.md)
* [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener datos de embudo de adquisiciones de aplicaciones](get-acquisition-funnel-data.md)
* [Obtener conversiones de aplicaciones por canal](get-app-conversions-by-channel.md)

 

 
