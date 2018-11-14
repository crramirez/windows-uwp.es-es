---
author: jwmsft
title: Animaciones de movimientos naturales
description: Más información sobre las animaciones de movimiento natural y cómo usarlas en la interfaz de usuario de tu aplicación.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: 537e722917f00d590428dd2b5ee2d24e023e52b6
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6280568"
---
# <a name="natural-motion-animations"></a>Animaciones de movimientos naturales

En este artículo se proporciona una breve descripción del espacio NaturalMotionAnimation y cómo pensar conceptualmente sobre el uso de estos tipos de animaciones en la interfaz de usuario.

## <a name="making-motion-feel-familiar-and-natural"></a>Hacer que el movimiento resulte familiar y natural

Las grandes aplicaciones son los que crean experiencias que capturan y conservan la atención del usuario y ayudan a los usuarios en las tareas. El movimiento es el factor diferenciador clave que distingue una interfaz de usuario de una experiencia de usuario, que provoca una conexión entre los usuarios y la aplicación con la que interactúan. Cuanto mejor sea la conexión, mayor participación y satisfacción obtendrán los usuarios finales.

Una forma en la que el movimiento puede ayudar a formar esta conexión es creando experiencias que a los usuarios les resulten familiares. Los usuarios tienen una expectativa inconsciente sobre cómo perciben el movimiento que se basa en sus experiencias reales. Vemos cómo los objetos se deslizan por el suelo, caen de la tabla, rebotan entre sí y oscilan con un muelle. El movimiento que aprovecha estas expectativas al estar basado en la física real resulta más natural a nuestros ojos. El movimiento se vuelve más natural e interactivo, pero lo más importante es que toda la experiencia es más memorable y agradable.

![Escalar el movimiento sin animación](images/animation/scale-no-animation.gif)
![Escalar el movimiento con Cubic Bezier](images/animation/scale-cubic-bezier.gif)
![Escalar el movimiento con animación de muelle](images/animation/scale-spring.gif)

El resultado final es una mayor participación y retención del usuario con la aplicación.

## <a name="balancing-control-and-dynamism"></a>Equilibrio del control y el dinamismo

En una interfaz de usuario tradicional, las [KeyFrameAnimations](https://docs.microsoft.com/uwp/api/windows.ui.composition.keyframeanimation) son la forma principal de describir el movimiento. Los fotogramas clave proporcionaban el mayor control a los diseñadores y desarrolladores para definir el inicio, el final y la interpolación. Aunque esto resulta muy útil en muchos casos, las animaciones de fotogramas clave no son muy dinámicas; el movimiento no es adaptativo y parece igual en todas las situaciones.

En el lado contrario hay simulaciones que suelen verse en los motores de juegos y física. A menudo estas experiencias son las más realistas y dinámica con las que los usuarios interactúan y cran esa sensación de ambiente y aleatoriedad que los usuarios ven a diario. Aunque esto hace que el movimiento resulte más viva y dinámica, los diseñadores y desarrolladores tienen menos control, por lo que resulta más difícil integrarlo en una interfaz de usuario tradicional.

![Diagrama del espectro de control](images/animation/natural-motion-diagram.png)

Las [NaturalMotionAnimations](https://docs.microsoft.com/uwp/api/windows.ui.composition.naturalmotionanimation) ayudan a salvar esta distancia, ya que permiten un equilibrio entre el control de los elementos importantes de una animación, como el inicio y el fin, y a la vez mantienen un movimiento que resulta dinámico y natural.

> [!NOTE]
> Las NaturalMotionAnimations no son un sustituto para las animaciones de fotogramas clave: todavía hay sitios en el lenguaje de diseño Fluent Design en los que se recomiendan los fotogramas clave. Las NaturalMotionAnimations están pensadas para usarse en lugares donde se requiere movimiento y en los que las animaciones de fotogramas clave no son lo suficientemente dinámicas.

## <a name="using-naturalmotionanimations"></a>Uso de NaturalMotionAnimations

A partir de la actualización Fall Creators, tienes acceso a una nueva experiencia de movimiento: las **animaciones de muelle**. Consulta [animaciones de muelle](spring-animations.md) para obtener un tutorial más detallado sobre los muelles.

Este tipo de movimiento se consigue mediante la nueva NaturalMotionAnimation: un nuevo tipo de animación centrado en permitir a los desarrolladores integrar un movimiento más natural y familiar en su interfaz de usuario, con un equilibrio entre el control y el dinamismo. Estas son sus capacidades:

- Definir los valores de inicio y finalización.
- Definir InitialVelocity y vincularse a la entrada con InteractionTracker.
- Definir las propiedades específicas del movimiento (por ejemplo, DampingRatio en el caso de los muelles).

Fórmula general para empezar:

1. Crea la NaturalMotionAnimation en el compositor mediante uno de los métodos **Create**.
1. Define las propiedades de la animación.
1. Pasa la NaturalMotionAnimation como un parámetro a la llamada StartAnimation desde un CompositionObject.
    - O establece la propiedad Motion de un InertiaModifier de InteractionTracker.

Un ejemplo básico que usa una NaturalMotionAnimation de un muelle para crear un objeto visual "muelle" a una nueva ubicación de X Offset:

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```
