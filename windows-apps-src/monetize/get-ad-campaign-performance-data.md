---
author: mcleanbyron
ms.assetid: A26A287C-B4B0-49E9-BB28-6F02472AE1BA
description: "Usa este método en la API de análisis de la Tienda Windows para obtener los datos agregados de rendimiento de la campaña de anuncios de la aplicación especificada durante un intervalo de fechas indicado y según otros filtros opcionales."
title: "Obtener los datos de rendimiento de la campaña de anuncios"
translationtype: Human Translation
ms.sourcegitcommit: 67845c76448ed13fd458cb3ee9eb2b75430faade
ms.openlocfilehash: 8859658675d31deccc3403e4862c47a54ca25b1a

---

# Obtener los datos de rendimiento de la campaña de anuncios


Usa este método en la API de análisis de la Tienda Windows para obtener un resumen agregado de datos de rendimiento de la campaña de anuncios de tus aplicaciones durante un intervalo de fechas indicado y según otros filtros opcionales. Este método devuelve los datos en formato JSON.

Este método devuelve los mismos datos que proporciona el [informe de anuncios de instalación de aplicaciones](../publish/app-install-ads-reports.md) en el panel del Centro de desarrollo de Windows. Para obtener más información acerca de las campañas de anuncios, consulta [Crear una campaña publicitaria para tu aplicación](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app).

