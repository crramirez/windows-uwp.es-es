---
title: Animaciones de fotograma clave y animaciones de función de aceleración
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: Las animaciones de fotograma clave lineales, las animaciones de fotograma clave con un valor KeySpline o las funciones de aceleración son tres técnicas distintas para prácticamente el mismo escenario.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e68c667fe1e6d3ef61e60095e7fed02400044d07
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172429"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>Animaciones de fotograma clave y animaciones de función de aceleración



Las animaciones de fotograma clave lineales, las animaciones de fotograma clave con un valor **KeySpline** o las funciones de aceleración son tres técnicas distintas para prácticamente el mismo escenario: crear una animación de guion gráfico que es un poco más compleja y que tiene un comportamiento de animación no lineal desde un estado inicial hasta un estado final.

## <a name="prerequisites"></a>Requisitos previos

Asegúrate de que has leído el tema sobre las [animaciones de guion gráfico](storyboarded-animations.md). Este tema se basa en los conceptos de animación que se explicaron en el tema sobre [animaciones de guion gráfico](storyboarded-animations.md) y no los explicaremos nuevamente. Por ejemplo, en la sección de [animaciones de guion gráfico](storyboarded-animations.md) se describe cómo seleccionar como destino animaciones y guiones gráficos para usarlos como recursos, los valores de la propiedad [**Timeline**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) tales como [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration), [**FillBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior), etc.

## <a name="animating-using-key-frame-animations"></a>Animación mediante animaciones de fotograma clave

Las animaciones de fotograma clave permiten que se alcance más de un valor de destino en un punto junto con la escala de tiempo de la animación. En otras palabras, cada fotograma clave puede especificar un valor intermedio diferente y el último fotograma clave alcanzado es el valor de animación final. Si especificas varios valores para animar, puedes crear animaciones más complejas. Las animaciones de fotograma clave también permiten una lógica de interpolación distinta, las cuales se implementan como una subclase **KeyFrame** diferente según el tipo de animación. Específicamente, cada tipo de animación de fotograma clave presenta una variación de **Discrete**, **Linear**, **Spline** y **Easing** de su clase **KeyFrame** para especificar sus fotogramas clave. Por ejemplo, para especificar una animación que selecciona como destino [**Double**](/dotnet/api/system.double) y usa fotogramas clave, puedes declarar fotogramas clave con [**DiscreteDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteDoubleKeyFrame), [**LinearDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.LinearDoubleKeyFrame), [**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame) y [**EasingDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.EasingDoubleKeyFrame). Puedes usar algunos de estos tipos, o todos, dentro de una única colección de **KeyFrames**, para cambiar la interpolación cada vez que se alcanza un nuevo fotograma clave.

En el caso del comportamiento de interpolación, cada fotograma clave controla la interpolación hasta que se alcanza el tiempo **KeyTime**. En ese momento también se alcanza su **Value**. Si hay más fotogramas clave detrás, el valor se convierte entonces en el valor inicial para el siguiente fotograma clave de una secuencia.

Al inicio de la animación, si no existe ningún fotograma clave con un elemento **KeyTime** con el valor "0:0:0", el valor inicial es igual al valor no animado de la propiedad. Esto es similar al modo en que la animación **de** / **a** / **by** actúa si no hay ningún **de**.

De forma implícita, la duración de una animación de fotograma clave es la duración del valor más alto de **KeyTime** establecido en cualquiera de los fotogramas clave. Si quieres, puedes configurar un valor de [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) explícito, pero recuerda que no puede ser menor que el valor **KeyTime** de tus propios fotogramas clave; de lo contrario, cortarás parte de la animación.

Además de la [**duración**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration), puede establecer todas las propiedades basadas en la [**escala de tiempo**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) en una animación de fotogramas clave, como puede hacerlo con una animación **de** / **a** / **by** , ya que las clases de animación de fotogramas clave también derivan de la **escala de tiempo**. Son las siguientes:

