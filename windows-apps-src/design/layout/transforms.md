---
author: Jwmsft
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: Información general sobre las transformaciones
description: Aprende a usar las transformaciones en la API de Windows Runtime&\#160;cambiando los sistemas de coordenadas relativos de los elementos de la interfaz de usuario.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4a68e91257e61856f4ac24603b9521fc2bcf30a3
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675402"
---
# <a name="transforms-overview"></a>Introducción a las transformaciones

Aprende a usar las transformaciones en la API de Windows Runtime cambiando los sistemas de coordenadas relativos de los elementos de la interfaz de usuario. Esto puede servir para ajustar la apariencia de cada uno de los elementos XAML, por ejemplo, escalar, girar o transformar la posición en el espacio x-y.

## <a name="span-idwhatisatransformspanspan-idwhatisatransformspanspan-idwhatisatransformspanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>¿Qué es una transformación?

Una *transformación* define cómo asignar, o transformar, puntos de un espacio de coordenadas a otro espacio de coordenadas. Cuando una transformación se aplica a los elementos de la interfaz de usuario, cambia el modo en que ese elemento de la interfaz de usuario se representa en la pantalla como parte de la interfaz de usuario.

Las transformaciones se podrían dividir en cuatro clasificaciones genéricas: traslación, giro, escalado y sesgo (o distorsión). Si la finalidad es usar las API de gráficos para cambiar la apariencia de los elementos de la interfaz de usuario, suele ser más fácil crear transformaciones que definir únicamente una operación cada vez. Por este motivo, Windows Runtime define una clase diferente para cada una de estas categorías de transformaciones:

-   [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027): Traslada un elemento en el espacio x-y; para ello, establece valores para [**X**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.x.aspx) e [**Y**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.translatetransform.y).
-   [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940): Escala la transformación en función de un punto central; para ello, establece valores para [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centerx.aspx), [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centery.aspx), [**ScaleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scalex.aspx) y [**ScaleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932): Gira en el espacio x-y; para ello, establece valores para [**Angle**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.angle.aspx), [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centerx.aspx) y [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.centery).
-   [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950): Aplica distorsiones o sesgados en el espacio x-y; para ello, establece valores para [**AngleX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.anglex.aspx), [**AngleY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.angley.aspx), [**CenterX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.skewtransform.centerx.aspx) y [**CenterY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.scaletransform.centeryproperty).

De estas, probablemente utilices [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) y [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) con mayor frecuencia en escenarios de interfaz de usuario.

Las transformaciones se pueden combinar y hay dos clases de Windows Runtime que admiten esto: [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105) y [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022). En una **CompositeTransform**, las transformaciones se aplican en este orden: escalar, distorsionar, girar, trasladar. Usa **TransformGroup** en lugar de **CompositeTransform** si quieres aplicar las transformaciones con un orden diferente. Para más información, consulta [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105).

## <a name="span-idtransformsandlayoutspanspan-idtransformsandlayoutspanspan-idtransformsandlayoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>Transformaciones y diseño

En un diseño XAML, las transformaciones se aplican una vez finalizado el cálculo de diseño, por lo que los cálculos del espacio disponible y otras decisiones sobre el diseño se han realizado antes de aplicar las transformaciones. Como el diseño es lo primero que se aplica, algunas veces obtendrás resultados inesperados si los elementos transformados están en una celda de [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) o en un contenedor de diseño similar que asigna espacio durante el diseño. El elemento transformado puede aparecer truncado u oscurecido porque está intentando dibujarse en un área que no calculó las dimensiones posteriores a la transformación a la hora de dividir el espacio dentro de su contenedor principal. Quizás tengas que experimentar con los resultados de las transformaciones y ajustar algunas opciones de configuración. Por ejemplo, en vez de basarte en el diseño adaptativo y empezar con el tamaño, posiblemente tengas que cambiar las propiedades **Center** o declarar medidas de píxel fijas para el espacio de diseño, a fin de procurar que el elemento principal asigne espacio suficiente.

**Nota sobre la migración:** Windows Presentation Foundation (WPF) tenía una propiedad **LayoutTransform** que aplicaba transformaciones antes del cálculo de diseño. Sin embargo, XAML en Windows Runtime no admite propiedades **LayoutTransform**. (Microsoft Silverlight también carecía de esta propiedad).

