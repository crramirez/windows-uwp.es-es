---
author: Jwmsft
Description: "El selector de hora te ofrece una forma estandarizada de permitir a los usuarios seleccionar una hora mediante entrada táctil, de mouse o de teclado."
title: Selector de hora
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 69f682b0edddbcf88515af537c33b3d8297f91f0

---
# Selector de hora
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

El selector de hora te ofrece una forma estandarizada de permitir a los usuarios seleccionar una hora mediante entrada táctil, de mouse o de teclado. 

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx"><strong>Clase TimePicker</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx"><strong>Propiedad Time</strong></a></li>
</ul>

</div>
</div>




## ¿Es este el control adecuado?
Usa un selector de hora para permitir que el usuario seleccione un único valor de hora.

Para obtener más información sobre cómo elegir el control adecuado, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## Ejemplos

El punto de entrada muestra la hora elegida y, cuando el usuario lo selecciona, se expande una superficie del selector en sentido vertical desde el medio para que el usuario realice una selección. El selector de hora se superpone a otra interfaz de usuario; no hace que la otra interfaz desaparezca.

![Ejemplo de expansión del selector de hora](images/controls_timepicker_expand.png)

## Crear un selector de hora

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

> **Nota**&nbsp;&nbsp;Para obtener información importante acerca de los valores de fecha y hora, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo *Controles de fecha y hora*.



## Temas relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Vista de calendario](calendar-view.md)
- [Selector de fecha](date-picker.md)



<!--HONumber=Aug16_HO3-->


