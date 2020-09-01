---
title: Animaciones de movimiento naturales
description: Obtenga información sobre las animaciones de movimiento naturales y cómo usarlas en la interfaz de usuario de la aplicación.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: 02c76991a60205042642f57fed475755db8c8071
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174089"
---
# <a name="natural-motion-animations"></a>Animaciones de movimiento naturales

En este artículo se proporciona información general breve sobre el espacio de NaturalMotionAnimation y cómo considerar conceptualmente el uso de estos tipos de animaciones en la interfaz de usuario.

## <a name="making-motion-feel-familiar-and-natural"></a>Conseguir que el movimiento esté familiarizado y sea natural

Las excelentes aplicaciones son aquellas que crean experiencias que capturan y retienen la atención del usuario y ayudan a los usuarios a realizar tareas. Motion es el factor de diferenciación clave que separa una interfaz de usuario de una experiencia de usuario, lo que produce una conexión entre los usuarios y la aplicación con la que interactúan. Cuanto mejor sea la conexión, mayor compromiso y satisfacción de los usuarios finales.

El movimiento unidireccional puede ayudar a compilar esta conexión mediante la creación de experiencias que les resultan familiares a los usuarios. Los usuarios tienen una expectativa inconsciencia de cómo perciben el movimiento basado en experiencias reales de la vida real. Vemos cómo los objetos se deslizan por el suelo, se desplazan por la tabla, se rebotan entre sí y oscilan con un muelle. Movimiento que aprovecha esta expectativa al basarse en el aspecto físico del mundo real y resulta más natural en los ojos. El movimiento se vuelve más natural e interactivo, pero lo que es más importante, toda la experiencia es más memorable y agradables.

![Movimiento de escala sin ](images/animation/scale-no-animation.gif)
 ![ movimiento de escala de animación con movimiento de escala Bézier cúbico ](images/animation/scale-cubic-bezier.gif)
 ![ con animación Spring](images/animation/scale-spring.gif)

El resultado neto es la mayor participación y retención de usuarios con la aplicación.

## <a name="balancing-control-and-dynamism"></a>Control de equilibrio y Dynamism

En la interfaz de usuario tradicional, [KeyFrameAnimation](/uwp/api/windows.ui.composition.keyframeanimation)s es la manera predominante de describir el movimiento. Los fotogramas clave proporcionan el mayor control a los diseñadores y desarrolladores para definir el inicio, el final y la interpolación. Aunque esto es muy útil en muchos casos, las animaciones de fotogramas clave no son muy dinámicas; el movimiento no es adaptable y tiene el mismo aspecto en cualquier condición.

En el otro extremo del espectro, se suelen observar simulaciones en los motores de juegos y físicos. Estas experiencias suelen ser las más reales y dinámicas con las que interactúan los usuarios: crear ese sentido de Ambiance y aleatoriedad que los usuarios ven todos los días. Aunque esto hace que el movimiento parezca más activo y dinámico, los diseñadores y desarrolladores tienen menos control, por lo que es más difícil integrarlos en la interfaz de usuario tradicional.

![Diagrama del espectro de control](images/animation/natural-motion-diagram.png)

[NaturalMotionAnimation](/uwp/api/windows.ui.composition.naturalmotionanimation)s para ayudar a salvar esta división, lo que permite un equilibrio de control para los elementos importantes de una animación como start/finish, pero manteniendo movimiento que parezca y sea natural y dinámico.

> [!NOTE]
> Los NaturalMotionAnimations no están diseñados como sustitutos de las animaciones de fotogramas clave; todavía hay lugares en el lenguaje de diseño fluida donde se recomiendan los fotogramas clave. Los NaturalMotionAnimations están diseñados para usarse en lugares donde se requiere movimiento, pero las animaciones de fotogramas clave no son lo suficientemente dinámicas.

## <a name="using-naturalmotionanimations"></a>Usar NaturalMotionAnimations

A partir de la actualización de Fall Creators, tiene acceso a una nueva experiencia de movimiento: **animaciones de Spring**. Consulte [animaciones de Spring](spring-animations.md) para obtener un tutorial más detallado de los muelles.

Este tipo de movimiento se consigue mediante el nuevo NaturalMotionAnimation: un nuevo tipo de animación centrado en permitir a los desarrolladores crear un movimiento de sensación más familiar y natural en su interfaz de usuario, con un equilibrio de control y dynamism. Exponen las siguientes funcionalidades:

- Defina los valores de inicio y fin.
- Defina InitialVelocity y ate a la entrada con InteractionTracker.
- Definir propiedades específicas de movimiento (por ejemplo, DampingRatio para muelles).

Fórmula general para empezar:

1. Cree NaturalMotionAnimation en el compositor con uno de los métodos **Create** .
1. Defina las propiedades de la animación.
1. Pase NaturalMotionAnimation como parámetro a la llamada StartAnimation de un CompositionObject.
    - O se establece en la propiedad Motion de un InertiaModifier InteractionTracker.

Un ejemplo básico que usa Spring NaturalMotionAnimation para crear un "muelle" visual en una nueva ubicación de desplazamiento X:

```csharp
_springAnimation = _compositor.CreateSpringScalarAnimation();
_springAnimation.Period = TimeSpan.FromSeconds(0.07);
_springAnimation.DelayTime = TimeSpan.FromSeconds(1);
_springAnimation.EndPoint = 500f;
sectionNav.StartAnimation("Offset.X", _springAnimation);
```