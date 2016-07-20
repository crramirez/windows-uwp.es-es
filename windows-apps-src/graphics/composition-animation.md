---
author: scottmill
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: "Animaciones de composición"
description: "Muchas de las propiedades de objetos y efectos de composición se pueden animar con animaciones de fotogramas y expresión clave, lo que permite a las propiedades de un elemento de la interfaz de usuario cambiar con el tiempo o según un cálculo concreto."
translationtype: Human Translation
ms.sourcegitcommit: 62f0ea80940ff862d26feaa063414d95b048f685
ms.openlocfilehash: e0088692b9de10c188f15b85b1f20b98cc113517

---
# Animaciones de composición

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

La API de WinRT Windows.UI.Composition te permite crear, animar, transformar y manipular objetos de compositor en una capa de API unificada. Las animaciones de composición proporcionan una forma eficaz y eficiente para ejecutar animaciones en la interfaz de usuario de la aplicación. Se han diseñado desde cero para garantizar que las animaciones se ejecuten a 60 FPS independientemente del subproceso de la interfaz de usuario, así como para proporcionarte flexibilidad para crear experiencias increíbles no solo con tiempo, sino también con la entrada y otras propiedades, para controlar las animaciones.
En este tema se proporciona una visión general de la funcionalidad disponible que permite animar las propiedades del objeto de composición.
En este documento se supone que estás familiarizado con los conceptos básicos de la estructura de la capa visual. Puedes obtener más información [aquí](./composition-visual-tree.md). Hay dos tipos de animaciones de composición: **animaciones de fotograma clave** y **animaciones de expresión**.  

![](./images/composition-animation-types.png)  
   
 
##Tipos de animaciones de composición

            Las **animaciones de fotograma clave** proporcionan experiencias de animación *fotograma a fotograma* basadas en tiempo tradicionales. Los desarrolladores pueden definir explícitamente *puntos de control* que describan los valores que una propiedad de animación necesita para estar en puntos específicos en la línea de tiempo de animación. Cabe destacar que puede usar las funciones de aceleración (también denominadas "interpoladores") para describir cómo realizar la transición entre estos puntos de control.  


            Las **animaciones de expresión** son un nuevo tipo de animación que se introdujo en la capa visual con la actualización de noviembre de Windows 10 (compilación 10586). La idea de las animaciones de expresión es que un desarrollador puede crear relaciones matemáticas entre las propiedades visuales y los valores discretos que se evaluarán y actualizarán en cada fotograma. Los desarrolladores pueden hacer referencia a las propiedades en objetos de composición o conjuntos de propiedades, usar aplicaciones auxiliares de funciones matemáticas e incluso hacer referencia a la entrada para derivar estas relaciones matemáticas. Las expresiones hacen que las experiencias como las del efecto parallax y los encabezados permanentes se puedan desarrollar sin problemas en la plataforma Windows.  

##Motivos para usar las animaciones de composición
**Rendimiento**  
 Al compilar aplicaciones universales de Windows, la mayoría del código de desarrollador se ejecuta en el subproceso de la interfaz de usuario. Por lo tanto, para garantizar que las animaciones se ejecuten sin problemas en las distintas categorías de dispositivos, el sistema realiza los cálculos de animación y trabaja en un subproceso independiente para mantener la velocidad de 60 FPS. Esto significa que los desarrolladores puedan contar en el sistema para proporcionar animaciones suaves, mientras que las aplicaciones realizan otras operaciones complejas para experiencias de usuario avanzado.    
 