-   [**AutoReverse**](/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse): una vez que se alcanza el último fotograma clave, los fotogramas se repiten en orden inverso desde el final. Esto duplica la duración aparente de la animación.
-   [**BeginTime**](/uwp/api/windows.ui.xaml.media.animation.timeline.begintime): retrasa el inicio de la animación. La escala de tiempo para los valores **KeyTime** de los fotogramas no inicia el recuento hasta que se alcanza **BeginTime**, de modo que no hay riesgo de cortar los fotogramas.
-   [**FillBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior): controla lo que sucede cuando se llega al último fotograma clave. **FillBehavior** no tiene ningún efecto en los fotogramas clave intermedios.
-   [**RepeatBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty):
    -   si está establecido como **Forever**, los fotogramas clave y sus escalas de tiempo se repiten de manera indefinida.
    -   Si está establecido como un recuento de iteraciones, la escala de tiempo se repite esa cantidad de veces.
    -   Si está establecido como un valor de [**Duration**](/uwp/api/Windows.UI.Xaml.Duration), la escala de tiempo se repite hasta llegar a dicho horario. Esto puede truncar parte de la animación en la secuencia de fotograma clave, si no es un factor entero de la duración implícita de la escala de tiempo.
-   [**SpeedRatio**](/uwp/api/windows.ui.xaml.media.animation.timeline.speedratioproperty) (no se usa con frecuencia)

### <a name="linear-key-frames"></a>Fotogramas clave lineales

Los fotogramas clave lineales dan como resultado una interpolación lineal simple del valor hasta que se alcanza el valor **KeyTime** del fotograma. Este comportamiento de interpolación es el más similar al más sencillo **de**las / **To** / animaciones que se describen en el tema sobre [animaciones con guiones gráficos](storyboarded-animations.md) .**By**

Aquí te mostramos cómo usar una animación de fotograma clave para escalar el alto de un rectángulo con fotogramas clave lineales. Este ejemplo ejecuta una animación en la que el alto del rectángulo aumenta levemente, de manera lineal, durante los primeros cuatro segundos y, luego, escala rápidamente hasta el último segundo hasta que el rectángulo presenta el doble del alto inicial.

```xml
<StackPanel>
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames
              Storyboard.TargetName="myRectangle"
              Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <LinearDoubleKeyFrame Value="1" KeyTime="0:0:0"/>
                <LinearDoubleKeyFrame Value="1.2" KeyTime="0:0:4"/>
                <LinearDoubleKeyFrame Value="2" KeyTime="0:0:5"/>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
    </StackPanel.Resources>
</StackPanel>
```

### <a name="discrete-key-frames"></a>Fotogramas clave discretos

Los fotogramas clave discretos no usan ninguna interpolación. Cuando se llega a un valor **KeyTime**, simplemente se aplica el nuevo **Value**. Según la propiedad de interfaz de usuario que se está animando, por lo general, esto genera una animación que simula un "salto". Asegúrate de que este sea el comportamiento estético que realmente quieres. Puedes minimizar los saltos aparentes al aumentar la cantidad de fotogramas clave declarados, pero si tu objetivo es una animación suave, lo mejor es que uses en cambio fotogramas clave lineales o spline.

> [!NOTE]
> Los fotogramas clave discretos son la única manera de animar un valor que no es de tipo [**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point)y [**color**](/uwp/api/Windows.UI.Color), con un [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame). Analizaremos esto en mayor detalle más adelante en este tema.

### <a name="spline-key-frames"></a>Fotogramas clave spline

