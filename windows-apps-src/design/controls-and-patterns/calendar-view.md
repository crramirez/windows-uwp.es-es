---
description: Una vista de calendario permite al usuario ver e interactuar con un calendario por el que se puede navegar por mes, año o década.
title: Vista de calendario
ms.assetid: d8ec5ba8-7a9d-405d-a1a5-5a1b502b9e64
label: Calendar view
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f26f2dce4847587869b35c7f04f0da2e45e3c0e2
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030958"
---
# <a name="calendar-view"></a>Vista de calendario

Una vista de calendario permite al usuario ver e interactuar con un calendario por el que se puede navegar por mes, año o década. Un usuario puede seleccionar una fecha determinada o un intervalo de fechas. No tiene una superficie de selector y el calendario es siempre visible.

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

> **API de plataforma:**  [Clase CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [Evento SelectedDatesChanged](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?
Usa una vista de calendario para permitir al usuario seleccionar una fecha determinada o un intervalo de fechas a partir de un calendario siempre visible.

Si necesitas permitir al usuario seleccionar varias fechas al mismo tiempo, debes usar una vista de calendario. Si necesitas que un usuario pueda elegir solo una fecha determinada y no necesitas que haya un calendario siempre visible, considera la posibilidad de usar un control [CalendarDatePicker](calendar-date-picker.md) o [DatePicker](date-picker.md).

Para obtener más información sobre cómo elegir el control adecuado, consulta el artículo [Controles de fecha y hora](date-and-time.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/CalendarView">abrir la aplicación y ver CalendarView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

La vista del calendario se compone de 3 vistas separadas: la vista de mes, la vista de año y la vista de década. De manera predeterminada, se inicia con la vista de mes abierta. Puedes especificar una vista de inicio estableciendo la propiedad [DisplayMode](/uwp/api/windows.ui.xaml.controls.calendarview.displaymode).

![Las 3 vistas de una vista de calendario](images/calendar-view-3-views.png)

Los usuarios hacen clic en el encabezado de la vista de mes para abrir la vista de año, y hacen clic en el encabezado de la vista de año para abrir la vista de década. Los usuarios seleccionan un año en la vista de década para volver a la vista de año y seleccionan un mes en la vista de año para volver a la vista de mes. Las dos flechas al lado del encabezado navegan hacia delante o hacia atrás por mes, año o década.

## <a name="create-a-calendar-view"></a>Crear una vista de calendario

En este ejemplo se muestra cómo crear una vista de calendario sencilla.

```xaml
<CalendarView/>
```

La vista de calendario resultante tiene este aspecto:

![Ejemplo de vista de calendario](images/controls_calendar_monthview.png)

### <a name="selecting-dates"></a>Seleccionar fechas

De manera predeterminada, la propiedad [SelectionMode](/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) se establece en **Single**. Esto permite al usuario seleccionar una fecha determinada en el calendario. Establece SelectionMode en **None** para deshabilitar la selección de fecha.

Establece SelectionMode en **Multiple** para permitir al usuario seleccionar varias fechas. Puedes seleccionar varias fechas mediante programación agregando objetos [DateTime](/dotnet/api/system.datetime)/[DateTimeOffset](/dotnet/api/system.datetimeoffset) a la colección [SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates), tal y como se muestra aquí:

```csharp
calendarView1.SelectedDates.Add(DateTimeOffset.Now);
calendarView1.SelectedDates.Add(new DateTime(1977, 1, 5));
```

Un usuario puede anular la selección de una determinada fecha haciendo clic o pulsándola en la cuadrícula del calendario.

Puedes controlar el evento [SelectedDatesChanged](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddateschanged) para que se notifique cuando la colección [SelectedDates](/uwp/api/windows.ui.xaml.controls.calendarview.selecteddates) haya cambiado.

> [!NOTE]
> Para obtener información importante sobre los valores de fecha, consulta [Valores de DateTime y Calendar](date-and-time.md#datetime-and-calendar-values) en el artículo Controles de fecha y hora.

### <a name="customizing-the-calendar-views-appearance"></a>Personalizar la apariencia de la vista de calendario

La vista del calendario se compone de elementos XAML definidos en ControlTemplate y de elementos visuales representados directamente por el control.
- Los elementos XAML que se definen en la plantilla de control incluyen el borde dentro del que se encuentran el control, el encabezado, los botones Anterior y Siguiente y los elementos DayOfWeek. Puedes aplicar un estilo y crear una nueva plantilla para estos elementos como cualquier control de XAML.
- La cuadrícula del calendario está compuesta por objetos [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem). No puedes aplicar un estilo o crear una nueva plantilla para estos elementos, pero se proporcionan diversas propiedades para que puedas personalizar su apariencia.

Este diagrama muestra los elementos que componen la vista de mes del calendario. Para obtener más información, consulta los comentarios sobre la clase [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem).

![Los elementos de una vista de mes del calendario](images/calendar-view-month-elements.png)

Esta tabla enumera las propiedades que puedes cambiar para modificar la apariencia de los elementos del calendario.

Elemento | Propiedades
--------|-----------
DayOfWeek | [DayOfWeekFormat](/uwp/api/windows.ui.xaml.controls.calendarview.dayofweekformat)
CalendarItem | [CalendarItemBackground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritembackground), [CalendarItemBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderbrush), [CalendarItemBorderThickness](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemborderthickness), [CalendarItemForeground](/uwp/api/windows.ui.xaml.controls.calendarview.calendaritemforeground)
DayItem | [DayItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontfamily), [DayItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontsize), [DayItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontstyle), [DayItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.dayitemfontweight), [HorizontalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontaldayitemalignment), [VerticalDayItemAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticaldayitemalignment), [CalendarViewDayItemStyle](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemstyle)
MonthYearItem (en las vistas año y década, equivalente a DayItem) | [MonthYearItemFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontfamily), [MonthYearItemFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontsize), [MonthYearItemFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontstyle), [MonthYearItemFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.monthyearitemfontweight)
FirstOfMonthLabel | [FirstOfMonthLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontfamily), [FirstOfMonthLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontsize), [FirstOfMonthLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontstyle), [FirstOfMonthLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofmonthlabelfontweight), [HorizontalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.horizontalfirstofmonthlabelalignment), [VerticalFirstOfMonthLabelAlignment](/uwp/api/windows.ui.xaml.controls.calendarview.verticalfirstofmonthlabelalignment), [IsGroupLabelVisible](/uwp/api/windows.ui.xaml.controls.calendarview.isgrouplabelvisible)
FirstofYearDecadeLabel (en las vistas año y década, equivalente a FirstOfMonthLabel) | [FirstOfYearDecadeLabelFontFamily](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontfamily), [FirstOfYearDecadeLabelFontSize](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontsize), [FirstOfYearDecadeLabelFontStyle](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontstyle), [FirstOfYearDecadeLabelFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.firstofyeardecadelabelfontweight)
Bordes de Estado visual | [FocusBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.focusborderbrush), [HoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.hoverborderbrush), [PressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.pressedborderbrush), [SelectedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedborderbrush), [SelectedForeground](/uwp/api/windows.ui.xaml.controls.calendarview.selectedforeground), [SelectedHoverBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedhoverborderbrush), [SelectedPressedBorderBrush](/uwp/api/windows.ui.xaml.controls.calendarview.selectedpressedborderbrush)
OutofScope | [IsOutOfScopeEnabled](/uwp/api/windows.ui.xaml.controls.calendarview.isoutofscopeenabled), [OutOfScopeBackground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopebackground), [OutOfScopeForeground](/uwp/api/windows.ui.xaml.controls.calendarview.outofscopeforeground)
Hoy | [IsTodayHighlighted](/uwp/api/windows.ui.xaml.controls.calendarview.istodayhighlighted), [TodayFontWeight](/uwp/api/windows.ui.xaml.controls.calendarview.todayfontweight), [TodayForeground](/uwp/api/windows.ui.xaml.controls.calendarview.todayforeground)

 De manera predeterminada, la vista de mes muestra 6 semanas a la vez. Puedes cambiar el número de semanas mostradas estableciendo la propiedad [NumberOfWeeksInView](/uwp/api/windows.ui.xaml.controls.calendarview.numberofweeksinview). El número mínimo de semanas para mostrar es 2; el máximo es 8.

De manera predeterminada, las vistas de año y década se muestran en una cuadrícula de 4 x 4. Para cambiar el número de filas o columnas, llama a [SetYearDecadeDisplayDimensions](/uwp/api/windows.ui.xaml.controls.calendarview.setyeardecadedisplaydimensions) con el número de filas y columnas deseado. Esto cambiará la cuadrícula para las vistas de año y década.

Aquí, las vistas de año y década están establecidas para mostrarse en una cuadrícula de 3x4.

```csharp
calendarView1.SetYearDecadeDisplayDimensions(3, 4);
```

De manera predeterminada, la fecha mínima que se muestra en la vista del calendario es 100 años antes de la fecha actual y la fecha máxima que se muestra es 100 años más allá de la fecha actual. Puedes cambiar las fechas mínimas y máximas que muestra el calendario estableciendo las propiedades [MinDate](/uwp/api/windows.ui.xaml.controls.calendarview.mindate) y [MaxDate](/uwp/api/windows.ui.xaml.controls.calendarview.maxdate).

```csharp
calendarView1.MinDate = new DateTime(2000, 1, 1);
calendarView1.MaxDate = new DateTime(2099, 12, 31);
```

### <a name="updating-calendar-day-items"></a>Actualizar elementos de calendario por día

Cada día del calendario se representa mediante un objeto [CalendarViewDayItem](/uwp/api/Windows.UI.Xaml.Controls.CalendarViewDayItem). Para acceder a un determinado elemento de día y usar sus propiedades y métodos, controla el evento [CalendarViewDayItemChanging](/uwp/api/windows.ui.xaml.controls.calendarview.calendarviewdayitemchanging) y usa la propiedad de elemento de los argumentos del evento para acceder a CalendarViewDayItem.

Puedes hacer que un día no se pueda seleccionar en la vista de calendario estableciendo su propiedad [CalendarViewDayItem.IsBlackout](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.isblackout) en **true**.

Puedes mostrar información contextual sobre la densidad de eventos en un día llamando al método [CalendarViewDayItem.SetDensityColors](/uwp/api/windows.ui.xaml.controls.calendarviewdayitem.setdensitycolors). Puedes mostrar de 0 a 10 barras de la densidad para cada día y establecer el color de cada barra.

Estos son algunos elementos de día en un calendario. Los días 1 y 2 están suprimidos. Los días 2, 3 y 4 tienen varias barras de densidad establecidas.

![Días del calendario con barras de densidad](images/calendar-view-density-bars.png)

### <a name="phased-rendering"></a>Representación por fases

Una vista de calendario puede contener un gran número de objetos CalendarViewDayItem. Para mantener la interfaz de usuario con capacidad de respuesta y habilitar la navegación fluida mediante el calendario, la vista de calendario admite la representación por fases. Esto permite dividir el procesamiento de un elemento de día en fases. Si un día se mueve fuera de la vista antes de que se completen todas las fases, no se dedica más tiempo a intentar procesar y representar ese elemento.

Este ejemplo muestra fases de representación de una vista de calendario para la programación de citas.
- En la fase 0, se procesa el elemento de día predeterminado.
- En la fase 1, no se pueden reservar las fechas no disponibles. Esto incluye fechas pasadas, domingos y fechas que ya están reservadas en su totalidad.
- En la fase 2, comprueba cada cita reservada para el día. Muestras una barra de densidad de color verde para cada cita confirmada y una barra de densidad azul para cada cita provisional.

La clase `Bookings` de este ejemplo procede de una aplicación de reserva de citas ficticia y no se muestra.

```xaml
<CalendarView CalendarViewDayItemChanging="CalendarView_CalendarViewDayItemChanging"/>
```

```csharp
private void CalendarView_CalendarViewDayItemChanging(CalendarView sender,
                                   CalendarViewDayItemChangingEventArgs args)
{
    // Render basic day items.
    if (args.Phase == 0)
    {
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set blackout dates.
    else if (args.Phase == 1)
    {
        // Blackout dates in the past, Sundays, and dates that are fully booked.
        if (args.Item.Date < DateTimeOffset.Now ||
            args.Item.Date.DayOfWeek == DayOfWeek.Sunday ||
            Bookings.HasOpenings(args.Item.Date) == false)
        {
            args.Item.IsBlackout = true;
        }
        // Register callback for next phase.
        args.RegisterUpdateCallback(CalendarView_CalendarViewDayItemChanging);
    }
    // Set density bars.
    else if (args.Phase == 2)
    {
        // Avoid unnecessary processing.
        // You don't need to set bars on past dates or Sundays.
        if (args.Item.Date > DateTimeOffset.Now &&
            args.Item.Date.DayOfWeek != DayOfWeek.Sunday)
        {
            // Get bookings for the date being rendered.
            var currentBookings = Bookings.GetBookings(args.Item.Date);

            List<Color> densityColors = new List<Color>();
            // Set a density bar color for each of the days bookings.
            // It's assumed that there can't be more than 10 bookings in a day. Otherwise,
            // further processing is needed to fit within the max of 10 density bars.
            foreach (booking in currentBookings)
            {
                if (booking.IsConfirmed == true)
                {
                    densityColors.Add(Colors.Green);
                }
                else
                {
                    densityColors.Add(Colors.Blue);
                }
            }
            args.Item.SetDensityColors(densityColors);
        }
    }
}
```

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de fecha y hora](date-and-time.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Selector de fecha](date-picker.md)
- [Selector de hora](time-picker.md)
