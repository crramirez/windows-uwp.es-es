---
title: Incorporación de cambios a la actualización con modificadores de origen
description: Obtenga información sobre cómo usar la característica SourceModifier de InteractionTracker para crear un control de extracción a actualización personalizado.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: b20b4b22d1de2252864287b97bedc4a1fc176602
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053965"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>Incorporación de cambios a la actualización con modificadores de origen

En este artículo, profundizaremos en el uso de la característica SourceModifier de InteractionTracker y demostrar su uso mediante la creación de un control personalizado de incorporación de cambios a la actualización.

## <a name="prerequisites"></a>Prerrequisitos

Aquí se supone que está familiarizado con los conceptos descritos en estos artículos:

- [Animaciones controladas por entradas](input-driven-animations.md)
- [Experiencias de manipulación personalizada con InteractionTracker](interaction-tracker-manipulations.md)
- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>¿Qué es un SourceModifier y por qué son útiles?

Al igual que [InertiaModifiers](inertia-modifiers.md), SourceModifiers proporciona un control granular más preciso sobre el movimiento de una InteractionTracker. Pero, a diferencia de InertiaModifiers, que define el movimiento después de que InteractionTracker entre en la inercia, SourceModifiers define el movimiento mientras InteractionTracker todavía está en su estado de interactuación. En estos casos, desea una experiencia diferente a la del "palo" tradicional del dedo.

Un ejemplo clásico es la experiencia de extracción a actualización: cuando el usuario extrae la lista para actualizar el contenido y la lista se mueve a la misma velocidad que el dedo y se detiene después de cierta distancia, el movimiento se sentiría brusco y mecánico. Una experiencia más natural sería introducir una sensación de resistencia mientras el usuario interactúa activamente con la lista. Este pequeño matiz ayuda a hacer que la experiencia global del usuario final interactúe con una lista más dinámica y atractiva. En la sección ejemplo, veremos con más detalle cómo crear esto.

Hay dos tipos de modificadores de origen:

- DeltaPosition: es la diferencia entre la posición del marco actual y la posición del marco anterior del dedo durante la interacción de la panorámica táctil. Este modificador de origen permite modificar la posición Delta de la interacción antes de enviarla para su posterior procesamiento. Se trata de un parámetro de tipo Vector3 y el desarrollador puede elegir modificar cualquiera de los atributos X o Y o Z de la posición antes de pasarlo a InteractionTracker.
- DeltaScale: es la diferencia entre la escala de fotogramas actual y la escala de fotograma anterior que se aplicó durante la interacción de zoom táctil. Este modificador de origen le permite modificar el nivel de zoom de la interacción. Se trata de un atributo de tipo float que el desarrollador puede modificar antes de pasarlo a InteractionTracker.

Cuando InteractionTracker está en su estado de interactuación, evalúa cada uno de los modificadores de origen asignados a él y determina si se aplica cualquiera de ellos. Esto significa que puede crear y asignar varios modificadores de origen a un InteractionTracker. Sin embargo, al definir cada uno, debe hacer lo siguiente:

1. Defina la condición: una expresión que define la instrucción condicional cuando se debe aplicar este modificador de origen específico.
1. Defina DeltaPosition/DeltaScale: expresión del modificador de origen que modifica DeltaPosition o DeltaScale cuando se cumple la condición definida anteriormente.

## <a name="example"></a>Ejemplo

Ahora veamos cómo se pueden usar los modificadores de origen para crear una experiencia de extracción a actualización personalizada con un control ListView de XAML existente. Usaremos un lienzo como el "panel de actualización" que se apilará sobre un ListView XAML para compilar esta experiencia.

En el caso de la experiencia del usuario final, queremos crear el efecto de "resistencia", ya que el usuario está colocando de forma activa la lista (con funcionalidad táctil) y ha dejado el movimiento panorámico una vez que la posición va más allá de un punto determinado.

![Lista con extracción para actualizar](images/animation/city-list.gif)

