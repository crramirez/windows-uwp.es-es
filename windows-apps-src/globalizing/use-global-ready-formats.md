---
author: DelfCo
Description: "Desarrolla una aplicación que todo el mundo pueda usar empleando el formato adecuado en fechas, horas, números de teléfono y divisas."
title: Usar formatos globales
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 5255da14ccdd0aed3852c41fa662de63a7160fba
ms.openlocfilehash: 3615d1301a9d163390a2d709690c1e583c9b4f7e

---

# Usar formatos globales

**API importantes**

-   [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
-   [**Windows.Globalization.PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting)

Desarrolla una aplicación que todo el mundo pueda usar empleando el formato adecuado en fechas, horas, números de teléfono y divisas. De esta forma, podrás adaptar la aplicación más adelante a otras culturas, regiones e idiomas del mercado global.

## Introducción

Muchos desarrolladores de aplicaciones crean de forma natural sus aplicaciones teniendo en cuenta únicamente su propio idioma y cultura. Pero cuando la aplicación empieza a crecer en otras regiones de comercialización, su adaptación a nuevos idiomas y regiones puede resultar difícil de formas inesperadas. Por ejemplo, elementos como las fechas, horarios, números, calendarios, monedas, números telefónicos, unidades de medida y tamaños de papel se pueden mostrar de diferente forma según la cultura o el idioma.

Este proceso de adaptación a nuevos mercados se puede simplificar teniendo en cuenta ciertas cuestiones a la hora de desarrollar la aplicación.

## Da un formato adecuado a las fechas y a las horas

Existen muchas formas diferentes de mostrar correctamente las fechas y las horas. Diferentes regiones y culturas usan convenciones distintas para el orden del mes y el día en la fecha, para la separación de horas y minutos en la hora e, incluso, para el separador usado. Además, las fechas se pueden mostrar en varios formatos largos ("Miércoles, 28 de marzo, 2012") o cortos ("28/3/12"), lo que puede variar de una cultura a otra. Y, por supuesto, los nombres y las abreviaturas de los días de la semana y los meses del año varían según cada idioma.

Si necesitas que los usuarios puedan elegir una fecha o seleccionar una hora, usa los controles de [selector de fecha y hora](https://msdn.microsoft.com/library/windows/apps/hh465466) estándar. Estos usarán de forma automática los formatos de fecha y hora del idioma y la región predeterminados del usuario.

Si tienes que mostrar fechas u horas, usa los formateadores [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) y [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) para mostrar automáticamente el formato preferido del usuario para fechas, horas y números. El código siguiente formatea un valor DateTime dado con el idioma y región preferidos actuales. Por ejemplo, si la fecha actual es 3 de junio de 2012, el formateador dará como resultado "6/3/2012" si el usuario prefiere el inglés (Estados Unidos) o "03.06.2012" si el usuario prefiere el alemán (Alemania):

```CSharp
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

## Da un formato adecuado a los números y a las monedas

Las diferentes culturas dan un formato distinto a los números. Entre esas diferencias de formato podemos encontrar la cantidad de decimales para mostrar, los caracteres usados como separadores decimales y el símbolo de moneda que se va a usar. Usa [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136) para mostrar decimales, porcentajes o tantos por mil, y monedas. En la mayoría de los casos, simplemente muestras números o monedas según las preferencias actuales del usuario. Pero también puedes usar los formateadores para mostrar una moneda para una región o un formato particular.

El siguiente código te muestra un ejemplo de cómo mostrar monedas según la región y el idioma preferidos del usuario, o para un sistema monetario determinado:

```CSharp
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

## Usa un calendario apropiado culturalmente

El calendario difiere según las regiones y los idiomas. Ten en cuenta que el calendario gregoriano no es el predeterminado de todas las regiones. Los usuarios de algunas regiones pueden elegir calendarios alternativos, como el calendario de la era japonesa o el calendario lunar árabe. Las fechas y horas en el calendario se basan en diferentes zonas horarias y el horario de verano.

Usa los controles de [selector de fecha y hora](https://msdn.microsoft.com/library/windows/apps/hh465466) estándar para que los usuarios puedan elegir una fecha y asegurarte de que se usa el formato de calendario preferido. Si quieres ver escenarios más complejos en los que puede ser necesario trabajar directamente con operaciones en fechas del calendario, Windows.Globalization proporciona una clase [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) que ofrece una representación de calendario adecuada para la cultura, la región y el tipo de calendario indicados.

## Da un formato adecuado a los números de teléfono
Los números de teléfono tienen un formato diferente según la región. El número de dígitos, cómo se agrupan esos dígitos y el significado de determinadas partes del número de teléfono varían de un país a otro. A partir de la versión 1607 de Windows 10, puedes usar [**PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting) para dar el formato adecuado a los números de teléfono de la región actual.

[**PhoneNumberInfo**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberinfo.aspx) analiza una cadena de dígitos y te permite determinar si esos dígitos pertenecen a un número de teléfono válido de la región, comparar dos números de teléfonos para ver si son iguales y extraer las diferentes partes funcionales del número de teléfono (por ejemplo, el código del país o el código del área geográfica).

[**PhoneNumberFormatter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberformatter.aspx) da formato a una cadena de dígitos o al elemento PhoneNumberInfo que se mostrará, incluso si la cadena de dígitos representa un número de teléfono parcial. (Puedes usar este formato de número parcial para dar formato a un número a medida que el usuario lo esté escribiendo). 

El siguiente código muestra cómo usar PhoneNumberFormatter para dar formato a un número de teléfono a medida que un usuario lo vaya escribiendo. Cada vez que el texto cambia en un elemento TextBox denominado gradualInput, se usa la región actual predeterminada para dar formato al contenido del cuadro de texto y mostrarlo en un TextBlock denominado outBox. A modo de ejemplo, se da formato a la cadena mediante la región correspondiente a Nueva Zelanda y se muestra en un TextBlock denominado NZOutBox.
    
```csharp
    using Windows.Globalization;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        // Note that you must check the results of TryCreate before you use the formatter.
        PhoneNumberFormatter.TryCreate("NZ", out NZFormatter);

    }

    private void gradualInput_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region into outBox.
        outBox.Text = currentFormatter.FormatPartialString(gradualInput.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZOutBox.
        if(NZFormatter != null)
        {
            NZOutBox.Text = NZFormatter.FormatPartialString(gradualInput.Text);
        }
    }
```    

## Respeta las preferencias de idioma y culturales del usuario

Si quieres ver escenarios en los que puedes ofrecer funcionalidades diferentes según las preferencias culturales, la región o el idioma del usuario, Windows te ofrece una forma de obtener acceso a estas preferencias, mediante [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825). Cuando sea necesario, usa la clase **GlobalizationPreferences** para conseguir el valor de la región geográfica actual del usuario, sus idiomas preferidos, monedas preferidas, etc.

## Temas relacionados

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



<!--HONumber=Aug16_HO3-->


