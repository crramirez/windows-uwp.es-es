---
author: serenaz
Description: An overview of the universal design features that are included in every UWP app to help you build apps that scale beautifully across a range of devices.
title: Introducción al diseño de aplicaciones para la Plataforma universal de Windows (aplicaciones de Windows)
ms.assetid: 50A5605E-3A91-41DB-800A-9180717C1E86
ms.author: sezhen
ms.date: 12/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d0527866777c7b5dbbc10697bb313d664f4555fa
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2018
ms.locfileid: "1817283"
---
#  <a name="introduction-to-uwp-app-design"></a>Introducción al diseño de aplicaciones para UWP

Las instrucciones sobre diseño para la Plataforma universal de Windows (UWP) son un recurso que te ayudan a diseñar y crear fantásticas aplicaciones.

No se trata de una lista de reglas prescriptivas: es un documento vivo, diseñado para adaptarse a la evolución de nuestro [sistema Fluent Design](../fluent-design-system/index.md), así como a las necesidades de nuestra comunidad de creación de aplicaciones.

Esta introducción proporciona una descripción general de las características de diseño universales que se incluyen en cada aplicación para UWP, lo que le ayuda a crear interfaces de usuario (IU) que se adapten con elegancia a una amplia gama de dispositivos.

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Designing-Universal-Windows-Platform-apps/player]

## <a name="effective-pixels-and-scaling"></a>Escalado y píxeles efectivos

Las aplicaciones para UWP ajustan automáticamente el tamaño de los controles, las fuentes y otros elementos de la interfaz de usuario para que sean legibles y fáciles de interaccionar con ellos en [todos los dispositivos que admitan aplicaciones para UWP](../devices/index.md).

Cuando la aplicación se ejecuta en un dispositivo, el sistema usa un algoritmo para normalizar la manera en que los elementos de la interfaz de usuario se muestran en la pantalla. Este algoritmo de escalado tiene en cuenta la distancia de visualización y la densidad de la pantalla (píxeles por pulgada) para optimizar el tamaño percibido (en lugar del tamaño físico). El algoritmo de escalado garantiza que una fuente de 24 px en un dispositivo Surface Hub a 3 metros de distancia sea tan legible para el usuario como una fuente de 24 px en un teléfono de 5 pulgadas a unos centímetros de distancia.

![distancias de visualización para diferentes dispositivos](images/1910808-hig-uap-toolkit-03.png)

Debido al funcionamiento del sistema de escalado, al diseñar la aplicación para UWP, estás diseñando en píxeles efectivos, no en píxeles físicos reales. Los píxeles efectivos (epx) son una unidad de medida virtual y se utilizan para expresar las dimensiones y el espaciado del diseño, con independencia de la densidad de la pantalla. (En nuestras instrucciones, epx, ep y px se usan indistintamente).

Al diseñar, puedes ignorar la densidad de píxeles y la resolución de pantalla real. En su lugar, diseña la resolución efectiva (la resolución en píxeles efectivos) de una clase de tamaño (para obtener más información, consulta el [artículo Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)).

> [!TIP]
> Al crear bocetos de pantalla en los programas de edición de imágenes, establece el valor de PPP en 72 y las dimensiones de imagen en la resolución eficaz de la clase de tamaño que quieres obtener. Para obtener una lista de las clases de tamaño y las resoluciones efectivas, consulta el [artículo Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md).


## <a name="layout"></a>Diseño

### <a name="margins-spacing-and-positioning"></a>Márgenes, espaciado y posicionamiento 
![cuadrícula](images/4epx.png)

Cuando el sistema escala la interfaz de usuario de la aplicación, lo hace en múltiplos de 4.

Como resultado, los tamaños, los márgenes y las posiciones de los **elementos de la interfaz de usuario siempre deberían estar en múltiplos de 4 epx**. Esto produce la mejor representación, al alinearse con píxeles completos. También garantiza que los elementos de la interfaz de usuario tengan bordes nítidos. 

Ten en cuenta que el texto no tiene este requisito; el texto puede tener cualquier tamaño y posición. Para obtener instrucciones sobre cómo alinear el texto con otros elementos de la interfaz de usuario, consulta la [Guía de tipografía para UWP](../style/typography.md)

![escalado en la cuadrícula](images/epx-4pixelgood.png)

### <a name="layout-patterns"></a>Patrones de diseño

![Un patrón de diseño común](images/page-components.png)

