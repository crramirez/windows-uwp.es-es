---
author: DelfCo
Description: Usa la API de Windows.Globalization.DateTimeFormatting con patrones personalizados para mostrar fechas y horas en el formato exacto que quieras.
title: Usar patrones para dar formato a fechas y horas
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c28210a6fca27976a2a4428d8001da87951f1e87
ms.lasthandoff: 02/07/2017

---

# <a name="use-patterns-to-format-dates-and-times"></a>Usar patrones para dar formato a fechas y horas

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Usa la API [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) con patrones personalizados para mostrar fechas y horas en el formato exacto que quieras.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)</li>
<li>[**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)</li>
<li>[**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)</li>
</ul>
</div>


## <a name="introduction"></a>Introducción


[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) proporciona varias maneras de dar el formato adecuado a fechas y horas para los idiomas y las regiones de todo el mundo. Puedes usar los formatos estándar para el año, el mes, día etc., o puedes usar plantillas de cadena estándar, como "longdate" o "month day".

Sin embargo, cuando quieras tener más control sobre el orden y el formato de los constituyentes de la cadena [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) que quieres mostrar, puedes usar una sintaxis especial para el parámetro de plantilla de cadena, denominada "patrón". La sintaxis de patrón te permite obtener los elementos individuales que componen un objeto **DateTime** (solo el nombre del mes o el valor del año, por ejemplo) para mostrarlos en el formato personalizado que desees. Además, el patrón se puede localizar para adaptarse a otros idiomas y regiones.

**Nota**  Se trata de una descripción general de los patrones de formato. Para ver un análisis completo de los patrones y plantillas de formato, consulta la sección Comentarios de la documentación de la clase [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828).

 

## <a name="what-you-need-to-know"></a>Lo que debes saber


Es importante tener en cuenta que, al usar patrones, lo que haces es compilar un formato personalizado que tal vez no sea válido en otras referencias culturales. Por ejemplo, considera la plantilla "día del mes":

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Esto crea un formateador basado en el valor de región e idioma del contexto actual. Por lo tanto, siempre muestra el mes y el día junto con un formato global apropiado. Por ejemplo, muestra "January 1" para inglés (Estados Unidos), pero "1 janvier" para francés (Francia) y "1月1日" para japonés. Esto sucede porque la plantilla está basada en una cadena de patrón específica de una referencia cultural, a la que se puede acceder mediante la propiedad de patrón:

**C#**
```CSharp
var monthdaypattern = datefmt.Patterns;
```
**JavaScript**
```JavaScript
var monthdaypattern = datefmt.patterns;
```

Esto genera resultados diferentes en función del idioma y la región del formateador. Ten en cuenta que es posible que las diferentes regiones usen distintos elementos y con un orden distinto, con o sin caracteres y espaciado adicionales:

``` syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

Puedes usar patrones para crear un objeto [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828) personalizado, como este que está basado en el patrón de idioma inglés de Estados Unidos:

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Windows devuelve valores específicos de una referencia cultural para cada elemento dentro de los corchetes {}. Pero con la sintaxis de patrón, el orden de los constituyentes no varía. Obtienes exactamente lo que pides, que puede no ser culturalmente apropiado:

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol is missing)
```

Es más, no se garantiza que la coherencia de los patrones se mantenga con el tiempo. Los países o las regiones pueden cambiar sus sistemas de calendario, lo que modifica una plantilla de formato. Windows actualiza el resultado de los formateadores para tener en cuenta dichos cambios. Por eso, solo debes usar la sintaxis de patrón para dar formato a [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) si:

-   No dependes de un resultado específico para un formato.
-   No necesitas que el formato cumpla con ningún estándar específico de una referencia cultural.
-   Específicamente tienes previsto que el patrón no varíe de una cultura a otra.
-   Tienes la intención de localizar el patrón.

Este es un resumen de las diferencias entre las plantillas de cadena estándar y los patrones de cadena no estándar:

**Plantillas de cadena, como “month day”:**

-   Es una representación abstracta de un formato de [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) que incluye valores para el mes y el día, en algún orden.
-   Garantiza que devuelve un formato estándar válido en todos los valores de idioma y región que admite Windows.
-   Garantiza que te proporcionará una cadena con un formato apropiado según la referencia cultural para el idioma o la región específicos.
-   No todas las combinaciones de elementos son válidas. Por ejemplo, no hay una plantilla de cadena para "dayofweek day".

**Patrones de cadena, como “{month.full} {day.integer}”:**

-   Es una cadena con un orden explícito que expresa el nombre del mes completo, seguido de un espacio y seguido de un número entero de día, en ese orden.
-   Es posible que no corresponda con un formato estándar válido para ningún par de idioma y región.
-   No garantiza que sea culturalmente apropiado.
-   Se puede especificar cualquier combinación de elementos, en cualquier orden.

## <a name="tasks"></a>Tareas


Supongamos que quieres mostrar el mes y el día actuales junto con la hora actual en un formato específico. Por ejemplo, quieres que los usuarios que hablan inglés de Estados Unidos vean algo así:

``` syntax
June 25 | 1:38 PM
```

La parte de fecha corresponde a la plantilla "month day" y la parte de hora corresponde a la plantilla "hour minute". Por lo tanto, puedes crear un formato personalizado que concatene los patrones que conforman esas plantillas.

Primero, obtén los formateadores pertinentes para las plantillas de fecha y hora y, después, los patrones de esas plantillas:

**C#**
```CSharp
// Get formatters for the date part and the time part.
var mydate = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var mytime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.Patterns[0];
var mytimepattern = mytime.Patterns[0];
```
**JavaScript**
```JavaScript
// Get formatters for the date part and the time part.
var dtf = Windows.Globalization.DateTimeFormatting;
var mydate = dtf.DateTimeFormatter("month day");
var mytime = dtf.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.patterns[0];
var mytimepattern = mytime.patterns[0];
```

Debes almacenar tu formato personalizado como una cadena de recursos localizable. Por ejemplo, la cadena para Inglés (Estados Unidos) sería "{date} | {time}". Los localizadores pueden ajustar esta cadena según la necesidad. Por ejemplo, pueden cambiar el orden de los constituyentes si parece más natural en algunos idiomas o regiones tener la hora delante de la fecha. O pueden reemplazar "|" con algún otro carácter separador. En tiempo de ejecución, reemplazas las partes {date} y {time} de la cadena con el patrón pertinente:

**C#**
```CSharp
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader();
var mydateplustime = resourceLoader.GetString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```
**JavaScript**
```JavaScript
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var mydateplustime = WinJS.Resources.getString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```

Después puedes generar un nuevo formateador en función del patrón personalizado:

**C#**
```CSharp
// Get the custom formatter.
var mydateplustimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(mydateplustime);
```
**JavaScript**
```JavaScript
// Get the custom formatter.
var mydateplustimefmt = new dtf.DateTimeFormatter(mydateplustime);
```

## <a name="related-topics"></a>Temas relacionados


* [Ejemplo de formato de fecha y hora](http://go.microsoft.com/fwlink/p/?LinkId=231618)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Foundation.DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)
 

 




