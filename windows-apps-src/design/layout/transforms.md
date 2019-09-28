---
ms.assetid: F46D5E18-10A3-4F7B-AD67-76437C77E4BC
title: Información general sobre las transformaciones
description: Aprenda a usar transformaciones en el & de Windows Runtime \#160; API, cambiando los sistemas de coordenadas relativas de los elementos de la interfaz de usuario.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aa60db28003c4f231cf36b653c5e69b422978c1a
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340067"
---
# <a name="transforms-overview"></a>Información general sobre las transformaciones

Aprende a usar las transformaciones en la API de Windows Runtime cambiando los sistemas de coordenadas relativos de los elementos de la interfaz de usuario. Esto puede servir para ajustar la apariencia de cada uno de los elementos XAML, por ejemplo, escalar, girar o transformar la posición en el espacio x-y.

## <a name="span-idwhat_is_a_transform_spanspan-idwhat_is_a_transform_spanspan-idwhat_is_a_transform_spanwhat-is-a-transform"></a><span id="What_is_a_transform_"></span><span id="what_is_a_transform_"></span><span id="WHAT_IS_A_TRANSFORM_"></span>¿Qué es una transformación?

Una *transformación* define cómo asignar, o transformar, puntos de un espacio de coordenadas a otro espacio de coordenadas. Cuando una transformación se aplica a los elementos de la interfaz de usuario, cambia el modo en que ese elemento de la interfaz de usuario se representa en la pantalla como parte de la interfaz de usuario.

Las transformaciones se podrían dividir en cuatro clasificaciones genéricas: traslación, giro, escalado y sesgo (o distorsión). Si la finalidad es usar las API de gráficos para cambiar la apariencia de los elementos de la interfaz de usuario, suele ser más fácil crear transformaciones que definir únicamente una operación cada vez. Por este motivo, Windows Runtime define una clase diferente para cada una de estas categorías de transformaciones:

-   [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform): traduce un elemento en el espacio de x [ **-y estableciendo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.y)valores para [**x**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x) e y.
-   [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform): escala la transformación en función de un punto central, estableciendo los valores de [**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centerx), [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centery), [**scaleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) y [**scaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaleyproperty).
-   [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform): gira en el espacio x-y estableciendo valores para [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle), [**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centerx) y [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.centery).
-   [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform): sesga o distorsiona en el espacio x-y estableciendo valores para [**AngleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.anglex), [**AngleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.angley), [**CenterX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.skewtransform.centerx) y [**CenterY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.centeryproperty).

De todas ellas, probablemente uses [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) y [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform) con mayor frecuencia en escenarios de interfaz de usuario.

Puede combinar transformaciones y hay dos clases Windows Runtime que admiten esto: [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform) y [**TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup). En una **CompositeTransform**, las transformaciones se aplican en este orden: escalar, distorsionar, girar, trasladar. Usa **TransformGroup** en lugar de **CompositeTransform** si quieres aplicar las transformaciones con un orden diferente. Para más información, consulta [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform).

## <a name="span-idtransforms_and_layoutspanspan-idtransforms_and_layoutspanspan-idtransforms_and_layoutspantransforms-and-layout"></a><span id="Transforms_and_layout"></span><span id="transforms_and_layout"></span><span id="TRANSFORMS_AND_LAYOUT"></span>Transformaciones y diseño

En un diseño XAML, las transformaciones se aplican una vez finalizado el cálculo de diseño, por lo que los cálculos del espacio disponible y otras decisiones sobre el diseño ya se han realizado antes de aplicar las transformaciones. Como el diseño es lo primero que se aplica, algunas veces obtendrás resultados inesperados si los elementos transformados están en una celda de [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) o en un contenedor de diseño similar que asigna espacio durante el diseño. El elemento transformado puede aparecer truncado u oscurecido porque está intentando dibujarse en un área que no calculó las dimensiones posteriores a la transformación a la hora de dividir el espacio dentro de su contenedor principal. Quizás tengas que experimentar con los resultados de las transformaciones y ajustar algunas opciones de configuración. Por ejemplo, en vez de basarte en el diseño adaptativo y empezar con el tamaño, posiblemente tengas que cambiar las propiedades **Center** o declarar medidas de píxel fijas para el espacio de diseño, a fin de procurar que el elemento principal asigne espacio suficiente.

**Nota de migración:**  Windows Presentation Foundation (WPF) tenía una propiedad **LayoutTransform** que aplicó transformaciones antes de la fase de diseño. Sin embargo, XAML en Windows Runtime no admite propiedades **LayoutTransform**. (Microsoft Silverlight también carecía de esta propiedad).

