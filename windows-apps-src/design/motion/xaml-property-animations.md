---
title: Animaciones de propiedades XAML
description: Obtenga información sobre cómo animar propiedades en un UIElement de XAML directamente mediante animaciones de composición de Plataforma universal de Windows (UWP).
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a7ab119df407b45fb391aebf50df5d85f89ec0f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238300"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animar elementos XAML con animaciones de composición

En este artículo se presentan nuevas propiedades que permiten animar un UIElement de XAML con el rendimiento de las animaciones de composición y la facilidad de configuración de las propiedades XAML.

Antes de la versión 1809 de Windows 10, tenía dos opciones para compilar animaciones en las aplicaciones para UWP:

- Use construcciones XAML como [animaciones con guiones gráficos](storyboarded-animations.md)o las clases _* ThemeTransition_ y _* ThemeAnimation_ en el espacio de nombres [Windows. UI. Xaml. Media. Animation](/uwp/api/windows.ui.xaml.media.animation) .
- Use animaciones de composición tal y como se describe en [uso de la capa visual con XAML](../../composition/using-the-visual-layer-with-xaml.md).

El uso del nivel visual proporciona un mejor rendimiento que el uso de las construcciones XAML. Pero usar [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) para obtener el objeto [Visual](/uwp/api/windows.ui.composition.visual) de composición subyacente del elemento y, a continuación, animar el objeto visual con animaciones de composición es más complejo de usar.

A partir de Windows 10, versión 1809, puede animar las propiedades de un UIElement directamente usando animaciones de composición sin necesidad de obtener el visual de composición subyacente.

> [!NOTE]
> Para usar estas propiedades en UIElement, la versión de destino del proyecto de UWP debe ser 1809 o posterior. Para obtener más información sobre la configuración de la versión del proyecto, consulte [versiones adaptables](../../debug-test-perf/version-adaptive-apps.md)de las aplicaciones.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene instalada la aplicación de <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> , haga clic aquí para <a href="xamlcontrolsgallery:/item/XamlCompInterop">abrir la aplicación y consulte interoperabilidad de animaciones en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Las nuevas propiedades de representación reemplazan las propiedades de representación anteriores

En esta tabla se muestran las propiedades que puede usar para modificar la representación de un UIElement, que también se puede animar con un [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Propiedad | Tipo | Descripción |
| -- | -- | -- |
| [Opacidad](/uwp/api/windows.ui.xaml.uielement.opacity) | Doble | El grado de opacidad del objeto |
| [Traducción](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Desplazar la posición X/Y/Z del elemento |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | Matriz de transformación que se va a aplicar al elemento. |
| [Escala](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Escalar el elemento, centrado en el CenterPoint |
| [Rotación](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | Girar el elemento alrededor de RotationAxis y CenterPoint |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | Eje de giro |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | El punto central de escala y giro |

El valor de la propiedad TransformMatrix se combina con las propiedades Scale, Rotation y Translation en el siguiente orden: TransformMatrix, Scale, rotation, Translation.

Estas propiedades no afectan al diseño del elemento, por lo que modificar estas propiedades no provoca un nuevo paso de organización de la [medida](/uwp/api/windows.ui.xaml.uielement.measure) / [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) .

Estas propiedades tienen el mismo propósito y comportamiento que las propiedades con el mismo nombre en la clase [Visual](/uwp/api/windows.ui.composition.visual) de composición (excepto para la traducción, que no se encuentra en visual).

### <a name="example-setting-the-scale-property"></a>Ejemplo: establecer la propiedad Scale

Este ejemplo muestra cómo establecer la propiedad Scale en un botón.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Exclusividad mutua entre propiedades nuevas y anteriores

> [!NOTE]
> La propiedad **Opacity** no exige la exclusividad mutua descrita en esta sección. Use la misma propiedad de opacidad si utiliza animaciones XAML o de composición.

Las propiedades que se pueden animar con un CompositionAnimation son reemplazos para varias propiedades de UIElement existentes:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Proyección](/uwp/api/windows.ui.xaml.uielement.projection)
- [Objeto Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Al establecer (o animar) cualquiera de las propiedades nuevas, no se pueden usar las propiedades anteriores. Por el contrario, si establece (o anima) cualquiera de las propiedades anteriores, no puede usar las nuevas propiedades.

Tampoco puede usar las nuevas propiedades si usa ElementCompositionPreview para obtener y administrar el visual mediante estos métodos:

- [ElementCompositionPreview. GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview. SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Si se intenta mezclar el uso de los dos conjuntos de propiedades, se producirá un error en la llamada a la API y se generará un mensaje de error.

Es posible cambiar de un conjunto de propiedades si se borran, aunque por simplicidad no se recomienda. Si la propiedad está respaldada por un DependencyProperty (por ejemplo, UIElement. Projection está respaldado por UIElement. ProjectionProperty), llame a ClearValue para restaurarlo a su estado "sin usar". En caso contrario (por ejemplo, la propiedad Scale), establezca la propiedad en su valor predeterminado.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animar propiedades de UIElement con CompositionAnimation

Puede animar las propiedades de representación enumeradas en la tabla con un CompositionAnimation. También se puede hacer referencia a estas propiedades mediante un [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Use los métodos [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) y [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) en UIElement para animar las propiedades de UIElement.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Ejemplo: animación de la propiedad Scale con un Vector3KeyFrameAnimation

En este ejemplo se muestra cómo animar la escala de un botón.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Ejemplo: animación de la propiedad Scale con un ExpressionAnimation

Una página tiene dos botones. El segundo botón anima para que sea dos veces más grande (a través de la escala) que el primer botón.

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>Temas relacionados

- [Animaciones con guion gráfico](storyboarded-animations.md)
- [Uso de la capa visual con XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Información general sobre transformaciones](../layout/transforms.md)
