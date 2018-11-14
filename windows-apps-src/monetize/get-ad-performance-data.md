---
author: Xansky
ms.assetid: 235EBA39-8F64-4499-9833-4CCA9C737477
description: Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados de rendimiento de los anuncios de una aplicación durante un intervalo de fechas concreto y según otros filtros opcionales.
title: Obtener los datos de rendimiento de los anuncios
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, servicios de Microsoft Store, Store services, API de análisis de la Store Windows, Microsoft Store analytics API, anuncios, ads, rendimiento, performance
ms.localizationpriority: medium
ms.openlocfilehash: 7310eeb04915933adc149165fa6774ed2f413814
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6655460"
---
# <a name="get-ad-performance-data"></a>Obtener los datos de rendimiento de los anuncios


Usa este método en la API de análisis de Microsoft Store para obtener los datos agregados de rendimiento de los anuncios de tus aplicaciones durante un intervalo de fechas concreto y según otros filtros opcionales. Este método devuelve los datos en formato JSON.

Este método devuelve los mismos datos que proporcionan el [informe de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

Para obtener más información, consulta [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md).

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción           |
|---------------|--------|--------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

Para recuperar los datos de rendimiento de anuncios de una aplicación en concreto, usa el parámetro *applicationId*. Para recuperar los datos de rendimiento de anuncios de todas las aplicaciones que hay asociadas a tu cuenta de desarrollador, omite el parámetro *applicationId*.

| Parámetro     | Tipo   | Descripción     | Obligatorio |
|---------------|--------|-----------------|----------|
| applicationId   | cadena    | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) de la aplicación para la que quieres recuperar datos de rendimiento de anuncios.  |    No      |
| startDate   | fecha    | La fecha de inicio del intervalo de fechas de los datos de rendimiento de anuncios que quieres recuperar en formato AAAA/MM/DD. El valor predeterminado es la fecha 30 días posterior al día en curso. |    No      |
| endDate   | fecha    | La fecha de finalización del intervalo de fechas de los datos de rendimiento de anuncios que quieres recuperar en formato AAAA/MM/DD. El valor predeterminado es la fecha del día anterior. |    No      |
| top   | entero    | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |    No      |
| skip   | entero    | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |    No      |
| filter   | cadena    | Una o más instrucciones que filtran las filas en la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación. |    No      |
| aggregationLevel   | cadena    | Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. |    No      |
| orderby   | cadena    | Una instrucción que ordena los valores de los datos de resultado. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li><strong>date</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitId</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |    No      |
| groupby   | cadena    | Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:</p><ul><li><strong>applicationId</strong></li><li><strong>applicationName</strong></li><li><strong>fecha</strong></li><li><strong>accountCurrencyCode</strong></li><li><strong>market</strong></li><li><strong>deviceType</strong></li><li><strong>adUnitName</strong></li><li><strong>adUnitId</strong></li><li><strong>pubCenterAppName</strong></li><li><strong>adProvider</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p> |    No      |


### <a name="filter-fields"></a>Campos de filtro

El parámetro *filter* del cuerpo de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Este es un ejemplo del parámetro *filter*:

-   *filter=market eq 'US' and deviceType eq 'phone'*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que los valores de la cadena deben estar entre comillas simples en el parámetro *filter*.

| Campo | Descripción                                                              |
|--------|--------------------------------------------------------------------------|
| market    | Cadena que contiene el código de país ISO 3166 del mercado al que se destinaron los anuncios. |
| deviceType    | Una de las siguientes cadenas: <strong>PC/Tablet</strong> o <strong>Phone</strong>. |
| adUnitId    | Cadena que especifica un identificador de unidad de anuncios que aplicar al filtro. |
| pubCenterAppName    | Cadena que especifica el nombre de pubCenter de la aplicación actual que aplicar al filtro. |
| adProvider    | Cadena que especifica un nombre de proveedor de anuncios que aplicar al filtro. |
| fecha    | Cadena que especifica una fecha en formato AAAA/MM/DD que aplicar al filtro. |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes mediante las que obtener los datos de rendimiento de anuncios. Reemplaza el valor *applicationId* por el identificador de la Store de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/adsperformance?applicationId=9NBLGGH4R315&startDate=8/1/2015&endDate=8/31/2015&skip=0&$filter=market eq 'US' and deviceType eq 'phone’ eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | matriz  | Matriz de objetos que contiene datos agregados de rendimiento de anuncios. Para más información sobre los datos de cada objeto, consulta la sección de [valores de rendimiento de anuncios](#ad-performance-values) que encontrarás a continuación.                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 5, pero resulta que hay más de 5 elementos de datos de la consulta. |
| TotalCount | entero    | Número total de filas del resultado de datos de la consulta.                          |


### <a name="ad-performance-values"></a>Valores de rendimiento de anuncios

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | cadena | La primera fecha del intervalo de fechas de los datos de rendimiento de anuncios. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId       | cadena | El identificador de la Store de la aplicación para la que quieres recuperar datos de rendimiento de anuncios.     |
| applicationName     | cadena | Nombre para mostrar de la aplicación.                         |
| adUnitId           | cadena | Identificador de la unidad de anuncio.        |
| adUnitName           | cadena | El nombre de la unidad de anuncio según lo especificado por el desarrollador del centro de partners.              |
| adProvider           |  cadena  |  Nombre del proveedor del anuncio.   |
| deviceType          | cadena | Tipo de dispositivo al que se destinan los anuncios. Para obtener una lista de las cadenas admitidas, consulta la sección previa [Campos de filtro](#filter-fields).                              |
| market              | cadena | Código de país ISO 3166 del mercado al que se destinaron los anuncios.             |
| accountCurrencyCode     | cadena | Código de divisa de la cuenta.        |
| pubCenterAppName       |  cadena  |   El nombre de la aplicación pubCenter que está asociado con la aplicación en el centro de partners.   |
| adProviderRequests        | entero | Número de solicitudes de anuncio para el proveedor de anuncios especificado.                 |
| impressions           | entero | Número de impresiones de anuncios.        |
| clicks            | entero | Número de clics en anuncios.       |
| revenueInAccountCurrency       | número | Ingresos en la moneda del país o región de la cuenta.       |
| requests              | entero | Número de solicitudes de anuncio.                 |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10765920",
      "adUnitName":"TestAdUnit",
      "revenueInAccountCurrency": 10.0,
      "impressions": 1000,
      "requests": 10000,
      "clicks": 1,
      "accountCurrencyCode":"USD"
    },
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGH4R315",
      "applicationName": "Contoso Demo",
      "market": "US",
      "deviceType": "phone",
      "adUnitId":"10795110",
      "adUnitName":"TestAdUnit2",
      "revenueInAccountCurrency": 20.0,
      "impressions": 2000,
      "requests": 20000,
      "clicks": 3,
      "accountCurrencyCode":"USD"
    },
  ],
  "@nextLink": "adsperformance?applicationId=9NBLGGH4R315&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=2&skip=2",
  "TotalCount": 191753
}

```

## <a name="related-topics"></a>Temas relacionados

* [Informe de rendimiento de la publicidad](../publish/advertising-performance-report.md)
* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