## <a name="span-idapplying_a_transform_to_a_ui_elementspanspan-idapplying_a_transform_to_a_ui_elementspanspan-idapplying_a_transform_to_a_ui_elementspanapplying-a-transform-to-a-ui-element"></a><span id="Applying_a_transform_to_a_UI_element"></span><span id="applying_a_transform_to_a_ui_element"></span><span id="APPLYING_A_TRANSFORM_TO_A_UI_ELEMENT"></span>Aplicar una transformación a un elemento de la interfaz de usuario

Cuando una transformación se aplica a un objeto, normalmente se hace para establecer la propiedad [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform). Establecer esta propiedad no cambia literalmente el objeto píxel a píxel. Lo que hace es aplicar la transformación dentro del espacio de coordenadas local donde existe el objeto. Después, la lógica y la operación de representación (posterior al diseño) representarán los espacios de coordenadas combinados, dando la sensación de que la apariencia del objeto ha cambiado, así como posiblemente su posición de diseño (si se aplicó [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)).

Cada transformación de representación está centrada de forma predeterminada en el origen del sistema de coordenadas local del objeto de destino, esto es, su (0,0). La única excepción es [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), que no tiene propiedades de centrado que establecer porque el efecto de traslación es el mismo independientemente del lugar donde esté centrado. Todas las demás transformaciones tienen propiedades que establecen valores **CenterX** y **CenterY**.

Siempre que use transformaciones con [**UIElement. RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform), recuerde que hay otra propiedad en [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) que afecta a la forma en que se comporta la transformación: [**RenderTransformOrigin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransformorigin). Lo que **RenderTransformOrigin** declara es si debe aplicarse toda la transformación al punto (0,0) predeterminado de un elemento o a algún otro punto de origen dentro del espacio de coordenadas relativo de dicho elemento. En los elementos típicos, (0,0) coloca la transformación en la esquina superior izquierda. En función del efecto que desees, quizás quieras cambiar **RenderTransformOrigin** en lugar de ajustar los valores **CenterX** y **CenterY** en las transformaciones. Ten en cuenta que, si aplicas ambos valores, **RenderTransformOrigin** y **CenterX** / **CenterY**, los resultados podrían resultar confusos, especialmente si estás animando alguno de los valores.

En las pruebas de acceso, un objeto al que se ha aplicado una transformación sigue respondiendo a entradas según lo previsto y en consonancia con la apariencia visual en el espacio x-y. Por ejemplo, si has usado [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) para mover un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 400 píxeles lateralmente en la interfaz de usuario, ese **Rectangle** responde a eventos [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed) cuando el usuario presiona el punto donde **Rectangle** aparece visualmente. No se obtendrán falsos eventos si el usuario presiona el área donde estaba **Rectangle** antes de ser trasladado. En cuanto a las consideraciones sobre el índice z que afectan a las pruebas de acceso, aplicar una transformación no supone ninguna diferencia; el índice z que rige qué elemento controla los eventos de entrada de un punto en el espacio x-y se sigue evaluando mediante el orden secundario declarado en un contenedor. Ese orden suele ser el mismo orden en que se declaran los elementos en XAML, aunque en el caso de los elementos secundarios de un objeto [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas), puedes ajustar el orden aplicando la propiedad adjunta [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) a sus elementos secundarios.

## <a name="span-idother_transform_propertiesspanspan-idother_transform_propertiesspanspan-idother_transform_propertiesspanother-transform-properties"></a><span id="Other_transform_properties"></span><span id="other_transform_properties"></span><span id="OTHER_TRANSFORM_PROPERTIES"></span>Otras propiedades de transformación

-   [**Brush. Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.transform), [**Brush. RelativeTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.brush.relativetransform): Estos influyen en la forma en que un [**pincel**](/uwp/api/Windows.UI.Xaml.Media.Brush) usa el espacio de coordenadas dentro del área en la que se aplica el **pincel** para establecer las propiedades visuales, como los objetos de primer plano y el fondo. Estas transformaciones no tienen relevancia en la mayoría de los pinceles (que suelen establecer colores sólidos con [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)), pero pueden ser de utilidad en un momento dado cuando se pintan áreas con [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) o con [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush).
-   [**Geometry. Transform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.geometry.transform): Puede usar esta propiedad para aplicar una transformación a un objeto Geometry antes de usar esa geometría para un valor de propiedad [**path. Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) .

