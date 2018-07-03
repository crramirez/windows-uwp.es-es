---
author: jwmsft
Description: Learn how Fluent motion uses timing and easing functions.
title: 'Sincronización y aceleración: animación en las aplicaciones para UWP'
label: Timing and easing
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 412ba7e36c2bb36562ceee13bb1e204ff402a882
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1843947"
---
# <a name="timing-and-easing"></a>Sincronización y aceleración

Mientras el movimiento se basa en el mundo real, también somos un medio digital, que viene con una expectativa de velocidad y rendimiento. 

## <a name="how-fluent-motion-uses-time"></a>Cómo el movimiento de Fluent usa el tiempo

La temporización es un elemento importante para hacer que el movimiento resulte natural para objetos que entren, salgan o se muevan dentro de la interfaz de usuario.

1. Los objetos o las escenas que entran en la vista son rápidas, pero célebres. La duración de estas animaciones suele ser más larga que las salidas para permitir la acumulación jerárquica de una escena.
1. Los objetos o las escenas que salen de la vista son muy rápidos. El usuario debe ser capaz de comprender dónde se fue la interfaz de usuario. Sin embargo, una vez que se descarta la interfaz de usuario, debe apartarse.
1. Los objetos que se traducen en una escena deben tener una duración adecuada para la distancia que recorren.

## <a name="timing-in-fluent-motion"></a>Temporización en movimiento de Fluent

La temporización de movimiento en Fluent usa 500milisegundos (o medio segundo) como línea base porque es el tiempo máximo que un usuario percibe como instante.

![imagen principal](images/time.gif)

### <a name="150ms-exit"></a>**150milisegundos** (Salir)

:::row::: :::column::: Uso para objetos o páginas que salen de la escena o la cierran.
Permite que los comentarios direccionales muy rápidos de la salida de la interfaz de usuario donde la temporización no obstaculiza que la velocidad de fotogramas logre una animación fluida.
:::column-end::: :::column::: ![movimiento de 150milisegundos](images/150msAlt.gif) :::column-end::: :::row-end:::

### <a name="300ms-enter"></a>**300milisegundos** (Entrar)

:::row::: :::column::: Uso para objetos o páginas que entran en la escena o la abren.
Permite una cantidad razonable de tiempo para celebrar el contenido conforme entra en la escena.
:::column-end::: :::column::: ![movimiento de 300milisegundos](images/300ms.gif) :::column-end::: :::row-end:::

### <a name="500ms-move"></a>**≤500milisegundos** (Mover)

:::row::: :::column::: Uso para objetos que se traducen en una sola escena o varias escenas. :::column-end::: :::column::: ![movimiento de 500milisegundos](images/500ms.gif) :::column-end::: :::row-end:::

## <a name="easing-in-fluent-motion"></a>Aceleración en movimiento de Fluent

La aceleración es una forma de manipular la velocidad de un objeto conforme se desplaza. Es el aglutinador que une todas las experiencias del movimiento de Fluent. Aunque extrema, la aceleración usada en el sistema permite unificar la apariencia física de los objetos que se mueven por todo el sistema. Esta es una manera para imitar el mundo real y hacer que los objetos en movimiento den la sensación de pertenecer a su entorno.

![imagen principal](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Aplicar la aceleración al movimiento

Estas aceleraciones te ayudarán a lograr una apariencia más natural y son la línea base que usamos para el movimiento de Fluent.

Los ejemplos de código muestran cómo aplicar valores de aceleración recomendados a animaciones de guión gráfico de historia (XAML) o animaciones de composición (C#).

### <a name="accelerate-exit"></a>**Acelerar** (Salida)

:::row::: :::column::: Uso para interfaz de usuario u objetos que salen de la escena.

        Objects become powered and gain momentum until they reach escape velocity.
        The resulting feel is that the object is trying its hardest to get out of the user's way and make room for new content to come in.
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**Desacelerar** (Introducir)

:::row::: :::column::: Uso para objetos o interfaz de usuario que entran en la escena, ya sea navegando o generando.

        Once on-scene, the object is met with extreme friction, which slows the object to rest.
        The resulting feel is that the object traveled from a long distance away and entered at an extreme velocity, or is quickly returning to a rest state.

        Even if it's preceded by a moment of unresponsiveness, the velocity of the incoming object has the effect of feeling fast and responsive.
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**Aceleración estándar** (Mover)

:::row::: :::column::: Esta es la aceleración de línea base para cualquier cambio de parámetro animado dentro del sistema.
Usa la aceleración estándar para los objetos que cambian de un estado a otro en pantalla, como un cambio de posición sencillo. Además, úsalo para objetos que se transforman en escena, como un objeto que crece.

        The resulting feel is that objects changing state from A to B are overcoming, and taken over by, natural forces.
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>Artículos relacionados

- [Introducción al movimiento](index.md)
- [Direccionalidad y gravedad](directionality-and-gravity.md)