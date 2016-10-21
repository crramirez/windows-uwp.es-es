---
author: TylerMSFT
title: "Iniciar la aplicación de la Tienda Windows"
description: "En este tema se describe el esquema de URI ms-windows-store. La aplicación puede usar este esquema de URI para iniciar la aplicación de la Tienda Windows en páginas específicas en la Tienda."
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
translationtype: Human Translation
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: b66ae37adec1b68653c0fe7d552a84f61d57acd9

---

# Iniciar la aplicación de la Tienda Windows


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tema se describe el esquema de URI **ms-windows-store:**. La aplicación puede usar este esquema de URI para iniciar la aplicación de la Tienda Windows en páginas específicas en la tienda.

## referencia del esquema de URI ms-windows-store:

<table>
<tr><th>Descripción</th><th></th><th>Esquema de URI</th></tr>
<tr><td>Inicia la página principal de la Tienda.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>Inicia un segmento vertical de nivel superior en la Tienda.<p>Nota: no todos los usuarios tienen acceso a todos los segmentos verticales.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">Inicia la página de detalles del producto (PDP) de un producto. <p>El Id. de la Tienda se recomienda para los clientes de Windows 10 y funcionará en todas las versiones del sistema operativo, pero aún se admiten las formas anteriores de hacerlo anteriores (por ejemplo, PFN).</p>
<p>Estos valores se pueden encontrar en el panel del Centro de desarrollo de Windows en la página <a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">Identidad de la aplicación</a> en la sección de administración de cada aplicación.</p>
</td>
<td>
Id. de la Tienda <p>(recomendado)</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>Nombre de familia de paquete (PFN)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>Id. de producto (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>Id. de producto (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">Inicia la escritura de una experiencia de revisión de un producto.</td>
<td>Id. de la Tienda <p>(recomendado)</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>Nombre de familia de paquete (PFN)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>Id. de producto (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>Id. de producto (Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>Inicia una búsqueda de productos asociados a una extensión de archivo. </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>Inicia una búsqueda de productos asociados a un protocolo.</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>Inicia una búsqueda de productos asociados a una o varias etiquetas. Las etiquetas deben separarse por comas.
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
Inicia una búsqueda de la consulta especificada. Se permiten los espacios en la consulta.
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>Inicia una búsqueda de productos en una categoría.</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>Inicia una búsqueda de productos del publicador especificado. Se permiten los espacios en el nombre.
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>Inicia las páginas de descargas y actualizaciones.</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>Inicia la página de configuración de la Tienda.</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 



<!--HONumber=Aug16_HO3-->


