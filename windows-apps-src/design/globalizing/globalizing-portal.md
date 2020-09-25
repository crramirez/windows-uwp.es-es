---
Description: Obtenga información sobre las ventajas de globalizar y localizar la aplicación, y exactamente lo que significan estos términos.
Search.SourceType: Video
title: Globalización y localización
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 2b288a4d0ceaa97a4f6357d99f9937e21305bf3f
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218338"
---
# <a name="globalization-and-localization"></a>Globalización y localización

Windows se usa en todo el mundo por personas de diferentes culturas, regiones e idiomas. Los usuarios hablan varios idiomas diferentes y en una diversidad de países y regiones diferentes. Algunos usuarios hablan más de un idioma. Por lo tanto, la aplicación se ejecuta en configuraciones que implican muchas permutaciones de configuración del sistema para el idioma, la región y la referencia cultural. Puede aumentar el posible mercado de su aplicación al diseñarla para que se pueda adaptar fácilmente, mediante la *globalización* y la *localización*.

Este vídeo proporciona una breve introducción sobre cómo preparar su aplicación para el mundo: [Introducción a la globalización y la localización](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

La **globalización** es el proceso de diseño y desarrollo de la aplicación de forma que funcione adecuadamente en diferentes mercados globales (en sistemas con distintas configuraciones de idioma y referencia cultural) sin necesidad de cambios o personalización específicos de la referencia cultural.

- Tenga en cuenta la referencia cultural al manipular cadenas; por ejemplo, no cambie las mayúsculas y minúsculas de las cadenas antes de compararlas.
- Utilice los calendarios adecuados para la referencia cultural actual.
- Use las API de globalización para mostrar los datos con el formato adecuado para el país o la región, como números, fechas, horas y divisas.
- Tenga en cuenta que las distintas referencias culturales tienen diferentes reglas para la intercalación (ordenación) de texto y otros datos.

El código debe funcionar igual de bien en cualquiera de las referencias culturales que ha determinado que la aplicación admitirá. Idealmente, el código funcionará igual de bien en el contexto de *cualquier* idioma, región o referencia cultural. La manera más eficaz de globalizar las funciones de la aplicación es usar el concepto de referencias culturales o configuraciones regionales. Una referencia cultural o configuración regional es un conjunto de reglas y un conjunto de datos que son específicos de un idioma y un área geográfica determinados. Estas reglas y datos incluyen información sobre lo siguiente.

- Clasificación de caracteres
- Sistema de escritura
- Formato de fecha y hora
- Convenciones numéricas, de moneda, de peso y de medida
- Ordenar reglas

>[!NOTE]
> Para obtener una lista de los nombres de configuración regional admitidos por la versión del sistema operativo Windows, consulte la columna etiqueta de idioma de la tabla en el [apéndice a: comportamiento del producto](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c) en la [Referencia del identificador de idioma de Windows (LCID)](/openspecs/windows_protocols/ms-lcid/70feba9f-294e-491e-b6eb-56532684c37f).

La **localizabilidad** es el proceso de preparación de una aplicación globalizada para la localización o comprobación de que la aplicación está lista para la localización. Una aplicación que se pueda traducir correctamente significa que el proceso de localización posterior no detectará ningún defecto funcional en la aplicación. La propiedad más esencial de una aplicación localizable es que su código ejecutable se ha separado claramente de los recursos localizables de la aplicación.

- Las cadenas traducidas en idiomas diferentes pueden variar en gran medida. Por lo tanto, diseñe la interfaz de usuario para acomodar diferentes longitudes de texto y tamaños de fuente para etiquetas y controles de entrada de texto.
- Intente evitar el texto o material con distinción cultural en las imágenes.
- No codifique las cadenas y las imágenes dependientes de la referencia cultural en el código y el marcado de la aplicación. En su lugar, almacénelos como recursos de cadena e imagen para que se puedan adaptar a distintos mercados locales independientemente de los binarios compilados de la aplicación.
- Localizar la aplicación para revelar cualquier problema de localizabilidad.

La **localización** es el proceso de adaptar o traducir los recursos localizables de la aplicación para satisfacer los requisitos de idioma, culturales y políticos de los mercados locales específicos que la aplicación tiene previsto admitir. En el momento en que llegue a la fase de localización de la aplicación, si la aplicación es localizable, no tendrá que modificar ninguna lógica durante este proceso.

- Traduzca los recursos de cadena y otros activos de la aplicación para el nuevo mercado.
- Modifique las imágenes dependientes de la referencia cultural según sea necesario.
- Los archivos también pueden variar en función de la región de un usuario, aparte de su idioma. Por ejemplo, un mapa puede tener distintos bordes en función de la ubicación del usuario, pero las etiquetas deben seguir el idioma preferido del usuario.

La mayoría de los equipos de localización usan herramientas especiales para ayudar al proceso. Por ejemplo, al reciclar las traducciones del texto recurrente.

| Artículo | Descripción |
|---------|-------------|
| [Directrices sobre globalización](guidelines-and-checklist-for-globalizing-your-app.md) | Diseñe y desarrolle su aplicación de forma que funcione correctamente en sistemas con distintas configuraciones de idioma y referencia cultural. |
| [Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación](manage-language-and-region.md) | En este tema se definen los términos "lista de lenguajes de Perfil de usuario", "lista de idiomas del manifiesto de la aplicación" y "lista de idiomas del tiempo de ejecución de la aplicación". Usaremos estos términos en este tema y otros temas en esta área de características, por lo que es importante saber lo que significan. |
| [Globalizar los formatos de fecha, hora y número](use-global-ready-formats.md) | Diseñe la aplicación para que esté preparada para el uso global mediante el formato adecuado de fechas, horas, números, números de teléfono y monedas. Después podrá adaptar su aplicación para obtener más referencias culturales, regiones y idiomas en el mercado mundial. |
| [Usar plantillas y patrones para dar formato a fechas y horas](use-patterns-to-format-dates-and-times.md) | Use las clases en el espacio de nombres [**Windows. Globalization. DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) con plantillas y patrones personalizados para mostrar las fechas y horas exactamente en el formato que desee. |
| [Ajustar el diseño y las fuentes y admitir la escritura RTL](adjust-layout-and-fonts--and-support-rtl.md) | Diseñe la aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo RTL (derecha a izquierda). |
| [Valores NumeralSystem](glob-numeralsystem-values.md) | En este tema se enumeran los valores disponibles para la propiedad **NumeralSystem** de varias clases en el espacio de nombres [**Windows. Globalization**](/uwp/api/windows.globalization?branch=live) . |
| [Hacer que la aplicación sea localizable](prepare-your-app-for-localization.md) | Una aplicación localizada es aquella que se puede localizar en otros mercados, idiomas o regiones sin que se detecten defectos funcionales en la aplicación. La propiedad más esencial de una aplicación localizable es que su código ejecutable se ha separado limpiamente de sus recursos localizables. |
| [Fuentes internacionales](loc-international-fonts.md) | En este tema se enumeran las fuentes disponibles para aplicaciones de Windows que están localizadas en idiomas distintos del Inglés de EE. UU. |
| [Diseñar la aplicación para texto bidireccional](design-for-bidi-text.md) | Diseñe la aplicación para proporcionar compatibilidad de texto bidireccional (BiDi) para poder combinar el script de los sistemas de escritura de izquierda a derecha y de derecha a izquierda. |
| [Usar el Kit de herramientas para aplicaciones multilingües 4.0](use-mat.md) | El kit de herramientas de aplicaciones multilingües (PASPARTÚ) 4,0 se integra con Microsoft Visual Studio 2017 y versiones posteriores para proporcionar a las aplicaciones de Windows compatibilidad con traducción, administración de archivos de traducción y herramientas de edición. |
| [Preguntas más frecuentes sobre el kit de herramientas multilingües 4,0 & solución de problemas](mat-faq-troubleshooting.md) | En este tema se proporcionan respuestas a preguntas y problemas frecuentes relacionados con el kit de herramientas de aplicaciones multilingües (PASPARTÚ) 4,0. |
| [Usa la página de códigos UTF-8](use-utf8-code-page.md) | UTF-8 es la página de códigos universal para la internacionalización. |
| [Preparar la aplicación para el cambio de la era japonesa](japanese-era-change.md) | Obtén información sobre el cambio de la era japonesa de mayo de 2019 y cómo preparar la aplicación. |
