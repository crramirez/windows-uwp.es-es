---
title: Manipulaciones personalizadas con InteractionTracker
description: Use las API de InteractionTracker para crear experiencias de manipulación personalizadas.
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, animación
ms.localizationpriority: medium
ms.openlocfilehash: 8a4b682f009a4ac1350ceee3b8c23fe5e772150d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163599"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>Experiencias de manipulación personalizada con InteractionTracker

En este artículo, se muestra cómo usar InteractionTracker para crear experiencias de manipulación personalizadas.

## <a name="prerequisites"></a>Requisitos previos

Aquí se supone que está familiarizado con los conceptos descritos en estos artículos:

- [Animaciones controladas por entradas](input-driven-animations.md)
- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>¿Por qué crear experiencias de manipulación personalizadas?

En la mayoría de los casos, el uso de los controles de manipulación pregenerados es lo suficientemente bueno como para crear experiencias de IU. Pero ¿qué ocurre si quisiera diferenciar de los controles comunes? ¿Qué ocurre si quisiera crear una experiencia específica controlada por la entrada o tener una interfaz de usuario en la que el movimiento de manipulación tradicional no sea suficiente? Aquí es donde entra en la creación de experiencias personalizadas. Permiten a los desarrolladores y diseñadores de aplicaciones ser más creativos: traer a las experiencias de movimiento de vida que mejoran su personalización de marca y su lenguaje de diseño personalizado. Desde el principio, se le concede acceso al conjunto correcto de bloques de creación para personalizar completamente una experiencia de manipulación: desde cómo debe responder el movimiento con el dedo hacia y hacia la pantalla hasta los puntos de acoplamiento y la cadena de entrada.

A continuación se muestran algunos ejemplos comunes de Cuándo crearía una experiencia de manipulación personalizada:

- Agregar un comportamiento personalizado de deslizar, eliminar y descartar
- Efectos controlados por entrada (la panorámica hace que el contenido se desenfoque)
- Controles personalizados con movimientos de manipulación adaptados (ListView personalizada, ScrollViewer, etc.)

![Ejemplo de deslizador deslizante](images/animation/swipe-scroller.gif)

