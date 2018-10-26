---
author: stevewhims
Description: Design your app to be global-ready by appropriately formatting dates, times, numbers, phone numbers, and currencies. You'll then be able later to adapt your app for additional cultures, regions, and languages in the global market.
title: Globalizar los formatos de fecha y hora o número
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.author: stwhi
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 173198c2c61530704dad02e2e92e6a7e47aae420
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5564426"
---
# <a name="globalize-your-datetimenumber-formats"></a>Globalizar los formatos de fecha y hora o número

Diseña una aplicación que todo el mundo pueda usar empleando el formato adecuado en fechas, horas, números de teléfono y divisas. Así podrás adaptar la aplicación más adelante a otras culturas, regiones e idiomas del mercado global.

## <a name="introduction"></a>Introducción

Al crear la aplicación, si piensas en más que un solo idioma y una sola cultura, tendrás menos problemas inesperados (o ninguno) cuando la aplicación crezca en los nuevos mercados. Por ejemplo, elementos como las fechas, horarios, números, calendarios, monedas, números telefónicos, unidades de medida y tamaños de papel se pueden mostrar de diferente forma según la cultura o el idioma.

Las diferentes regiones y culturas usan diferentes formatos de fecha y hora. Incluyen convenciones distintas para el orden del mes y el día en la fecha, para la separación de horas y minutos en la hora e, incluso, para el separador usado. Además, las fechas se pueden mostrar en varios formatos largos ("Miércoles, 28 de marzo, 2012") o cortos ("28/3/12"), lo que varía de una cultura a otra. Y, por supuesto, los nombres y las abreviaturas de los días de la semana y los meses del año varían según cada idioma.

Puedes obtener una vista previa de los formatos utilizados para los diferentes idiomas. Ve a **Configuración** > **Hora e idioma** > **Región e idioma** y haz clic en **Opciones adicionales de fecha, hora y configuración regional** > **Cambiar formatos de fecha, hora o número**. En la pestaña **Formatos** selecciona un idioma de la lista desplegable **Formato** y obtendrás una vista previa de los formatos en **Ejemplos **.

Este tema usa los términos "Lista de idiomas del perfil del usuario", "Lista de idiomas de manifiesto de la aplicación" y "Lista de idiomas del tiempo de ejecución de la aplicación". Para obtener información detallada sobre el significado exacto de esos términos y cómo acceder a sus valores, consulta [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md).

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>Dar formato a fechas y horas para la lista de idiomas del tiempo de ejecución

Si necesitas que los usuarios puedan elegir una fecha o seleccionar una hora, usa el [Calendario, controles de fecha y hora](../controls-and-patterns/date-and-time.md) estándar. Estos utilizan automáticamente el mejor formato de fecha y hora de la lista de idiomas del tiempo de ejecución.

Si tienes que mostrar fechas u horas, puedes usar la clase [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live). De manera predeterminada, **DateTimeFormatter** utiliza automáticamente el mejor formato de fecha y hora de la lista de idiomas del tiempo de ejecución. Por lo tanto, el código siguiente formatea un valor **DateTime** determinado de la mejor manera para esa lista. Como ejemplo, supongamos que la lista de idiomas de manifiesto de aplicación incluye inglés (Estados Unidos), que es también el valor predeterminado, y alemán (Alemania). Si la fecha actual es 6 de noviembre de 2017 y la lista de idiomas de perfil de usuario contiene primero alemán (Alemania), el formateador dará como resultado "06/11/2017". Si la lista de idiomas de perfil de usuario contiene primero inglés (Estados Unidos) (o bien, si no contiene ni inglés ni alemán), el formateador dará como resultado "6/11/2017" (ya que "en-US" coincide con o se usa como el valor predeterminado).

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

Puedes probar el código anterior en tu propio PC así.

