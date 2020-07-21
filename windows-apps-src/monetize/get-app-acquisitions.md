---
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición agregados para una aplicación durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener los datos de las adquisiciones de la aplicación
ms.date: 03/23/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, adquisiciones de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: 9d0b19ee837debfa7807de7c594ae076271d3537
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493350"
---
# <a name="get-app-acquisitions"></a>Obtener los datos de las adquisiciones de la aplicación


Use este método en la API de Microsoft Store Analytics para obtener datos de adquisición agregados en formato JSON para una aplicación durante un intervalo de fechas determinado y otros filtros opcionales. Esta información también está disponible en el [Informe de adquisiciones](../publish/acquisitions-report.md) del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de adquisición.  |  Sí  |
| startDate | date | La fecha de inicio del intervalo de fechas de los datos de compra que se han de recuperar. La fecha predeterminada es la actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de compra que se han de recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Por ejemplo, *filtro = Market EQ ' EE. UU. ' y sexo EQ ' m '*. <p/><p/>Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>orderName</strong></li></ul> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Instrucción que ordena los valores de datos resultantes de cada compra. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li><strong>date</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>orderName</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:<ul><li><strong>date</strong></li><li><strong>applicationName</strong></li><li><strong>acquisitionType</strong></li><li><strong>ageGroup</strong></li><li><strong>storeClient</strong></li><li><strong>sexo</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>orderName</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>acquisitionQuantity</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em> &amp; GroupBy = edad, Market &amp; aggregationLevel = Week</em></p> |  No  |

### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener los datos de compra de la aplicación. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de adquisición agregados para la aplicación. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra](#acquisition-values) que encontrarás a continuación.                                                                                                                      |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra de la solicitud. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.                 |


### <a name="acquisition-values"></a>Valores de compra

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId       | string | El Id. de la Tienda de la aplicación sobre la que estás recuperando los datos de compra.     |
| applicationName     | string | El nombre para mostrar de la aplicación.   |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo la adquisición:<ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>    |
| orderName           | string | Nombre del pedido.  |
| storeClient         | string | Una de las siguientes cadenas que indica la versión del almacén donde se produjo la adquisición:<ul><li>**Tienda de Windows Phone (cliente)**</li><li>**Microsoft Store (cliente)** (o la **tienda Windows (cliente)** si consulta los datos antes del 23 de marzo de 2018)</li><li>**Microsoft Store (Web)** (o la **tienda Windows (Web)** si consulta los datos antes del 23 de marzo de 2018)</li><li>**Compras por volumen de empresas**</li><li>**Otros**</li></ul>                                                                                            |
| osVersion           | string | Una de las siguientes cadenas que especifica la versión del sistema operativo en la que se produjo la adquisición:<ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>  |
| market              | string | Código de país ISO 3166 del mercado donde se realizó la compra.  |
| gender              | string | Una de las siguientes cadenas que especifica el sexo del usuario que realizó la adquisición:<ul><li><strong>m</strong></li><li><strong>f</strong></li><li><strong>Unknown</strong></li></ul>    |
| ageGroup            | string | Una de las siguientes cadenas que especifica el grupo de edad del usuario que realizó la adquisición:<ul><li><strong>menor que 13</strong></li><li><strong>13-17</strong></li><li><strong>18-24</strong></li><li><strong>25-34</strong></li><li><strong>35-44</strong></li><li><strong>44-55</strong></li><li><strong>mayor que 55</strong></li><li><strong>Unknown</strong></li></ul>  |
| acquisitionType     | string | Una de las siguientes cadenas que indica el tipo de adquisición:<ul><li><strong>Gratis</strong></li><li><strong>Versión de prueba</strong></li><li><strong>Pagado</strong></li><li><strong>Código promocional</strong></li><li><strong>IAP</strong></li><li><strong>Suscripción IAP</strong></li><li><strong>Audiencia privada</strong></li><li><strong>Pedido previo</strong></li><li><strong>Juego de Xbox Pass</strong> (o <strong>Pass Pass</strong> si consulta los datos antes del 23 de marzo de 2018)</li><li><strong>Disco</strong></li><li><strong>Código de prepago</strong></li></ul>   |
| acquisitionQuantity | number | Número de compras que se realizaron durante el nivel de agregación especificado.    |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "IT",
      "gender": "m",
      "ageGroup": "0-17",
      "acquisitionType": "Free",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "appacquisitions?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount": 466766
}
```

## <a name="related-topics"></a>Temas relacionados

* [Informe Adquisiciones](../publish/acquisitions-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de embudo de adquisiciones de aplicaciones](get-acquisition-funnel-data.md)
* [Obtener conversiones de aplicaciones por canal](get-app-conversions-by-channel.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
