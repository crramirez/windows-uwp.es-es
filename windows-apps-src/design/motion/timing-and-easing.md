---
Description: Obtenga información sobre cómo el movimiento fluida usa las funciones de tiempo y de aceleración.
title: Sincronización y aceleración
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 098a75da573a977aa393197a61a62b0337f0dc06
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970510"
---
# <a name="timing-and-easing"></a>Sincronización y aceleración

Aunque el movimiento se basa en el mundo real, también somos un medio digital, que viene con una expectativa de velocidad y rendimiento.

## <a name="examples"></a>Ejemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene instalada la aplicación de <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> , haga clic aquí para <a href="xamlcontrolsgallery:/item/EasingFunction">abrir la aplicación y ver las funciones de aceleración en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Cómo el movimiento fluida usa el tiempo

El control de tiempo es un elemento importante para hacer que el movimiento parezca natural en los objetos que entran, salen o se mueven dentro de la interfaz de usuario.

1. Los objetos o escenas que entran en la vista son rápidos, pero celebrated. Estas animaciones suelen durar más tiempo que las salidas para permitir la creación jerárquica de una escena.
1. Los objetos o las escenas que salen de la vista son muy rápidas. El usuario debe ser capaz de comprender dónde fue la interfaz de usuario. Sin embargo, una vez descartada la interfaz de usuario, debería salir del camino.
1. Los objetos que se traducen en una escena deben tener una duración adecuada para la cantidad de distancia que viajan.

## <a name="timing-in-fluent-motion"></a>Temporización en movimiento fluida

El tiempo del movimiento en Fluent usa 500 ms (o una segunda mitad) como línea base, ya que se trata de la cantidad máxima de tiempo que un usuario percibe como instantáneo.

![imagen principal](images/time.gif)

### <a name="150ms-exit"></a>**150MS** (salir)

:::row:::
    :::column:::
Se usa para los objetos o las páginas que salen de la escena o el cierre.
Permite una retroalimentación muy rápida de la salida de la interfaz de usuario, donde el tiempo no impide la velocidad de fotogramas para lograr una animación fluida.
    :::column-end:::
    :::column:::
        ![movimiento 150MS](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 ms** (Enter)

:::row:::
    :::column:::
Se usa para los objetos o las páginas que entran en la escena o se abren.
Permite un período de tiempo razonable para celebrar el contenido a medida que entra en la escena.
    :::column-end:::
    :::column:::
        ![movimiento 300 ms](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤ 500 ms** (Move)

:::row:::
    :::column:::
Se usa para los objetos que se traducen en una sola escena o en varias escenas. 
    :::column-end:::
    :::column:::
        ![movimiento 500 ms](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Aceleración en movimiento fluida

La aceleración es una manera de manipular la velocidad de un objeto a medida que se desplaza. Es el pegado que reúne todas las experiencias de movimiento fluida. Mientras que Extreme, la aceleración utilizada en el sistema ayuda a unificar la sensación física de los objetos que se mueven por todo el sistema. Esta es una manera de imitar el mundo real y hacer que los objetos en movimiento parezcan que pertenecen a su entorno.

![imagen principal](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Aplicar aceleración al movimiento

Estas entradas y salidas lentas le ayudarán a lograr un funcionamiento más natural y son la línea base que usamos para el movimiento fluida.

En los ejemplos de código se muestra cómo aplicar los valores de aceleración recomendados a las animaciones de guion gráfico (XAML) o a las animaciones de composición (C#).

### <a name="accelerate-exit"></a>**Acelerar** (salir)

:::row:::
    :::column:::
Se usa para la interfaz de usuario u objetos que salen de la escena.

Los objetos se encienden y obtienen impulsos hasta que alcanzan la velocidad de escape.
El resultado es que el objeto está intentando sacar provecho del camino del usuario y dejar espacio para que el nuevo contenido se ponga en marcha.
    :::column-end:::
    :::column:::
        ![acelere la aceleración](images/accelEase.gif)
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

### <a name="decelerate-enter"></a>**Deceleración** (entrar)

:::row:::
    :::column:::
Use para objetos o la interfaz de usuario que entra en la escena, navegando o generando.

Una vez en la escena, el objeto se cumple con la fricción extrema, lo que ralentiza el objeto.
El resultado es que el objeto viaja desde una distancia larga y entra en una velocidad extrema, o bien se vuelve rápidamente a un estado de REST.

Incluso si está precedido por un momento de falta de respuesta, la velocidad del objeto entrante tiene el efecto de la sensación rápida y la capacidad de respuesta.
    :::column-end:::
    :::column:::
        ![deceleración lenta](images/decelEase.gif)
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

### <a name="standard-easing-move"></a>**Aceleración estándar** (movimiento)

:::row:::
    :::column:::
Esta es la aceleración de línea de base para cualquier cambio de parámetro animado dentro del sistema.
Use la aceleración estándar para los objetos que cambian de estado en pantalla, como un cambio de posición simple. Además, Úsela para objetos que se transforman en escena, como un objeto que crece.

El resultado es que los objetos que cambian el estado de a a B están llegando y se han tomado por las fuerzas naturales.
    :::column-end:::
    :::column:::
        ![aceleración estándar](images/standardEase.gif)
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

- [Información general sobre movimiento](index.md)
- [Direccionalidad y gravedad](directionality-and-gravity.md)
