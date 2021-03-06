---
description: El selector de fecha del calendario es un control desplegable que está optimizado para seleccionar una fecha determinada desde una vista de calendario en la que la información contextual es importante, por ejemplo, el día de la semana o el tiempo de calendario transcurrido.
title: Selector de fecha del calendario
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c7061de6098c77214136f3441b43abbf35a10686
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829621"
---
# <a name="calendar-date-picker"></a>Selector de fecha del calendario

El selector de fecha del calendario es un control desplegable que está optimizado para seleccionar una fecha determinada desde una vista de calendario en la que la información contextual es importante, por ejemplo, el día de la semana o el tiempo de calendario transcurrido. Puedes modificar el calendario para que proporcione contexto adicional o para que limite las fechas disponibles.

**Obtención de la biblioteca de la interfaz de usuario de Windows**

:::row:::
   :::column:::
      ![Logotipo de WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](../style/rounded-corner.md). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de plataforma**: [clase CalendarDatePicker](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), [propiedad Date](/uwp/api/windows.ui.xaml.controls.calendardatepicker.date), [evento DateChanged](/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un **selector de fecha del calendario** para permitir al usuario seleccionar una fecha determinada desde una vista de calendario contextual. Úsalo para, por ejemplo, elegir la fecha de una cita o de una salida.

Para permitir que el usuario seleccione una fecha conocida, como una fecha de nacimiento, donde el contexto del calendario no es importante, considera la posibilidad de usar un [selector de fecha](date-picker.md).

Para obtener más información sobre cómo elegir el control adecuado, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/CalendarDatePicker">abrir la aplicación y ver CalendarDatePicker en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

El punto de entrada muestra texto del marcador de posición si no se ha establecido una fecha; de lo contrario, muestra la fecha elegida. Cuando el usuario selecciona el punto de entrada, se expande una vista de calendario para que el usuario realice una selección de fecha. La vista de calendario se superpone a otra interfaz de usuario; no hace que la otra interfaz desaparezca.

![Captura de pantalla de un selector de fecha del calendario que muestra un cuadro de texto seleccionar una fecha vacío y, luego, uno rellenado con un calendario debajo de él.](images/calendar-date-picker-2-views.png)

## <a name="create-a-date-picker"></a>Crear un selector de fecha

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

El selector de fecha del calendario resultante tiene este aspecto:

![Captura de pantalla de un selector de fecha del calendario rellenado con una etiqueta que indica Arrival date.](images/calendar-date-picker-closed.png)

El selector de fecha del calendario tiene un componente [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) interno para seleccionar una fecha. Existe en CalendarDatePicker un subconjunto de propiedades CalendarView, como [IsTodayHighlighted](/uwp/api/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted) y [FirstDayOfWeek](/uwp/api/windows.ui.xaml.controls.calendardatepicker.firstdayofweek), que se envían al componente CalendarView interno para que puedas modificarlo. 

Aun así, no puedes cambiar el [SelectionMode](/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) del componente CalendarView interno para permitir la selección múltiple. Para permitir al usuario elegir varias fechas o si necesitas un calendario para estar siempre visible, considera la posibilidad de usar una vista de calendario en lugar de un selector de fecha del calendario. Consulta el artículo [Vista de calendario](calendar-view.md) para obtener más información sobre cómo puedes modificar la presentación de calendario.

### <a name="selecting-dates"></a>Seleccionar fechas

Usa la propiedad [Date](/uwp/api/windows.ui.xaml.controls.calendardatepicker.date) para obtener o establecer la fecha seleccionada. De manera predeterminada, la propiedad Date es **null**. Cuando un usuario selecciona una fecha en la vista de calendario, esta propiedad se actualiza. Un usuario puede borrar la fecha haciendo clic en la fecha seleccionada en la vista de calendario para anular la selección. 

Puedes establecer la fecha en el código de la siguiente forma.

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

Al establecer la propiedad Date en el código, el valor está restringido por las propiedades [MinDate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.mindate) y [MaxDate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.maxdate).
- Si **Date** es menor que **MinDate**, el valor se establece en **MinDate**.
- Si **Date** es mayor que **MaxDate**, el valor se establece en **MaxDate**.

Puedes controlar el evento [DateChanged](/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged) para obtener una notificación cuando el valor Date haya cambiado.

> [!NOTE]
> Para obtener información importante sobre los valores de fecha, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo Controles de fecha y hora.

### <a name="setting-a-header-and-placeholder-text"></a>Establecer un encabezado y un texto de marcador de posición

Puedes agregar un elemento [Header](/uwp/api/windows.ui.xaml.controls.calendardatepicker.header) (o etiqueta) y un [PlaceholderText](/uwp/api/windows.ui.xaml.controls.calendardatepicker.placeholdertext) (o marca de agua) al selector de fecha del calendario a fin de indicarle al usuario para qué se usa. Para personalizar el aspecto del encabezado, puedes establecer la propiedad [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.headertemplate) en lugar de Header.

El texto de marcador de posición predeterminado es "seleccionar una fecha". Puedes quitar esto estableciendo la propiedad PlaceholderText en una cadena vacía, o bien puedes proporcionar texto personalizado como se muestra aquí.

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Vista del calendario](calendar-view.md)
- [Selector de fecha](date-picker.md)
- [Selector de hora](time-picker.md)
