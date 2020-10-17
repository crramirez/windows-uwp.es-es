---
title: Animaciones basadas en relaciones
description: Aprenda a usar ExpressionAnimations para crear animaciones basadas en relaciones cuando el movimiento depende de una propiedad de otro objeto.
ms.date: 10/16/2020
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: 75adcd2f762fd4314d7b852811760d523ef522aa
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152417"
---
# <a name="relation-based-animations"></a>Animaciones basadas en relaciones

En este artículo se proporciona una breve introducción sobre cómo crear animaciones basadas en relaciones mediante ExpressionAnimations de composición.

## <a name="dynamic-relation-based-experiences"></a>Experiencias basadas en relaciones dinámicas

Al crear experiencias de movimiento en una aplicación, hay veces en las que el movimiento no se basa en el tiempo, sino que depende de una propiedad en otro objeto. KeyFrameAnimations no pueden expresar fácilmente estos tipos de experiencias de movimiento. En estas instancias específicas, el movimiento ya no necesita ser discreto y predefinido. En su lugar, el movimiento puede adaptarse dinámicamente en función de su relación con otras propiedades del objeto. Por ejemplo, puede animar la opacidad de un objeto en función de su posición horizontal. Otros ejemplos incluyen experiencias de movimiento como encabezados permanentes y Parallax.

Estos tipos de experiencias de movimiento le permiten crear una interfaz de usuario que está más conectada, en lugar de sentirse singular e independiente. Para el usuario, esto da la impresión de una experiencia dinámica de IU.

![Círculo en órbita](images/animation/orbit.gif)

![Vista de lista con Parallax](images/animation/parallax.gif)

## <a name="using-expressionanimations"></a>Usar ExpressionAnimations

Para crear experiencias de movimiento basadas en relaciones, se usa el tipo ExpressionAnimation. ExpressionAnimations (o Expresiones abreviadas), son un nuevo tipo de animación que permite expresar una relación matemática: una relación que el sistema utiliza para calcular el valor de una propiedad de animación en cada fotograma. En otras palabras, las expresiones son simplemente una ecuación matemática que define el valor deseado de una propiedad de animación por fotograma. Las expresiones son un componente muy versátil que se puede usar en una amplia variedad de escenarios, entre los que se incluyen:

- Tamaño relativo, animaciones de desplazamiento.
- Encabezados permanentes, Parallax con ScrollViewer. (Vea [mejorar las experiencias de ScrollViewer existentes](scroll-input-animations.md)).
- Puntos de acoplamiento con InertiaModifiers y InteractionTracker. (Consulte [creación de puntos de acoplamiento con modificadores de inercia](inertia-modifiers.md)).

Al trabajar con ExpressionAnimations, hay un par de cosas que merece la pena mencionar:

- Nunca termina: a diferencia de su elemento del mismo nivel KeyFrameAnimation, las expresiones no tienen una duración finita. Dado que las expresiones son relaciones matemáticas, son animaciones que se ejecutan constantemente. Tiene la opción de detener estas animaciones si lo prefiere.
- En ejecución, pero no siempre evaluando, el rendimiento siempre es un problema con las animaciones que se están ejecutando constantemente. Sin necesidad de preocuparse, el sistema es lo suficientemente inteligente como para que la expresión se vuelva a evaluar solo si alguna de sus entradas o parámetros ha cambiado.
- Resolver el tipo de objeto correcto: dado que las expresiones son relaciones matemáticas, es importante asegurarse de que la ecuación que define la expresión se resuelve en el mismo tipo de la propiedad de destino de la animación. Por ejemplo, si se anima el desplazamiento, la expresión se debe resolver como un tipo Vector3.

### <a name="components-of-an-expression"></a>Componentes de una expresión

Al generar la relación matemática de una expresión, hay varios componentes principales:

- Parámetros: valores que representan valores constantes o referencias a otros objetos de composición.
- Operadores matemáticos: los operadores matemáticos más (+), menos (-), multiplicación (*), división (/) que se combinan con los parámetros para formar una ecuación. También se incluyen operadores condicionales como mayor que (>), igual (= =), operador ternario (condición? ifTrue: ifFalse), etc.
- Funciones matemáticas: funciones o accesos directos basados en System. Numerics. Para obtener una lista completa de las funciones admitidas, vea [ExpressionAnimation](/uwp/api/Windows.UI.Composition.ExpressionAnimation).

Las expresiones también admiten un conjunto de palabras clave: frases especiales que tienen un significado distinto solo dentro del sistema ExpressionAnimation. Estos se enumeran (junto con la lista completa de funciones matemáticas) en la documentación de [ExpressionAnimation](/uwp/api/Windows.UI.Composition.ExpressionAnimation) .

### <a name="creating-expressions-with-expressionbuilder"></a>Crear expresiones con ExpressionBuilder

Hay dos opciones para compilar expresiones en la aplicación para UWP:

1. Cree la ecuación como una cadena a través de la API pública oficial.
1. Cree la ecuación en un modelo de objetos con seguridad de tipos a través de la herramienta ExpressionBuilder incluida en el kit de herramientas de la [comunidad de Windows](/windows/communitytoolkit/animations/expressions).

