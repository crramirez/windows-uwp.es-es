---
author: mcleanbyron
ms.assetid: 252C44DF-A2B8-4F4F-9D47-33E423F48584
description: "Usa este método en la API de análisis de la Tienda Windows para obtener los datos agregados del informe de errores de un intervalo de fechas y otros filtros opcionales."
title: Obtener los datos del informe de errores
ms.sourcegitcommit: 02131e641cdaa76256845b38bcc50aa42d718601
ms.openlocfilehash: 5b2421daf9df4ca417d5089166c0927e2b2f7436

---

# Obtener los datos del informe de errores


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa este método en la API de análisis de la Tienda Windows para obtener los datos agregados del informe de errores de un intervalo de fechas y otros filtros opcionales. Este método devuelve los datos en formato JSON.

## Requisitos previos


Para usar este método, necesitas lo siguiente:

-   Asociar la aplicación de Azure AD que usarás para llamar a este método con la cuenta del Centro de desarrollo.

-   Obtener un token de acceso de Azure AD para la aplicación.

Para más información, consulta [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md).

## Solicitud


### Sintaxis de la solicitud

| Método | URI de solicitud                                                          |
|--------|----------------------------------------------------------------------|
| GET    | https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits |

 

### Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorización | cadena | Obligatorio. El token de acceso de Azure AD del formulario **Bearer**&lt;*token*&gt;. |

 

### Cuerpo de la solicitud

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
<td align="left">El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos del informe de errores. El Id. de la Tienda está disponible en la [página Identidad de la aplicación](../publish/view-app-identity-details.md) del panel del Centro de desarrollo. Un ejemplo de un Id. de la Tienda sería 9WZDNCRFJ3Q8.</td>
<td align="left">Sí</td>
</tr>
<tr class="even">
<td align="left">startDate</td>
<td align="left">fecha</td>
<td align="left">La fecha de inicio del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual.</td>
<td align="left">No</td>
</tr>
<tr class="odd">
<td align="left">endDate</td>
<td align="left">fecha</td>
<td align="left">Fecha de finalización del intervalo de fechas de los datos del informe de errores que se han de recuperar. El valor predeterminado es la fecha actual.</td>
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
<td align="left">Especifica el intervalo de tiempo necesario para recuperar los datos agregados. Puede ser una de las siguientes cadenas: <strong>día</strong>, <strong>semana</strong> o <strong>mes</strong>. Si no se especifica, el valor predeterminado es <strong>día</strong>. Si especificas los valores <strong>semana</strong> o <strong>mes</strong>, los valores <em>failureName</em> y <em>failureHash</em> se limitarán a 1 000 depósitos.</td>
<td align="left">No</td>
</tr>
<tr class="even">
<td align="left">groupby</td>
<td align="left">cadena</td>
<td align="left">Instrucción que aplica la agregación de datos únicamente a los campos especificados. Puedes especificar los siguientes campos:
<ul>
<li><strong>failureName</strong></li>
<li><strong>failureHash</strong></li>
<li><strong>symbol</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>eventType</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>packageName</strong></li>
<li><strong>packageVersion</strong></li>
</ul>
<p>Las filas de datos que se devuelvan, contendrán los campos especificados en el parámetro <em>groupby</em> y en los siguientes:</p>
<ul>
<li><strong>date</strong></li>
<li><strong>applicationId</strong></li>
<li><strong>applicationName</strong></li>
<li><strong>deviceCount</strong></li>
<li><strong>eventCount</strong></li>
</ul>
<p>Puedes usar el parámetro <em>groupby</em> con <em>aggregationLevel</em>. Por ejemplo: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">orderby</td>
<td align="left">cadena</td>
<td align="left">Instrucción que ordena los valores de datos resultantes de cada compra. La sintaxis es <em>orderby=field [order],field [order],...</em>. El parámetro <em>field</em> puede ser una de las siguientes cadenas:
<ul>
<li><strong>date</strong></li>
<li><strong>failureName</strong></li>
<li><strong>failureHash</strong></li>
<li><strong>symbol</strong></li>
<li><strong>osVersion</strong></li>
<li><strong>eventType</strong></li>
<li><strong>market</strong></li>
<li><strong>deviceType</strong></li>
<li><strong>packageName</strong></li>
<li><strong>packageVersion</strong></li>
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
<th align="left">Campos</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">failureName</td>
<td align="left">El nombre del error.</td>
</tr>
<tr class="even">
<td align="left">failureHash</td>
<td align="left">Identificador único del error.</td>
</tr>
<tr class="odd">
<td align="left">symbol</td>
<td align="left">Símbolo que se asigna al error.</td>
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
<td align="left">eventType</td>
<td align="left">Una de las cadenas siguientes:
<ul>
<li><strong>bloquear</strong></li>
<li><strong>colgar</strong></li>
<li><strong>memoria</strong></li>
<li><strong>jse</strong></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">market</td>
<td align="left">Cadena que contiene el código de país ISO 3166 del mercado del dispositivo.</td>
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
<td align="left">packageName</td>
<td align="left">Nombre único del paquete de la aplicación que está asociado al error.</td>
</tr>
<tr class="odd">
<td align="left">packageVersion</td>
<td align="left">Versión del paquete de la aplicación que está asociado al error.</td>
</tr>
</tbody>
</table>

 

