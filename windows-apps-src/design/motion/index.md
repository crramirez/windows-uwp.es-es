---
Description: Un movimiento intencionado y bien diseñado da vida a las aplicaciones y ofrece una experiencia que se percibe elaborada y elegante. Ayuda a los usuarios a comprender los cambios contextuales y relaciona las experiencias con transiciones visuales.
title: Movimiento y la animación en las aplicaciones para UWP
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 31cf2134fb8f77809b75a5abf3e6980443452059
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "68867402"
---
# <a name="motion-for-uwp-apps"></a>Movimiento para las aplicaciones para UWP

![Icono de movimiento](../images/motion-2x.png)

El movimiento de Fluent tiene un propósito en la aplicación. Proporciona comentarios inteligentes basados en el comportamiento del usuario, mantiene dinámica la sensación de la interfaz de usuario y guía la navegación del usuario en tu aplicación. El movimiento de Fluent provoca una conexión emocional entre un usuario y su experiencia digital. Nos basamos en el movimiento natural que el usuario comprende del mundo físico y ampliamos nuestro sistema desde ahí.

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

## <a name="fluent-motion-principles"></a>Principios de movimiento de Fluent

### <a name="physical"></a>Físico

Los objetos en movimiento muestran los comportamientos de los objetos en el mundo real. El movimiento fluido y dinámico hace que la experiencia resulte natural, ya que se crean conexiones emocionales y se agrega personalidad.

![Ejemplo de interfaz de usuario de movimiento físico](images/Physical.gif)
> Al interactuar con la interfaz de usuario a través de la función táctil, el movimiento de la interfaz de usuario está relacionado directamente con la velocidad de la interacción. Y como la función táctil es manipulación directa, el objeto con el que interactúas afecta a los objetos que están a su alrededor.

### <a name="functional"></a>Funcional

El movimiento tiene un propósito y es convincente. Guía al usuario a través de la complejidad y ayuda a establecer una jerarquía. El movimiento ofrece la sensación de rendimiento mejorado y optimiza la experiencia del usuario al ocultar la latencia percibida.

![Ejemplo de interfaz de usuario de movimiento funcional](images/functional.gif)
> Las transiciones de página son específicas. Ofrecen sugerencias sobre cómo se relacionan las páginas entre sí. Se mueven de forma que se perciben como rápidas incluso cuando el rendimiento no es óptimo.

### <a name="continuous"></a>Continua

El movimiento fluido de punto a punto llama la atención de forma natural y guía al usuario. Une de manera elegante la tarea de un usuario, lo que hace que resulte más cómoda y agradable.

![Ejemplo de interfaz de usuario de movimiento continuo](images/continuous3.gif)
> Los objetos pueden pasar de una escena a otra o alterar su morfología dentro de una escena para proporcionar continuidad y ayudar al usuario a mantener el contexto.

### <a name="contextual"></a>Contextual

El movimiento inteligente proporciona información al usuario de una manera que concuerda con cómo manipuló la interfaz de usuario. La interacción se centra en torno al usuario. El movimiento resulta adecuado para el factor de forma y está diseñado alrededor del escenario. Debe ser cómodo para cada usuario.

![Ejemplo de interfaz de usuario de movimiento contextual](images/Contextual.gif)
> La animación se debe vincular a la interacción del usuario. Se implementa un menú contextual desde un punto donde lo activó el usuario.

## <a name="motion-articles"></a>Artículos sobre movimiento

:::row:::
    :::column:::
### <a name="timing-and-easing"></a>[Sincronización y aceleración](timing-and-easing.md)
La sincronización y la aceleración son elementos importantes para hacer que el movimiento resulte natural para objetos que entran, salen o se mueven dentro de la interfaz de usuario.
    :::column-end:::
    :::column:::
### <a name="directionality-and-gravity"></a>[Direccionalidad y gravedad](directionality-and-gravity.md)
Las señales direccionales ayudan a proporcionar un sólido modelo mental del recorrido de un usuario a través de experiencias. El movimiento direccional está sujeto a fuerzas como la gravedad, lo que refuerza la sensación natural de movimiento.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
### <a name="page-transitions"></a>[Transiciones de página](page-transitions.md)
Las transiciones de página sirven para que los usuarios se desplacen entre las páginas de una aplicación, proporcionando comentarios acerca de la relación entre ellas. Ayudan a los usuarios a entender dónde se encuentran en la jerarquía de navegación.
    :::column-end:::
    :::column:::
### <a name="connected-animation"></a>[Animación conectada](connected-animation.md)
Las animaciones conectadas te permiten crear una experiencia de navegación dinámica y atractiva al animar la transición de un elemento entre dos vistas distintas.
    :::column-end:::
:::row-end:::
