---
description: Más información sobre Fluent Design y cómo aplicarlo a tus aplicaciones
title: Fluent Design System para Windows
keywords: diseño de aplicaciones para uwp, plataforma universal de windows, diseño de aplicaciones, interfaz, sistema fluent design
ms.date: 3/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7c5d2c1b112b96dc86d1dfef3015f9b52f43cb83
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8479359"
---
# <a name="the-fluent-design-system-for-windows-app-creators"></a>Los creadores de la aplicación de Fluent Design System para Windows

![Encabezado de diseño Fluent](images/fluentdesign-app-header.jpg)

## <a name="introduction"></a>Introducción

El sistema Fluent Design es nuestro sistema para crear adaptables, comprensivas y las interfaces de usuario atractivas.

## <a name="principles"></a>Principios

**Adaptables: las experiencias Fluent parecen naturales en todos los dispositivos**

Las experiencias Fluent se adaptan al entorno. Una experiencia Fluent resulta cómoda en una tableta, un equipo de escritorio y una consola Xbox, incluso funciona perfectamente en unos cascos de realidad mixta. Y cuando agregas más hardware, como un monitor adicional para tu PC, una experiencia Fluent puede sacarle mayor partido.

**Comprensivas: las experiencias Fluent son intuitivas y eficaces**

Las experiencias Fluent se ajustan el comportamiento y propósito&mdash;que entienden y se anticipan cuando es necesario. Reúnen personas e ideas, tanto si están en lados opuestos del mundo o sentados una al lado de la otra.

**Atractivas: las experiencias Fluent son interesantes y envolventes**

Mediante la incorporación de elementos del mundo físico, una experiencia Fluent aprovecha algo fundamental. Usa la luz, sombra, movimiento, profundidad y textura para organizar la información de manera que te resulte más intuitiva e instintiva.


## <a name="applying-fluent-design-to-your-app-with-uwp"></a>Aplica Fluent Design a tu aplicación con UWP

![Logotipo de diseño Fluent](images/fluentdesign_header.png)

Nuestras directrices de diseño explican cómo aplicar los principios de diseño Fluent a las aplicaciones. ¿Qué tipo de aplicaciones? Mientras que muchas de nuestras directrices pueden aplicarse a cualquier plataforma, hemos creado para UWP (la plataforma Universal de Windows) para admitir Fluent Design.

Las características de Fluent Design están integradas en UWP. Algunas de estas características&mdash;como píxeles efectivos y el sistema de entrada universal&mdash;son automáticas. No tienes que escribir código adicional para poder usarlas. Otras características, como acrílico, son opcionales; puedes agregarlas a tu aplicación escribiendo el código que las incluye.

> Estamos incorporando controles de UWP en el escritorio para que puedas mejorar el aspecto, la sensación y la funcionalidad de las aplicaciones de Windows o WPF existentes con características de Fluent Design. Para obtener más información, vea [los controles de UWP de Host en aplicaciones de WPF y Windows Forms](/windows/uwp/xaml-platform/xaml-host-controls).

<!-- To apply Fluent Design to your app, follow our guidelines and use UWP (Universal Windows Platform) you can use UWP UI features combined with best practices for creating apps that perform beautifully on all types of Windows-powered devices. -->

Además de instrucciones de diseño, nuestros artículos de Fluent Design también muestran cómo escribir el código que hace que los diseños a producirse. UWP usa XAML, un lenguaje de marcado que es más fácil crear interfaces de usuario. A continuación te mostramos un ejemplo:

```xaml
<Grid BorderBrush="Blue" BorderThickness="4">
    <TextBox Text="Design with XAML" Margin="20" Padding="24,16"/>
</Grid>
```

![](images/xaml-example.png)


