---
author: stevewhims
Description: Learn about the benefits of globalizing and localizing your app, and exactly what these terms mean.
Search.SourceType: Video
title: Globalización y localización
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: b437055704c307dc3e3fa506a9885ff892585503
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038964"
---
# <a name="globalization-and-localization"></a>Globalización y localización

Windows se usa en todo el mundo, lo que incluye a personas con diferentes culturas, regiones e idiomas. Los usuarios hablan varios idiomas diferentes y en una variedad de países y regiones diferentes. Algunos usuarios hablan más de un idioma. Por lo tanto, tu aplicación se ejecuta en configuraciones que implican muchas permutaciones de configuración de sistema para idioma, región y cultura. Puedes aumentar el mercado potencial de la aplicación diseñándola para que sea fácilmente adaptable mediante la *globalización* y la *localización*.

Este vídeo proporciona una introducción breve sobre cómo preparar la aplicación para todo el mundo: [Introducción a la globalización y la localización](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

**Globalización** es el proceso de diseñar y desarrollar tu aplicación de manera que funciona correctamente en diferentes mercados globales (en sistemas con diferentes configuraciones de idioma y referencias culturales) sin necesidad de cambios específicos de referencia cultural ni personalización.

- Ten en cuenta la cultura al manipular cadenas. Por ejemplo, no cambies el formato de mayúscula/minúscula de las cadenas antes de compararlas.
- Usa calendarios que sean adecuados para la cultura actual.
- Usa API de globalización para mostrar los datos que tengan un formato adecuado para el país o la región, como valores numéricos, fechas, horas y monedas.
- Ten en cuenta que diferentes culturas tienen reglas diferentes para intercalar (ordenar) el texto y otros datos.

El código debe funcionar igual de bien en cualquiera de las referencias culturales que hayas determinado que admitirá tu aplicación. En teoría, tu código funcionará igual de bien en el contexto de *cualquier* cultura, región o idioma. La manera más eficaz de globalizar las funciones de tu aplicación es usar el concepto de culturas o configuraciones regionales. Una cultura o configuración regional es un conjunto de reglas y datos específicos de un determinado idioma y espacio geográfico. Estas reglas y datos incluyen información acerca de lo siguiente.

- Clasificación de caracteres
- Sistema de escritura
- Formato de fecha y hora
- Normas numéricas, de moneda, peso y medida
- Reglas de ordenación

**Localizabilidad** es el proceso de preparación de una aplicación globalizada para la localización y/o la comprobación de que la aplicación está lista para la localización. Hacer que una aplicación sea localizable significa que el proceso de localización siguiente no descubrirá ningún defecto funcionales en la aplicación. La propiedad más esencial de una aplicación localizable es que su código ejecutable se ha separado limpiamente de los recursos localizables de la aplicación.

- Las cadenas traducidas en diferentes idiomas pueden variar mucho en longitud. Por lo tanto, diseña la interfaz de usuario para que se adapte a distintas longitudes de texto y tamaños de fuente para etiquetas y controles de entrada de texto.
- Intenta evitar texto y/o material culturalmente sensible en las imágenes.
- No integres cadenas e imágenes que dependan de la cultura en el código y la revisión de la aplicación. En su lugar, almacénalas como recursos de imágenes y cadenas para que se puedan adaptar a diferentes mercados locales, independientemente de los binarios integrados de la aplicación.
- Pseudolocaliza tu aplicación para descubrir los problemas de localizabilidad.

**Localización** es el proceso de adaptar o traducir los recursos localizables de la aplicación a fin de cumplir con los requisitos de idioma, culturales y políticos de los mercados locales concretos con los que la aplicación va a ser compatible. Cuando llegas a la fase de localización de tu aplicación, si tu aplicación es localizable, no tendrás que modificar ninguna lógica durante este proceso.

- Traduce los recursos de cadena y otros activos de la aplicación para el nuevo mercado.
- Modifica las imágenes dependientes de la referencia cultural según sea necesario.
- Los archivos también pueden variar según la región de un usuario, independientemente del idioma. Por ejemplo, un mapa podría tener diferentes fronteras según la ubicación del usuario, pero las etiquetas deberían seguir el idioma preferido del usuario.

La mayoría de los equipos de localización usa herramientas especiales para facilitar el proceso. Por ejemplo, mediante el reciclaje de traducciones de texto periódico.

| Artículo | Descripción |
|---------|-------------|
| [Directrices sobre globalización](guidelines-and-checklist-for-globalizing-your-app.md) | Diseña y desarrolla tu aplicación de manera que funcione correctamente en sistemas con diferentes configuraciones de idioma y cultura. |
| [Comprender los idiomas del perfil del usuario y los idiomas del manifiesto de la aplicación](manage-language-and-region.md) | Este artículo define los términos "Lista de idiomas del perfil del usuario", "Lista de idiomas del manifiesto de la aplicación" y "Lista de idiomas del tiempo de ejecución de la aplicación". Utilizaremos estos términos en este y otros temas en esta área de funciones, por lo que es importante que sepas lo que significan. |
| [Globaliza los formatos de fecha, hora o número](use-global-ready-formats.md) | Diseña una aplicación que todo el mundo pueda usar empleando el formato adecuado en fechas, horas, números de teléfono y divisas. Así podrás adaptar la aplicación más adelante a otras culturas, regiones e idiomas del mercado global. |
| [Usa plantillas para dar formato a fechas y horas](use-patterns-to-format-dates-and-times.md) | Usa las clases en el espacio de nombres [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) con plantillas y patrones personalizados para mostrar fechas y horas en el formato exacto que quieras. |
| [Ajusta el diseño y las fuentes, y haz que sea compatible con RTL](adjust-layout-and-fonts--and-support-rtl.md) | Diseña tu aplicación para admitir los diseños y fuentes de varios idiomas, incluida la dirección de flujo de derecha a izquierda (RTL). |
| [Valores NumeralSystem](glob-numeralsystem-values.md) | En este tema se enumeran los valores disponibles en la propiedad **NumeralSystem** de diversas clases en el espacio de nombres [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live). |
| [Haz que tu aplicación sea localizable](prepare-your-app-for-localization.md) | Una aplicación localizada es aquella que puede localizarse a otros mercados, idiomas o regiones sin descubrir defectos funcionales en la aplicación. La propiedad más esencial de una aplicación localizable es que su código ejecutable se ha separado limpiamente de sus recursos localizables. |
| [Fuentes internacionales](loc-international-fonts.md) | En este tema se enumeran las fuentes disponibles para las aplicaciones para UWP que se localizan en otros idiomas además de inglés de EE. UU. |
| [Diseña tu aplicación para el texto bidireccional](design-for-bidi-text.md) | Diseña tu aplicación para proporcionar compatibilidad con texto bidireccional (BiDi) de manera que puedas combinar script desde sistemas de escritura de izquierda a derecha y de derecha a izquierda. |
| [Usa el Kit de herramientas para aplicaciones multilingües 4.0](use-mat.md) | El Kit de herramientas para aplicaciones multilingües (MAT) 4.0 se integra con Microsoft Visual Studio 2017 para proporcionar a las aplicaciones UWP compatibilidad con traducción, administración de archivos de traducción y herramientas de editor. |
| [Solución de problemas y preguntas más frecuentes sobre el Kit de herramientas para aplicaciones multilingües 4.0](mat-faq-troubleshooting.md) | Este tema proporciona respuestas a preguntas frecuentes y problemas relacionados con el Kit de herramientas para aplicaciones multilingües (MAT) 4.0. |
