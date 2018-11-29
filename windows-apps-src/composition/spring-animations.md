---
title: Animaciones de muelle
description: Aprende a usar animaciones de muelle con movimiento natural.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: 9e00aa383bcce17b7cd6b67514647c2f6137cc32
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7974655"
---
# <a name="spring-animations"></a>Animaciones de muelle

En este artículo se muestra cómo usar NaturalMotionAnimations de muelle.

## <a name="prerequisites"></a>Requisitos previos

En este artículo damos por hecho que estás familiarizado con los conceptos tratados en estos artículos:

- [Animaciones de movimientos naturales](natural-animations.md)

## <a name="why-springs"></a>¿Por qué los muelles?

Los muelles son una experiencia de movimiento común que todos hemos experimentado en algún momento en nuestras vidas; desde juguetes ondulantes a experimentos en clase de física con un bloque atado a un muelle. El movimiento oscilante de un muelle a menudo provoca una respuesta emocional alegre y desenfadada de quienes lo ven. Como resultado, el movimiento de un muelle se traslada bien a la interfaz de usuario de la aplicación para quienes quieren crear una experiencia de movimiento más alegre que sorprenda más al usuario final que un Cubic Bezier tradicional. En estos casos, el movimiento del muelle no solo crea una experiencia de movimiento más alegre, sino que también puede ayudar a llamar la atención respecto a un contenido nuevo o que se anime en ese momento. En función el idioma de personalización de marca o movimiento de la aplicación, la oscilación es más pronunciada y visible, pero en otros casos es más sutil.

![Movimiento con animación de muelle](images/animation/offset-spring.gif)
![Movimiento con animación de Cubic Bezier](images/animation/offset-cubic-bezier.gif)

## <a name="using-springs-in-your-ui"></a>Uso de muelles en la interfaz de usuario

Como se ha mencionado anteriormente, los muelles pueden ser un movimiento útil que integrar en la aplicación para presentar una experiencia de la interfaz de usuario familiar y divertida. Los usos habituales de los muelles en una interfaz de usuario son:

| Descripción de uso de los muelles | Ejemplo visual |
| ------------------------ | -------------- |
| Crear una experiencia de movimiento "sorprendente" y más alegre. (Animación de la escala) | ![Escalar el movimiento con una animación de muelle](images/animation/scale-spring.gif) |
| Hacer que una experiencia de movimiento parezca sutilmente más dinámica. (Animación del desplazamiento) | ![Desplazar el movimiento con una animación de muelle](images/animation/offset-spring.gif) |

En todos estos casos, el movimiento de muelle se puede desencadenar mediante el salto a un nuevo valor y la oscilación alrededor de él o mediante una oscilación alrededor de su valor actual con una cierta velocidad inicial.

![Oscilación de la animación de muelle](images/animation/spring-animation-diagram.png)

## <a name="defining-your-spring-motion"></a>Definición del movimiento de muelle

Para crear una experiencia de muelle, se usan las API NaturalMotionAnimation. Específicamente se crea una SpringNaturalMotionAnimation mediante los métodos Create* del Compositor. Luego es posible definir las siguientes propiedades del movimiento:

- DampingRatio: Expresa el nivel de amortiguación del movimiento de muelle usado en la animación.

| Valor de la relación de atenuación | Descripción |
| ------------------- | ----------- |
| DampingRatio = 0 | Sin atenuación: El muelle oscilará durante mucho tiempo. |
| 0 < DampingRatio < 1 | Sin atenuación: El muelle oscilará entre poco tiempo y mucho tiempo. |
| DampingRatio = 1 | Con mucha atenuación: El muelle no realizará ninguna oscilación. |
| DampingRation > 1 | Sobreatenuación: El muelle llegará rápidamente a su destino con una desaceleración brusca y sin oscilación. |

