---
description: Obtenga información sobre el uso de la ayuda en la aplicación reactiva como método predeterminado para mostrar la ayuda de los usuarios y sobre los tipos de ayuda en la aplicación.
title: Directrices para el diseño de la ayuda en la aplicación.
label: In-app help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6208b71b-37a7-40f5-91b0-19b665e7458a
ms.localizationpriority: medium
ms.openlocfilehash: d7144a72dd3af1c9c902e0dfd401799f50ea55b0
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054175"
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

-   **Sé conciso:** Una gran biblioteca de temas de ayuda resulta difícil de usar y no es adecuada para la ayuda en la aplicación.
-   **Sé coherente:** asegúrate de que los usuarios puedan llegar a las páginas de ayuda de la misma forma desde cualquier parte de la aplicación. Nunca deberían buscar la ayuda.
-   **Los usuarios ojean, no leen:** dado que la ayuda que un usuario está buscando puede estar en la misma página que otros temas de ayuda, asegúrate de que puedan distinguir fácilmente la que necesitan.


#### <a name="popups"></a>Elementos emergentes

Los elementos emergentes permiten ofrecer una ayuda muy contextual, es decir, mostrar las instrucciones y recomendaciones relevantes para la tarea específica que el usuario intenta realizar.

-   **Céntrate en un problema:** el espacio está aún más restringido en un elemento emergente que una página de ayuda. Los elementos emergentes de ayuda deben hacer referencia de manera específica a una sola tarea para resultar eficaces.
-   **La visibilidad es importante:** los elementos emergentes solo pueden verse desde una ubicación, por lo que debes asegurarte de que sean claramente visibles para el usuario sin obstaculizar. Si no lo ve claramente, es posible que el usuario abandone el elemento emergente en busca de una página de ayuda.
-   **No uses demasiados recursos:** la ayuda no debe retrasarse ni cargarse lentamente. Si incluyes vídeos o archivos de audio de alta resolución en los elementos emergentes, es más probable que frustres al usuario en lugar de ayudarlo.

#### <a name="descriptions"></a>Descripciones

En ocasiones, puede ser útil proporcionar más información acerca de una característica cuando un usuario la esté inspeccionando. Las descripciones son similares a la interfaz de usuario informativa, pero la principal diferencia entre ellas reside en que la segunda intenta enseñar y ofrecer indicaciones al usuario sobre características que no conocen, mientras que una descripción detallada mejora la comprensión sobre características de aplicaciones en las que el usuario ya está interesado.

-   **No enseñes los conceptos básicos:** da por hecho que el usuario ya conoce los conceptos básicos sobre el uso del elemento que estás describiendo. Aclarar u ofrecer más información resulta útil; decirle lo que ya conoce, no.
-   **Describe interacciones interesantes:** uno de los usos recomendados de las descripciones es enseñar al usuario el modo en que interactúan las características que ya conocen. De este modo, obtendrá más información sobre algo que ya le gusta usar.
-   **Hazte a un lado:** del mismo modo que la interfaz de usuario informativa, las descripciones no deben interferir con la experiencia de un usuario en la aplicación.

## <a name="related-articles"></a>Artículos relacionados

* [Directrices para la ayuda de la aplicación](guidelines-for-app-help.md)