## <a name="span-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanspan-idapplyingatransformtoauielementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>Aplicar una transformación a un elemento de la interfaz de usuario

Cuando una transformación se aplica a un objeto, normalmente se hace para establecer la propiedad [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980). Establecer esta propiedad no cambia literalmente el objeto píxel a píxel. Lo que hace es aplicar la transformación dentro del espacio de coordenadas local donde existe el objeto. Después, la lógica y la operación de representación (posterior al diseño) representarán los espacios de coordenadas combinados, dando la sensación de que la apariencia del objeto ha cambiado, así como posiblemente su posición de diseño (si se aplicó [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027)).

Cada transformación de representación está centrada de forma predeterminada en el origen del sistema de coordenadas local del objeto de destino, esto es, su (0,0). La única excepción es [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), que no tiene propiedades de centrado que establecer porque el efecto de traslación es el mismo independientemente del lugar donde esté centrado. Todas las demás transformaciones tienen propiedades que establecen valores **CenterX** y **CenterY**.

Siempre que uses transformaciones con [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980), recuerda que hay otra propiedad en [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) que afecta al comportamiento de la transformación: [**RenderTransformOrigin**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransformorigin.aspx). Lo que **RenderTransformOrigin** declara es si debe aplicarse toda la transformación al punto (0,0) predeterminado de un elemento o a algún otro punto de origen dentro del espacio de coordenadas relativo de dicho elemento. En los elementos típicos, (0,0) coloca la transformación en la esquina superior izquierda. En función del efecto que desees, quizás quieras cambiar **RenderTransformOrigin** en lugar de ajustar los valores **CenterX** y **CenterY** en las transformaciones. Ten en cuenta que, si aplicas ambos valores, **RenderTransformOrigin** y **CenterX** / **CenterY**, los resultados podrían resultar confusos, especialmente si estás animando alguno de los valores.

En las pruebas de acceso, un objeto al que se ha aplicado una transformación sigue respondiendo a entradas según lo previsto y en consonancia con la apariencia visual en el espacio x-y. Por ejemplo, si has usado [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) para mover un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 400píxeles lateralmente en la interfaz de usuario, ese **Rectangle** responde a eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx) cuando el usuario presiona el punto donde **Rectangle** aparece visualmente. No se obtendrán falsos eventos si el usuario presiona el área donde estaba **Rectangle** antes de ser trasladado. En cuanto a las consideraciones sobre el índice z que afectan a las pruebas de acceso, aplicar una transformación no supone ninguna diferencia; el índice z que rige qué elemento controla los eventos de entrada de un punto en el espacio x-y se sigue evaluando mediante el orden secundario declarado en un contenedor. Ese orden suele ser el mismo orden en que se declaran los elementos en XAML, aunque en el caso de los elementos secundarios de un objeto [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267), puedes ajustar el orden aplicando la propiedad adjunta [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.zindex.aspx) a sus elementos secundarios.

## <a name="span-idothertransformpropertiesspanspan-idothertransformpropertiesspanspan-idothertransformpropertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>Otras propiedades de las transformaciones

