---
author: jwmsft
title: Animaciones controladas por entradas
description: 
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, animación"
ms.localizationpriority: medium
ms.openlocfilehash: 24e5378d27ea5ce80f317ab951f1a19aa5f87b9b
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="input-driven-animations"></a>Animaciones controladas por entradas

En este artículo se proporciona una introducción a la API InputAnimation y recomienda cómo utilizar estos tipos de animaciones en la interfaz de usuario.

## <a name="prerequisites"></a>Requisitos previos

En este artículo damos por hecho que estás familiarizado con los conceptos tratados en estos artículos:

- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>Movimiento suave controlado por interacciones del usuario

En el lenguaje de diseño Fluent Design, la interacción entre los usuarios finales y las aplicaciones es de suma importancia. Las aplicaciones no solo tienen que buscar el elemento, sino que también deben responder de forma natural y dinámica a los usuarios que interactúan con ellas. Esto significa que cuando se coloca un dedo en la pantalla, la interfaz de usuario debe reaccionar de forma elegante a los distintos grados de la entrada, y el desplazamiento debe parecer homogéneo y obedecer al dedo que se desplaza por la pantalla.

Crear una interfaz de usuario que responda de forma dinámica y fluida a los resultados de las entradas del usuario produce una mayor participación de los usuarios: el movimiento no solo se visualizará correctamente sino que transmitirá una sensación agradable y natural cuando los usuarios interactúen con las distintas experiencias de la interfaz de usuario. Esto permite a los usuarios finales conectar de un modo más sencillo con la aplicación, lo que hace que la experiencia sea memorable y agradable.

## <a name="expanding-past-just-touch"></a>Expansión justo más allá de la pulsación

Aunque la entrada táctil es una de las interfaces más comunes que los usuarios finales utilizan para manipular el contenido de la interfaz de usuario, también utilizarán algunas otras modalidades de entrada, como el mouse y el lápiz. En estos casos, es importante que los usuarios finales perciban que la interfaz de usuario responde dinámicamente a su entrada, independientemente de qué modalidad de entrada decidan usar. Debes estar al corriente de las distintas modalidades de entrada a la hora de diseñar experiencias de movimiento basado en entradas.

## <a name="different-input-driven-motion-experiences"></a>Distintas experiencias de movimiento basado en entradas

El espacio InputAnimation proporciona varias experiencias diferentes para crear movimiento que responda de forma dinámica. Al igual que el resto del sistema de animación de la interfaz de usuario de Windows, estas animaciones controladas por entradas funcionan en un subproceso independiente, lo que ayuda a contribuir a la experiencia de movimiento dinámico. Sin embargo, en algunos casos en los que la experiencia aprovecha los controles y componentes XAML existentes, partes de esas experiencias siguen vinculadas al subproceso de la interfaz de usuario.

Existen tres experiencias principales que se crean al producir animaciones de movimiento dinámico controlado por entradas:

1. Mejora de las experiencias existentes de ScrollViewer: Permite que la posición de un ScrollViewer de XAML controle las experiencias de animación dinámica.
1. Experiencias controladas por la posición del puntero: Aprovecha la posición de un cursor en un UIElement con posicionamiento probado para controlar experiencias de animación dinámica.
1. Experiencias de manipulación personalizadas con InteractionTracker: Crear experiencias de manipulación totalmente personalizadas fuera del subproceso con InteractionTracker (por ejemplo, un lienzo de desplazamiento o zoom).

## <a name="enhancing-existing-scrollviewer-experiences"></a>Mejora de las experiencias existentes de ScrollViewer

Una de las formas habituales de crear experiencias más dinámicas es basarse en un control ScrollViewer de XAML existente. En estas situaciones, se aprovecha la posición de desplazamiento de un ScrollViewer para crear componentes adicionales de la interfaz de usuario que hagan que una experiencia de desplazamiento simple sea más dinámica. Algunos ejemplos incluyen los encabezados permanentes o discretos y el efecto parallax.

![Vista de lista con efecto parallax](images/animation/parallax.gif)

![Un encabezado discreto](images/animation/shy-header.gif)

Al crear estos tipos de experiencias, hay una fórmula general que se debe seguir:

1. Accede a ScrollManipulationPropertySet fuera del ScrollViewer de XAML que desees que controle una animación.
    - Se hace a través de la API ElementCompositionPreview.GetScrollViewerManipulationPropertySet(UIElement element).
    - Devuelve un CompositionPropertySet que contiene una propiedad denominada **Translation**.
1. Crea una ExpressionAnimation de Composición con una ecuación que haga referencia a la propiedad Translation.
1. Inicia la animación en una propiedad de un CompositionObject.

Para obtener más información sobre la creación de estas experiencias, consulta [Mejorar las experiencias existentes de ScrollViewer](scroll-input-animations.md).

## <a name="pointer-position-driven-experiences"></a>Experiencias controladas por la posición del puntero

Otra experiencia dinámica habitual en la que participan las entradas es controlar una animación en función de la posición de un puntero, como el cursor de un mouse. En estos casos, los desarrolladores aprovechan la ubicación de un cursor cuando se prueba su posicionamiento dentro de un UIElement de XAML que hace posible crear experiencias como Spotlight Reveal (Mostrar el foco de luz).

![Ejemplo de un foco de luz del puntero](images/animation/spotlight-reveal.gif)

![Ejemplo de rotación del puntero](images/animation/pointer-rotate.gif)

Al crear estos tipos de experiencias, hay una fórmula general que se debe seguir:

1. Accede a PointerPositionPropertySet de un UIElement de XAML del que desees saber la posición del cursor cuando se pruebe su posicionamiento.
    - Se hace a través de la API ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element).
    - Devuelve un CompositionPropertySet que contiene una propiedad denominada **Position**.
1. Crea una CompositionExpressionAnimation con una ecuación que haga referencia a la propiedad Position.
1. Inicia la animación en una propiedad de un CompositionObject.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>Experiencias de manipulación personalizadas con InteractionTracker

Uno de los desafíos a la hora de utilizar ScrollViewer de XAML es que está vinculado al subproceso de la interfaz de usuario. Como resultado, la experiencia de desplazamiento y zoom a menudo puede retrasarse y vibrar si el subproceso de interfaz de usuario está ocupado, lo que produce una experiencia poco atractiva. Además, muchos aspectos de la experiencia de ScrollViewer no se pueden personalizar. InteractionTracker se creó para resolver ambos problemas al proporcionar un conjunto de bloques de creación para crear experiencias de manipulación personalizadas que se ejecuten en un subproceso independiente.

![Ejemplo de interacciones en 3D](images/animation/interactions-3d.gif)

![Ejemplo de tirar para animar](images/animation/pull-to-animate.gif)

Al crear estos experiencias con InteractionTracker, hay una fórmula general que se debe seguir:

1. Crea el objeto InteractionTracker y define sus propiedades.
1. Crear VisualInteractionSources para cualquier CompositionVisual que debe capturar entradas para que las consuma InteractionTracker.
1. Crea una ExpressionAnimation de composición con una ecuación que haga referencia la propiedad Position de InteractionTracker.
1. Inicia la animación en una propiedad de un elemento visual de composición que desees que esté controlado por InteractionTracker.