![Extraer del ejemplo de animación](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>¿Por qué usar InteractionTracker?

InteractionTracker se presentó en el espacio de nombres Windows. UI. Composition. Interactions en la versión del SDK de 10586. InteractionTracker habilita:

- **Flexibilidad completa** : queremos poder personalizar y adaptar todos los aspectos de una experiencia de manipulación. en concreto, los movimientos exactos que se producen durante o en respuesta a la entrada. Al crear una experiencia de manipulación personalizada con InteractionTracker, todos los botones que necesita están a su disposición.
- **Rendimiento uniforme** : uno de los desafíos de la manipulación es que su rendimiento depende del subproceso de la interfaz de usuario. Esto puede afectar negativamente a cualquier experiencia de manipulación cuando la interfaz de usuario está ocupada. InteractionTracker se creó para que use el nuevo motor de animación que funciona en un subproceso independiente a 60 FPS, lo que da lugar a un movimiento suave.

## <a name="overview-interactiontracker"></a>Información general: InteractionTracker

Al crear experiencias de manipulación personalizadas, hay dos componentes principales con los que interactúa. Los analizaremos primero:

- [InteractionTracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) : el objeto principal que mantiene una máquina de Estados cuyas propiedades están controladas por la entrada de usuario activa o las actualizaciones directas y animaciones. Está diseñado para enlazar a un CompositionAnimation para crear el movimiento de manipulación personalizado.
- [VisualInteractionSource](/uwp/api/windows.ui.composition.interactions.visualinteractionsource) : un objeto de complemento que define cuándo y en qué condiciones se envía la entrada a InteractionTracker. Define el CompositionVisual que se usa para la prueba de posicionamiento, así como otras propiedades de configuración de entrada.

Como una máquina de Estados, las propiedades de InteractionTracker se pueden controlar mediante cualquiera de los siguientes elementos:

- Interacción directa del usuario: el usuario final está manipulando directamente dentro de la región de prueba de posicionamiento de VisualInteractionSource
- Inercia: desde la velocidad de programación o un gesto de usuario, las propiedades de InteractionTracker se animan bajo una curva de inercia.
- CustomAnimation: una animación personalizada que se dirige directamente a una propiedad de InteractionTracker

### <a name="interactiontracker-state-machine"></a>Máquina de Estados de InteractionTracker

Como se mencionó anteriormente, InteractionTracker es una máquina de Estados con 4 Estados: cada uno de los cuales puede realizar la transición a cualquiera de los otros cuatro Estados. (Para obtener más información sobre cómo InteractionTracker transiciones entre estos Estados, consulte la documentación de la clase [InteractionTracker](/uwp/api/windows.ui.composition.interactions.interactiontracker) ).

| State | Descripción |
|-------|-------------|
| Inactivo | No hay entradas ni animaciones activas. |
| Interacción | Entrada de usuario activo detectada |
| Inercia | Movimiento activo resultante de la entrada activa o la velocidad de programación |
| CustomAnimation | Movimiento activo resultante de una animación personalizada |

En cada uno de los casos en los que el estado de InteractionTracker cambia, se genera un evento (o una devolución de llamada) que puede escuchar. Para poder escuchar estos eventos, deben implementar la interfaz [IInteractionTrackerOwner](/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) y crear su objeto InteractionTracker con el método CreateWithOwner. En el diagrama siguiente también se describe cuándo se desencadenan los distintos eventos.

![Máquina de Estados de InteractionTracker](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>Usar VisualInteractionSource

Para que InteractionTracker se controle por entrada, debe conectar un VisualInteractionSource (VIS) a él. El VIS se crea como un objeto de complemento mediante CompositionVisual para definir:

1. La región de prueba de posicionamiento en la que se realizará el seguimiento de la entrada y los movimientos de espacio de coordenadas se detectan en
1. Las configuraciones de entrada que se detectarán y enrutarán, algunas incluyen:
    - Movimientos detectables: posición X e y (movimiento panorámico horizontal y vertical), escala (Pinch)
    - Inercia
    - Encadenamiento de raíles &
    - Modos de redireccionamiento: Qué datos de entrada se redirigen automáticamente a InteractionTracker

> [!NOTE]
> Dado que el VisualInteractionSource se crea basándose en la posición de la prueba de posicionamiento y en el espacio de coordenadas de un visual, se recomienda no usar un visual que esté en movimiento o cambie la posición.

> [!NOTE]
> Puede usar varias instancias de VisualInteractionSource con el mismo InteractionTracker si hay varias regiones de prueba de posicionamiento. Sin embargo, el caso más común es usar un solo VIS.

VisualInteractionSource también es responsable de la administración cuando los datos de entrada de diferentes modalidades (Touch, PTP, Pen) se enrutan a InteractionTracker. Este comportamiento se define mediante la propiedad ManipulationRedirectionMode. De forma predeterminada, todas las entradas de puntero se envían al subproceso de interfaz de usuario y la entrada Touchpad de precisión va a VisualInteractionSource y InteractionTracker.

Por lo tanto, si desea que el toque y el lápiz (Creators Update) controlen una manipulación a través de VisualInteractionSource y InteractionTracker, debe llamar al método VisualInteractionSource. TryRedirectForManipulation. En el siguiente fragmento de código corto de una aplicación XAML, se llama al método cuando se produce un evento Touch pressed en la cuadrícula de UIElement en la parte superior:

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>Vincular con ExpressionAnimations

Al usar InteractionTracker para impulsar una experiencia de manipulación, interactúa principalmente con las propiedades de escala y posición. Al igual que otras propiedades de CompositionObject, estas propiedades pueden ser el destino y se puede hacer referencia a ellas en una CompositionAnimation, normalmente ExpressionAnimations.

Para usar InteractionTracker dentro de una expresión, haga referencia a la propiedad Position (o Scale) del rastreador como en el ejemplo siguiente. Como la propiedad de InteractionTracker se modifica debido a cualquiera de las condiciones descritas anteriormente, el resultado de la expresión también cambia.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> Al hacer referencia a la posición de InteractionTracker en una expresión, debe negarse el valor de la expresión resultante para que se mueva en la dirección correcta. Esto se debe a que la progresión de InteractionTracker desde el origen en un gráfico y le permite pensar en la progresión de InteractionTracker en coordenadas del mundo real, como la distancia desde su origen.

## <a name="get-started"></a>Introducción

Para empezar a usar InteractionTracker para crear experiencias de manipulación personalizadas:

1. Cree el objeto InteractionTracker con InteractionTracker. Create o InteractionTracker. CreateWithOwner.
    - (Si usa CreateWithOwner, asegúrese de que implementa la interfaz IInteractionTrackerOwner).
1. Establezca la posición mínima y máxima de la InteractionTracker recién creada.
1. Cree su VisualInteractionSource con CompositionVisual.
    - Asegúrese de que el valor visual que se pasa tiene un tamaño distinto de cero. De lo contrario, no se realizará correctamente la prueba de posicionamiento.
1. Establezca las propiedades de VisualInteractionSource.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - Encadenamiento de raíles &
1. Agregue VisualInteractionSource a InteractionTracker mediante InteractionTracker. InteractionSources. Add.
1. Configure TryRedirectForManipulation para cuando se detecten entradas táctiles y manuscritas.
    - En el caso de XAML, esto se realiza normalmente en el evento PointerPressed del UIElement.
1. Cree un ExpressionAnimation que haga referencia a la posición de InteractionTracker y tenga como destino la propiedad de CompositionObject.

Este es un fragmento de código breve que muestra #1-5 en acción:

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSource
    var source = VisualInteractionSource.Create(hitTestRoot);

    // #4 Set the properties for the VisualInteractionSource
    source.ManipulationRedirectionMode =
        VisualInteractionSourceRedirectionMode.CapableTouchpadOnly;
    source.PositionXSourceMode = InteractionSourceMode.EnabledWithInertia;
    source.PositionYSourceMode = InteractionSourceMode.EnabledWithInertia;

    // #5 Add the VisualInteractionSource to InteractionTracker
    tracker.InteractionSources.Add(source);
}
```

Para obtener usos más avanzados de InteractionTracker, consulte los siguientes artículos:

- [Crear puntos de acoplamiento con InertiaModifiers](inertia-modifiers.md)
- [Incorporación de cambios a la actualización con SourceModifiers](source-modifiers.md)