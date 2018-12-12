---
Description: The universal design features included in every UWP app help you build apps that scale beautifully across a range of devices.
title: Introducción al diseño de aplicaciones para la Plataforma universal de Windows (aplicaciones de Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 05/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0006d20c1db7da16b885e82fb3f066b081e27349
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8936188"
---
# <a name="introduction-to-uwp-app-design"></a>Introducción al diseño de aplicaciones para UWP

![muestra de aplicación de iluminación](images/introUWP-header.jpg)

Las instrucciones sobre diseño para la Plataforma universal de Windows (UWP) son un recurso que te ayuda a diseñar y crear fantásticas aplicaciones.

No se trata de una lista de reglas prescriptivas: es un documento vivo, diseñado para adaptarse a la evolución de nuestro [sistema Fluent Design](../fluent-design-system/index.md), así como a las necesidades de nuestra comunidad de creación de aplicaciones.

Esta introducción proporciona una descripción general de las características de diseño universales que se incluyen en cada aplicación para UWP, lo que le ayuda a crear interfaces de usuario (IU) que se adapten con elegancia a una amplia gama de dispositivos.

## <a name="effective-pixels-and-scaling"></a>Escalado y píxeles efectivos

Las aplicaciones para UWP se ejecutan en todos los [dispositivos Windows 10](../devices/index.md), desde la televisión a tu tableta o PC. ¿Cómo se puede diseñar una interfaz de usuario que se vea bien en una amplia variedad de dispositivos y tamaños de pantalla?

![misma aplicación en varios dispositivos](images/universal-image-1.jpg)

UWP ayuda ajustando automáticamente los elementos de la interfaz de usuario para que sean legibles y fáciles de interaccionar en todos los dispositivos y tamaños de pantalla.

Cuando la aplicación se ejecuta en un dispositivo, el sistema usa un algoritmo para normalizar la manera en que los elementos de la interfaz de usuario se muestran en la pantalla. Este algoritmo de escalado tiene en cuenta la distancia de visualización y la densidad de la pantalla (píxeles por pulgada) para optimizar el tamaño percibido (en lugar del tamaño físico). El algoritmo de escalado garantiza que una fuente de 24 px en un dispositivo Surface Hub a 3 metros de distancia sea tan legible para el usuario como una fuente de 24 px en un teléfono de 5 pulgadas a unos centímetros de distancia.

![distancias de visualización para diferentes dispositivos](images/scaling-chart.png)

Debido al funcionamiento del sistema de escalado, al diseñar la aplicación para UWP, estás diseñando en píxeles efectivos, no en píxeles físicos reales. Los píxeles efectivos (epx) son una unidad de medida virtual y se utilizan para expresar las dimensiones y el espaciado del diseño, con independencia de la densidad de la pantalla. (En nuestras instrucciones, epx, ep y px se usan indistintamente).

Al diseñar, puedes ignorar la densidad de píxeles y la resolución de pantalla real. En su lugar, diseña la resolución efectiva (la resolución en píxeles efectivos) de una clase de tamaño (para obtener más información, consulta el [artículo Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Al crear bocetos de pantalla en los programas de edición de imágenes, establece el valor de PPP en 72 y las dimensiones de imagen en la resolución eficaz de la clase de tamaño que quieres obtener. Para obtener una lista de las clases de tamaño y las resoluciones efectivas, consulta el [artículo Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

### <a name="multiples-of-four"></a>Múltiplos de cuatro

:::row:::
    :::column span:::
        The sizes, margins, and positions of UI elements should always be in **multiples of 4 epx** in your UWP apps.

        UWP scales across a range of devices with scaling plateaus of 100%, 125%, 150%, 175%, 200%, 225%, 250%, 300%, 350%, and 400%. The base unit is 4 because it's the only integer that can be scaled by non-whole numbers (e.g. 4*1.5 = 6). Using multiples of four aligns all UI elements with whole pixels and ensures UI elements have crisp, sharp edges. (Note that text doesn't have this requirement; text can have any size and position.)
    :::column-end:::
    :::column:::
        ![grid](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>Diseño

Dado que las aplicaciones para UWP automáticamente escalan en todos los dispositivos, el diseño de una aplicación para UWP para cualquier dispositivo sigue la misma estructura. Empecemos desde el comienzo de la interfaz de usuario de tu aplicación para UWP.

### <a name="windows-frames-and-pages"></a>Ventanas, marcos y páginas

:::row:::
    :::column:::
        When a UWP app is launched on any Windows 10 device, it launches in a [Window](/uwp/api/Windows.UI.Xaml.Controls.Window) with a [Frame](/uwp/api/Windows.UI.Xaml.Controls.Frame), which can navigate between [Page](/uwp/api/Windows.UI.Xaml.Controls.Page) instances.
    :::column-end:::
    :::column:::
        ![Frame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        You can think of your app's UI as a collection of pages. It's up to you to decide what should go on each page, and the relationships between pages.

        To learn how you can organize your pages, see [Navigation basics](navigation-basics.md).
    :::column-end:::
    :::column:::
        ![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>Diseño de página

¿Qué aspecto deberían tener esas páginas? Bien, la mayoría de las páginas siguen una estructura común para proporcionar coherencia, para que los usuarios puedan navegar fácilmente entre y dentro las páginas de tu aplicación. Las páginas suelen contener tres tipos de elementos de interfaz de usuario:

- Los elementos de [navegación](navigation-basics.md) ayudan a los usuarios a elegir el contenido que quieren mostrar.
- Los elementos de [comando](commanding-basics.md) inician acciones, como manipular, guardar o compartir contenido.
- Los elementos de [contenido](content-basics.md) muestran el contenido de la aplicación.

![Un patrón de diseño común](../layout/images/page-components.svg)

Para obtener más información sobre cómo implementar patrones comunes de la aplicación para UWP, consulta el artículo [Diseño de página](../layout/page-layout.md).

También puedes usar el [WindowsTemplateStudio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) en Visual Studio para empezar a trabajar con un diseño para la aplicación.

## <a name="controls"></a>Controles

La plataforma de diseño de UWP proporciona un conjunto de controles comunes cuyo funcionamiento está garantizado en todos los dispositivos de Windows y que cumplen nuestros principios de [Sistema Fluent Design](../fluent-design-system/index.md). Estos controles incluyen todo, desde controles simples, como botones y los elementos de texto, hasta controles sofisticados que pueden generar listas a partir de un conjunto de datos y una plantilla.

![controles UWP](../style/images/color/windows-controls.svg)

Para obtener una lista completa de los controles de la UWP y los patrones que se pueden crear a partir de ellos, consulta la sección [Controles y patrones](../controls-and-patterns/index.md).

## <a name="style"></a>Estilo

Los controles comunes automáticamente reflejan el tema y el color de énfasis del sistema, el trabajo con todo tipo de entradas y la escala para todos los dispositivos. De esa manera, reflejan el sistema Fluent Design - son adaptables, comprensivos y atractivos. Los controles comunes usan luz, movimiento y profundidad en sus estilos predeterminados, con lo cual al utilizarlos, vas a incorporar nuestro sistema Fluent Design en tu aplicación.

Los controles comunes son altamente personalizables: puedes cambiar el color de primer plano de un control o personalizar su apariencia completamente. Para invalidar los estilos predeterminados en los controles, usa [estilo ligero](../controls-and-patterns/xaml-styles.md#lightweight-styling) o crea [controles personalizados](../controls-and-patterns/control-templates.md) en XAML.

![Gif de color de énfasis](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
        Your UWP app will interact with the broader Windows experience with tiles and notifications in the Windows [Shell](../shell/tiles-and-notifications/creating-tiles.md).

        Tiles are displayed in the Start menu and when your app launches, and they provide a glimpse of what's going on in your app. Their power comes from the content behind them, and the intelligence and craft with which they're offered up.

        UWP apps have four tile sizes (small, medium, wide, and large) that can be customized with the app's icon and identity. For guidance on designing tiles for your UWP app, see [Guidelines for tile and icon assets](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
        ![tiles on start menu](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>Entradas

:::row:::
    :::column:::
        UWP apps rely on smart interactions. You can design around a click interaction without having to know or define whether the click comes from a mouse, a stylus, or a tap of a finger. However, you can also design your apps for [specific input modes](../input/input-primer.md).
    :::column-end:::
    :::column:::
        ![inputs](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>Dispositivos

![dispositivos](../layout/images/size-classes.svg)

Del mismo modo, si bien la UWP escala automáticamente tu aplicación para diferentes dispositivos, también puedes [optimizar tu aplicación para UWP para dispositivos específicos](../devices/index.md).

## <a name="usability"></a>Facilidad de uso

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

Por último, aunque no menos importante, la usabilidad consiste en hacer que la experiencia de tu aplicación esté abierta a todos los usuarios. Todo el mundo pueden beneficiarse de una experiencias del usuario realmente inclusiva; consulta [Facilidad de uso para aplicaciones para UWP](../usability/index.md) para ver cómo hacer que tu aplicación sea fácil de usar para todos los usuarios.

Si estás diseñando para un público internacional, quizás quieras consultar [Globalización y localización](../globalizing/globalizing-portal.md).

También puedes pensar en las [características de accesibilidad](../accessibility/accessibility-overview.md) para usuarios con limitaciones visuales, auditivas y de movilidad. Si la accesibilidad se integra en el diseño desde el principio, [hacer que tu aplicación sea accesible](../accessibility/accessibility-in-the-store.md) requiere muy poco más tiempo y esfuerzo.

## <a name="tools-and-design-toolkits"></a>Herramientas y kits de herramientas de diseño

Ahora que conoces las características de diseño básicas, ¿qué tal si empiezas a diseñar tu aplicación para UWP?

Proporcionamos una serie de herramientas que te ayudarán en el proceso de diseño:

- Consulta nuestra [página de kits de herramientas de diseño](../downloads/index.md) para conocer los kits de herramientas XD, Illustrator, Photoshop, Framer y Sketch, así como para obtener descargas de herramientas de diseño y fuentes adicionales.

- Para lograr que el equipo esté configurado para escribir código para aplicaciones para UWP, consulta nuestro artículo [Introducción &gt; Prepárate](../../get-started/get-set-up.md).

- Para obtener ideas sobre cómo implementar la interfaz de usuario para la UWP, echa un vistazo a nuestras [aplicaciones para UWP de muestra](https://developer.microsoft.com/windows/samples) completas.

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>A continuación: el sistema Fluent Design

Si quieres obtener información sobre los principios que rigen Fluent Design (el sistema de diseño de Microsoft) y ver más características que puedes incorporar a tu aplicación para UWP, sigue con [sistema Fluent Design](../fluent-design-system/index.md).

## <a name="related-articles"></a>Artículos relacionados

- [¿Qué es una aplicación para UWP?](../../get-started/universal-application-platform-guide.md)
- [Sistema FluentDesign](../fluent-design-system/index.md)
- [Información general sobre la plataforma XAML](../../xaml-platform/index.md)