- Asegúrate de que los archivos de recursos en el proyecto estén calificados para "en-US" y "de-DE" (consulta [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- Cambia la lista de idiomas de perfil de usuario en **Configuración** > **Hora e idioma** > **Región e idioma** > **Idiomas **. Agrega Alemán (Alemania), márcalo como el valor predeterminado y vuelve a ejecutar el código.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>Dar formato a fechas y horas para la lista de idiomas del perfil de usuario

Recuerda que, de manera predeterminada, **DateTimeFormatter** coincide con la lista de idiomas del tiempo de ejecución. De este modo, si se muestran cadenas como "La fecha es &lt;fecha&gt;", el idioma coincidirá con el formato de fecha.

Si por cualquier motivo quieres dar formato a las fechas u horas solo según la lista de idiomas de perfil de usuario, puedes hacerlo con un código como en el siguiente ejemplo. Pero si lo haces, debes saber que puede que el usuario elija un idioma para el que la aplicación no tenga cadenas traducidas. Por ejemplo, si la aplicación no está localizada en Alemán (Alemania), pero el usuario lo elige como idioma preferido, podrían mostrarse cadenas probablemente peculiares cadenas como "La fecha es 06/11/2017".

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>Dar un formato adecuado a los números y a las monedas

Las diferentes culturas dan un formato distinto a los números. Entre esas diferencias de formato podemos encontrar la cantidad de decimales para mostrar, los caracteres usados como separadores decimales y el símbolo de moneda que se va a usar. Usa las clases del espacio de nombres [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) para mostrar decimales, porcentajes o tantos por mil y monedas. La mayoría de las veces, querrás que estas clases de formateador usen el mejor formato para el perfil de usuario. Pero también puedes usar los formateadores para mostrar una moneda para cualquier región o un formato.

Este ejemplo muestra cómo mostrar monedas según el perfil de usuario y para un sistema monetario determinado y específico.

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

Puedes probar el código anterior en tu PC cambiando el país o región en **Configuración** > **Hora e idioma** > **Región e idioma** > **País o región **. Elige un país o región (quizás Islandia) y vuelve a ejecutar el código.

## <a name="use-a-culturally-appropriate-calendar"></a>Usar un calendario apropiado culturalmente

El calendario difiere según las regiones y los idiomas. Ten en cuenta que el calendario gregoriano no es el predeterminado de todas las regiones. Los usuarios de algunas regiones pueden elegir calendarios alternativos, como el calendario de la era japonesa o el calendario lunar árabe. Las fechas y horas en el calendario se basan en diferentes zonas horarias y el horario de verano.

Para asegurarte de que se emplee el formato de calendario preferido, puedes usar los [Controles de calendario, fecha y hora](../controls-and-patterns/date-and-time.md) estándar. Si quieres ver escenarios más complejos en los que puede ser necesario trabajar directamente con operaciones en fechas del calendario, **Windows.Globalization** proporciona una clase [**Calendario**](/uwp/api/windows.globalization.calendar?branch=live) que ofrece una representación de calendario adecuada para la cultura, la región y el tipo de calendario indicados.

## <a name="format-phone-numbers-appropriately"></a>Da un formato adecuado a los números de teléfono

Los números de teléfono tienen un formato diferente según la región. El número de dígitos, cómo se agrupan esos dígitos y el significado de determinadas partes del número de teléfono varían de un país a otro. A partir de la versión 1607 de Windows 10, puedes usar las clases en el espacio de nombres [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) para dar el formato adecuado a los números de teléfono de la región actual.

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) analiza una cadena de dígitos y te permite: determinar si esos dígitos pertenecen a un número de teléfono válido de la región, comparar dos números de teléfonos para ver si son iguales y extraer las diferentes partes funcionales del número de teléfono (por ejemplo, el código del país o el código del área geográfica).

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) da formato a una cadena de dígitos o al elemento **PhoneNumberInfo** que se mostrará, incluso si la cadena de dígitos representa un número de teléfono parcial. Puedes usar este formato de número parcial para dar formato a un número a medida que el usuario lo esté escribiendo.

El siguiente ejemplo muestra cómo usar **PhoneNumberFormatter** para dar formato a un número de teléfono a medida que un usuario lo vaya escribiendo. Cada vez que el texto cambia en un elemento **TextBox** denominado phoneNumberInputTextBox, se usa la región actual predeterminada para dar formato al contenido del cuadro de texto y mostrarlo en un **TextBlock** denominado phoneNumberOutputTextBlock. A modo de ejemplo, se da formato a la cadena mediante la región correspondiente a Nueva Zelanda y se muestra en un TextBlock denominado phoneNumberOutputTextBlockNZ.
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

Puedes probar el código anterior en tu PC cambiando el país o región en **Configuración** > **Hora e idioma** > **Región e idioma** > **País o región **. Elige un país o región (quizás Nueva Zelanda para confirmar que coinciden los formatos) y vuelve a ejecutar el código. Para los datos de prueba, puedes hacer una búsqueda web del número de teléfono de un negocio en Nueva Zelanda.

## <a name="the-users-language-and-cultural-preferences"></a>Las preferencias de idioma y culturales del usuario

En los escenarios en los que quieras ofrecer funcionalidades diferentes solo según las preferencias culturales, la región o el idioma del usuario, Windows te ofrece una forma de obtener acceso a estas preferencias, mediante [**Windows.System.UserProfile.GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live). Cuando sea necesario, usa la clase **GlobalizationPreferences** para conseguir el valor de la región geográfica actual del usuario, sus idiomas preferidos, monedas preferidas, etc. Pero recuerda que si las cadenas o las imágenes de la aplicación no están localizadas para idioma preferido del usuario, las fechas y horas y otros datos fomateados para el idioma preferido no coincidirán con las cadenas que se muestran.

## <a name="important-apis"></a>API importantes

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Calendario](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>Artículos relacionados

* [Calendario, controles de fecha y hora](../controls-and-patterns/date-and-time.md)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md)
* [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>Muestras

* [Detalles del calendario y ejemplo matemático](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [Ejemplo de formato de fecha y hora](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [Ejemplo de preferencias de globalización](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [Ejemplo de análisis y formato de números](http://go.microsoft.com/fwlink/p/?linkid=231620)
