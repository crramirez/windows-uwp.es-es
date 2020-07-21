---
Description: Use la API Windows. Globalization. DateTimeFormatting con patrones y plantillas personalizadas para mostrar fechas y horas exactamente en el formato que desee.
title: Usar patrones para dar formato a fechas y horas
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: da4d9b2c7380a085efdcb234ad210eafca40b1c3
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493610"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>Usar plantillas y patrones para dar formato a fechas y horas

Use las clases en el espacio de nombres [**Windows. Globalization. DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) con plantillas y patrones personalizados para mostrar las fechas y horas exactamente en el formato que desee.

## <a name="introduction"></a>Introducción

La clase [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) proporciona varias maneras de dar formato correctamente a fechas y horas para idiomas y regiones de todo el mundo. Puede usar formatos estándar para año, mes, día, etc. También puede pasar una plantilla de formato al argumento *formatTemplate* del constructor **DateTimeFormatter** , como "LongDate" o "month Day".

Pero si desea tener aún más control sobre el orden y el formato de los componentes del objeto [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) que desea mostrar, puede pasar un modelo de formato al argumento *formatTemplate* del constructor. Un modelo de formato usa una sintaxis especial, que permite obtener componentes individuales de un objeto **DateTime** &mdash; solo el nombre del mes, o simplemente el valor del año, por ejemplo &mdash; para mostrarlos en el formato personalizado que elija. Además, el patrón se puede localizar para adaptarse a otros idiomas y regiones.

**Nota:**    Esta es solo una introducción a los patrones de formato. Para ver un análisis completo de los patrones y plantillas de formato, consulta la sección Comentarios de la documentación de la clase [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live).

## <a name="the-difference-between-format-templates-and-format-patterns"></a>La diferencia entre las plantillas de formato y los patrones de formato

Una plantilla de formato es una cadena de formato independiente de la referencia cultural. Por lo tanto, si se construye un **DateTimeFormatter** con una plantilla de formato, el formateador muestra los componentes de formato en el orden correcto para el idioma actual. A la inversa, un modelo de formato es específico de la referencia cultural. Si construye un **DateTimeFormatter** con un modelo de formato, el formateador usará el patrón exactamente como se indica. Por consiguiente, un patrón no es necesssarily válido en todas las referencias culturales.

Vamos a ilustrar esta distinción con un ejemplo. Pasaremos una plantilla de formato simple (no un patrón) al constructor **DateTimeFormatter** . Esta es la plantilla de formato "month Day".

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

Esto crea un formateador basado en el valor de región e idioma del contexto actual. No importa el orden de los componentes en una plantilla de formato; el formateador las muestra en el orden correcto para el lenguaje actual. Por lo tanto, muestra "1 de enero" para inglés (Estados Unidos), "1 Janvier" para francés (Francia) y "1 月 1 日" para japonés.

Por otro lado, un modelo de formato es específico de la referencia cultural. Vamos a acceder al patrón de formato de nuestra plantilla de formato.

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

Esto produce resultados diferentes en función del idioma y la región en tiempo de ejecución. Distintas regiones pueden utilizar distintos componentes, en distintos pedidos, con o sin caracteres y espaciado adicionales.

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

En el ejemplo anterior, se ha introducido una cadena de formato independiente de la referencia cultural y se ha devuelto una cadena de formato específica de la referencia cultural (que era una función del idioma y la región que se producían en vigor cuando llamamos a `dateFormatter.Patterns` ). Por lo tanto, si crea un **DateTimeFormatter** a partir de un patrón de formato específico de la referencia cultural, solo será válido para determinados idiomas o regiones.

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

El formateador anterior devuelve valores específicos de la referencia cultural para los componentes individuales dentro de los corchetes {} . Pero el orden de los componentes en un modelo de formato es invariable. Obtendrá exactamente lo que le pide, que puede ser o no ser culturalmente apropiado. Este formateador es válido para inglés (Estados Unidos), pero no para francés (Francia) ni para japonés.

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

Además, un patrón correcto hoy podría no ser correcto en el futuro. Los países o regiones pueden cambiar sus sistemas de calendario, que modifican una plantilla de formato. Windows actualiza la salida de los formateadores basados en las plantillas de formato para dar cabida a dichos cambios. Por lo tanto, solo debe utilizar la sintaxis de patrón en una o más de estas condiciones.

-   No dependes de un resultado específico para un formato.
-   No necesitas que el formato cumpla con ningún estándar específico de una referencia cultural.
-   Específicamente tienes previsto que el patrón no varíe de una cultura a otra.
-   Piensa localizar la propia cadena de modelo de formato real.

A continuación se muestra un resumen de la distinción entre plantillas de formato y modelos de formato.

**Plantillas de formato, como "month Day"**

-   Representación abstracta de un formato de [fecha y hora](/uwp/api/windows.foundation.datetime?branch=live) que incluye valores para el mes, día, etc., en cualquier orden.
-   Garantiza que devuelve un formato estándar válido en todos los valores de idioma y región que admite Windows.
-   Garantiza que te proporcionará una cadena con un formato apropiado según la referencia cultural para el idioma o la región específicos.
-   No todas las combinaciones de componentes son válidas. Por ejemplo, "DayOfWeek Day" no es válido.

**Modelos de formato, como "{month. Full} {Day. integer}"**

-   Cadena ordenada explícitamente que expresa el nombre completo del mes, seguido de un espacio, seguido del entero del día, en ese orden o en el patrón de formato específico que especifique.
-   Es posible que no corresponda con un formato estándar válido para ningún par de idioma y región.
-   No garantiza que sea culturalmente apropiado.
-   Se puede especificar cualquier combinación de componentes, en cualquier orden.

## <a name="examples"></a>Ejemplos

Supongamos que quieres mostrar el mes y el día actuales junto con la hora actual en un formato específico. Por ejemplo, quieres que los usuarios que hablan inglés de Estados Unidos vean algo así:

``` syntax
June 25 | 1:38 PM
```

La parte de fecha corresponde a la plantilla de formato "month Day" y la parte de hora correspondiente a la plantilla de formato "hour minute". Por lo tanto, puede crear formateadores para las plantillas de formato de fecha y hora pertinentes y, a continuación, concatenar sus resultados junto con una cadena de formato localizable.

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString`es un identificador de recursos que hace referencia a un recurso traducible en un archivo de recursos (. resw). Para un idioma predeterminado de inglés (Estados Unidos), se establecerá en un valor de " {0} | {1} " junto con un comentario que indica que " {0} " es la fecha y " {1} " es la hora. De este modo, los traductores pueden ajustar los elementos de formato según sea necesario. Por ejemplo, pueden cambiar el orden de los elementos si parece más natural en algún idioma o región que preceda a la fecha. O pueden reemplazar "|" con algún otro carácter separador.

Otra manera de implementar este ejemplo consiste en consultar los modelos de formato de los dos formateadores, concatenarlos y, a continuación, construir un tercer formateador a partir del patrón de formato resultante.

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

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de formato de fecha y hora](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
