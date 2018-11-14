---
author: jwmsft
title: Manipulaciones personalizadas con InteractionTracker
description: Usa las API InteractionTracker para crear experiencias de manipulación personalizadas.
ms.author: jimwalk
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, animación
ms.localizationpriority: medium
ms.openlocfilehash: 0a991d692b4ba4c7a221932218a7d25e48fe16ca
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "6448112"
---
# <a name="custom-manipulation-experiences-with-interactiontracker"></a>Experiencias de manipulación personalizadas con InteractionTracker

En este artículo mostraremos cómo usar las API InteractionTracker para crear experiencias de manipulación personalizadas.

## <a name="prerequisites"></a>Requisitos previos

En este artículo damos por hecho que estás familiarizado con los conceptos tratados en estos artículos:

- [Animaciones controladas por entradas](input-driven-animations.md)
- [Animaciones basadas en relaciones](relation-animations.md)

## <a name="why-create-custom-manipulation-experiences"></a>¿Por qué crear experiencias de manipulación personalizadas?

En la mayoría de los casos, el uso de los controles de manipulación pregenerados son suficientes crear experiencias de interfaz de usuario. Pero, ¿qué ocurre si deseas diferenciarte de los controles comunes? ¿Qué ocurre si desea crear una experiencia específica controlada por entradas o si tiene una interfaz de usuario en la que un movimiento de manipulación tradicional no es suficiente? Aquí entra en juego la creación de experiencias personalizadas. Estas permiten a los desarrolladores y diseñadores de aplicaciones sea más creativos, dar vida a experiencias de movimiento que ejemplifiquen mejor su marca y su lenguaje de diseño personalizado. Desde el principio, se otorga acceso al conjunto adecuado de bloques de creación para personalizar totalmente una experiencia de manipulación: desde cómo debe responder el movimiento con el dedo en la pantalla y fuera de ella a los puntos de acoplamiento y el encadenamiento de entradas.

A continuación mostramos algunos ejemplos de cuándo se podría crear una experiencia de manipulación personalizada:

- Agregar un comportamiento personalizado de deslizamiento del dedo, eliminar o descartar
- Efectos controlados por entradas (el movimiento panorámico hace que el contenido se desenfoque)
- Controles personalizados con movimientos de manipulación adaptados (ListView, ScrollViewer, etc. personalizados)

![Ejemplo de barra de desplazamiento al deslizar el dedo](images/animation/swipe-scroller.gif)

![Ejemplo de tirar para animar](images/animation/pull-to-animate.gif)

## <a name="why-use-interactiontracker"></a>¿Por qué usar InteractionTracker?

InteractionTracker se introdujo en el espacio de nombres Windows.UI.Composition.Interactions en la versión 10586 del SDK. InteractionTracker permite:

- **Una completa flexibilidad** : Queremos que puedas personalizar y adaptar todos los aspectos de una experiencia de manipulación; en concreto, los movimientos exactos que se producen durante una entrada o en respuesta a esta. Al crear una experiencia de manipulación personalizada con InteractionTracker, todas las herramientas que necesitas están a tu disposición.
- **Rendimiento óptimo** : Una de las dificultades de las experiencias de manipulación es que su rendimiento depende del subproceso de la interfaz de usuario. Esto puede afectar negativamente al cualquier experiencia de manipulación si la interfaz de usuario está ocupada. InteractionTracker se creó para aprovechar el nuevo motor de animación, que funciona en un subproceso independiente a 60FPS, lo que produce un movimiento optimizado.

## <a name="overview-interactiontracker"></a>Información general: InteractionTracker

Al crear experiencias de manipulación personalizadas, hay dos componentes principales con los que se interactúa. Primero los analizaremos esto:

- [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker) : El objeto principal para el mantenimiento de una máquina de estado cuyas propiedades se controlan por medio de la entrada del usuario activo o de actualizaciones directas y animaciones. Está pensado para unirse luego a un CompositionAnimation para crear el movimiento de manipulación personalizado.
- [VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource) : Un objeto complementario que define cuándo y en qué condiciones se envía la entrada a InteractionTracker. Define tanto el CompositionVisual usado para pruebas de posicionamiento como otras propiedades de configuración de las entradas.

Como máquina de estado, las propiedades de InteractionTracker se pueden controlar mediante cualquiera de las siguientes formas:

- Interacción directa del usuario: El usuario final manipula directamente en la región de las pruebas de posicionamiento de VisualInteractionSource.
- Inercia: A partir de una velocidad controlada por programación o un gesto del usuario, las propiedades de InteractionTracker se animan según una curva de inercia.
- CustomAnimation: Una animación personalizada que se dirige directamente a una propiedad de InteractionTracker.

### <a name="interactiontracker-state-machine"></a>Máquina de estado InteractionTracker

Como se mencionó anteriormente, InteractionTracker es una máquina de estado con 4 estados, cada uno de los cuales puede realizar la transición a cualquiera de los otros fourstates. (Para obtener más información acerca de cómo realiza InteractionTracker una transición entre estos estados, consulta la documentación de la clase [InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker)).

| Estado | Descripción |
|-------|-------------|
| Inactivo | No hay activa ninguna animación ni entrada de control. |
| Interactuando | Se ha detectado una entrada del usuario activo. |
| Inercia | Movimiento activo como resultado de una entrada activa o una velocidad debida a programación. |
| CustomAnimation | Un movimiento activo producto de una animación personalizada. |

En todos los casos en los que cambie el estado de InteractionTracker, se genera un evento (o devolución de llamada) que puedes escuchar. Para escuchar estos eventos, deben implementar la interfaz [IInteractionTrackerOwner](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.iinteractiontrackerowner) y crear el objeto InteractionTracker con el método CreateWithOwner. El diagrama siguiente también delinea cuándo se desencadenan los distintos eventos.

