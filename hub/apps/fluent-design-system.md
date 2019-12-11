---
description: Más información sobre Fluent Design y cómo incorporarlo a las aplicaciones
title: Fluent Design System para Windows
keywords: uwp app layout, universal windows platform, app design, interface, fluent design system
ms.date: 03/07/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.author: mcleans
author: mcleanbyron
ms.openlocfilehash: f46d6ba99d4342e1e4fe7a9926082a5981fd690a
ms.sourcegitcommit: 11c5cb559c949b6fe645499c2265434885882d8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2019
ms.locfileid: "74666910"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Los creadores de aplicaciones de Fluent Design System para Windows

![Encabezado de Fluent Design](images/fluent/fluentdesign-app-header.jpg)

## <a name="introduction"></a>Introducción

Fluent Design System es nuestro sistema para la creación de interfaces de usuario adaptables, comprensivas y atractivas.

## <a name="principles"></a>Principios

**Adaptables: las experiencias Fluent parecen naturales en todos los dispositivos**.

Las experiencias Fluent se adaptan al entorno. Una experiencia Fluent resulta cómoda en una tableta, un PC de escritorio y una Xbox; incluso funciona perfectamente en unos cascos de realidad mixta. Y cuando agregas más hardware, como un monitor adicional para tu PC, una experiencia Fluent puede sacarle mayor partido.

**Comprensivas: las experiencias Fluent son intuitivas e intensas**.

Las experiencias Fluent se ajustan al comportamiento y la intención: entienden y anticipan cuando es necesario. Reúnen a personas e ideas, tanto si están en lados opuestos del mundo o sentadas una al lado de la otra.

**Atractivas: las experiencias Fluent son interesantes y envolventes**.

Mediante la incorporación de elementos del mundo físico, una experiencia Fluent aprovecha algo fundamental. El uso de luz, sombra, movimiento, profundidad y textura permite organizar la información de manera que resulte intuitiva e instintiva.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>Aplicar Fluent Design a la aplicación con UWP

![Logotipo de Fluent Design](images/fluent/fluentdesign_header.png)

Nuestras directrices de diseño explican cómo aplicar los principios de Fluent Design a las aplicaciones. ¿Qué tipo de aplicaciones? Aunque muchas de nuestras directrices se pueden aplicar a cualquier plataforma, hemos creado UWP (Plataforma universal de Windows) para admitir Fluent Design.

Las características de Fluent Design están integradas en UWP. Algunas de estas características, como píxeles efectivos y el sistema de entrada universal, son automáticas. No tienes que escribir código adicional para poder usarlas. Otras características, como acrílico, son opcionales; puedes agregarlas a tu aplicación escribiendo el código que las incluya.

> Estamos incorporando controles de UWP al escritorio para que puedas mejorar el aspecto, la sensación y la funcionalidad de las aplicaciones de Windows o WPF existentes con características de Fluent Design. Para más información, consulta [Hospedar los controles de UWP en aplicaciones de WPF y Windows Forms](/windows/uwp/xaml-platform/xaml-host-controls).

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

