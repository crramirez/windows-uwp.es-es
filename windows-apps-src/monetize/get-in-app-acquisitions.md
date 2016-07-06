---
author: mcleanbyron
ms.assetid: 1599605B-4243-4081-8D14-40F6F7734E25
description: "Usa este método en la API de análisis de la Tienda Windows para obtener los datos de compra agregados de un producto desde la aplicación (IAP), durante un intervalo de fechas especificado y otros filtros opcionales."
title: Obtener adquisiciones de IAP
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: 21e634b1d5ab6c3ba7762c1b83c94d076d094af5

---

# Obtener adquisiciones de IAP


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa este método en la API de análisis de la Tienda Windows para obtener los datos de compra agregados de un producto desde la aplicación (IAP), durante un intervalo de fechas especificado y otros filtros opcionales. Este método devuelve los datos en formato JSON.

## Requisitos previos


Para usar este método, necesitas lo siguiente:

-   Asociar la aplicación de Azure AD que usarás para llamar a este método con la cuenta del Centro de desarrollo.

-   Obtener un token de acceso de Azure AD para la aplicación.

Para más información, consulta [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md).

## Solicitud


### Sintaxis de la solicitud

| Método | URI de solicitud                                                                |
|--------|----------------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions |

 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. El token de acceso de Azure AD del formulario **Bearer**&lt;*token*&gt;. |

 

### Cuerpo de la solicitud

El parámetro *applicationId* o *inAppProductId* es obligatorio. Para recuperar los datos de compra de todos los IAP registrados en la aplicación, especifica el parámetro *applicationId*. Para recuperar los datos de compra de un solo IAP, especifica el parámetro *inAppProductId*. Si especificas ambos, el parámetro *inAppProductId* se pasará por alto.

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parámetro</th>
<th align="left">Tipo</th>
<th align="left">Descripción</th>
<th align="left">Obligatorio</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">applicationId</td>
<td align="left">cadena</td>
<td align="left">El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos de compra de IAP. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8.</td>
<td align="left">Sí</td>
</tr>
<tr class="even">
<td align="left">inAppProductId</td>
<td align="left">cadena</td>
<td align="left">El id. del producto del IAP sobre el que quieres recuperar los datos de compra.</td>
<td align="left">Sí</td>
</tr>
<tr class="odd">
<td align="left">startDate</td>
<td align="left">fecha</td>
<td align="left">Fecha de inicio del intervalo de fechas de los datos de compra del IAP que se han de recuperar. El valor predeterminado es la fecha actual.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">endDate</td>
<td align="left">fecha</td>
<td align="left">Fecha de finalización del intervalo de fechas de los datos de compra que se han de recuperar del IAP. El valor predeterminado es la fecha actual.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">top</td>
<td align="left">entero</td>
<td align="left">Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">skip</td>
<td align="left">entero</td>
<td align="left">Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">filter</td>
<td align="left">cadena</td>
<td align="left">Una o más instrucciones que filtran las filas en la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">aggregationLevel</td>
<td align="left">cadena</td>
<td align="left">Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">cadena</td>
<td align="left">Instrucción que ordena los valores de datos resultantes de cada compra de IAP. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:
<ul>
<li><strong>fecha</strong></li>
<li><strong>acquisitionType</strong></li>
<li><strong>ageGroup</strong></li>
<li><strong>storeClient</strong></li>
<li><strong>gender</strong></li>
<li><strong>market</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>orderName</strong></li>
</ul>
<p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p>
<p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p></td>
<td align="left">No</td>
</tr>
</tbody>
</table>

 

### Campos de filtro

El parámetro *filter* del cuerpo de la solicitud contiene una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Estos son algunos ejemplos de parámetros *filter*:

-   *filter=market eq 'US' and gender eq 'm'*
-   *filter=(market ne 'US') and (gender ne 'Desconocido') and (gender ne 'm') and (market ne 'NO') and (ageGroup ne 'mayor que 55' or ageGroup ne ‘menor que 13’)*

