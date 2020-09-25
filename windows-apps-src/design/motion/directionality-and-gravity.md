---
title: 'Direccionalidad y gravedad: animación en aplicaciones de Windows'
description: Obtenga información sobre el uso de la dirección de movimiento, la dirección de navegación y la gravedad en escenas animadas mediante la visualización de ejemplos.
label: Directionality and gravity
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 649fb9e2833a425ccad78f5f0bcb69ccb4091bca
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217808"
---
# <a name="directionality-and-gravity"></a>Direccionalidad y gravedad

Las señales direccionales ayudan a solidificar el modelo mental del viaje que un usuario toma entre las experiencias. Es importante que la dirección de cualquier movimiento admita tanto la continuidad del espacio como la integridad de los objetos en el espacio.

El movimiento direccional está sujeto a fuerzas como la gravedad. Aplicar fuerzas al movimiento refuerza el funcionamiento natural del movimiento.

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

## <a name="direction-of-movement"></a>Dirección del movimiento

:::row:::
    :::column:::
La dirección del movimiento corresponde al movimiento físico. Al igual que en la naturaleza, los objetos pueden moverse en cualquier eje mundial (X, Y, Z). Así es como pensamos en el movimiento de objetos en la pantalla.
Cuando mueva objetos, evite colisiones no naturales. Tenga en cuenta que los objetos proceden de y van a, y siempre admiten construcciones de nivel superior que se pueden usar en la escena, como la dirección de desplazamiento o la jerarquía de diseño.
    :::column-end:::
    :::column:::
        ![dirección hacia atrás en](images/Direction.gif)
    :::column-end:::
:::row-end:::

## <a name="direction-of-navigation"></a>Dirección de navegación

La dirección de navegación entre las escenas de la aplicación es conceptual. Los usuarios navegan hacia delante y hacia atrás. Las escenas se mueven y salen de la vista. Estos conceptos se combinan con el traslado físico para guiar al usuario.

Cuando la navegación hace que un objeto viaje de la escena anterior a la nueva escena, el objeto hace que se mueva una simple a B en la pantalla. Para asegurarse de que el movimiento es más físico, se agrega la aceleración estándar, así como la sensación de gravedad.

Para la navegación hacia atrás, se invierte el movimiento (de B a a). Cuando el usuario se desplaza hacia atrás, se espera que se devuelva al estado anterior lo antes posible. El tiempo es más rápido, más directo y usa la aceleración de deceleración.

En este caso, estos principios se aplican cuando el elemento seleccionado permanece en pantalla durante la navegación hacia delante y hacia atrás.

![Ejemplo de interfaz de usuario de movimiento continuo](images/continuous3.gif)

Cuando la navegación hace que se reemplacen los elementos de la pantalla, es importante mostrar dónde salió la escena de salida y de dónde procede la nueva escena.

Esto presenta varias ventajas:

- Solidifica el modelo mental del usuario del espacio.
- La duración de la escena de salida proporciona más tiempo para preparar el contenido que se animará en para la escena entrante.
- Mejora el rendimiento percibido de la aplicación.

Hay 4 instrucciones discretas de navegación que se deben tener en cuenta.

:::row:::
    :::column:::
**Hacia delante** Celebre el contenido que entra en la escena de manera que no entre en conflicto con el contenido saliente. El contenido se ralentiza en la escena.
    :::column-end:::
    :::column:::
        ![dirección hacia delante en](images/forwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Reenvío** El contenido se cierra rápidamente. Los objetos se aceleran en pantalla.
    :::column-end:::
    :::column:::
        ![Dirección remitir hacia fuera](images/forwardOUT.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Hacia atrás** Igual que hacia delante, pero invertido.
    :::column-end:::
    :::column:::
        ![dirección hacia atrás en](images/backwardIN.gif)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
**Hacia atrás** Igual que el reenvío, pero invertido.
    :::column-end:::
    :::column:::
        ![dirección hacia atrás hacia atrás](images/backwardOUT.gif)
    :::column-end:::
:::row-end:::

## <a name="gravity"></a>Peso

La gravedad hace que las experiencias parezcan más naturales. Los objetos que se mueven en el eje Z y que no están anclados a la escena por una prestación en pantalla tienen la posibilidad de que se vea afectado por la gravedad. A medida que un objeto se rompe sin la escena y antes de que llegue a la velocidad de escape, la gravedad se extrae del objeto, lo que crea una curva más natural de la trayectoria del objeto a medida que se mueve.

Normalmente, la gravedad se manifiesta cuando un objeto debe saltar de una escena a otra. Debido a esto, la animación conectada utiliza el concepto de gravedad.

Aquí, la gravedad afecta a un elemento de la fila superior de la cuadrícula, lo que hace que se coloque ligeramente mientras deja su lugar y se desplaza hacia delante.

![dirección hacia atrás en](images/continuity-photos.gif)

## <a name="related-articles"></a>Artículos relacionados

- [Información general sobre movimiento](index.md)
- [Sincronización y aceleración](timing-and-easing.md)