La interfaz de usuario está compuesta de tres tipos de elementos: 
1. Los **elementos de navegación** ayudan a los usuarios a elegir el contenido que quieren mostrar. Consulta [Conceptos básicos de navegación](navigation-basics.md).
2. Los **elementos de comandos** inician acciones, como manipular, guardar o compartir contenido. Consulta [Conceptos básicos de comandos](commanding-basics.md).
3. Los **elementos de contenido** muestran el contenido de la aplicación. Consulta [Conceptos básicos de contenido](content-basics.md).

### <a name="adaptive-behavior"></a>Comportamiento adaptable
![comportamiento adaptable para el teléfono y el escritorio](../controls-and-patterns/images/patterns_masterdetail.png)

Aunque la interfaz de usuario de la aplicación será legibles y se podrá usar en todos los dispositivos Windows, es posible que quieras personalizarla para dispositivos y tamaños de pantalla específicos. Para obtener instrucciones más específicas, consulta [Tamaños de pantalla y puntos de interrupción](../layout/screen-sizes-and-breakpoints-for-responsive-design.md) y [Técnicas de diseño con capacidad de respuesta](../layout/responsive-design.md).

## <a name="type"></a>Tipo

De manera predeterminada, las aplicaciones para UWP usan la fuente **Segoe UI**, y la rampa de tipografías UWP incluye siete clases de tipografía, buscando el enfoque más eficaz para todos los tamaños de pantalla. 

![Rampa de tipografías](images/type-ramp.png)

