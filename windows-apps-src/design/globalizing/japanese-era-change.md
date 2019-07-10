---
title: Preparar la aplicación para el cambio de la era japonesa
description: Obtén información sobre el cambio de la era japonesa de mayo de 2019 y cómo preparar la aplicación.
ms.assetid: 5A945F9A-8632-4038-ADD6-C0568091EF27
ms.date: 4/26/2019
ms.topic: article
keywords: windows 10, uwp, posibilidad de localización, localización, japonesa, era
ms.localizationpriority: high
ms.openlocfilehash: 54d66d0426e5f0c41d48b93ba96781786d6fab92
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66363777"
---
# <a name="prepare-your-application-for-the-japanese-era-change"></a>Preparar la aplicación para el cambio de la era japonesa

> [!NOTE]
> El 1 de abril de 2019 se anunció el nombre de la nueva era: Reiwa (令和). El 25 de abril, Microsoft publicó paquetes para los distintos sistemas operativos de Windows que contenían la clave del Registro actualizado con el nombre de la nueva era. Actualice el dispositivo y compruebe el Registro para ver si tiene la nueva clave y luego pruebe la aplicación. Consulte [este artículo de soporte técnico](https://support.microsoft.com/help/4469068/summary-of-new-japanese-era-updates-kb4469068) para asegurarse de que el sistema operativo debe haber recibido la clave del registro actualizado.

El calendario japonés se divide en eras y, durante la mayor parte de la época moderna de la informática, hemos estado dentro de la era Heisei; sin embargo, se iniciará una nueva era el 1 de mayo de 2019. Dado que es la primera vez en décadas que hay un cambio de era, el software compatible con el calendario japonés deberá probarse para asegurar que funcionará correctamente cuando se inicie la nueva era.

En las secciones siguientes, aprenderás lo que puedes hacer para preparar y probar la aplicación para la nueva era que está al llegar.

> [!NOTE]
> Te recomendamos que uses una máquina de pruebas para esto, porque los cambios que realices afectarán al comportamiento de toda la máquina.

## <a name="add-a-registry-key-for-the-new-era"></a>Agregar una clave del Registro para la nueva era

> [!NOTE]
> Las instrucciones siguientes están diseñadas para los dispositivos que aún no se han actualizado con la nueva clave del Registro. En primer lugar, comprueba si el dispositivo contiene la nueva clave del Registro y, si no, prueba con las siguientes instrucciones.

Es importante comprobar si hay problemas de compatibilidad antes de que cambie la era, y ahora puedes hacerlo con el nombre de la nueva era. Para ello, agrega una clave del Registro para la nueva era con **Editor del Registro**:

1. Navega a **Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Nls\Calendars\Japanese\Eras**.
2. Selecciona **Editar > Nuevo > Valor de cadena** y asígnale el nombre **01 05 2019**.
3. Haz clic con el botón derecho en la clave y selecciona **Modificar**.
4. En el campo **Datos del valor**, escribe **令和_令_Reiwa_R** (puedes copiar y pegar desde aquí para que sea más fácil).

Consulta [Tratamiento de las eras del calendario japonés](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar) para obtener más información sobre el formato de estas claves del Registro.

El 1 de abril de 2019 se anunció el nombre de la nueva era. El 25 de abril se publicó una actualización con una nueva clave del Registro para las versiones compatibles de Windows que contiene el nombre, lo que te permite validar que la aplicación la controla correctamente. Esta actualización se propagará a las versiones anteriores de Windows 10 compatibles, así como a Windows 8 y 7.

Puedes eliminar la clave del Registro de marcador de posición cuando hayas terminado de probar la aplicación. Esto garantiza que no interfiera con la nueva clave del Registro que se agregará al actualizar Windows.

## <a name="change-your-devices-calendar-format"></a>Cambiar el formato de calendario del dispositivo

Una vez que hayas agregado la clave del Registro de la nueva era, hay que configurar el dispositivo para usar el calendario japonés. No necesitas un dispositivo con idioma japonés para hacerlo. Para realizar pruebas exhaustivas, puedes instalar también el paquete de idioma japonés, pero no es necesario para las pruebas básicas.

Para configurar el dispositivo para usar el calendario japonés:

1. Abre **intl.cpl** (búscalo desde la barra de búsqueda de Windows).
2. Desde el menú desplegable **Formato**, selecciona **Japonés (Japón)** .
3. Selecciona **Configuración adicional**.
4. Selecciona la pestaña **Fecha**.
5. En el menú desplegable **Tipo de calendario**, selecciona **和暦** (*wareki*, el calendario japonés). Debería ser la segunda opción.
6. Haga clic en **Aceptar**.
7. Haz clic en **Aceptar**, en la ventana **Región**.

Ahora, el dispositivo debería quedar configurado para usar el calendario japonés y reflejará todas las eras que haya en el Registro. A continuación se incluye un ejemplo de lo que pudiera aparecer en la esquina inferior derecha de la pantalla:

![Fecha y hora en formato de calendario japonés](images/japanese-calendar-format.png)

## <a name="adjust-your-devices-clock"></a>Ajustar el reloj del dispositivo

Para comprobar que la aplicación funciona con la nueva era, debes adelantar el reloj del equipo al 1 de mayo de 2019 o fecha posterior. Las siguientes instrucciones corresponden a Windows 10, pero deberían funcionar de forma similar con Windows 8 y 7:

1. Haz clic con el botón derecho en el área de fecha y hora de la esquina inferior derecha de la pantalla.
2. Selecciona **Ajustar fecha/hora**.
3. En la aplicación Configuración, en **Cambiar fecha y hora**, selecciona **Cambiar**.
4. Cambia la fecha al 1 de mayo de 2019 o a otra fecha posterior.

> [!NOTE]
> Es posible que, según la configuración de la organización, no puedas cambiar la fecha; si este es el caso, habla con tu administrador. Como alternativa, puedes editar la clave del Registro de marcador de posición para que tenga una fecha que ya ha pasado.

## <a name="test-your-application"></a>Probar la aplicación

Ahora, prueba cómo la aplicación controla la nueva era. Comprueba los lugares donde se muestra la fecha, como selectores de fecha y marcas de tiempo. Asegúrate de que la era sea correcta antes del 1 de mayo de 2019 (Heisei, 平成) y después (Reiwa, 令和).

### <a name="gannen-"></a>*Gannen* (元年)

El formato para el calendario japonés suele ser **&lt;Nombre de la era&gt;&lt;Año de la era&gt;** . Por ejemplo, el año 2018 es **Heisei 30** (平成30年).  Sin embargo, el primer año de cada era es especial; en lugar de ser **&lt;Nombre de la era&gt; 1**, es **&lt;Nombre de la era&gt; 元年** (*gannen*). Por lo tanto, el primer año de la era Heisei sería 平成元年 (*Heisei gannen*). Asegúrate de que tu aplicación maneje correctamente el primer año de la nueva era y presente correctamente 令和元年.

## <a name="related-apis"></a>API relacionadas

Hay varias API de WinRT, .NET y Win32 que se van a actualizar para manejar el cambio de era, por lo que, si las usas, no deberías tener demasiados problemas. Sin embargo, aunque confíes totalmente en estas API, sigue siendo una buena idea que pruebes la aplicación y te asegures de obtener el comportamiento buscado, especialmente si haces algo especial con ellas, como analizar.

Puedes seguir con las actualizaciones para el sistema operativo y el SDK en [Actualizaciones para el cambio de era japonesa de mayo de 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change).

Se verán afectadas las API siguientes:

### <a name="winrt"></a>WinRT

* [Windows.Globalization Namespace](https://docs.microsoft.com/uwp/api/windows.globalization)
  * [Clase Calendar](https://docs.microsoft.com/uwp/api/windows.globalization.calendar)
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
    * [Propiedad Era](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.era)
    * [Método EraAsString](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.eraasstring)
    * [Propiedad FirstYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.firstyearinthisera)
    * [Propiedad LastEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastera)
    * [Propiedad LastYearInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.lastyearinthisera)
    * [Propiedad NumberOfYearsInThisEra](https://docs.microsoft.com/uwp/api/windows.globalization.calendar.numberofyearsinthisera)
* [Espacio de nombres Windows.Globalization.DateTimeFormatting](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting)
  * [Clase DateTimeFormatter](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)
    * [Método Format](https://docs.microsoft.com/uwp/api/windows.globalization.datetimeformatting.datetimeformatter.format)

### <a name="net"></a>.NET

* [Espacio de nombres System](https://docs.microsoft.com/dotnet/api/system)
  * [Estructura DateTime](https://docs.microsoft.com/dotnet/api/system.datetime)
  * [Estructura DateTimeOffset](https://docs.microsoft.com/dotnet/api/system.datetimeoffset)
* [Espacio de nombres System.Globalization](https://docs.microsoft.com/dotnet/api/system.globalization)
  * [Clase Calendar](https://docs.microsoft.com/dotnet/api/system.globalization.calendar)
  * [Clase DateTimeFormatInfo](https://docs.microsoft.com/dotnet/api/system.globalization.datetimeformatinfo)
  * [Clase JapaneseCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japanesecalendar)
  * [Clase JapaneseLunisolarCalendar](https://docs.microsoft.com/dotnet/api/system.globalization.japaneselunisolarcalendar)

### <a name="win32"></a>Win32

* [Encabezado datetimeapi.h](https://docs.microsoft.com/windows/desktop/api/datetimeapi/)
  * [Función GetDateFormatA](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformata)
  * [Función GetDateFormatEx](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatex)
  * [Función GetDateFormatW](https://docs.microsoft.com/windows/desktop/api/datetimeapi/nf-datetimeapi-getdateformatw)
* [Encabezado winnls.h](https://docs.microsoft.com/windows/desktop/api/winnls/)
  * [Función EnumDateFormatsA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsa)
  * [Función EnumDateFormatsExA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexa)
  * [Función EnumDateFormatsExEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexex)
  * [Función EnumDateFormatsExW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsexw)
  * [Función EnumDateFormatsW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-enumdateformatsw)
  * [Función GetCalendarInfoA](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoa)
  * [Función GetCalendarInfoEx](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfoex)
  * [Función GetCalendarInfoW](https://docs.microsoft.com/windows/desktop/api/winnls/nf-winnls-getcalendarinfow)

## <a name="see-also"></a>Consulte también

* [Tratamiento de las eras en el calendario japonés](https://docs.microsoft.com/windows/desktop/Intl/era-handling-for-the-japanese-calendar)
* [Momento Y2K del calendario japonés](https://blogs.msdn.microsoft.com/shawnste/2018/04/12/the-japanese-calendars-y2k-moment/)
* [Uso del Registro para probar la nueva era japonesa en Windows](https://blogs.msdn.microsoft.com/shawnste/2018/08/07/using-the-registry-to-test-the-new-japanese-era-on-windows/)
* [Gannen frente a Ichinen](https://blogs.msdn.microsoft.com/shawnste/2018/11/12/gannen-vs-ichinen/)
* [Actualizaciones para el cambio de era japonesa de mayo de 2019](https://support.microsoft.com/help/4470918/updates-for-may-2019-japan-era-change)
* [Manejo de una nueva era del calendario japonés en .NET](https://devblogs.microsoft.com/dotnet/handling-a-new-era-in-the-japanese-calendar-in-net/)
