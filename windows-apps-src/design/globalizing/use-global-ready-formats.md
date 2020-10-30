---
description: Diseñe la aplicación para que esté preparada para el uso global mediante el formato adecuado de fechas, horas, números, números de teléfono y monedas. Después podrá adaptar su aplicación para obtener más referencias culturales, regiones y idiomas en el mercado mundial.
title: Globalizar los formatos de fecha, hora y número
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.date: 11/07/2017
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 8c3bacbfbbe944cddfe014fcd34038ca9a56c36e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034348"
---
# <a name="globalize-your-datetimenumber-formats"></a>Globalizar los formatos de fecha, hora y número

Diseñe la aplicación para que esté preparada para el uso global mediante el formato adecuado de fechas, horas, números, números de teléfono y monedas. Después podrá adaptar su aplicación para obtener más referencias culturales, regiones y idiomas en el mercado mundial.

## <a name="introduction"></a>Introducción

Al crear la aplicación, si cree una idea más amplia que un único idioma y una referencia cultural, tendrá menos problemas inesperados (si los hay) cuando la aplicación crezca en nuevos mercados. Por ejemplo, elementos como las fechas, horarios, números, calendarios, monedas, números telefónicos, unidades de medida y tamaños de papel se pueden mostrar de diferente forma según la cultura o el idioma.

Las distintas regiones y referencias culturales usan diferentes formatos de fecha y hora. Incluyen convenciones para el orden del día y el mes de la fecha, para la separación de horas y minutos en el tiempo, e incluso para qué puntuación se usa como separador. Además, las fechas se pueden mostrar en varios formatos largos ("miércoles, 28 de marzo de 2012") o formatos cortos ("3/28/12"), que varían en las referencias culturales. Y, por supuesto, los nombres y las abreviaturas de los días de la semana y los meses del año difieren entre los idiomas.

Puede obtener una vista previa de los formatos usados para los distintos idiomas. Vaya a **configuración**  >  **hora &**  >  **región de idioma & idioma** y haga clic en **fecha adicional, hora, & configuración regional**  >  **cambiar los formatos de fecha, hora o número** . En la pestaña **formatos** , seleccione un idioma en la lista desplegable **formato** y obtenga una vista previa de los formatos en **ejemplos** .

En este tema se usan los términos "lista de lenguaje de Perfil de usuario", "lista de idiomas del manifiesto de la aplicación" y "lista de idiomas del tiempo de ejecución de la aplicación". Para obtener información detallada sobre exactamente lo que significan esos términos y cómo obtener acceso a sus valores, consulte comprender los lenguajes de [Perfil de usuario y los lenguajes de manifiesto de aplicación](manage-language-and-region.md).

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>Dar formato a las fechas y horas para la lista de idiomas del tiempo de ejecución de aplicaciones

Si necesita permitir que los usuarios elijan una fecha, o para seleccionar una hora, use los controles estándar de [calendario, fecha y hora](../controls-and-patterns/date-and-time.md). Estos usan automáticamente el mejor formato de fecha y hora para la lista de idiomas de tiempo de ejecución de la aplicación.

Si necesita mostrar fechas u horas usted mismo, puede usar la clase [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) . De forma predeterminada, **DateTimeFormatter** usa automáticamente el mejor formato de fecha y hora para la lista de idiomas de tiempo de ejecución de la aplicación. Por lo tanto, el código siguiente da formato a una **fecha y hora** determinada de la mejor manera para esa lista. Por ejemplo, supongamos que la lista de idiomas del manifiesto de la aplicación incluye inglés (Estados Unidos), que también es el valor predeterminado y alemán (Alemania). Si la fecha actual es 6 2017 de noviembre y la lista de idioma del perfil de usuario contiene el alemán (Alemania) en primer lugar, el formateador dará "06.11.2017". Si la lista de idioma del perfil de usuario contiene el inglés (Estados Unidos) en primer lugar (o si no contiene el inglés ni el alemán), el formateador dará "11/6/2017" (ya que "en-US" coincide o se utiliza como valor predeterminado).

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

Puede probar el código anterior en su propio equipo, como este.

- Asegúrese de que tiene archivos de recursos en el proyecto calificados para "en-US" y "de-DE" (consulte [adaptar los recursos para el idioma, la escala, el contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)).
- Cambie la lista de idiomas de Perfil de usuario en **configuración**  >  **hora & región de idioma**  >  **&**  >  **idiomas** . Agregue el alemán (Alemania), conviértalo en el valor predeterminado y vuelva a ejecutar el código.

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>Dar formato a las fechas y horas para la lista de idiomas del perfil de usuario

Recuerde que, de forma predeterminada, **DateTimeFormatter** coincide con la lista de idiomas de tiempo de ejecución de la aplicación. De este modo, si se muestran cadenas como "la fecha es la fecha &lt; &gt; ", el idioma coincidirá con el formato de fecha.

Si, por cualquier motivo, desea dar formato a las fechas y horas solo según la lista de idiomas del perfil de usuario, puede hacerlo mediante código como el ejemplo siguiente. Pero si lo hace así, comprenda que el usuario puede elegir un idioma para el que la aplicación no tenga cadenas traducidas. Por ejemplo, si la aplicación no está localizada en alemán (Alemania), pero el usuario la elige como su lenguaje preferido, esto podría dar lugar a la visualización de cadenas posiblemente impares, como "la fecha es 06.11.2017".

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>Da un formato adecuado a los números y a las monedas

