---
author: Jwmsft
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: Animaciones con guion gráfico
description: Las animaciones con guion gráfico no son simples animaciones en el sentido visual.
ms.author: jimwalk
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7e4aa010915ba681869b4ae27ba63e081a31ef78
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5705437"
---
# <a name="storyboarded-animations"></a>Animaciones con guion gráfico

Las animaciones de guión gráfico no son solo animaciones en un sentido visual. Una animación de guión gráfico es una forma de cambiar el valor de una propiedad de dependencia como una función de tiempo. Una de las razones principales por las que quizás necesites una animación con guión gráfico que no provenga de la Biblioteca de animaciones es para definir el estado visual de un control, como parte de una plantilla de control o una definición de página.

## <a name="differences-with-silverlight-and-wpf"></a>Diferencias con Silverlight y WPF

Si estás familiarizado con Microsoft Silverlight o Windows Presentation Foundation (WPF), lee esta sección; si no, puedes saltártela.

En general, la creación de animaciones de guión gráfico en una aplicación de Windows Runtime es como Silverlight o WPF. Aunque existe una serie de diferencias importantes:

-   Las animaciones de guión gráfico no son la única forma de animar visualmente una interfaz de usuario para una aplicación. Tampoco representan la forma de animación más sencilla para los desarrolladores de aplicaciones. En lugar de usar animaciones de guión gráfico, muchas veces un procedimiento de diseño más recomendado es usar animaciones de tema y animaciones de transición. Estas animaciones pueden crear rápidamente animaciones recomendadas para la interfaz de usuario sin necesidad de meterse en la complejidad que implica la selección de propiedades de animación como destino. Para obtener más información, consulta [Información general sobre animaciones](xaml-animation.md).
-   En Windows Runtime, muchos controles XAML incluyen animaciones de tema y animaciones de transición como parte de su comportamiento integrado. Para la mayoría, los controles de WPF y Silverlight no tenían un comportamiento de animación predeterminado.
-   No todas las animaciones personalizadas que creas pueden ejecutarse de forma predeterminada en una aplicación de Windows Runtime, si el sistema de animación determina que la animación puede provocar un mal rendimiento en la interfaz de usuario. Las animaciones que según el sistema pueden afectar al rendimiento se llaman *animaciones dependientes*. Son dependientes porque la temporización de tu animación funciona directamente en contra del subproceso de la interfaz de usuario, que además es donde la entrada del usuario activo y otras actualizaciones intentan aplicar los cambios en tiempo de ejecución a la interfaz de usuario. Una animación dependiente que consuma muchos recursos del sistema en el subproceso de la interfaz de usuario puede hacer que la aplicación parezca que no responde en ciertas situaciones. Si tu animación produce un cambio en el diseño o tiene el potencial para afectar al rendimiento del subproceso de la interfaz de usuario, tendrás que habilitar de forma explícita la animación para poder verla en ejecución. Ese es precisamente el cometido de la propiedad **EnableDependentAnimation** en clases de animación específicas. Consulta [Animaciones dependientes e independientes](./storyboarded-animations.md#dependent-and-independent-animations) para obtener más información.
-   Actualmente no se admiten funciones de aceleración personalizadas en Windows Runtime.

## <a name="defining-storyboarded-animations"></a>Definir de animaciones de guion gráfico

Una animación de guion gráfico es una forma de cambiar el valor de una propiedad de dependencia como una función de tiempo. La propiedad que estás animando no siempre es una propiedad que afecta directamente la interfaz de usuario de tu aplicación. Pero dado que XAML está relacionado con la definición de la interfaz de usuario para una aplicación, por lo general, se trata de una propiedad que está relacionada con la interfaz de usuario que estás animando. Por ejemplo, puedes animar el ángulo de [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) o el valor de color de fondo de un botón.

Una de las razones principales por la que quizás quieras definir una animación de guión gráfico es porque eres un autor de control o deseas realizar nuevamente la plantilla de un control, y estás definiendo estados visuales. Para obtener más información, consulta el tema sobre [Animaciones con guion gráfico para estados visuales](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

Ya sea que quieras definir estados visuales o una animación personalizada para una aplicación, los conceptos y las API para las animaciones de guión gráfico que se describen en este tema se aplican para ambos casos.

Para que pueda ser animada, la propiedad que estás seleccionando como destino con una animación de guion gráfico debe ser una *propiedad de dependencia*. Una propiedad de dependencia es una función clave de la implementación XAML de Windows Runtime. Las propiedades que se pueden escribir de los elementos más comunes de la interfaz de usuario se implementan generalmente como propiedades de dependencia, de modo que puedas animarlas, aplicar valores de enlazado a datos, o aplicar un [**Estilo**](https://msdn.microsoft.com/library/windows/apps/BR208849) y seleccionar como destino la propiedad con un [**Establecedor**](https://msdn.microsoft.com/library/windows/apps/BR208817). Si quieres obtener más información sobre cómo funcionan las propiedades de dependencia, consulta [Introducción a las propiedades de dependencia](https://msdn.microsoft.com/library/windows/apps/Mt185583).

La mayoría de las veces, defines una animación de guión gráfico al escribir XAML. Si usas una herramienta como Microsoft Visual Studio, generará el XAML para ti. También es posible definir una animación de guión gráfico mediante código, pero no es tan común.

Veamos un ejemplo simple. En este ejemplo de XAML, la propiedad [**Opacidad**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) se anima en un objeto [**Rectángulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) determinado.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>

  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

### <a name="identifying-the-object-to-animate"></a>Identificar el objeto que se va a animar

En el ejemplo anterior, el guión gráfico animaba la propiedad [**Opacidad**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) de un [**Rectángulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). No debes declarar las animaciones en el objeto en sí. En cambio, tienes que hacerlo dentro de la definición de animación de un guión gráfico. Los guiones gráficos, por lo general, se definen en el XAML que no está junto a la definición de interfaz de usuario de XAML del objeto que quieres animar. En cambio, normalmente se establecen como un recurso XAML.

Para conectar una animación a un destino, debes hacer referencia al destino mediante su nombre de programación de identificación. Siempre debes aplicar el [atributo x:Name](https://msdn.microsoft.com/library/windows/apps/Mt204788) en la definición de interfaz de usuario de XAML para nombrar el objeto que quieres animar. Luego, debes seleccionar como destino el objeto que quieres animar mediante la configuración de [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/Hh759823) dentro de la definición de animación. Para el valor de **Storyboard.TargetName**, usa la cadena de nombre del objeto de destino, que es el que estableciste antes en otra parte con el atributo x:Name.

### <a name="targeting-the-dependency-property-to-animate"></a>Seleccionar como destino la propiedad de dependencia que se va a animar

Establece un valor para [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824) en la animación. De esta forma, se determina qué propiedad específica del objeto de destino se anima.

A veces debes seleccionar como destino una propiedad que no es una propiedad inmediata del objeto de destino, pero que se anida más profundamente en una relación de objeto-propiedad. Muchas veces debes hacer esto para explorar en profundidad un conjunto de objetos de contribución y valores de propiedades hasta que puedas hacer referencia a un tipo de propiedad que se pueda animar ([**Doble**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Punto**](https://msdn.microsoft.com/library/windows/apps/BR225870), [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)). Este concepto se denomina *selección indirecta de destino*y la sintaxis para seleccionar una propiedad como destino de esta manera se conoce como *ruta de propiedad*.

Observa el siguiente ejemplo. Un escenario común para una animación con guión gráfico es cambiar el color de una parte de la interfaz de usuario o el control de una aplicación para representar que el control tiene un estado en particular. Supongamos que quieres animar el [**Primer plano**](https://msdn.microsoft.com/library/windows/apps/BR209665) de un [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652), de modo que cambie de rojo a verde. Seguro que esperas que aparezca [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066). Estás en lo cierto. Sin embargo, ninguna de las propiedades de los elementos de la interfaz de usuario que afectan al color del objeto son realmente del tipo [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723). Por el contrario, son del tipo [**Pincel**](/uwp/api/Windows.UI.Xaml.Media.Brush). Por lo tanto, lo que en realidad debes seleccionar como destino para la animación es la propiedad [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) de la clase [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), que es un tipo de derivado de **Pincel** que se usa normalmente para estas propiedades de interfaz de usuario relacionadas con color. Este es el aspecto en términos de formación de una ruta de acceso de propiedades para la selección de propiedad de tu animación:

```xaml
<Storyboard x:Name="myStoryboard">
  <ColorAnimation
    Storyboard.TargetName="tb1"
    Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
    From="Red" To="Green"/>
</Storyboard>
```

Aquí te mostramos lo que debes tener en cuenta en la sintaxis en cuanto a sus partes:

- Cada conjunto de paréntesis () encierra el nombre de una propiedad.
- Dentro del nombre de la propiedad, hay un punto, y ese punto separa un nombre de tipo y un nombre de propiedad, para que la propiedad que estás identificando no sea ambigua.
- El punto del medio, el que está fuera de los paréntesis, es un paso. Lo que la sintaxis interpreta es tomar el valor de la primera propiedad (que es un objeto), meterse en el modelo de su objeto y seleccionar como destino una subpropiedad específica del valor de la primera propiedad.

Aquí hay una lista de escenarios de selección de destino de animaciones en los que probablemente uses la selección indirecta de destino de propiedades y algunas cadenas de rutas de acceso de propiedades que se aproximan a la sintaxis que debes usar:

- Animar el valor [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) de la clase [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), tal como se aplica a la propiedad [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980): `(UIElement.RenderTransform).(TranslateTransform.X)`
- Animar un [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) dentro de la clase [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) de [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108), tal como se aplica a la propiedad [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill): `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
- Animar el valor [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) de la clase [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), que es una de cuatro transformaciones en [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022), tal como se aplica a la propiedad [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980):`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

Notarás que en algunos de estos ejemplos se usan corchetes para encerrar los números. Es un indizador. Indica que el nombre de la propiedad que precede tiene una colección como valor y que quieres un elemento (identificado por un índice basado en cero) de la colección.

También puedes animar propiedades adjuntas de XAML. Siempre encierra el nombre completo de la propiedad adjunta entre paréntesis, por ejemplo, `(Canvas.Left)`. Para obtener más información, consulta [Animación de propiedades adjuntas de XAML](./storyboarded-animations.md#animating-xaml-attached-properties).

Para más información sobre cómo usar una ruta de propiedad para la selección indirecta como destino de la propiedad que deseas animar, consulta [Sintaxis de property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586) o [**propiedade anexada Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824).

### <a name="animation-types"></a>Tipos de animación

El sistema de animación de Windows Runtime tiene tres tipos específicos a los que se pueden aplicar animaciones con guión gráfico:

-   [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) se puede animar con cualquier clase [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)
-   [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) se puede animar con cualquier clase [**PointAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210346)
-   [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) se puede animar con cualquier clase [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066)

También hay un tipo de animación [**Objeto**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) generalizada para los valores de referencia del objeto, que analizaremos más adelante.

### <a name="specifying-the-animated-values"></a>Especificar los valores animados

Hasta el momento te hemos mostrado cómo seleccionar como destino el objeto y la propiedad que quieras animar, pero aún no hemos descrito qué hace la animación en el valor de la propiedad cuando se ejecuta.

Los tipos de animación que hemos descrito, muchas veces se conocen como animaciones **From**/**To**/**By**. Esto significa que la animación cambia el valor de una propiedad a lo largo del tiempo, mediante una o varias de las entradas que provienen de la definición de la animación:

-   El valor comienza en el valor **De origen**. Si no especificas un valor **De origen**, el valor inicial es cualquier valor que la propiedad animada tenga en el momento anterior a que se ejecute la animación. Puede ser un valor predeterminado, un valor de un estilo o una plantilla, o un valor específicamente aplicado por código de aplicación o una definición de interfaz de usuario de XAML.
-   Al final de la animación, el valor es **De destino**.
-   O bien, para especificar un valor final relativo al valor inicial, configura la propiedad **Por**. Deberías configurar esto en lugar de la propiedad **De destino**.
-   Si no especificas un valor **De destino** o un valor **Por**, el valor final es cualquier valor que la propiedad animada tenga en el momento anterior a que se ejecute la animación. En este caso, es mejor tener un valor **De origen** porque, de lo contrario, la animación no cambiará el valor. Los valores iniciales y finales son los mismos.
-   Generalmente, una animación tiene al menos uno de los valores **De origen**, **Por** o **De destino**, pero nunca los tres juntos.

Volvamos al ejemplo de XAML anterior y observemos nuevamente los valores **De origen** y **De destino**, y **Duración**. En el ejemplo se está animando la propiedad [**Opacidad**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity), y el tipo de propiedad de **Opacidad** es [**Doble**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Entonces, la animación que debemos usar aquí es [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136).

`From="1.0" To="0.0"` especifica que cuando la animación se ejecuta, la propiedad [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) comienza con el valor 1 y se anima hasta el 0. En otras palabras, en términos de lo que estos valores [**Doble**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) representan para la propiedad **Opacidad**, la animación provocará que el objeto comience siendo opaco y después se atenúe hasta convertirse en transparente.

```xaml
...
<Storyboard x:Name="myStoryboard">
  <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
</Storyboard>
...
```

`Duration="0:0:1"` especifica la duración de la animación, es decir, la rapidez con la que se atenúa el rectángulo. Una propiedad [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207) se especifica con el formato del tipo *horas*:*minutos*:*segundos*. La duración en este ejemplo es de un segundo.

Para más información sobre los valores [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR242377) y la sintaxis XAML, consulta [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR242377).

> [!NOTE]
> En el ejemplo que hemos mostrado, si tuvieras la seguridad de que el estado inicial del objeto que se está animando tiene siempre el valor [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) establecido en 1, ya sea a través de un conjunto explícito o predeterminado, podrías omitir el valor **From**, la animación usaría el valor inicial implícito y el resultado sería el mismo.

### <a name="fromtoby-are-nullable"></a>From/To/By pueden tener valores null

Antes mencionamos que puedes omitir **De origen**, **De destino** o **Por** y, por ende, usar los valores actuales no aminados como sustitutos para un valor que falta. Las propiedades **From**, **To** o **By** de una animación no son del tipo que puedas adivinar. Por ejemplo, el tipo de la propiedad [**DoubleAnimation.To**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) no es [**Doble**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). En cambio, es un [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx) para **Double**. Y su valor predeterminado es **null**, no 0. Este valor **null** es la forma en la que el sistema de animación distingue que no has establecido específicamente un valor para una propiedad **From**, **To** o **By**. Las extensiones de componentes VisualC ++ (C++ / CX) no tiene un tipo **Nullable** , por lo que usan [**IReference**](https://msdn.microsoft.com/library/windows/apps/BR225864) en su lugar.

### <a name="other-properties-of-an-animation"></a>Otras propiedades de una animación

Las próximas propiedades que se describen en esta sección son todas opcionales, ya que tienen valores predeterminados que son adecuados para la mayoría de las animaciones.

### **<a name="autoreverse"></a>AutoReverse**

Si no especificas [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) o [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) en una animación, la animación se ejecutará una vez y durante el tiempo especificado en [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207).

La propiedad [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) especifica si se reproduce la línea de tiempo a la inversa después de que llega al final de su [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207). Si lo estableces como **true**, la animación se invierte después de llegar al final del valor de [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207) declarado, y cambia el valor de su valor final (**De destino**) nuevamente a su valor original (**De origen**). Esto significa que la animación se ejecuta de manera eficaz durante el doble de tiempo de su [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207).

### **<a name="repeatbehavior"></a>RepeatBehavior**

La propiedad [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) especifica cuántas veces se reproduce una línea de tiempo, o una duración más prolongada dentro de la cual debe repetirse la línea de tiempo. De manera predeterminada, una línea de tiempo tiene un recuento de iteraciones de "1x", lo que significa que se reproduce una vez para su [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207) y no se repite.

Puedes lograr que la animación ejecute varias iteraciones. Por ejemplo, un valor de "3 x" hace que la animación se ejecute tres veces. O bien, puedes especificar una [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) diferente para [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211). Esa **Duración** no debe ser mayor que la **Duración** de la animación en sí para que sea eficaz. Por ejemplo, si especificas un **RepeatBehavior** de "0:0:10", para una animación que tiene una [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207) de "0:0:2", la animación se repite cinco veces. Si no se dividen en partes iguales, la animación se trunca en el momento en que se alcanza el tiempo de **RepeatBehavior**, que puede ser la mitad. Por último, puedes especificar el valor especial "Forever" para hacer que la animación se ejecute de manera indefinida hasta que se la detenga.

Para más información sobre los valores [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411) y la sintaxis XAML, consulta [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411).

### **<a name="fillbehaviorstop"></a>FillBehavior="Stop"**

De manera predeterminada, cuando una animación finaliza, la animación deja el valor de la propiedad como el valor **De destino** final o el valor modificado por **Por** incluso después de que se supere su duración. Pero, si estableces el valor de la propiedad [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) en [**FillBehavior.Stop**](https://msdn.microsoft.com/library/windows/apps/BR210306), el valor del valor animado se revierte al valor que estaba antes de que se aplique la animación o, más precisamente, al valor actual eficaz determinado por el sistema de propiedades de dependencia (para obtener más información sobre esta distinción, consulta [Introducción a las propiedades de dependencia](https://msdn.microsoft.com/library/windows/apps/Mt185583)).

### **<a name="begintime"></a>BeginTime**

De manera predeterminada, el [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) de una animación es "0:0:0", para que comience tan pronto como se ejecute el [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) que contiene. Puedes cambiar esto si el **Guión gráfico** contiene más de una animación y quieres escalonar las horas de inicio de las otras animaciones contra una animación inicial, o para crear una breve demora deliberada.

### **<a name="speedratio"></a>SpeedRatio**

Si tienes más de una animación en un [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) puedes cambiar la frecuencia de tiempo de una o varias de las animaciones relativas al **Guión gráfico**. Es el **Guión gráfico** principal que controla en última instancia de qué manera transcurre el tiempo de la [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR242377) mientras se ejecutan las animaciones. Esta propiedad no se usa con mucha frecuencia. Para obtener más información, consulta [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/BR243213).

## <a name="defining-more-than-one-animation-in-a-storyboard"></a>Definir más de una animación en un **Guión gráfico**

El contenido de un [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) puede comprender más de una definición de animación. Es posible que tengas más de una animación si estás aplicando animaciones relacionadas a dos propiedades del mismo objeto de destino. Por ejemplo, puedes cambiar las propiedades [**TranslateX**](https://msdn.microsoft.com/library/windows/apps/BR228122) y [**TranslateY**](https://msdn.microsoft.com/library/windows/apps/BR228124) de [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) usadas como [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) de un elemento de la interfaz de usuario. Esto generará que el elemento se traduzca diagonalmente. Para lograrlo, necesitas dos animaciones diferentes. Es posible que quieras que las animaciones formen parte del mismo **Guión gráfico** porque siempre quieres que esas dos animaciones se ejecuten juntas.

No es necesario que las animaciones sean del mismo tipo o seleccionen como destino el mismo objeto. Pueden tener distintas duraciones y no compartir ningún valor de las propiedades.

Cuando se ejecuta el [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) principal, se ejecutarán también cada una de las animaciones.

La clase [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490), en realidad, tiene muchas propiedades de animación iguales que los tipos de animación, ya que ambas comparten la clase base de la [**Línea de tiempo**](https://msdn.microsoft.com/library/windows/apps/BR210517). De este modo, un **Guión gráfico** puede tener [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) o [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204). Por lo general, estos valores no se configuran en un **Guion gráfico** a menos que quieras que todas las animaciones tengan ese comportamiento. Como regla general, cualquier propiedad **Línea de tiempo** configurada en un **Guión gráfico** se aplica a todas sus animaciones secundarias. Si lo dejas sin configurar, el **Guión gráfico** tiene una duración implícita que se calcula desde el valor [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR242377) de las animaciones contenidas. Una [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207) configurada explícitamente en un **Guión gráfico** que es más breve que la de sus animaciones secundarias provocará que la animación se corte, algo que nadie quiere.

Un guion gráfico no puede contener dos animaciones que intenten seleccionar como destino y animar la misma propiedad en el mismo objeto. Si lo intentas, obtendrás un error en tiempo de ejecución cuando el guion gráfico intente ejecutarse. Esta restricción se aplica aunque las animaciones no se superpongan en tiempo debido a distintas duraciones y valores [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) deliberados. Si realmente quieres aplicar una línea de tiempo de animación más compleja a la misma propiedad en un solo guión gráfico, la forma de hacerlo es usar una animación de fotograma clave. Consulta [Fotograma clave y animaciones de función de aceleración](key-frame-and-easing-function-animations.md).

El sistema de animación puede aplicar más de una animación al valor de una propiedad, si dichas entradas provienen de varios guiones gráficos. Usar este comportamiento de manera deliberada para ejecutar simultáneamente guiones gráficos no es común. Sin embargo, es posible que una animación definida por la aplicación que aplicaste a una propiedad de control modifique el valor **HoldEnd** de una animación que se ejecutó previamente como parte del modelo de estado visual del control.

## <a name="defining-a-storyboard-as-a-resource"></a>Definir un guión gráfico como un recurso

Un [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) es el contenedor en el que colocas los objetos de la animación. Por lo general, defines el **Guión gráfico** como un recurso que está disponible en el objeto que quieres animar, ya sea en [**Recursos**](https://msdn.microsoft.com/library/windows/apps/BR208740) o [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/BR242338).

En el ejemplo siguiente, se muestra de qué manera en el ejemplo anterior el [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) formaría parte de una definición de [**Recursos**](https://msdn.microsoft.com/library/windows/apps/BR208740) de nivel de página, donde el **Guión gráfico** es un recurso con clave de la raíz [**Página**](https://msdn.microsoft.com/library/windows/apps/BR227503). Observa el [atributo x:Name](https://msdn.microsoft.com/library/windows/apps/Mt204788). Este atributo indica cómo defines un nombre de variable para el **Guión gráfico** para que otros elementos de XAML, así como el código, puedan hacer referencia al **Guión gráfico** más adelante.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

Definir recursos en la raíz XAML de un archivo XAML como page.xaml o app.xaml es una práctica común para organizar recursos con clave en tu XAML. También puedes incluir recursos en archivos independientes y combinarlos en aplicaciones o páginas. Para más información, consulta [Referencias a ResourceDictionary y a recursos XAML](https://msdn.microsoft.com/library/windows/apps/Mt187273).

> [!NOTE]
> El XAML de Windows Runtime admite la identificación de recursos con el [atributo x:Key](https://msdn.microsoft.com/library/windows/apps/Mt204787) o el [atributo x:Name](https://msdn.microsoft.com/library/windows/apps/Mt204788). Usar el atributo x:Name es más común para un [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) porque posiblemente quieras hacer referencia a él finalmente por un nombre de variable y llamar a su método [**Comenzar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) y ejecutar las animaciones. Si usas el [atributo x:Key](https://msdn.microsoft.com/library/windows/apps/Mt204787), deberás usar los métodos [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) como el indexador [**Elemento**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.item) para recuperarlo como un recurso con clave y después convertir el objeto recuperado en **Guión gráfico** para usar los métodos **Guión gráfico**.

### <a name="storyboards-for-visual-states"></a>Guiones gráficos para estados visuales

También puedes colocar tus animaciones en una unidad del [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) cuando declares las animaciones de estado visual para la apariencia visual de un control. En ese caso, los elementos de **Guión gráfico** que definas irán en un contenedor de [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) anidado más profundamente en un [**Estilo**](https://msdn.microsoft.com/library/windows/apps/BR208849) (el **Estilo** es el recurso con clave). No necesitas una clave o un nombre para tu **Guión gráfico** en este caso porque es el **VisualState** quien tiene un nombre de destino al que puede invocar el [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager). Los estilos para los controles a menudo se incluyen en archivos XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) separados en vez de colocarse en una colección de **Recursos** de una página o aplicación. Para obtener más información, consulta el tema sobre [Animaciones con guion gráfico para estados visuales](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

## <a name="dependent-and-independent-animations"></a>Animaciones dependientes e independientes

En este punto debemos introducir algunas cuestiones importantes sobre el funcionamiento del sistema de animación. En particular, la animación interactúa fundamentalmente con la forma en la que una aplicación de Windows Runtime aparece en la pantalla y la forma en la que esa representación usa subprocesos. Una aplicación de Windows Runtime siempre tiene un subproceso de interfaz de usuario principal y este subproceso es responsable de actualizar la pantalla con la información actual. Además, una aplicación de Windows Runtime tiene un subproceso de composición, que se usa para precalcular los diseños inmediatamente antes de que se muestren. Cuando animas la interfaz de usuario, existe un potencial de generar mucho trabajo para el subproceso de la interfaz de usuario. El sistema debe volver a dibujar grandes áreas de la pantalla usando intervalos de tiempo bastante cortos entre cada actualización. Esto es necesario para capturar el último valor de propiedad de la propiedad animada. Si no eres cuidadoso, hay riesgos de que una animación pueda hacer que la interfaz de usuario tenga menos capacidad de respuesta o impacte en el rendimiento de las demás funciones de la aplicación que se encuentran en el mismo subproceso de la interfaz de usuario.

La variedad de animación que está determinada por imponer cierto riesgo de ralentizar el subproceso de la interfaz de usuario se denomina *animación dependiente*. Una animación que no está sujeta a este riesgo es una *animación independiente*. La distinción entre animaciones dependientes e independientes no está determinada solamente por los tipos de animación ([**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136), etc.) tal como describimos anteriormente. Más bien, está determinada por las propiedades específicas que se animan y otros factores como la herencia y la composición de controles. Hay circunstancias en las que aunque una animación cambie la interfaz de usuario, la animación puede tener un impacto mínimo en el subproceso de la interfaz de usuario y, en cambio, puede controlarse con el subproceso de composición como una animación independiente.

Una animación es independiente si presenta alguna de estas características:

-   La [**Duración**](https://msdn.microsoft.com/library/windows/apps/BR243207) de la animación es de 0 segundos (consulta la advertencia).
-   La animación selecciona como destino [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)
-   La animación selecciona como destino un valor de subpropiedad de estas propiedades de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911): [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx), [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980), [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection.aspx), [**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)
-   La animación selecciona como destino [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) o [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772)
-   La animación selecciona como destino un valor [**Pincel**](/uwp/api/Windows.UI.Xaml.Media.Brush) y usa [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) para animar su [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)
-   La animación es un [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)

> [!WARNING]
> Para que la animación se trate como independiente, debes establecer explícitamente `Duration="0"`. Por ejemplo, si quitas `Duration="0"` de este XAML, la animación se trata como dependiente, aunque el [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR243169) del marco es "0:0:0".

```xaml
<Storyboard>
  <DoubleAnimationUsingKeyFrames
    Duration="0"
    Storyboard.TargetName="Button2"
    Storyboard.TargetProperty="Width">
    <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200"/>
  </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Si tu animación no cumple con estos criterios, probablemente sea una animación dependiente. De manera predeterminada, el sistema de animación no ejecutará una animación dependiente. Por lo tanto, durante el proceso de desarrollo y prueba, es posible que no veas tu animación en ejecución. De todos modos, puedes usar este tipo de animación, pero debes habilitar de manera específica cada animación dependiente. Para habilitar una animación, establece la propiedad **EnableDependentAnimation** en el objeto de animación como **true**. (Cada subclase de [**Línea de tiempo**](https://msdn.microsoft.com/library/windows/apps/BR210517) que representa una animación tiene una implementación diferente de la propiedad, pero todas se llaman `EnableDependentAnimation`).

El requisito de habilitar animaciones dependientes que recae sobre el desarrollador de la aplicación es un aspecto de diseño consciente del sistema de animación y la experiencia de desarrollo. Queremos que los desarrolladores estén al tanto de que las animaciones tienen un costo de rendimiento para la capacidad de respuesta de la interfaz de usuario. Las animaciones con un rendimiento pobre son difíciles de aislar y depurar en una aplicación de escala total. Por lo tanto, es mejor activar solo las animaciones dependientes que realmente necesitas para la experiencia de interfaz de usuario de tu aplicación. No queríamos que fuera tan fácil comprometer el rendimiento de tu aplicación por el solo hecho de incluir animaciones decorativas que usan muchos ciclos. Para más información sobre sugerencias de rendimiento, consulta [Optimizar las animaciones y multimedia](https://msdn.microsoft.com/library/windows/apps/Mt204774).

Como desarrollador de una aplicación, también puedes optar por aplicar una configuración en toda la aplicación que siempre deshabilite las animaciones dependientes, incluso aquellas que tienen la opción **EnableDependentAnimation** establecida como **true**. Consulta [**Timeline.AllowDependentAnimations**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.allowdependentanimations).

> [!TIP]
> Si estás componiendo estados visuales para un control con Visual Studio, el diseñador producirá advertencias cada vez que intentes aplicar una animación dependiente a una propiedad de estado visual.

## <a name="starting-and-controlling-an-animation"></a>Iniciar y controlar una animación

Todo lo que te hemos mostrado hasta el momento no sirve en realidad para ejecutar o aplicar una animación. Hasta que la animación no se inicie y esté en ejecución, los cambios de valor que declara una animación en XAML están latentes y no se producen. Debes iniciar explícitamente una animación de alguna forma que esté relacionada con la vigencia de la aplicación o la experiencia del usuario. En el nivel más simple, inicia una animación al llamar el método [**Comenzar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) en el [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) que sea el elemento principal para la animación. No puedes llamar los métodos de XAML directamente, por lo que todo lo que hagas para habilitar las animaciones, lo estarás haciendo desde el código. Será el código que se encuentra detrás de las páginas o los componentes de tu aplicación, o quizás la lógica de tu control si estás definiendo una clase de control personalizada.

Normalmente, llamas a [**Comenzar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) y dejas que la animación se ejecute hasta completar su duración. Sin embargo, también puedes usar los métodos [**Pausar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx), [**Reanudar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) y [**Detener**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) para controlar el [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) en el tiempo de ejecución, así como otras API que se usan para escenarios de control de animación más avanzados.

Cuando llamas a [**Comenzar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) en un guión gráfico que contiene una animación que se repite indefinidamente (`RepeatBehavior="Forever"`), esta animación se ejecuta hasta que la página que la contiene se descarga o llamas, de manera específica, a [**Pausar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) o [**Detener**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop).

### <a name="starting-an-animation-from-app-code"></a>Iniciar una animación desde el código de la aplicación

Puedes iniciar las animaciones automáticamente o en respuesta a acciones de los usuarios. En el caso automático, generalmente usas un evento de vigencia del objeto como [**Cargado**](https://msdn.microsoft.com/library/windows/apps/BR208723) para que actúe como el desencadenador de la animación. El evento **Cargado** es un buen evento para usar porque en este momento la interfaz de usuario está lista para la interacción y la animación no se cortará al comienzo porque otra parte de la interfaz de usuario ya se estaba cargando.

En este ejemplo, el evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed) está adjunto al rectángulo por lo que, cuando el usuario hace clic en el rectángulo, empieza la animación.

```xaml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
```

El controlador de eventos inicia la clase [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) (la animación) mediante el uso del método [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) de **Storyboard**.

```csharp
myStoryboard.Begin();
```

```cppwinrt
myStoryboard().Begin();
```

```cpp
myStoryboard->Begin();
```

```vb
myStoryBoard.Begin()
```

Puedes controlar el evento [**Completed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.completed.aspx) si quieres que se ejecute otra lógica después de que la animación haya terminado de aplicar valores. De igual modo, el método [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/BR242358) resulta útil para solucionar problemas de interacción entre animaciones y sistema de propiedades.

> [!TIP]
> Cada vez que codifiques para un escenario de aplicaciones en el que estás iniciando una animación desde un código de aplicación, es posible que quieras revisar nuevamente si ya existe una animación o una transición en la biblioteca de animaciones para tu escenario de interfaz de usuario. Las animaciones de la biblioteca permiten una experiencia de interfaz de usuario más coherente en todas las aplicaciones de Windows Runtime y son más fáciles de usar.

 

### <a name="animations-for-visual-states"></a>Animaciones para estados visuales

El comportamiento de ejecución para un [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490) que se usa para definir un estado visual del control difiere de cómo puede ejecutar una aplicación un guión gráfico directamente. Tal como se aplica a una definición de estado visual en XAML, el **Guión gráfico** es un elemento del [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) que lo contiene y el estado en su totalidad está controlado mediante la API de [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager). Las animaciones se ejecutarán de acuerdo con sus valores de animación y las propiedades [**Línea de tiempo**](https://msdn.microsoft.com/library/windows/apps/BR210517) cuando se use el **VisualState** que lo contiene mediante un control. Para más información, consulta el tema sobre [guiones gráficos para estados visuales](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808). Para los estados visuales, el [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) aparente es diferente. Si un estado visual se cambia a otro estado, todos los cambios de propiedades aplicados por el estado visual anterior y sus animaciones se cancelan, incluso si el nuevo estado visual no aplica específicamente una nueva animación a una propiedad.

### <a name="storyboard-and-eventtrigger"></a>**Storyboard** y **EventTrigger**

Hay una forma de iniciar una animación que se pueda declarar completamente en XAML. Pero, esta técnica ya no se usa tanto. Es una sintaxis heredada de WPF y las versiones anteriores de Silverlight antes de admitir [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager). Esta sintaxis [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) aún funciona en XAML de Windows Runtime por razones de importación/compatibilidad, pero solo funciona para un comportamiento desencadenador en función del evento [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723); intentar desencadenar otros eventos arrojará excepciones o impedirá la compilación. Para obtener más información, consulta [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) o [**BeginStoryboard**](https://msdn.microsoft.com/library/windows/apps/BR243053).

## <a name="animating-xaml-attached-properties"></a>Animación de propiedades adjuntas de XAML

No es algo habitual, pero puedes aplicar un valor animado a una propiedad adjunta XAML. Si quieres obtener más información sobre propiedades adjuntas y cómo funcionan, consulta [Introducción a las propiedades adjuntas](https://msdn.microsoft.com/library/windows/apps/Mt185579). Para seleccionar una propiedad adjunta como destino hace falta una [sintaxis de property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586) que incluya el nombre de la propiedad entre paréntesis. Puedes animar las propiedades adjuntas integradas tales como [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/Hh759773) con un [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) que aplique valores enteros discretos. Sin embargo, existe una limitación en la implementación XAML de Windows Runtime y es que no puedes animar una propiedad adjunta personalizada.

## <a name="more-animation-types-and-next-steps-for-learning-about-animating-your-ui"></a>Más tipos de animaciones y los pasos que deben seguirse para obtener información sobre la animación de la interfaz de usuario

Hasta ahora, hemos mostrado las animaciones personalizadas que se animan entre dos valores, y después cómo interpolar de forma lineal los valores según sea necesario mientras se ejecuta la animación. Se denominan animaciones **From**/**To**/**By**. Hay otro tipo de animación que permite declarar valores intermedios que se encuentran entre el inicio y el final. Estas se denominan *animaciones de fotograma clave*. También hay una forma de alterar la lógica de interpolación en una animación **From**/**To**/**By** o en una animación de fotograma clave. Esto implica aplicar una función de aceleración. Para más información sobre estos conceptos, consulta [Fotograma clave y animaciones de función de aceleración](key-frame-and-easing-function-animations.md).

## <a name="related-topics"></a>Temas relacionados

* [Sintaxis de property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [Introducción a las propiedades de dependencia](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [Fotograma clave y animaciones de función de aceleración](key-frame-and-easing-function-animations.md)
* [Animaciones con guion gráfico para estados visuales](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)
* [Plantillas de control](https://msdn.microsoft.com/library/windows/apps/Mt210948)
* [**Guión gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824)
 

 