**Posibilidades**  
El objetivo de las animaciones de composición de la capa visual es permitir que la interfaz de usuario tenga un aspecto atractivo. Queremos ofrecer a los desarrolladores flexibilidad y distintos tipos de animaciones para ayudarles a dar forma a sus magníficas ideas y a trascender los límites de UWP en adelante.
 
 (También puedes consultar el [tema sobre la composición en GitHub](http://go.microsoft.com/fwlink/?LinkID=789439) para ver ejemplos sobre cómo usar las API y muestras de mayor fidelidad de las API en acción).  

**Aplicación de plantillas**  
 Todas las animaciones de composición de la capa visual son plantillas, lo que significa que los desarrolladores pueden usar una animación en varios objetos sin necesidad de crear animaciones independientes. Esto permite a los desarrolladores usar la misma animación y retocar las propiedades o los parámetros para satisfacer otras necesidades sin preocuparse por obstruir los usos anteriores.  
 
##¿Qué se puede animar con las animaciones de composición?
Las animaciones de composición se pueden aplicar a la mayoría de las propiedades de objetos de composición como Visual e InsetClip. También puedes aplicar animaciones de composición a los efectos de composición y los conjuntos de propiedades. **Al elegir lo que quieres animar, toma nota del tipo: usa esta opción para determinar qué tipo de animación de fotograma clave quieres construir o qué tipo de expresión debes resolver.**  
 
###Visual
|Propiedades Visual que se pueden animar|  Tipo|
|------|------|
|AnchorPoint|   Vector2|
|CenterPoint|   Vector3|
|Offset|    Vector3|
|Opacity|   Scalar|
|Orientation|   Quaternion|
|RotationAngle| Scalar|
|RotationAngleInDegrees|    Scalar|
|RotationAxis|  Vector3|
|Scale| Vector3|
|Size|  Vector2|
|TransformMatrix*|  Matrix4x4|
* Si quieres animar la propiedad TransformMatrix completa como una matriz 4x4, debes usar una animación de expresión para hacerlo. De lo contrario, puedes elegir celdas individuales de la matriz y usar en ellas las animaciones de fotograma clave o expresión.  

###InsetClip
|Propiedades InsetClip que se pueden animar|   Tipo|
|-------------------------------|-------|
|BottomInset|   Scalar|
|LeftInset| Scalar|
|RightInset|    Scalar|
|TopInset|  Scalar|

##Propiedades Visual de canal secundario
Además de animar las propiedades Visual, también puedes usar los componentes de *canal secundario* de estas propiedades para las animaciones. Por ejemplo, pongamos que solo quieres animar la propiedad X Offset de Visual en lugar de la propiedad Offset completa. La animación puede tener como destino la propiedad Offset de tipo Vector3 o el componente escalar de X de la propiedad Offset. Además de poder elegir como destino un componente de subcanal individual de una propiedad, también puedes seleccionar varios componentes. Por ejemplo, puedes elegir el componente X e Y de Scale.

|Propiedades Visual de canal secundario que se pueden animar|  Tipo|
|----------------------------------------|------|
|AnchorPoint.x, y|Scalar|
|AnchorPoint.xy|Vector2|
|CenterPoint.x, y, z|Scalar|
|CenterPoint.xy, xz, yz|Vector2|
|Offset.x, y, z|Scalar|
|Offset.xy, xz, yz|Vector2|
|RotationAxis.x, y, z|Scalar|
|RotationAxis.xy, xz, yz|Vector2|
|Scale.x, y, z|Scalar|
|Scale.xy, xz, yz|Vector2|
|Size.x, y|Scalar|
|Size.xy|Vector2|
|TransformMatrix._11 ... TransformMatrix._NN,|Scalar|
|TransformMatrix._11_12 ... TransformMatrix._NN_NN|Vector2|
|TransformMatrix._11_12_13 ... TransformMatrix._NN_NN_NN|Vector3|
|TransformMatrix._11_12_13_14|Vector4|
|Color*|    Colors (Windows.UI)|

* El canal secundario Color de la propiedad Brush se anima de manera un poco diferente. Puedes conectar StartAnimation() a Visual.Brush y declarar la propiedad para que se anime en el parámetro como "Color". (Posteriormente se tratan más detalles sobre la animación del color).

##Conjuntos de propiedades y efectos
Además de la animación de propiedades de objetos visuales de composición e InsetClip, también puedes animar propiedades de una colección PropertySet o un efecto. Para los conjuntos de propiedades, se define una propiedad y se almacena en un conjunto de propiedades de composición. Esta propiedad puede ser posteriormente el destino de una animación (y también se puede hacer referencia a esta simultáneamente en otra). Esto se explica con más detalle en las siguientes secciones.  

Para los efectos, es posible definir efectos gráficos con las API de efectos de composición (consulta aquí la [introducción a los efectos](./composition-effects.md). Además de definir efectos, también puedes animar los valores de propiedad del efecto. Para ello, el componente de propiedades de la propiedad Brush se establece como destino en las clases SpriteVisual.

##Fórmula rápida: Introducción a las animaciones de composición
Antes de profundizar en los detalles sobre cómo crear y usar los diferentes tipos de animaciones, a continuación se incluye una fórmula rápida de alto nivel sobre cómo reunir las animaciones de composición.  
1.  Decide qué propiedad, propiedad del canal secundario o efecto quieres animar (toma nota del tipo).  
2.  Crea un nuevo objeto para tu animación (será un fotograma clave o una animación de expresión).  
    *  Para las animaciones de fotograma clave, asegúrate de crear un tipo de animación de fotograma clave que coincida con el tipo de propiedad que quieres animar.  
    *  Solo hay un tipo de animación de expresión.  
3.  Define el contenido de la animación: inserta los fotogramas clave o define la cadena de expresión.  
    *  Para las animaciones de fotograma clave, asegúrate de que el valor de los fotogramas clave sea del mismo tipo que la propiedad que quieres animar.  
    *  Para las animaciones de expresión, asegúrate de que la cadena de expresión se resolverá en el mismo tipo que la propiedad que quieres animar.  
4.  Inicia la animación en el objeto Visual cuya propiedad quieres animar. Llama a StartAnimation e incluye como parámetros: el nombre de la propiedad que quieres animar (con formato de cadena) y el objeto de la animación.  

```cs
// KeyFrame Animation Example to target Opacity property
// Step 2 - Create your animation object
var animation = _compositor.CreateScalarKeyFrameAnimation();
// Step 3 - Define Content
animation.InsertKeyFrameAnimation(1.0f, 0.2f); 
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", animation); 
  
// Expression Animation Example to target Opacity property
// Step 2 - Create your animation object
var expression = _compositor.CreateExpressionAnimation(); 
// Step 3 - Define Content (you can also define the string as part of the expression object
// declaration)
expression.Expression = "targetVisual.Offset.X / windowWidth";
expression.SetReferenceParameter("targetVisual", _target);
expression.SetScalarParameter("windowWidth", _xSizeWindow);
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", expression);

```

##Uso de animaciones de fotograma clave
Las animaciones de fotograma clave son animaciones basadas en el tiempo que usan uno o más fotogramas clave para especificar cómo el valor animado debe cambiar con el tiempo. Los fotogramas representan marcadores o puntos de control, lo que permite definir cuál debe ser el valor animado en un momento específico.  
 
###Creación de la animación y definición de fotogramas clave
Para crear una animación de fotograma clave, usa el método del constructor del objeto Compositor que se relaciona con el tipo de la propiedad que quieres animar. Los distintos tipos de animación de fotograma clave son:
*   ColorKeyFrameAnimation
*   QuaternionKeyFrameAnimation
*   ScalarKeyFrameAnimation
*   Vector2KeyFrameAnimation
*   Vector3KeyFrameAnimation
*   Vector4KeyFrameAnimation  

Ejemplo que crea una animación de fotograma clave Vector3:     
```cs
var animation = _compositor.CreateVector3KeyFrameAnimation(); 
```

Cada animación de fotograma clave se construye insertando segmentos individuales de fotograma clave que definen dos componentes (con un tercero opcional)  
*   Tiempo: estado de progreso normalizado del fotograma clave entre 0,0 y 1,0
*   Valor: valor específico del valor de animación en el estado de tiempo
*   (Opcional) Función de aceleración: función para describir la interpolación entre el fotograma clave anterior y el actual (se explica más adelante en este tema)  

Ejemplo que inserta un fotograma clave en el punto medio de la animación:
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```


            **Nota:** Al animar el color con animaciones de fotograma clave, hay algunos aspectos adicionales a tener en cuenta:
1.  StartAnimation se asocia a Visual.Brush, en lugar de Visual, con **Color** como el parámetro de propiedad que se quiere animar.
2.  El componente "valor" del fotograma clave se define mediante el objeto Colors fuera del espacio de nombres Windows.UI.
3.  Tienes la opción de definir el espacio de colores por el que pasará la interpolación, para lo cual debes establecer la propiedad InterpolationColorSpace. Algunos valores posibles son: a.  CompositionColorSpace.Rgb b.  CompositionColorSpace.Hsl


##Propiedades de animación de fotograma clave
Una vez que hayas definido la animación de fotograma clave y los fotogramas clave individuales, puedes definir varias propiedades fuera de la animación:
*   DelayTime: el tiempo antes del inicio de una animación después de la llamada a StartAnimation()
*   Duration: duración de la animación.
*   IterationBehavior: recuento o comportamiento de repetición infinito de una animación.
*   IterationCount: el número de veces limitadas que se repetirá una animación de fotograma clave
*   KeyFrameCount: lectura del número de fotogramas clave en una animación de fotograma clave determinada
*   StopBehavior: especifica el comportamiento de un valor de propiedad de animación cuando se llama a StopAnimation  

Ejemplo que establece la duración de la animación en 5 segundos:  
```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

##Funciones de aceleración
Las funciones de aceleración (CompositionEasingFunction) indican cómo progresan los valores intermedios desde el valor de fotograma clave anterior hasta el valor de fotograma clave actual. Si no proporcionas una función de aceleración para el fotograma clave, se usará una curva predeterminada.  
Se admiten dos tipos de funciones de aceleración:
*   Linear
*   Cubic Bezier  

Las funciones Cubic Bezier son funciones paramétricas que suelen usarse para describir curvas suaves que se pueden escalar. Cuando se usan animaciones de fotograma clave de composición, deben definirse dos puntos de control que sean objetos Vector2. Estos puntos de control se usan para definir la forma de la curva. Se recomienda usar sitios similares, como [este](http://cubic-bezier.com/#0,-0.01,.48,.99) para visualizar cómo construyen la curva dos puntos de control para una función Cubic Bezier.

Para crear una función de aceleración, usa el método de constructor fuera del objeto Compositor. Los dos ejemplos siguientes crean una función de aceleración Linear y una función Cubic Bezier easeIn.  
```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```
Para agregar la función de aceleración al fotograma clave, simplemente agrega el tercer parámetro al fotograma clave cuando lo insertes en la animación.   
Ejemplo en el que se agrega una función de aceleración easeIn con el fotograma clave:  
```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

##Iniciar y detener animaciones de fotograma clave
Después de definir la animación y los fotogramas clave, estás listo para enlazar la animación. Al iniciar la animación, debes especificar el objeto Visual y la propiedad de destino que se van a animar, así como una referencia a la animación. Para ello, llama a la función StartAnimation(). Recuerda que al llamar a StartAnimation() en una propiedad, se desconectarán y quitarán todas las animaciones que se ejecutaban anteriormente.  

            **Nota:** La referencia a la propiedad que elijas para animar tendrá el formato de una cadena.  

Ejemplo en el que se establece e inicia una animación en la propiedad Offset de Visual:  
```cs
targetVisual.StartAnimation("Offset", animation);
```  

Si quieres como destino una propiedad de subcanal, debes agregar el subcanal a la cadena que define la propiedad que quieres animar. En los ejemplos anteriores, la sintaxis cambiaría a StartAnimation("Offset.X, animation2), donde animation2 es un objeto ScalarKeyFrameAnimation.  

Después de iniciar la animación, también puedes detenerla antes de que finalice. Esta acción se lleva a cabo mediante la función StopAnimation().  
Ejemplo en el que se detiene una animación en la propiedad Offset de Visual:    
```cs
targetVisual.StopAnimation("Offset");
```

También puedes definir el comportamiento de la animación cuando se detiene explícitamente. Para ello, debes definir la propiedad Stop Behavior fuera de la animación. Existen tres opciones:
*   LeaveCurrentValue: la animación marcará el valor de la propiedad animada para que sea el último valor calculado de la animación.
*   SetToFinalValue: la animación marcará el valor de la propiedad animada para que sea el valor del fotograma clave final.
*   SetToInitialValue: la animación marcará el valor de la propiedad animada para que sea el valor del primer fotograma clave.  

Ejemplo en el que se establece la propiedad StopBehavior para una animación de fotograma clave:  
```cs
animation.StopBehavior = AnimationStopBehavior.LeaveCurrentValue;
```

##Eventos de finalización de animación
Con las animaciones de fotograma clave, los desarrolladores pueden usar un lote de animaciones para agregarlo al completar una determinada animación (o grupo de animaciones). Solo los eventos de finalización de animación de fotograma clave pueden procesarse por lotes. Las expresiones no tienen un fin definido, de modo que no desencadenan un evento de finalización. Si se inicia una animación de expresión dentro de un lote, la animación se ejecutará según lo previsto y no influirá en el momento en que debe desencadenarse el lote.    

Un evento de finalización de lote se desencadena cuando se completan todas las animaciones del lote. El tiempo necesario para que el evento de un lote se desencadene depende de la animación más larga o con un mayor retraso del lote.
La agregación de estados finales es de utilidad cuando necesitas saber cuándo se completan determinados grupos de animaciones para programar otras tareas.  

Los lotes se eliminarán cuando se desencadene el evento de finalización. También puedes llamar a Dispose() en cualquier momento para liberar el recurso de manera anticipada. También puedes eliminar manualmente el objeto de lote si una animación por lotes finaliza de manera anticipada y no quieres retomar el evento de finalización. Si se interrumpe o cancela una animación, se desencadenará el evento de finalización y formará parte del lote en que se estableció. Esto se demuestra en el ejemplo del SDK Animation_Batch en la entrada sobre [Windows/Composition en GitHub](http://go.microsoft.com/fwlink/p/?LinkId=789439).  
 
##Lotes con ámbito
Para agregar un grupo específico de animaciones o establecer como destino un evento de finalización de una única animación, puedes crear un lote con ámbito.    
```cs
CompositionScopedBatch myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
``` 
Después de crear un lote con ámbito, todas las animaciones iniciadas se agregan hasta que el lote se suspende o finaliza explícitamente mediante la función Suspend o End.    

Al llamar a la función Suspend se dejan de agregar estados de finalización de animación hasta que se llama a Resume. Esto permite excluir explícitamente el contenido de un lote determinado.  

En el siguiente ejemplo, la animación destinada a la propiedad Offset de VisualA no se incluirá en el lote:  
```cs
myScopedBatch.Suspend();
VisualA.StartAnimation("Offset", myAnimation);
myScopeBatch.Resume();
```

Para completar el lote, debes llamar a End(). Sin una llamada a End, el lote permanecerá abierto y seguirá recopilando objetos de manera ilimitada.  
 
El fragmento de código y el diagrama siguientes muestran un ejemplo de cómo el lote agregará animaciones para realizar un seguimiento de los estados de finalización. Ten en cuenta que, en este ejemplo, las animaciones 1, 3 y 4 tendrán estados de finalización de los que este lote realizará un seguimiento, pero no la animación 2.  
```cs
myScopedBatch.End();
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
// Start Animation1
[…]
myScopedBatch.Suspend();
// Start Animation2 
[…]
myScopedBatch.Resume();
// Start Animation3
[…]
// Start Animation4
[…]
myScopedBatch.End();
```  
![](./images/composition-scopedbatch.png)
 
##Procesamiento por lotes de un evento de finalización de una única animación
Si quieres saber cuándo finaliza una única animación, debes crear un lote con ámbito que incluya solo la animación de destino. Por ejemplo:  
```cs
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

##Recuperación de un evento de finalización de un lote

Al procesar por lotes una o varias animaciones, el evento de finalización del lote se recuperará de la misma manera. Debes registrar el método de control de eventos para el evento Completed del lote de destino.  

```cs
myScopedBatch.Completed += OnBatchCompleted;
``` 

##Estados de lote
Existen dos propiedades que puedes usar para determinar el estado de un lote existente: IsActive e IsEnded.  

La propiedad IsActive devuelve true si un lote destino está abierto para agregar animaciones. IsActive devolverá false si un lote se suspende o finaliza.   

La propiedad IsEnded devuelve true si no puedes agregar una animación a ese lote específico. Un lote finalizará cuando se llames explícitamente a End() para un lote específico.  
 
##Uso de animaciones de expresión
Las animaciones de expresión son un nuevo tipo de animación que el equipo de composición introdujo con la actualización de noviembre de Windows 10 (10586). En un nivel alto, las animaciones de expresión se basan en una ecuación o relación matemática entre valores discretos y referencias a otras propiedades de objetos de composición. A diferencia de las animaciones de fotograma clave, que usan una función de interpolador (Cubic Bezier, Quad, Quintic, etc.) para describir cómo cambia el valor con el tiempo, las animaciones de expresión usan una ecuación matemática para definir cómo se calcula el valor animado en cada fotograma. Es importante destacar que las animaciones de expresión no tienen una duración definida: una vez iniciadas, se ejecutarán y usarán la ecuación matemática para determinar el valor de la propiedad de animación hasta que se detengan explícitamente.

**Por lo tanto, ¿para qué sirven las animaciones de expresión?** El poder real de las animaciones de expresión proviene de su capacidad para crear una relación matemática que incluya referencias a parámetros o propiedades de otros objetos. Esto significa que puede tener una ecuación que haga referencia a valores de propiedades en otros objetos de composición, variables locales o incluso en valores compartidos de conjuntos de propiedades de composición. Debido a este modelo de referencia y a que la ecuación se evalúa en cada fotograma, si los valores que definen una ecuación cambian, también cambiará el resultado de la ecuación. Esto abre posibilidades más allá de las animaciones de fotograma clave tradicionales, en que los valores deben ser discretos y predefinidos. Por ejemplo, experiencias como los encabezados permanentes y el efecto parallax se pueden describir fácilmente mediante animaciones de expresión.


            **Nota:** Los términos "Expresión" o "Cadena de expresión" se usan como referencia para la ecuación matemática que define el objeto de animación de expresión.

##Crear y adjuntar la animación de expresión
Antes de pasar a la sintaxis de creación de animaciones de expresión, hay algunos principios básicos que debemos mencionar:  
*   Las animaciones de expresión usan una ecuación matemática definida para determinar el valor de la propiedad de animación en cada fotograma.
*   La ecuación matemática se introduce en la expresión como una cadena.
*   El resultado de la ecuación matemática debe resolverse en el mismo tipo que la propiedad que se va a animar. Si no coinciden, obtendrá un error cuando se calcule la expresión. Si la ecuación se resuelve en Nan (número/0), el sistema usará el último valor calculado anteriormente.
*   Las animaciones de expresión tienen un *duración infinita*, siguen ejecutándose hasta que se detienen.  

Para crear la animación de expresión, solo tienes que usar el constructor fuera del objeto de composición, donde defines la expresión matemática.  
 
Ejemplo del constructor donde se define una expresión muy básica que suma dos valores Scalar (se analizarán expresiones más complejas en la siguiente sección):  
```cs
var expression = _compositor.CreateExpressionAnimation("0.2 + 0.3");
```
De manera similar a las animaciones de fotograma clave, después de definir la animación de expresión, debes adjuntarla al objeto Visual y declarar la propiedad que quieres que anime la animación. A continuación, seguiremos con el ejemplo anterior y adjuntaremos nuestra animación de expresión a la propiedad Opacity de Visual (tipo Scalar):  
```cs
targetVisual.StartAnimation("Opacity", expression);
```

##Componentes de la cadena de expresión
En el ejemplo de la sección anterior se muestran dos valores Scalar simples que se agregan juntos. Aunque es un ejemplo de expresiones válido, no muestra completamente el potencial de las expresiones. Una cosa a tener en cuenta sobre el ejemplo anterior es que dado que estos valores son discretos, cada fotograma de la ecuación se resolverá en 0,5 y nunca cambiará mientras dure la animación. El potencial real de las expresiones radica en definir una relación matemática en la que los valores puedan periódicamente o en todo momento.  
 
Veamos las distintas partes que forman estos tipos de expresiones.  

###Operadores, prioridad y asociatividad
La cadena de expresión admite el uso de operadores típicos que cabría esperar para describir relaciones matemáticas entre los distintos componentes de la ecuación:  

|Categoría|  Operadores|
|--------|-----------|
|Unario| -|
|Multiplicativo|    * /|
|Aditivo|  + -|

De forma similar, cuando se evalúe la expresión, obedecerá a la prioridad y la asociatividad de los operadores como se define en la especificación del lenguaje C#. Dicho de otro modo, respetará el orden básico de las operaciones.  

En el siguiente ejemplo, cuando se evalúen, los paréntesis se resolverán primero antes de resolver el resto de la ecuación según el orden de las operaciones:  
```cs
"(5.0 * (72.4 – 36.0) + 5.0" // (5.0 * 36.4 + 5) -> (182 + 5) -> 187
```

###Parámetros de propiedad
Los parámetros de propiedad son uno de los componentes más eficaces de las animaciones de expresión. En la cadena de expresión, puede hacer referencia a valores de propiedades de otros objetos, como objetos visuales de composición, conjuntos de propiedades de composición u otros objetos de C#.   

Para usarlos en una cadena de expresión, simplemente debes definir las referencias como parámetros para la animación de expresión. Esto se realiza mediante la asignación de la cadena usada en la expresión al objeto real. Esto permite al sistema saber qué debe inspeccionar para calcular el valor al evaluar la ecuación. Existen diferentes tipos de parámetros que se corresponden con el tipo del objeto que quieres incluir en la ecuación:  

|Tipo|  Función para crear el parámetro|
|----|------------------------------|
|Scalar|    SetScalarParameter(String ref, Scalar obj)|
|Vector|    SetVector2Parameter(String ref, Vector2 obj)<br/>SetVector3Parameter(String ref, Vector3 obj)<br/>SetVector4Parameter(String ref, Vector4 obj)|
|Matrix|    SetMatrix3x2Parameter(String ref, Matrix3x2 obj)<br/>SetMatrix4x4Parameter(String ref, Matrix4x4 obj)|
|Quaternion|    SetQuaternionParameter(String ref, Quaternion obj)|
|Color| SetColorParameter(String ref, Color obj)|
|CompositionObject| SetReferenceParameter(String ref, Composition object obj)|

En el siguiente ejemplo, se crea una animación de expresión que hará referencia a la propiedad Offset de otros dos elementos visuales de composición y a un objeto System.Numerics Vector3 básico.  
```cs
var commonOffset = new Vector3(25.0, 17.0, 10.0);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + additionalOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetVector3Parameter("additionalOffset", commonOffset);
```

Además, puedes hacer referencia a un valor en un conjunto de propiedades de una expresión utilizando el mismo modelo descrito anteriormente. Los conjuntos de propiedades de composición ofrecen un método útil para almacenar los datos que se usan en las animaciones, así como para crear datos reutilizables que se puedan compartir y que no estén vinculados a la duración de ningún otro objeto de composición. Se puede hacer referencia a los valores de conjunto de propiedades en una expresión similar a otras referencias a propiedades. (Los conjuntos de propiedades se explican con más detalle en una sección posterior).  

Se puede modificar el ejemplo anterior, de modo que un conjunto de propiedades se use para definir la variable commonOffset en lugar de una variable local:
```cs
_sharedProperties = _compositor.CreatePropertySet();
_sharedProperties.InsertVector3("commonOffset", offset);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + sharedProperties.commonOffset);
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

Por último, al hacer referencia a propiedades de otros objetos, también es posible hacer referencia a las propiedades de subcanal en la cadena de expresión o como parte del parámetro de referencia.  
 
En el ejemplo siguiente, se hace referencia al subcanal x de las propiedades Offset de dos objetos Visual: uno de la propia cadena de expresión y otro al crear la referencia de parámetro.
Ten en cuenta que al hacer referencia al componente X de Offset, cambiamos nuestro tipo de parámetro a Scalar en lugar de a Vector3 como en el ejemplo anterior:  
```cs
var expression = _compositor.CreateExpressionAnimation("xOffset/ ParentOffset.X");
expression.SetScalarParameter("xOffset", childVisual.Offset.X);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
```

###Funciones auxiliares de expresión y constructores
Además de tener acceso a operadores y parámetros de propiedad, puedes aprovechar una lista de funciones matemáticas para usar en sus expresiones. Estas funciones se proporcionan para realizar cálculos y operaciones en distintos tipos como lo harías con objetos System.Numerics.  

El ejemplo siguiente crea una expresión dirigida a valores Scalar que aprovechan las ventajas de la función auxiliar Clamp:  
```cs
var expression = _compositor.CreateExpressionAnimation("Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)"
```

Además de una lista de funciones auxiliares, también puedes usar los métodos Constructor integrados de una cadena de expresión, que generarán una instancia de ese tipo en función de los parámetros proporcionados.  

El ejemplo siguiente crea una expresión que define un nuevo valor Vector3 en la cadena de expresión:  
```cs
var expression = _compositor.CreateExpressionAnimation("Offset / Vector3(targetX, targetY, targetZ");
```

Encontrarás la lista completa de constructores y funciones auxiliares en la sección Apéndice, o de cada tipo en la lista siguiente:  
*   [Scalar](#scalar)
*   [Vector2](#vector2)
*   [Vector3](#vector3)
*   [Matrix3x2](#matrix3x2)
*   [Matrix4x4](#matrix4x4)
*   [Quaternion](#quaternion)
*   [Color](#color)  

###Palabras clave de expresión
Puedes aprovechar las "palabras clave" especiales, que se tratan de forma diferente cuando se evalúa la cadena de expresión. Dado que se consideran "palabras clave" no pueden usarse como la parte del parámetro de cadena de sus referencias a propiedades.  
 
|Palabra clave|   Descripción|
|-------|--------------|
|This.StartingValue| Proporciona una referencia al valor de inicio original de la propiedad que se está animando.|
|This.CurrentValue| Proporciona una referencia al valor "conocido" de la propiedad.|
|Pi| Proporciona una referencia de palabra clave al valor de PI.|

El ejemplo siguiente muestra el uso de la palabra clave this.StartingValue:  
```cs
var expression = _compositor.CreateExpressionAnimation("this.StartingValue + delta");
```

###Expresiones con condicionales
Además de admitir relaciones matemáticas con operadores, referencias a propiedades, y funciones y constructores, también puedes crear una expresión que contenga un operador ternario:  
```
(condition ? ifTrue_expression : ifFalse_expression)
```

Las instrucciones condicionales permiten escribir expresiones para que, siguiendo una condición concreta, el sistema use distintas relaciones matemáticas para calcular el valor de la propiedad de animación. Los operadores ternarios se pueden anidar como las expresiones para las instrucciones true o false.  

Los operadores condicionales siguientes son compatibles con la instrucción condicional: 
*   Es igual a (==)
*   No es igual a (!=)
*   Menor que (<)
*   Menor o igual que (<=)
*   Mayor que (>)
*   Mayor o igual que (>=)  

Las conjunciones siguientes se admiten como operadores o funciones en la instrucción condicional:
*   No: ! / Not(bool1)
*   Y: && / And(bool1, bool2)
*   O: || / Or(bool1, bool2)  

A continuación se incluye un ejemplo de una animación de expresión con una expresión condicional.  
```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f + (target.Offset.x / parent.Offset.x) : 1.0f");
```

##Fotogramas clave de expresión
Anteriormente en este documento, hemos descrito cómo crear animaciones de fotograma clave y presentado las animaciones de expresión y todos los componentes que puedes usar para formar la cadena de expresión. ¿Qué ocurre si quieres la eficacia de las animaciones de expresión, pero la interpolación de tiempo que ofrecen las animaciones de fotograma clave? La respuesta son los fotogramas clave de expresión.  

En lugar de definir un valor discreto para cada punto de control de la animación de fotograma clave, puedes hacer que el valor sea una cadena de expresión. En esta situación, el sistema usará la cadena de expresión para calcular cuál debe ser el valor de la propiedad de animación en el punto especificado en la línea de tiempo. A continuación, el sistema se interpola en este valor como en una animación de fotograma clave normal.    

No es necesario crear animaciones especiales para usar fotogramas clave de expresión: simplemente inserta un objeto ExpressionKeyFrame en la animación de fotograma clave estándar, y proporciona el tiempo y la cadena de expresión como el valor. Esto se muestra en el ejemplo siguiente con una cadena de expresión como el valor de uno de los fotogramas clave:   
```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

##Ejemplo de expresión
El siguiente código muestra un ejemplo de configuración de una animación de expresión para una experiencia de efecto parallax básica que extrae los valores de entrada del Visor de desplazamiento.
```cs
// Get scrollviewer
ScrollViewer myScrollViewer = ThumbnailList.GetFirstDescendantOfType<ScrollViewer>();
_scrollProperties = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);

// Setup the expression
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 *  ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5   * ItemHeight))";
```

##Animación con conjuntos de propiedades
Los conjuntos de propiedades de composición proporcionan la capacidad de almacenar valores que se pueden compartir entre varias animaciones y no están vinculados a la duración de otro objeto de composición. Los conjuntos de propiedades son muy útiles para almacenar valores comunes y, después, hacer referencia a estos fácilmente en las animaciones. También puedes usar los conjuntos de propiedades para almacenar datos en función de la lógica de la aplicación para controlar una expresión.  

Para crear un conjunto de propiedades, usa el método constructor fuera del objeto Compositor:  
```cs
_sharedProperties = _compositor.CreatePropertySet();
```

Cuando hayas creado tu conjunto de propiedades, le puedes agregar una propiedad y un valor:  
```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

El proceso es similar al que hemos visto anteriormente, se puede hacer referencia a este valor de conjunto de propiedades en una animación de expresión:  
```cs
var expression = _compositor.CreateExpressionAnimation("this.target.Offset + sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
targetVisual.StartAnimation("Offset", expression);
```

Los valores de conjunto de propiedades también se pueden animar. Para ello, asocia la animación al objeto PropertySet y, después, haz referencia al nombre de propiedad de la cadena. A continuación, animamos la propiedad NewOffset del conjunto de propiedades mediante una animación de fotograma clave.  
```cs
var keyFrameAnimation = _compositor.CreateVector3KeyFrameAnimation()
keyFrameAnimation.InsertKeyFrame(0.5f, new Vector3(25.0f, 68.0f, 0.0f);
keyFrameAnimation.InsertKeyFrame(1.0f, new Vector3(89.0f, 125.0f, 0.0f);
_sharedProperties.StartAnimation("NewOffset", keyFrameAnimation);
```


Si este código se ejecuta en una aplicación, te preguntarás qué sucede en el valor de propiedad animada a la que está asociada la animación de expresión. En esta situación, la expresión se generaría inicialmente en un valor, pero una vez que la animación de fotograma clave comience a animar la propiedad en el conjunto de propiedades, el valor de expresión también se actualizará, ya que es la ecuación se calcula en cada fotograma. Este es el atractivo de los conjuntos de propiedades con las animaciones de expresión y las animaciones de fotograma clave.  
 
##ManipulationPropertySet
Además de usar los conjuntos de propiedades de composición, un desarrollador también es capaz de obtener acceso a la propiedad ManipulationPropertySet que permite el acceso a las propiedades fuera de un control ScrollViewer de XAML. Es posible usar y hacer referencia a estas propiedades en una animación de expresión para impulsar experiencias como las del efecto parallax y los encabezados permanentes. Nota: Puedes tomar el control ScrollViewer de cualquier control XAML (ListView, GridView, etc.) que tenga contenido desplazable y usarlo para obtener la propiedad ManipulationPropertySet de esos controles desplazables.  

En la expresión, es posible hacer referencia a las siguientes propiedades del Visor de desplazamiento:  

|Propiedad| Tipo|  
|--------|-----|  
|Translation| Vector3|  
|Pan| Vector3|  
|Scale| Vector3|  
|CenterPoint| Vector3|  
|Matrix| Matrix4x4|  

Para obtener una referencia a la ManipulationPropertySet, usa el método GetScrollViewerManipulationPropertySet de ElementCompositionPreview.  
```csharp
CompositionPropertySet manipPropSet = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScroller);
```

Cuando tengas una referencia a este conjunto de propiedades, puedes hacer referencia a las propiedades del Visor de desplazamiento que se encuentran en el conjunto de propiedades. El primer paso es crear el parámetro de referencia.  
```csharp
ExpressionAnimation exp = compositor.CreateExpressionAnimation();
exp.SetReferenceParameter("ScrollManipulation", manipPropSet);
```

Después de configurar el parámetro de referencia, puedes hacer referencia a las propiedades de ManipulationPropertySet en la expresión.  
```csharp
exp.Expression = “ScrollManipulation.Translation.Y / ScrollBounds”;
_target.StartAnimation(“Opacity”, exp);
```



 
 
##Apéndice
###Funciones de expresión por tipo de estructura
###Scalar  

|Operaciones con funciones y constructores| Descripción|  
|-----------------------------------|--------------|  
|Abs(Float value)|  Devuelve un valor Float que representa el valor absoluto del parámetro Float.|  
|Clamp(Float value, Float min, Float max)|  Devuelve un valor Float que es mayor que min y menor que max, o min si el valor es menor que min o max si el valor es mayor que max|  
|Max (Float value1, Float value2)|  Devuelve el valor Float más alto entre value1 y value2.|  
|Min (Float value1, Float value2)|  Devuelve el valor Float más bajo entre value1 y value2.|  
|Lerp(Float value1, Float value2, Float progress)|  Devuelve un valor Float que representa el cálculo de interpolación lineal realizado entre los dos valores Scalar basado en el progreso (nota: El progreso es entre 0,0 y 1,0).|  
|Slerp(Float value1, Float value2, Float progress)| Devuelve un valor Float que representa la interpolación esférica calculada entre los dos valores Float basada en el progreso (nota: El progreso es entre 0,0 y 1,0).|  
|Mod(Float value1, Float value2)|   Devuelve el resto del valor Float resultante de la división de value1 y value2.|  
|Ceil(Float value)|     Devuelve el parámetro Float redondeado al siguiente número entero mayor.|  
|Floor(Float value)|    Devuelve el parámetro Float hasta el siguiente número entero menor.|  
|Sqrt(Float value)| Devuelve la raíz cuadrada del parámetro Float.|  
|Square(Float value)|   Devuelve el cuadrado del parámetro Float.|  
|Sin(Float value1)||
|Asin(Float value2)|    Devuelve el valor Sin o ArcSin del parámetro Float.|
|Cos(Float value1)||
|ACos(Float value2)|    Devuelve el valor Cos o ArcCos del parámetro Float.|
|Tan(Float value1)||
|ATan(Float value2)|    Devuelve el valor Tan o ArcTan del parámetro Float.|
|Round(Float value)|    Devuelve el parámetro Float redondeado al número entero más próximo.|
|Log10(Float value)|    Devuelve el resultado del registro (base 10) del parámetro Float.|
|Ln(Float value)|   Devuelve el resultado del registro natural del parámetro Float.|
|Pow(Float value, Float power)| Devuelve el resultado del parámetro Float elevado a una potencia determinada.|
|ToDegrees(Float radians)|  Devuelve el parámetro Float convertido a grados.|
|ToRadians(Float degrees)|  Devuelve el parámetro Float convertido a radianes.|

###Vector2  

|Operaciones con funciones y constructores|   Descripción|
|-----------------------------------|--------------|
|Abs (Vector2 value)|   Devuelve un valor Vector2 con el valor absoluto aplicado a cada componente.|
|Clamp (Vector2 value1, Vector2 min, Vector2 max)|  Devuelve un valor Vector2 que contiene los valores comprendidos de cada componente respectivo.|
|Max (Vector2 value1, Vector2 value2)|  Devuelve un valor Vector2 con ha obtenido Max en cada uno de los componentes correspondientes de value1 y value2.|
|Min (Vector2 value1, Vector2 value2)|  Devuelve un valor Vector2 con ha obtenido Min en cada uno de los componentes correspondientes de value1 y value2.|
|Scale(Vector2 value, Float factor)|    Devuelve un valor Vector2 con cada componente del vector multiplicado por el factor de escala.|
|Transform(Vector2 value, Matrix3x2 matrix)|    Devuelve un valor Vector2 resultante de la transformación lineal entre Vector2 y Matrix3x2 (lo que también se conoce como multiplicar un vector por una matriz).|
|Lerp(Vector2 value1, Vector2 value2, Float progress)|  Devuelve un valor Vector2 que representa el cálculo de interpolación lineal realizado entre los dos valores Vector2 basado en el progreso (nota: El progreso es entre 0,0 y 1,0).|
|Length(Vector2 value)| Devuelve un valor Float que representa la longitud o la magnitud del valor Vector2.|
|LengthSquared(Vector2)|    Devuelve un valor Float que representa el cuadrado de la longitud o la magnitud de un valor Vector2.|
|Distance(Vector2 value1, Vector2 value2)|  Devuelve un valor Float que representa la distancia entre dos valores Vector2.|
|DistanceSquared(Vector2 value1, Vector2 value2)|   Devuelve un valor Float que representa el cuadrado de la distancia entre dos valores Vector2.|
|Normalize(Vector2 value)|  Devuelve un valor Vector2 que representa el vector de unidad del parámetro donde se han normalizado todos los componentes.|
|Vector2(Float x, Float y)| Construye un valor Vector2 con dos parámetros Float.|

###Vector3  

|Operaciones con funciones y constructores|   Descripción|
|-----------------------------------|--------------|
|Abs (Vector3 value)|   Devuelve un valor Vector3 con el valor absoluto aplicado a cada componente.|
|Clamp (Vector3 value1, Vector3 min, Vector3 max)|  Devuelve un valor Vector3 que contiene los valores comprendidos de cada componente respectivo.|
|Max (Vector3 value1, Vector3 value2)|  Devuelve un valor Vector3 con ha obtenido Max en cada uno de los componentes correspondientes de value1 y value2.|
|Min (Vector3 value1, Vector3 value2)|  Devuelve un valor Vector3 con ha obtenido Min en cada uno de los componentes correspondientes de value1 y value2.|
|Scale(Vector3 value, Float factor)|    Devuelve un valor Vector3 con cada componente del vector multiplicado por el factor de escala.|
|Lerp(Vector3 value1, Vector3 value2, Float progress)|  Devuelve un valor Vector3 que representa el cálculo de interpolación lineal realizado entre los dos valores Vector3 basado en el progreso (nota: El progreso es entre 0,0 y 1,0).|
|Length(Vector3 value)| Devuelve un valor Float que representa la longitud o la magnitud del valor Vector3.|
|LengthSquared(Vector3)|    Devuelve un valor Float que representa el cuadrado de la longitud o la magnitud de un valor Vector3.|
|Distance(Vector3 value1, Vector3 value2)|  Devuelve un valor Float que representa la distancia entre dos valores Vector3.|
|DistanceSquared(Vector3 value1, Vector3 value2)|   Devuelve un valor Float que representa el cuadrado de la distancia entre dos valores Vector3.|
|Normalize(Vector3 value)|  Devuelve un valor Vector3 que representa el vector de unidad del parámetro donde se han normalizado todos los componentes.|
|Vector3(Float x, Float y, Float z)|    Construye un valor Vector3 con tres parámetros Float|

###Vector4  

|Operaciones con funciones y constructores|   Descripción|
|-----------------------------------|--------------|
|Abs (Vector4 value)|   Devuelve un valor Vector3 con el valor absoluto aplicado a cada componente.|
|Clamp (Vector4 value1, Vector4 min, Vector4 max)|  Devuelve un valor Vector4 que contiene los valores comprendidos de cada componente respectivo.|
|Max (Vector4 value1 Vector4 value2)|   Devuelve un valor Vector4 con ha obtenido Max en cada uno de los componentes correspondientes de value1 y value2.|
|Min (Vector4 value1 Vector4 value2)|   Devuelve un valor Vector4 con ha obtenido Min en cada uno de los componentes correspondientes de value1 y value2.|
|Scale(Vector3 value, Float factor)|    Devuelve un valor Vector3 con cada componente del vector multiplicado por el factor de escala.|
|Transform(Vector4 value, Matrix4x4 matrix)|    Devuelve un valor Vector4 resultante de la transformación lineal entre Vector4 y Matrix4x4 (lo que también se conoce como multiplicar un vector por una matriz).|
|Lerp(Vector4 value1, Vector4 value2, Float progress)|  Devuelve un valor Vector4 que representa el cálculo de interpolación lineal realizado entre los dos valores Vector4 basado en el progreso (nota: El progreso es entre 0,0 y 1,0).|
|Length(Vector4 value)| Devuelve un valor Float que representa la longitud o la magnitud del valor Vector4.|
|LengthSquared(Vector4)|    Devuelve un valor Float que representa el cuadrado de la longitud o la magnitud de un valor Vector4.|
|Distance(Vector4 value1, Vector4 value2)|  Devuelve un valor Float que representa la distancia entre dos valores Vector4.|
|DistanceSquared(Vector4 value1, Vector4 value2)|   Devuelve un valor Float que representa el cuadrado de la distancia entre dos valores Vector4.|
|Normalize(Vector4 value)|  Devuelve un valor Vector4 que representa el vector de unidad del parámetro donde se han normalizado todos los componentes.|
|Vector4(Float x, Float y, Float z, Float w)|   Construye un valor Vector4 con cuatro parámetros Float|

###Matrix3x2  

|Operaciones con funciones y constructores|   Descripción|
|-----------------------------------|--------------|
|Scale(Matrix3x2 value, Float factor)|  Devuelve un valor Matrix3x2 con cada componente de la matriz multiplicado por el factor de escala.|
|Inverse(Matrix 3x2 value)| Devuelve un objeto Matrix3x2 que representa la matriz recíproca.|
|Lerp(Matrix3x2 value1, Matrix3x2 value2, Float progress)|  Devuelve un valor Matrix3x2 que representa el cálculo de interpolación lineal realizado entre los dos valores Matrix3x2 basado en el progreso (nota: El progreso es entre 0,0 y 1,0).|
|Matrix3x2(Float M11, Float M12, Float M21, Float M22, Float M31, Float M32)|   Construye un valor Matrix3x2 con 6 parámetros Float.|
|Matrix3x2.CreateFromScale(Vector2 scale)|  Construye un valor Matrix3x2 a partir de un valor Vector2 que representa la escala.<br/>\[scale.X, 0.0<br/> 0.0, scale.Y<br/> 0.0, 0.0 \]|
|Matrix3x2.CreateFromTranslation(Vector2 translation)|  Construye un valor Matrix3x2 a partir de un valor Vector2 que representa la traslación.<br/>\[1.0, 0.0,<br/> 0.0, 1.0,<br/> translation.X, translation.Y\]|
    
###Matrix4x4  

|Operaciones con funciones y constructores|   Descripción|
|-----------------------------------|--------------|
|Scale(Matrix4x4 value, Float factor)|  Devuelve un valor Matrix4x4 con cada componente de la matriz multiplicado por el factor de escala.|
|Inverse(Matrix4x4)|    Devuelve un objeto Matrix4x4 que representa la matriz recíproca.|
|Lerp(Matrix4x4 value1, Matrix4x4 value2, Float progress)|  Devuelve un valor Matrix4x4 que representa el cálculo de interpolación lineal realizado entre los dos valores Matrix4x4 basado en el progreso (nota: El progreso es entre 0,0 y 1,0).|
|Matrix4x4(Float M11, Float M12, Float M13, Float M14,<br/>Float M21, Float M22, Float M23, Float M24,<br/>    Float M31, Float M32, Float M33, Float M34,<br/>    Float M41, Float M42, Float M43, Float M44)| Construye un valor Matrix4x4 con 16 parámetros Float.|
|Matrix4x4.CreateFromScale(Vector3 scale)|  Construye un valor Matrix4x4 a partir de un valor Vector3 que representa la escala.<br/>\[scale.X, 0.0, 0.0, 0.0,<br/> 0.0, scale.Y, 0.0, 0.0,<br/> 0.0, 0.0, scale.Z, 0.0,<br/> 0.0, 0.0, 0.0, 1.0\]|
|Matrix4x4.CreateFromTranslation(Vector3 translation)|  Construye un valor Matrix4x4 a partir de un valor Vector3 que representa la traslación.<br/>\[1.0, 0.0, 0.0, 0.0,<br/> 0.0, 1.0, 0.0, 0.0,<br/> 0.0, 0.0, 1.0, 0.0,<br/> translation.X, translation.Y, translation.Z, 1.0\]|
|Matrix4x4.CreateFromAxisAngle(Vector3 axis, Float angle)|  Construye un valor Matrix4x4 a partir de un eje Vector3 y un valor Float que representan un ángulo.|

###Quaternion  

|Operaciones con funciones y constructores|   Descripción|
|-----------------------------------|--------------|
|Slerp(Quaternion value1, Quaternion value2, Float progress)|   Devuelve un valor Quaternion que representa la interpolación esférica calculada entre los dos valores Quaternion basada en el progreso (nota: El progreso es entre 0,0 y 1,0).|
|Concatenate(Quaternion value1 Quaternion value2)|  Devuelve un valor Quaternion que representa una concatenación de dos valores Quaternion (también conocido como un valor Quaternion que representa dos rotaciones individuales combinadas).|
|Length(Quaternion value)|  Devuelve un valor Float que representa la longitud o la magnitud del valor Quaternion.|
|LengthSquared(Quaternion)| Devuelve un valor Float que representa el cuadrado de la longitud o la magnitud de un valor Quaternion.|
|Normalize(Quaternion value)|   Devuelve un valor Quaternion cuyos componentes se han normalizado.|
|Quaternion.CreateFromAxisAngle(Vector3 axis, Scalar angle)|    Crea un valor Quaternion a partir de un eje Vector3 y un valor Scalar que representan un ángulo.|
|Quaternion(Float x, Float y, Float z, Float w)|    Construye un valor Quaternion a partir de cuatro valores Float.|

###Color

|Operaciones con funciones y constructores|   Descripción|
|-----------------------------------|--------------|
|ColorLerp(Color colorTo, Color colorFrom, Float progress)| Devuelve un objeto Color que representa el valor de interpolación lineal calculado entre dos objetos de color en función de un progreso determinado. (Nota: El Progreso es entre 0,0 y 1,0).|
|ColorLerpRGB(Color colorTo, Color colorFrom, Float progress)|  Devuelve un objeto Color que representa el valor de interpolación lineal calculado entre dos objetos en función de un progreso determinado en el espacio de colores RGB.|
|ColorLerpHSL(Color colorTo, Color colorFrom, Float progress)|  Devuelve un objeto Color que representa el valor de interpolación lineal calculado entre dos objetos en función de un progreso determinado en el espacio de colores HSL.|
|ColorArgb(Float a, Float r, Float g, Float b)| Construye un objeto que representa el valor Color definido por los componentes ARGB.|
|ColorHsl(Float h, Float s, Float l)|   Construye un objeto que representa el valor Color definido por los componentes HSL (nota: El tono se define a partir de 0 y 2pi)|







<!--HONumber=Jul16_HO1-->