## <a name="span-idanimating_a_transformspanspan-idanimating_a_transformspanspan-idanimating_a_transformspananimating-a-transform"></a><span id="Animating_a_transform"></span><span id="animating_a_transform"></span><span id="ANIMATING_A_TRANSFORM"></span>Animar una transformación

Los objetos de [**transformación**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) se pueden animar. Para animar un objeto **Transform**, aplica una animación de un tipo compatible a la propiedad que quieras animar. Esto suele significar que estás usando objetos [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) o [**DoubleAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimationusingkeyframes) para definir la animación, porque todas las propiedades de transformación son del tipo [**Double**](https://docs.microsoft.com/dotnet/api/system.double). Las animaciones que afectan a una transformación que se usa para un valor [**UIElement.RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) no se consideran animaciones dependientes, aunque tengan una duración distinta de cero. Para más información sobre animaciones dependientes, consulta [Animaciones con guion gráfico](/windows/uwp/design/motion/storyboarded-animations).

Si animas propiedades para lograr un efecto similar en una transformación en cuanto a la apariencia visual neta como, por ejemplo, animando las propiedades [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) y [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) de una clase [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) en lugar de aplicar una clase [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), dichas animaciones siempre se tratarán como animaciones dependientes. Tendrías que habilitar las animaciones y la animación podría producir problemas de rendimiento importantes, especialmente si estás tratando de permitir las interacciones del usuario mientras ese objeto está animado. Por ese motivo, es preferible usar una transformación y animarla en lugar de animar otras propiedades que harían que la animación se trate como una animación dependiente.

Para seleccionar el destino de la transformación, debe existir una [**Transform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) como valor de [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform). Normalmente, se pone un elemento para el tipo de transformación apropiado en el XAML inicial, a veces sin establecer propiedades para dicha transformación.

Para aplicar las animaciones a las propiedades de una transformación, se suele usar una técnica de selección de destino indirecta. Para más información sobre la sintaxis de la selección de destino indirecta, consulta [Animaciones con guion gráfico](/windows/uwp/design/motion/storyboarded-animations) y [Sintaxis de la ruta de una propiedad](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax).

En ocasiones, los estilos predeterminados de los controles definen animaciones de transformaciones como parte del comportamiento de su estado visual. Por ejemplo, los estados visuales de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) usan valores de [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) animados para "hacer girar" los puntos del anillo.

Este es un ejemplo sencillo de cómo animar una transformación. En este caso, se trata de la animación del [**Angle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rotatetransform.angle) de una [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) para hacer girar un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) sobre sí mismo alrededor de su centro visual. En este ejemplo, se asigna un nombre a **RotateTransform**, por lo que no es necesario seleccionar indirectamente el destino de la animación, pero podrías dejar la transformación sin nombre, asignar un nombre al elemento al que se aplica la transformación y usar la selección indirecta de destino como `(UIElement.RenderTransform).(RotateTransform.Angle)`.

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

## <a name="span-idaccounting_for_coordinate_frames_of_reference_at_run_timespanspan-idaccounting_for_coordinate_frames_of_reference_at_run_timespanspan-idaccounting_for_coordinate_frames_of_reference_at_run_timespanaccounting-for-coordinate-frames-of-reference-at-run-time"></a><span id="Accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="accounting_for_coordinate_frames_of_reference_at_run_time"></span><span id="ACCOUNTING_FOR_COORDINATE_FRAMES_OF_REFERENCE_AT_RUN_TIME"></span>Contabilización de los marcos de coordenadas de referencia en tiempo de ejecución

[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) tiene un método denominado [**TransformToVisual**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transformtovisual), que puede generar una [**transformación**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform) que correlaciona los marcos de coordenadas de referencia para dos elementos de la interfaz de usuario. Puedes usarlo para comparar un elemento con el marco de coordenadas de referencia predeterminado de la aplicación si pasas el objeto visual raíz como primer parámetro. Esto puede resultar útil si has capturado un evento de entrada de un elemento diferente o si estás intentando predecir el comportamiento del diseño sin solicitar realmente un pase de diseño.

