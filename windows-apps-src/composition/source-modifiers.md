---
title: Extracción de actualización con modificadores de origen (SourceModifiers)
description: Crea controles personalizados Pull-to-Refresh con SourceModifiers
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: 834f631cd5c4b8696e75f83f194b95f809b1cf8a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8347728"
---
# <a name="pull-to-refresh-with-source-modifiers"></a>Extracción de actualización con modificadores de origen

En este artículo analizaremos más a fondo cómo usar la característica de SourceModifier de un InteractionTracker y mostraremos su uso mediante la creación de un control personalizado de extracción de actualización (Pull-to-Refresh).

## <a name="prerequisites"></a>Requisitos previos

En este artículo damos por hecho que estás familiarizado con los conceptos tratados en estos artículos:

- [Animaciones controladas por entradas](input-driven-animations.md)
- [Experiencias de manipulación personalizadas con InteractionTracker](interaction-tracker-manipulations.md)
- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="what-is-a-sourcemodifier-and-why-are-they-useful"></a>¿Qué es un SourceModifier y por qué resultan útiles?

Al igual que los [InertiaModifiers](inertia-modifiers.md), los SourceModifiers proporcionan un mayor control sobre el movimiento de un InteractionTracker. Pero a diferencia de los InertiaModifiers, que definen el movimiento después de que InteractionTracker entre en el estado de inercia, los SourceModifiers definen el movimiento mientras InteractionTracker sigue en el estado de interacción. En estos casos, la experiencia que se desea diferente que la tradicional de "obedecer al dedo".

Un ejemplo clásico de esto es la experiencia de extracción para actualización: cuando el usuario extrae la lista para actualizar el contenido y la lista se mueve de forma panorámica a la misma velocidad que el dedo y se detiene tras una distancia determinada, el movimiento puede parecer brusco y mecánico. Una experiencia más natural sería presentar una especie de resistencia mientras el usuario interactúa activamente con la lista. Este pequeño matice ayuda a la experiencia general del usuario final cuando interactúa con una lista, que es más dinámica y atractiva. En la sección Ejemplo, analizaremos en más detalle sobre cómo crear esta experiencia.

Existen 2 tipos de SourceModifiers:

- DeltaPosition: Es la diferencia entre la posición actual del fotograma y la posición anterior de fotograma del dedo durante la interacción de movimiento panorámico táctil. Este modificador de origen te permite modificar la posición diferencial de la interacción antes de enviarla para su procesamiento posterior. Se trata de un parámetro de tipo Vector3 y el desarrollador puede optar por modificar cualquiera de los atributos X, Y o Z de la posición antes de pasarlo al InteractionTracker.
- DeltaScale: Es la diferencia entre la escala actual del fotograma del y la escala del fotograma anterior que se ha aplicado durante la interacción de zoom táctil. Este modificador de origen te permite modificar el nivel de zoom de la interacción. Se trata de un tipo de atributo float que el desarrollador puede modificar antes de pasarlo a InteractionTracker.

Cuando InteractionTracker entra en estado de inercia, evalúa todos los SourceModifiers que tiene asignados y determina si alguno de ellos es aplicable. Esto significa que puedes crear y asignar varios modificadores de origen a un InteractionTracker. Pero, al definir cada uno de ellos, debes hacer lo siguiente:

1. Definir la condición: Una expresión que defina la instrucción condicional cuando debe aplicarse este SourceModifier específico.
1. Definir DeltaPosition/DeltaScale: La expresión del modificador de origen que modifique DeltaPosition o DeltaScale cuando se cumpla la condición definida anteriormente.

## <a name="example"></a>Ejemplo

Ahora veamos cómo puedes usar los modificadores de origen para crear una experiencia de extracción de actualización personalizada con un control ListView de XAML existente. Vamos a usar un lienzo como el "Panel de actualización" que se apilará sobre un control ListView de XAML para crear esta experiencia.

Para la experiencia del usuario final, queremos crear el efecto de "resistencia" mientras el usuario esté realizando activamente el movimiento panorámico de la lista (con la función táctil) y deje de realizarlo después de que la posición vaya más allá de un punto determinado.

![Lista con extracción de actualización](images/animation/city-list.gif)

El código de trabajo para esta experiencia se puede encontrar en el [repositorio de Windows UI Dev Labs en GitHub](https://github.com/Microsoft/WindowsUIDevLabs). Este es el recorrido paso a paso para la creación de esa experiencia.
En el código de marcado XAML, tienes lo siguiente:

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

Dado que ListView (`ThumbnailList`) es un control XAML que ya desplaza, necesitas que el desplazamiento se encadene a su elemento principal(`ContentPanel`) cuando alcance el elemento más alto y no se pueda desplazar más. (Los modificadores de origen se aplicarán en ContentPanel). Para ello debes establecer ScrollViewer.IsVerticalScrollChainingEnabled como **true** en el marcado de ListView. También deberás establecer el modo de encadenamiento de VisualInteractionSource en **Always**.

Debes establecer el controlador PointerPressedEvent con el parámetro _handledEventsToo_ en **true**. Sin esta opción, PointerPressedEvent no se encadenará a ContentPanel, ya que el control ListView marcará esos eventos como controlados y no se enviarán a la cadena visual.

```csharp
//The PointerPressed handler needs to be added using AddHandler method with the //handledEventsToo boolean set to "true"
//instead of the XAML element's "PointerPressed=Window_PointerPressed",
//because the list view needs to chain PointerPressed handled events as well.
ContentPanel.AddHandler(PointerPressedEvent, new PointerEventHandler( Window_PointerPressed), true);
```

Ahora estás listo para vincular esto con InteractionTracker. Empieza por configurar InteractionTracker, VisualInteractionSource y la expresión que aprovechará la posición de InteractionTracker.

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

Con esta configuración, el panel de actualización está fuera de la ventanilla en su posición inicial y todo lo el usuario ve es listView. Cuando el movimiento panorámico alcanza ContentPanel, se activará el evento PointerPressed, en el que pedirás al sistema que use InteractionTracker para controlar la experiencia de manipulación.

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
> Si no es necesario el encadenamiento de eventos controlados, se puede agregar el controlador PointerPressedEvent directamente mediante el marcado XAML con el atributo (`PointerPressed="Window_PointerPressed"`).

El paso siguiente es configurar los modificadores de origen. Vas a utilizar 2 modificadores de origen para obtener este comportamiento; _Resistance_ y _Stop_.

- Resistance: Mueve DeltaPosition.Y a la mitad de la velocidad hasta que llega a la altura del panel de actualización (RefreshPanel).

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

- Stop: Deja de moverse una vez que RefreshPanel está en pantalla.

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

Este diagrama ofrece una visualización de la configuración de los SourceModifiers.

![Diagrama del movimiento panorámico](images/animation/source-modifiers-diagram.png)

Ahora, con los SourceModifiers, observarás que cuando el se realiza el movimiento panorámico de ListView hacia abajo y se alcanza al elemento de nivel superior, el panel de actualización se baja a la mitad del ritmo del movimiento panorámico hasta que llega a la altura de RefreshPanel y luego deja de moverse.

En la muestra completa se usa una animación de fotograma clave para hacer girar un icono durante la interacción en el lienzo RefreshPanel. En su lugar se puede usar cualquier contenido, o puedes utilizar la posición de InteractionTracker para controlar esa animación por separado.
