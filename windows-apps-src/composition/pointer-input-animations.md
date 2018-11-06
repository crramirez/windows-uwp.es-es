---
author: jwmsft
title: Animaciones basadas en puntero
description: Aprende cómo usar la posición de un puntero para crear experiencias dinámicas "fijadas al cursor".
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: b69899761e1c4a139fd2b15d6810440df5192487
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6048854"
---
# <a name="pointer-based-animations"></a>Animaciones basadas en puntero

Este artículo muestra cómo usar la posición de un puntero para crear experiencias de dinámicas "fijadas al cursor".

## <a name="prerequisites"></a>Requisitos previos

En este artículo damos por hecho que estás familiarizado con los conceptos tratados en estos artículos:

- [Animaciones controladas por entradas](input-driven-animations.md)
- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="why-create-pointer-position-driven-experiences"></a>Por qué crear experiencias controladas por la posición del puntero

En el lenguaje de diseño Fluent Design, las entradas táctiles no son la única forma de interactuar con la interfaz de usuario. Como la UWP se expande a través de los factores de forma de diversos dispositivos, los usuarios finales interactúan con aplicaciones mediante otras modalidades de entrada, como el ratón y el lápiz. El uso de los datos de posición de estas otras modalidades de entrada proporciona la oportunidad para que los usuarios finales se sienta aún más conectados con tu aplicación.

Las experiencias controladas por la posición del puntero te permiten aprovechar la posición en pantalla de una modalidad de entrada de puntero para crear movimientos y experiencias de la interfaz de usuario adicionales para tu aplicación. Estas experiencias a menudo pueden proporcionar un contexto adicional y comentarios a los usuarios finales sobre el comportamiento y la estructura de la interfaz de usuario. La experiencia ya no es una secuencia unidireccional, sino que empieza a ser una corriente bidireccional en la que el usuario final proporciona entradas con su modalidad de entrada y interfaz de usuario de la aplicación puede responder a ellas.

Algunos ejemplos incluyen:

- Animación de la posición de un foco de luz para seguir el cursor

    ![Ejemplo de un foco de luz del puntero](images/animation/spotlight-reveal.gif)

- Rotación de una imagen en función de la posición del puntero

    ![Ejemplo de rotación del puntero](images/animation/pointer-rotate.gif)

## <a name="using-pointerpositionpropertyset"></a>Uso de PointerPositionPropertySet

Puede crear estas experiencias mediante PointerPositionPropertySet. Este PropertySet se crea para que un UIElement mantenga la posición del puntero mientras UIElement se somete a pruebas de posicionamiento. El valor de posición es relativo al espacio de coordenadas del UIElement (una posición de < 0,0 > es la esquina superior izquierda del UIElement). Luego puedes aprovechar este conjunto de propiedades en una animación para controlar el movimiento de otra propiedad.

Para cada una de las diferentes modalidades de entrada de puntero hay diversos estados de entrada en los que la entrada podría estar donde cambia la posición: Hover (Mantener el puntero), Pressed (Presionado) y and Pressed & Moved (Presionado y movido). PointerPositionPropertySet solo mantiene la posición del puntero en los estados de movimiento del ratón y el lápiz Hover, Pressed y Pressed and Moved.

Pasos generales para empezar:

1. Identifica el UIElement en el que quieras que se realice el seguimiento de la posición del puntero.
1. Accede a PointerPositionPropertySet mediante ElementCompositionPreview.
    - Pasa el UIElement al método ElementCompositionPreview.GetPointerPositionPropertySet.
1. Crea una ExpressionAnimation que haga referencia a la propiedad Position de PropertySet.
    - No olvides establecer el parámetro de referencia.
1. Dirígete a una propiedad de CompositionObject con la ExpressionAnimation.

> [!NOTE]
> Se recomienda asignar el PropertySet devuelto desde el método GetPointerPositionPropertySet a una variable de clase. Esto garantiza que la recolección de elementos no utilizados no limpie el conjunto de propiedades y, por consiguiente, no tenga ningún efecto en la ExpressionAnimation en la que se le hace referencia. Las ExpressionAnimations no mantienen ninguna referencia fuerte con ningún objeto usado en la ecuación.

## <a name="example"></a>Ejemplo

Veamos un ejemplo donde aprovechamos la posición Hover de una modalidad de entrada de lápiz y ratón para girar una imagen de forma dinámica.

![Ejemplo de rotación del puntero](images/animation/pointer-rotate.gif)

La imagen es un UIElement, así que primero vamos a obtener una referencia a PointerPositionPropertySet

```csharp
_pointerPositionPropSet = ElementCompositionPreview.GetPointerPositionPropertySet(UIElement element);
```

En este ejemplo, hay dos expresiones en funcionamiento:

- Una expresión en la que la imagen gira en función de lo lejos que el puntero está del centro de la imagen. Cuanto más alejado, mayor será el giro.
- Una expresión en la que el eje de rotación cambia en función de la posición del puntero. Lo recomendable es que el eje de rotación sea perpendicular al vector de la posición.

Definamos las dos expresiones, una que apunte a la propiedad RotationAngle y la otra a la propiedad RotationAxis. Haz referencia a PointerPositionPropertySet como a cualquier otro PropertySet.

En este ejemplo, crearás las expresiones mediante las clases de ExpressionBuilder.

```csharp
// || DEFINE THE EXPRESSION FOR THE ROTATION ANGLE ||
var hoverPosition = _pointerPositionPropSet.GetSpecializedReference
<PointerPositionPropertySetReferenceNode>().Position;
var angleExpressionNode =
EF.Conditional(
 hoverPosition == new Vector3(0, 0, 0),
 ExpressionValues.CurrentValue.CreateScalarCurrentValue(),
 35 * ((EF.Clamp(EF.Distance(center, hoverPosition), 0, distanceToCenter) % distanceToCenter) / distanceToCenter));
_tiltVisual.StartAnimation("RotationAngleInDegrees", angleExpressionNode);

// || DEFINE THE EXPRESSION FOR THE ROTATION AXIS ||
var axisAngleExpressionNode = EF.Vector3(
-(hoverPosition.Y - center.Y) * EF.Conditional(hoverPosition.Y == center.Y, 0, 1),
 (hoverPosition.X - center.X) * EF.Conditional(hoverPosition.X == center.X, 0, 1),
 0);
_tiltVisual.StartAnimation("RotationAxis", axisAngleExpressionNode);
```