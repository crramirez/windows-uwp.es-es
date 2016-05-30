---
author: DelfCo
Description: Desarrolla una aplicación lista para todo el mundo mediante la aplicación del formato adecuado a las fechas, las horas, los números y las divisas.
title: Usar formatos globales
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
---

# <span id="dev_globalizing.use_global-ready_formats"></span>Usar formatos globales





**API importantes**

-   [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)

Desarrolla una aplicación lista para todo el mundo mediante la aplicación del formato adecuado a las fechas, las horas, los números y las divisas. De esta forma, podrás adaptarla más adelante para otras culturas, regiones e idiomas en el mercado global.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Introducción


Muchos desarrolladores de aplicaciones crean de forma natural sus aplicaciones teniendo en cuenta únicamente su propio idioma y cultura. Pero cuando la aplicación empieza a crecer en otras regiones de comercialización, su adaptación a nuevos idiomas y regiones puede resultar difícil de formas inesperadas. Por ejemplo, elementos como las fechas, horarios, números, calendarios, monedas, números telefónicos, unidades de medida y tamaños de papel se pueden mostrar de diferente forma según la cultura o el idioma.

Este proceso de adaptación a nuevos mercados se puede simplificar teniendo en cuenta ciertas cuestiones a la hora de desarrollar tu aplicación.

## <span id="Prerequisites"></span><span id="prerequisites"></span><span id="PREREQUISITES"></span>Requisitos previos


[Planeación para un mercado global](https://msdn.microsoft.com/library/windows/apps/hh465405)
## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Tareas


1.  **Da un formato adecuado a las fechas y las horas.**

    Existen muchas formas diferentes de mostrar correctamente fechas y horas. Diferentes regiones y culturas usan convenciones distintas para el orden del mes y el día en la fecha, para la separación de horas y minutos en la hora e, incluso, para el separador usado. Además, las fechas se pueden mostrar en varios formatos largos ("Miércoles, 28 de marzo, 2012") o cortos ("28/3/12"), lo que puede variar de una cultura a otra. Y, por supuesto, los nombres y las abreviaturas de los días de la semana y los meses del año varían según cada idioma.

    Si necesitas que los usuarios puedan elegir una fecha o seleccionar una hora, usa los controles de [selector de fecha y hora](https://msdn.microsoft.com/library/windows/apps/hh465466) estándar. Estos usarán de forma automática los formatos de fecha y hora del idioma y la región predeterminados del usuario.

    Si tienes que mostrar fechas u horas, usa los formateadores [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) y [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) para mostrar automáticamente el formato preferido del usuario para fechas, horas y números. El código siguiente formatea un valor DateTime dado con el idioma y región preferidos actuales. Por ejemplo, si la fecha actual es 3 de junio de 2012, el formateador dará como resultado "6/3/2012" si el usuario prefiere inglés (Estados Unidos) o "03.06.2012" si el usuario prefiere alemán (Alemania):

    **C#**
    ```    CSharp
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = DateTime.Now;

    // Perform the actual formatting.
    var sdate = sdatefmt.Format(dateToFormat);
    var stime = stimefmt.Format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```
    **JavaScript**
    ```    JavaScript
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = new Date();

    // Perform the actual formatting.
    var sdate = sdatefmt.format(dateToFormat);
    var stime = stimefmt.format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
    ```

2.  **Da un formato adecuado a los números y las monedas.**

    Las diferentes culturas dan un formato distinto a los números. Entre esas diferencias de formato podemos encontrar la cantidad de decimales para mostrar, los caracteres usados como separadores decimales y el símbolo de moneda que se va a usar. Usa [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136) para mostrar decimales, porcentajes o tantos por mil, y monedas. En la mayoría de los casos, simplemente muestras números o monedas según las preferencias actuales del usuario. Pero también puedes usar los formateadores para mostrar una moneda para una región o un formato particular.

    El siguiente código te muestra un ejemplo de cómo mostrar monedas según la región y el idioma preferidos del usuario, o para un sistema monetario determinado:

    **C#**
    ```    CSharp
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyEuroFR = currencyFormatEuroFR.Format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```
    **JavaScript**
    ```    JavaScript
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.currencies;

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", ["fr-FR"], "FR");
    var currencyEuroFR = currencyFormatEuroFR.format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
    ```

3.  **Usa un calendario apropiado culturalmente.**

    El calendario difiere según las regiones e idiomas. El calendario gregoriano no es el predeterminado de todas las regiones. Los usuarios de algunas regiones pueden elegir calendarios alternativos, como el calendario de la era japonesa o el calendario lunar árabe. Las fechas y horas en el calendario se basan en diferentes zonas horarias y el horario de verano.

    Usa los controles de [selector de fecha y hora](https://msdn.microsoft.com/library/windows/apps/hh465466) estándar para que los usuarios puedan elegir una fecha y asegurarte de que se usa el formato de calendario preferido. Si quieres ver escenarios más complejos en los que puede ser necesario trabajar directamente con operaciones en fechas del calendario, Windows.Globalization proporciona una clase [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) que ofrece una representación de calendario adecuada para la cultura, la región y el tipo de calendario indicados.

4.  **Respeta las preferencias de idioma y culturales del usuario.**

    Si quieres ver escenarios en los que puedes ofrecer funcionalidad diferente según las preferencias culturales, la región o el idioma del usuario, Windows te ofrece una forma de acceder a estas preferencias, mediante [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825). Cuando sea necesario, usa la clase **GlobalizationPreferences** para conseguir el valor de la región geográfica actual del usuario, sus idiomas preferidos, monedas preferidas, etc.

## <span id="related_topics"></span>Temas relacionados


* [Planeación para un mercado global](https://msdn.microsoft.com/library/windows/apps/hh465405)
* [Directrices para los controles de fecha y hora](https://msdn.microsoft.com/library/windows/apps/hh465466)

**Referencia**
* [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
* [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825)

**Ejemplos**
* [Detalles del calendario y ejemplo matemático](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [Ejemplo de formato de fecha y hora](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [Ejemplo de preferencias de globalización](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [Ejemplo de análisis y formato de números](http://go.microsoft.com/fwlink/p/?linkid=231620)
 

 





<!--HONumber=May16_HO2-->


