---
author: Jwmsft
Description: "El selector de hora te ofrece una forma estandarizada de permitir a los usuarios seleccionar una hora mediante entrada táctil, de mouse o de teclado."
title: Selector de hora
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 83ef423ce4e6fd57726879dd83a704c47cc43744
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="time-picker"></a>Selector de hora
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

El selector de hora te ofrece una forma estandarizada de permitir a los usuarios seleccionar una hora mediante entrada táctil, de mouse o de teclado. 

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Clase TimePicker**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)</li>
<li>[**Propiedad Time**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)</li>
</ul>
</div>

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?
Usa un selector de hora para permitir que el usuario seleccione un único valor de hora.

Para obtener más información sobre cómo elegir el control adecuado, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## <a name="examples"></a>Ejemplos

El punto de entrada muestra la hora elegida y, cuando el usuario lo selecciona, se expande una superficie del selector en sentido vertical desde el medio para que el usuario realice una selección. El selector de hora se superpone a otra interfaz de usuario; no hace que la otra interfaz desaparezca.

![Ejemplo de expansión del selector de hora](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>Crear un selector de hora

En este ejemplo, se muestra cómo crear un sencillo selector de hora con un encabezado.

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

El selector de hora resultante tiene este aspecto:

![Ejemplo de selector de hora](images/time-picker-closed.png)

> [!NOTE]
> Para obtener información importante sobre los valores de fecha y hora, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo *Controles de fecha y hora*.



## <a name="related-topics"></a>Temas relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Vista del calendario](calendar-view.md)
- [Selector de fecha](date-picker.md)
