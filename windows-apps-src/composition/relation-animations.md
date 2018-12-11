---
title: Animaciones basadas en relaciones
description: Crea un movimiento basado en una propiedad de otro objeto.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: b6fdc59e8a7203a3bb8c6ad79adabd446b884639
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8926301"
---
# <a name="relation-based-animations"></a>Animaciones basadas en relaciones

En este artículo se proporciona una breve introducción a cómo realizar animaciones basadas en relaciones con ExpressionAnimations de Composición.

## <a name="dynamic-relation-based-experiences"></a>Experiencias basadas en relaciones dinámicas

Al crear experiencias de movimiento en una aplicación, a veces el movimiento no depende del tiempo sino de una propiedad de otro objeto. Las KeyFrameAnimations no son capaces de expresar estos tipos de experiencias de movimiento con sencillez. En estos casos concretos, ya no es necesario que el movimiento sea diferenciado y esté predefinido. En su lugar, el movimiento se puede adaptar de forma dinámica en función de su relación con otras propiedades de objetos. Por ejemplo, puedes animar la opacidad de un objeto en base a su posición horizontal. Otros ejemplos incluyen experiencias de movimiento como los encabezados permanentes y el efecto parallax.

Estos tipos de experiencias de movimiento permiten crear una interfaz de usuario con mayor conexión y que no parezca separada e independiente. Para el usuario, esto provoca la impresión de una experiencia de interfaz de usuario dinámica.

![Círculo en órbita](images/animation/orbit.gif)

