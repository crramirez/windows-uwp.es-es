---
Description: Las características de diseño universal incluidas en la aplicación de UWP te ayudan a crear aplicaciones que se adaptan perfectamente a una gama de dispositivos.
title: Introducción al diseño de aplicaciones para la Plataforma universal de Windows (aplicaciones de Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.date: 05/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8568a4613b5bb62b98edc7edd302a42153e5e33b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "79209320"
---
# <a name="introduction-to-uwp-app-design"></a>Introducción al diseño de aplicaciones para UWP

![ejemplo de aplicación de iluminación](images/introUWP-header.jpg)

Las instrucciones de diseño para la Plataforma universal de Windows (UWP) son un recurso que te ayuda a diseñar y crear fantásticas aplicaciones.

No se trata de una lista de reglas prescriptivas, sino de un documento dinámico, diseñado para adaptarse a la evolución de nuestro [sistema Fluent Design](/windows/apps/fluent-design-system), así como a las necesidades de nuestra comunidad de creación de aplicaciones.

Esta introducción proporciona una descripción general de las características de diseño universal que se incluyen en cada aplicación para UWP, que te ayudan a crear interfaces de usuario (IU) que se adapten con elegancia a una amplia gama de dispositivos.

## <a name="effective-pixels-and-scaling"></a>Escalado y píxeles efectivos

Las aplicaciones para UWP se ejecutan en todos los [dispositivos Windows 10](../devices/index.md), desde la televisión hasta la tableta o el PC. ¿Cómo puedo diseñar una interfaz de usuario que se vea bien en una amplia variedad de dispositivos y tamaños de pantalla?

![misma aplicación en varios dispositivos](images/universal-image-1.jpg)

UWP te ayuda al ajustar automáticamente los elementos de la interfaz de usuario para que sean legibles y sea fácil interactuar con ellos en todos los dispositivos y tamaños de pantalla.

Cuando la aplicación se ejecuta en un dispositivo, el sistema usa un algoritmo para normalizar la manera en que los elementos de la interfaz de usuario se muestran en la pantalla. Este algoritmo de escalado tiene en cuenta la distancia de visualización y la densidad de la pantalla (píxeles por pulgada) para optimizar el tamaño percibido (en lugar del tamaño físico). El algoritmo de escalado garantiza que una fuente de 24 px en un dispositivo Surface Hub a 3 metros de distancia sea tan legible para el usuario como una fuente de 24 px en un teléfono de 5 pulgadas a unos centímetros de distancia.

![distancias de visualización para diferentes dispositivos](images/scaling-chart.png)

Debido al funcionamiento del sistema de escalado, al diseñar la aplicación para UWP, estás diseñando en píxeles efectivos, no en píxeles físicos reales. Los píxeles efectivos (epx) son una unidad de medida virtual y se utilizan para expresar las dimensiones y el espaciado del diseño, con independencia de la densidad de la pantalla. (En nuestras instrucciones, epx, ep y px se usan indistintamente).

Al diseñar, puedes ignorar la densidad de píxeles y la resolución de pantalla real. En su lugar, diseña la resolución efectiva (la resolución en píxeles efectivos) de una clase de tamaño (para obtener más información, consulta el [artículo Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Al crear bocetos de pantalla en los programas de edición de imágenes, establece el valor de PPP en 72 y las dimensiones de imagen en la resolución eficaz de la clase de tamaño que quieres obtener. Para obtener una lista de clases de tamaño y resoluciones efectivas, consulta el artículo [Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).

### <a name="multiples-of-four"></a>Múltiplos de cuatro

:::row:::
    :::column span:::
Los tamaños, los márgenes y las posiciones de los elementos de la interfaz de usuario siempre deberían estar en **múltiplos de 4 epx** en tus aplicaciones para UWP.

UWP se escala en una variedad de dispositivos con niveles predefinidos de escalado del 100 %, 125 %, 150 %, 175 %, 200 %, 225 %, 250 %, 300 %, 350 % y 400 %. La unidad base es 4 porque es el único entero que se puede escalar por números no enteros (por ejemplo, 4*1,5 = 6). Al usar múltiplos de cuatro, se alinean todos los elementos de la interfaz de usuario con píxeles enteros y se garantiza que los elementos de la interfaz de usuario tengan contornos afilados y nítidos. (Ten en cuenta que el texto no tiene este requisito; el texto puede tener cualquier tamaño y posición).
    :::column-end:::
    :::column:::
![cuadrícula](images/4epx.svg)
    :::column-end:::
:::row-end:::

## <a name="layout"></a>Diseño

Dado que las aplicaciones para UWP se adaptan automáticamente a todos los dispositivos, el diseño de una de estas aplicaciones para cualquier dispositivo sigue la misma estructura. Empecemos desde el comienzo de la interfaz de usuario de la aplicación para UWP.

### <a name="windows-frames-and-pages"></a>Ventanas, marcos y páginas

:::row:::
    :::column:::
Cuando se inicia una aplicación para UWP en cualquier dispositivo con Windows 10, se inicia en una [Ventana](/uwp/api/windows.ui.xaml.window) con un [Marco](/uwp/api/windows.ui.xaml.controls.frame), lo que permite navegar entre instancias de [Página](/uwp/api/windows.ui.xaml.controls.page).
    :::column-end:::
    :::column:::
![Frame](images/frame.svg)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
Puedes considerar la interfaz de usuario de tu aplicación como una colección de páginas. Depende de ti decidir qué debe ir en cada página y las relaciones entre las páginas.

Para obtener información sobre cómo puedes organizar las páginas, consulta [Conceptos básicos de navegación](navigation-basics.md).
    :::column-end:::
    :::column:::
![Frame](images/collection-pages.svg)
    :::column-end:::
:::row-end:::

### <a name="page-layout"></a>Diseño de página

¿Qué aspecto deberían tener esas páginas? Bien, la mayoría de las páginas siguen una estructura común para proporcionar coherencia, de forma que los usuarios puedan desplazarse fácilmente entre las páginas de la aplicación y dentro de ellas. Las páginas suelen contener tres tipos de elementos de interfaz de usuario:

- Los elementos de [navegación](navigation-basics.md) ayudan a los usuarios a elegir el contenido que quieren mostrar.
- Los elementos de [comandos](commanding-basics.md) inician acciones, como manipular, guardar o compartir contenido.
- Los elementos de [contenido](content-basics.md) muestran el contenido de la aplicación.

![Un patrón de diseño común](../layout/images/page-components.svg)

Para más información sobre cómo implementar patrones comunes de aplicaciones para UWP, consulta el artículo [Diseño de página](../layout/page-layout.md).

También puedes usar [Windows Template Studio](https://github.com/Microsoft/WindowsTemplateStudio/tree/master) en Visual Studio para empezar a trabajar con un diseño para la aplicación.

## <a name="controls"></a>Controles

La plataforma de diseño de UWP proporciona un conjunto de controles comunes cuyo funcionamiento está garantizado en todos los dispositivos de Windows y que cumplen los principios de nuestro [sistema Fluent Design](/windows/apps/fluent-design-system). Estos controles incluyen todo, desde controles simples, como botones y los elementos de texto, hasta controles sofisticados que pueden generar listas a partir de un conjunto de datos y una plantilla.

![controles UWP](../style/images/color/windows-controls.svg)

Para obtener una lista completa de los controles de UWP y los patrones que se pueden crear a partir de ellos, consulta la sección [Controles y patrones](../controls-and-patterns/index.md).

## <a name="style"></a>Estilo

Los controles comunes reflejan automáticamente el tema y el color de énfasis del sistema, el trabajo con todo tipo de entradas y la adaptación a todos los dispositivos. De esa manera, reflejan el sistema Fluent Design, en cuanto que son adaptables, comprensivos y atractivos. Los controles comunes usan luz, movimiento y profundidad en sus estilos predeterminados, con lo cual al usarlos, vas a incorporar nuestro sistema Fluent Design en tu aplicación.

Los controles comunes son enormemente personalizables: puedes cambiar el color de primer plano de un control o personalizar su apariencia completamente. Para invalidar los estilos predeterminados de los controles, usa [estilo ligero](../controls-and-patterns/xaml-styles.md#lightweight-styling) o crea [controles personalizados](../controls-and-patterns/control-templates.md) en XAML.

![Gif de color de énfasis](images/intro-style.gif)

## <a name="shell"></a>Shell

:::row:::
    :::column:::
Tu aplicación para UWP interactuará con la experiencia más amplia de Windows con iconos y notificaciones en el [Shell](../shell/tiles-and-notifications/creating-tiles.md) de Windows.

En el menú Inicio y cuando se inicia tu aplicación se muestran ventanas que proporcionan una idea de qué está sucediendo en la aplicación. Su potencial procede del contenido que incluyen y de la inteligencia y la capacidad con la que se ofrecen.

Las aplicaciones para UWP tienen cuatro tamaños de ventana (pequeño, mediano, ancho y grande) que se pueden personalizar con el icono y la identidad de la aplicación. Para obtener instrucciones sobre cómo diseñar ventanas para tu aplicación para UWP, consulta [Directrices de activos de ventanas e iconos](../shell/tiles-and-notifications/app-assets.md).
    :::column-end:::
    :::column:::
![ventanas en el menú Inicio](images/shell.svg)
    :::column-end:::
:::row-end:::

## <a name="inputs"></a>Entradas

:::row:::
    :::column:::
Las aplicaciones para UWP dependen de interacciones inteligentes. Puedes diseñar basándote en una interacción mediante clic sin tener que saber o definir si el clic procede de un mouse o un lápiz, o de la pulsación de un dedo. En cambio, también puedes diseñar tus aplicaciones para [modos de entrada específicos](../input/input-primer.md).
    :::column-end:::
    :::column:::
![entradas](images/inputs.svg)
    :::column-end:::
:::row-end:::

## <a name="devices"></a>Dispositivos

![dispositivos](../layout/images/size-classes.svg)

Del mismo modo, si bien UWP adapta automáticamente la aplicación a diferentes dispositivos, también puedes [optimizar tu aplicación para UWP para dispositivos específicos](../devices/index.md).

## <a name="usability"></a>Facilidad de uso

<img src="https://img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REYaAb?ver=727c">

Por último, aunque no menos importante, la usabilidad consiste en hacer que la experiencia de tu aplicación esté abierta a todos los usuarios. Todo el mundo pueden beneficiarse de una experiencia del usuario realmente inclusiva; consulta [Facilidad de uso para aplicaciones para UWP](../usability/index.md) para ver cómo hacer que tu aplicación sea fácil de usar por todos los usuarios.

Si estás diseñando para un público internacional, quizás quieras consultar [Globalización y localización](../globalizing/globalizing-portal.md).

También puedes pensar en las [características de accesibilidad](../accessibility/accessibility-overview.md) para usuarios con limitaciones visuales, auditivas y de movilidad. Si la accesibilidad se integra en el diseño desde el principio, [hacer que tu aplicación sea accesible](../accessibility/accessibility-in-the-store.md) no requiere mucho más tiempo y esfuerzo.

## <a name="tools-and-design-toolkits"></a>Herramientas y kits de herramientas de diseño

Ahora que conoces las características de diseño básicas, ¿qué tal si empiezas a diseñar tu aplicación para UWP?

Proporcionamos una serie de herramientas que te ayudarán en el proceso de diseño:

- Consulta nuestra [página de kits de herramientas de diseño](../downloads/index.md) para conocer los kits de herramientas XD, Illustrator, Photoshop, Framer y Sketch, así como para obtener descargas de herramientas de diseño y fuentes adicionales.

- Para lograr que el equipo esté configurado para escribir código para aplicaciones para UWP, consulta nuestro artículo [Introducción &gt; Preparación](../../get-started/get-set-up.md).

- Para obtener ideas sobre cómo implementar la interfaz de usuario para UWP, echa un vistazo a nuestras completas [aplicaciones para UWP de ejemplo](https://developer.microsoft.com/windows/samples).

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="next-fluent-design-system"></a>Siguiente: Fluent Design System

Si quieres obtener información sobre los principios que rigen Fluent Design (el sistema de diseño de Microsoft) y ver más características que puedes incorporar a tu aplicación para UWP, sigue con [Sistema Fluent Design](/windows/apps/fluent-design-system).

## <a name="related-articles"></a>Artículos relacionados

- [¿Qué es una aplicación para UWP?](../../get-started/universal-application-platform-guide.md)
- [Sistema Fluent Design](/windows/apps/fluent-design-system)
- [Información general sobre la plataforma XAML](../../xaml-platform/index.md)