En este documento, se definirán las expresiones mediante ExpressionBuilder.

### <a name="parameters"></a>Parámetros

Los parámetros componen el núcleo de una expresión. Hay dos tipos de parámetros:

- Constantes: son parámetros que representan variables System. Numeric con tipo. Estos parámetros obtienen sus valores asignados una vez cuando se inicia la animación.
- Referencias: son parámetros que representan referencias a CompositionObjects: estos parámetros obtienen continuamente sus valores actualizados después de que se inicie una animación.

En general, las referencias son el aspecto principal del modo en que la salida de una expresión puede cambiar dinámicamente. A medida que estas referencias cambian, el resultado de la expresión cambia como resultado. Si crea la expresión con cadenas o las usa en un escenario de plantillas (mediante la expresión para tener como destino varios CompositionObjects), deberá asignar un nombre y establecer los valores de los parámetros. Consulta la sección Ejemplos para obtener más información.

### <a name="working-with-keyframeanimations"></a>Trabajar con KeyFrameAnimations

Las expresiones también se pueden utilizar con KeyFrameAnimations. En estos casos, desea utilizar una expresión para definir el valor de un fotograma clave en un momento dado: estos tipos fotogramas clave se denominan ExpressionKeyFrames.

```csharp
KeyFrameAnimation.InsertExpressionKeyFrame(Single, String)
KeyFrameAnimation.InsertExpressionKeyFrame(Single, ExpressionNode)
```

Sin embargo, a diferencia de ExpressionAnimations, ExpressionKeyFrames solo se evalúa una vez cuando se inicia KeyFrameAnimation. Tenga en cuenta que no pasa un ExpressionAnimation como valor del fotograma clave, en lugar de una cadena (o un ExpressionNode, si está usando ExpressionBuilder).

## <a name="example"></a>Ejemplo

Ahora veremos un ejemplo del uso de expresiones, específicamente el ejemplo PropertySet de la galería de ejemplos de la interfaz de usuario de Windows. Veremos la expresión que administra el comportamiento de movimiento de órbita de la bola azul.

![Círculo en órbita](images/animation/orbit.gif)

Hay tres componentes en juego para la experiencia total:

1. KeyFrameAnimation, que anima el desplazamiento Y de la bola roja.
1. PropertySet con una propiedad de **giro** que ayuda a que se impulsa la órbita, animada por otro KeyFrameAnimation.
1. ExpressionAnimation que controla el desplazamiento de la bola azul que hace referencia al desplazamiento de esfera rojo y la propiedad Rotation para mantener una órbita perfecta.

Nos centraremos en el ExpressionAnimation definido en #3. También usaremos las clases ExpressionBuilder para construir esta expresión. Al final se muestra una copia del código que se usa para generar esta experiencia a través de cadenas.

En esta ecuación, hay dos propiedades a las que debe hacer referencia desde el PropertySet; uno es un desplazamiento CenterPoint y el otro es la rotación.

```
var propSetCenterPoint =
_propertySet.GetReference().GetVector3Property("CenterPointOffset");

// This rotation value will animate via KFA from 0 -> 360 degrees
var propSetRotation = _propertySet.GetReference().GetScalarProperty("Rotation");
```

A continuación, debe definir el componente Vector3 que tiene en cuenta la rotación en órbita real.

```
var orbitRotation = EF.Vector3(
    EF.Cos(EF.ToRadians(propSetRotation)) * 150,
    EF.Sin(EF.ToRadians(propSetRotation)) * 75, 0);
```

> [!NOTE]
> `EF` es una notación abreviada de "uso" para definir ExpressionFunctions.
>
> `using EF = Microsoft.Toolkit.Uwp.UI.Animations.Expressions.ExpressionFunctions;`

Por último, combine estos componentes juntos y haga referencia a la posición de la bola roja para definir la relación matemática.

```
var orbitExpression = redSprite.GetReference().Offset + propSetCenterPoint + orbitRotation;
blueSprite.StartAnimation("Offset", orbitExpression);
```

En una situación hipotética, ¿qué ocurre si quiere usar esta misma expresión pero con otros dos objetos visuales, lo que significa 2 conjuntos de círculos en órbitas. Con CompositionAnimations, puede volver a usar la animación y seleccionar varios CompositionObjects de destino. Lo único que debe cambiar al usar esta expresión para el caso de órbita adicional es la referencia al elemento visual. Llamamos a esta plantilla.

En este caso, debe modificar la expresión generada anteriormente. En lugar de "obtener" una referencia a CompositionObject, se crea una referencia con un nombre y, a continuación, se asignan valores diferentes:

```
var orbitExpression = ExpressionValues.Reference.CreateVisualReference("orbitRoundVisual");
orbitExpression.SetReferenceParameter("orbitRoundVisual", redSprite);
blueSprite.StartAnimation("Offset", orbitExpression);
// Later on … use same Expression to assign to another orbiting Visual
orbitExpression.SetReferenceParameter("orbitRoundVisual", yellowSprite);
greenSprite.StartAnimation("Offset", orbitExpression);
```

Este es el código si definió la expresión con cadenas a través de la API pública.

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
