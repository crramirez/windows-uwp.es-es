---
title: Preparar la aplicación para el cambio de la era japonesa
description: Obtén información sobre el cambio de la era japonesa de mayo de 2019 y cómo preparar la aplicación.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 12/7/2018
ms.topic: article
keywords: windows 10, uwp, posibilidad de localización, localización, japonesa, era
ms.localizationpriority: high
ms.openlocfilehash: 0d5de4c1713ab80afcdf2e028d39340aebcc018b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617660"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>Preparar la aplicación para el cambio de la era japonesa

El calendario japonés se divide en eras y, para la mayoría de la informática de la época moderna, hemos estado dentro de la era Heisei; sin embargo, se iniciará una nueva era el 1 de mayo de 2019. Dado que es la primera vez en décadas que cambiará la era, el software compatible con el calendario japonés deberá probarse para asegurar que funcionará correctamente cuando se inicie la nueva era.

En las secciones siguientes, aprenderás lo que puedes hacer para preparar y probar la aplicación para la nueva era que está al llegar.

> [!NOTE]
> Te recomendamos que uses un equipo de prueba para esto, porque los cambios que realices afectarán al comportamiento de todo el equipo.

## <a name="add-a-registry-key-for-the-new-era"></a>Agregar una clave del Registro para la nueva era

Es importante comprobar si hay problemas de compatibilidad antes de que cambie la era, y puedes hacerlo ahora usando un nombre de marcador de posición. Para ello, agrega una clave del Registro para la nueva era, mediante **Editor del Registro**:

1. Ve a **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**.
2. Selecciona **Editar > Nuevo > Valor de cadena** y asígnale el nombre **2019 05 01**.
3. Haz clic con el botón derecho en la clave y selecciona **Modificar**.
4. ¿En el **datos del valor** , introduzca **?？\_？\_??????\_?** (puedes copiar y pegar desde aquí para que sea más fácil).

Consulta [Tratamiento de las eras del calendario japonés](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar) para obtener más información sobre el formato de estas claves del Registro.

Una vez que se anuncie el nombre de la nueva era, una actualización con una nueva clave del registro para las versiones compatibles de Windows contendrá el nombre, con lo que podrás validar que la aplicación trata el cambio correctamente. Esta actualización se propagará a las versiones anteriores de Windows 10 compatibles, así como a Windows 8 y 7.

Puedes eliminar la clave del Registro de marcador de posición cuando hayas terminado de probar la aplicación. Esto garantiza que no interfiera con la nueva clave del Registro que se agregará al actualizar Windows.

## <a name="change-your-devices-calendar-format"></a>Cambiar el formato de calendario del dispositivo

Una vez que hayas agregado la clave del registro de la nueva era, debes configurar tu dispositivo para usar el calendario japonés. No necesitas un dispositivo con idioma japonés para hacerlo. Para realizar pruebas exhaustivas, puede convenirte instalar también el paquete de idioma japonés, pero no es necesario para las pruebas básicas.

Para configurar el dispositivo para usar el calendario japonés:

1. Abre **intl.cpl** (búscalo desde la barra de búsqueda de Windows).
2. Desde el menú desplegable **Formato**, selecciona **Japonés (Japón)**.
3. Selecciona **Configuración adicional**.
4. Selecciona la pestaña **Fecha**.
5. En el menú desplegable **Tipo de calendario**, selecciona **和暦** (*wareki*, el calendario japonés). Debería ser la segunda opción.
6. Haga clic en **Aceptar**.
7. Haz clic en **Aceptar**, en la ventana **Región** .

Ahora, el dispositivo debería quedar configurado para usar el calendario japonés y reflejará todas las eras que haya en el registro. A continuación se incluye un ejemplo de lo que pudiera aparecer en la esquina inferior derecha de la pantalla:

![Fecha y hora en formato de calendario japonés](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>Ajustar el reloj del dispositivo

Para comprobar que la aplicación funciona con la nueva era, debes adelantar el reloj del equipo al 1 de mayo de 2019 o fecha posterior. Las siguientes instrucciones son para Windows 10, pero deberían funcionar de forma similar con Windows 8 y 7:

1. Haz clic con el botón derecho en el área de fecha y hora de la esquina inferior derecha de la pantalla.
2. Selecciona **Ajustar fecha/hora**.
3. En la aplicación Configuración, en **Cambiar fecha y hora**, selecciona **Cambiar**.
4. Cambia la fecha al 1 de mayo de 2019 o a otra fecha posterior.

> [!NOTE]
> Es podrán que no pueda cambiar la fecha según la configuración de la organización; Si este es el caso, hable con su administrador. Como alternativa, puede editar la clave del registro de marcador de posición para que tenga una fecha que ya ha pasado.

## <a name="test-your-application"></a>Probar la aplicación

Ahora, prueba cómo la aplicación maneja la nueva era. Comprueba los lugares donde se muestra la fecha, como selectores de fecha y marcas de tiempo. ¿Asegúrese de que la era es correcta antes 1 de mayo de 2019 (Heisei, 平成) y después (?？).

### <a name="gannen-"></a>*Gannen* (元年)

Suele ser el formato para el calendario japonés  **&lt;nombre de la Era&gt; &lt;año de era&gt;**. Por ejemplo, el año 2018 es **Heisei 30** (平成30年).  Sin embargo, el primer año de cada era es especial; en lugar de ser **&lt;Nombre de la era&gt; 1**, es **&lt;Nombre de la era&gt; 元年** (*gannen*). Por lo tanto, el primer año de la era Heisei sería 平成元年 (*Heisei gannen*). ¿Asegúrese de que la aplicación maneja correctamente el primer año de la nueva era y se genera correctamente?? 元年.

## <a name="related-apis"></a>API relacionadas

Hay varias API de WinRT, .NET y Win32 que se van a actualizar para manejar el cambio de era, por lo que, si las usas, no deberías tener demasiados problemas. Sin embargo, incluso si dependes totalmente de estas API, sigue siendo una buena idea probar la aplicación y asegurarse de obtener el comportamiento deseado, especialmente si haces algo especial con ellas, como analizar.

Puedes seguir con las actualizaciones para el sistema operativo y el SDK en [Actualizaciones para el cambio de era de mayo de 2019 de Japón](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change).

Se verán afectadas las API siguientes:

### <a name="winrt"></a>WinRT

* [Windows.Globalization Namespace](https://docs.microsoft.com/uwp/api/windows.globalization)
    * [Clase de calendario](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
        * [Método AddDays](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adddays)
        * [Método AddEras](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.adderas)
        * [Método AddHours](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addhours)
        * [Método AddMinutes](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addminutes)
        * [Método AddMonths](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addmonths)
        * [Método AddNanoseconds](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addnanoseconds)
        * [Método AddPeriods](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addperiods)
        * [Método AddSeconds](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addseconds)
        * [Método AddWeeks](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addweeks)
        * [Método AddYears](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.addyears)
        * [Propiedad era](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
        * [Método EraAsString](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
        * [Propiedad FirstYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
        * [Propiedad LastEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
        * [Propiedad LastYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
        * [Propiedad NumberOfYearsInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)     
* [Windows.Globalization.DateTimeFormatting Namespace](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
    * [Clase DateTimeFormatter](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
        * [Método de formato](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [Sistema Namespace](https://docs.microsoft.com/dotnet/api/system)
    * [Estructura de fecha y hora](https://docs.microsoft.com/dotnet/api/system.datetime)
    * [DateTimeOffset Struct](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [System.Globalization Namespace](https://docs.microsoft.com/dotnet/api/system.globalization)
    * [Clase de calendario](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
    * [Clase DateTimeFormatInfo](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
    * [JapaneseCalendar (clase)](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
    * [Clase JapaneseLunisolarCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [datetimeapi.h header](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
    * [Función GetDateFormatA](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
    * [Función GetDateFormatEx](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
    * [Función GetDateFormatW](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [encabezado winnls.h](https://docs.microsoft.com/windows/desktop/api/winnls/)
    * [Función EnumDateFormatsA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
    * [Función EnumDateFormatsExA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
    * [Función EnumDateFormatsExEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
    * [Función EnumDateFormatsExW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
    * [Función EnumDateFormatsW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
    * [Función GetCalendarInfoA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
    * [Función GetCalendarInfoEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
    * [GetCalendarInfoW function](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>Consulte también

* [Era el control para el calendario japonés](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [Momento de Y2K del calendario japonés](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [Mediante el registro para probar la nueva Era de japonés en Windows](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [Vs Gannen Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [Las actualizaciones de 2019 Japón Era pueden cambiar](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Control de una nueva era en el calendario japonés en .NET](https://blogs.msdn.microsoft.com/dotnet/2018/11/14/handling-a-new-era-in-the-japanese-calendar-in-net/)