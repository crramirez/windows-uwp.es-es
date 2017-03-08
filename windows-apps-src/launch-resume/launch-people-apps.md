---
author: TylerMSFT
title: "Iniciar la aplicación Contactos"
description: "En este tema se describe el esquema de URI ms-people. La aplicación puede usar este esquema de URI para iniciar la aplicación Contactos para acciones específicas."
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 25abea87eaf374a1a8b5432c522d51bd7bbc1c1e
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-people-app"></a>Iniciar la aplicación Contactos


\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este tema se describe el esquema de URI **ms-people:**. La aplicación puede usar este esquema de URI para iniciar la aplicación Personas para acciones específicas.

## <a name="ms-people-uri-scheme-reference"></a>Referencia del esquema de URI ms-people:


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Resultados</th>
<th align="left">Esquema de URI</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Permite que otras aplicaciones inicien la página principal de la aplicación Personas.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Permite que otras aplicaciones inicien la página de configuración de la aplicación Personas.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Permite que otras aplicaciones proporcionen una cadena de búsqueda que iniciará la aplicación Personas con la página de resultados de la búsqueda.
<div class="alert">
**Nota**  
<p>Los parámetros distinguen mayúsculas de minúsculas.</p>
<p>Si no especificas la sintaxis correctamente o falta el valor de cadena de búsqueda, el comportamiento predeterminado es devolver una lista completa de los contactos sin ningún filtro.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Se inicia en una tarjeta de contacto existente, si se encuentra el contacto. O bien, se inicia en una tarjeta de contacto temporal, si no se encuentra ningún contacto. Si no se proporciona ningún parámetro de entrada, se iniciará la aplicación Personas con una lista de contactos.
<div class="alert">
**Nota**  
<p>Los parámetros distinguen mayúsculas de minúsculas.</p>
<p>No importa el orden de los parámetros.</p>
<p>Si hay más de una coincidencia, se devolverá la primera coincidencia del contacto.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Se inicia en una página de guardado de contactos en la aplicación Contactos para guardar el contacto determinado con la dirección de correo electrónico o el número de teléfono proporcionados.
<div class="alert">
**Nota**  
<p>Los parámetros distinguen mayúsculas de minúsculas.</p>
<p>No importa el orden de los parámetros.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

 

## <a name="ms-peoplesearch-parameter-reference"></a>Referencia del parámetro ms-people:search:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parámetro</th>
<th align="left">Descripción</th>
<th align="left">Ejemplo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**SearchString**</td>
<td align="left"><p>Opcional.</p>
<p>La cadena de búsqueda para obtener la información de la búsqueda de contactos.</p>
<p>El número de teléfono o el nombre del contacto.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

 

## <a name="ms-peopleviewcontact-parameter-reference"></a>Referencia del parámetro ms-people:viewcontact:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parámetro</th>
<th align="left">Descripción</th>
<th align="left">Ejemplo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**ContactId**</td>
<td align="left"><p>Opcional.</p>
<p>Identificador de contacto de un contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Opcional.</p>
<p>Número de teléfono del contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left">**Email**</td>
<td align="left"><p>Opcional.</p>
<p>Correo electrónico del contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left">**ContactName**</td>
<td align="left"><p>Opcional.</p>
<p>Nombre del contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left">**Contact**</td>
<td align="left"><p>Opcional.</p>
<p>Objeto Contact.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

 

## <a name="ms-peoplesavetocontact-parameter-reference"></a>Referencia del parámetro ms-people:savetocontact:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parámetro</th>
<th align="left">Descripción</th>
<th align="left">Ejemplo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Opcional.</p>
<p>Número de teléfono del contacto.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left">**Email**</td>
<td align="left"><p>Opcional.</p>
<p>Correo electrónico del contacto.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left">**ContactName**</td>
<td align="left"><p>Opcional.</p>
<p>Nombre del contacto.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

 

 

 

