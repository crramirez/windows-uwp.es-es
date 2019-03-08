---
ms.assetid: 4E4CB1E3-D213-4324-91E4-7D4A0EA19C53
description: Utilice este método en la API de análisis de Microsoft Store para obtener datos de uso mensual de aplicación para un intervalo de fechas dado y otros filtros opcionales.
title: Obtener el uso mensual de la aplicación
ms.date: 08/15/2018
ms.topic: article
keywords: Windows 10, uwp, servicios de Store, API, el uso de análisis de Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 48ad049b3f310f8b375a28d9695dd9280d686c43
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662930"
---
# <a name="get-monthly-app-usage"></a>Obtener el uso mensual de la aplicación

Utilice este método en la API de análisis de Microsoft Store para obtener datos de uso agregado (no incluido Xbox multijugador) en formato JSON para una aplicación durante un intervalo de fechas determinado (últimos 90 días sólo) y otros filtros opcionales. Esta información también está disponible en el [informe de uso](../publish/usage-report.md) en el centro de partners.

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de la solicitud                                                                 |
|--------|-----------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | string | Obligatorio. El token de acceso de Azure AD en el formulario **portador** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud

| Parámetro     | Tipo   |  Descripción                                                                                                    |  Requerido  |
|---------------|--------|-----------------------------------------------------------------------------------------------------------------|------------|
| applicationId | string | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) de la aplicación sobre la que quieres recuperar los datos de revisión. |  Sí       |
| startDate     | fecha   | La fecha de inicio del intervalo de fechas de los datos de revisión que se han de recuperar. El valor predeterminado es la fecha actual.                   |  No        |
| endDate       | fecha   | Fecha de finalización del intervalo de fechas de los datos de revisión que se han de recuperar. El valor predeterminado es la fecha actual.                     |  No        |
| top           | entero    | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.                          |  No        |
| skip          | entero    | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente.                         |  No        |  
| filter        |string  | Una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un nombre de campo desde el cuerpo de respuesta y el valor que están asociados con los operadores eq o ne, y las instrucciones se pueden combinar con y o. Los valores de cadena deben estar entre comillas simples en el parámetro de filtro. Puedes especificar los campos siguientes del cuerpo de respuesta: <ul><li>**market**</li><li>**deviceType**</li><li>**packageVersion**</li></ul>                                                                                                                                              | No         |  
| orderby       | string | Una instrucción que ordena los valores de datos resultantes. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:<ul><li>**date**</li><li>**applicationId**</li><li>**applicationName**</li><li>**market**</li><li>**packageVersion**</li><li>**deviceType**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser **asc** o **desc** para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es **asc**.</p><p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p> |  No        |
| groupby       | string | Una instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los campos siguientes del cuerpo de respuesta:<ul><li>**applicationName**</li><li>**subscriptionName**</li><li>**deviceType**</li><li>**packageVersion**</li><li>**market**</li><li>**date**</li></ul><p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p><ul><li>**applicationId**</li><li>**subscriptionName**</li><li>**monthlySessionCount**</li><li>**engagementDurationMinutes**</li><li>**monthlyActiveUsers**</li><li>**monthlyActiveDevices**</li><li>**monthlyNewUsers**</li><li>**averageDailyActiveUsers**</li><li>**averageDailyActiveDevices**</li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  No        |


### <a name="request-example"></a>Ejemplo de solicitud

El ejemplo siguiente muestra una solicitud para obtener datos de uso mensual de aplicación. Sustituye el valor *applicationId* por el id. de la Tienda de la aplicación.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/usagemonthly?applicationId=XXXXXXXXXXXX&startDate=2018-06-01&endDate=2018-07-01 HTTP/1.1  
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta


### <a name="response-body"></a>Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                         |
|------------|--------|-------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | array  | Una matriz de objetos que contienen datos de uso agregado. Para más información sobre los datos de cada objeto, consulta la tabla siguiente. |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de opiniones de la solicitud.                 |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta.                                                                          |

 
### <a name="usage-values"></a>Valores de uso

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor                     | Tipo    | Descripción                                                                                 |
|---------------------------|---------|---------------------------------------------------------------------------------------------|
| fecha                      | string  | La primera fecha del intervalo de fechas para los datos de uso. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas.                          |
| applicationId             | string  | El identificador de Store de la aplicación para el que va a recuperar datos de uso.                            |
| applicationName           | string  | El nombre para mostrar de la aplicación.                                                                |
| market                    | string  | El código de país ISO 3166 del mercado donde el cliente usa la aplicación.                   |
| packageVersion            | string  | La versión del paquete donde se produjo el uso.                                            |
| deviceType                | string  | Una de las siguientes cadenas que especifica el tipo de dispositivo donde se produjo el uso de:<ul><li>**PC**</li><li>**Teléfono**</li><li>**Consola de**</li><li>**Tablet**</li><li>**IoT**</li><li>**Server**</li><li>**Holográfica**</li><li>**Unknown**</li></ul>                                                                                                                           |
| subscriptionName          | string  | Indica si el uso era a través de Xbox Game Pass.                                              |
| monthlySessionCount       | largo    | El número de sesiones de usuario durante ese mes.                                              |
| engagementDurationMinutes | double  | Los minutos, donde los usuarios están utilizando la aplicación que se mide por un período de tiempo, empezando cuando se inicia la aplicación distinto (inicio de proceso) y termina cuando se cierra (final del proceso) o tras un período de inactividad activamente.                               |
| monthlyActiveUsers        | largo    | El número de clientes mediante la aplicación de ese mes.                                           |
| monthlyActiveDevices      | largo    | El número de dispositivos que ejecutan la aplicación durante un período de tiempo, empezando cuando se inicia la aplicación (inicio de proceso) distinto y termina cuando se cierra (final del proceso) o tras un período de inactividad.                                                        |
| monthlyNewUsers           | largo    | El número de clientes que utilizaron la aplicación por primera vez ese mes.                    |
| averageDailyActiveUsers   | double  | El número promedio de los clientes que usan la aplicación a diario.                             |
| averageDailyActiveDevices | double  | El número promedio de los dispositivos usados para interactuar con la aplicación, todos los usuarios a diario. |


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

* [Acceder a los datos de análisis con servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener ussage diaria de aplicación](get-app-usage-daily.md)
* [Obtener las adquisiciones de la aplicación](get-app-acquisitions.md)
* [Obtener las adquisiciones de complemento](get-in-app-acquisitions.md)
* [Obtener datos de informes de errores](get-error-reporting-data.md)
