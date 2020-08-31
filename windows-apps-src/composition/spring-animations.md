---
title: Animaciones de muelle
description: Aprenda a crear experiencias de Spring Motion en sus aplicaciones mediante las API de NaturalMotionAnimation.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: ecfb6fc001fbf42f70d40ee16abc45aa221c0a75
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053875"
---
# <a name="spring-animations"></a>Animaciones de muelle

En el artículo se muestra cómo usar Spring NaturalMotionAnimations.

## <a name="prerequisites"></a>Prerrequisitos

Aquí se supone que está familiarizado con los conceptos descritos en estos artículos:

- [Animaciones de movimiento naturales](natural-animations.md)

## <a name="why-springs"></a>¿Por qué muelles?

Los muelles son una experiencia de movimiento común que hemos experimentado en algún momento en nuestras vidas; desde Slinky Toys hasta las experiencias de la clase física con un bloque asociado con muelles. El movimiento oscilante de un muelle suele mencionar una respuesta emocional playful y lighthearted de las personas que lo ven. Como resultado, el movimiento de un muelle se traduce bien en la interfaz de usuario de la aplicación para aquellos que desean crear una experiencia de movimiento de livelier que "extrae" más al usuario final que una Bézier cúbica tradicional. En estos casos, el movimiento de primavera no solo crea una experiencia de movimiento de livelier, sino que también puede ayudar a atraer la atención al contenido nuevo o que anima actualmente. En función de la marca de la aplicación o del lenguaje de movimiento, la oscilación es más pronunciada y visible, pero en otros casos es más sutil.

