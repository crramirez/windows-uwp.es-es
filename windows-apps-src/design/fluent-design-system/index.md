---
description: Más información sobre Fluent Design y cómo aplicarlo a tus aplicaciones
title: Fluent Design System para las aplicaciones para UWP
author: mijacobs
keywords: diseño de aplicaciones para uwp, plataforma universal de windows, diseño de aplicaciones, interfaz, sistema fluent design
ms.author: mijacobs
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: high
ms.openlocfilehash: 5b57dc2ddae4c6e260df663097db5649866b96aa
ms.sourcegitcommit: ef5a1e1807313a2caa9c9b35ea20b129ff7155d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2018
ms.locfileid: "1638793"
---
# <a name="the-fluent-design-system-for-uwp-apps"></a>El sistema Fluent Design para las aplicaciones para UWP

## <a name="introduction"></a>Introducción

<img src="images/fluentdesign-app-header.jpg" alt=" " />

La interfaz de usuario está evolucionando. Está en expansión con el objetivo de incluir nuevas dimensiones e interfaces, desde 2D a 3D y posteriores, desde el teclado y el mouse a la función de mirada, lápiz y función táctil.  

El sistema Fluent Design es un conjunto de innovadoras características para UWP combinado con procedimientos recomendados para crear aplicaciones que funcionan a la perfección en todo tipo de dispositivos Windows.

Es nuestro sistema para crear interfaces de usuario adaptables, comprensivas y atractivas. 

**Adaptables: las experiencias Fluent parecen naturales en todos los dispositivos**

Las experiencias Fluent se adaptan al entorno. Una experiencia Fluent resulta cómoda en una tableta, un PC de escritorio y una Xbox&mdash;incluso funciona perfectamente en unos cascos de realidad mixta. Y cuando agregas más hardware, como un monitor adicional para tu PC, una experiencia Fluent puede sacarle mayor partido. 

**Comprensivas: las experiencias Fluent son intuitivas y eficaces**

Las experiencias Fluent se ajustan el comportamiento y propósito&mdash;que entienden y se anticipan cuando es necesario. Reúnen personas e ideas, tanto si están en lados opuestos del mundo o sentados una al lado de la otra. 

Demostrar empatía consiste en hacer lo correcto en el momento correcto. 

Las experiencias Fluent usan controles y patrones de forma coherente, por lo que se comportan tal y como espera el usuario. Las experiencias Fluent son accesibles a personas con una amplia diversidad de capacidades físicas e incorporan características de globalización para que todo el mundo pueda usarlas. 

**Atractivas: las experiencias Fluent son interesantes y envolventes** 

Mediante la incorporación de elementos del mundo físico, una experiencia Fluent aprovecha algo fundamental. Usa la luz, sombra, movimiento, profundidad y textura para organizar la información de manera que te resulte más intuitiva e instintiva. 

Fluent Design no tiene que ver con efectos llamativos. Incorpora efectos físicos que realmente mejoran la experiencia del usuario, ya que emulan experiencias para las que se ha programado nuestro cerebro a fin de procesarlas eficazmente. 

## <a name="applying-fluent-design-to-your-app"></a>Aplica Fluent Design a tu aplicación

Las características de Fluent Design están integradas en UWP. Algunas de estas características&mdash;como píxeles efectivos y el sistema de entrada universal&mdash;son automáticas. No tienes que escribir código adicional para poder usarlas. Otras características, como acrílico, son opcionales; puedes agregarlas a tu aplicación escribiendo el código que las incluye. 