> Si estás familiarizado con el desarrollo de UWP, consulta nuestro [empezar a trabajar con la página UWP](https://developer.microsoft.com/windows/apps/getstarted).

## <a name="find-a-natural-fit"></a>Encontrar un ajuste natural

¿Cómo puedes hacer natural una aplicación en una variedad de dispositivos? Haciéndola sentir como si se hubiese diseñado teniendo en cuenta cada uno de los dispositivos. Un diseño de interfaz de usuario que se adapta a los diferentes tamaños de pantalla&mdash;de modo que no haya ningún espacio desperdiciado (tampoco ninguna congestión)&mdash;hace que la experiencia parezca natural, como si se hubiese diseñado para ese dispositivo.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-size-classes.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for the right breakpoints**

        Instead of designing for every individual screen size, focusing on a few key widths (also called "breakpoints") can greatly simplify your designs and code while still making your app look great on small to large screens.

        [Learn about screen sizes and breakpoints](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/rspd-resize.gif)
    :::column-end:::
    :::column span="2":::
        **Create a responsive layout**

        For an app to feel natural, it should adapt its layout to different screen sizes and devices. You can use automatic sizing, layout panels, visual states, and even separate UI definitions in XAML to create a responsive UI.

        [Learn about responsive design](/windows/uwp/design/layout/responsive-design)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/devices.jpg)
    :::column-end:::
    :::column span="2":::
        **Design for a spectrum of devices**

        UWP apps can run on a wide variety of Windows-powered devices. It's helpful to understand which devices are available, what they're made for, and how users interact with them.

        [Learn about UWP devices](/windows/uwp/design/devices/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/keyboard-shortcuts.jpg)
    :::column-end:::
    :::column span="2":::
        **Optimize for the right input**

        UWP apps automatically support common mouse, keyboard, pen, and touch interactions&mdash;there's nothing extra you have to do. But you can enhance your app with optimized support for specific inputs, like pen and the Surface Dial.

        [Learn about inputs and interactions](/windows/uwp/design/input/input-primer)
:::row-end:::

## <a name="make-it-intuitive"></a>Hacerla intuitiva

Una experiencia resulta intuitiva cuando se comporta la forma y como espera el usuario. Al usar controles y patrones establecidos, y aprovechar el soporte de plataformas para accesibilidad y globalización, estás creando una experiencia sin esfuerzo que ayuda a los usuarios a ser más productivos.

Demostrar empatía consiste en hacer lo correcto en el momento correcto.

Las experiencias Fluent usan controles y patrones de forma coherente, por lo que se comportan tal y como espera el usuario. Las experiencias Fluent son accesibles a personas con una amplia diversidad de capacidades físicas e incorporan características de globalización para que todo el mundo pueda usarlas.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-navview.png)
    :::column-end:::
    :::column span="2":::
        **Provide the right navigation**

        Create an effortless experience by using the right app structure and navigation components.

        [Learn about navigation](/windows/uwp/design/basics/navigation-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-commanding.png)
    :::column-end:::
    :::column span="2":::
        **Be interactive**

        Buttons, command bars, keyboard shortcuts, and context menus enable users to interact with your app; they're the tools that change a static experience into something dynamic.

        [Learn about commanding](/windows/uwp/design/basics/commanding-basics/)
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-controls-2.jpg)
    :::column-end:::
    :::column span="2":::
        **Use the right control for the job**

        Controls are the building blocks of the user interface; using the right control helps you create a user interface that behaves the way users expect it to.  UWP provides more than 45 controls,ranging from simple buttons to powerful data controls.

        [Learn about UWP controls](/windows/uwp/design/controls-and-patterns/)
:::row-end:::

:::row:::
    :::column:::
        ![inclusive image](images/thumbnail-inclusive.png)
    :::column-end:::
    :::column span="2":::
        **Be inclusive**
        A well-design app is accessible to people with disabilities. With some extra coding, you can share your app with people around the world.

        [Learn about Usability](/windows/uwp/design/usability/)
:::row-end:::

## <a name="be-engaging-and-immersive"></a>Aplicación atractiva y envolvente

Fluent Design no tiene que ver con efectos llamativos. Incorpora efectos físicos que realmente mejoran la experiencia del usuario, ya que emulan experiencias para las que se ha programado nuestro cerebro a fin de procesarlas eficazmente.

