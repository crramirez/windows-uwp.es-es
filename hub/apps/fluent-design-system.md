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
ms.openlocfilehash: 63e0cf18c2df28db22e79a057761996f9e8d679b
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215181"
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

Además de instrucciones de diseño, nuestros artículos sobre Fluent Design también muestran cómo escribir el código que permite hacer realidad tu diseño. UWP usa XAML, un lenguaje de marcado que facilita la creación de interfaces de usuario. Por ejemplo:

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
        ![fpo image](images/fluent/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Intuitiva

Una experiencia resulta intuitiva cuando se comporta tal y como espera el usuario. Al usar controles y patrones establecidos, y aprovechar la compatibilidad de la plataforma con la accesibilidad y la globalización, estás creando una experiencia sin esfuerzo que ayuda a los usuarios a ser más productivos.

Demostrar empatía consiste en hacer lo correcto en el momento correcto.

Las experiencias Fluent usan controles y patrones de forma coherente, por lo que se comportan tal y como espera el usuario. Las experiencias Fluent son accesibles a personas con una amplia diversidad de capacidades físicas e incorporan características de globalización para que todo el mundo pueda usarlas.

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/fluent/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Atractiva y envolvente

Fluent Design no tiene que ver con efectos llamativos. Incorpora efectos físicos que realmente mejoran la experiencia del usuario, ya que emulan experiencias para las que se ha programado nuestro cerebro a fin de procesarlas eficazmente.

## <a name="use-light"></a>Usar luz

La luz tiene una forma de atraer nuestra atención. Crea atmósfera y sensación de espacio, y es una herramienta muy práctica para iluminar la información.

Agrega luz a tu aplicación para UWP:

:::row:::
    :::column:::
        ![fpo image](images/fluent/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](/windows/uwp/design/style/reveal) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/fluent/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](/windows/uwp/design/style/reveal-focus) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Crear una sensación de profundidad

Vivimos en un mundo tridimensional. Al incorporar intencionadamente profundidad a la interfaz de usuario, hemos transformado una interfaz plana y en 2D en algo más, algo que presenta de manera eficaz los conceptos y la información mediante la creación de una jerarquía visual. Reinventa cómo las cosas se relacionan entre sí, dentro de un entorno físico y por niveles.

Agrega profundidad a tu aplicación para UWP:

:::row:::
    :::column:::
        ![fpo image](images/fluent/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](/windows/uwp/design/motion/parallax) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>Incorporar movimiento

Piensa en un diseño de movimiento similar al de una película. Las transiciones sin interrupciones te ayudan a concentrarte en la historia y a darle vida a tus experiencias. Podemos trasladar esas sensaciones a nuestros diseños, y llevar a los usuarios de una tarea a otra con una calidad cinematográfica.

Agrega movimiento a tu aplicación para UWP:

:::row:::
    :::column:::
        ![continuity gif](images/fluent/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](/windows/uwp/design/motion/connected-animation) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Crearla con el material adecuado

Las cosas que nos rodean en el mundo real son sensoriales y estimulantes. Se doblan, se estiran, rebotan, se dispersan y se deslizan. Estas cualidades materiales se trasladan a entornos digitales, lo que hace que la gente quiera alcanzarlos y tocar nuestros diseños.

Agrega material a tu aplicación para UWP:

:::row:::
    :::column:::
        ![fpo image](images/fluent/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](/windows/uwp/design/style/acrylic) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Kits de herramientas de diseño y ejemplos de código

¿Quieres empezar a crear tus propias aplicaciones con Fluent Design? Nuestros kits de herramientas para Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer y Sketch te ayudarán a empezar a crear tus diseños, y nuestros ejemplos te ayudarán a crear código con mayor rapidez.

:::row:::
    :::column:::
        ![fpo image](images/fluent/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::








