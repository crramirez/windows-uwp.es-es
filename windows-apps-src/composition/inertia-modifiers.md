---
title: Uso de modificadores de inercia para crear puntos de acoplamiento
description: Obtener más información sobre cómo usar la característica InertiaModifier de InteractionTracker para crear experiencias de movimiento que se acoplen a un punto especificado.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: f99ebc4b98c87a4bc6d77fd2c626f481563e50c5
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7712350"
---
# <a name="create-snap-points-with-inertia-modifiers"></a>Creación de puntos de acoplamiento con modificadores de inercia

En este artículo analizaremos más a fondo cómo usar la característica InertiaModifier de InteractionTracker para crear experiencias de movimiento que se acoplen a un punto especificado.

## <a name="prerequisites"></a>Requisitos previos

En este artículo damos por hecho que estás familiarizado con los conceptos tratados en estos artículos:

- [Animaciones controladas por entradas](input-driven-animations.md)
- [Experiencias de manipulación personalizadas con InteractionTracker](interaction-tracker-manipulations.md)
- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="what-are-snap-points-and-why-are-they-useful"></a>¿Qué son los puntos de acoplamiento y por qué son útiles?

Al crear experiencias de manipulación personalizadas, a veces resulta útil crear _puntos de posición_ especializados en el lienzo ampliable o desplazable en el que InteractionTracker se sitúa siempre. Estos se suelen llamar _puntos de acoplamiento_.

Observa en el siguiente ejemplo cómo el desplazamiento puede dejar la interfaz de usuario en una posición rara entre las diferentes imágenes:

![Desplazamiento sin puntos de acoplamiento](images/animation/snap-points-none.gif)

Si agregas puntos de acoplamiento, cuando el desplazamiento entre las imágenes se detiene, estas se "ajustan" en una posición especificada. Los puntos de acoplamiento hacen que la experiencia de desplazamiento por las imágenes sea mucho más limpia y tenga más capacidad de respuesta.

![Desplazamiento con un solo punto de acoplamiento](images/animation/snap-points-single.gif)

## <a name="interactiontracker-and-inertiamodifiers"></a>InteractionTracker e InertiaModifiers

Al crear experiencias de manipulación personalizadas con InteractionTracker, puedes crear experiencias de movimiento con un punto de acoplamiento mediante el uso de InertiaModifiers. InertiaModifiers son básicamente una forma de definir dónde y cómo InteractionTracker llega a su destino al entrar en el estado de inercia. Puedes aplicar InertiaModifiers para tener influir en la posición X o Y o en las propiedades de escala de InteractionTracker.

Existen 3 tipos de InertiaModifiers:

- InteractionTrackerInertiaRestingValue: Una manera de modificar la posición final después de una interacción o una velocidad controlada por programación. Un movimiento predefinido llevará InteractionTracker a esa posición.
- InteractionTrackerInertiaMotion: Una forma de definir un movimiento específico que InteractionTracker realizará después de una interacción o una velocidad controlada por programación. La posición final se derivará de este movimiento.
- InteractionTrackerInertiaNaturalMotion: Una forma de definir la posición final después de una interacción o una velocidad controlada por programación, pero con una animación basada en física (NaturalMotionAnimation).

Al entrar en estado de inercia, InteractionTracker evalúa todos los InertiaModifiers que tiene asignados y determina si alguno de ellos es aplicable. Esto significa que puedes crear y asignar varios InertiaModifiers a un InteractionTracker, pero al definir cada uno de ellos debes hacer lo siguiente:

1. Definir la condición: Una expresión que defina la instrucción condicional cuando debe realizarse este InertiaModifier específico. A menudo esto requiere atender a la NaturalRestingPosition del InteractionTracker (el destino en función de la inercia predeterminada).
1. Definir el RestingValue/Motion/NaturalMotion: Define la expresión real de Resting Value, Motion o NaturalMotionAnimation que tendrá lugar cuando se cumpla la condición.

