---
author: stevewhims
Description: Design and develop your app in such a way that it functions appropriately on systems with different language and culture configurations.
Search.Refinement.TopicID: 180
title: Directrices sobre globalización
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
template: detail.hbs
ms.author: stwhi
ms.date: 11/02/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 177332515db26eca7cef7a7be75c5752a239a8f1
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703590"
---
# <a name="guidelines-for-globalization"></a>Directrices sobre globalización

Diseña y desarrolla tu aplicación de manera que funcione correctamente en sistemas con diferentes configuraciones de idioma y cultura. Usa API de [**Globalización**](/uwp/api/Windows.Globalization?branch=live) para formatear datos; evita suposiciones en tu código sobre el idioma, la región, la clasificación de caracteres, el sistema de escritura, el formato de fecha y hora, los números, la moneda, los pesos y las normas de ordenación.

| Recomendación | Descripción |
| ------------- | ----------- |
| Ten en cuenta la cultura al manipular y comparar las cadenas. | Por ejemplo, no cambies el formato mayúscula/minúscula de las cadenas antes de compararlas. Consulta [Recomendaciones para el uso de cadenas](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage). |
| Al intercalar (ordenar) cadenas y otros datos, no asumas que siempre se hace alfabéticamente. | Para los idiomas que no usan un alfabeto latino, la ordenación se basa en factores como la pronunciación o el número de trazos al escribir. Incluso los idiomas que usan el alfabeto latino no siempre usan la ordenación alfabética. Por ejemplo, en algunas culturas, una guía telefónica probablemente no se ordene alfabéticamente. Windows puede controlar la ordenación por ti, pero si creas tu propio algoritmo de ordenación, asegúrate de tener en cuenta los métodos de ordenación usados en las regiones de comercialización de destino. |
| Usa los formatos correctos para números, fechas, horas, direcciones y números de teléfono. | Estos formatos varían entre culturas, regiones, idiomas y mercados. Si estás mostrando estos datos, usa las API de [**Globalización**](/uwp/api/Windows.Globalization?branch=live) para obtener el formato adecuado para un público determinado. Consulta [Globaliza los formatos de fecha, hora o número](use-global-ready-formats.md). El orden y el formato en el que aparecen los nombres, los apellidos y las direcciones también puede ser diferente. Usa presentaciones estándar para fecha, hora y número. Usa controles estándar de selector de hora y fecha. Usa información de dirección estándar. |
| Haz que sea compatible con unidades de medida y monedas internacionales. | Se usan diferentes unidades y escalas en diferentes países, aunque las más populares son el sistema métrico y el sistema imperial. Asegúrate de la compatibilidad con el sistema de medida correcto si trabajas con medidas, como longitud, temperatura o área. Usa la propiedad [**GeographicRegion.CurrenciesInUse**](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse) para fijar las divisas en uso en una región. |
| Usa Unicode para la codificación de caracteres. | De manera predeterminada, Microsoft Visual Studio usa la codificación de caracteres Unicode para todos los documentos. Si estás usando un editor diferente, asegúrate de guardar los archivos de origen con las codificaciones de caracteres Unicode apropiadas. Todas las API de UWP devuelven cadenas codificadas mediante UTF-16. |
| Admite tamaños de papel internacionales. | El tamaño de papel más común difiere de un país a otro. Por ello, si incluyes funciones que dependen del tamaño de papel, como la impresión, asegúrate de admitir y probar tamaños internacionales comunes. |
| Registra el idioma del teclado o el IME. | Cuando tu aplicación le pide al usuario una entrada de texto, registro la etiqueta de idioma para la distribución de teclado habilitadas actualmente o el Editor de métodos de entrada (IME). Esto garantiza que cuando se muestre la entrada más adelante, se presente al usuario con el formato apropiado. Usa la propiedad [**Language.CurrentInputMethodLanguageTag**](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag) para obtener el idioma de entrada actual. |
| No te guíes por el idioma para deducir la región de un usuario, y no uses la región para deducir su idioma. | El idioma y la región son conceptos independientes. Un usuario puede hablar una variedad regional de un idioma, como el en-GB para el inglés del Reino Unido, pero puede estar en una región o país totalmente diferente. Considera si las aplicaciones necesitan obtener información sobre idioma del usuario (por ejemplo, para el texto de la interfaz de usuario) o sobre su región (por ejemplo, para la concesión de licencias). Para obtener más información, consulta [Comprender los idiomas del perfil del usuario y los idiomas del manifiesto de la aplicación](manage-language-and-region.md). |
| Las reglas para comparar etiquetas de idioma son no triviales. | Las [etiquetas de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) son complejas. Existen algunos problemas al comparar etiquetas de idioma, como problemas con la coincidencia de información del script, etiquetas heredadas y múltiples variedades regionales. El Sistema de administración de recursos de Windows se encarga de las coincidencias por ti. Puedes especificar un conjunto de recursos en cualquier idioma, y el sistema elige el apropiado para el usuario y la aplicación. Consulta [Recursos de aplicaciones y sistema de administración de recursos](../../app-resources/index.md) y [Cómo compara etiquetas de idioma el sistema de administración de recursos](../../app-resources/how-rms-matches-lang-tags.md). |
| Diseña la interfaz de usuario para que se adapte a distintas longitudes de texto y tamaños de fuente para etiquetas y controles de entrada de texto. | Las cadenas traducidas en diferentes idiomas pueden variar mucho en longitud, por lo que los controles de interfaz de usuario tendrán que adaptar su tamaño al contenido de manera dinámica. Los caracteres comunes en otros idiomas incluyen marcas por encima o debajo de lo que se usa normalmente en inglés (como Å o Ņ). Usa los valores estándar de tamaños de fuente y alturas de línea para ofrecer el espacio vertical adecuado. Ten en cuenta que las fuentes para otros idiomas pueden requerir que los tamaños de fuente mínimos se puedan leer. Consulta las clases en el espacio de nombres [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live). |
| Admite la creación de reflejo del orden de lectura. | La alineación de texto y el orden de lectura pueden ser de izquierda a derecha (por ejemplo, como en inglés) o de derecha a izquierda (como en árabe o hebreo). Si estás localizando tu producto a idiomas que usan un orden de lectura diferente del tuyo, asegúrate de que el diseño de los elementos de tu interfaz de usuario admita la creación de reflejos. Es posible que incluso los elementos como los botones de retroceso, los efectos de transición de interfaz de usuario y las imágenes deban reflejarse. Para obtener más información, consulta [Ajustar el diseño y las fuentes, y admitir la escritura RTL](adjust-layout-and-fonts--and-support-rtl.md). |
| Muestra el texto y las fuentes de forma correcta. | Las opciones ideales de dirección de texto, tamaño de fuente y fuente varían de un mercado a otro. Para obtener más información, consulta [**Ajustar el diseño y las fuentes y admitir la escritura RTL**](adjust-layout-and-fonts--and-support-rtl.md) y [Fuentes internacionales](loc-international-fonts.md). |

