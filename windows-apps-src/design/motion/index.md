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
ms.openlocfilehash: 2701844ccefdc5a535fa8fc20086c550cb7bc29e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57583452"
---
# <a name="motion-for-uwp-apps"></a>Movimiento para las aplicaciones para UWP

![imagen principal](images/header-motion2.svg)

El movimiento de Fluent tiene un propósito en la aplicación. Proporciona comentarios inteligentes basados en el comportamiento del usuario, mantiene dinámica la sensación de la interfaz de usuario y guía la navegación del usuario en tu aplicación. El movimiento de Fluent provoca una conexión emocional entre un usuario y su experiencia digital. Nos basamos en el movimiento natural que el usuario comprende del mundo físico y ampliamos nuestro sistema desde ahí.

## <a name="fluent-motion-principles"></a>Principios de movimiento de Fluent

### <a name="physical"></a>Físico

Los objetos en movimiento muestran los comportamientos de los objetos en el mundo real. El movimiento fluido y dinámico hace que la experiencia resulte natural, ya que se crean conexiones emocionales y se agrega personalidad.

![Ejemplo de interfaz de usuario de movimiento físico](images/Physical.gif)
> Al interactuar con la interfaz de usuario a través de la función táctil, el movimiento de la interfaz de usuario está relacionado directamente con la velocidad de la interacción. Y como la función táctil es manipulación directa, el objeto con el que interactúas afecta a los objetos que lo rodean.

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
        ### [Timing and easing](timing-and-easing.md)
        Timing and easing are important elements that make motion feel natural for objects entering, exiting, or moving within the UI.
    :::column-end:::
    :::column:::
        ### [Directionality and gravity](directionality-and-gravity.md)
        Directional signals help provide a solid mental model of the journey a user takes across experiences. Directional movement is subject to forces like gravity, which reinforces the natural feel of the movement.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ### [Page transitions](page-transitions.md)
        Page transitions navigate users between pages in an app, providing feedback about the relationship between pages. They help users understand where they are in the navigation hierarchy.
    :::column-end:::
    :::column:::
        ### [Connected animation](connected-animation.md)
        Connected animations let you create a dynamic and compelling navigation experience by animating the transition of an element between two different views.
    :::column-end:::
:::row-end:::
