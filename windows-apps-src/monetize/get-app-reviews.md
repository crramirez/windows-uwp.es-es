---
author: mcleanbyron
ms.assetid: 2967C757-9D8A-4B37-8AA4-A325F7A060C5
description: "Usa este método en la API de análisis de la Tienda Windows para obtener los datos de opiniones de un intervalo de fechas proporcionado y otros filtros opcionales."
title: "Obtener opiniones de la aplicación"
translationtype: Human Translation
ms.sourcegitcommit: 6d0fa3d3b57bcc01234aac7d6856416fcf9f4419
ms.openlocfilehash: 4190012c08e22f4efb086c711183332b23ccee38

---

# Obtener opiniones de la aplicación




Usa este método en la API de análisis de la Tienda Windows para obtener los datos de opiniones de un intervalo de fechas proporcionado y otros filtros opcionales. Este método devuelve los datos en formato JSON.

## Requisitos previos


Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de la Tienda Windows.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## Solicitud


### Sintaxis de la solicitud

| Método | URI de la solicitud                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews``` |

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
<td align="left">El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos de revisión. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8.</td>
<td align="left">Sí</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">fecha</td>
<td align="left">La fecha de inicio del intervalo de fechas de los datos de revisión que se han de recuperar. El valor predeterminado es la fecha actual.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">fecha</td>
<td align="left">Fecha de finalización del intervalo de fechas de los datos de revisión que se han de recuperar. El valor predeterminado es la fecha actual.</td>
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
<td align="left">orderby</td>
<td align="left">cadena</td>
<td align="left">Instrucción que ordena los valores de datos resultantes de cada clasificación. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:
<ul>
<li><strong>date</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>isRevised</strong></li>
<li><strong>packageVersion</strong></li>
<li><strong>deviceModel</strong></li>
<li><strong>productFamily</strong></li>
<li><strong>deviceScreenResolution</strong></li>
<li><strong>isTouchEnabled</strong></li>
<li><strong>reviewerName</strong></li>
<li><strong>reviewTitle</strong></li>
<li><strong>reviewText</strong></li>
<li><strong>helpfulCount</strong></li>
<li><strong>notHelpfulCount</strong></li>
<li><strong>responseDate</strong></li>
<li><strong>responseText</strong></li>
<li><strong>deviceRAM</strong></li>
<li><strong>deviceStorageCapacity</strong></li>
<li><strong>rating</strong></li>
</ul>
<p>El parámetro <em>order</em>, en cambio, es opcional y puede ser <strong>asc</strong> o <strong>desc</strong> para especificar el orden ascendente o descendente de cada campo. El valor predeterminado es <strong>asc</strong>.</p>
<p>Aquí tienes un ejemplo de una cadena <em>orderby</em>: <em>orderby=date,market</em></p></td>
<td align="left">No</td>
</tr>
</tbody>
</table>

<span/>
 
### Campos de filtro

El parámetro *filter* de la solicitud contiene una o más instrucciones que filtran las filas de la respuesta. Cada instrucción contiene un campo y un elemento que se asocian a los operadores **eq** o **ne**; asimismo, algunos campos también son compatibles con los operadores **contains**, **gt**, **lt**, **ge**, y **le**. Las instrucciones se pueden combinar mediante **and** u **or**.

Este es un ejemplo de una cadena *filter*: *filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US'*

Para obtener una lista de los campos y operadores compatibles de cada campo, consulta la siguiente tabla. Ten en cuenta que los valores de la cadena deben estar entre comillas simples en el parámetro *filter*.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Campos</th>
<th align="left">Operadores compatibles</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">market</td>
<td align="left">eq, ne</td>
<td align="left">Cadena que contiene el código de país ISO 3166 del mercado del dispositivo.</td>
</tr>
<tr class="even">
<td align="left">osVersion</td>
<td align="left">eq, ne</td>
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
<td align="left">eq, ne</td>
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
<td align="left">eq, ne</td>
<td align="left">Especifica <strong>true</strong> para filtrar las opiniones que hayan sido revisadas; de lo contrario, especifica <strong>false</strong>.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">eq, ne</td>
<td align="left">Versión del paquete de aplicaciones que se revisó.</td>
</tr>
<tr class="even">
<td align="left">deviceModel</td>
<td align="left">eq, ne</td>
<td align="left">Tipo de dispositivo en el cual se revisó la aplicación.</td>
</tr>
<tr class="odd">
<td align="left">productFamily</td>
<td align="left">eq, ne</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>PC</strong></li>
<li><strong>Tableta</strong></li>
<li><strong>Teléfono</strong></li>
<li><strong>Dispositivo transportable</strong></li>
<li><strong>Servidor</strong></li>
<li><strong>De colaboración</strong></li>
<li><strong>Otros</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">deviceScreenResolution</td>
<td align="left">eq, ne</td>
<td align="left">Resolución de la pantalla del dispositivo en el formato &quot;<em>ancho</em> x <em>alto</em>&quot;.</td>
</tr>
<tr class="odd">
<td align="left">isTouchEnabled</td>
<td align="left">eq, ne</td>
<td align="left">Especifica <strong>true</strong> para filtrar según los dispositivos táctiles; en caso contrario, especifica <strong>false</strong>.</td>
</tr>
<tr class="even">
<td align="left">reviewerName</td>
<td align="left">eq, ne</td>
<td align="left">Nombre del revisor.</td>
</tr>
<tr class="odd">
<td align="left">helpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">Número de veces que la opinión se ha marcado como útil.</td>
</tr>
<tr class="even">
<td align="left">notHelpfulCount</td>
<td align="left">eq, ne</td>
<td align="left">Número de veces que la opinión se ha marcado como no útil.</td>
</tr>
<tr class="odd">
<td align="left">reviewTitle</td>
<td align="left">eq, ne, contains</td>
<td align="left">Título de la opinión.</td>
</tr>
<tr class="even">
<td align="left">reviewText</td>
<td align="left">eq, ne, contains</td>
<td align="left">Contenido de texto de la opinión.</td>
</tr>
<tr class="odd">
<td align="left">responseText</td>
<td align="left">eq, ne, contains</td>
<td align="left">Contenido de texto de la respuesta.</td>
</tr>
<tr class="even">
<td align="left">responseDate</td>
<td align="left">eq, ne</td>
<td align="left">Fecha en la que se envió la respuesta.</td>
</tr>
<tr class="odd">
<td align="left">deviceRAM</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">RAM física, en MB.</td>
</tr>
<tr class="even">
<td align="left">deviceStorageCapacity</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">Capacidad del disco de almacenamiento principal, en GB.</td>
</tr>
<tr class="odd">
<td align="left">rating</td>
<td align="left">eq, ne, gt, lt, ge, le</td>
<td align="left">Clasificación de la aplicación, en estrellas.</td>
</tr>
</tbody>
</table>

<span/> 

### Ejemplo de solicitud

Los ejemplos siguientes muestran varias solicitudes para obtener datos de revisión. Reemplaza el valor *applicationId* por el Id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=contains(reviewText,'great') and contains(reviewText,'ads') and deviceRAM lt 2048 and market eq 'US' HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta


### Cuerpo de la respuesta

| Valor      | Tipo   | Descripción                                                                                                                                                                                                                                                                            |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | matriz  | Matriz de objetos que contienen los datos de revisión. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de revisión](#review-values) que encontrarás a continuación.                                                                                                                                      |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de datos de compra de la solicitud. |
| TotalCount | entero    | Número total de filas en el resultado de datos de la consulta.                                                                                                                                                                                                                             |

<span/>
 
### Valores de revisión

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor                  | Tipo    | Descripción                                                                                                                                                                                                                          |
|------------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date                   | cadena  | Es la primera fecha del intervalo de fechas de los datos de clasificación. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, un mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId          | cadena  | El Id. de la Tienda de la aplicación sobre la que estás recuperando los datos de clasificación.                                                                                                                                                                 |
| applicationName        | cadena  | Nombre para mostrar de la aplicación.                                                                                                                                                                                                         |
| market                 | cadena  | Código de país ISO 3166 del mercado desde el cual se envió la clasificación.                                                                                                                                                              |
| osVersion              | cadena  | Versión del sistema operativo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                               |
| deviceType             | cadena  | Tipo de dispositivo desde el cual se envió la clasificación. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                           |
| isRevised              | Booleano | El valor **true** indica que la opinión fue revisada; en caso contrario, será **false**.                                                                                                                                                       |
| packageVersion         | cadena  | Versión del paquete de aplicaciones que se revisó.                                                                                                                                                                                    |
| deviceModel            | cadena  | Tipo de dispositivo en el cual se revisó la aplicación.                                                                                                                                                                                    |
| productFamily          | cadena  | Nombre de la familia de dispositivos. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                                         |
| deviceScreenResolution | cadena  | Resolución de la pantalla del dispositivo en el formato "*ancho* x *alto*".                                                                                                                                                                     |
| isTouchEnabled         | Booleano | El valor **true** indica que la función táctil está habilitada; de lo contrario el valor será **false**.                                                                                                                                                             |
| reviewerName           | cadena  | Nombre del revisor.                                                                                                                                                                                                                   |
| helpfulCount           | número  | Número de veces que la opinión se ha marcado como útil.                                                                                                                                                                                   |
| notHelpfulCount        | número  | Número de veces que la opinión se ha marcado como no útil.                                                                                                                                                                               |
| reviewTitle            | cadena  | Título de la opinión.                                                                                                                                                                                                             |
| reviewText             | cadena  | Contenido de texto de la opinión.                                                                                                                                                                                                     |
| responseText           | cadena  | Contenido de texto de la respuesta.                                                                                                                                                                                                   |
| responseDate           | cadena  | Fecha en la que se envió una respuesta.                                                                                                                                                                                                   |
| deviceRAM              | número  | RAM física, en MB.                                                                                                                                                                                                             |
| deviceStorageCapacity  | número  | Capacidad del disco de almacenamiento principal, en GB.                                                                                                                                                                                     |
| rating                 | número  | Clasificación de la aplicación, en estrellas.                                                                                                                                                                                                            |

<span/> 

### Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo, realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-07-29",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso demo",
      "market": "US",
      "osVersion": "10.0.10240.16410",
      "deviceType": "PC",
      "isRevised": true,
      "packageVersion": "",
      "deviceModel": "Microsoft Corporation-Virtual Machine",
      "productFamily": "PC",
      "deviceRAM": -1,
      "deviceScreenResolution": "1024 x 768",
      "deviceStorageCapacity": 51200,
      "isTouchEnabled": false,
      "reviewerName": "ALeksandra",
      "rating": 5,
      "reviewTitle": "Love it",
      "reviewText": "Great app for demos and great dev response.",
      "helpfulCount": 0,
      "notHelpfulCount": 0,
      "responseDate": "2015-08-07T01:50:22.9874488Z",
      "responseText": "1"
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## Temas relacionados

* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Obtener los datos de compra de la aplicación](get-app-acquisitions.md)
* [Obtener los datos de las adquisiciones de complementos](get-in-app-acquisitions.md)
* [Obtener los datos del informe de errores](get-error-reporting-data.md)
* [Obtener la clasificación de la aplicación](get-app-ratings.md)



<!--HONumber=Aug16_HO5-->


