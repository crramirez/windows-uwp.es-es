---
title: Iniciar la aplicación de Microsoft Store
description: En este tema se describe el esquema de URI ms-windows-store. La aplicación puede usar este esquema de URI para iniciar la aplicación Microsoft Store en páginas específicas del almacén.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a31c762c002e711a87e99e2f97de6c26e2c8b48
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172989"
---
# <a name="launch-the-microsoft-store-app"></a>Iniciar la aplicación de Microsoft Store



En este tema se describe el esquema de URI **ms-windows-store:**. La aplicación puede usar este esquema de URI para iniciar la aplicación Microsoft Store en páginas específicas del almacén mediante el método [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) .

En este ejemplo se muestra cómo abrir la tienda en la página juegos:

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

## <a name="ms-windows-store-uri-scheme-reference"></a>referencia del esquema de URI ms-windows-store:

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
<p>Estos valores se pueden encontrar en <a href="https://partner.microsoft.com/dashboard">centro de Partners</a> en la página identidad de la <a href="https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details">aplicación</a> en la sección Administración de aplicaciones de cada aplicación.</p>
</td>
<td>
Id. de Store <p>(Se recomienda)</p>
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
<td>Id. de Store <p>(Se recomienda)</p></td>
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

 

 