Además de instrucciones de diseño, nuestros artículos sobre Fluent Design también muestran cómo escribir el código que permite hacer realidad tu diseño. UWP usa XAML, un lenguaje de marcado que facilita la creación de interfaces de usuario. A continuación te mostramos un ejemplo:

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="16,24"/>
</Grid>
```

![](images/fluent/xaml-example.png)


> Si no estás familiarizado con el desarrollo para UWP, consulta nuestra [página de introducción a UWP](/windows/uwp/get-started).

## <a name="find-a-natural-fit"></a>Encontrar un ajuste natural

¿Cómo puedes hacer natural una aplicación en una variedad de dispositivos? Haciéndola sentir como si se hubiese diseñado teniendo en cuenta cada uno de los dispositivos. Un diseño de interfaz de usuario que se adapta a diferentes tamaños de pantalla, de modo que no hay ningún espacio desperdiciado (tampoco ninguna congestión), hace que la experiencia parezca natural, como si se hubiese diseñado para ese dispositivo.

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
**Diseño de puntos de interrupción adecuados**

En lugar de diseñar cada tamaño de pantalla individual, el hecho de centrarse en unos anchos claves (también denominados "puntos de interrupción") puede simplificar en gran medida tus diseños y código mientras tu aplicación se ve estupendamente en pantallas pequeñas y grandes.

[Más información sobre los tamaños de pantalla y puntos de interrupción](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
**Creación de un diseño dinámico**

Para que una aplicación funcione de manera natural, debe adaptar su diseño a los distintos tamaños de pantalla y dispositivos. Puedes usar ajustes de tamaño automático, paneles de diseño, estados visuales e incluso definiciones de interfaz de usuario independientes en XAML para crear una interfaz de usuario adaptativa.

[Más información sobre el diseño dinámico](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
**Diseño para una variedad de dispositivos**

Las aplicaciones para UWP pueden ejecutarse en una amplia variedad de dispositivos Windows. Resulta útil comprender qué dispositivos están disponibles, para qué se han diseñado y cómo interactúan los usuarios con ellos.

[Más información sobre los dispositivos UWP](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
**Optimización para la entrada adecuada**

Las aplicaciones para UWP admiten automáticamente interacciones comunes con mouse, teclado, lápiz y función táctil. No necesitas hacer nada más. Sin embargo, si quieres, puedes mejorar tu aplicación gracias a la compatibilidad optimizada con entradas específicas, como el lápiz y Surface Dial.

[Más información sobre las entradas y las interacciones](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Intuitiva

Una experiencia resulta intuitiva cuando se comporta tal y como espera el usuario. Al usar controles y patrones establecidos, y aprovechar la compatibilidad de la plataforma con la accesibilidad y la globalización, estás creando una experiencia sin esfuerzo que ayuda a los usuarios a ser más productivos.

Demostrar empatía consiste en hacer lo correcto en el momento correcto.

Las experiencias Fluent usan controles y patrones de forma coherente, por lo que se comportan tal y como espera el usuario. Las experiencias Fluent son accesibles a personas con una amplia diversidad de capacidades físicas e incorporan características de globalización para que todo el mundo pueda usarlas.

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
**Provisión de la navegación adecuada**

Crea una experiencia fácilmente mediante la estructura de la aplicación y los componentes de navegación adecuados.

[Más información sobre la navegación](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
**Interactividad**

Los botones, las barras de comandos, los métodos abreviados de teclado y los menús contextuales permiten a los usuarios interactuar con la aplicación; son las herramientas que convierten una experiencia estática en algo dinámico.

[Más información sobre los comandos](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
**Uso del control adecuado para el trabajo**

Los controles son los bloques de creación de la interfaz de usuario. Usar el control adecuado te ayuda a crear una interfaz de usuario que se comporta tal y como esperan los usuarios. UWP proporciona más de 45 controles, que van desde botones simples hasta los eficaces controles de datos.

[Más información sobre los controles de UWP](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![imagen inclusiva](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
**Inclusividad** Una aplicación bien diseñada es accesible para las personas con discapacidades. Con algo de código adicional, puedes compartir tu aplicación con personas de todo el mundo.

[Más información sobre la facilidad de uso](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Atractiva y envolvente

Fluent Design no tiene que ver con efectos llamativos. Incorpora efectos físicos que realmente mejoran la experiencia del usuario, ya que emulan experiencias para las que se ha programado nuestro cerebro a fin de procesarlas eficazmente.

## <a name="use-light"></a>Usar luz

La luz tiene una forma de atraer nuestra atención. Crea atmósfera y sensación de espacio, y es una herramienta muy práctica para iluminar la información.

Agrega luz a tu aplicación para UWP:

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
**Mostrar resaltado**

La opción [Mostrar resaltado](/windows/uwp/design/style/reveal) usa luz para hacer que los elementos interactivos destaquen entre los otros. La luz ilumina los elementos con los que el usuario puede interactuar, de forma que se revelan los bordes ocultos. La opción Revelar está habilitada automáticamente en algunos controles, como las vistas de lista y de cuadrícula. Para habilitarla en otros controles, aplica nuestros estilos predefinidos de Mostrar resaltado.
:::row-end:::

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
**Reveal focus**

La opción [Revelar foco](/windows/uwp/design/style/reveal-focus) utiliza luz para destacar el elemento que actualmente tiene el foco de entrada.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Crear una sensación de profundidad

Vivimos en un mundo tridimensional. Al incorporar intencionadamente profundidad a la interfaz de usuario, hemos transformado una interfaz plana y en 2D en algo más, algo que presenta de manera eficaz los conceptos y la información mediante la creación de una jerarquía visual. Reinventa cómo las cosas se relacionan entre sí, dentro de un entorno físico y por niveles.

Agrega profundidad a tu aplicación para UWP:

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
**Parallax**

[Parallax](/windows/uwp/design/motion/parallax) crea la ilusión de profundidad al hacer que los objetos en primer plano se muevan más rápidamente que los objetos en segundo plano.
:::row-end:::

## <a name="incorporate-motion"></a>Incorporar movimiento

Piensa en un diseño de movimiento similar al de una película. Las transiciones sin interrupciones te ayudan a concentrarte en la historia y a darle vida a tus experiencias. Podemos trasladar esas sensaciones a nuestros diseños, y llevar a los usuarios de una tarea a otra con una calidad cinematográfica.

Agrega movimiento a tu aplicación para UWP:

:::row:::
    :::column:::
        ![GIF de continuidad](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
**Animaciones conectadas**

[Las animaciones conectadas](/windows/uwp/design/motion/connected-animation) ayudan al usuario a mantener el contexto creando una transición sin interrupciones entre escenas.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Crearla con el material adecuado

Las cosas que nos rodean en el mundo real son sensoriales y estimulantes. Se doblan, se estiran, rebotan, se dispersan y se deslizan. Estas cualidades materiales se trasladan a entornos digitales, lo que hace que la gente quiera alcanzarlos y tocar nuestros diseños.

Agrega material a tu aplicación para UWP:

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
**Acrílico**

[Acrílico](/windows/uwp/design/style/acrylic) es un tipo de material translúcido que permite al usuario ver las capas de contenido, estableciendo una jerarquía de elementos de interfaz de usuario.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Kits de herramientas de diseño y ejemplos de código

¿Quieres empezar a crear tus propias aplicaciones con Fluent Design? Nuestros kits de herramientas para Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer y Sketch te ayudarán a empezar a crear tus diseños, y nuestros ejemplos te ayudarán a crear código con mayor rapidez.

:::row:::
    :::column:::
        ![imagen fpo](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
**Página de kits de herramientas de diseño y ejemplos**

Echa un vistazo a nuestra [página de kits de herramientas de diseño y ejemplos](/windows/uwp/design/downloads/).
:::row-end:::