## <a name="important-apis"></a>API importantes
 
* [Globalización](/uwp/api/Windows.Globalization?branch=live)
* [GeographicRegion.CurrenciesInUse](/uwp/api/windows.globalization.geographicregion.CurrenciesInUse)
* [Language.CurrentInputMethodLanguageTag](/uwp/api/windows.globalization.language.CurrentInputMethodLanguageTag)
* [Windows.Globalization.Fonts](/uwp/api/windows.globalization.fonts?branch=live)

## <a name="related-topics"></a>Artículos relacionados

* [Recomendaciones para el uso de cadenas](/dotnet/standard/base-types/best-practices-strings?branch=live#recommendations_for_string_usage)
* [Globaliza los formatos de fecha, hora o número](use-global-ready-formats.md)
* [Comprende los idiomas del perfil de usuario y los idiomas del manifiesto de la aplicación](manage-language-and-region.md)
* [Etiquetas de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Recursos de aplicaciones y sistema de administración de recursos](../../app-resources/index.md)
* [Cómo compara el sistema de administración de recursos las etiquetas de idioma](../../app-resources/how-rms-matches-lang-tags.md)
* [Ajusta el diseño y las fuentes, y admite la escritura de derecha a izquierda](adjust-layout-and-fonts--and-support-rtl.md)
* [Fuentes internacionales](loc-international-fonts.md)
* [Haz que tu aplicación sea localizable](prepare-your-app-for-localization.md)

## <a name="samples"></a>Ejemplos

* [Ejemplo de preferencias de globalización](http://go.microsoft.com/fwlink/p/?linkid=231608)
