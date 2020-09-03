---
Description: El selector de hora te ofrece una forma estandarizada de permitir a los usuarios seleccionar una hora mediante entrada táctil, de mouse o de teclado.
title: Selector de hora
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 05/08/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 71f9d7e42b39b91186c9a2aa3ab1b87ed7f4d8b4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168719"
---
# <a name="time-picker"></a>Selector de hora
 

El selector de hora te ofrece una forma estandarizada de permitir a los usuarios seleccionar una hora mediante entrada táctil, de mouse o de teclado.

![Ejemplo de selector de hora](images/time-picker-closed.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](../style/rounded-corner.md). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de plataforma**: [Clase TimePicker](/uwp/api/Windows.UI.Xaml.Controls.TimePicker), [propiedad Time](/uwp/api/windows.ui.xaml.controls.timepicker.time)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?
Usa un selector de hora para permitir que el usuario seleccione un único valor de hora.

Para obtener más información sobre cómo elegir el control adecuado, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/TimePicker">abrir la aplicación y ver TimePicker en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

El punto de entrada muestra la hora elegida y, cuando el usuario lo selecciona, se expande una superficie del selector en sentido vertical desde el medio para que el usuario realice una selección. El selector de hora se superpone a otra interfaz de usuario; no hace que la otra interfaz desaparezca.

![Ejemplo de expansión del selector de hora](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>Crear un selector de hora

En este ejemplo, se muestra cómo crear un sencillo selector de hora con un encabezado.

```xaml
<TimePicker x:Name="arrivalTimePicker" Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

El selector de hora resultante tiene este aspecto:

![Ejemplo de selector de hora](images/time-picker-closed.png)

> [!NOTE]
> Para obtener información importante sobre los valores de fecha y hora, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo *Controles de fecha y hora*.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Temas relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Vista del calendario](calendar-view.md)
- [Selector de fecha](date-picker.md)