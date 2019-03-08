---
title: Iniciar la aplicación Microsoft Store
description: En este tema se describe el esquema de URI ms-windows-store. La aplicación puede utilizar este esquema de URI para iniciar la aplicación de Microsoft Store a páginas específicas en el Store.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cda37ee9964a3e7e02f4e4ce3829a8b55e823692
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660900"
---
# <a name="launch-the-microsoft-store-app"></a>Iniciar la aplicación Microsoft Store



Este tema se describe la **ms-windows-store:** Esquema de URI. La aplicación puede usar este esquema de URI para iniciar la aplicación de Microsoft Store a páginas específicas en el almacén mediante el uso de la [ **LaunchUriAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701476) método.

En este ejemplo se muestra cómo abrir Microsoft Store a la página Juegos:

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>ms-windows-store: Referencia de esquema URI

<table>
<tr><th>Descripción</th><th></th><th>Esquema de URI</th></tr>
<tr><td>Inicia la página principal de la Tienda.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>Inicia un segmento vertical de nivel superior en la Tienda.<p>Nota: No todos los usuarios tengan acceso a todos los mercados verticales.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">Inicia la página de detalles del producto (PDP) de un producto. <p>Se recomienda para los clientes de Windows 10 Store ID y funcionará en todas las versiones de sistema operativo, pero las formas de la operación anteriores (p. ej.: PFN) son todavía compatibles.</p>
<p>Estos valores se pueden encontrar en <a href="https://partner.microsoft.com/dashboard">centro de partners</a> en el <a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">identidad de aplicación</a> página en la sección de administración de la aplicación para cada aplicación.</p>
</td>
<td>
Id. de Store <p>(recomendado)</p>
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
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d</td>
</tr>
<tr>
<td>Id. de producto (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">Inicia la escritura de una experiencia de revisión de un producto.</td>
<td>Id. de Store <p>(recomendado)</p></td>
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

 

 