## <a name="use-light"></a>Usar luz

La luz tiene una forma de dibujar nuestra atención. Crea una atmósfera y una sensación de espacio, y es una herramienta muy práctica para iluminar la información.

Agrega luz a tu aplicación para UWP:

:::row:::
    :::column:::
        ![fpo image](../style/images/Nav_Reveal_Animation.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal highlight**

        [Reveal highlight](../style/reveal.md) uses light to make interactive elements stand out. Light illuminates the elements the user can interact with, revealing hidden borders. Reveal is automatically enabled on some controls, such as list view and grid view. You can enable it on other controls by applying our predefined Reveal highlight styles.
:::row-end:::

:::row:::
    :::column:::
        ![fpo image](../style/images/traveling-focus-fullscreen-light-rf.gif)
    :::column-end:::
    :::column span="2":::
        **Reveal focus**

        [Reveal focus](../style/reveal-focus.md) uses light to call attention to the element that currently has input focus.
:::row-end:::

## <a name="create-a-sense-of-depth"></a>Crear una sensación de profundidad

Vivimos en un mundo tridimensional. Al incorporar intencionadamente profundidad a la interfaz de usuario, hemos transformado una interfaz plana y en 2D en algo más&mdash;algo que presenta de manera eficaz los conceptos y la información mediante la creación de una jerarquía visual. Reinventa cómo las cosas se relacionan entre sí, dentro de un entorno físico y por niveles

Agrega profundidad a tu aplicación para UWP:

:::row:::
    :::column:::
        ![fpo image](../motion/images/_parallax_v2.gif)
    :::column-end:::
    :::column span="2":::
        **Parallax**

        [Parallax](../motion/parallax.md) creates the illusion of depth by making items in the foreground appear to move more quickly than items in the background.
:::row-end:::

## <a name="incorporate-motion"></a>Incorporar movimiento

Piensa en un diseño de movimiento similar al de una película. Las transiciones sin interrupciones te ayudan a concentrarte en la historia y a darle vida a tus experiencias. Podemos llevar esas sensaciones a nuestros diseños, llevando a los usuarios de una tarea a la siguiente con una calidad cinematográfica.

Agrega movimiento a tu aplicación para UWP:

:::row:::
    :::column:::
        ![continuity gif](images/continuityXbox.gif)
    :::column-end:::
    :::column span="2":::
        **Connected animations**

        [Connected animations](../motion/connected-animation.md) help the user maintain context by creating a seamless transition between scenes.
:::row-end:::

## <a name="build-it-with-the-right-material"></a>Crearla con el material adecuado

Las cosas que nos rodean en el mundo real son sensoriales y estimulantes. Se doblan, se estiran, rebotan, se dispersan y se deslizan. Estas cualidades materiales se trasladan a entornos digitales, haciendo que la gente quiera alcanzarlos y tocar nuestros diseños.

Agrega material a tu aplicación para UWP:

:::row:::
    :::column:::
        ![fpo image](../style/images/acrylic_lighttheme_base.png)
    :::column-end:::
    :::column span="2":::
        **Acrylic**

        [Acrylic](../style/acrylic.md) is a translucent material that lets the user see layers of content, establishing a hierarchy of UI elements.
:::row-end:::

## <a name="design-toolkits-and-code-samples"></a>Kits de herramientas de diseño y muestras de código

¿Quieres empezar a crear tus propias aplicaciones con Fluent Design? Nuestros kits de herramientas para Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer y Sketch te ayudarán a empezar a crear tus diseños, y nuestras muestras te ayudarán a codificar con mayor rapidez.

:::row:::
    :::column:::
        ![fpo image](images/thumbnail-toolkits.jpg)
    :::column-end:::
    :::column span="2":::
        **Design toolkits and samples page**

        Check out our [Design toolkits and samples page](/windows/uwp/design/downloads/)
:::row-end:::









