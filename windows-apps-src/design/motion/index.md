---
author: mijacobs
Description: Purposeful, well-designed motion brings your app to life and makes the experience feel crafted and polished. Help users understand context changes, and tie experiences together with visual transitions.
title: Movimiento y animación en las aplicaciones para UWP
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: jeffarn
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: def37c31ef0a64a9b1017d40d281457513fba0db
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6984383"
---
# <a name="motion-for-uwp-apps"></a>Movimiento para las aplicaciones para UWP

![imagen principal](images/header-motion2.svg)

El movimiento de Fluent sirve para un objetivo de la aplicación. Proporciona comentarios inteligentes basado en el comportamiento del usuario, mantiene dinámica la sensación de la interfaz de usuario y guía la navegación del usuario en tu aplicación. El movimiento de Fluent provoca una conexión emocional entre un usuario y su experiencia digital. Nos basamos en el movimiento natural que el usuario comprende del mundo físico y ampliamos nuestro sistema desde allí.

## <a name="fluent-motion-principles"></a>Principios de movimiento de Fluent

### <a name="physical"></a>Físico

Los objetos en movimiento muestran los comportamientos de los objetos en el mundo real. El movimiento fluido y dinámico hace que la experiencia resulte natural, ya que se crean conexiones emocionales y se agrega personalidad.

![Ejemplo de interfaz de usuario de movimiento físico](images/Physical.gif)
> Al interactuar con la interfaz de usuario a través de la función táctil, el movimiento de la interfaz de usuario está relacionado directamente con la velocidad de la interacción. Y como la función táctil es manipulación directa, el objeto con el que interactúas afecta a los objetos que lo rodean.

### <a name="functional"></a>Funcional

El movimiento sirva para un propósito y tiene certeza. Guía al usuario a través de la complejidad y ayuda a establecer jerarquía. El movimiento ofrece la sensación de rendimiento mejorado y optimiza la experiencia del usuario ocultando latencia percibida.

![Ejemplo de interfaz de usuario de movimiento funcional](images/functional.gif)
> Las transiciones de página son específicas. Ofrecen sugerencias sobre cómo se relacionan las páginas entre sí. Se mueven de forma que se percibe como rápida incluso cuando el rendimiento no es óptimo.

### <a name="continuous"></a>Continua

El movimiento fluido de punto a punto llama la atención de forma natural y guía al usuario. Une de manera elegante la tarea de un usuario, lo que hace que resulte más consumible y agradable.

![Ejemplo de interfaz de usuario de movimiento continuo](images/continuous3.gif)
> Los objetos pueden pasar de una escena a otra o alterar su morfología dentro de una escena para proporciona continuidad y ayudar al usuario a mantener el contexto.

### <a name="contextual"></a>Contextual

El movimiento inteligente proporciona información al usuario de una manera que está conforme a cómo manipuló la interfaz de usuario. La interacción se centra en torno al usuario. El movimiento resulta adecuado para el factor de forma y está diseñado alrededor del escenario. Debe ser cómodo para cada usuario.

![Ejemplo de interfaz de usuario de movimiento contextual](images/Contextual.gif)
> La animación se debe vincular de vuelta a la interacción del usuario. Se implementa un menú contextual desde un punto donde lo activó el usuario. 

## <a name="motion-articles"></a>Artículos de movimiento

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
