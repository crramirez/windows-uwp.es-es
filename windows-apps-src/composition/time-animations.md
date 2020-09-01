---
title: Animaciones basadas en el tiempo
description: Aprenda a usar las clases KeyFrameAnimations para crear animaciones basadas en tiempo que guíen a los usuarios a través de cambios en la interfaz de usuario.
ms.date: 12/12/2018
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: 8846fc11dc39a3931d8f3278caf13b7aff464bc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160839"
---
# <a name="time-based-animations"></a>Animaciones basadas en tiempo

Cuando un componente de, o toda la experiencia del usuario cambia, los usuarios finales suelen observar esto de dos maneras: a lo largo del tiempo o de forma instantánea. En la plataforma Windows, la primera es preferible a las experiencias de usuario más recientes que cambian con frecuencia y se sorprende a los usuarios finales, ya que no pueden seguir lo que ha sucedido. A continuación, el usuario final percibe la experiencia como discordante y no natural.

En su lugar, puede cambiar la interfaz de usuario a lo largo del tiempo para guiar al usuario final a través de o notificarles los cambios en la experiencia. En la plataforma de Windows, esto se hace mediante animaciones basadas en el tiempo, también conocido como KeyFrameAnimations. KeyFrameAnimations permiten cambiar una interfaz de usuario a lo largo del tiempo y controlar cada aspecto de la animación, lo que incluye cómo y cuándo se inicia y cómo alcanza su estado final. Por ejemplo, la animación de un objeto en una nueva posición superior a 300 milisegundos es más agradable que el "teleporting" al instante. Al usar animaciones en lugar de cambios instantáneos, el resultado neto es una experiencia más agradable y atractiva.

## <a name="types-of-time-based-animations"></a>Tipos de animaciones basadas en tiempo

Existen dos categorías de animaciones basadas en el tiempo que puede usar para crear experiencias de usuario atractivas en Windows:

**Animaciones explícitas** : como el nombre indica, se inicia explícitamente la animación para efectuar actualizaciones.
**Animaciones implícitas** : el sistema inicia estas animaciones en su nombre cuando se cumple una condición.

En este artículo, se explica cómo crear y usar animaciones _explícitas_ basadas en tiempo con KeyFrameAnimations.

En el caso de animaciones basadas en tiempo implícitas y explícitas, hay tipos diferentes, correspondientes a los diferentes tipos de propiedades de CompositionObjects, que se pueden animar.

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>Crear animaciones basadas en tiempo con KeyFrameAnimations

Antes de describir cómo crear animaciones explícitas basadas en tiempo con KeyFrameAnimations, vamos a analizar algunos conceptos.

- Fotogramas clave: son las "instantáneas" individuales en las que se animará una animación.
  - Definido como pares clave-valor &. La clave representa el progreso entre 0 y 1, también conocido en qué punto de la duración de la animación tiene lugar la "instantánea". El otro parámetro representa el valor de la propiedad en este momento.
- Propiedades de KeyFrameAnimation: opciones de personalización que se pueden aplicar para satisfacer las necesidades de la interfaz de usuario.
  - DelayTime: tiempo antes de que se inicie una animación después de llamar a StartAnimation.
  - Duración: duración de la animación.
  - IterationBehavior: recuento o comportamiento de repetición infinito para una animación.
  - IterationCount: número de veces finitas que se repetirá una animación de fotogramas clave.
  - Recuento de fotogramas clave: lectura de cuántos fotogramas clave en una animación de fotogramas clave determinada.
  - StopBehavior: especifica el comportamiento de un valor de propiedad de animación cuando se llama a StopAnimation
  - Direction: especifica la dirección de la animación para la reproducción.
- Grupo de animación: iniciar varias animaciones al mismo tiempo.
  - Se utiliza a menudo cuando se desea animar varias propiedades al mismo tiempo.

Para obtener más información, consulte [CompositionAnimationGroup](/uwp/api/windows.ui.composition.compositionanimationgroup).

Teniendo en cuenta estos conceptos, vamos a consultar la fórmula general para construir un KeyFrameAnimation:

1. Identifique el CompositionObject y su propiedad respectiva que debe animar.
1. Cree una plantilla de tipo KeyFrameAnimation del compositor que coincida con el tipo de propiedad que desee animar.
1. Con la plantilla de animación, empiece a agregar fotogramas clave y a definir propiedades de la animación.
    - Se requiere al menos un fotograma clave (el fotograma clave 100% o 1F).
    - También se recomienda definir una duración.
1. Una vez que esté listo para ejecutar esta animación, llame a StartAnimation (...) en el CompositionObject, dirigido a la propiedad que desea animar. Concretamente:
    - `visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. Si tiene una animación en ejecución y desea detener la animación o el grupo de animaciones, puede usar estas API:
    - `visual.StopAnimation("targetProperty");`
    - `visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

Echemos un vistazo a un ejemplo para ver esta fórmula en acción.

## <a name="example"></a>Ejemplo

En este ejemplo, desea animar el desplazamiento de un control visual desde <0, 0, 0> a <200, 0, 0> más de 1 segundo. Además, desea ver la animación visual entre estas posiciones 10 veces.

![Animación de fotogramas clave](images/animation/animated-rectangle.gif)

Primero debe empezar por identificar el CompositionObject y la propiedad que desea animar. En este caso, el cuadrado rojo se representa mediante un visual de composición denominado `redVisual` . La animación se inicia a partir de este objeto.

Después, como desea animar la propiedad offset, debe crear un Vector3KeyFrameAnimation (el desplazamiento es del tipo Vector3). También se definen los fotogramas clave correspondientes para el KeyFrameAnimation.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

A continuación, defina las propiedades de KeyFrameAnimation para describir su duración, junto con el comportamiento para animar entre las dos posiciones (actual y <200, 0,0>) 10 veces.

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

Por último, para ejecutar una animación, debe iniciarla en una propiedad de un CompositionObject.

```csharp
redVisual.StartAnimation("Offset", animation);
```

Este es el código completo.

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redVisual)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    redVisual.StartAnimation("Offset", animation);
} 
```