Para obtener una lista de los campos compatibles, consulta la tabla siguiente. Ten en cuenta que los valores de la cadena deben estar entre comillas simples en el parámetro *filter*.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Campo</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">acquisitionType</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>gratuita</strong></li>
<li><strong>versión de prueba</strong></li>
<li><strong>de pago</strong></li>
<li><strong>código promocional</strong></li>
<li><strong>IAP</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">ageGroup</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>menor que 13</strong></li>
<li><strong>13-17</strong></li>
<li><strong>18-24</strong></li>
<li><strong>25-34</strong></li>
<li><strong>35-44</strong></li>
<li><strong>44-55</strong></li>
<li><strong>mayor que 55</strong></li>
<li><strong>Desconocido</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">storeClient</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>Tienda de Windows Phone (cliente)</strong></li>
<li><strong>Tienda Windows (cliente)</strong></li>
<li><strong>Tienda Windows (web)</strong></li>
<li><strong>Compras por volumen de empresas</strong></li>
<li><strong>Otros</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">gender</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>m</strong></li>
<li><strong>f</strong></li>
<li><strong>Desconocido</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">market</td>
<td align="left">Es una cadena que contiene el código de país ISO 3166 del mercado donde se realizó la compra.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>Windows Phone 7.5</strong></li>
<li><strong>Windows Phone 8</strong></li>
<li><strong>Windows Phone 8.1</strong></li>
<li><strong>Windows Phone 10</strong></li>
<li><strong>Windows 8</strong></li>
<li><strong>Windows 8.1</strong></li>
<li><strong>Windows 10</strong></li>
<li><strong>Desconocido</strong></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">deviceType</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tableta</strong></li>
<li><strong>Teléfono</strong></li>
<li><strong>IoT</strong></li>
<li><strong>Dispositivo transportable</strong></li>
<li><strong>Servidor</strong></li>
<li><strong>De colaboración</strong></li>
<li><strong>Otros</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">orderName</td>
<td align="left">Es una cadena que especifica el nombre del pedido del código promocional que se usó para comprar la aplicación (esto solo es aplicable si el usuario adquirió la aplicación canjeando un código promocional).</td>
</tr>
</tbody>
</table>

 

### Ejemplo de solicitud

En los siguientes ejemplos se muestran varias solicitudes para obtener los datos de compra del IAP. Reemplaza los valores *inAppProductId* y *applicationId* por los correspondientes id. de producto del IAP e Id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?inAppProductId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=7/3/2015&top=100&skip=0&filter=market ne 'US' and gender ne 'Unknown' and gender ne 'm' and market ne 'NO' and ageGroup ne '>55' HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta


### Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                                |
|------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Valor      | matriz  | Matriz de objetos que contienen los datos de compra agregados del IAP. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra del IAP](#iap-acquisition-values) que encontrarás a continuación.                                                                                                              |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra del IAP de la solicitud. |
| TotalCount | entero    | Número total de filas en el resultado de datos de la consulta.                                                                                                                                                                                                                                 |


### Valores de compra del IAP

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo    | Descripción                                                                                                                                                                                                                              |
|---------------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | cadena  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| inAppProductId      | cadena  | Identificador de producto del IAP de la cual recuperas los datos de compra.                                                                                                                                                                 |
| inAppProductName    | cadena  | El nombre para mostrar del IAP.                                                                                                                                                                                                             |
| applicationId       | cadena  | El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos de compra de IAP.                                                                                                                                                           |
| applicationName     | cadena  | Nombre para mostrar de la aplicación.                                                                                                                                                                                                             |
| deviceType          | cadena  | Tipo de dispositivo que completó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                  |
| orderName           | cadena  | Nombre del pedido.                                                                                                                                                                                                                   |
| storeClient         | cadena  | Versión de la Tienda donde se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                            |
| osVersion           | cadena  | Versión del sistema operativo en el que se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                   |
| market              | cadena  | Código de país ISO 3166 del mercado donde se realizó la compra.                                                                                                                                                                  |
| gender              | cadena  | Género del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                    |
| ageGroup            | cadena  | Grupo de edad del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                 |
| acquisitionType     | cadena  | Tipo de compra (gratuita, de pago, etc.). Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                    |
| acquisitionQuantity | número | Número de compras que se realizaron.                                                                                                                                                                                                |

 

### Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo, realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-01-02",
      "inAppProductId": "9NBLGGH3LHKL",
      "inAppProductName": "Contoso IAP 7",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "deviceType": "Phone",
      "orderName": "",
      "storeClient": "Windows Phone Store (client)",
      "osVersion": "Windows Phone 8.1",
      "market": "GB",
      "gender": "m",
      "ageGroup": "50orover",
      "acquisitionType": "Iap",
      "acquisitionQuantity": 1
    }
  ],
  "@nextLink": "inappacquisitions?applicationId=9NBLGGGZ5QDR&inAppProductId=&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 33677
}
```

## Temas relacionados

* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Get app acquisitions (Obtener los datos de compra de la aplicación)](get-app-acquisitions.md)
* [Get error reporting data (Obtener los datos del informe de errores)](get-error-reporting-data.md)
* [Get app ratings (Obtener la clasificación de la aplicación)](get-app-ratings.md)
* [Get app reviews (Obtener opiniones de la aplicación)](get-app-reviews.md)

 

 



<!--HONumber=Jun16_HO5-->