> Para obtener más información sobre las características básicas que se incluyen automáticamente en cada aplicación para UWP, consulta el artículo [Introducción al diseño de aplicación para UWP](../basics/design-and-ui-intro.md). Si eres totalmente principiante en el desarrollo de UWP, es recomendable que primero eches un vistazo a nuestra [Introducción a la página UWP](https://developer.microsoft.com/windows/apps/getstarted). 

Para obtener más información sobre las nuevas características que te ayudarán a incorporar Fluent Design en tu aplicación, sigue leyendo.

## <a name="find-a-natural-fit"></a>Encontrar un ajuste natural

¿Cómo puedes hacer natural una aplicación en una variedad de dispositivos? Haciéndola sentir como si se hubiese diseñado teniendo en cuenta cada uno de los dispositivos. Un diseño de interfaz de usuario que se adapta a los diferentes tamaños de pantalla&mdash;de modo que no haya ningún espacio desperdiciado (tampoco ninguna congestión)&mdash;hace que la experiencia parezca natural, como si se hubiese diseñado para ese dispositivo. 

*  **Diseñar para los adecuados puntos de interrupción**

    En lugar de diseñar cada tamaño de pantalla individual, el hecho de centrarse en unos anchos claves (también denominados "puntos de interrupción") puede simplificar en gran medida tus diseños y código mientras tu aplicación se ve estupendamente en pantallas pequeñas y grandes.

    [Más información sobre los tamaños de pantalla y puntos de interrupción](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)

*  **Crear un diseño dinámico**

    Para que una aplicación se vea natural, necesita rellenar el espacio de pantalla disponible sin parecer demasiado abarrotada. UWP proporciona paneles que organizan el contenido en tablas, pilas y flujos, y puedes anidarlos uno dentro del otro.

    [Más información sobre los paneles de diseño de UWP](/windows/uwp/design/layout/layout-panels)

* **Diseñar para una variedad de dispositivos**

    Las aplicaciones para UWP pueden ejecutarse en una amplia variedad de dispositivos de Windows. Es útil entender qué dispositivos están disponibles, de que están hechos y cómo interactúan los usuarios con ellos.

    [Más información sobre dispositivos UWP](/windows/uwp/design/devices/)

* **Optimizar para la entrada derecha**

    Las aplicaciones para UWP admiten automáticamente interacciones comunes con mouse, teclado, lápiz y función táctil&mdash;no necesitas hace nada más. Sin embargo, puedes mejorar tu aplicación con el soporte optimizado para las entradas específicas, como el lápiz y el Surface Dial.

    [Más información sobre entradas e interacciones](/windows/uwp/design/input/input-primer)


## <a name="make-it-intuitive-and-powerful"></a>Hacerla intuitiva y eficaz

Una experiencia resulta intuitiva cuando se comporta tal y como espera el usuario. Al usar controles y patrones establecidos, y aprovechar el soporte de plataformas para accesibilidad y globalización, estás creando una experiencia sin esfuerzo que ayuda a los usuarios a ser más productivos. 

* **Usar el control adecuado para el trabajo**

    Los controles son los bloques de creación de la interfaz de usuario. Usar el control adecuado te ayuda a crear una interfaz de usuario que se comporta tal y como esperan los usuarios.  UWP proporciona más de 45 controles, que van desde botones simples hasta los eficaces controles de datos. 

    [Más información sobre los controles de UWP](/windows/uwp/design/controls-and-patterns/)

* **Aplicación inclusiva** 

    Una aplicación bien diseñada es accesible a personas con discapacidades. Con algún código adicional, puedes compartir tu aplicación con personas de todo el mundo.

    [Más información sobre la facilidad de uso](/windows/uwp/design/usability/)


## <a name="be-engaging-and-immersive"></a>Aplicación atractiva y envolvente 

Consigue que tu aplicación sea atractiva mediante la incorporación de elementos físicos, como la luz y el movimiento. 

## <a name="use-light"></a>Usar luz

La luz tiene una forma de dibujar nuestra atención. Crea una atmósfera y una sensación de espacio, y es una herramienta muy práctica para iluminar la información.
        
Agrega luz a tu aplicación para UWP:
        
* [Mostrar resaltado](../style/reveal.md) utiliza la luz para resaltar los elementos interactivos. La luz ilumina los elementos interactivos, el usuario puede interactuar con ella y mostrar bordes ocultos. La función Reveal está habilitada automáticamente en algunos controles, como la vista de lista y vista de cuadrícula. Puede habilitarla en otros controles aplicando nuestros estilos predefinidos de Mostrar resaltado. 

* [Reveal focus](../style/reveal-focus.md) utiliza luz para llamar la atención sobre el elemento que actualmente tiene el foco de entrada.  

## <a name="create-a-sense-of-depth"></a>Crear una sensación de profundidad

Vivimos en un mundo tridimensional. Al incorporar intencionadamente profundidad a la interfaz de usuario, hemos transformado una interfaz plana y en 2D en algo más&mdash;algo que presenta de manera eficaz los conceptos y la información mediante la creación de una jerarquía visual. Reinventa cómo las cosas se relacionan entre sí, dentro de un entorno físico y por niveles

Agrega profundidad a tu aplicación para UWP:

* [Acrílico](../style/acrylic.md) es un material translúcido que permite al usuario ver las capas de contenido, estableciendo una jerarquía de elementos de interfaz de usuario.

* [Parallax](../motion/parallax.md) crea la ilusión de profundidad al hacer que los objetos en primer plano se muevan más rápidamente que los objetos en segundo plano.

## <a name="incorporate-motion"></a>Incorporar movimiento

Piensa en un diseño de movimiento similar al de una película. Las transiciones sin interrupciones te ayudan a concentrarte en la historia y a darle vida a tus experiencias. Podemos llevar esas sensaciones a nuestros diseños, llevando a los usuarios de una tarea a la siguiente con una calidad cinematográfica.

Agrega movimiento a tu aplicación para UWP:

* [Las animaciones conectadas](../motion/connected-animation.md) ayudan al usuario a mantener el contexto creando una transición sin interrupciones entre escenas. 

## <a name="build-it-with-the-right-material"></a>Crearla con el material adecuado

Las cosas que nos rodean en el mundo real son sensoriales y estimulantes. Se doblan, se estiran, rebotan, se dispersan y se deslizan. Estas cualidades materiales se trasladan a entornos digitales, haciendo que la gente quiera alcanzarlos y tocar nuestros diseños.

Agrega material a tu aplicación para UWP: 
        
* [Acrílico](../style/acrylic.md) es un material translúcido que permite al usuario ver las capas de contenido, estableciendo una jerarquía de elementos de interfaz de usuario. 

## <a name="design-toolkits-and-code-samples"></a>Kits de herramientas de diseño y muestras de código

¿Quieres empezar a crear tus propias aplicaciones con Fluent Design? Nuestros kits de herramientas para Adobe XD, Adobe Illustrator, Adobe Photoshop, Framer y Sketch te ayudarán a empezar a crear tus diseños, y nuestras muestras te ayudarán a codificar con mayor rapidez.

* Echa un vistazo a nuestra [Página kits de herramientas de diseño y muestras](/windows/uwp/design/downloads/)

<img src="images/fluentdesign_header.png" alt=" " />








