---
author: jwmsft
Description: Learn how Fluent motion uses directionality and gravity.
title: Direccionalidad y gravedad - animación en aplicaciones para UWP
label: Directionality and gravity
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: b61abf00d5ab8820457742f16feb9b496b7d7d1c
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "4502680"
---
# <a name="directionality-and-gravity"></a>Direccionalidad y gravedad

Las señales bidireccionales ayudan a solidificar el modelo mental del viaje de un usuario a través de experiencias. Es importante que la dirección del movimiento admita tanto la continuidad del espacio como la integridad de los objetos en el espacio.

El movimiento direccional está sujeto a fuerzas como la gravedad. Aplicar fuerzas al movimiento refuerza la apariencia natural del movimiento.

## <a name="direction-of-movement"></a>Dirección de movimiento

:::row:::
    :::column:::
        Direction of movement corresponds to physical motion. Just like in nature, objects can move in any world axis - X,Y,Z. This is how we think of the movement of objects on the screen.

        When you move objects, avoid unnatural collisions. Keep in mind where objects come from and go to, and alway support higher level constructs that may be used in the scene, such as scroll direction or layout hierarchy.
    :::column-end:::
    :::column:::
        ![direction backward in](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Dirección de navegación

La dirección de navegación entre escenas en tu aplicación es conceptual. Los usuarios navegan hacia adelante y atrás. Las escenas entran y salen de la vista. Estos conceptos se combinan con un movimiento físico para guiar al usuario.

Cuando la navegación hace que el objeto viaje de la escena anterior a la nueva escena, el objeto hace un simple movimiento de A-a-B en la pantalla. Para garantizar que el movimiento sea más físico, se agrega la aceleración estándar, así como la sensación de gravedad.

Para la navegación hacia atrás, se invierte el movimiento (B-a-A). Cuando el usuario navega hacia atrás, tienen una expectativa de ser devuelto al estado anterior lo antes posible. El tiempo es más rápido, más directo y usa deceleración.

Aquí, se aplican estos principios ya que el elemento seleccionado permanece en pantalla durante la navegación hacia delante y hacia atrás.

![Ejemplo de interfaz de usuario de movimiento continuo](images/continuous3.gif)

Cuando la navegación provoca la sustitución de elementos en la pantalla, es importante mostrar a dónde se fue la escena existente y de dónde procede la nueva escena.

Esto presenta varias ventajas:

- Solidifica el modelo mental del usuario del espacio.
- La duración de la escena que sale proporciona más tiempo para preparar el contenido en el que se va a animar para la escena entrante.
- Mejora el rendimiento percibido de la aplicación.

Existen 4 direcciones de navegación discretas a tener en cuenta.

:::row:::
    :::column:::
        **Forward-In**

        Celebrate content entering the scene in a manner that does not collide with outgoing content. Content decelerates into the scene.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Forward-Out**

        Content exits quickly. Objects accelerate off screen.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-In**

        Same as Forward-In, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Backward-Out**

        Same as Forward-Out, but reversed.
    :::column-end:::
    :::column:::
        ![direction backward out](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Gravedad

La gravedad hace que tu experiencia resulte más natural. Los objetos que se mueven en el eje Z y no están anclados a la escena mediante una prestación en pantalla tienen el potencial de verse afectada por la gravedad. Cuando un objeto escapa de la escena y antes de que alcance la velocidad de escape, la gravedad atrae hacia abajo el objeto, crea una curva más natural de la trayectoria de objeto conforme se mueve.

Por lo general, la gravedad se manifiesta cuando un objeto debe saltar de una escena a otra. Por este motivo, la animación conectada utiliza el concepto de gravedad.

Aquí, un elemento de la fila superior de la cuadrícula se ve afectado por la gravedad, lo que hace que caiga ligeramente conforme deja su sitio y se mueve hacia la parte frontal.

![dirección hacia atrás dentro](images/continuity-photos.gif)

## <a name="related-articles"></a>Artículos relacionados

- [Introducción al movimiento](index.md)
- [Sincronización y aceleración](timing-and-easing.md)