---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: Animaciones de composición
description: Muchas de las propiedades de objetos y efectos de composición se pueden animar con animaciones de fotogramas y expresión clave, lo que permite a las propiedades de un elemento de la interfaz de usuario cambiar con el tiempo o según un cálculo concreto.
---
# Animaciones de composición

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Muchas de las propiedades de objetos y efectos de composición se pueden animar con animaciones de fotogramas y expresión clave, lo que permite a las propiedades de un elemento de la interfaz de usuario cambiar con el tiempo o según un cálculo concreto. Hay dos tipos de animaciones, animaciones de fotograma clave, que se representan con la clase [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830), y animaciones de expresión, que se representan con la clase [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817).

-   [Propiedades que se pueden animar](./composition-animation.md#animatable_properties)
-   [Animaciones de fotograma clave](./composition-animation.md#keyframe-animations)
    -   [Crear animaciones y fotogramas clave](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [Propiedades de fotograma clave](./composition-animation.md#keyframe-properties)
    -   [Funciones de aceleración](./composition-animation.md#easing-functions)
    -   [Iniciar y detener animaciones de fotograma clave](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [Eventos de finalización de la animación](./composition-animation.md#animation-completion-events)
-   [Animaciones de expresión](./composition-animation.md#expression-animations)
    -   [Crear expresiones](./composition-animation.md#creating-your-expression)
    -   [Conjuntos de propiedades](./composition-animation.md#property-sets)
    -   [Fotogramas clave de expresión](./composition-animation.md#expression_keyframes)

## Propiedades que se pueden animar

Las siguientes propiedades se pueden animar al asociarlas con un fotograma clave o animación de expresión:

-   CompositionColorBrush.Color
-   InsetClip.BottomInset
-   InsetClip.LeftInset
-   InsetClip.RightInset
-   InsetClip.TopInset
-   Visual.AnchorPoint
-   Visual.CenterPoint
-   Visual.Offset
-   Visual.Opacity
-   Visual.Orientation
-   Visual.RotationAngle
-   Visual.RotationAxis
-   Visual.Size
-   Visual.TransformMatrix
-   CompositionPropertySet

## Animaciones de fotograma clave

Las animaciones de fotograma clave son animaciones basadas en el tiempo que usan uno o más fotogramas clave para especificar cómo el valor animado debe cambiar con el tiempo. Los fotogramas representan marcadores, lo que permite definir qué debe ser el valor animado en un momento específico.

### Crear animaciones y fotogramas clave

Para crear una animación de fotograma clave, usa uno de los métodos del constructor de la clase Compositor que se relaciona con el tipo de estructura de la propiedad que se va a animar.

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

Por ejemplo, en el siguiente fragmento de código se crea una animación de fotograma clave Vector3:

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

Cada fotograma clave se construye con una llamada al método InsertKeyFrame de un objeto KeyFrameAnimation y especificando dos componentes y, opcionalmente, un tercer componente:

-   Tiempo: estado de progreso normalizado del fotograma clave entre 0,0 y 1,0
-   Valor: valor específico del valor de animación en el estado de tiempo
-   (Opcional) Función de aceleración: función para describir la interpolación entre el fotograma clave anterior y el actual (se explica más adelante)

Por ejemplo, en el siguiente fragmento de código se inserta un fotograma clave en un objeto KeyFrameAnimation que se desencadenará en la mitad de la animación:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### Propiedades de fotograma clave

Una vez que hayas definido la animación de fotograma clave y los fotogramas clave individuales, puedes definir varias propiedades de la animación:

-   DelayTime: el tiempo antes del inicio de una animación después de la llamada a StartAnimation().
-   Duration: duración de la animación.
-   IterationBehavior: recuento o comportamiento de repetición infinito de una animación.
-   IterationCount: el número de veces limitadas que se repetirá una animación de fotograma clave.
-   KeyFrameCount: el número de fotogramas clave en un objeto KeyFrameAnimation determinado.
-   StopBehavior: especifica el comportamiento de un valor de propiedad de animación cuando se llama a StopAnimation.

Por ejemplo, en el siguiente fragmento de código se establece la duración de la animación en cinco segundos:

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### Funciones de aceleración

Una función de aceleración de fotograma clave, un objeto CompositionEasingFunction, indica cómo los valores intermedios progresan desde el valor de fotograma clave anterior al valor de fotograma clave actual. Si no proporcionas una función de aceleración para el fotograma clave, se usará una curva predeterminada.

Se admiten dos tipos de funciones de aceleración:

-   Lineal ([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   Bézier cúbica ([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

Para crear una función de aceleración, usa el método [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) o [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791) de la clase [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761):

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

Nota: aquí se encuentra una herramienta de utilidad para generar los puntos de la curva Bézier cúbica.

Para agregar la función de aceleración al fotograma clave, simplemente agrega en el tercer parámetro al fotograma clave cuando lo insertes en la animación:

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### Iniciar y detener animaciones de fotograma clave

Después de definir la animación, los fotogramas clave y las propiedades, estás listo para asociar la animación a la propiedad que quieras animar. La animación se asocia a la propiedad que tienes previsto animar con una llamada a [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840) en el objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) al que pertenece la propiedad.

A continuación ofrecemos la sintaxis general y un ejemplo:

```cs
targetVisual.StartAnimation("Offset", animation);
```

Después de iniciar la animación, también puedes detenerla y desasociarla. Para ello, usa el método [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841) y especifica la propiedad cuya animación quieres detener.

Por ejemplo:

```cs
targetVisual.StopAnimation("Offset");
```

### Eventos de finalización de la animación

Las animaciones de fotograma clave tienen un final definido, a diferencia de las animaciones de expresión, que son condicionales. Los desarrolladores pueden determinar cuándo finalizan los grupos o una animación de fotograma clave única mediante lotes. Los lotes se pueden crear o recuperar, en función del tipo de objeto de lote, y se usan para agregar el estado final de las animaciones. Los lotes con ámbito se crean durante la recuperación de los lotes Commit (el uso de estos distintos lotes se describe más adelante).

Un evento de finalización de lote se desencadena cuando se hayan completado todas las animaciones del lote. El tiempo necesario para que el evento de finalización de un lote se desencadene depende de la animación más larga o de mayor retraso del lote. La agregación de estados finales es de utilidad cuando necesitas saber cuándo se completan grupos de animaciones selectas para programar el trabajo relacionado.

### Lotes con ámbito

Para agregar un grupo específico o una animación única, primero debes crear un lote con ámbito con una llamada a [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425).

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

Después de crear un lote con ámbito, todas las animaciones iniciadas se agregan hasta que el lote se suspenda o finalice explícitamente con [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) o [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844).

Una llamada a [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) deja de agregar estados de finalización de animación hasta que se llame a [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847). Esto permite excluir explícitamente el contenido de un lote determinado.

```cs
myScopedBatch.Suspend();
```

Para completar el lote, debes llamar a [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844). Sin una llamada a End, el lote permanecerá abierto y seguirá recopilando objetos de manera ilimitada.

```cs
myScopedBatch.End();
```

Si quieres saber cuándo finaliza una animación, tienes que crear un lote con ámbito, iniciar la animación y finalizar el lote.

Por ejemplo:

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

### Lotes de confirmación

Un lote de confirmación es un lote que se crea implícitamente durante el ciclo de confirmación. El lote de confirmación actual se puede recuperar con una llamada a [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430) en cualquier momento durante el ciclo de confirmación. El ciclo de confirmación se define como el tiempo entre las actualizaciones desde el compositor. Las actualizaciones se ponen en cola hasta que el sistema esté listo para procesar los cambios y dibujar bits en la pantalla. Los títulos no controlan cuándo se producen confirmaciones. El lote de confirmación agregará todos los objetos dentro del ciclo de confirmación, tanto los anteriores como los posteriores a la llamada a GetCommitBatch. Hay solo un lote de confirmación por ciclo de confirmación y no es necesario llamar explícitamente a [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) en el lote de confirmación.

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### Estados de lote

Para determinar si un lote está abierto para agregar animaciones, puedes usar la propiedad **IsActive**. Si la propiedad **IsEnded** devuelve true, no puedes agregar una animación a ese lote específico. Los lotes con ámbito se "terminan" cuando se hayan finalizado explícitamente con una llamada al método [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844). Los lotes de confirmación finalizan cuando haya finalizado el ciclo de confirmación actual.

## Animaciones de expresión

Las animaciones de expresión son animaciones que usan una expresión matemática para especificar cómo se calcula el valor animado para cada fotograma. El lenguaje de expresión en sí puede hacer referencia a propiedades de otros objetos de composición. A diferencia de las animaciones de fotograma clave, las expresiones no se basan en el tiempo y se procesan en cada fotograma (si es necesario). Las expresiones pueden ser de utilidad a la hora de describir una experiencia de usuario más compleja que se puede procesar mediante el motor Composition, por ejemplo encabezados permanentes y parallax.

### Crear expresiones

Para crear una expresión, llama a [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002) en el objeto Compositor y especifica la expresión que quieres usar:

```cs
var expression = _compositor.CreateExpressionAnimation("INSERT_EXPRESSION_STRING")
```

### Operadores, prioridad y asociatividad

El lenguaje de expresiones admite los siguientes operadores y es conforme con la prioridad y asociatividad de operadores según se define en la especificación del lenguaje C#:

-   Unario (-)
-   Multiplicativo (\* /)
-   Aditivo (-+)

El lenguaje de expresión también admite el concepto de operaciones matemáticas por componente. Estos operadores de acceso de miembro (x.y) solo se admiten en tipos "primitivos" (vector y matrices). Por ejemplo:

```cs
Offset.x + 5.0
```

### Parámetros de propiedad

Uno de los componentes eficaces del lenguaje de expresión está poder usar parámetros como variables en la expresión. La cadena de expresión puede contener parámetros que se reemplazan con un valor constante o son referencias a la propiedad de otro objeto cuando se calcula. Por ejemplo:

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

En este ejemplo, ChildOffset y ParentOffset son parámetros que representan la propiedad de desplazamiento de dos elementos visuales. Los parámetros se definen con los métodos **Set*Parameter** de la clase [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708):

-   Para crear un parámetro de vector, se usa [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx), [**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx) o [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter).
-   Para crear un parámetro de matriz, se usa [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx) o [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter).
-   Para crear un parámetro de objeto escalar, se usa [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter).
-   Para crear un parámetro de color, se usa [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter).
-   Para crear un parámetro de cuaternión, se usa [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter).
-   Para crear un parámetro de referencia, se usa [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter).

En la cadena de expresión anterior, tendríamos que crear dos parámetros para definir los dos elementos visuales:

```cs
Expression.SetReferenceParameter("ChildVisual", childVisual);
Expression.SetReferenceParameter("ParentVisual", parentVisual);
```

### Funciones auxiliares de expresión

Además de tener acceso a los operadores y parámetros de propiedad, también tienes acceso a la biblioteca completa de funciones auxiliares del lenguaje de expresión. Encontrarás la lista completa de funciones auxiliares en la sección de observaciones de la clase [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817), pero aquí te damos algunas:

-   Max (Scalar value1, Scalar value2)
-   Scale (Vector3 value, Scalar factor)
-   Transform(Vector2 value, Matrix 3x2 matrix)

Aquí se ofrece un ejemplo más complejo de una cadena de expresión que usa la función auxiliar Clamp:

```cs
Clamp((scroller.Offset.y * -1.0) - container.Offset.y, 0, container.Size.y - header.Size.y)
```

### Iniciar y detener la animación de expresión

Para iniciar y detener las animaciones de expresión, usas las mismas funciones ([**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840), [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841)) que las que usas con animaciones de fotograma clave. Nota: las animaciones de expresión seguirán ejecutándose hasta que se detengan con el método **StopAnimation**: a diferencia de las animaciones de fotograma clave, no tienen una duración limitada.

### Conjuntos de propiedades

Además de poder hacer referencia a las propiedades de otros objetos Composition, también puedes crear y almacenar valores de propiedad propios que se pueden usar entre varias animaciones. Los conjuntos de propiedades se representan con la clase [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772). Los conjuntos de propiedades te permiten crear una propiedad con un valor y hacer referencia a ella más adelante en una expresión, o incluso establecerla como el destino de una animación de fotograma clave.

Para crear un conjunto de propiedades, usa el método [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802) de la clase Compositor. Por ejemplo:

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Cuando hayas creado el conjunto de propiedades, puedes agregarle una propiedad y valor mediante uno de los métodos **Insert\*** del objeto [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772). Por ejemplo:

```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

Después de crear la animación de expresión, puedes hacer referencia a las propiedades del conjunto de propiedades establecido en la cadena de expresión mediante un parámetro de referencia. Por ejemplo:

```cs
var expression = _compositor.CreateExpressionAnimation("sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

### Fotogramas clave de expresión

Anteriormente en este documento, describimos cómo crear animaciones de fotograma clave y sus fotogramas clave correspondientes. Además de crear un fotograma clave tradicional con un tiempo normalizado y componente de valor, también puedes definir fotogramas clave de expresión.

En lugar de definir un valor explícito para un tiempo concreto en el fotograma clave, podemos usar la sintaxis de expresión para describir cómo se debe calcular el valor de este punto concreto en la escala de tiempo del fotograma clave. Es posible combinar fotogramas clave de expresión con fotogramas clave básicos en la animación de fotograma clave.

### Construir fotogramas clave de expresión

Al igual que los fotogramas clave tradicionales, los fotogramas clave de expresión se construyen al especificar 2 componentes:

-   Tiempo: estado de tiempo normalizado del fotograma clave entre 0,0 y 1,0
-   Valor: la cadena de expresión que se usa para describir cómo se debe calcular el valor

Por ejemplo, en el siguiente fragmento de código se usa una combinación de fotogramas clave normales y de expresión:

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

### Usar valores actuales e iniciales

En el lenguaje de expresión, es posible hacer referencia tanto al valor actual y como al valor inicial de una animación. Estos valores se preestablecen en el lenguaje de expresión con "this":

-   this.CurrentValue
-   this.StartingValue

Ejemplo del uso de un fotograma clave de expresión:

```cs
animation.InsertExpressionKeyFrame(0.0f, "this.CurrentValue + delta");
```

### Expresiones condicionales

Además de admitir expresiones matemáticas básicas, también puedes definir una expresión condicional con un operador ternario:

```cs
(condition ? first_expression : second_expression)
```

Dentro de las expresiones en sí, en la instrucción condicional se admiten los siguientes operadores condicionales:

-   Es igual a (==)
-   No es igual a (!=)
-   Menor que (<)
-   Menor o igual que (<=)
-   Mayor que (>)
-   Mayor o igual que (>=)

Por último, en la instrucción condicional se admiten las siguientes conjunciones como operadores o funciones:

-   No: ! / Not(bool1)
-   Y: && / And(bool1, bool2)
-   O: || / Or(bool1, bool2)

En el siguiente fragmento de código se muestra un ejemplo del uso de instrucciones condicionales en una expresión:

```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f");
```

 

 






<!--HONumber=May16_HO2-->


