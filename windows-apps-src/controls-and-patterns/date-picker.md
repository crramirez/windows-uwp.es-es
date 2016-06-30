---
author: Jwmsft
Description: "El selector de fecha te ofrece una forma estandarizada de permitir a los usuarios seleccionar un valor de fecha localizada usando entrada táctil, de mouse o de teclado."
title: Selector de fecha
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: c237d4bc013ad0a1d0d16f695f4332a6aac7efdc

---

# Selector de fecha

El selector de fecha te ofrece una forma estandarizada de permitir a los usuarios seleccionar un valor de fecha localizada usando entrada táctil, de mouse o de teclado. 

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Clase DatePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)
-   [**Propiedad Date**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)

## ¿Es este el control adecuado?
Usa un selector de fecha para permitir que el usuario seleccione una fecha conocida, como una fecha de nacimiento, donde el contexto del calendario no es importante.

Para obtener más información acerca de cómo elegir el control de fecha adecuada, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## Ejemplos

El punto de entrada muestra la fecha elegida y, cuando el usuario lo selecciona, se expande una superficie del selector en sentido vertical desde el medio para que el usuario realice una selección. El selector de fecha se superpone a otra interfaz de usuario; no la hace desaparecer.

![Ejemplo de expansión del selector de fecha](images/controls_datepicker_expand.png)

## Crear un selector de fecha

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

> **Nota**
            &nbsp;&nbsp;Para obtener información importante acerca de los valores de fecha, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo Controles de fecha y hora.



## Artículos relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Vista de calendario](calendar-view.md)
- [Selector de hora](time-picker.md)



<!--HONumber=Jun16_HO4-->