El código de trabajo para esta experiencia puede encontrarse en el repositorio de desarrollo de la [interfaz de usuario de Windows en github](https://github.com/microsoft/WindowsCompositionSamples). Este es el tutorial paso a paso para crear esa experiencia.
En el código de marcado XAML, tiene lo siguiente:

```xaml
<StackPanel Height="500" MaxHeight="500" x:Name="ContentPanel" HorizontalAlignment="Left" VerticalAlignment="Top" >
 <Canvas Width="400" Height="100" x:Name="RefreshPanel" >
<Image x:Name="FirstGear" Source="ms-appx:///Assets/Loading.png" Width="20" Height="20" Canvas.Left="200" Canvas.Top="70"/>
 </Canvas>
 <ListView x:Name="ThumbnailList"
 MaxWidth="400"
 Height="500"
ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.IsScrollInertiaEnabled="False" ScrollViewer.IsVerticalScrollChainingEnabled="True" >
 <ListView.ItemTemplate>
 ……
 </ListView.ItemTemplate>
 </ListView>
</StackPanel>
```

Como ListView ( `ThumbnailList` ) es un control XAML que ya se desplaza, necesita que el desplazamiento se encadene hasta su elemento primario ( `ContentPanel` ) cuando alcanza el elemento de nivel superior y ya no se puede desplazar. (ContentPanel es donde se aplicarán los modificadores de origen). Para que esto suceda, debe establecer ScrollViewer. IsVerticalScrollChainingEnabled en **true** en el marcado de ListView. También tendrá que establecer el modo de encadenamiento en el VisualInteractionSource en **Always**.

Debe establecer el controlador PointerPressedEvent con el parámetro _handledEventsToo_ como **true**. Sin esta opción, PointerPressedEvent no se encadenará a ContentPanel, ya que el control ListView marcará esos eventos como controlados y no se enviarán a la cadena visual.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

Ahora ya está listo para asociarlo con InteractionTracker. Empiece por configurar InteractionTracker, VisualInteractionSource y la expresión que aprovechará la posición de InteractionTracker.

```csharp
// InteractionTracker and VisualInteractionSource setup.
_root = ElementCompositionPreview.GetElementVisual(Root);
_compositor = _root.Compositor;
_tracker = InteractionTracker.Create(_compositor);
_interactionSource = VisualInteractionSource.Create(_root);
_interactionSource.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;
_interactionSource.PositionYChainingMode = InteractionChainingMode.Always;
_tracker.InteractionSources.Add(_interactionSource);
float refreshPanelHeight = (float)RefreshPanel.ActualHeight;
_tracker.MaxPosition = new Vector3((float)Root.ActualWidth, 0, 0);
_tracker.MinPosition = new Vector3(-(float)Root.ActualWidth, -refreshPanelHeight, 0);

// Use the Tacker's Position (negated) to apply to the Offset of the Image.
// The -{refreshPanelHeight} is to hide the refresh panel
m_positionExpression = _compositor.CreateExpressionAnimation($"-tracker.Position.Y - {refreshPanelHeight} ");
m_positionExpression.SetReferenceParameter("tracker", _tracker);
_contentPanelVisual.StartAnimation("Offset.Y", m_positionExpression);
```

Con esta configuración, el panel de actualización está fuera de la ventanilla en su posición de inicio y todo lo que ve el usuario es listView cuando el movimiento panorámico alcanza el valor de ContentPanel, se desencadena el evento PointerPressed, donde se pide al sistema que use InteractionTracker para impulsar la experiencia de manipulación.

```csharp
private void Window_PointerPressed(object sender, PointerRoutedEventArgs e)
{
if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch) {
 // Tell the system to use the gestures from this pointer point (if it can).
 _interactionSource.TryRedirectForManipulation(e.GetCurrentPoint(null));
 }
}
```

> [!NOTE]
> Si no es necesario encadenar eventos controlados, se puede Agregar el controlador PointerPressedEvent directamente a través del marcado XAML mediante el atributo ( `PointerPressed="Window_PointerPressed"` ).

El siguiente paso consiste en configurar los modificadores de origen. Usará 2 modificadores de origen para obtener este comportamiento; _Resistencia_ y _detención_.

- Resistencia: mueva el DeltaPosition. Y a la mitad de la velocidad hasta que alcance el alto de la RefreshPanel.

```csharp
CompositionConditionalValue resistanceModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation resistanceCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y < {pullToRefreshDistance}");
resistanceCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation resistanceAlternateValue = _compositor.CreateExpressionAnimation(
 "source.DeltaPosition.Y / 3");
resistanceAlternateValue.SetReferenceParameter("source", _interactionSource);
resistanceModifier.Condition = resistanceCondition;
resistanceModifier.Value = resistanceAlternateValue;
```

- Detener: dejar de moverse después de que todo el RefreshPanel esté en la pantalla.

```csharp
CompositionConditionalValue stoppingModifier = CompositionConditionalValue.Create (_compositor);
ExpressionAnimation stoppingCondition = _compositor.CreateExpressionAnimation(
 $"-tracker.Position.Y >= {pullToRefreshDistance}");
stoppingCondition.SetReferenceParameter("tracker", _tracker);
ExpressionAnimation stoppingAlternateValue = _compositor.CreateExpressionAnimation("0");
stoppingModifier.Condition = stoppingCondition;
stoppingModifier.Value = stoppingAlternateValue;
Now add the 2 source modifiers to the InteractionTracker.
List<CompositionConditionalValue> modifierList = new List<CompositionConditionalValue>()
{ resistanceModifier, stoppingModifier };
_interactionSource.ConfigureDeltaPositionYModifiers(modifierList);
```

Este diagrama proporciona una visualización de la configuración de SourceModifiers.

![Diagrama de movimiento panorámico](images/animation/source-modifiers-diagram.png)

Ahora con el SourceModifiers, observará que cuando se desplaza hacia abajo el control ListView y llega al elemento superior, el panel de actualización se extrae a la mitad del ritmo del pan hasta que alcanza el alto de RefreshPanel y, a continuación, se detiene.

En el ejemplo completo, se usa una animación de fotogramas clave para girar un icono durante la interacción en el lienzo RefreshPanel. Cualquier contenido puede usarse en su lugar o usar la posición de InteractionTracker para dirigir esa animación por separado.
