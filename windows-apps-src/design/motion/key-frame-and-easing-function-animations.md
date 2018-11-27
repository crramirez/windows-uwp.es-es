---
title: Animaciones de fotograma clave y animaciones de función de aceleración
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: Las animaciones de fotograma clave lineales, las animaciones de fotograma clave con un valor KeySpline o las funciones de aceleración son tres técnicas distintas para prácticamente el mismo escenario.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 145912f08075678c98dfb34ac491e123577c69e3
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7703984"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>Animaciones de fotograma clave y animaciones de función de aceleración



Las animaciones de fotograma clave lineales, las animaciones de fotograma clave con un valor **KeySpline** o las funciones de aceleración son tres técnicas distintas para prácticamente el mismo escenario: crear una animación de guion gráfico que es un poco más compleja y que tiene un comportamiento de animación no lineal desde un estado inicial hasta un estado final.

## <a name="prerequisites"></a>Requisitos previos

Asegúrate de que has leído el tema sobre las [animaciones de guion gráfico](storyboarded-animations.md). Este tema se basa en los conceptos de animación que se explicaron en el tema sobre [animaciones de guion gráfico](storyboarded-animations.md) y no los explicaremos nuevamente. Por ejemplo, en la sección de [animaciones de guion gráfico](storyboarded-animations.md) se describe cómo seleccionar como destino animaciones y guiones gráficos para usarlos como recursos, los valores de la propiedad [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) tales como [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration), [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior), etc.

## <a name="animating-using-key-frame-animations"></a>Animación mediante animaciones de fotograma clave

Las animaciones de fotograma clave permiten que se alcance más de un valor de destino en un punto junto con la escala de tiempo de la animación. En otras palabras, cada fotograma clave puede especificar un valor intermedio diferente y el último fotograma clave que se alcanza se convierte en el valor de animación final. Si especificas varios valores para animar, puedes crear animaciones más complejas. Las animaciones de fotograma clave también permiten una lógica de interpolación distinta, las cuales se implementan como una subclase **KeyFrame** diferente según el tipo de animación. Específicamente, cada tipo de animación de fotograma clave presenta una variación de **Discrete**, **Linear**, **Spline** y **Easing** de su clase **KeyFrame** para especificar sus fotogramas clave. Por ejemplo, para especificar una animación que selecciona como destino [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) y usa fotogramas clave, puedes declarar fotogramas clave con [**DiscreteDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243130), [**LinearDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210316), [**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446) y [**EasingDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210269). Puedes usar algunos de estos tipos, o todos, dentro de una única colección de **KeyFrames**, para cambiar la interpolación cada vez que se alcanza un nuevo fotograma clave.

En el caso del comportamiento de interpolación, cada fotograma clave controla la interpolación hasta que se alcanza el tiempo **KeyTime**. En ese momento también se alcanza su **Value**. Si hay más fotogramas clave detrás, el valor se convierte entonces en el valor inicial para el siguiente fotograma clave de una secuencia.

Al inicio de la animación, si no existe ningún fotograma clave con un elemento **KeyTime** con el valor "0:0:0", el valor inicial es igual al valor no animado de la propiedad. Esto es similar a cómo funciona una animación **From**/**To**/**By** si no hay un valor **From**.

De forma implícita, la duración de una animación de fotograma clave es la duración del valor más alto de **KeyTime** establecido en cualquiera de los fotogramas clave. Si quieres, puedes configurar un valor de [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) explícito, pero recuerda que no puede ser menor que el valor **KeyTime** de tus propios fotogramas clave; de lo contrario, cortarás parte de la animación.

Además de la propiedad [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration), puedes configurar todas las propiedades basadas en la clase [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) en una animación de fotograma clave, de la misma forma que con una animación **From**/**To**/**By**, ya que las clases de animación de fotograma clave también derivan de **Timeline**. Estas son:

-   [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.autoreverse): una vez que se alcanza el último fotograma clave, los fotogramas se repiten en orden inverso desde el final. Esto duplica la duración aparente de la animación.
-   [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.begintime): retrasa el inicio de la animación. La escala de tiempo para los valores **KeyTime** de los fotogramas no inicia el recuento hasta que se alcanza **BeginTime**, de modo que no hay riesgo de cortar los fotogramas.
-   [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior): controla lo que sucede cuando se llega al último fotograma clave. **FillBehavior** no tiene ningún efecto en los fotogramas clave intermedios.
-   [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty):
    -   si está establecido como **Forever**, los fotogramas clave y sus escalas de tiempo se repiten de manera indefinida.
    -   Si está establecido como un recuento de iteraciones, la escala de tiempo se repite esa cantidad de veces.
    -   Si está establecido como un valor de [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377), la escala de tiempo se repite hasta llegar a dicho horario. Esto puede truncar parte de la animación en la secuencia de fotograma clave, si no es un factor entero de la duración implícita de la escala de tiempo.
-   [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.speedratioproperty) (no se usa habitualmente)

### <a name="linear-key-frames"></a>Fotogramas clave lineales

