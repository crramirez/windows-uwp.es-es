---
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: Use este método en la API de Microsoft Store Analytics para obtener datos de rendimiento de campaña de ad agregada para la aplicación especificada durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener los datos de rendimiento de la campaña de anuncios
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, API de Microsoft Store Analytics, campañas de ad
ms.localizationpriority: medium
ms.openlocfilehash: c40ab5d6aea67477e0900441cfb02b858dbef9b2
ms.sourcegitcommit: 5d84d8fe60e83647fa363b710916cf8b92c6e331
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91878458"
---
# <a name="get-ad-campaign-performance-data"></a>Obtener los datos de rendimiento de la campaña de anuncios


Use este método en la API de Microsoft Store Analytics para obtener un resumen agregado de los datos de rendimiento de campaña de publicidad promocional para las aplicaciones durante un intervalo de fechas determinado y otros filtros opcionales. Este método devuelve los datos en formato JSON.

Este método devuelve los mismos datos proporcionados por el [Informe de campaña de ad](/windows/uwp/publish/ad-campaign-report) en el centro de Partners. Para obtener más información acerca de las campañas de anuncios, consulta [Crear una campaña publicitaria para tu aplicación](./index.md).

Para crear, actualizar o recuperar detalles de las campañas de ad, puede usar los métodos de [Administración de campañas de ad](manage-ad-campaigns.md) en la API de promociones de [Microsoft Store](run-ad-campaigns-using-windows-store-services.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                |
|---------------|--------|---------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

Para recuperar los datos de rendimiento de una campaña de anuncios de una aplicación en concreto, usa el parámetro *applicationId*. Para recuperar los datos de rendimiento de anuncios de todas las aplicaciones que hay asociadas a tu cuenta de desarrollador, omite el parámetro *applicationId*.

| Parámetro     | Tipo   | Descripción     | Requerido |
|---------------|--------|-----------------|----------|
| applicationId   | string    | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de rendimiento de la campaña de ad. |    No      |
|  startDate  |  date   |  La fecha de inicio del intervalo de fechas de los datos de rendimiento de campaña de anuncios que quieres recuperar en formato AAAA/MM/DD. El valor predeterminado es la fecha 30 días posterior al día en curso.   |   No    |
| endDate   |  date   |  La fecha de finalización del intervalo de fechas de los datos de rendimiento de campaña de anuncios que quieres recuperar en formato AAAA/MM/DD. El valor predeterminado es la fecha del día anterior.   |   No    |
| top   |  int   |  Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.   |   No    |
| skip   | int    |  Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente.   |   No    |
| filter   |  string   |  Una o más instrucciones que filtran las filas de la respuesta. El único filtro admitido es **campaignId**. Cada instrucción puede utilizar los operadores **eq** o **ne**, y las instrucciones se pueden combinar mediante **and** u **or**.  Este es un ejemplo del parámetro *filter*: ```filter=campaignId eq '100023'```.   |   No    |
|  aggregationLevel  |  string   | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>.    |   No    |
| orderby   |  string   |  <p>Instrucción que ordena los valores de los datos resultantes de los datos de rendimiento de una campaña de anuncios. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:</p><ul><li><strong>date</strong></li><li><strong>campaignId</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, campaignId</em></p>   |   No    |
|  groupby  |  string   |  <p>Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>date</strong></li><li><strong>currencyCode</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em> &amp; GroupBy = ApplicationID &amp; aggregationLevel = Week</em></p>   |   No    |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener los datos de rendimiento de una campaña de anuncios.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Response body

| Value      | Tipo   | Descripción  |
|------------|--------|---------------|
| Value      | array  | Una matriz de objetos que contiene datos agregados de rendimiento de una campaña de anuncios. Para obtener más información sobre los datos de cada objeto, consulta la sección siguiente [Objeto de rendimiento de campañas](#campaign-performance-object).          |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 5, pero resulta que hay más de 5 elementos de datos de la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.                                |


<span id="campaign-performance-object" />


### <a name="campaign-performance-object"></a>Objeto de rendimiento de campañas

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción            |
|---------------------|--------|------------------------|
| date                | string | Es la primera fecha del intervalo de fechas de los datos de rendimiento de una campaña de anuncios. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId       | string | El identificador de la Tienda de la aplicación para la que quieres recuperar los datos de rendimiento de campaña de anuncios.                     |
| campaignId     | string | El identificador de la campaña de anuncios.           |
| lineId     | string |    IDENTIFICADOR de la [línea de entrega](manage-delivery-lines-for-ad-campaigns.md) de campaña de ad que generó estos datos de rendimiento.        |
| currencyCode              | string | El código de divisa del presupuesto de la campaña.              |
| spend          | string |  El presupuesto que se ha invertido en la campaña de anuncios.     |
| impressions           | long | El número de impresiones de anuncios de la campaña.        |
| installs              | long | El número de instalaciones de aplicaciones relacionadas con la campaña.   |
| clicks            | long | El número de clics de anuncios de la campaña.      |
| iapInstalls            | long | El número de complementos (también denominados compra desde la aplicación o IAP) se instala en relación con la campaña.      |
| activeUsers            | long | El número de usuarios que han haga clic en un anuncio que forma parte de la campaña y que se devuelve a la aplicación.      |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "lineId": "0001",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8,
      "iapInstalls": 0,
      "activeUsers": 0
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "lineId": "0002",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5,
      "iapInstalls": 0,
      "activeUsers": 0
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

## <a name="related-topics"></a>Temas relacionados

* [Crear una campaña publicitaria para tu aplicación](./index.md)
* [Ejecutar campañas de ad con Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)