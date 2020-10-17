---
title: Mejorar las experiencias de ScrollViewer existentes
description: Obtenga información sobre cómo usar un ScrollViewer de XAML y ExpressionAnimations para crear experiencias dinámicas de movimiento controlado por entrada.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: 438f108a07349da6515443e64bd4494529b8e6a0
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152399"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>Mejorar las experiencias de ScrollViewer existentes

En este artículo se explica cómo usar un control ScrollViewer y ExpressionAnimations de XAML para crear experiencias de movimiento basadas en la entrada dinámica.

## <a name="prerequisites"></a>Requisitos previos

Aquí se supone que está familiarizado con los conceptos descritos en estos artículos:

- [Animaciones controladas por entradas](input-driven-animations.md)
- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>¿Por qué basarse en ScrollViewer?

Normalmente, se usa el ScrollViewer XAML existente para crear una superficie desplazable y ampliable para el contenido de la aplicación. Con la introducción del lenguaje de diseño de Fluent, ahora también debe centrarse en cómo usar la acción de desplazarse o acercar una superficie para impulsar otras experiencias de movimiento. Por ejemplo, si se usa el desplazamiento para impulsar una animación de desenfoque de un fondo o para impulsar la posición de un "encabezado permanente".

En estos escenarios, se aprovecha el comportamiento o las experiencias de manipulación como el desplazamiento y el zoom para hacer que otras partes de la aplicación sean más dinámicas. A su vez, esto permite que la aplicación se sienta más coherente, lo que permite que las experiencias sean más memorables en los ojos de los usuarios finales. Al hacer que la interfaz de usuario de la aplicación sea más memorable, los usuarios finales interactuarán con la aplicación con más frecuencia y durante períodos más largos.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>¿Qué se puede crear sobre ScrollViewer?

Puede aprovechar la posición de un ScrollViewer para compilar una serie de experiencias dinámicas:

- Parallax: Use la posición de un ScrollViewer para mover el contenido de fondo o de primer plano a una velocidad relativa a la posición de desplazamiento.
- StickyHeaders: Use la posición de un ScrollViewer para animar y "pegar" un encabezado en una posición.
- Input-Driven Effects: Use la posición de un ScrollViewer para animar un efecto de composición como el desenfoque.

En general, al hacer referencia a la posición de un ScrollViewer con un ExpressionAnimation, es posible crear una animación que cambie dinámicamente con respecto a la cantidad de desplazamiento.

![Vista de lista con Parallax](images/animation/parallax.gif)

![Un encabezado tímida](images/animation/shy-header.gif)

## <a name="using-scrollviewermanipulationpropertyset"></a>Usar ScrollViewerManipulationPropertySet

Para crear estas experiencias dinámicas mediante un ScrollViewer de XAML, debe poder hacer referencia a la posición de desplazamiento en una animación. Esto se hace mediante el acceso a un CompositionPropertySet fuera del ScrollViewer de XAML denominado ScrollViewerManipulationPropertySet.
ScrollViewerManipulationPropertySet contiene una única propiedad Vector3 denominada Translation que proporciona acceso a la posición de desplazamiento de ScrollViewer. Después, puede hacer referencia a este como cualquier otro CompositionPropertySet de su ExpressionAnimation.

Pasos generales para empezar:

1. Acceda a ScrollViewerManipulationPropertySet a través de ElementCompositionPreview.
    - `ElementCompositionPreview.GetScrollViewerManipulationPropertySet(ScrollViewer scroller)`
1. Cree un ExpressionAnimation que haga referencia a la propiedad Translation de la PropertySet.
    - No se olvide de establecer el parámetro de referencia.
1. Se dirige a la propiedad de un CompositionObject con ExpressionAnimation.

> [!NOTE]
> Se recomienda asignar el PropertySet devuelto desde el método GetScrollViewerManipulationPropertySet a una variable de clase. Esto garantiza que la recolección de elementos no utilizados no limpia el conjunto de propiedades y, por tanto, no tiene ningún efecto en el ExpressionAnimation en el que se hace referencia. ExpressionAnimations no mantienen una referencia segura a ninguno de los objetos utilizados en la ecuación.

## <a name="example"></a>Ejemplo

Echemos un vistazo a cómo se junta el ejemplo de Parallax mostrado anteriormente. Como referencia, todo el código fuente de la aplicación se encuentra en el [repositorio de desarrollo](https://github.com/microsoft/WindowsCompositionSamples)de la interfaz de usuario de Windows en github.

Lo primero es obtener una referencia a ScrollViewerManipulationPropertySet.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

El siguiente paso consiste en crear el ExpressionAnimation que define una ecuación que use la posición de desplazamiento de ScrollViewer.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> También puede utilizar las clases auxiliares de ExpressionBuilder para construir esta misma expresión sin necesidad de cadenas:

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

Por último, tome este ExpressionAnimation y destine el visual que desea aplicar a Parallax. En este caso, es la imagen para cada uno de los elementos de la lista.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```