Los fotogramas clave lineales dan como resultado una interpolación lineal simple del valor hasta que se alcanza el valor **KeyTime** del fotograma. El comportamiento de interpolación es lo que más se asemeja a las animaciones **From**/**To**/**By** más simples que se describen en el tema [Animaciones de guion gráfico](storyboarded-animations.md).

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
> Los fotogramas clave discretos son la única forma de animar un valor que no sea del tipo [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) y [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) con un [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132). Analizaremos esto en mayor detalle más adelante en este tema.

### <a name="spline-key-frames"></a>Fotogramas clave spline

Un fotograma clave spline crea una transición variable entre valores de acuerdo con el valor de la propiedad **KeySpline**. Esta propiedad especifica el primer y el segundo punto de control de una curva Bézier, que describen la aceleración de la animación. Básicamente, un [**KeySpline**](https://msdn.microsoft.com/library/windows/apps/BR210307) define una función sobre una relación de tiempo en la que el gráfico de tiempo de la función tiene la forma de la curva Bézier. Normalmente especificas un valor de **KeySpline** en una cadena de atributos XAML abreviada que tiene cuatro valores [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) separados por espacios o comas. Estos valores son pares "X,Y" para dos puntos de control de la curva Bézier. X representa al tiempo e Y es el modificador de función para el valor. Cada valor debe estar comprendido siempre entre 0 y 1, ambos inclusive. Sin una modificación del punto de control a un valor **KeySpline**, la línea recta de 0,0 a 1,1 es la representación de una función a lo largo del tiempo para una interpolación lineal. Los puntos de control cambian la forma de la curva y, por ende, el comportamiento de la función a lo largo del tiempo para la animación spline. Lo mejor es verlo en un gráfico. Puedes ejecutar la [muestra del visualizador de spline clave de Silverlight](http://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample) en un explorador para ver de qué manera los puntos de control modifican la curva y cómo se ejecuta una animación de muestra cuando se la usa como un valor **KeySpline**.

El ejemplo que sigue muestra tres fotogramas clave diferentes aplicados a una animación. El último es una animación spline clave para un valor [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) ([**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446)). Observa la cadena "0.6,0.0 0.9,0.00" que se aplica a **KeySpline**. Esta produce una curva en la que la animación parece ejecutarse lentamente al principio, pero luego alcanza con velocidad el valor justo antes de que se llegue **KeyTime**.

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

Un fotograma clave de aceleración es un fotograma clave en el que se aplica interpolación, y la función a lo largo del tiempo de la interpolación está controlada por varias fórmulas matemáticas predefinidas. En realidad, puedes producir el mismo resultado con un fotograma clave spline que el que puedes obtener con algunos de los tipos de funciones de aceleración, pero hay algunas funciones de aceleración, tales como [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049), que no puedes reproducir con un spline.

Para aplicar una función de aceleración a un fotograma clave de aceleración, debes establecer la propiedad **EasingFunction** como un elemento de propiedad en XAML para dicho fotograma clave. Para el valor, especifica un elemento de objeto para uno de los tipos de funciones de aceleración.

En este ejemplo se aplica [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126) y, luego, [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) como fotogramas clave sucesivos a [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) para crear un efecto de rebote.

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

Las funciones de aceleración te permiten aplicar fórmulas matemáticas personalizadas a tus animaciones. Por lo general, las operaciones matemáticas resultan útiles para producir animaciones que simulan una física del mundo real en un sistema de coordenadas en 2D. Por ejemplo, es posible que quieras que un objeto rebote de forma realista o se comporte como si estuviera colgado de un muelle. Para ello, podrías usar animaciones de fotogramas clave o incluso de **From**/**To**/**By** para mostrar estos efectos, pero representaría una carga de trabajo considerable y la animación resultaría menos realista que si usas una fórmula matemática.

Las funciones de aceleración se pueden aplicar a las animaciones de tres formas:

-   Mediante el uso de un fotograma clave de aceleración en una animación de fotograma clave, tal como describimos en la sección anterior. Usa [**EasingColorKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210267), [**EasingDoubleKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction.aspx) o [**EasingPointKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210279).
-   Mediante la configuración de la propiedad **EasingFunction** en uno de los tipos de animaciones **From**/**To**/**By**. Usa [**ColorAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR243075), [**DoubleAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) o [**PointAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210354).
-   Mediante la configuración de [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) como parte de una [**VisualTransition**](https://msdn.microsoft.com/library/windows/apps/BR209034). Esto es específico para definir estados visuales para los controles; para obtener más información, consulta [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) o el tema sobre [guiones gráficos para estados visuales](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808).

A continuación te indicamos una lista de funciones de aceleración:

-   [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049): retira el movimiento de una animación un poco antes de que comience dicha animación en la ruta indicada.
-   [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057): crea un efecto de rebote.
-   [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063): crea una animación que se acelera o desacelera con una función circular.
-   [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126): crea una animación que se acelera o desacelera con la fórmula f(t) = t3.
-   [**ElasticEase**](https://msdn.microsoft.com/library/windows/apps/BR210282): crea una animación que se asemeja a un muelle que oscila de arriba abajo hasta que se detiene.
-   [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294): crea una animación que se acelera o desacelera con una fórmula exponencial.
-   [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399): crea una animación que se acelera o desacelera con la fórmula f(t) = tp, donde p equivale a la propiedad [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power).
-   [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403): crea una animación que se acelera o desacelera con la fórmula f(t) = t2.
-   [**QuarticEase**](https://msdn.microsoft.com/library/windows/apps/BR210405): crea una animación que se acelera o desacelera con la fórmula f(t) = t4.
-   [**QuinticEase**](https://msdn.microsoft.com/library/windows/apps/BR210407): crea una animación que se acelera o desacelera con la fórmula f(t) = t5.
-   [**SineEase**](https://msdn.microsoft.com/library/windows/apps/BR210439): crea una animación que se acelera o desacelera con una fórmula senoidal.

Algunas de las funciones de aceleración tienen sus propias propiedades. Por ejemplo, [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) tiene dos propiedades [**Bounces**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounces.aspx) y [**Bounciness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounciness.aspx) que modifican el comportamiento a lo largo del tiempo de dicha función **BounceEase**. Otras funciones de aceleración, como [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126), no tienen otras propiedades que no sean la propiedad [**EasingMode**](https://msdn.microsoft.com/library/windows/apps/BR210275) que comparten todas las funciones de aceleración y siempre producen el mismo comportamiento de la función a lo largo del tiempo.

Algunas de estas funciones de aceleración se superponen un poco, según cómo configures las propiedades en las funciones de aceleración que contienen propiedades. Por ejemplo, [**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403) es exactamente igual que [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399) con un valor de [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power) que equivale a 2. Y [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063) es básicamente un valor predeterminado de [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294).

La función de aceleración [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049) es única porque puede cambiar el valor fuera del intervalo normal establecido por **From**/**To** o los valores de los fotogramas clave. Se encarga de iniciar la animación al cambiar el valor en la dirección contraria que se esperaría de un comportamiento **From**/**To** normal, y vuelve al valor inicial o al valor **From**; a continuación, ejecuta la animación normalmente.

En un ejemplo anterior, mostramos cómo declarar una función de aceleración para una animación de fotograma clave. En el ejemplo siguiente aplicaremos una función de aceleración a una animación **From**/**To**/**By**.

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

Cuando se aplica una función de aceleración a una animación **From**/**To**/**By**, se cambian las características de la función a lo largo del tiempo, con respecto a cómo el valor se interpola entre los valores **From** y **To** durante el transcurso de la propiedad [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) de la animación. Sin una función de aceleración, se trataría de una interpolación lineal.

## <a name="span-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspanspan-iddiscreteobjectvalueanimationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>Animaciones de valores de objetos discretas

Hay un tipo de animación que merece especial atención porque es la única forma en la que se puede aplicar un valor animado a las propiedades que no son del tipo [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) o [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723). Se trata de la animación de fotograma clave [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320). La animación con valores [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) es diferente porque no es posible interpolar los valores entre los fotogramas. Cuando se alcanza el valor [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR210342) del fotograma, el valor animado se establece inmediatamente en el valor especificado en la propiedad **Value** en el fotograma clave. Dado que no hay interpolación, hay un solo fotograma clave que puedes usar en la colección de fotogramas clave **ObjectAnimationUsingKeyFrames** [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132).

Por lo general, la propiedad [**Value**](https://msdn.microsoft.com/library/windows/apps/BR210344) de [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) se establece con una sintaxis de elementos de propiedades, ya que el valor del objeto que intentas establecer a menudo no se expresa como una cadena para rellenar la propiedad **Value** en una sintaxis de atributos. También puedes usar la sintaxis de atributos si usas una referencia como [StaticResource](https://msdn.microsoft.com/library/windows/apps/Mt185588).

Verás que se usa [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) en las plantillas predeterminadas cuando una propiedad de la plantilla hace referencia a un recurso [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush). Estos recursos son objetos [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962), no solo un valor [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723), y usan recursos definidos como temas del sistema ([**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807)). Se pueden asignar directamente a un valor del tipo **Brush** como [**TextBlock.Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) y no necesitan usar selección indirecta. Pero, dado que **SolidColorBrush** no es [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) ni **Color**, debes usar **ObjectAnimationUsingKeyFrames** para poder usar el recurso.

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

También puedes usar [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) para animar propiedades que usan un valor de enumeración. Aquí tienes otro ejemplo de un estilo con nombre procedente de las plantillas predeterminadas de Windows Runtime. Observa que se establece la propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) que toma una constante de enumeración de [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR209006). En este caso, puedes establecer el valor con la sintaxis de atributos. Solo necesitas el nombre de constante no completo de una enumeración para establecer una propiedad con un valor de enumeración, por ejemplo, "Collapsed".

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

Puedes usar más de un [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) para un conjunto de fotogramas [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320). Esta puede ser una forma interesante de crear una animación de presentación al animar el valor de [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760), como un escenario de ejemplo para el cual varios valores de objetos pueden resultar útiles.

 ## <a name="related-topics"></a>Temas relacionados

* [Sintaxis de property-path](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [Introducción a las propiedades de dependencia](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [**Guion gráfico**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)