Para recuperar todos los detalles de todas las campañas de anuncios que se han creado para tu cuenta del Centro de desarrollo, puedes usar el [método Obtener detalles de todas las campañas de anuncios](#get-details-for-all-ad-campaigns) que se describe más adelante en este artículo.

## Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## Solicitud


### Sintaxis de la solicitud

| Método | URI de la solicitud                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion``` |

<span />

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                |
|---------------|--------|---------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span />

### Parámetros de solicitud

Para recuperar los datos de rendimiento de una campaña de anuncios de una aplicación en concreto, usa el parámetro *applicationId*. Para recuperar los datos de rendimiento de anuncios de todas las aplicaciones que hay asociadas a tu cuenta de desarrollador, omite el parámetro *applicationId*.

| Parámetro     | Tipo   | Descripción     | Obligatorio |
|---------------|--------|-----------------|----------|
| applicationId   | cadena    | El identificador de la Tienda de la aplicación para la que quieres recuperar los datos de rendimiento de campaña de anuncios. El identificador de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un identificador de la Tienda es 9NBLGGH4R315. |    No      |
|  startDate  |  fecha   |  La fecha de inicio del intervalo de fechas de los datos de rendimiento de campaña de anuncios que quieres recuperar en formato AAAA/MM/DD. El valor predeterminado es la fecha 30 días posterior al día en curso.   |   No    |
| endDate   |  fecha   |  La fecha de finalización del intervalo de fechas de los datos de rendimiento de campaña de anuncios que quieres recuperar en formato AAAA/MM/DD. El valor predeterminado es la fecha del día anterior.   |   No    |
| top   |  entero   |  Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.   |   No    |
| skip   | entero    |  Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente.   |   No    |
| filter   |  cadena   |  Una o más instrucciones que filtran las filas de la respuesta. El único filtro admitido es **campaignId**. Cada instrucción puede utilizar los operadores **eq** o **ne**, y las instrucciones se pueden combinar mediante **and** u **or**.  Este es un ejemplo del parámetro *filter*: ```filter=campaignId eq '100023'```.   |   No    |
|  aggregationLevel  |  cadena   | Especifica el intervalo de tiempo necesario para el que quieres recuperar datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>.    |   No    |
| orderby   |  cadena   |  <p>Instrucción que ordena los valores de los datos resultantes de los datos de rendimiento de una campaña de anuncios. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:</p><ul><li><strong>fecha</strong></li><li><strong>campaignId</strong></li></ul><p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p><p>Este es un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,campaignId</em></p>   |   No    |
|  groupby  |  cadena   |  <p>Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:</p><ul><li><strong>campaignId</strong></li><li><strong>applicationId</strong></li><li><strong>fecha</strong></li><li><strong>currencyCode</strong></li></ul><p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=applicationId&amp;aggregationLevel=week</em></p>   |   No    |


<span />
 

### Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener los datos de rendimiento de una campaña de anuncios.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?aggregationLevel=week&groupby=applicationId,campaignId,date  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/promotion?applicationId=9NBLGGH0XK8Z&startDate=2015/1/20&endDate=2016/8/31&skip=0&filter=campaignId eq '31007388' HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta


### Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | matriz  | Una matriz de objetos que contiene datos agregados de rendimiento de una campaña de anuncios. Para obtener más información sobre los datos de cada objeto, consulta la sección siguiente [Objeto de rendimiento de campañas](#campaign-performance-object).                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 5, pero resulta que hay más de 5 elementos de datos de la consulta. |
| TotalCount | entero    | Número total de filas del resultado de datos de la consulta.                                                                                                                                                                                                                             |

<span id="campaign-performance-object" />
### Objeto de rendimiento de campañas

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción            |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fecha                | cadena | Es la primera fecha del intervalo de fechas de los datos de rendimiento de una campaña de anuncios. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId       | cadena | El identificador de la Tienda de la aplicación para la que quieres recuperar los datos de rendimiento de campaña de anuncios.                     |
| campaignId     | cadena | El identificador de la campaña de anuncios.           |
| currencyCode              | cadena | El código de divisa del presupuesto de la campaña.              |
| spend          | cadena |  El presupuesto que se ha invertido en la campaña de anuncios.     |
| impressions           | largo | El número de impresiones de anuncios de la campaña.        |
| installs              | largo | El número de instalaciones de aplicaciones relacionadas con la campaña.   |
| clicks            | largo | El número de clics de anuncios de la campaña.      |

<span />

### Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-04-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "4568",
      "currencyCode": "USD",
      "spend": 700.6,
      "impressions": 200,
      "installs": 30,
      "clicks": 8
    },
    {
      "date": "2015-05-12",
      "applicationId": "9WZDNCRFJ31Q",
      "campaignId": "1234",
      "currencyCode": "USD",
      "spend": 325.3,
      "impressions": 20,
      "installs": 2,
      "clicks": 5
    }
  ],
  "@nextLink": "promotion?applicationId=9NBLGGGZ5QDR&aggregationLevel=day& startDate=2015/1/20&endDate=2016/8/31&top=2&skip=2",
  "TotalCount": 1917
}
```

<span id="get-details-for-all-ad-campaigns" />
## Obtener detalles de todas las campañas de anuncios


Usa este método para obtener información detallada de todas las campañas de anuncios que se han creado para las aplicaciones registradas en tu cuenta del Centro de desarrollo de Windows. Este método devuelve los datos en formato JSON. Para obtener más información acerca de las campañas de anuncios, consulta [Crear una campaña publicitaria para tu aplicación](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app).

### Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

### Solicitud


#### Sintaxis de la solicitud

| Método | URI de la solicitud                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign``` |

<span />

#### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |

<span />

#### Parámetros de solicitud

| Parámetro     | Tipo   | Descripción     | Obligatorio |
|---------------|--------|-----------------|----------|
| top           | entero    | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 1000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |    No      |
| skip           | entero    | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=100 y skip=0 recuperan las primeras 100 filas de datos, los valores top=100 y skip=100 recuperan las siguientes 100 filas de datos, y así sucesivamente. |    No      |


<span />
 

#### Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener más información de todas las campañas de anuncios.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/adscampaign?top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>
```

### Respuesta


#### Cuerpo de la respuesta

| Valor      | Tipo   | Descripción  |
|------------|--------|--------------|
| Value      | matriz  | Una matriz de objetos que contienen detalles de tus campañas de anuncios. Para obtener más información acerca de los datos de cada objeto, consulta la sección siguiente [Objeto de campañas](#campaign-object).                        |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 5, pero resulta que hay más de 5 elementos de datos de la consulta. |                                   |

<span id="campaign-object" />
#### Objeto de campañas

Los elementos de la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción          |
|---------------------|--------|------------------|
| id     | cadena | El identificador de la campaña de anuncios. |
| name     | cadena | El nombre de la campaña de anuncios. |
| createDate     | cadena | La fecha en que se creó la campaña de anuncios en la zona horaria UTC. |
| startDate     | cadena | La fecha de inicio de la campaña de anuncios en la zona horaria UTC. |
| endDate     | cadena | La fecha de finalización de la campaña de anuncios en la zona horaria UTC. |
| applicationId       | cadena | El identificador de la Tienda de la aplicación a la que está asociada la campaña de anuncios. El identificador de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un identificador de la Tienda es 9NBLGGH4R315.                    |
| budget     | número | El presupuesto para la campaña de anuncios.           |
| budgetType    | cadena | El tipo de presupuesto para la campaña de anuncios. Puede ser una de las siguientes cadenas: **Monthly (Mensual)** o **Total**.           |
| currencyCode              | cadena | El código de divisa del presupuesto de la campaña.              |
| type              | cadena | El tipo de campaña de anuncios. Puede ser una de las siguientes cadenas: **Paid**, **House** o **Community**.             |
| status              | cadena | El estado de la campaña de anuncios. Puede ser una de las siguientes cadenas: **Active**, **UserPaused**, **Pending**, **Ended** o **SystemPaused**.             |
| targetingType              | cadena | El tipo de público objetivo de la campaña de anuncios. Puede ser una de las siguientes cadenas: **Auto**, **Manual** o **Segment**.             |
| target              | diccionario | Un diccionario de pares de claves y valores que contienen información del público objetivo de la campaña. Para obtener más información acerca de este objeto, consulte la sección siguiente [Objeto de destino](#target-object).             |

<span />

<span id="target-object" />
#### Objeto de destino

Este recurso es un diccionario de los siguientes pares de claves y valores.

| Clave               | Tipo   | Valor          |
|---------------------|--------|------------------|
| Country     | matriz |   Una o varias de las cadenas que contienen los códigos ISO 3166 alpha-2 de los países o regiones a las que se dirige la campaña de anuncios. |
| OsVersion     | matriz | Una o varias de las siguientes cadenas que especifican las versiones de SO a las que se dirige la campaña de anuncios: **10** u **8.x**.  |
| Age     |  matriz |  Una o varias de las siguientes cadenas que especifica el intervalo de edad del público objetivo: **Age13To17**, **Age18To24**, **Age25To34**, **Age35To49** o **Age50AndAbove**. |
| Gender     |  matriz |  Una o varias de las siguientes cadenas que especifican el sexo del público objetivo: **Female** o **Male**. |
| DeviceType       |  matriz  |  Una o varias de las siguientes cadenas que especifican el tipo de dispositivo objetivo: **PC/Tablet** o **Phone**.                  |
| Segment     |  matriz |    Uno o varias cadenas que contienen los identificadores de los segmentos de clientes objetivo.       |


<span />

#### Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
   "Value": [
      {
         "id":31010026,
         "name": "Contoso App 1 Campaign",
         "createDate":"2016-07-14T10:01:21Z",
         "startDate":"2016-07-21T11:23:36Z",
         "applicationId":"9NBLGGH0XK8Z",
         "budget": 100,
         "budgetType":"Monthly",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Active",
         "targetingType": "Auto",
         "target": null
      },
      {
         "id":31010025,
         "name":"Contoso App 2 Campaign",
         "createDate":"2016-07-14T10:02:21Z",
         "startDate":"2016-07-21T11:18:44Z",
         "endDate":"2016-08-21T11:18:44Z",
         "applicationId":"9WZDNCRDX48S",
         "budget": 50,
         "budgetType":"Total",
         "currencyCode": "USD",
         "type": "Paid",
         "status": "Ended",
         "targetingType": "Manual",
         "target": {
            "Country": [
               "US",
               "BR",
               "FR",
               "IN",
               "IT"
            ],
            "OsVersion": [
               "8.X"
            ],
            "Age": [
               "Age18To24",
               "Age25To34",
               "Age35To49",
               "Age50AndAbove"
            ],
            "Gender": [
               "Male"
            ],
            "DeviceType": [
               "PC/Tablet",
               "Phone"
            ]
         }
      }
   ]
}
```

## Temas relacionados

* [Informe de anuncios de instalación de aplicaciones](../publish/app-install-ads-reports.md)
* [Crear una campaña publicitaria para tu aplicación](https://msdn.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app)
* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)



<!--HONumber=Nov16_HO1-->