Las diferentes culturas dan un formato distinto a los números. Entre esas diferencias de formato podemos encontrar la cantidad de decimales para mostrar, los caracteres usados como separadores decimales y el símbolo de moneda que se va a usar. Use las clases del espacio de nombres [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) para mostrar los números de decimales, porcentajes o de Percent y las monedas. La mayoría de las veces, querrá que estas clases de formateador usen el mejor formato para el perfil de usuario. Sin embargo, puede usar los formateadores para mostrar una moneda para cualquier región o formato.

En este ejemplo se muestra cómo mostrar monedas tanto por Perfil de usuario como por un determinado sistema de moneda.

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

Para probar el código anterior en su propio equipo, cambie el país o región en **configuración**  >  **hora & región de idioma**  >  **&**  >  **país o región** de idioma. Elija un país o región (quizás Islandia) y vuelva a ejecutar el código.

## <a name="use-a-culturally-appropriate-calendar"></a>Usa un calendario apropiado culturalmente

El calendario difiere según las regiones y los idiomas. Ten en cuenta que el calendario gregoriano no es el predeterminado de todas las regiones. Los usuarios de algunas regiones pueden elegir calendarios alternativos, como el calendario de la era japonesa o los calendarios lunares árabes. Las fechas y horas del calendario también distinguen entre distintas zonas horarias y el horario de verano.

Para asegurarse de que se usa el formato de calendario preferido, puede usar los [controles estándar de calendario, fecha y hora](../controls-and-patterns/date-and-time.md). En escenarios más complejos, es posible que sea necesario trabajar directamente con las operaciones en fechas del calendario, **Windows. Globalization** proporciona una clase de [**calendario**](/uwp/api/windows.globalization.calendar?branch=live) que proporciona una representación de calendario adecuada para la referencia cultural, región y tipo de calendario especificados.

## <a name="format-phone-numbers-appropriately"></a>Da un formato adecuado a los números de teléfono

Los números de teléfono tienen un formato diferente según la región. El número de dígitos, cómo se agrupan los dígitos y la importancia de ciertas partes del número de teléfono varían entre países. A partir de Windows 10, versión 1607, puede usar las clases del espacio de nombres [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) para dar formato adecuado a los números de teléfono para la región actual.

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) analiza una cadena de dígitos y permite determinar si los dígitos son un número de teléfono válido en la región actual. compara dos números para determinar si son iguales; y para extraer las distintas partes funcionales del número de teléfono, como el código de país o el código de área geográfica.

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) da formato a una cadena de dígitos o un **PhoneNumberInfo** para su presentación, incluso cuando la cadena de dígitos representa un número de teléfono parcial. Puede usar este formato de número parcial para dar formato a un número cuando un usuario escribe el número.

En el ejemplo siguiente se muestra cómo usar **PhoneNumberFormatter** para dar formato a un número de teléfono a medida que se escribe. Cada vez que cambia el texto **en un cuadro de texto denominado** phoneNumberInputTextBox, se da formato al contenido del cuadro de texto mediante la región predeterminada actual y se muestra en un **TextBlock** denominado phoneNumberOutputTextBlock. Para fines de demostración, la cadena también tiene el formato que usa la región para Nueva Zelanda y se muestra en un TextBlock denominado phoneNumberOutputTextBlockNZ.
  
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

Para probar el código anterior en su propio equipo, cambie el país o región en **configuración**  >  **hora & región de idioma**  >  **&**  >  **país o región** de idioma. Elija un país o región (quizás Nueva Zelanda para confirmar que los formatos coinciden) y vuelva a ejecutar el código. En el caso de los datos de prueba, puede realizar una búsqueda web para el número de teléfono de una empresa en Nueva Zelanda.

## <a name="the-users-language-and-cultural-preferences"></a>El idioma y las preferencias culturales del usuario

En el caso de los escenarios en los que desea proporcionar una funcionalidad diferente basada únicamente en el idioma, la región o las preferencias culturales del usuario, Windows le ofrece una forma de acceder a estas preferencias, a través de [**Windows.SysTEM. UserProfile. GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live). Cuando sea necesario, usa la clase **GlobalizationPreferences** para conseguir el valor de la región geográfica actual del usuario, sus idiomas preferidos, monedas preferidas, etc. Pero recuerde que si las imágenes o cadenas de la aplicación no están localizadas para el idioma preferido del usuario, las fechas y horas y otros datos con formato para ese idioma preferido no coincidirán con las cadenas que se muestran.

## <a name="important-apis"></a>API importantes

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Calendario](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>Temas relacionados

* [Calendario, controles de fecha y hora](../controls-and-patterns/date-and-time.md)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md)
* [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>Ejemplos

* [Detalles del calendario y ejemplo matemático](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Calendar%20details%20and%20math%20sample%20(Windows%208))
* [Ejemplo de formato de fecha y hora](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Date%20and%20time%20formatting%20sample%20(Windows%208))
* [Ejemplo de preferencias de globalización](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Globalization%20preferences%20sample%20(Windows%208))
* [Ejemplo de análisis y formato de números](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Number%20formatting%20and%20parsing%20sample%20(Windows%208))
