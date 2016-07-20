---
author: mcleanbyron
ms.assetid: DD4F6BC4-67CD-4AEF-9444-F184353B0072
description: "Usa este método en la API de análisis de la Tienda Windows para obtener los datos de clasificación agregados de un intervalo de fechas y otros filtros opcionales."
title: "Obtener la clasificación de la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: f7e67a4ff6cb900fb90c5d5643e2ddc46cbe4dd2
ms.openlocfilehash: 6f6a94e030f1733ca4224766526386ef1956ff03

---

# Obtener la clasificación de la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa este método en la API de análisis de la Tienda Windows para obtener los datos de clasificación agregados de un intervalo de fechas y otros filtros opcionales. Este método devuelve los datos en formato JSON.

## Requisitos previos


Para usar este método, necesitas lo siguiente:

-   Asociar la aplicación de Azure AD que usarás para llamar a este método con la cuenta del Centro de desarrollo.

-   Obtener un token de acceso de Azure AD para la aplicación.

Para obtener más información, consulta [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md).

## Solicitud


### Sintaxis de la solicitud

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings``` |

 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. El token de acceso de Azure AD con la forma **Portador**&lt;*token*&gt;. |

<span/> 

### Parámetros de la solicitud

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
<td align="left">El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos de clasificación. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8.</td>
<td align="left">Sí</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">fecha</td>
<td align="left">La fecha de inicio del intervalo de fechas de los datos de clasificación que se han de recuperar. El valor predeterminado es la fecha actual.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">fecha</td>
<td align="left">Fecha de finalización del intervalo de fechas de los datos de clasificación que se han de recuperar. El valor predeterminado es la fecha actual.</td>
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
<td align="left">Instrucción que ordena los valores de datos resultantes de cada clasificación. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
</ul>
<p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p>
<p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p></td>
<td align="left">No</td>
</tr>
</tbody>
</table>

<span/>
 
### Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas en la respuesta. Cada instrucción contiene un campo y un valor asociados a los operadores **eq** o **ne**; asimismo, puedes combinar las instrucciones mediante **and** u **or**.

Este es un ejemplo de la cadena *filter*: *filter=market eq 'US' and deviceType eq 'teléfono' and isRevised eq true*

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
<td align="left">market</td>
<td align="left">Cadena que contiene el código de país ISO 3166 del mercado del dispositivo.</td>
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
<td align="left">isRevised</td>
<td align="left">Especifica <strong>true</strong> para filtrar las clasificaciones que hayan sido revisadas; de lo contrario, especifica <strong>false</strong>.</td>
</tr>
</tbody>
</table>

<span/> 

### Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de clasificación. Reemplaza el valor *applicationId* por el Id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta


### Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | matriz  | Matriz de objetos que contienen datos de clasificación agregados. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de clasificación](#rating-values) que encontrarás a continuación.                                                                                                                           |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra de la solicitud. |
| TotalCount | entero    | Número total de filas en el resultado de datos de la consulta.                                                                                                                                                                                                                             |

<span/>

### Valores de clasificación

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date            | cadena  | Es la primera fecha del intervalo de fechas de los datos de clasificación. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | cadena  | El Id. de la Tienda de la aplicación sobre la que estás recuperando los datos de clasificación.                                                                                                                                                                 |
| applicationName | cadena  | Nombre para mostrar de la aplicación.                                                                                                                                                                                                         |
| market          | cadena  | Código de país ISO 3166 del mercado desde el cual se envió la clasificación.                                                                                                                                                              |
| osVersion       | cadena  | Versión del sistema operativo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                               |
| deviceType      | cadena  | Tipo de dispositivo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                           |
| isRevised       | Booleano | El valor **true** indica que la clasificación fue revisada; en caso contrario, será **false**.                                                                                                                                                       |
| oneStar         | número  | Número de clasificaciones de una estrella.                                                                                                                                                                                                      |
| twoStars        | número  | Número de clasificaciones de dos estrellas.                                                                                                                                                                                                      |
| threeStars      | número  | Número de clasificaciones de tres estrellas.                                                                                                                                                                                                    |
| fourStars       | número  | Número de clasificaciones de cuatro estrellas.                                                                                                                                                                                                     |
| fiveStars       | número  | Número de clasificaciones de cinco estrellas.                                                                                                                                                                                                     |
 
<span/>

### Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo, realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-02-13",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "CN",
      "osVersion": "8.0.10517.0",
      "deviceType": "Phone",
      "isRevised": false,
      "oneStar": 0,
      "twoStars": 0,
      "threeStars": 0,
      "fourStars": 0,
      "fiveStars": 2
    }
  ],
  "@nextLink": "ratings?applicationId=9NBLGGGZ5QDR&aggregationLevel=day&startDate=2015/01/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 15242
}

```

## Temas relacionados

* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Get app acquisitions (Obtener los datos de compra de la aplicación)](get-app-acquisitions.md)
* [Get IAP acquisitions (Obtener los datos de compra del IAP)](get-in-app-acquisitions.md)
* [Get error reporting data (Obtener los datos del informe de errores)](get-error-reporting-data.md)
* [Get app reviews (Obtener opiniones de la aplicación)](get-app-reviews.md)



<!--HONumber=Jul16_HO1-->