-   [**Brush.Transform**](https://msdn.microsoft.com/library/windows/apps/BR228082), [**Brush.RelativeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228080): influyen en la manera en que un elemento [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) usa el espacio de coordenadas dentro del área a la que **Brush** se aplica para establecer propiedades visuales como primeros planos y fondos. Estas transformaciones no tienen relevancia en la mayoría de los pinceles (que suelen establecer colores sólidos con [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)), pero pueden ser de utilidad en un momento dado cuando se pintan áreas con [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/BR210101) o con [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108).
-   [**Geometry.Transform**](https://msdn.microsoft.com/library/windows/apps/BR210066): Esta propiedad se podría usar para aplicar una transformación a una geometría antes de usar dicha geometría para un valor de propiedad [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/BR243356).

## <a name="span-idanimatingatransformspanspan-idanimatingatransformspanspan-idanimatingatransformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>Animar una transformación

[**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) indica objetos que pueden animarse. Para animar un objeto **Transform**, aplica una animación de un tipo compatible a la propiedad que quieras animar. Esto suele significar que estás usando objetos [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) o [**DoubleAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) para definir la animación, porque todas las propiedades de transformación son del tipo [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Las animaciones que afectan a una transformación que se usa para un valor [**UIElement.RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) no se consideran animaciones dependientes, aunque tengan una duración distinta de cero. Para más información sobre animaciones dependientes, consulta [Animaciones con guion gráfico](/windows/uwp/design/motion/storyboarded-animations).

Si animas propiedades para lograr un efecto similar en una transformación en cuanto a la apariencia visual neta (por ejemplo, animar las propiedades [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) and [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) of a [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) en lugar de aplicar una [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), dichas animaciones siempre se tratarán como animaciones dependientes. Tendrías que habilitar las animaciones y la animación podría producir problemas de rendimiento importantes, especialmente si estás tratando de permitir las interacciones del usuario mientras ese objeto está animado. Por ese motivo, es preferible usar una transformación y animarla en lugar de animar otras propiedades que harían que la animación se trate como una animación dependiente.

Para seleccionar el destino de la transformación, debe existir una [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) como valor de [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980). Normalmente, se pone un elemento para el tipo de transformación apropiado en el XAML inicial, a veces sin establecer propiedades para dicha transformación.

Para aplicar las animaciones a las propiedades de una transformación, se suele usar una técnica de selección de destino indirecta. Para más información sobre la sintaxis de la selección de destino indirecta, consulta [Animaciones con guion gráfico](/windows/uwp/design/motion/storyboarded-animations) y [Sintaxis de la ruta de una propiedad](https://msdn.microsoft.com/library/windows/apps/Mt185586).

Algunas veces, los estilos predeterminados de los controles definen animaciones de transformaciones como parte del comportamiento de su estado visual. Por ejemplo, los estados visuales de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) usan valores de [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) animados para "hacer girar" los puntos del anillo.

Este es un ejemplo sencillo de cómo animar una transformación. En este caso, se trata de la animación del [**Angle**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.rotatetransform.angle.aspx) de una [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) para hacer girar un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sobre sí mismo alrededor de su centro visual. En este ejemplo, se asigna un nombre a **RotateTransform**, por lo que no es necesario seleccionar indirectamente el destino de la animación, pero podrías dejar la transformación sin nombre, asignar un nombre al elemento al que se aplica la transformación y usar la selección indirecta de destino como `(UIElement.RenderTransform).(RotateTransform.Angle)`.

```xml
<StackPanel Margin="15">
  <StackPanel.Resources>
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
       Storyboard.TargetName="myTransform"
       Storyboard.TargetProperty="Angle"
       From="0" To="360" Duration="0:0:5" 
       RepeatBehavior="Forever" />
    </Storyboard>
  </StackPanel.Resources>
  <Rectangle Width="50" Height="50" Fill="RoyalBlue"
   PointerPressed="StartAnimation">
    <Rectangle.RenderTransform>
      <RotateTransform x:Name="myTransform" Angle="45" CenterX="25" CenterY="25" />
    </Rectangle.RenderTransform>
  </Rectangle>
</StackPanel>
```

```xml
void StartAnimation (object sender, RoutedEventArgs e) {
    myStoryboard.Begin();
}
```

## <a name="span-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanspan-idaccountingforcoordinateframesofreferenceatruntimespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>Explicación de los marcos de coordenadas de referencia en el tiempo de ejecución

[**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) tiene un método denominado [**TransformToVisual**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.transformtovisual.aspx), que puede generar una clase [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) que se correlaciona con los marcos de coordenadas de referencia de dos elementos de la interfaz de usuario. Puedes usarlo para comparar un elemento con el marco de coordenadas de referencia predeterminado de la aplicación si pasas el objeto visual raíz como primer parámetro. Esto puede resultar útil si has capturado un evento de entrada de un elemento diferente o si estás intentando predecir el comportamiento del diseño sin solicitar realmente un pase de diseño.

Los datos de evento obtenidos a partir de eventos de puntero proporcionan acceso a un método [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/BR212141), en el que puedes especificar un parámetro *relativeTo* para cambiar el marco de coordenadas de referencia por un elemento específico en lugar del predeterminado de la aplicación. Lo que este método hace es, simplemente, aplicar internamente una transformación de traslación y transformar los datos de las coordenadas x-y automáticamente cuando crea el objeto [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/BR242038) devuelto.

## <a name="span-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspanspan-iddescribingatransformmathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>Describir una transformación matemáticamente

Una transformación se puede describir en términos de una matriz de transformación. Para describir las transformaciones en un plano x-y bidimensional se usa una matriz de 3x3. Las matrices de transformación afín se pueden multiplicar para formar cualquier número de transformaciones lineales, como rotación y distorsión (sesgado), seguidas de una traslación. La última columna de una matriz de transformación afín equivale a (0, 0, 1), por lo que solo tienes que especificar los miembros de las dos primeras columnas en la descripción matemática.

La descripción matemática de una transformación podría resultarte útil si tienes conocimientos matemáticos o estás familiarizado con las técnicas de programación de gráficos que también usan matrices para describir las transformaciones del espacio de coordenadas. Hay una clase derivada de [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006) que te permite expresar una transformación directamente en términos de su matriz de 3x3 [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137). **MatrixTransform** tiene una propiedad [**Matrix**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.matrixtransform.matrix.aspx) que contiene una estructura con seis propiedades: [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847), [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853), [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851), [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849), [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) y [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816). Cada propiedad [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) usa un valor **Double** y se corresponde con los seis valores pertinentes (columnas 1 y 2) de una matriz de transformación afín.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://msdn.microsoft.com/library/windows/apps/Hh673847)         | [**M21**](https://msdn.microsoft.com/library/windows/apps/Hh673851)         | 0   |
| [**M12**](https://msdn.microsoft.com/library/windows/apps/Hh673853)         | [**M22**](https://msdn.microsoft.com/library/windows/apps/Hh673849)         | 0   |
| [**OffsetX**](https://msdn.microsoft.com/library/windows/apps/Hh673810) | [**OffsetY**](https://msdn.microsoft.com/library/windows/apps/Hh673816) | 1   |

 

Cualquier transformación que puedas describir con un objeto [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027), [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940), [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) o [**SkewTransform**](https://msdn.microsoft.com/library/windows/apps/BR242950) podría describirse igualmente mediante una [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137) con un valor [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127). Sin embargo, lo normal es usar solamente **TranslateTransform** y las demás, porque conceptualizar las propiedades de estas clases de transformaciones es más fácil que establecer los componentes vectoriales de una **Matrix**. También es más fácil animar las diferentes propiedades de las transformaciones; una **Matrix** es en realidad una estructura y no un [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) y, por lo tanto, no admite valores individuales animados.

Algunas herramientas de diseño XAML que permiten aplicar operaciones de transformación serializarán los resultados como una [**MatrixTransform**](https://msdn.microsoft.com/library/windows/apps/BR210137). En este caso, quizás sea mejor usar de nuevo la misma herramienta de diseño para cambiar el efecto de la transformación y volver a serializar el código XAML, en lugar de intentar manipular tú mismo los valores de [**Matrix**](https://msdn.microsoft.com/library/windows/apps/BR210127) directamente en el código XAML.

## <a name="span-id3-dtransformsspanspan-id3-dtransformsspanspan-id3-dtransformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>Transformaciones 3D

En Windows10, XAML ha introducido una nueva propiedad, [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx), que puede usarse para crear efectos 3D con la interfaz de usuario. Para ello, usa [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.perspectivetransform3d.aspx) para agregar a la escena una perspectiva 3D o "cámara" compartida y, a continuación, emplea [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.media3d.compositetransform3d.aspx) para transformar un elemento en el espacio 3D, igual que si utilizaras [**CompositeTransform**](https://msdn.microsoft.com/library/windows/apps/BR228105). Consulta [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx) para obtener una explicación de cómo implementar transformaciones 3D.

 Para realizar efectos 3D más simples que se apliquen a un solo objeto, se puede usar la propiedad [**UIElement.Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection). Usar una [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/br210192) como el valor de esta propiedad equivale a aplicar al elemento una transformación de perspectiva fija y una o más transformaciones 3D. Este tipo de transformación se describe en mayor profundidad en [Efectos de perspectiva 3D para interfaces de usuario XAML](3-d-perspective-effects.md).

## <a name="span-idrelatedtopicsspanrelated-topics"></a><span id="related_topics"></span>Temas relacionados

* [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)
* [Efectos de perspectiva 3D para interfaces de usuario XAML](3-d-perspective-effects.md)
* [**Transform**](https://msdn.microsoft.com/library/windows/apps/BR243006)
 

 




