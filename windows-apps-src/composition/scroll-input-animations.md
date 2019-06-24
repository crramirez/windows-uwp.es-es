---
title: Mejorar las experiencias existentes de ScrollViewer
description: Aprende cómo usar ScrollViewer de XAML y ExpressionAnimations para crear experiencias de movimiento dinámicas controladas por entradas.
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: 25b0732b7c29653d18f0e018698ab4b6398d402a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318073"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>Mejorar las experiencias existentes de ScrollViewer

En este artículo se explica cómo usar ScrollViewer de XAML y ExpressionAnimations para crear experiencias de movimiento dinámicas controladas por entradas.

## <a name="prerequisites"></a>Requisitos previos

En este artículo damos por hecho que estás familiarizado con los conceptos tratados en estos artículos:

- [Animaciones de entrada](input-driven-animations.md)
- [Animaciones en función de relación](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>¿Por qué crear basándose en ScrollViewer?

Normalmente se usa el control ScrollViewer de XAML existente para crear una superficie desplazable y en la que se puede hacer zoom para el contenido de la aplicación. Con la introducción del lenguaje de diseño Fluent Design, ahora también se debe centrar en cómo usar la acción de desplazar o hacer zoom en una superficie para controlar otras experiencias de movimiento. Por ejemplo, utilizar el desplazamiento para controlar una animación de desenfoque de un fondo o controlar la posición de un "encabezado permanente".

En estos escenarios, se aprovechan experiencias de comportamiento o manipulación como el desplazamiento y el zoom para hacer que otras partes de la aplicación sean más dinámicas. A su vez estas permiten que la aplicación parezca más cohesionada, por lo que las experiencias resultan más memorable a ojos de los usuarios finales. Al hacer que la interfaz de usuario de la aplicación sea más memorable, los usuarios finales interaccionarán con la aplicación con más frecuencia y durante más tiempo.

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>¿Qué puede crear basándose en ScrollViewer?

Puedes aprovechar la posición de un ScrollViewer para crear diversas experiencias dinámicas:

- Parallax: Usa la posición de un ScrollViewer para mover el contenido en primer o segundo plano a una velocidad relativa a la posición de desplazamiento.
- Encabezados permanentes: Usa la posición de un ScrollViewer para animar y hacer que un encabezado permanezca en una posición.
- Efectos controlados por entradas: Usa la posición de un ScrollViewer para animar un efecto de composición, como el desenfoque.

En general, si haces referencia a la posición de un ScrollViewer con una ExpressionAnimation, puedes crear una animación que cambie dinámicamente en relación a la cantidad de desplazamiento.

![Vista de lista con efecto parallax](images/animation/parallax.gif)

![Un encabezado discreto](images/animation/shy-header.gif)

## <a name="using-scrollmanipulationpropertyset"></a>Uso de ScrollManipulationPropertySet

Para crear estas experiencias dinámicas con un ScrollViewer de XAML, debes poder hacer referencia a la posición de desplazamiento en una animación. Esto se realizan mediante el acceso a un CompositionPropertySet fuera del ScrollViewer de XAML llamado ScrollManipulationPropertySet.
El ScrollManipulationPropertySet contiene una sola propiedad de Vector3 llamada Translation que proporciona acceso a la posición de desplazamiento del ScrollViewer. Luego puedes hacer referencia a esto en tu ExpressionAnimation igual que lo haces a cualquier otro CompositionPropertySet.

Pasos generales para empezar:

1. Accede a ScrollManipulationPropertySet mediante ElementCompositionPreview.
    - `ElementCompositionPreview.GetScrollManipulationPropertySet(ScrollViewer scroller)`
1. Crea una ExpressionAnimation que haga referencia a la propiedad Translation del PropertySet.
    - No olvides establecer el parámetro Reference.
1. Dirígete a una propiedad de CompositionObject con la ExpressionAnimation.

> [!NOTE]
> Se recomienda asignar el PropertySet devuelto desde el método GetScrollManipulationPropertySet a una variable de clase. Esto garantiza que la recolección de elementos no utilizados no limpie el conjunto de propiedades y, por consiguiente, no tenga ningún efecto en la ExpressionAnimation en la que se le hace referencia. Las ExpressionAnimations no mantienen ninguna referencia fuerte con ningún objeto usado en la ecuación.

## <a name="example"></a>Ejemplo

Echemos un vistazo a cómo se crea el ejemplo de Parallax que se muestra anteriormente. Como referencia, todo el código fuente de la aplicación se encuentra en el [repositorio de Windows UI Dev Labs en GitHub](https://github.com/microsoft/WindowsCompositionSamples).

Lo primero es obtener una referencia al ScrollManipulationPropertySet.

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

El siguiente paso es crear la ExpressionAnimation que define una ecuación que utilice la posición de desplazamiento del ScrollViewer.

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> También puedes utilizar las clases auxiliares de ExpressionBuilder para construir esta misma expresión sin necesidad de cadenas:

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

Por último, toma esta ExpressionAnimation y dirígela al objeto visual al que desees aplicar parallax. En este caso, se trata la imagen correspondiente a cada uno de los elementos de la lista.

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```