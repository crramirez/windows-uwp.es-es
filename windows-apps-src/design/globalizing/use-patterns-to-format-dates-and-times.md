---
author: stevewhims
Description: Use the Windows.Globalization.DateTimeFormatting API with custom templates and patterns to display dates and times in exactly the format you wish.
title: Usar patrones para dar formato a fechas y horas
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 04a0288d0b28c12eb68cf56225747224e8df9777
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5743207"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>Usar plantillas para dar formato a fechas y horas

Usa las clases en el espacio de nombres [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) con plantillas y patrones personalizados para mostrar fechas y horas en el formato exacto que quieras.

## <a name="introduction"></a>Introducción

La clase [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) proporciona varias maneras de dar el formato adecuado a fechas y horas para los idiomas y las regiones de todo el mundo. Puedes usar formatos estándares para el año, el mes, el día, etc. O puedes pasar una plantilla de formato al argumento *formatTemplate* del constructor **DateTimeFormatter**, como "longdate" o "month day".

Sin embargo, cuando quieras tener aún más control sobre el orden y el formato de los componentes del objeto [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) que quieres mostrar, puedes pasar el patrón de formato al argumento *formatTemplate* del constructor. Un patrón de formato usa una sintaxis especial que te permite obtener los componentes individuales de un objeto **DateTime**&mdash;solo el nombre del mes o el valor del año, por ejemplo&mdash; para mostrarlos en el formato personalizado que desees. Además, el patrón se puede localizar para adaptarse a otros idiomas y regiones.

**Nota**esto es solo una descripción general de patrones de formato. Para ver un análisis completo de los patrones y plantillas de formato, consulta la sección Comentarios de la documentación de la clase [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live).

## <a name="the-difference-between-format-templates-and-format-patterns"></a>La diferencia entre los patrones y plantillas de formato

Una plantilla de formato es una cadena de formato independiente de la cultura. Por lo tanto, si construyes un **DateTimeFormatter** con una plantilla de formato, el formateador muestra los componentes del formato en el orden correcto del idioma actual. Por el contrario, los patrones de formato son específicos de cada cultura. Si construyes un **DateTimeFormatter** con un patrón de formato, el formateador usará el patrón exactamente como se ha indicado. Por lo tanto, un patrón no es necesariamente válido en varias referencias culturales.

Vamos a mostrar esta diferencia con un ejemplo. Pasaremos una plantilla de formato sencilla (no un patrón) al constructor **DateTimeFormatter**. Se trata de la plantilla de formato "month day".

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Esto crea un formateador basado en el valor de región e idioma del contexto actual. No importa el orden de los componentes de la plantilla de formato; el formateador los muestra en el orden correcto del idioma actual. Por ejemplo, muestra "January 1" para inglés (Estados Unidos), pero "1 janvier" para francés (Francia) y "1月1日" para japonés.

Por otro lado, los patrones de formato son específicos de cada cultura. Accedamos ahora al patrón de formato de nuestra plantilla de formato.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

Esto genera resultados diferentes en función del idioma y la región de tiempo de ejecución. Es posible que las diferentes regiones usen distintos componentes y con un orden distinto, con o sin caracteres y espaciado adicionales.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

En el ejemplo anterior, hemos introducido una cadena de formato independiente de la cultura y hemos vuelto a una cadena con formato específico de la cultura (que era una función del idioma y región que estaban en vigor en el momento en que llamamos `dateFormatter.Patterns`). Por tanto, si se construye un **DateTimeFormatter** a partir de un patrón de formato específico de la cultural, solo será válido para idiomas o regiones específicos.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

El formateador anterior devuelve valores específicos de la referencia cultural para los componentes individuales dentro de los corchetes {}. Pero el orden de los componentes de un patrón de formato no varía. Obtienes exactamente lo que pides, que puede o no ser culturalmente apropiado. Este formateador es válido para inglés (Estados Unidos), pero no para francés (Francia) ni para japonés.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

Además, un patrón que sea correcto hoy puede que no lo sea en el futuro. Los países o las regiones pueden cambiar sus sistemas de calendario, lo que modifica una plantilla de formato. Windows actualiza el resultado de los formateadores en función de las plantillas de formato para tener en cuenta dichos cambios. Por lo tanto, solo debes usar la sintaxis de patrón en una o varias de estas condiciones.

-   No dependes de un resultado específico para un formato.
-   No necesitas que el formato cumpla con ningún estándar específico de una referencia cultural.
-   Específicamente tienes previsto que el patrón no varíe de una cultura a otra.
-   Pretendes localizar la propia cadena de patrón de formato real.

Este es un resumen de la distinción entre los patrones y plantillas de formato.

**Plantillas de formato, como “month day”**

-   Es una representación abstracta de un formato de [DateTime](/uwp/api/windows.foundation.datetime?branch=live) que incluye valores para el mes, el día, etc. en algún orden.
-   Garantiza que devuelve un formato estándar válido en todos los valores de idioma y región que admite Windows.
-   Garantiza que te proporcionará una cadena con un formato apropiado según la referencia cultural para el idioma o la región específicos.
-   No todas las combinaciones de componentes son válidas. Por ejemplo, "dayofweek day" no es válido.

**Patrones de formato, como “{month.full} {day.integer}”**

-   Es una cadena con un orden explícito que expresa el nombre del mes completo, seguido de un espacio y seguido de un número entero de día, en ese orden o del patrón de formato especificado.
-   Es posible que no corresponda con un formato estándar válido para ningún par de idioma y región.
-   No garantiza que sea culturalmente apropiado.
-   Se puede especificar cualquier combinación de componentes, en cualquier orden.

## <a name="examples"></a>Ejemplos

Supongamos que quieres mostrar el mes y el día actuales junto con la hora actual en un formato específico. Por ejemplo, quieres que los usuarios que hablan inglés de Estados Unidos vean algo así:

``` syntax
June 25 | 1:38 PM
```

La parte de fecha corresponde a la plantilla de formato "month day" y la parte de hora corresponde a la plantilla de formato "hour minute". Por lo tanto, puedes crear los formateadores pertinentes para las plantillas de formato de fecha y hora correspondientes y concatenar sus salidas de forma conjunta con una cadena de formato localizable.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` es un identificador de recursos que hace referencia a un recurso localizable del archivo de recursos (.resw). Para un idioma predeterminado de inglés (Estados Unidos), se debe establecer en un valor de "{0} | {1}"junto con un comentario que indique que"{0}"es la fecha y"{1}"es el tiempo. De este modo, los traductores pueden ajustar los elementos de formato según sea necesario. Por ejemplo, pueden cambiar el orden de los elementos si parece más natural en algunos idiomas o regiones tener la hora delante de la fecha. O pueden reemplazar "|" con algún otro carácter separador.

Otra forma de implementar este ejemplo es solicitar a los dos formateadores sus patrones de formato, concatenarlos juntos y generar a un tercer formateador a partir del patrón de formato resultante.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>API importantes

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>Artículos relacionados

* [Ejemplo de formato de fecha y hora](http://go.microsoft.com/fwlink/p/?LinkId=231618)