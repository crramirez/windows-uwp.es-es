---
title: Iniciar la aplicación Contactos
description: En este tema se describe el esquema de URI ms-people. La aplicación puede usar este esquema de URI para iniciar la aplicación Contactos para acciones específicas.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 46a297c3a611882724b18242d1c6272c3345ffc2
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8472356"
---
# <a name="launch-the-people-app"></a>Iniciar la aplicación Contactos

En este tema se describe el esquema de URI **ms-people:**. La aplicación puede usar este esquema de URI para iniciar la aplicación Contactos para acciones específicas.

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
<td align="left">Permite que otras aplicaciones inicien la página principal de la aplicación Contactos.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Permite que otras aplicaciones inicien la página de configuración de la aplicación Contactos.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Permite que otras aplicaciones proporcionen una cadena de búsqueda que iniciará la aplicación Contactos con la página de resultados de la búsqueda.
<div class="alert">
<p>Los parámetros distinguen mayúsculas de minúsculas.</p>
<p>Si no especificas la sintaxis correctamente o falta el valor de cadena de búsqueda, el comportamiento predeterminado es devolver una lista completa de los contactos sin ningún filtro.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Se inicia en una tarjeta de contacto existente, si se encuentra el contacto. O bien, se inicia en una tarjeta de contacto temporal, si no se encuentra ningún contacto. Si no se proporciona ningún parámetro de entrada, se iniciará la aplicación Contactos con una lista de contactos.
<div class="alert">
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
<p>Los parámetros distinguen mayúsculas de minúsculas.</p>
<p>No importa el orden de los parámetros.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
<tr class="even">
<td align="left">Se inicia para agregar una página de contacto nueva en la aplicación Contactos para guardar el contacto determinado.
<div class="alert"><p>Usa <a href="https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a> para abrir la página para guardar contacto nuevo. Con <strong>LaunchUriAsync</strong> solo se iniciará la página principal de la aplicación Contactos.</p>
<p>Los parámetros distinguen mayúsculas de minúsculas.</p>
<p>No importa el orden de los parámetros.</p>
<p>Puede usar cualquier combinación de parámetros admitidos.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savecontacttask?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
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
<td align="left"><b>SearchString</b></td>
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
<td align="left"><b>ContactId</b></td>
<td align="left"><p>Opcional.</p>
<p>Identificador de contacto de un contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de teléfono del contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Email</b></td>
<td align="left"><p>Opcional.</p>
<p>Correo electrónico del contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Opcional.</p>
<p>Nombre del contacto.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Contact</b></td>
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
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de teléfono del contacto.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>Email</b></td>
<td align="left"><p>Opcional.</p>
<p>Correo electrónico del contacto.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Opcional.</p>
<p>Nombre del contacto.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>Referencia del parámetro ms-people:savecontacttask:

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

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Company</b></td>
<td align="left"><p>Opcional.</p>
<p>Nombre de la compañía del contacto.</p></td>

</tr>
<tr class="even">
<td align="left"><b>FirstName</b></td>
<td align="left"><p>Opcional.</p>
<p>Nombre del contacto.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>Opcional.</p>
<p>Ciudad de la dirección particular.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>Opcional.</p>
<p>País de la dirección particular.</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>Opcional.</p>
<p>Estado de la dirección particular.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>Opcional.</p>
<p>Calle de la dirección particular.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>Opcional.</p>
<p>Código postal de la dirección particular.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomePhone</b></td>
<td align="left"><p>Opcional.</p>
<p>Teléfono particular del contacto.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>Opcional.</p>
<p>Puesto del contacto.</p></td>
</tr>

<tr class="even">
<td align="left"><b>LastName</b></td>
<td align="left"><p>Opcional.</p>
<p>Apellidos del contacto.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>MiddleName</b></td>
<td align="left"><p>Opcional.</p>
<p>Segundo nombre del contacto.</p></td>
</tr>

<tr class="even">
<td align="left"><b>MobilePhone</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de teléfono móvil del contacto.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Nickname</b></td>
<td align="left"><p>Opcional.</p>
<p>Alias del contacto.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Notes</b></td>
<td align="left"><p>Opcional.</p>
<p>Notas acerca del contacto.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>Opcional.</p>
<p>Otro correo electrónico del contacto.</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>Opcional.</p>
<p>Correo electrónico personal del contacto.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Suffix</b></td>
<td align="left"><p>Opcional.</p>
<p>Sufijo del contacto.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Title</b></td>
<td align="left"><p>Opcional.</p>
<p>Título del contacto.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Website</b></td>
<td align="left"><p>Opcional.</p>
<p>Sitio web del contacto.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressCity</b></td>
<td align="left"><p>Opcional.</p>
<p>Ciudad de la dirección del trabajo.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressCountry</b></td>
<td align="left"><p>Opcional.</p>
<p>País de la dirección del trabajo.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressState</b></td>
<td align="left"><p>Opcional.</p>
<p>Estado de la dirección del trabajo.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressStreet</b></td>
<td align="left"><p>Opcional.</p>
<p>Calle de la dirección del trabajo.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>Opcional.</p>
<p>Código postal de la dirección del trabajo.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkEmail</b></td>
<td align="left"><p>Opcional.</p>
<p>Correo electrónico del trabajo del contacto.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkPhone</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de teléfono del trabajo del contacto.</p></td>
</tr>
</tbody>
</table>