Para obtener más información sobre la rampa de tipografías, consulta la [Guía de tipografía para UWP](../style/typography.md). Para aprender a utilizar los diferentes niveles de la rampa de tipografías UWP en tu aplicación, consulta [Recursos de temas](../controls-and-patterns/xaml-theme-resources.md#the-xaml-type-ramp).

## <a name="color"></a>Color

El color proporciona una forma intuitiva de comunicar información a los usuarios. Puede usarse para indicar interactividad, proporciona comentarios respecto a las acciones del usuario, transmitir información sobre el estado y conferir a la interfaz una sensación de continuidad visual. 

Windows10 proporciona una paleta de colores universal compartida de 48 colores, que se aplica en el shell y en las aplicaciones para UWP. 

![paleta de colores universales de Windows](images/colors.png)

El sistema aplica color automáticamente a tu aplicación para UWP con el color de énfasis del sistema. Cuando los usuarios eligen un color de énfasis de la paleta de colores en su configuración, el color aparece como parte del tema del sistema. En función las preferencias del usuario, el color de énfasis del sistema también puede mostrarse en Inicio, los iconos de la pantalla Inicio, la barra de tareas y las barras de título. 

![seleccionar el color de énfasis en la configuración](images/selectcolor.png)

En su aplicación para UWP, los hipervínculos y los estados seleccionados en los controles comunes reflejarán el color de énfasis del sistema.

![color de énfasis del sistema en los controles](images/accentcolor.png)

Las aplicaciones para UWP pueden impedir que el color de énfasis del sistema se muestre en los controles mediante el uso de [estilos ligeros](../controls-and-patterns/xaml-styles.md#lightweight-styling) o la creación de [controles personalizados](../controls-and-patterns/control-templates.md).

Para obtener instrucciones adicionales sobre el uso del color en tu aplicación para UWP, consulta el artículo [Color](../style/color.md).

### <a name="themes"></a>Temas

Los usuarios pueden elegir entre un tema claro, oscuro o de contraste alto. Puedes elegir entre modificar la apariencia de la aplicación en función de la preferencia de tema del usuario o no participar.

El tema claro es ideal para las tareas de productividad y la lectura.

El tema oscuro permite un contraste más visible para las aplicaciones centradas en el contenido multimedia y los escenarios que incluyen bastantes vídeos o imágenes. En estos escenarios, la tarea principal es más probable que sea ver películas que leer y que se produzca en condiciones ambientales de baja iluminación.

![Vista de la calculadora en los temas claro y oscuro](images/light-dark.png)

El tema de contraste alto usa una pequeña paleta de colores que contrastan entre sí, lo que facilita la visualización de la interfaz.
![La calculadora con el tema claro y el tema Negro en contraste alto.](../accessibility/images/high-contrast-calculators.png)


Para obtener más información sobre el uso de los temas y la rampa de colores UWP en la aplicación, consulta [Recursos de temas](../controls-and-patterns/xaml-theme-resources.md).

## <a name="icons"></a>Iconos
![iconos](images/icons.png)

Los iconos son un tipo de lenguaje visual que se han convertido en parte cotidiana de nuestras vidas. Nos permiten expresar conceptos y acciones de una forma visualmente atractiva y ahorrar espacio en pantalla, y actúan como una forma de navegar por nuestra vida digital. 

Todas las aplicaciones para UWP tienen acceso a los iconos de la [fuente Segoe MDL2](../style/segoe-ui-symbol-font.md). Estos iconos se basan en formas establecidas que resultan familiares y fáciles de identificar para todos los usuarios, pero también se han modernizado para que parezca que los ha dibujado una sola mano.

Si quieres crear tus propios iconos, consulta [Iconos de aplicaciones para UWP](../style/icons.md).

## <a name="tiles"></a>Ventanas
![ventanas en el menú Inicio](images/tiles.png)

En el menú Inicio se muestran ventanas que proporcionan una idea de qué está sucediendo en tu aplicación. Su potencial procede del contenido que incluyen y de la inteligencia y la capacidad con la que se ofrecen. 

Las aplicaciones para UWP tienen cuatro tamaños de ventana (pequeño, mediano, ancho y grande) que se pueden personalizar con el icono y la identidad de la aplicación. Para obtener instrucciones sobre cómo diseñar ventanas para tu aplicación para UWP, consulta [Directrices de activos de ventanas e iconos](../shell/tiles-and-notifications/app-assets.md).

## <a name="controls"></a>Controles
![control de botón](images/1910808-hig-uap-toolkit-01.png)

La UWP proporciona un conjunto de controles comunes que se garantiza que funcionan bien en todos los dispositivos Windows. Estos controles incluyen todo, desde controles simples, como botones y los elementos de texto, hasta controles sofisticados que pueden generar listas a partir de un conjunto de datos y una plantilla.

La UWP controla automáticamente la reelección del tema y el color de énfasis del sistema, el trabajo con todo tipo de entradas y la escala para todos los dispositivos. Y también son altamente personalizables: puedes cambiar el color de primer plano de un control o personalizar su apariencia completamente. 

Para obtener una lista completa de los controles de la UWP y los patrones que se pueden crear a partir de ellos, consulta la sección [Controles y patrones](../controls-and-patterns/index.md).

## <a name="input"></a>Entrada
![entradas](../input/images/input-interactions/icons-inputdevices03.png)

Las aplicaciones para UWP dependen de interacciones inteligentes. Puedes diseñar basándote en una interacción mediante clic sin tener que saber o definir si el clic procede de un mouse o un lápiz, o de la pulsación de un dedo. Sin embargo, también puedes diseñar tus aplicaciones para [dispositivos y modos de entrada específicos](../input/input-primer.md).

## <a name="accessibility"></a>Accesibilidad
![personas de diseño inclusivo](images/inclusive.png)

Por último, aunque no menos importante, la accesibilidad consiste en hacer que la experiencia de tu aplicación esté abierta a todos los usuarios. Que sea relevante para todo el mundo, no solo para las personas con discapacidades. Todo el mundo pueden beneficiarse de una experiencias del usuario realmente inclusiva; consulta [Facilidad de uso para aplicaciones para UWP](../usability/index.md) para ver cómo hacer que tu aplicación sea fácil de usar para todos los usuarios. También puedes pensar en las [características de accesibilidad](../accessibility/accessibility-overview.md) para usuarios con limitaciones visuales, auditivas y de movilidad. 

Si la accesibilidad se integra en el diseño desde el principio, [hacer que tu aplicación sea accesible](../accessibility/accessibility-in-the-store.md) requiere muy poco más tiempo y esfuerzo.

## <a name="tools-and-design-toolkits"></a>Herramientas y kits de herramientas de diseño
Ahora que conoces las características de diseño básicas, ¿qué tal si empiezas a diseñar tu aplicación para UWP?

Proporcionamos una serie de herramientas que te ayudarán en el proceso de diseño:

* Consulta nuestra [página de kits de herramientas de diseño](../downloads/index.md) para conocer los kits de herramientas XD, Illustrator, Photoshop, Framer y Sketch, así como para obtener descargas de herramientas de diseño y fuentes adicionales. 

* Para lograr que el equipo esté configurado para escribir código para aplicaciones para UWP, consulta nuestro artículo [Introducción &gt; Prepárate](../../get-started/get-set-up.md). 

* Para obtener ideas sobre cómo implementar la interfaz de usuario para la UWP, echa un vistazo a nuestras [aplicaciones para UWP de muestra](https://developer.microsoft.com/windows/samples) completas.

## <a name="next-fluent-design-system"></a>A continuación: el sistema Fluent Design
Si quieres obtener información sobre los principios que rigen Fluent Design (el sistema de diseño de Microsoft) y ver más características que puedes incorporar a tu aplicación para UWP, sigue con [sistema Fluent Design](../fluent-design-system/index.md).