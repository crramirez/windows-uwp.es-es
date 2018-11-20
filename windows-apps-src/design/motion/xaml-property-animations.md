---
author: Jwmsft
title: Animaciones de propiedad XAML
description: Animación de elementos XAML con las animaciones de composición.
ms.author: jimwalk
ms.date: 09/13/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2a7e2c3a08fc925c57e19b9c51011854b947a52d
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7288111"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>Animación de elementos XAML con las animaciones de composición

En este artículo se presenta nuevas propiedades que te permiten animar un UIElement de XAML con el rendimiento de las animaciones de composición y la facilidad de establecer las propiedades de XAML.

Antes de Windows 10, versión 1809, tenía 2 opciones para crear animaciones en las aplicaciones para UWP:

- usar construcciones XAML como [animaciones con guion gráfico](storyboarded-animations.md), o el _* objeto ThemeTransition_ y _* objeto ThemeAnimation_ las clases en el espacio de nombres [Windows.UI.Xaml.Media.Animation](/uwp/api/windows.ui.xaml.media.animation) .
- usa animaciones de composición como se describe en el [uso de la capa Visual con XAML](../../composition/using-the-visual-layer-with-xaml.md).

El uso de la capa visual proporciona un mejor rendimiento que se construye con el XAML. Pero mediante [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) para obtener el objeto del elemento subyacente composición [Visual](/uwp/api/windows.ui.composition.visual) y, a continuación, animar el objeto Visual con las animaciones de composición, son más difícil de usar.

A partir de Windows 10, versión 1809, puedes animar las propiedades de un UIElement directamente mediante animaciones de composición sin necesidad de obtener el elemento Visual de composición subyacente.

> [!NOTE]
> Para usar estas propiedades en UIElement, debe ser la versión de destino del proyecto UWP 1809 o posterior. Para obtener más información acerca de cómo configurar la versión del proyecto, consulta [aplicaciones adaptables para versiones](../../debug-test-perf/version-adaptive-apps.md).

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>Nuevas propiedades de representación reemplazar la antigua propiedades de representación

Esta tabla muestran las propiedades que puedes usar para modificar la representación de un UIElement, que también se puede animar con un [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation).

| Propiedad | Tipo | Descripción |
| -- | -- | -- |
| [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | El grado de opacidad del objeto |
| [Translation](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | Desplazar la posición X o Y o Z del elemento |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | La matriz de transformación para aplicar al elemento |
| [Scale](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | Escalar el elemento, centrado en el punto central. |
| [Rotación](/uwp/api/windows.ui.xaml.uielement.rotation) | Flotante | Girar el elemento alrededor del RotationAxis y el punto central. |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | El eje de rotación |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | El punto central de la escala y la rotación |

El valor de propiedad TransformMatrix se combina con las propiedades de escala, la rotación y traslación en el siguiente orden: TransformMatrix, la escala, la rotación, la traducción.

Estas propiedades no afectan al diseño del elemento, por lo tanto, modificar estas propiedades no implica una nueva [medida](/uwp/api/windows.ui.xaml.uielement.measure)/pase de[organización](/uwp/api/windows.ui.xaml.uielement.arrange) .

Estas propiedades tienen el mismo propósito y comportamiento que las propiedades con el mismo nombre en la composición [Visual](/uwp/api/windows.ui.composition.visual) clase (excepto la traducción, lo que no está en Visual).

### <a name="example-setting-the-scale-property"></a>Ejemplo: Al establecer la propiedad de escala

En este ejemplo se muestra cómo establecer la propiedad de escala en un botón.

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>Exclusividad mutua entre las propiedades de la nuevas y antiguas

> [!NOTE]
> La propiedad **Opacity** no impone la exclusividad mutua descrita en esta sección. Usar la misma propiedad de opacidad si usas animaciones de composición o XAML.

Las propiedades que se pueden animar con una clase CompositionAnimation son reemplazos de varias propiedades UIElement existentes:

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [RenderTransformOrigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [Proyección](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

Cuando se establece (o animar) cualquiera de las nuevas propiedades, no puedes usar las propiedades anteriores. Por el contrario, si se establece (o animar) cualquiera de las propiedades anteriores, no puedes usar las nuevas propiedades.

También se pueden utilizar las nuevas propiedades si usas ElementCompositionPreview para obtener y administrar el objeto Visual mediante estos métodos:

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> Intentar combinar el uso de los dos conjuntos de propiedades, hará que la llamada de API para un error y generar un mensaje de error.

Es posible cambiar de un conjunto de propiedades desactivando, aunque por cuestiones de simplicidad no se recomienda. Si la propiedad está respaldada por un DependencyProperty (por ejemplo, si UIElement.Projection está respaldado por UIElement.ProjectionProperty), a continuación, llama a ClearValue para restaurarla al estado "". En caso contrario (por ejemplo, la propiedad de escala), Establece la propiedad a su valor predeterminado.

## <a name="animating-uielement-properties-with-compositionanimation"></a>Animación de propiedades UIElement con CompositionAnimation

Puedes animar las propiedades de representación que se enumeran en la tabla con una clase CompositionAnimation. También pueden hacer referencia a estas propiedades mediante una [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation).

Usa los métodos [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) y [a StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) en UIElement para animar las propiedades de UIElement.

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>Ejemplo: Animación de la propiedad de escala con una Vector3KeyFrameAnimation

En este ejemplo se muestra cómo animar la escala de un botón.

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>Ejemplo: Animación de la propiedad de escala con una ExpressionAnimation

Una página tiene dos botones. Anima el segundo botón para que sea dos veces más grande (a través de la escala) como el primer botón.

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
- [Introducción a las transformaciones](../layout/transforms.md)