![Máquina de estado InteractionTracker](images/animation/interaction-tracker-diagram.png)

## <a name="using-the-visualinteractionsource"></a>Uso de VisualInteractionSource

Para que InteractionTracker se controle mediante entradas (Input), debe conectar a él un VisualInteractionSource (VIS). El VIS se crea como un objeto complementario utilizando un CompositionVisual para definir:

1. La región de pruebas de posicionamiento en la que se realizará el seguimiento de la entrada y el espacio de coordenadas en el que se detectan los gestos.
1. Las configuraciones de las entradas que se detectarán y se enrutarán, como, por ejemplo:
    - Gestos detectables: Posición de X e Y (movimiento panorámico horizontal y vertical), escala (gesto de reducir).
    - Inercia
    - Guías y encadenamiento
    - Modos de redirección: Qué datos de entrada se redirigen automáticamente a InteractionTracker

> [!NOTE]
> Dado que el VisualInteractionSource se crea basándose desactivar la posición de la prueba de posicionamiento y el espacio de coordenadas de un elemento Visual, se recomienda no utilizar un elemento Visual que se encontrará en movimiento o cambiará de posición.

> [!NOTE]
> Puedes usar varias instancias de VisualInteractionSource con el mismo InteractionTracker si hay varias regiones de prueba de posicionamiento. Sin embargo, lo más habitual es usar un único VIS

El VisualInteractionSource también es responsable de administrar cuándo los datos de entrada procedentes de diferentes modalidades (táctil, PTP, lápiz) se enrutan a InteractionTracker. Este comportamiento lo define la propiedad ManipulationRedirectionMode. De manera predeterminada, todas las entradas de puntero se envía al subproceso de la interfaz de usuario y las entradas del panel táctil de precisión van a VisualInteractionSource y a InteractionTracker.

Por lo tanto, si quieres que las entradas táctiles y de lápiz (Creators Update) controlen una manipulación mediante un VisualInteractionSource e InteractionTracker, debes realizar una llamada al método VisualInteractionSource.TryRedirectForManipulation. En el fragmento de código breve de debajo, procedente de una aplicación XAML, se llama al método cuando se produce un evento de pulsación táctil en la parte superior de la cuadrícula de UIElement:

```csharp
private void root_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Touch)
    {
        _source.TryRedirectForManipulation(e.GetCurrentPoint(root));
    }
}
```

## <a name="tie-in-with-expressionanimations"></a>Vinculación con ExpressionAnimations

Al utilizar InteractionTracker para controlar una experiencia de manipulación, se interactúa principalmente con las propiedades Scale y Position. Al igual que otras propiedades de CompositionObject, estas propiedades pueden ser tanto el destino como la referencia de una clase CompositionAnimation, normalmente ExpressionAnimations.

Para usar InteractionTracker en una expresión, haz referencia a la propiedad Position (o Scale) del rastreador, como en el siguiente ejemplo. A medida que la propiedad de InteractionTracker se modifica debido a cualquiera de las condiciones descritas anteriormente, también cambia la salida de la expresión.

```csharp
// With Strings
var opacityExp = _compositor.CreateExpressionAnimation("-tracker.Position");
opacityExp.SetReferenceParameter("tracker", _tracker);

// With ExpressionBuilder
var opacityExp = -_tracker.GetReference().Position;
```

> [!NOTE]
> Al hacer referencia a la posición de InteractionTracker en una expresión, debes invalidar el valor de la expresión resultante para moverlo en la dirección correcta. Esto se debe al progreso de InteractionTracker desde el origen en un gráfico y te permite pensar sobre el progreso de InteractionTracker en coordenadas del "mundo real", por ejemplo, la distancia desde su origen.

## <a name="get-started"></a>Introducción

Para empezar a usar InteractionTracker para crear experiencias de manipulación personalizadas:

1. Crea el objeto InteractionTracker con InteractionTracker.Create o InteractionTracker.CreateWithOwner.
    - (Si usas CreateWithOwner, asegúrate de implementar la interfaz IInteractionTrackerOwner.)
1. Establece la posición Min y Max del InteractionTracker recién creado.
1. Crea tu VisualInteractionSource con un CompositionVisual.
    - Asegúrate de que el objeto visual que pases tenga un tamaño de distinto de cero. De lo contrario, su posicionamiento no se probará correctamente.
1. Establece las propiedades de VisualInteractionSource.
    - VisualInteractionSourceRedirectionMode
    - PositionXSourceMode, PositionYSourceMode, ScaleSourceMode
    - Guías y encadenamiento
1. Agrega VisualInteractionSource a InteractionTracker mediante InteractionTracker.InteractionSources.Add.
1. Configura TryRedirectForManipulation para cuando se detecten entradas de lápiz y entradas táctiles.
    - En el caso de XAML, esto suele hacerse en el evento PointerPressed de UIElement.
1. Crea una ExpressionAnimation que haga referencia a la posición y el destino de InteractionTracker de una propiedad de CompositionObject.

Este es un breve fragmento de código que muestra los pasos del 1 al 5 en acción:

```csharp
private void InteractionTrackerSetup(Compositor compositor, Visual hitTestRoot)
{ 
    // #1 Create InteractionTracker object
    var tracker = InteractionTracker.Create(compositor);

    // #2 Set Min and Max positions
    tracker.MinPosition = new Vector3(-1000f);
    tracker.MaxPosition = new Vector3(1000f);

    // #3 Setup the VisualInteractionSourc
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

Para los usos más avanzados de InteractionTracker, recomendamos consultar los siguientes artículos:

- [Creación de puntos de acoplamiento con InertiaModifiers](inertia-modifiers.md)
- [Extracción de actualización con SourceModifiers](source-modifiers.md)