### Ejemplo de solicitud

Los siguientes ejemplos muestran varias solicitudes para obtener los datos del informe de errores. Reemplaza el valor *applicationId* por el Id. de la Tienda de la aplicación.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId=9NBLGGGZ5QDR&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'phone' HTTP/1.1
Authorization: Bearer <your access token>
```

## Respuesta


### Cuerpo de la respuesta

| Valor      | Tipo    | Descripción                                                                                                                                                                                                                                                                    |
|------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Value      | matriz   | Matriz de objetos que contienen los datos agregados del informe de errores. Para obtener más información sobre los datos de cada objeto, consulta la sección [valores de error](#error-values) que encontrarás a continuación.                                                                                                          |
| @nextLink  | cadena  | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10 000, pero resulta que hay más de 10 000 filas de errores de la solicitud. |
| TotalCount | número | Número total de filas en el resultado de datos de la consulta.                                                                                                                                                                                                                     |

 
### Valores de error

Los elementos en la matriz *Value* contienen los siguientes valores.

| Valor           | Tipo    | Descripción                                                                                                                                                                                                                              |
|-----------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| date            | cadena  | Es la primera fecha del intervalo de fechas de los datos de compra. Si la solicitud especifica un solo día, este valor será esa fecha. Si, por el contrario, la solicitud especifica una semana, mes u otro intervalo de fechas, este valor será la primera fecha de ese intervalo de fechas. |
| applicationId   | cadena  | El Id. de la Tienda de la aplicación sobre la que quieres recuperar los datos de compra de IAP.                                                                                                                                                           |
| applicationName | cadena  | Nombre para mostrar de la aplicación.                                                                                                                                                                                                             |
| failureName     | cadena  | El nombre del error.                                                                                                                                                                                                                 |
| failureHash     | cadena  | Identificador único del error.                                                                                                                                                                                                   |
| symbol          | cadena  | Símbolo que se asigna al error.                                                                                                                                                                                                       |
| osVersion       | cadena  | Versión del sistema operativo en el que sucedió el error. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                       |
| eventType       | cadena  | Tipo de evento de error. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                                          |
| market          | cadena  | Código de país ISO 3166 del mercado del dispositivo.                                                                                                                                                                                          |
| deviceType      | cadena  | Tipo de dispositivo que completó la compra. Para obtener una lista de las cadenas admitidas, consulta la sección previa [filtrar campos](#filter-fields).                                                                                                  |
| packageName     | cadena  | Nombre único del paquete de la aplicación que está asociado al error.                                                                                                                                                                 |
| packageVersion  | cadena  | Versión del paquete de la aplicación que está asociado al error.                                                                                                                                                                     |
| eventCount      | número | Número de eventos atribuidos a este error del nivel de agregación especificado.                                                                                                                                            |
| deviceCount     | número | Número de dispositivos únicos que corresponden a este error del nivel de agregación especificado.                                                                                                                                        |

 

### Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo, realizada para esta solicitud.

```json
{
  "Value": [
    {
      "date": "2015-03-09",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso Demo",
      "failureName": "APPLICATION_FAULT_8013150a_StoreWrapper.ni.DLL!70475e55",
      "failureHash": "5a6b2170-1661-ed47-24d7-230fed0077af",
      "symbol": "storewrapper_ni!70475e55",
      "osVersion": "Windows Phone 8",
      "eventType": "crash",
      "market": "US",
      "deviceType": "mobile",
      "packageName": "",
      "packageVersion": "0.0.0.0",
      "deviceCount": 0.0,
      "eventCount": 1.0
    }
  ],
  "@nextLink": "failurehits?applicationId=9NBLGGGZ5QDR&aggregationLevel=week&startDate=2015/03/01&endDate=2016/02/01&top=1&skip=1",
  "TotalCount": 191753
}

```

## Temas relacionados

* [Acceder a los datos de análisis mediante los servicios de la Tienda Windows](access-analytics-data-using-windows-store-services.md)
* [Get app acquisitions (Obtener los datos de compra de la aplicación)](get-app-acquisitions.md)
* [Get IAP acquisitions (Obtener los datos de compra del IAP)](get-in-app-acquisitions.md)
* [Get app ratings (Obtener la clasificación de la aplicación)](get-app-ratings.md)
* [Get app reviews (Obtener opiniones de la aplicación)](get-app-reviews.md)



<!--HONumber=Jun16_HO5-->


