---
Description: Obtenga información sobre cómo Fluent movimiento usa direccionalidad y gravedad.
title: Direccionalidad y gravedad - animación en aplicaciones para UWP
label: Directionality and gravity
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8f1e36f0febeeaac5a12d408d7be8a717f0ab398
ms.sourcegitcommit: 7c3b88198178d6f6a535f35e1bf8665410d41d92
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569123"
---
# <a name="directionality-and-gravity"></a>Direccionalidad y gravedad

Las señales bidireccionales ayudan a solidificar el modelo mental del viaje de un usuario a través de experiencias. Es importante que la dirección del movimiento admita tanto la continuidad del espacio como la integridad de los objetos en el espacio.

El movimiento direccional está sujeto a fuerzas como la gravedad. Aplicar fuerzas al movimiento refuerza la apariencia natural del movimiento.

## <a name="examples"></a>Ejemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/category/Motion">abrir la aplicación y ver Motion en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="direction-of-movement"></a>Dirección de movimiento

:::row:::
    :::column:::
Dirección del movimiento corresponde al movimiento físico. Al igual que en la naturaleza, los objetos se pueden mover en cualquier eje del mundo - X, Y, Z. Así es cómo consideramos el movimiento de objetos en la pantalla.
Al mover objetos, evitar colisiones no naturales. Tenga en cuenta que objetos proceden de y vayan a y siempre admiten construcciones de nivel superior que se pueden usar en la escena, como la jerarquía de dirección o el diseño de desplazamiento.
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
**Avance en** celebrar contenido introducir la escena de manera que no entren en conflicto con el contenido saliente. Contenido disminuye la velocidad a la escena.
    :::column-end:::
    :::column:::
        ![direction forward in](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Avance horizontal** contenido se cierra rápidamente. Aceleran los objetos fuera de la pantalla.
    :::column-end:::
    :::column:::
        ![direction forward out](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Con versiones anteriores de** igual que avance en, pero puede revertir.
    :::column-end:::
    :::column:::
        ![direction backward in](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Con versiones anteriores de salida** igual que el reenvío de salida, pero puede revertir.
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

- [Información general del movimiento](index.md)
- [Control de tiempo y de aceleración](timing-and-easing.md)
