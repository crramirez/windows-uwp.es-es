---
Description: Los controles de fecha y hora permiten ver y establecer la fecha y la hora. Este artículo ofrece directrices de diseño y ayuda a elegir el control correcto.
title: Directrices para los controles de fecha y hora
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 54da5ea68dad0bbd2c7f00ae5fc4128f14d1ce43
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362882"
---
# <a name="calendar-date-and-time-controls"></a>Calendario, controles de fecha y hora

 

Los controles de fecha y hora ofrecen maneras estándar y localizadas para que el usuario pueda ver y establecer los valores de fecha y hora en la aplicación. Este artículo ofrece directrices de diseño y ayuda a elegir el control correcto.

> **API importantes**: [Clase CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView), [CalendarDatePicker clase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker), [DatePicker clase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker), [TimePicker clase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/category/DataInput">abrir la aplicación y ver estos controles en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>¿Qué control fecha u hora debería usar?

Hay cuatro controles de fecha y hora para elegir; el control que uses dependerá de la situación. Usa esta información para seleccionar el control adecuado para tu aplicación.

&nbsp;|&nbsp;|&nbsp;                                                                                                                      
--------------------|-------|-------------------------------------------------------------------------------------------------------------------------------
Vista de calendario       |![Ejemplo de vista de calendario](images/controls_calendar_monthview_small.png)|Se usa para seleccionar una fecha determinada o un intervalo de fechas de un calendario siempre visible.                   
Selector de fecha del calendario|![Ejemplo de selector de fecha del calendario](images/calendar-date-picker-closed.png)|Se usa para seleccionar una fecha determinada de un calendario contextual. 
Selector de fecha         |![Ejemplo de selector de fecha](images/date-picker-closed.png)|Se usa para seleccionar una sola fecha conocida cuando la información contextual no es importante.
Selector de hora         |![Ejemplo de selector de hora](images/time-picker-closed.png)|Se usa para seleccionar un único valor de hora.                                        

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>Vista de calendario

**CalendarView** permite al usuario ver e interactuar con un calendario por el que se puede navegar por mes, año o década. Un usuario puede seleccionar una fecha determinada o un intervalo de fechas. No tiene una superficie de selector y el calendario es siempre visible.

La vista del calendario se compone de 3 vistas separadas: la vista de mes, la vista de año y la vista de década. De manera predeterminada, se abre en la vista de mes, pero se puede especificar cualquier vista como la inicial.

![Ejemplo de selector de fecha del calendario](images/calendar-view-3-views.png)

- Si es necesario que un usuario pueda seleccionar varias fechas, debes usar una **CalendarView**.
- Si es necesario permitir que un usuario elija solo una fecha determinada y no necesita que haya un calendario siempre visible, considera usar un control **CalendarDatePicker** o **DatePicker**.

### <a name="calendar-date-picker"></a>Selector de fecha del calendario

**CalendarDatePicker** es un control desplegable que está optimizado para seleccionar una fecha determinada de una vista de calendario en el que la información contextual es importante, por ejemplo, el día de la semana o lo que se ha completado del calendario. Puedes modificar el calendario para que proporcione contexto adicional o para que limite las fechas disponibles.

El punto de entrada muestra texto del marcador de posición si no se ha establecido una fecha; de lo contrario, muestra la fecha elegida. Cuando el usuario selecciona el punto de entrada, se expande una vista de calendario para que el usuario realice una selección de fecha. La vista de calendario se superpone a otra interfaz de usuario; no hace que la otra interfaz desaparezca.

![Ejemplo de selector de fecha del calendario](images/calendar-date-picker-2-views.png)

- Usa un selector de fecha del calendario para cosas como la elección de una cita o la fecha de salida. 

### <a name="date-picker"></a>Selector de fecha

El control **DatePicker** ofrece una forma estandarizada de elegir una fecha específica. 

El punto de entrada muestra la fecha elegida y, cuando el usuario lo selecciona, se expande una superficie del selector en sentido vertical desde el medio para que el usuario realice una selección. El selector de fecha se superpone a otra interfaz de usuario; no la hace desaparecer.

![Ejemplo de expansión del selector de fecha](images/controls_datepicker_expand.png)

- Usa un selector de fecha para permitir que el usuario seleccione una fecha conocida, como una fecha de nacimiento, donde el contexto del calendario no es importante.

### <a name="time-picker"></a>Selector de hora

El **TimePicker** se usa para seleccionar un único valor para cosas como citas o una hora de salida. Es una visualización estática que establece el usuario o se establece en el código, pero no se actualiza para mostrar la hora actual. 

El punto de entrada muestra la hora elegida y, cuando el usuario lo selecciona, se expande una superficie del selector en sentido vertical desde el medio para que el usuario realice una selección. El selector de hora se superpone a otra interfaz de usuario; no hace que la otra interfaz desaparezca.

![Ejemplo de expansión del selector de hora](images/controls_timepicker_expand.png)

- Usa un selector de hora para permitir que el usuario seleccione un único valor de hora.

## <a name="create-a-date-or-time-control"></a>Crea un control de fecha u hora

Consulte los siguientes artículos para obtener información y ejemplos específicos de cada control de fecha y hora.

- [Vista del calendario](calendar-view.md)
- [Selector de fecha del calendario](calendar-date-picker.md)
- [Selector de fecha](date-picker.md)
- [Selector de hora](time-picker.md)

### <a name="globalization"></a>Globalización

Los controles de fecha XAML admiten todos los sistemas de calendario compatibles con Windows. Estos calendarios se especifican en la clase [Windows.Globalization.CalendarIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.CalendarIdentifiers). Cada control usa el calendario correcto para el idioma predeterminado de tu aplicación o puedes establecer la propiedad **CalendarIdentifier** para usar un sistema de calendario específico.

El control de selector de hora admite cada uno de los sistemas de reloj especificados en la clase [Windows.Globalization.ClockIdentifiers](https://docs.microsoft.com/uwp/api/Windows.Globalization.ClockIdentifiers). Puedes establecer la propiedad [ClockIdentifier](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) para usar un reloj de 12 horas o un reloj de 24 horas. El tipo de la propiedad es String, pero debes usar valores que correspondan a las propiedades de cadena estática de la clase ClockIdentifiers. Estos son: TwelveHour (la cadena "12HourClock") y TwentyFourHour (la cadena "24HourClock"). El valor predeterminado es "12HourClock".


### <a name="datetime-and-calendar-values"></a>Valores de DateTime y Calendar

Los objetos de fecha usados en los controles de fecha y hora XAML tienen una representación diferente según el lenguaje de programación. 
- C# y Visual Basic usan la estructura [System.DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN) que forma parte de .NET. 
- C++/CX usa la estructura [Windows::Foundation::DateTime](https://docs.microsoft.com/windows/desktop/api/windows.foundation/ns-windows-foundation-datetime). 

Un concepto relacionado es la clase Calendar, que influye en la interpretación de fechas en contexto. Todas las aplicaciones de Windows Runtime pueden usar la clase [Windows.Globalization.Calendar](https://docs.microsoft.com/uwp/api/Windows.Globalization.Calendar). Las aplicaciones de C# y Visual Basic pueden usar también la clase [System.Globalization.Calendar](https://docs.microsoft.com/dotnet/api/system.globalization.calendar?redirectedfrom=MSDN), que tiene una funcionalidad muy similar. (Las aplicaciones de Windows Runtime pueden usar la clase base Calendar de .NET, pero no las implementaciones específicas; por ejemplo, GregorianCalendar.)

.NET admite también un tipo denominado [DateTime](https://docs.microsoft.com/dotnet/api/system.datetime?redirectedfrom=MSDN), que implícitamente se puede convertir a una estructura [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN). Por lo tanto, es posible que veas un tipo de "DateTime" que se usa en el código .NET que se usa para establecer valores que son realmente DateTimeOffset. Para obtener más información sobre la diferencia entre DateTime y DateTimeOffset, consulta las observaciones de la clase [DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN).

> **Nota**&nbsp;&nbsp;No se pueden establecer propiedades que toman objetos de fecha como una cadena de atributos XAML, porque el analizador de XAML de Windows Runtime no tiene una lógica de conversión para convertir cadenas en fechas como objetos DateTime o DateTimeOffset. En general, estableces estos valores en el código. Otra técnica posibles consiste en definir una fecha que está disponible como un objeto de datos o en el contexto de datos y, después, establezca la propiedad como un atributo XAML que hace referencia a un [ \{enlace\} extensión de marcado](../../xaml-platform/binding-markup-extension.md) expresión que puede tener acceso a la fecha como datos.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo
* [Ejemplo de conceptos básicos de XAML UI](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)


## <a name="related-topics"></a>Temas relacionados

**Para desarrolladores (XAML)**
- [Clase CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)
- [Clase CalendarDatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)
- [Clase DatePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePicker)
- [Clase TimePicker](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePicker)
