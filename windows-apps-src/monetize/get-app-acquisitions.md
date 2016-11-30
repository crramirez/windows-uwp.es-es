---
author: mcleanbyron
ms.assetid: C1E42E8B-B97D-4B09-9326-25E968680A0F
description: "Usa este método en la API de análisis de la Tienda Windows para obtener los datos de compra agregados de una aplicación durante un intervalo de fechas especificado y según otros filtros opcionales."
title: "Obtener los datos de compra de la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: 7b73682ea36574f8b675193a174d6e4b4ef85841
ms.openlocfilehash: db271b0d1ec3b20ab2ead2e35e06fd97adb2ce0c

---

# Obtener los datos de compra de la aplicación


Usa este método en la API de análisis de la Tienda Windows para obtener los datos de compra agregados en formato JSON de una aplicación pertenecientes a un intervalo de fechas dado y según otros filtros opcionales. Esta información también está disponible en el [informe de adquisiciones](../publish/acquisitions-report.md) del panel del Centro de desarrollo de Windows.

## Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## Solicitud


### Sintaxis de la solicitud

| Método | URI de la solicitud                                                              |
|--------|--------------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions``` |

<span/>

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. Token de acceso de Azure AD con el formato **Bearer** &lt;*token*&gt;. |

<span/> 

### Parámetros de solicitud

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
<td align="left">El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos de compra. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8.</td>
<td align="left">Sí</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">fecha</td>
<td align="left">La fecha de inicio del intervalo de fechas de los datos de compra que se han de recuperar. El valor predeterminado es la fecha actual.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">fecha</td>
<td align="left">Fecha de finalización del intervalo de fechas de los datos de compra que se han de recuperar. El valor predeterminado es la fecha actual.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">top</td>
<td align="left">entero</td>
<td align="left">Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">skip</td>
<td align="left">entero</td>
<td align="left">Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">filter</td>
<td align="left">cadena</td>
<td align="left">Una o más instrucciones que filtran las filas en la respuesta. Para obtener más información, consulta la sección [filtrar campos](#filter-fields) a continuación.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">aggregationLevel</td>
<td align="left">cadena</td>
<td align="left">Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">orderby</td>
<td align="left">cadena</td>
<td align="left">Instrucción que ordena los valores de datos resultantes de cada compra. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:
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
<tr class="odd">
<td align="left">groupby</td>
<td align="left">cadena</td>
<td align="left"><p>Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:</p>
<ul>
<li><strong>fecha</strong></li>
<li><strong>applicationName</strong></li>
<li><strong>acquisitionType</strong></li>
<li><strong>ageGroup</strong></li>
<li><strong>storeClient</strong></li>
<li><strong>gender</strong></li>
<li><strong>market</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>orderName</strong></li>
</ul>
<p>Las filas de datos que se devuelvan contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p>
<ul>
<li><strong>date</strong></li>
<li><strong>applicationId</strong></li>
<li><strong>acquisitionQuantity</strong></li>
</ul>
<p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=ageGroup,market&amp;aggregationLevel=week</em></p></td>
<td align="left"></td>
</tr>
</tbody>
</table>

<span/>
 
### Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**. Estos son algunos ejemplos de parámetros *filter*:

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
<th align="left">Campos</th>
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

<span/> 

### Ejemplo de solicitud

El siguiente ejemplo muestra varias solicitudes para obtener los datos de compra de la aplicación. Reemplaza el valor *applicationId* por el Id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US'; and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta


### Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | matriz  | Matriz de objetos que contienen datos de clasificación agregados. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de compra](#acquisition-values) que encontrarás a continuación.                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra de la solicitud. |
| TotalCount | entero    | Número total de filas en el resultado de datos de la consulta.                                                                                                                                                                                                                             |

<span/>
 
### Valores de compra

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor               | Tipo   | Descripción                                                                                                                                                                                                                              |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                | cadena | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId       | cadena | El Id. de la Tienda de la aplicación sobre la que estás recuperando los datos de compra.                                                                                                                                                                 |
| applicationName     | cadena | Nombre para mostrar de la aplicación.                                                                                                                                                                                                             |
| deviceType          | cadena | Tipo de dispositivo que completó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                  |
| orderName           | cadena | Nombre del pedido.                                                                                                                                                                                                                   |
| storeClient         | cadena | Versión de la Tienda donde se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                            |
| osVersion           | cadena | Versión del sistema operativo en el que se realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                   |
| market              | cadena | Código de país ISO 3166 del mercado donde se realizó la compra.                                                                                                                                                                  |
| gender              | cadena | Género del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                    |
| ageGroup            | cadena | Grupo de edad del usuario que realizó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                 |
| acquisitionType     | cadena | Tipo de compra (gratuita, de pago, etc.). Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                    |
| acquisitionQuantity | número | Número de compras que se realizaron durante el nivel de agregación especificado.                                                                                                                                                         |

<span/> 

### Ejemplo de respuesta

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

## Temas relacionados

* [Informe de adquisiciones](../publish/acquisitions-report.md)
* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener la clasificación de la aplicación](get-app-ratings.md)
* [Obtener las opiniones de la aplicación](get-app-reviews.md)



<!--HONumber=Nov16_HO1-->


