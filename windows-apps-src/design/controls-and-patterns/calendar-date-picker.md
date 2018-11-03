---
author: muhsinking
Description: The calendar date picker is a drop down control that’s optimized for picking a single date from a calendar view where contextual information like the day of the week or fullness of the calendar is important.
title: Selector de fecha del calendario
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6756d8c64f33de8d16d6aa455c3fad694a6306d5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2018
ms.locfileid: "5995996"
---
# <a name="calendar-date-picker"></a>Selector de fecha del calendario

 

El selector de fecha del calendario es un control desplegable que está optimizado para seleccionar una fecha determinada desde una vista de calendario en la que la información contextual es importante, por ejemplo, el día de la semana o lo que se haya completado del calendario. Puedes modificar el calendario para que proporcione contexto adicional o para que limite las fechas disponibles.

> **API importantes**: [Clase CalendarDatePicker](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx), [Propiedad Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx), [Evento DateChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?
Usa un **selector de fecha del calendario** para permitir al usuario seleccionar una fecha determinada desde una vista de calendario contextual. Úsalo para, por ejemplo, elegir la fecha de una cita o de una salida.

Para permitir que el usuario seleccione una fecha conocida, como una fecha de nacimiento, donde el contexto del calendario no es importante, considera la posibilidad de usar un [selector de fecha](date-picker.md).

Para obtener más información sobre cómo elegir el control adecuado, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/CalendarDatePicker">abrir la aplicación y ver CalendarDatePicker en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

El punto de entrada muestra texto del marcador de posición si no se ha establecido una fecha; de lo contrario, muestra la fecha elegida. Cuando el usuario selecciona el punto de entrada, se expande una vista de calendario para que el usuario realice una selección de fecha. La vista de calendario se superpone a otra interfaz de usuario; no hace que la otra interfaz desaparezca.

![Ejemplo de selector de fecha del calendario](images/calendar-date-picker-2-views.png)

## <a name="create-a-date-picker"></a>Crear un selector de fecha

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

El selector de fecha del calendario resultante tiene este aspecto:

![Ejemplo de selector de fecha del calendario](images/calendar-date-picker-closed.png)

El selector de fecha del calendario tiene un componente [CalendarView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx) interno para seleccionar una fecha. Existe un subconjunto de propiedades CalendarView como [IsTodayHighlighted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted.aspx) y [FirstDayOfWeek](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.firstdayofweek.aspx) en CalendarDatePicker y se envían al componente CalendarView interno para que puedas modificarlo. 

Sin embargo, no puedes cambiar el [SelectionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) del componente CalendarView interno para permitir la selección múltiple. Para permitir al usuario elegir varias fechas o si necesitas un calendario para estar siempre visible, considera la posibilidad de usar una vista de calendario en lugar de un selector de fecha del calendario. Consulta el artículo [Vista de calendario](calendar-view.md) para obtener más información sobre cómo puedes modificar la presentación de calendario.

### <a name="selecting-dates"></a>Seleccionar fechas

Usa el la propiedad [Date](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx) para obtener o establecer la fecha seleccionada. De manera predeterminada, la propiedad Date es **null**. Cuando un usuario selecciona una fecha en la vista de calendario, esta propiedad se actualiza. Un usuario puede borrar la fecha haciendo clic en la fecha seleccionada en la vista de calendario para anular la selección. 

Puedes establecer la fecha en el código de la siguiente forma.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Al configurar la fecha en el código, el valor está restringido por las propiedades [MinDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.mindate.aspx) y [MaxDate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.maxdate.aspx).
- Si **Date** es menor que **MinDate**, el valor se establece en **MinDate**.
- Si **Date** es mayor que **MaxDate**, el valor se establece en **MaxDate**.

Puedes controlar el evento [DateChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx) para obtener una notificación cuando el valor Date haya cambiado.

> [!NOTE]
Para obtener información importante sobre los valores de fecha, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo Controles de fecha y hora.

### <a name="setting-a-header-and-placeholder-text"></a>Establecer un encabezado y un texto de marcador de posición

Puedes agregar un [Encabezado](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.header.aspx) (o etiqueta) y un [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.placeholdertext.aspx) (o marca de agua) para dar al usuario una indicación de para qué se usa el selector de fecha del calendario. Para personalizar el aspecto del encabezado, puedes establecer la propiedad [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.headertemplate.aspx) en lugar de Header.

El texto de marcador de posición predeterminado es "seleccionar una fecha". Puedes quitar esto estableciendo la propiedad PlaceholderText en una cadena vacía, o bien puedes proporcionar texto personalizado como se muestra aquí.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Vista de calendario](calendar-view.md)
- [Selector de fecha](date-picker.md)
- [Selector de hora](time-picker.md)
