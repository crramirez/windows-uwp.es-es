---
title: Animaciones controladas por entradas
description: Obtenga información sobre la API de InputAnimation y cómo usar animaciones controladas por la entrada para crear un movimiento de respuesta dinámica en la interfaz de usuario de la aplicación.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: 0f9abd902e39b645f27b7a0f5d521097ca4aff8a
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054365"
---
# <a name="input-driven-animations"></a>Animaciones controladas por entradas

En este artículo se proporciona una introducción a la API de InputAnimation y se recomienda usar estos tipos de animaciones en la interfaz de usuario.

## <a name="prerequisites"></a>Prerrequisitos

Aquí se supone que está familiarizado con los conceptos descritos en estos artículos:

- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="smooth-motion-driven-from-user-interactions"></a>Movimiento suave controlado por las interacciones del usuario

En el lenguaje de diseño fluida, la interacción entre los usuarios finales y las aplicaciones es de la máxima importancia. Las aplicaciones no solo tienen que buscar el elemento, sino que también responden de forma natural y dinámica a los usuarios que interactúan con ellos. Esto significa que cuando se coloca un dedo en la pantalla, la interfaz de usuario debe reaccionar correctamente a los grados de entrada cambiantes; el desplazamiento debe ser suave y ceñirse a la panorámica de un dedo por la pantalla.

La creación de una interfaz de usuario que responda de forma dinámica y fluida a los resultados de los datos proporcionados por el usuario con mayor interacción de usuario: el movimiento ahora no solo tiene un buen aspecto, sino que es bueno y natural cuando los usuarios interactúan con sus diferentes experiencias de IU. Esto permite a los usuarios finales conectarse con mayor facilidad a su aplicación, lo que hace que la experiencia sea más memorable y agradables.

## <a name="expanding-past-just-touch"></a>Expandir solo la entrada táctil

Aunque Touch es una de las interfaces más comunes que los usuarios finales usan para manipular el contenido de la interfaz de usuario, también usarán otras modalidades de entrada, como el mouse y el lápiz. En estos casos, es importante que los usuarios finales perciban que la interfaz de usuario responde de forma dinámica a la entrada, independientemente de qué modal de entrada decida usar. Debe ser Cognizant de las distintas modalidades de entrada al diseñar experiencias de movimiento controladas por la entrada.

## <a name="different-input-driven-motion-experiences"></a>Experiencias de movimiento controladas por entrada diferentes

El espacio InputAnimation proporciona varias experiencias diferentes para crear un movimiento de respuesta dinámica. Al igual que el resto del sistema de animación de la interfaz de usuario de Windows, estas animaciones controladas por entrada operan en un subproceso independiente, lo que ayuda a contribuir a la experiencia de movimiento dinámico. Sin embargo, en algunos casos en los que la experiencia aprovecha los controles y componentes de XAML existentes, algunas de esas experiencias siguen siendo enlazadas a subprocesos de interfaz de usuario.

Hay tres experiencias principales que se crean al compilar animaciones de movimiento basadas en la entrada dinámica:

1. Mejora de las experiencias de ScrollView existentes: habilita la posición de un ScrollViewer de XAML para impulsar experiencias de animación dinámicas.
1. Experiencias orientadas a la posición del puntero: Use la posición de un cursor en un UIElement de la prueba de posicionamiento para impulsar experiencias dinámicas de animación.
1. Experiencias de manipulación personalizada con InteractionTracker: cree una experiencia de manipulación completamente personalizada y sin subproceso con InteractionTracker (por ejemplo, un lienzo de desplazamiento y zoom).

## <a name="enhancing-existing-scrollviewer-experiences"></a>Mejorar las experiencias de ScrollViewer existentes

Una de las formas más comunes de crear experiencias más dinámicas es basarse en un control ScrollViewer XAML existente. En estas situaciones, se aprovecha la posición de desplazamiento de un ScrollViewer para crear componentes adicionales de la interfaz de usuario que permiten una experiencia de desplazamiento simple más dinámica. Algunos ejemplos son los encabezados de permanencia y tímida y el Parallax.

![Vista de lista con Parallax](images/animation/parallax.gif)

![Un encabezado tímida](images/animation/shy-header.gif)

Al crear estos tipos de experiencias, hay una fórmula general que debe seguir:

1. Obtenga acceso a ScrollManipulationPropertySet fuera de la ScrollViewer de XAML que quiere controlar una animación.
    - Se realiza a través de la API ElementCompositionPreview. GetScrollViewerManipulationPropertySet (Elemento UIElement)
    - Devuelve un CompositionPropertySet que contiene una propiedad denominada **Translation** .
1. Cree una composición ExpressionAnimation con una ecuación que haga referencia a la propiedad Translation.
1. Inicie la animación en la propiedad de un CompositionObject.

Para obtener más información sobre la creación de estas experiencias, vea [mejorar las experiencias de ScrollViewer existentes](scroll-input-animations.md).

## <a name="pointer-position-driven-experiences"></a>Experiencias orientadas a la posición del puntero

Otra experiencia dinámica común que implica la entrada es impulsar una animación basada en la posición de un puntero, como un cursor del mouse. En estas situaciones, los desarrolladores aprovechan la ubicación de un cursor cuando se prueba el posicionamiento en un UIElement de XAML que permite crear experiencias como Spotlight.

![Ejemplo de Spotlight de puntero](images/animation/spotlight-reveal.gif)

![Ejemplo de rotación de puntero](images/animation/pointer-rotate.gif)

Al crear estos tipos de experiencias, hay una fórmula general que debe seguir:

1. Acceda a PointerPositionPropertySet fuera de un UIElement de XAML que quieras conocer la posición del cursor cuando se probó la visita.
    - Se realiza a través de la API ElementCompositionPreview. GetPointerPositionPropertySet (Elemento UIElement)
    - Devuelve un CompositionPropertySet que contiene una propiedad denominada **Position** .
1. Cree un CompositionExpressionAnimation con una ecuación que haga referencia a la propiedad Position.
1. Inicie la animación en la propiedad de un CompositionObject.

## <a name="custom-manipulation-experiences-with-interactiontracker"></a>Experiencias de manipulación personalizada con InteractionTracker

Uno de los desafíos relacionados con el uso de un ScrollViewer de XAML es que está enlazado al subproceso de interfaz de usuario. Como resultado, la experiencia de desplazamiento y zoom a menudo se puede retrasar y vibrar si el subproceso de la interfaz de usuario está ocupado y tiene como resultado una experiencia no atractiva. Además, no es posible personalizar muchos aspectos de la experiencia de ScrollViewer. InteractionTracker se creó para resolver ambos problemas proporcionando un conjunto de bloques de creación para crear experiencias de manipulación personalizadas que se ejecutan en un subproceso independiente.

![ejemplo de interacciones 3D](images/animation/interactions-3d.gif)

![Extraer del ejemplo de animación](images/animation/pull-to-animate.gif)

Al crear experiencias con InteractionTracker, hay una fórmula general que debe seguir:

1. Cree el objeto InteractionTracker y defina sus propiedades.
1. Cree VisualInteractionSources para cualquier CompositionVisual que debería capturar la entrada de InteractionTracker que se va a consumir.
1. Cree una composición ExpressionAnimation con una ecuación que haga referencia a la propiedad Position de InteractionTracker.
1. Inicie la animación en la propiedad de un control visual de composición que quiera que esté controlada por InteractionTracker.
