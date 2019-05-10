---
title: Animaciones de propiedad XAML
description: Animar elementos XAML con animaciones de composición.
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 183a5433553ff6fdfcb09f6960f6a642f2c8bc08
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444154"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animar elementos XAML con animaciones de composición

Este artículo presentan nuevas propiedades que le permiten animar un elemento de IU XAML con el rendimiento de las animaciones de composición y la facilidad de establecer las propiedades de XAML.

Antes de Windows 10, versión 1809, había 2 opciones para crear animaciones en sus aplicaciones para UWP:

- usar construcciones XAML como [amplía su información animaciones](storyboarded-animations.md), o el _* ThemeTransition_ y _* ThemeAnimation_ clases en el [ Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) espacio de nombres.
- usar animaciones de composición, como se describe en [con la capa Visual XAML](../../composition/using-the-visual-layer-with-xaml.md).

Uso de la capa visual proporciona un mejor rendimiento que se construye utilizando el XAML. Pero al usar [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) obtener el elemento subyacente de la composición [Visual](/uwp/api/windows.ui.composition.visual) objeto y, a continuación, animar el objeto Visual con animaciones de composición, es más difícil de usar.

A partir de Windows 10, versión 1809, puede animar las propiedades de un objeto UIElement directamente mediante animaciones de composición sin necesidad de obtener el objeto Visual de composición subyacente.

> [!NOTE]
> Para usar estas propiedades en el elemento de IU, debe ser la versión de destino del proyecto UWP 1809 o posterior. Para obtener más información acerca de cómo configurar la versión del proyecto, vea [aplicaciones adaptables versión](../../debug-test-perf/version-adaptive-apps.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/XamlCompInterop">abra la aplicación y vea la interoperabilidad de animación en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Nuevas propiedades de representación reemplazar las propiedades de representación antiguo

Esta tabla muestran las propiedades que puede usar para modificar la representación de un UIElement, que también se puede animar con un [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Property | Tipo | Descripción |
| -- | -- | -- |
| [Opacidad](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | El grado de opacidad del objeto |
| [traducción](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Variar la posición X, Y y Z del elemento |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | La matriz de transformación para aplicar al elemento |
| [Escalar](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Escala el elemento, centrado en el punto central |
| [Rotación](/uwp/api/windows.ui.xaml.uielement.rotation) | Flotante | Girar el elemento en torno a la RotationAxis y el punto central |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | El eje de giro |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | El punto central de la escala y rotación |

El valor de propiedad TransformMatrix se combina con las propiedades de escala, rotación y traslación en el orden siguiente:  TransformMatrix, escala, rotación, traducción.

Estas propiedades no afecta al diseño del elemento, por lo que modificar estas propiedades no provoca una nueva [medida](/uwp/api/windows.ui.xaml.uielement.measure)/[organizar](/uwp/api/windows.ui.xaml.uielement.arrange) pasar.

Estas propiedades tienen el mismo propósito y comportamiento que las propiedades con el mismo nombre en la composición [Visual](/uwp/api/windows.ui.composition.visual) clase (excepto para la traducción, no se encuentra en el objeto Visual).

### <a name="example-setting-the-scale-property"></a>Por ejemplo: Establecer la propiedad de escala

En este ejemplo se muestra cómo establecer la propiedad de escala en un botón.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Exclusividad mutua entre las propiedades nuevas y antiguas

> [!NOTE]
> El **opacidad** propiedad no exige la exclusividad mutua descrita en esta sección. Utilice la misma propiedad de opacidad independientemente de si usa animaciones de composición o XAML.

Las propiedades que se pueden animar con un CompositionAnimation son reemplazos para varias propiedades UIElement existentes:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Proyección](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Cuando se establece (o animar) cualquiera de las propiedades nuevo, no puede usar las propiedades de la antiguas. Por el contrario, si establece (o animar) cualquiera de las propiedades de la antiguas, no puede usar las nuevas propiedades.

Tampoco puede utilizar las nuevas propiedades si usas ElementCompositionPreview para obtener y administrar el objeto Visual mediante estos métodos:

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Al intentar combinar el uso de los dos conjuntos de propiedades, se producirá la llamada de API a los errores y producir un mensaje de error.

Es posible cambiar de un conjunto de propiedades por borrarlos, aunque por motivos de simplicidad, no se recomienda. Si la propiedad está respaldada por un DependencyProperty (por ejemplo, si UIElement.Projection está respaldado por UIElement.ProjectionProperty), a continuación, llame a ClearValue para restaurarla a su estado "no usado". En caso contrario (por ejemplo, la propiedad escala), establezca la propiedad en su valor predeterminado.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animar propiedades UIElement con CompositionAnimation

Puede animar las propiedades de representación que se muestran en la tabla con un CompositionAnimation. También se pueden hacer referencia a estas propiedades mediante un [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Use la [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) y [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) métodos en UIElement para animar las propiedades de elemento de IU.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Por ejemplo: Animar la propiedad de escala con un Vector3KeyFrameAnimation

En este ejemplo se muestra cómo animar la escala de un botón.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Por ejemplo: Animar la propiedad de escala con un ExpressionAnimation

Una página tiene dos botones. Anima el segundo botón para tener un tamaño de (a través de la escala) dos veces como el primer botón.

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

- [Animaciones amplía su información](storyboarded-animations.md)
- [Uso de la capa Visual con XAML](../../composition/using-the-visual-layer-with-xaml.md)
- [Información general sobre transformaciones](../layout/transforms.md)