![Vista de lista con efecto parallax](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>Uso de ExpressionAnimations

Para crear experiencias de movimiento basadas en relaciones, se usa el tipo ExpressionAnimation. Las ExpressionAnimations (o expresiones, para abreviar) son un nuevo tipo de animación que permite expresar una relación matemática: una relación que el sistema usa para calcular el valor de una propiedad de animación en cada fotograma. Dicho de otro modo, las expresiones son simplemente una ecuación matemática que define el valor deseado de una propiedad de animación en cada fotograma. Las expresiones son un componente muy versátil que se puede usar en una amplia variedad de escenarios, que incluyen:

- Animaciones de desplazamiento y de tamaño relativo.
- Encabezados permanentes y efecto parallax con ScrollViewer. (Consulta [Mejorar las experiencias existentes de ScrollViewer](scroll-input-animations.md)).
- Puntos de acoplamiento con InertiaModifiers e InteractionTracker. (Consulta [Creación de puntos de acoplamiento con modificadores de inercia](inertia-modifiers.md)).

Al trabajar con ExpressionAnimations, hay un par de cosas que merece la pena mencionar por adelantado:

- Sin finalización: a diferencia de su pariente, la KeyFrameAnimation, la duración de las expresiones no está limitada. Como las expresiones son relaciones matemáticas, son animaciones que se "ejecutan" constantemente. Tienes la opción de detener estas animaciones cuando decidas.
- Siempre en ejecución, pero no siempre se evalúan: en el caso de las animaciones que se ejecutan constantemente, el rendimiento siempre es un problema. Pero no es necesario preocuparse, el sistema es lo suficientemente inteligente para que la expresión solo se vuelva a evaluar si cambian cualquiera de sus entradas o parámetros.
- Resolución en el tipo de objeto adecuado: como expresiones son relaciones matemáticas, es importante asegurarse de que la ecuación que define la expresión se resuelve en el mismo tipo de la propiedad que es el objetivo de la animación. Por ejemplo, si se anima el desplazamiento, la expresión debería resolverse en un tipo Vector3.

### <a name="components-of-an-expression"></a>Componentes de una expresión

Al crear la relación matemática de una expresión, existen varios componentes principales:

- Parámetros: Valores que representan valores de constantes o referencias a otros objetos de composición.
- Operadores matemáticos: los operadores matemáticos normales más (+), menos (-), multiplicar (*), dividir (/) que unen parámetros para formar una ecuación. También se incluyen operadores condicionales, como mayor que (>), igual l(==), operador ternario (condición ? ifTrue : ifFalse), etc.
- Funciones matemáticas: Funciones matemáticas y métodos abreviados basados en System.Numerics. Para obtener una lista completa de las funciones admitidas, consulta [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

Las expresiones también admiten un conjunto de palabras clave: frases especiales que tienen un significado diferenciado únicamente en el sistema de ExpressionAnimation. Se muestran (junto con la lista completa de funciones matemáticas) en la documentación de [ExpressionAnimation](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ExpressionAnimation).

### <a name="creating-expressions-with-expressionbuilder"></a>Creación de expresiones con ExpressionBuilder

Existen dos opciones para crear expresiones en la aplicación para UWP:

1. Crear la ecuación como una cadena a través de la API pública oficial.
1. Crear la ecuación en un modelo de objetos con seguridad de tipos mediante la herramienta ExpressionBuilder de código abierto. Consulta [el código fuente y la documentación de Github](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/ExpressionBuilder).

Para este documento, definiremos nuestras expresiones mediante ExpressionBuilder.

### <a name="parameters"></a>Parámetros

Los parámetros constituyen el núcleo de una expresión. Hay dos tipos de parámetros:

- Constantes: Los parámetros que representan variables System.Numeric con tipo. A estos parámetros se les asignan sus valores una vez iniciada la animación.
- Referencias: Los parámetros que representan las referencias a CompositionObjects; los valores de estos parámetros se actualizan continuamente después de que se inicie una animación.

En general, las referencias son el aspecto principal en el modo en que la salida de una expresión puede cambiar dinámicamente. A medida que cambian estas referencias, también cambia el resultado de la expresión. Si creas una expresión con cadenas o las usas en un escenario de plantillas (usando la expresión para dirigirte a varios CompositionObjects), deberás asignar un nombre a los parámetros y establecer sus valores. Consulta la sección Ejemplos para obtener más información.

### <a name="working-with-keyframeanimations"></a>Trabajar con KeyFrameAnimations

Las expresiones también pueden usarse con KeyFrameAnimations. En estos casos, es útil usar una expresión para definir el valor de un fotograma clave en un punto del tiempo; estos tipos de fotogramas clave se denominan ExpressionKeyFrames.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

Sin embargo, a diferencia de las ExpressionAnimations, los ExpressionKeyFrames se evalúan solo una vez cuando se inicia la KeyFrameAnimation. Recuerda no pasar una ExpressionAnimation como el valor del fotograma clave en lugar de una cadena (o un ExpressionNode, si usas ExpressionBuilder).

## <a name="example"></a>Ejemplo

Ahora veamos un ejemplo del uso de expresiones, específicamente la muestra PropertySet de la Galería de ejemplos de la interfaz de usuario de Windows. Echemos un vistazo a la expresión que administra el comportamiento de movimiento orbital de la pelota azul.

![Círculo en órbita](images/animation/orbit.gif)

Para crear la experiencia total participan tres componentes:

1. Una KeyFrameAnimation que anima el desplazamiento Y de la pelota roja.
1. Un PropertySet con una propiedad **Rotation** que ayuda a controlar la órbita, animada por otra KeyFrameAnimation.
1. Una ExpressionAnimation que controla el desplazamiento de la pelota azul en referencia al desplazamiento de la pelota roja y la propiedad Rotation para mantener una órbita perfecta.

Nos centraremos en la ExpressionAnimation definida en el punto 3. También usaremos las clases de ExpressionBuilder para construir esta expresión. Al final aparece una copia del código usado para crear esta experiencia a través de cadenas.

En esta ecuación hay dos propiedades a las que debes hacer referencia desde el PropertySet; una es un desplazamiento del punto central y la otra es la rotación.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

A continuación debes definir el componente de Vector3 que representa el giro en órbita real.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` es una notación abreviada para definir ExpressionBuilder.ExpressionFunctions.

Por último, combina estos componentes y haz referencia a la posición de la pelota roja para definir la relación matemática.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

En una situación hipotética, ¿qué ocurriría si desearas usar esta misma expresión pero con 2 objetos visuales distintos, que son 2 conjuntos de círculos en órbita. Con CompositionAnimations, puedes volver a usar la animación y dirigirte a varios CompositionObjects. Lo único que tendrás que cambiar al usar esta expresión para el caso de la órbita adicional es la referencia al objeto visual. A esto lo llamamos aplicación de plantillas.

En este caso se modifica la expresión creada anteriormente. En lugar de "obtener" una referencia para el CompositionObject, se crea una referencia con un nombre y luego se le asignan valores diferentes:

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

Este es el código si has definido la expresión con cadenas a través de la API pública.

```
ExpressionAnimation expressionAnimation =
compositor.CreateExpressionAnimation("visual.Offset + " +
"propertySet.CenterPointOffset + " +
"Vector3(cos(ToRadians(propertySet.Rotation)) * 150," + "sin(ToRadians(propertySet.Rotation)) * 75, 0)");
 var propSetCenterPoint = _propertySet.GetReference().GetVector3Property("CenterPointOffset");
 var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
expressionAnimation.SetReferenceParameter("propertySet", _propertySet);
expressionAnimation.SetReferenceParameter("visual", redSprite);
```
