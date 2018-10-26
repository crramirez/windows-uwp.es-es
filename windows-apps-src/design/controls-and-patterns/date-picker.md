---
author: muhsinking
Description: The date picker gives you a standardized way to let users pick a localized date value using touch, mouse, or keyboard input.
title: Selector de fecha
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1f258ff63d2cf9badfc1066f46c97ecc14b90f5f
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5596160"
---
# <a name="date-picker"></a>Selector de fecha

 

El selector de fecha te ofrece una forma estandarizada de permitir a los usuarios seleccionar un valor de fecha localizada usando entrada táctil, de mouse o de teclado. 

> **API importantes**: [Clase DatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx), [Propiedad Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?
Usa un selector de fecha para permitir que el usuario seleccione una fecha conocida, como una fecha de nacimiento, donde el contexto del calendario no es importante.

Para obtener más información acerca de cómo elegir el control de fecha adecuada, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/DatePicker">abrir la aplicación y ver DatePicker en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

El punto de entrada muestra la fecha elegida y, cuando el usuario lo selecciona, se expande una superficie del selector en sentido vertical desde el medio para que el usuario realice una selección. El selector de fecha se superpone a otra interfaz de usuario; no la hace desaparecer.

![Ejemplo de expansión del selector de fecha](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>Crear un selector de fecha

En este ejemplo, se muestra cómo crear un sencillo selector de fecha con un encabezado.

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

El selector de fecha resultante tiene este aspecto:

![Ejemplo de selector de fecha](images/date-picker-closed.png)

> **Nota**&nbsp;&nbsp;Para obtener información importante sobre los valores de fecha, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo Controles de fecha y hora.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Vista de calendario](calendar-view.md)
- [Selector de hora](time-picker.md)