Los datos de evento obtenidos a partir de eventos de puntero proporcionan acceso a un método [**GetCurrentPoint**](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpoint.getcurrentpoint), en el que puedes especificar un parámetro *relativeTo* para cambiar el marco de coordenadas de referencia por un elemento específico en lugar del predeterminado de la aplicación. Lo que este método hace es, simplemente, aplicar internamente una transformación de traslación y transformar los datos de las coordenadas x-y automáticamente cuando crea el objeto [**PointerPoint**](https://docs.microsoft.com/uwp/api/Windows.UI.Input.PointerPoint) devuelto.

## <a name="span-iddescribing_a_transform_mathematicallyspanspan-iddescribing_a_transform_mathematicallyspanspan-iddescribing_a_transform_mathematicallyspandescribing-a-transform-mathematically"></a><span id="Describing_a_transform_mathematically"></span><span id="describing_a_transform_mathematically"></span><span id="DESCRIBING_A_TRANSFORM_MATHEMATICALLY"></span>Describir una transformación matemáticamente

Una transformación se puede describir en términos de una matriz de transformación. Para describir las transformaciones en un plano x-y bidimensional se usa una matriz de 3x3. Las matrices de transformación afín se pueden multiplicar para formar cualquier número de transformaciones lineales, como rotación y distorsión (sesgado), seguidas de una traslación. La última columna de una matriz de transformación afín equivale a (0, 0, 1), por lo que solo tienes que especificar los miembros de las dos primeras columnas en la descripción matemática.

La descripción matemática de una transformación podría resultarte útil si tienes conocimientos matemáticos o estás familiarizado con las técnicas de programación de gráficos que también usan matrices para describir las transformaciones del espacio de coordenadas. Hay una clase derivada de [**transformación**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)que permite expresar una transformación directamente en términos de su matriz 3 × 3: [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform). **MatrixTransform** tiene una propiedad de [**matriz**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrixtransform.matrix) , que contiene una estructura que tiene seis propiedades: [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11), [**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12), [**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21), [**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22), [**offsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) y [**offsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety). Cada propiedad [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) usa un valor **Double** y se corresponde con los seis valores pertinentes (columnas 1 y 2) de una matriz de transformación afín.

|                                             |                                             |     |
|---------------------------------------------|---------------------------------------------|-----|
| [**M11**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m11)         | [**M21**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m21)         | 0   |
| [**M12**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m12)         | [**M22**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.m22)         | 0   |
| [**OffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsetx) | [**Desplazamiento**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.matrix.offsety) | 1   |

 

Cualquier transformación que puedas describir con un objeto [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform), [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform), [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) o [**SkewTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SkewTransform) podría describirse igualmente mediante una [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform) con un valor [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix). Sin embargo, lo normal es usar solamente **TranslateTransform** y las demás, porque conceptualizar las propiedades de estas clases de transformaciones es más fácil que establecer los componentes vectoriales de una **Matrix**. También es más fácil animar las diferentes propiedades de las transformaciones; una **Matrix** es en realidad una estructura y no un [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) y, por lo tanto, no admite valores individuales animados.

Algunas herramientas de diseño XAML que permiten aplicar operaciones de transformación serializarán los resultados como una [**MatrixTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.MatrixTransform). En este caso, quizás sea mejor usar de nuevo la misma herramienta de diseño para cambiar el efecto de la transformación y volver a serializar el código XAML, en lugar de intentar manipular tú mismo los valores de [**Matrix**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix) directamente en el código XAML.

## <a name="span-id3-d_transformsspanspan-id3-d_transformsspanspan-id3-d_transformsspan3-d-transforms"></a><span id="3-D_transforms"></span><span id="3-d_transforms"></span><span id="3-D_TRANSFORMS"></span>transformaciones 3D

En Windows 10, XAML ha introducido una nueva propiedad, [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d), que puede usarse para crear efectos 3D con la interfaz de usuario. Para ello, usa [**PerspectiveTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.perspectivetransform3d) para agregar a la escena una perspectiva 3D o "cámara" compartida y, a continuación, emplea [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.media3d.compositetransform3d) para transformar un elemento en el espacio 3D, igual que si utilizaras [**CompositeTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositeTransform). Consulta [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d) para obtener una explicación de cómo implementar transformaciones 3D.

 Para realizar efectos 3D más simples que se apliquen a un solo objeto, se puede usar la propiedad [**UIElement.Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection). Usar una [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) como el valor de esta propiedad equivale a aplicar al elemento una transformación de perspectiva fija y una o más transformaciones 3D. Este tipo de transformación se describe en mayor profundidad en [Efectos de perspectiva 3D para interfaces de usuario XAML](3-d-perspective-effects.md).

## <a name="span-idrelated_topicsspanrelated-topics"></a><span id="related_topics"></span>Temas relacionados

* [**UIElement. Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
* [efectos de perspectiva 3D para la interfaz de usuario XAML](3-d-perspective-effects.md)
* [**Transformación**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Transform)
 

 





