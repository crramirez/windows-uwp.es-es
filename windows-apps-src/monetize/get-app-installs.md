---
ms.assetid: FAD033C7-F887-4217-A385-089F09242827
description: Use este método en la API de Microsoft Store Analytics para obtener datos de instalación agregados para una aplicación durante un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener instalaciones de aplicaciones
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, instalaciones de aplicaciones
ms.localizationpriority: medium
ms.openlocfilehash: 9391ac3222f98a52d6c9aaf4ed39eb8ac2e3de56
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492790"
---
# <a name="get-app-installs"></a>Obtener instalaciones de aplicaciones


Use este método en la API de Microsoft Store Analytics para obtener datos de instalación agregados en formato JSON para una aplicación durante un intervalo de fechas determinado y otros filtros opcionales. Esta información también está disponible en el [Informe de adquisiciones](../publish/acquisitions-report.md) del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro        | Tipo   |  Descripción      |  Requerido  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de instalación.  |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de los datos de instalación que se van a recuperar. La fecha predeterminada es la actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos de instalación que se van a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |
| filter | string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores **EQ** o **ne** , y las instrucciones se pueden combinar con **and** u **or**. Ten en cuenta que en el parámetro *filter* los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>TipoDeDispositivo</strong></li><li><strong>packageVersion</strong></li></ul> | No   |
| aggregationLevel | string | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. | No |
| orderby | string | Instrucción que ordena los valores de los datos de resultado de cada instalación. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser uno de los siguientes campos del cuerpo de respuesta:<p/><ul><li><strong>applicationName</strong></li><li><strong>date</strong><li><strong>TipoDeDispositivo</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li><li><strong>successfulInstallCount</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>ASC</strong>.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No  |
| groupby | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puede especificar los siguientes campos del cuerpo de la respuesta:<p/><ul><li><strong>applicationName</strong></li><li><strong>date</strong><li><strong>TipoDeDispositivo</strong></li><li><strong>datamarket</strong></li><li><strong>osVersion</strong></li><li><strong>packageVersion</strong></li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li><strong>date</strong></li><li><strong>applicationId</strong></li><li><strong>successfulInstallCount</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em> &amp; GroupBy = edad, Market &amp; aggregationLevel = Week</em></p> |  No  |

 
### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestran varias solicitudes para obtener datos de instalación de la aplicación. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/installs?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de instalación agregados. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente.                                                                                                                      |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 10000, pero hay más de 10000 filas de datos de instalación para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.    |


Los elementos de la matriz *Value* contienen los siguientes valores.

| Value               | Tipo   | Descripción                           |
|---------------------|--------|-------------------------------------------|
| date                | string | Primera fecha del intervalo de fechas de los datos de instalación. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId       | string | El identificador de la tienda de la aplicación para la que se recuperan los datos de instalación.     |
| applicationName     | string | El nombre para mostrar de la aplicación.     |
| deviceType          | string | Una de las siguientes cadenas que especifica el tipo de dispositivo que completó la instalación:<p/><ul><li><strong>PC</strong></li><li><strong>Número</strong></li><li><strong>Consola: Xbox One</strong></li><li><strong>Consola: serie Xbox X</strong></li><li><strong>IoT</strong></li><li><strong>Holographic</strong></li><li><strong>Unknown</strong></li></ul>  |
| packageVersion           | string | Versión del paquete que se instaló.  |
| osVersion           | string | Una de las siguientes cadenas que especifica la versión del sistema operativo en la que se ha producido la instalación:<p/><ul><li><strong>Windows Phone 7.5</strong></li><li><strong>Windows Phone 8</strong></li><li><strong>Windows Phone 8.1</strong></li><li><strong>Windows Phone 10</strong></li><li><strong>Windows 8</strong></li><li><strong>Windows 8.1</strong></li><li><strong>Windows 10</strong></li><li><strong>Unknown</strong></li></ul>   |
| market              | string | El código de país ISO 3166 del mercado en el que se realizó la instalación.    |
| successfulInstallCount | number | El número de instalaciones correctas que se produjeron durante el nivel de agregación especificado.     |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2016-02-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "packageVersion": "1.0.0.0",
      "deviceType": "Phone",
      "market": "IT",
      "osVersion": "Windows Phone 8.1",
      "successfulInstallCount": 1
    }
  ],
  "@nextLink": "installs?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1&orderby=date desc",
  "TotalCount":23012
}
```

## <a name="related-topics"></a>Temas relacionados

* [Instala el informe](../publish/installs-report.md)
* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