![Movimiento con movimiento de la animación Spring ](images/animation/offset-spring.gif)
 ![ con animación Bézier cúbica](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>Usar muelles en la interfaz de usuario

Como se mencionó anteriormente, los muelles pueden ser un movimiento útil para integrarse en la aplicación con el fin de presentar una experiencia de interfaz de usuario muy familiar y playful. El uso común de los muelles en la interfaz de usuario es:

| Descripción del uso de Spring | Ejemplo visual |
| ------------------------ | -------------- |
| Realización de una experiencia de movimiento "pop" y mire livelier. (Animar escala) | ![Movimiento de escala con la animación de Spring](images/animation/scale-spring.gif) |
| Crear una experiencia de movimiento ligeramente más enérgico (animación de desplazamiento) | ![Movimiento de desplazamiento con animación Spring](images/animation/offset-spring.gif) |

En cada uno de estos casos, el movimiento de resorte se puede desencadenar mediante la "primavera" y el oscilar alrededor de un nuevo valor o oscilar alrededor de su valor actual con cierta velocidad inicial.

![Oscilación de la animación Spring](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>Definición del movimiento de muelle

Puede crear una experiencia de Spring mediante las API de NaturalMotionAnimation. En concreto, cree un SpringNaturalMotionAnimation con los métodos Create * en el compositor. A continuación, podrá definir las siguientes propiedades del movimiento:

- DampingRatio: expresa el nivel de amortiguación del movimiento de muelle usado en la animación.

| Valor de ratio de amortiguación | Descripción |
| ------------------- | ----------- |
| DampingRatio = 0 | No atenuado: el muelle oscilará durante mucho tiempo. |
| 0 < DampingRatio < 1 | Subatenuada: el muelle oscilará entre un poco y mucho. |
| DampingRatio = 1 | Criticallydamped: el muelle no llevará a cabo ninguna oscilación. |
| DampingRation > 1 | Sobreatenuado: el muelle alcanzará rápidamente su destino con una deceleración brusca y sin oscilación. |

- Period: el tiempo que tarda el muelle en realizar una oscilación única.
- Valor final o inicial: se definen las posiciones inicial y final del movimiento de muelle (si no se define, el valor inicial o el valor final serán el valor actual).
- Velocidad inicial: velocidad inicial mediante programación para el movimiento.

También puede definir un conjunto de propiedades del movimiento que son iguales que KeyFrameAnimations:

- Comportamiento de DelayTime/Delay
- StopBehavior

En los casos comunes de animación de desplazamiento y escalado/tamaño, el equipo de diseño de Windows recomienda los siguientes valores para DampingRatio y period para diferentes tipos de muelles:

| Propiedad | Muelle normal | Muelle amortiguado | Muelle menos humedecido |
| -------- | ------------- | --------------- | -------------------- |
| Offset | Relación de amortiguación = 0,8 <br/> Período = 50 ms | Relación de amortiguación = 0,85 <br/> Período = 50 ms | Relación de amortiguación = 0,65 <br/> Período = 60 ms |
| Escala/tamaño | Relación de amortiguación = 0,7 <br/> Período = 50 ms | Relación de amortiguación = 0,8 <br/> Período = 50 ms | Relación de amortiguación = 0,6 <br/> Período = 60 ms |

Una vez que haya definido las propiedades, puede pasar el NaturalMotionAnimation de Spring en el método StartAnimation de un CompositionObject o la propiedad Motion de una InteractionTracker InertiaModifier.

## <a name="example"></a>Ejemplo

En este ejemplo, se crea una experiencia de navegación y de interfaz de usuario de lienzo en la que, cuando el usuario hace clic en un botón de expansión, un panel de navegación se anima con un movimiento de oscilación.

![Animación de resorte al hacer clic](images/animation/spring-animation-on-click.gif)

Empiece por definir la animación Spring dentro del evento en el que se hizo clic para cuando aparezca el panel de navegación. A continuación, defina las propiedades de la animación mediante la característica InitialValueExpression para usar una expresión para definir el FinalValue. También puede realizar un seguimiento de si el panel está abierto o no y, cuando esté listo, iniciar la animación.

```csharp
private void Button_Clicked(object sender, RoutedEventArgs e)
{
 _springAnimation = _compositor.CreateSpringScalarAnimation();
 _springAnimation.DampingRatio = 0.75f;
 _springAnimation.Period = TimeSpan.FromSeconds(0.5);

 if (!_expanded)
 {
 _expanded = true;
 _propSet.InsertBoolean("expanded", true);
 _springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue + 250";
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression["FinalValue"] = "this.StartingValue - 250";
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

¿Ahora qué ocurre si desea asociar este movimiento a la entrada? Por lo tanto, si el usuario final se desliza hacia afuera, los paneles tienen un movimiento de muelle? Y lo que es más importante, si el usuario desliza el dedo más duro o más rápido, el movimiento se adapta en función de la velocidad del usuario final.

![Animación Spring al deslizar rápidamente](images/animation/spring-animation-on-swipe.gif)

Para ello, puede realizar la misma animación Spring y pasarla a un InertiaModifier con InteractionTracker. Para obtener más información sobre InputAnimations y InteractionTracker, vea [experiencias de manipulación personalizada con InteractionTracker](interaction-tracker-manipulations.md). En este ejemplo de código, se asumirá que ya ha configurado InteractionTracker y VisualInteractionSource. Nos centraremos en crear el InertiaModifiers que se tomará en un NaturalMotionAnimation, en este caso un muelle.

```csharp
// InteractionTracker and the VisualInteractionSource previously setup
// The open and close ScalarSpringAnimations defined earlier
private void SetupInput()
{
 // Define the InertiaModifier to manage the open motion
 var openMotionModifer = InteractionTrackerInertiaNaturalMotion.Create(compositor);

 // Condition defines to use open animation if panes in non-expanded view
 // Property set value to track if open or closed is managed in other part of code
 openMotionModifer.Condition = _compositor.CreateExpressionAnimation(
"propset.expanded == false");
 openMotionModifer.Condition.SetReferenceParameter("propSet", _propSet);
 openMotionModifer.NaturalMotion = _openSpringAnimation;

 // Define the InertiaModifer to manage the close motion
 var closeMotionModifier = InteractionTrackerInertiaNaturalMotion.Create(_compositor);

 // Condition defines to use close animation if panes in expanded view
 // Property set value to track if open or closed is managed in other part of code
 closeMotionModifier.Condition = 
_compositor.CreateExpressionAnimation("propSet.expanded == true");
 closeMotionModifier.Condition.SetReferenceParameter("propSet", _propSet);
 closeMotionModifier.NaturalMotion = _closeSpringAnimation;

 _tracker.ConfigurePositionXInertiaModifiers(new 
InteractionTrackerInertiaNaturalMotion[] { openMotionModifer, closeMotionModifier});

 // Take output of InteractionTracker and assign to the pane
 var exp = _compositor.CreateExpressionAnimation("-tracker.Position.X");
 exp.SetReferenceParameter("tracker", _tracker);
 ElementCompositionPreview.GetElementVisual(pageNavigation).
StartAnimation("Translation.X", exp);
}
```

Ahora ya tiene una animación de Spring de programación y controlada por entrada en la interfaz de usuario.

En Resumen, los pasos para usar una animación de Spring en la aplicación:

1. Cree su SpringAnimation fuera del compositor.
1. Defina las propiedades de SpringAnimation si desea valores no predeterminados:
    - DampingRatio
    - Período
    - Valor final
    - Valor inicial
    - Velocidad inicial
1. Asignar al destino.
    - Si va a animar una propiedad CompositionObject, pase SpringAnimation como parámetro a StartAnimation.
    - Si desea usar with Input, establezca la propiedad NaturalMotion de un InertiaModifier en SpringAnimation.