Un fotograma clave spline crea una transición variable entre los valores según el valor de la propiedad **KeySpline** . Esta propiedad especifica el primer y el segundo punto de control de una curva Bézier, que describen la aceleración de la animación. Básicamente, un [**KeySpline**](/uwp/api/Windows.UI.Xaml.Media.Animation.KeySpline) define una relación de función a tiempo en la que el gráfico de tiempo de función es la forma de esa curva de Bézier. Normalmente, se especifica un valor de **KeySpline** en una cadena de atributo abreviado XAML que tiene cuatro valores [**Double**](/dotnet/api/system.double) separados por espacios o comas. Estos valores son pares "X,Y" para dos puntos de control de la curva Bézier. X representa al tiempo e Y es el modificador de función para el valor. Cada valor debe estar comprendido siempre entre 0 y 1, ambos inclusive. Sin una modificación del punto de control a un valor **KeySpline**, la línea recta de 0,0 a 1,1 es la representación de una función a lo largo del tiempo para una interpolación lineal. Los puntos de control cambian la forma de la curva y, por ende, el comportamiento de la función a lo largo del tiempo para la animación spline. Lo mejor es verlo en un gráfico. Puedes ejecutar la [muestra del visualizador de spline clave de Silverlight](https://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample) en un explorador para ver de qué manera los puntos de control modifican la curva y cómo se ejecuta una animación de muestra cuando se la usa como un valor **KeySpline**.

El ejemplo que sigue muestra tres fotogramas clave diferentes aplicados a una animación. El último es una animación spline clave para un valor [**Double**](/dotnet/api/system.double) ([**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame)). Observa la cadena "0.6,0.0 0.9,0.00" que se aplica a **KeySpline**. Esta produce una curva en la que la animación parece ejecutarse lentamente al principio, pero luego alcanza con velocidad el valor justo antes de que se llegue **KeyTime**.

```xml
<Storyboard x:Name="myStoryboard">
    <!-- Animate the TranslateTransform's X property
        from 0 to 350, then 50,
        then 200 over 10 seconds. -->
    <DoubleAnimationUsingKeyFrames
        Storyboard.TargetName="MyAnimatedTranslateTransform"
        Storyboard.TargetProperty="X"
        Duration="0:0:10" EnableDependentAnimation="True">

        <!-- Using a LinearDoubleKeyFrame, the rectangle moves 
            steadily from its starting position to 500 over 
            the first 3 seconds.  -->
        <LinearDoubleKeyFrame Value="500" KeyTime="0:0:3"/>

        <!-- Using a DiscreteDoubleKeyFrame, the rectangle suddenly 
            appears at 400 after the fourth second of the animation. -->
        <DiscreteDoubleKeyFrame Value="400" KeyTime="0:0:4"/>

        <!-- Using a SplineDoubleKeyFrame, the rectangle moves 
            back to its starting point. The
            animation starts out slowly at first and then speeds up. 
            This KeyFrame ends after the 6th second. -->
        <SplineDoubleKeyFrame KeySpline="0.6,0.0 0.9,0.00" Value="0" KeyTime="0:0:6"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

### <a name="easing-key-frames"></a>Fotogramas clave de aceleración

Un fotograma clave de aceleración es un fotograma clave en el que se aplica la interpolación, y la función a lo largo del tiempo de la interpolación se controla mediante varias fórmulas matemáticas predefinidas. En realidad, puede producir prácticamente el mismo resultado con un fotograma clave spline como se puede hacer con algunos de los tipos de función de aceleración, pero también hay algunas funciones de aceleración, como la [**aceleración**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase), que no se pueden reproducir con una spline.

Para aplicar una función de aceleración a un fotograma clave de aceleración, debes establecer la propiedad **EasingFunction** como un elemento de propiedad en XAML para dicho fotograma clave. Para el valor, especifica un elemento de objeto para uno de los tipos de funciones de aceleración.

En este ejemplo se aplica [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase) y, luego, [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) como fotogramas clave sucesivos a [**DoubleAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) para crear un efecto de rebote.

```xml
<Storyboard x:Name="myStoryboard">
    <DoubleAnimationUsingKeyFrames Duration="0:0:10"
        Storyboard.TargetProperty="Height"
        Storyboard.TargetName="myEllipse">

        <!-- This keyframe animates the ellipse up to the crest 
            where it slows down and stops. -->
        <EasingDoubleKeyFrame Value="-300" KeyTime="00:00:02">
            <EasingDoubleKeyFrame.EasingFunction>
                <CubicEase/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>

        <!-- This keyframe animates the ellipse back down and makes
            it bounce. -->
        <EasingDoubleKeyFrame Value="0" KeyTime="00:00:06">
            <EasingDoubleKeyFrame.EasingFunction>
                <BounceEase Bounces="5"/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Este es solo un ejemplo de una función de aceleración. Podrás encontrar más información en la siguiente sección.

## <a name="easing-functions"></a>Funciones de aceleración

Las funciones de aceleración le permiten aplicar fórmulas matemáticas personalizadas a las animaciones. Por lo general, las operaciones matemáticas resultan útiles para producir animaciones que simulan una física del mundo real en un sistema de coordenadas en 2D. Por ejemplo, puede que quiera que un objeto rebote de forma realista o se comporte como si estuviera sobre un muelle. Puede usar animaciones de fotogramas clave o incluso **de** / **a** / **por** animaciones para aproximar estos efectos, pero tomaría una cantidad significativa de trabajo y la animación sería menos precisa que usar una fórmula matemática.

Las funciones de aceleración se pueden aplicar a las animaciones de tres formas:

-   Mediante el uso de un fotograma clave de aceleración en una animación de fotograma clave, tal como describimos en la sección anterior. Use [**EasingColorKeyFrame. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingcolorkeyframe.easingfunction), [**EasingDoubleKeyFrame. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction)o [**EasingPointKeyFrame. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingpointkeyframe.easingfunction).
-   Estableciendo la propiedad **EasingFunction** en uno de los tipos **de animación from** / **to** / **by** . Use [**ColorAnimation. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.coloranimation.easingfunction), [**DoubleAnimation. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction) o [**PointAnimation. EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.pointanimation.easingfunction).
-   Mediante la configuración de [**GeneratedEasingFunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) como parte de una [**VisualTransition**](/uwp/api/Windows.UI.Xaml.VisualTransition). Esto es específico para definir los Estados visuales de los controles; para obtener más información, vea [**GeneratedEasingFunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) o [storyboards for Visual States](/previous-versions/windows/apps/jj819808(v=win.10)).

A continuación te indicamos una lista de funciones de aceleración:

-   [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase): retira el movimiento de una animación un poco antes de que comience dicha animación en la ruta indicada.
-   [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase): crea un efecto de rebote.
-   [**CircleEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase): crea una animación que se acelera o desacelera con una función circular.
-   [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase): crea una animación que se acelera o desacelera con la fórmula f(t) = t3.
-   [**ElasticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ElasticEase): crea una animación que se asemeja a un muelle que oscila de arriba abajo hasta que se detiene.
-   [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase): crea una animación que se acelera o desacelera con una fórmula exponencial.
-   [**PowerEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase): crea una animación que se acelera o desacelera con la fórmula f(t) = tp, donde p equivale a la propiedad [**Power**](/uwp/api/windows.ui.xaml.media.animation.powerease.power).
-   [**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase): crea una animación que se acelera o desacelera con la fórmula f(t) = t2.
-   [**QuarticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuarticEase): crea una animación que se acelera o desacelera con la fórmula f(t) = t4.
-   [**QuinticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuinticEase): crea una animación que se acelera o desacelera con la fórmula f(t) = t5.
-   [**SineEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.SineEase): crea una animación que se acelera o desacelera con una fórmula senoidal.

Algunas de las funciones de aceleración tienen sus propias propiedades. Por ejemplo, [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) tiene dos propiedades [**Bounces**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounces) y [**Bounciness**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounciness) que modifican el comportamiento a lo largo del tiempo de dicha función **BounceEase**. Otras funciones de aceleración, como [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase), no tienen otras propiedades que no sean la propiedad [**EasingMode**](/uwp/api/windows.ui.xaml.media.animation.easingfunctionbase.easingmode) que comparten todas las funciones de aceleración y siempre producen el mismo comportamiento de la función a lo largo del tiempo.

Algunas de estas funciones de aceleración se superponen un poco, según cómo configures las propiedades en las funciones de aceleración que contienen propiedades. Por ejemplo, [**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase) es exactamente igual que [**PowerEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase) con un valor de [**Power**](/uwp/api/windows.ui.xaml.media.animation.powerease.power) que equivale a 2. Y [**CircleEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase) es básicamente un [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase)de valor predeterminado.

La función **de** [**aceleración de la aceleración**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase) es única, ya que puede cambiar el valor fuera del intervalo normal de acuerdo con / los valores de**a** o de fotogramas clave. Inicia la animación cambiando el valor en la dirección opuesta como cabría esperar de un comportamiento normal **de** / **a** , vuelve al valor **desde** o iniciando de nuevo y, a continuación, ejecuta la animación como normal.

En un ejemplo anterior, mostramos cómo declarar una función de aceleración para una animación de fotograma clave. En el siguiente ejemplo se aplica una función de aceleración a una animación **de** / **a** / **by** .

```xml
<StackPanel x:Name="LayoutRoot" Background="White">
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation From="30" To="200" Duration="00:00:3" 
                Storyboard.TargetName="myRectangle" 
                Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <DoubleAnimation.EasingFunction>
                    <BounceEase Bounces="2" EasingMode="EaseOut" 
                                Bounciness="2"/>
                </DoubleAnimation.EasingFunction>
            </DoubleAnimation>
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle x:Name="myRectangle" Fill="Blue" Width="200" Height="30"/>
</StackPanel>
```

Cuando se aplica una función de entradas y salidas lentas a una animación **de** / **a** / **by** , se cambian las características de función en tiempo de cómo se interpola el valor entre los valores **desde** y **hasta** durante la [**duración**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration) de la animación. Sin una función de aceleración, se trataría de una interpolación lineal.

## <a name="span-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>Animaciones de valores de objeto discretos

Hay un tipo de animación que merece especial atención porque es la única forma en la que se puede aplicar un valor animado a las propiedades que no son del tipo [**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point) o [**Color**](/uwp/api/Windows.UI.Color). Se trata de la animación de fotograma clave [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames). La animación con valores [**Object**](/dotnet/api/system.object) es diferente porque no es posible interpolar los valores entre los fotogramas. Cuando se alcanza el valor [**KeyTime**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.keytime) del fotograma, el valor animado se establece inmediatamente en el valor especificado en la propiedad **Value** en el fotograma clave. Dado que no hay interpolación, solo hay un fotograma clave que se usa en la colección de fotogramas clave de **ObjectAnimationUsingKeyFrames** : [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame).

Por lo general, la propiedad [**Value**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) de la clase [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) se establece con una sintaxis de elementos de propiedades, ya que el valor del objeto que intentas establecer a menudo no se expresa como una cadena para rellenar la propiedad **Value** en una sintaxis de atributos. Todavía puede usar la sintaxis de atributo si usa una referencia como [StaticResource](../../xaml-platform/staticresource-markup-extension.md).

Verás que se usa la clase [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) en las plantillas predeterminadas, cuando una propiedad de la plantilla hace referencia a un recurso [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Estos recursos son objetos [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush), no solo un valor [**Color**](/uwp/api/Windows.UI.Color), y usan recursos definidos como temas del sistema ([**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)). Se pueden asignar directamente a un valor del tipo **Brush** como [**TextBlock.Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground) y no necesitan usar selección indirecta. Pero, dado que **SolidColorBrush** no es [**Double**](/dotnet/api/system.double), [**Point**](/uwp/api/Windows.Foundation.Point) ni **Color**, debes usar **ObjectAnimationUsingKeyFrames** para poder usar el recurso.

```xml
<Style x:Key="TextButtonStyle" TargetType="Button">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid Background="Transparent">
                    <TextBlock x:Name="Text"
                        Text="{TemplateBinding Content}"/>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal"/>
                            <VisualState x:Name="PointerOver">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPointerOverForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="Pressed">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPressedForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
...
                       </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

También puedes usar [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) para animar propiedades que usan un valor de enumeración. Aquí tienes otro ejemplo de un estilo con nombre procedente de las plantillas predeterminadas de Windows Runtime. Observa que se establece la propiedad [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) que toma una constante de enumeración de [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility). En este caso, puedes establecer el valor con la sintaxis de atributos. Solo necesitas el nombre de constante no completo de una enumeración para establecer una propiedad con un valor de enumeración, por ejemplo, "Collapsed".

```xml
<Style x:Key="BackButtonStyle" TargetType="Button">
    <Setter Property="Template">
      <Setter.Value>
        <ControlTemplate TargetType="Button">
          <Grid x:Name="RootGrid">
            <VisualStateManager.VisualStateGroups>
              <VisualStateGroup x:Name="CommonStates">
              <VisualState x:Name="Normal"/>
...           <VisualState x:Name="Disabled">
                <Storyboard>
                  <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Visibility">
                    <DiscreteObjectKeyFrame Value="Collapsed" KeyTime="0"/>
                  </ObjectAnimationUsingKeyFrames>
                </Storyboard>
              </VisualState>
            </VisualStateGroup>
...
          </VisualStateManager.VisualStateGroups>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
  </Setter>
</Style>
```

Puedes usar más de un [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) para un conjunto de fotogramas [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames). Esta puede ser una forma interesante de crear una animación de presentación al animar el valor de [**Image.Source**](/uwp/api/windows.ui.xaml.controls.image.source), como un escenario de ejemplo para el cual varios valores de objetos pueden resultar útiles.

 ## <a name="related-topics"></a>Temas relacionados

* [Sintaxis de property-path](../../xaml-platform/property-path-syntax.md)
* [Introducción a las propiedades de dependencia](../../xaml-platform/dependency-properties-overview.md)
* [**Guión gráfico**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**Storyboard.TargetProperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)