---
author: jwmsft
title: Animaciones basadas en tiempo
description: Usa las clases KeyFrameAnimation para cambiar la interfaz de usuario con el tiempo.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: bf6d3f16c7b240ca370c01a787fef09862f35863
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5830155"
---
# <a name="time-based-animations"></a>Animaciones basadas en tiempo

Cuando cambian un componente de la experiencia de usuario o esta en su totalidad, los usuarios finales a menudo lo ven de dos formas: con el tiempo o de forma instantánea. En la plataforma de Windows, el primero se prefiere el último: experiencias de usuario que cambian al instante a menudo confunden y sorprenden a los usuarios finales porque no pueden seguir qué ha ocurrido. Por ello, el usuario final percibe la experiencia como molesta y poco natural.

En su lugar, puedes cambiar la interfaz de usuario con el tiempo para guiar al usuario final a través de los cambios en la experiencia o notificárselos. En la plataforma de Windows esto se realiza mediante el uso de animaciones basadas en tiempo, también conocidas como KeyFrameAnimations. Las KeyFrameAnimations te permiten cambiar una interfaz de usuario con el tiempo y controlar todos los aspectos de la animación, incluido cómo y cuándo se inicia y cómo llega a su estado final. Por ejemplo, animar un objeto a una nueva posición en 300milisegundos es más agradable que "teletransportarlo" allí de forma instantánea. Cuando se usan animaciones en lugar de cambios instantáneos, el resultado final es una experiencia más agradable y atractiva.

## <a name="types-of-time-based-animations"></a>Tipos de animaciones basadas en tiempo

Existen dos categorías de animaciones basadas en tiempo que puedes usar para crear experiencias de usuario atractivas en Windows:

**Animaciones explícitas**: Como su nombre indica, inicias la animación explícitamente para realizar actualizaciones.
**Animaciones implícitas**: Estas animaciones las activa el sistema por ti cuando se cumple una condición.

En este artículo analizaremos cómo crear y usar animaciones basadas en tiempo _explícitas_ con KeyFrameAnimations.

Tanto para las animaciones basadas en implícitas como para las explícitas existen diferentes tipos, que se corresponden a los distintos tipos de propiedades de CompositionObjects que se pueden animar.

- ColorKeyFrameAnimation
- QuaternionKeyFrameAnimation
- ScalarKeyFrameAnimation
- Vector2KeyFrameAnimation
- Vector3KeyFrameAnimation
- Vector4KeyFrameAnimation

## <a name="create-time-based-animations-with-keyframeanimations"></a>Creación de animaciones basadas en tiempo con KeyFrameAnimations

Antes de describir cómo crear animaciones basadas en tiempo explícitas con KeyFrameAnimations, vamos a revisar algunos conceptos.

- Fotogramas clave: Se trata de la "instantáneas" individuales que una animación irá animando.
  - Se definen como pares de clave y valor. La clave representa el progreso entre 0 y 1,es decir, en qué punto de la duración de la animación tiene lugar esta "instantánea". El otro parámetro representa el valor de la propiedad en este momento.
- Propiedades de KeyFrameAnimation: Opciones de personalización puedes aplicar para satisfacer las necesidades de la interfaz de usuario.
  - DelayTime: El tiempo que transcurre antes del inicio de una animación tras una llamada a StartAnimation.
  - Duration: La duración de la animación.
  - IterationBehavior: recuento o comportamiento repetitivo infinito de una animación.
  - IterationCount: número de veces limitadas que se repetirá una animación de fotogramas clave
  - KeyFrameCount: lectura del número de fotogramas clave en una animación de fotogramas clave determinada
  - StopBehavior: especifica el comportamiento del valor de una propiedad de animación cuando se llama a StopAnimation.
  - Direction: Especifica la dirección de la animación para su reproducción.
- Animation Group: Inicio de varias animaciones al mismo tiempo.
  - Se suele usar cuando se desea animar varias propiedades a la vez.

Para obtener más información, consulta [CompositionAnimationGroup](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionanimationgroup).

Con estos conceptos en mente, vamos a comentar la fórmula general para crear una KeyFrameAnimation:

1. Identifica el CompositionObject y su propiedad respectiva que tengas que animar.
1. Crea una plantilla de tipo KeyFrameAnimation en el compositor que coincida con el tipo de propiedad que quieres animar.
1. Con la plantilla de animación, empezar a agregar fotogramas clave y a definir las propiedades de la animación.
    - Se requiere al menos un fotograma clave (el fotograma clave 100% o 1f).
    - Se recomienda definir también una duración.
1. Una vez que estés listo para ejecutar esta animación, llama a StartAnimation(...) en el CompositionObject, y fija como destino la propiedad que desees animar. Concretamente:
    - `Visual.StartAnimation("targetProperty", CompositionAnimation animation);`
    - `Visual.StartAnimationGroup(AnimationGroup animationGroup);`
1. Si tienes una animación en ejecución y deseas detener la animación o el grupo de animación, puedes usar esta API:
    - `Visual.StopAnimation("targetProperty");`
    - `Visual.StopAnimationGroup(AnimationGroup AnimationGroup);`

Veamos un ejemplo para ver esta fórmula en acción.

## <a name="example"></a>Ejemplo

En este ejemplo quieres animar el desplazamiento de un objeto visual de < 0,0,0 > a < 20,20,20 > durante 1segundo. Además quieres ver que el objeto visual se anime entre estas posiciones 10 veces.

![Animación de los fotogramas clave](images/animation/animated-rectangle.gif)

Primero tenemos que empezar por identificar el CompositionObject y la propiedad que quieres animar. En este caso, el cuadrado rojo lo representa una objeto visual de composición denominado `redSquare`. La animación se inicia desde este objeto.

A continuación, ya que quieres animar la propiedad Offset, deberás crear una Vector3KeyFrameAnimation (Offset es de tipo Vector3). También debes definir los fotogramas clave correspondientes para la KeyFrameAnimation.

```csharp
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
```

Luego vamos a definir las propiedades de la KeyFrameAnimation para describir su duración junto con el comportamiento para animar entre las dos posiciones (actual y < 200,0,0 >) 10 veces.

```csharp
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
```

Por último, para poder ejecutar una animación, debes iniciarla en una propiedad de un CompositionObject.

```csharp
redVisual.StartAnimation("Offset.X", animation);
```

Este es el código completo.

```csharp
private void AnimateSquare(Compositor compositor, SpriteVisual redSquare)
{ 
    Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
    animation.InsertKeyFrame(1f, new Vector3(200f, 0f, 0f));
    animation.Duration = TimeSpan.FromSeconds(2);
    animation.Direction = Windows.UI.Composition.AnimationDirection.Alternate;
    // Run animation for 10 times
    animation.IterationCount = 10;
    visual.StartAnimation("Offset.X", animation);
} 
```
