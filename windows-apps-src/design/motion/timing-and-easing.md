---
Description: Obtenga información sobre cómo Fluent usos de movimiento agotar el tiempo y funciones de aceleración.
title: 'Sincronización y aceleración: animación en las aplicaciones para UWP'
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
ms.openlocfilehash: b736a10a7284e3cc9aa193e082dc654e908afe40
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444173"
---
# <a name="timing-and-easing"></a>Sincronización y aceleración

Mientras el movimiento se basa en el mundo real, también somos un medio digital, que viene con una expectativa de velocidad y rendimiento.

## <a name="examples"></a>Ejemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/EasingFunction">abra la aplicación y ver las funciones de aceleración en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Cómo el movimiento de Fluent usa el tiempo

La temporización es un elemento importante para hacer que el movimiento resulte natural para objetos que entren, salgan o se muevan dentro de la interfaz de usuario.

1. Los objetos o las escenas que entran en la vista son rápidas, pero célebres. La duración de estas animaciones suele ser más larga que las salidas para permitir la acumulación jerárquica de una escena.
1. Los objetos o las escenas que salen de la vista son muy rápidos. El usuario debe ser capaz de comprender dónde se fue la interfaz de usuario. Sin embargo, una vez que se descarta la interfaz de usuario, debe apartarse.
1. Los objetos que se traducen en una escena deben tener una duración adecuada para la distancia que recorren.

## <a name="timing-in-fluent-motion"></a>Temporización en movimiento de Fluent

La temporización de movimiento en Fluent usa 500 milisegundos (o medio segundo) como línea base porque es el tiempo máximo que un usuario percibe como instante.

![imagen principal](images/time.gif)

### <a name="150ms-exit"></a>**150 milisegundos** (Salir)

:::row:::
    :::column:::
Uso de objetos o páginas que se sale de la escena o cerrar.
Permite que los comentarios direccionales muy rápidos de la salida de la interfaz de usuario donde la temporización no obstaculiza que la velocidad de fotogramas logre una animación fluida.
    :::column-end:::
    :::column:::
        ![150ms motion](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 milisegundos** (Entrar)

:::row:::
    :::column:::
Uso de objetos o páginas que entran en la escena o abrir.
Permite una cantidad razonable de tiempo para celebrar el contenido conforme entra en la escena.
    :::column-end:::
    :::column:::
        ![300ms motion](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500 milisegundos** (Mover)

:::row:::
    :::column:::
Se utiliza para objetos que se traduce en una sola escena o varias escenas. 
    :::column-end:::
    :::column:::
        ![500ms motion](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Aceleración en movimiento de Fluent

La aceleración es una forma de manipular la velocidad de un objeto conforme se desplaza. Es el aglutinador que une todas las experiencias del movimiento de Fluent. Aunque extrema, la aceleración usada en el sistema permite unificar la apariencia física de los objetos que se mueven por todo el sistema. Esta es una manera para imitar el mundo real y hacer que los objetos en movimiento den la sensación de pertenecer a su entorno.

![imagen principal](images/easing.gif)

## <a name="apply-easing-to-motion"></a>Aplicar la aceleración al movimiento

Estas aceleraciones te ayudarán a lograr una apariencia más natural y son la línea base que usamos para el movimiento de Fluent.

Los ejemplos de código muestran cómo aplicar valores de aceleración recomendados a animaciones de guión gráfico de historia (XAML) o animaciones de composición (C#).

### <a name="accelerate-exit"></a>**Acelerar** (Salida)

:::row:::
    :::column:::
Use la interfaz de usuario u objetos que están saliendo de la escena.

Los objetos se convierten en con la tecnología y obtienen momentum hasta que alcanzan la velocidad de escape.
El comportamiento resultante es que el objeto está tratando su más difícil sacar partido al usuario y dejar espacio para el nuevo contenido al respecto.
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

:::row:::
    :::column:::
Uso de objetos o la interfaz de usuario introducir la escena, navegar o generando.

Una vez en escena, el objeto se cumple con extrema fricción, lo que disminuye el objeto en el resto.
El comportamiento resultante es que el objeto viajado desde una larga distancia y especificado a una velocidad extrema o es volver rápidamente a un estado de rest.

Incluso si se está precedido por un momento de falta de respuesta, la velocidad del objeto entrante tiene el efecto de sentirse rápido y con capacidad de respuesta.
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

:::row:::
    :::column:::
Se trata de la línea de base de aceleración para cualquier cambio animado parámetro dentro del sistema.
Usa la aceleración estándar para los objetos que cambian de un estado a otro en pantalla, como un cambio de posición sencillo. Además, úsalo para objetos que se transforman en escena, como un objeto que crece.

El comportamiento resultante es que son superar objetos cambiando el estado de la A B, y realiza a través, fuerza natural.
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

- [Información general del movimiento](index.md)
- [Direccionalidad y gravedad](directionality-and-gravity.md)