- Period: El tiempo que tarda el muelle en llevar a cabo una única oscilación.
- Final/Starting Value: Las posiciones definidas inicial y final del movimiento del muelle (si no se definen, el valor inicial o el valor final serán el valor actual).
- Initial Velocity: La velocidad inicial del movimiento controlada por programación.

También puedes definir un conjunto de propiedades del movimiento que sean iguales que KeyFrameAnimations:

- DelayTime/Delay Behavior
- StopBehavior

En los casos habituales de la animación del desplazamiento y de la escala y el tamaño, el equipo de diseño de Windows recomienda los siguientes valores por para DampingRatio y Period para diferentes tipos de muelles:

| Propiedad | Muelle normal | Muelle atenuado | Muelle menos atenuado |
| -------- | ------------- | --------------- | -------------------- |
| Offset | DampingRatio = 0.8 <br/> Period = 50ms | DampingRatio = 0.85 <br/> Period = 50ms | DampingRatio = 0.65 <br/> Period = 60ms |
| Scale/Size | DampingRatio = 0.7 <br/> Period = 50ms | DampingRatio = 0.8 <br/> Period = 50ms | DampingRatio = 0.6 <br/> Period = 60ms |

Una vez que hayas definido las propiedades, puedes pasar la NaturalMotionAnimation de tu muelle en el método StartAnimation de un CompositionObject o en la propiedad Motion de InertiaModifier de un InteractionTracker.

## <a name="example"></a>Ejemplo

En este ejemplo, crearás una experiencia de interfaz de usuario de navegación y lienzo en la que, cuando el usuario haga clic en un botón de expansión, un panel de navegación se animará con un movimiento oscilación tipo muelle.

![Animación de muelle al hacer clic](images/animation/spring-animation-on-click.gif)

Empieza por definir la animación de muelle en el evento de clic para cuando se abra el panel de navegación. A continuación define las propiedades de la animación utilizando la característica de InitialValueExpression para emplear una expresión para definir FinalValue. También realizas el seguimiento de si el panel se abre o no y, cuando esté listo, si se inicia la animación.

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
 _springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue + 250”;
 } else
 {
 _expanded = false;
 _propSet.InsertBoolean("expanded", false);
_springAnimation.InitialValueExpression[“FinalValue”] = “this.StartingValue - 250”;
 }
 _naviPane.StartAnimation("Offset.X", _springAnimation);
}
```

¿Y si quisieras vincular este movimiento a una entrada? Por lo tanto, si el usuario final desliza el dedo hacia fuera, ¿los paneles aparecen con un movimiento de muelle? Y lo que más importante, si el usuario desliza el dedo con más o menos velocidad o fuerza, ¿el movimiento se adapta a la velocidad del usuario final?

![Animación de muelle al deslizar el dedo](images/animation/spring-animation-on-swipe.gif)

Para ello, puedes tomar la misma animación de muelle y pasarla a un InertiaModifier con InteractionTracker. Para obtener más información sobre InputAnimations e InteractionTracker, consulta [Experiencias de manipulación personalizadas con InteractionTracker](interaction-tracker-manipulations.md). Para este ejemplo de código vamos a suponer que ya has configurado InteractionTracker y VisualInteractionSource. Nos centraremos en crear los InertiaModifiers que tomarán una NaturalMotionAnimation, en este caso un muelle.

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

Ahora en la interfaz de usuario tienes una animación de muelle tanto controlada por programación como controlada por entradas.

En resumen, los pasos para usar una animación de muelle en la aplicación son:

1. Crea tu SpringAnimation en el compositor.
1. Define las propiedades de la SpringAnimation si deseas valores que no sean los predeterminados:
    - DampingRatio
    - Period
    - Final Value
    - Initial Value
    - Initial Velocity
1. Asigna a un destino.
    - Si vas a animar una propiedad de CompositionObject, pasa SpringAnimation como parámetro a StartAnimation.
    - Si quieres usarla con la entrada, establece la propiedad de NaturalMotion de un InertiaModifier como SpringAnimation.

