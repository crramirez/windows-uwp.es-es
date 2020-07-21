---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Use este método en la API de Microsoft Store Analytics para obtener datos de uso de la aplicación mensuales para un intervalo de fechas determinado y otros filtros opcionales.
title: Obtener el uso mensual de la aplicación
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, uso
ms.localizationpriority: medium
ms.openlocfilehash: a1b82e1f538a0abfda8cb8d4f7ac677464025c8e
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492820"
---
# <a name="get-monthly-app-usage"></a>Obtener el uso mensual de la aplicación

Use este método en la API de Microsoft Store Analytics para obtener datos de uso agregados (sin incluir Xbox multijugador) en formato JSON para una aplicación durante un intervalo de fechas determinado (solo los últimos 90 días) y otros filtros opcionales. Esta información también está disponible en el [Informe de uso](../publish/usage-report.md) del centro de Partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro     | Tipo   |  Descripción                                                                                                    |  Requerido  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | string | El [identificador](in-app-purchases-and-trials.md#store-ids) de la tienda de la aplicación para la que desea recuperar los datos de revisión. |  Sí       |
| startDate     | date   | La fecha de inicio del intervalo de fechas de los datos de revisión que se han de recuperar. La fecha predeterminada es la actual.                   |  No        |
| endDate       | date   | Fecha de finalización del intervalo de fechas de los datos de revisión que se han de recuperar. La fecha predeterminada es la actual.                     |  No        |
| top           | int    | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.                          |  No        |
| skip          | int    | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente.                         |  No        |  
| filter        |string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo del cuerpo de la respuesta y el valor que están asociados a los operadores EQ o NE, y las instrucciones se pueden combinar con and u or. Ten en cuenta que en el parámetro filter los valores de la cadena deben estar entre comillas simples. Puede especificar los siguientes campos del cuerpo de la respuesta: <ul><li>**datamarket**</li><li>**TipoDeDispositivo**</li><li>**packageVersion**</li></ul>                                                                                                                                              | No         |  
| orderby       | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es <em>OrderBy = Field [order], Field [order],...</em>. El parámetro de <em>campo</em> puede ser una de las cadenas siguientes:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**datamarket**</li><li>**packageVersion**</li><li>**TipoDeDispositivo**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **ASC**.</p><p>Este es un ejemplo de cadena <em>OrderBy</em> : <em>OrderBy = Date, Market</em></p> |  No        |
| groupby       | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puede especificar los siguientes campos del cuerpo de la respuesta:<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**TipoDeDispositivo**</li><li>**packageVersion**</li><li>**datamarket**</li><li>**date**</li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em> &amp; GroupBy = edad, Market &amp; aggregationLevel = Week</em></p> |  No        |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos de uso de la aplicación mensuales. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response


### <a name="response-body"></a>Cuerpo de la respuesta

| Value      | Tipo   | Descripción                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Value      | array  | Matriz de objetos que contienen datos de uso agregados. Para obtener más información acerca de los datos de cada objeto, vea la tabla siguiente. |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de opiniones de la solicitud.                 |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta.                                                                          |

 
### <a name="usage-values"></a>Valores de uso

Los elementos de la matriz *Value* contienen los siguientes valores.

| Value                     | Tipo    | Descripción                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| date                      | string  | Primera fecha del intervalo de fechas de los datos de uso. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas.                          |
| applicationId             | string  | El identificador de la tienda de la aplicación para la que se recuperan los datos de uso.                            |
| applicationName           | string  | El nombre para mostrar de la aplicación.                                                                |
| market                    | string  | El código de país ISO 3166 del mercado en el que el cliente usó su aplicación.                   |
| packageVersion            | string  | Versión del paquete en la que se ha producido el uso.                                            |
| deviceType                | string  | Una de las siguientes cadenas que especifica el tipo de dispositivo en el que se produjo el uso:<ul><li>**PC**</li><li>**Número**</li><li>**Consola: Xbox One**</li><li>**Consola: serie Xbox X**</li><li>**Tableta**</li><li>**IoT**</li><li>**Server**</li><li>**Holographic**</li><li>**Unknown**</li></ul>                                                                                                                           |
| subscriptionName          | string  | Indica si el uso se realizó a través del juego de Xbox.                                              |
| monthlySessionCount       | long    | El número de sesiones de usuario durante ese mes.                                              |
| engagementDurationMinutes | double  | Los minutos en los que los usuarios usan activamente la aplicación, medidos en un período de tiempo distinto, a partir del momento en que se inicia la aplicación (inicio del proceso) y finaliza cuando termina (fin del proceso) o después de un período de inactividad.                               |
| monthlyActiveUsers        | long    | El número de clientes que usan la aplicación de ese mes.                                           |
| monthlyActiveDevices      | long    | El número de dispositivos que ejecutan la aplicación durante un período de tiempo distinto, a partir del momento en que se inicia la aplicación (inicio del proceso) y finaliza cuando termina (fin del proceso) o después de un período de inactividad.                                                        |
| monthlyNewUsers           | long    | El número de clientes que usaron la aplicación por primera vez ese mes.                    |
| averageDailyActiveUsers   | double  | El número promedio de clientes que usan la aplicación diariamente.                             |
| averageDailyActiveDevices | double  | El número medio de dispositivos usados para interactuar con la aplicación por todos los usuarios diariamente. |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```http
{
  "Value": [
    {
      "date": "2018-06-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 582973,
      "engagementDurationMinutes": 16941418.7,
      "monthlyActiveUsers": 139604,
      "monthlyActiveDevices": 132296,
      "monthlyNewUsers": 88127,
      "averageDailyActiveUsers": 9099.23,
      "averageDailyActiveDevices": 8999.0
    },
    {
      "date": "2018-07-01",
      "applicationId": "XXXXXXXXXXXX",
      "applicationName": "My App",
      "market": "All",
      "packageVersion": "All",
      "deviceType": "All",
      "subscriptionName": "All",
      "monthlySessionCount": 681460,
      "engagementDurationMinutes": 21656645.3,
      "monthlyActiveUsers": 130481,
      "monthlyActiveDevices": 123583,
      "monthlyNewUsers": 78465,
      "averageDailyActiveUsers": 8257.55,
      "averageDailyActiveDevices": 8170.58
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener ussage de la aplicación diaria](get-app-usage-daily.md)
* [Obtener los datos de las adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