> [!NOTE]
> El aspecto de la condición de los InertiaModifiers se evalúa solo cuando InteractionTracker entra en la fase de inercia. Sin embargo, solo para InertiaMotion, el la expresión de Motion se evalúa en cada fotograma para el modificador cuya condición sea true.

## <a name="example"></a>Ejemplo

Ahora veamos cómo puedes usar InertiaModifiers para crear algunas experiencias de punto de acoplamiento para recrear el lienzo con desplazamiento de las imágenes. En este ejemplo, cada manipulación está destinada a moverse potencialmente a través de una sola imagen, lo que se suele conocer como puntos de acoplamiento obligatorios únicos.

Empecemos por configurar InteractionTracker, VisualInteractionSource y la expresión que aprovechará la posición de InteractionTracker.

```csharp
private void SetupInput()
{
    _tracker = InteractionTracker.Create(_compositor);
    _tracker.MinPosition = new Vector3(0f);
    _tracker.MaxPosition = new Vector3(3000f);

    _source = VisualInteractionSource.Create(_root);
    _source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    _source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
    _tracker.InteractionSources.Add(_source);

    var scrollExp = _compositor.CreateExpressionAnimation("-tracker.Position.Y");
    scrollExp.SetReferenceParameter("tracker", _tracker);
    ElementCompositionPreview.GetElementVisual(scrollPanel).StartAnimation("Offset.Y", scrollExp);
}
```

A continuación, ya que el comportamiento de un punto de acoplamiento obligatorio único moverá el contenido hacia arriba o hacia abajo, necesitarás dos modificadores de inercia diferentes: uno que suba el contenido desplazable y otro que lo baje.

```csharp
// Snap-Point to move the content up
var snapUpModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
// Snap-Point to move the content down
var snapDownModifier = InteractionTrackerInertiaRestingValue.Create(_compositor);
```

El acoplamiento hacia arriba o hacia abajo se determina en función de dónde aterrizaría InteractionTracker naturalmente en relación con la distancia de acoplamiento: la distancia entre las ubicaciones de acoplamiento. Si se supera el punto medio, se acopla hacia abajo, y si no, hacia arriba. (En este ejemplo, se almacena la distancia de acoplamiento en un PropertySet).

```csharp
// Is NaturalRestingPosition less than the halfway point between Snap Points?
snapUpModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y < (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapUpModifier.Condition.SetReferenceParameter("prop", _propSet);
// Is NaturalRestingPosition greater than the halfway point between Snap Points?
snapDownModifier.Condition = _compositor.CreateExpressionAnimation(
"this.Target.NaturalRestingPosition.y >= (this.StartingValue – ” + “mod(this.StartingValue, prop.snapDistance) + prop.snapDistance / 2)");
snapDownModifier.Condition.SetReferenceParameter("prop", _propSet);
```

Este diagrama proporciona una descripción visual de la lógica que tiene lugar:

![Diagrama de modificadores de inercia](images/animation/inertia-modifier-diagram.png)

Ahora solo tienes que definir los Resting Values de cada InertiaModifier: mover la posición de InteractionTracker a la posición de acoplamiento anterior o a la siguiente.

```csharp
snapUpModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue - mod(this.StartingValue, prop.snapDistance)");
snapUpModifier.RestingValue.SetReferenceParameter("prop", _propSet);
snapForwardModifier.RestingValue = _compositor.CreateExpressionAnimation(
"this.StartingValue + prop.snapDistance - mod(this.StartingValue, ” + “prop.snapDistance)");
snapForwardModifier.RestingValue.SetReferenceParameter("prop", _propSet);
```

Por último, agrega los InertiaModifiers a InteractionTracker. Ahora cuando InteractionTracker entre en su InertiaState, comprobará las condiciones de su InertiaModifiers para ver si se debe modificar su posición.

```csharp
var modifiers = new InteractionTrackerInertiaRestingValue[] { 
snapUpModifier, snapDownModifier };
_tracker.ConfigurePositionYInertiaModifiers(modifiers);
```