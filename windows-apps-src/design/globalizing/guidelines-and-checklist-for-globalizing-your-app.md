---
Description: Diseñe y desarrolle su aplicación de forma que funcione correctamente en sistemas con distintas configuraciones de idioma y referencia cultural.
Search.Refinement.TopicID: 180
title: Directrices sobre globalización
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.date: 11/02/2017
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: d71cf2289654860b47aef18c117ac9d6d36fab0a
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493250"
---
# <a name="guidelines-for-globalization"></a>Directrices sobre globalización

Diseñe y desarrolle su aplicación de forma que funcione correctamente en sistemas con distintas configuraciones de idioma y referencia cultural. Usar las API de [**globalización**](/uwp/api/Windows.Globalization?branch=live) para dar formato a los datos; y evite las suposiciones en el código sobre el idioma, la región, la clasificación de caracteres, el sistema de escritura, el formato de fecha y hora, los números, las monedas, las ponderaciones y las reglas de ordenación.

| Recomendación | Descripción |
| ------------- | ----------- |
| Tenga en cuenta la referencia cultural al manipular y comparar cadenas. | Por ejemplo, no cambie las mayúsculas y minúsculas de las cadenas antes de compararlas. Vea [recomendaciones para el uso de cadenas](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| Al realizar la intercalación (ordenación) de cadenas y otros datos, no suponga que siempre se realiza alfabéticamente. | En el caso de los idiomas que no usan el alfabeto latino, la intercalación se basa en factores como la pronunciación o el número de trazos del lápiz. Incluso los lenguajes que usan el alfabeto latino no siempre utilizan la ordenación alfabética. Por ejemplo, en algunas culturas, una libreta de teléfonos probablemente no se ordene alfabéticamente. Windows puede controlar la ordenación, pero si crea su propio algoritmo de ordenación, asegúrese de tener en cuenta los métodos de ordenación usados en los mercados de destino. |
| Dé formato a los números, las fechas, las horas, las direcciones y los números de teléfono adecuadamente. | Estos formatos varían entre las referencias culturales, las regiones, los idiomas y los mercados. Si va a mostrar estos datos, use las API de [**globalización**](/uwp/api/Windows.Globalization?branch=live) para obtener el formato adecuado para una audiencia determinada. Consulte [globalizar los formatos de fecha, hora y número](use-global-ready-formats.md). El orden en que se muestran la familia y los nombres proporcionados, y el formato de las direcciones, también pueden ser diferentes. Use las pantallas estándar de fecha, hora y número. Usar controles de selector de fecha y hora estándar. Utilice la información de dirección estándar. |
| Admitir unidades internacionales de medida y moneda. | Se usan diferentes unidades y escalas en diferentes países, aunque las más populares son el sistema métrico y el sistema imperial. Asegúrese de que admite la medida correcta del sistema si se ocupa de medidas como la longitud, la temperatura y el área. Use la propiedad [**GeographicRegion. CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) para obtener el conjunto de divisas en uso en una región. |
| Usa Unicode para la codificación de caracteres. | De forma predeterminada, Microsoft Visual Studio usa la codificación de caracteres Unicode para todos los documentos. Si estás usando un editor diferente, asegúrate de guardar los archivos de origen con las codificaciones de caracteres Unicode apropiadas. Todas las API de Windows en tiempo de ejecución ejecutan cadenas codificadas mediante UTF-16. |
| Admite tamaños de papel internacionales. | Los tamaños de papel más comunes difieren entre países, por lo que si incluye características que dependen del tamaño de papel, como la impresión, asegúrese de admitir y probar tamaños internacionales comunes. |
| Grabe el idioma del teclado o del IME. | Cuando la aplicación pide al usuario una entrada de texto, grabe la etiqueta de idioma para la distribución de teclado o el editor de métodos de entrada (IME) que esté habilitado. Esto garantiza que cuando se muestre la entrada más adelante, se presente al usuario con el formato apropiado. Use la propiedad [**Language. CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) para obtener el idioma de entrada actual. |
| No use el lenguaje para asumir la región de un usuario. y no utilice region para asumir el idioma de un usuario. | El idioma y la región son conceptos independientes. Un usuario puede hablar de una variante regional determinada de un idioma, como en-GB para inglés, tal como se habla en el Reino Unido, pero el usuario podría estar en un país o región completamente diferente. Tenga en cuenta si la aplicación requiere conocimiento sobre el idioma del usuario (por ejemplo, para el texto de la interfaz de usuario) o región (para las licencias, por ejemplo). Para obtener más información, consulte Descripción de los lenguajes de [Perfil de usuario y los lenguajes de manifiesto de aplicación](manage-language-and-region.md). |
| Las reglas para comparar etiquetas de idioma son no triviales. | Las [etiquetas de lenguaje BCP-47](https://tools.ietf.org/html/bcp47) son complejas. Existen algunos problemas al comparar etiquetas de idioma, como problemas con la coincidencia de información del script, etiquetas heredadas y múltiples variedades regionales. El sistema de administración de recursos en Windows se encarga de la búsqueda de coincidencias. Puedes especificar un conjunto de recursos en cualquier idioma, y el sistema elige el apropiado para el usuario y la aplicación. Vea [recursos de la aplicación y el sistema de administración de recursos](../../app-resources/index.md) y [cómo el sistema de administración de recursos coincide con las etiquetas de idioma](../../app-resources/how-rms-matches-lang-tags.md). |
| Diseñe la interfaz de usuario para acomodar diferentes longitudes de texto y tamaños de fuente para etiquetas y controles de entrada de texto. | Las cadenas traducidas en lenguajes diferentes pueden variar considerablemente de longitud, por lo que necesitará los controles de interfaz de usuario para ajustar el tamaño de forma dinámica a su contenido. Los caracteres comunes en otros idiomas incluyen marcas por encima o por debajo de lo que se usa normalmente en inglés (como Å o Ņ). Use los tamaños de fuente y el alto de línea estándar para proporcionar el espacio vertical adecuado. Tenga en cuenta que las fuentes para otros idiomas pueden requerir tamaños de fuente mínimos más grandes para ser legibles. Vea las clases en el espacio de nombres [Windows. Globalization. Fonts](/uwp/api/windows.globalization.fonts?branch=live) . |
| Admite la creación de reflejo del orden de lectura. | La alineación del texto y el orden de lectura pueden ser de izquierda a derecha (como en inglés, por ejemplo) o de derecha a izquierda (RTL) (como en árabe o hebreo, por ejemplo). Si localiza su producto en idiomas que usan un orden de lectura diferente del suyo, asegúrese de que el diseño de los elementos de la interfaz de usuario admita la creación de reflejo. Incluso pueden ser necesarios reflejos de elementos como los botones atrás, los efectos de transición de la interfaz de usuario y las imágenes. Para obtener más información, consulte [ajustar el diseño y las fuentes, y compatibilidad con RTL](adjust-layout-and-fonts--and-support-rtl.md). |
| Muestra el texto y las fuentes de forma correcta. | La dirección del texto, el tamaño de fuente y la fuente ideales varían de un mercado a otro. Para obtener más información, consulte [**ajustar el diseño y las fuentes, y compatibilidad con**](adjust-layout-and-fonts--and-support-rtl.md) las fuentes RTL y [internacional](loc-international-fonts.md). |

## <a name="important-apis"></a>API importantes
 
* [Globalización](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language. CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>Temas relacionados

* [Recomendaciones para el uso de cadenas](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Globalizar los formatos de fecha, hora y número](use-global-ready-formats.md)
* [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md)
* [Etiquetas de lenguaje BCP-47](https://tools.ietf.org/html/bcp47)
* [Recursos de aplicación y el sistema de administración de recursos](../../app-resources/index.md)
* [Cómo el sistema de administración de recursos compara etiquetas de idioma](../../app-resources/how-rms-matches-lang-tags.md)
* [Ajustar el diseño y las fuentes y admitir la escritura RTL](adjust-layout-and-fonts--and-support-rtl.md)
* [Fuentes internacionales](loc-international-fonts.md)
* [Hacer que la aplicación sea localizable](prepare-your-app-for-localization.md)

## <a name="samples"></a>Ejemplos

* [Ejemplo de preferencias de globalización](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Globalization%20preferences%20sample%20(Windows%208))
