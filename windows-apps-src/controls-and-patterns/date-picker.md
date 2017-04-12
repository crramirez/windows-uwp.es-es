---
author: Jwmsft
Description: "El selector de fecha te ofrece una forma estandarizada de permitir a los usuarios seleccionar un valor de fecha localizada usando entrada táctil, de mouse o de teclado."
title: Selector de fecha
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: True
label: Date picker
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 114e91276ee4f080ead825b655fc05fcf460bacb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="date-picker"></a>Selector de fecha

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

El selector de fecha te ofrece una forma estandarizada de permitir a los usuarios seleccionar un valor de fecha localizada usando entrada táctil, de mouse o de teclado. 

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Clase DatePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)</li>
<li>[**Propiedad Date**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx) </li>

</ul>
</div>


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?
Usa un selector de fecha para permitir que el usuario seleccione una fecha conocida, como una fecha de nacimiento, donde el contexto del calendario no es importante.

Para obtener más información acerca de cómo elegir el control de fecha adecuada, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## <a name="examples"></a>Ejemplos

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



## <a name="related-articles"></a>Artículos relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Vista de calendario](calendar-view.md)
- [Selector de hora](time-picker.md)
