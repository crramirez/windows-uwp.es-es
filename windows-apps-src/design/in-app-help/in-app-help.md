---
Description: Diseñar una ayuda eficaz para que se muestre de forma reactiva desde la aplicación.
title: Directrices para el diseño de la ayuda en la aplicación.
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: 4783d28e4da6c06df0d0676f4a7d28ef3995481a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610060"
---
# <a name="in-app-help-pages"></a>Páginas de ayuda en la aplicación

La mayoría de las veces, es mejor que la ayuda se muestre dentro la aplicación y cuando el usuario solicite verla.

## <a name="when-to-use-in-app-help-pages"></a>Cuándo usar páginas de ayuda en la aplicación

La ayuda en la aplicación debe ser el método predeterminado para mostrar ayuda al usuario. Se debe usar para todo tipo de ayuda manera simple y sencilla y sin introducir contenido nuevo para el usuario. Instrucciones, consejos, sugerencias y trucos son el contenido adecuado para incluir en la ayuda en la aplicación.

No es fácil hacer referencia de manera rápida a instrucciones complejas o tutoriales y además, ocupan mucho espacio. Por lo que es preferible hospedarlas externamente y no incorporados en la propia aplicación.

Los usuarios no deben buscar para obtener ayuda sobre instrucciones básicas o para descubrir nuevas características. Si necesitas una ayuda que enseñe a los usuarios, usa una interfaz de usuario informativa.

## <a name="types-of-in-app-help"></a>Tipos de ayuda en la aplicación

Hay varias maneras de incluir la ayuda en la aplicación, aunque todas siguen los mismos principios generales de diseño y facilidad de uso.

#### <a name="help-pages"></a>Páginas de ayuda

Incluir una página independiente o varias dentro de la aplicación es una forma rápida y sencilla de mostrar instrucciones útiles.

-   **Ser conciso:** Una gran biblioteca de temas de ayuda es difícil de manejar y adecuado para obtener ayuda en la aplicación.
-   **Ser coherente:** Asegúrese de que pueden llegar a las páginas de ayuda a los usuarios del mismo modo desde cualquier parte de la aplicación. Nunca deberían buscar la ayuda.
-   **Examen de los usuarios, no de lectura:** Dado que la Ayuda que se va a buscar un usuario podría estar en la misma página que otros temas de ayuda, asegúrese de que pueden saber fácilmente que quieran centrarse en.


#### <a name="popups"></a>Elementos emergentes

Los elementos emergentes permiten ofrecer una ayuda muy contextual, es decir, mostrar las instrucciones y recomendaciones relevantes para la tarea específica que el usuario intenta realizar.

-   **Se centran en uno de los problemas:** Espacio está restringido aún más en un menú emergente a una página de ayuda. Los elementos emergentes de ayuda deben hacer referencia de manera específica a una sola tarea para resultar eficaces.
-   **Visibilidad es importante:** Dado que solo se pueden ver elementos emergentes de Ayuda desde una ubicación, asegúrese de que está claramente visibles para el usuario sin ser obstructiva. Si no lo ve claramente, es posible que el usuario abandone el elemento emergente en busca de una página de ayuda.
-   **No usa demasiados recursos:** Ayuda no debe retrasarse o ser lentitud al cargar. Si incluyes vídeos o archivos de audio de alta resolución en los elementos emergentes, es más probable que frustres al usuario en lugar de ayudarlo.

#### <a name="descriptions"></a>Descripciones

En ocasiones, puede ser útil proporcionar más información acerca de una característica cuando un usuario la esté inspeccionando. Las descripciones son similares a la interfaz de usuario informativa, pero la principal diferencia entre ellas reside en que la segunda intenta enseñar y ofrecer indicaciones al usuario sobre características que no conocen, mientras que una descripción detallada mejora la comprensión sobre características de aplicaciones en las que el usuario ya está interesado.

-   **No se enseñan los conceptos básicos:** Se supone que el usuario ya conoce los aspectos básicos de cómo usar el elemento que se describe. Aclarar u ofrecer más información resulta útil; decirle lo que ya conoce, no.
-   **Describe las interacciones interesantes:** Uno de los mejores usos de descripciones es instruye al usuario sobre cómo pueden interactuar una que ya conocen acerca de características. De este modo, obtendrá más información sobre algo que ya le gusta usar.
-   **Permanecer fuera de la vista:** Mucho como interfaz de usuario con instrucciones, descripciones deben evitar la interferencia con el disfrute de un usuario de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

* [Directrices para obtener ayuda de la aplicación](guidelines-for-app-help.md)
