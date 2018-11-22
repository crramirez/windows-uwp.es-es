---
author: Jwmsft
Description: The hub control uses a hierarchical navigation pattern to support apps with a relational information architecture.
title: Controles de navegación centralizada
ms.assetid: F1319960-63C6-4A8B-8DA1-451D59A01AC2
label: Hub
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3d73ff59b03f288227b1435b0b931d11860259ec
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2018
ms.locfileid: "7580665"
---
# <a name="hub-controlpattern"></a>Patrón o control de navegación centralizada

 


Un control de navegación centralizada permite organizar el contenido de la aplicación en secciones o categorías distintas, aunque relacionadas. Las secciones de un control de navegación centralizada están pensadas para que se recorran en un orden preferido y pueden servir como punto de partida para experiencias más detalladas.

> **API importantes**: [Clase Hub](https://msdn.microsoft.com/library/windows/apps/dn251843), [Clase HubSection](https://msdn.microsoft.com/library/windows/apps/dn251845)

![Ejemplo de un control de navegación centralizada](images/hub_example_tablet.png)

El contenido en un control de navegación centralizada se puede mostrar en una vista panorámica que permite a los usuarios obtener una vista rápida de lo que es nuevo, lo que está disponible y lo que es relevante. Los hubs suelen tener un encabezado de página y cada sección de contenido, un encabezado de sección.


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

El control de hub funciona bien para mostrar grandes cantidades de contenido organizado en una jerarquía. Los concentradores dan prioridad a la exploración y detección de contenido nuevo, lo que los hace útiles para mostrar elementos en una tienda o una colección de elementos multimedia.

El control de navegación centralizada tiene varias funciones que hacen que funcione bien para crear un patrón de navegación de contenido.

-   **Navegación visual**

    Un control de navegación centralizada permite que el contenido se muestre en una matriz diversa, breve y fácil de examinar.

-   **Categorización**

    Cada sección de navegación centralizada permite organizar el contenido en un orden lógico.

-   **Tipos de contenido mixto**

    Con los tipos de contenido mixto, son comunes los tamaños y las relaciones de activos variables. Un control de navegación centralizada permite que cada tipo de contenido se ordene de forma exclusiva y clara en cada sección del control de navegación centralizada.

-   **Anchos de página y de contenido variables**

    Dado que es un modelo panorámico, el control de navegación centralizada permite variabilidad en sus anchos de sección. Esto es estupendo para contenido de distintas profundidades o cantidades.

-   **Arquitectura flexible**

    Si prefieres mantener la arquitectura de la aplicación superficial, puedes tener todo el contenido del canal en un resumen de sección del control de navegación centralizada.

La navegación centralizada es solo uno de varios elementos de navegación que puedes usar. Para más información sobre patrones de navegación y otros elementos de navegación, consulta el artículo [Conceptos básicos del diseño de navegación para aplicaciones para la Plataforma universal de Windows (UWP)](../basics/navigation-basics.md).

## <a name="hub-architecture"></a>Arquitectura de navegación centralizada

El control de navegación centralizada tiene un patrón de navegación jerárquico que admite aplicaciones con una arquitectura de información relacional. Un control de navegación centralizada consta de distintas categorías de contenido, cada una de las cuales se asigna a las páginas de sección de la aplicación. Las páginas de sección se pueden mostrar de la manera que mejor represente el escenario y el contenido que incluye la sección.

![estructura reticular de la aplicación jerárquica Comida con amigos](images/navigation_diagram_food_with_friends_app_new.png)

## <a name="layouts-and-panningscrolling"></a>Diseños y desplazamiento y movimiento panorámico

Hay varias maneras de diseñar y navegar por el contenido en un control de navegación centralizada; simplemente asegúrate de que las listas de contenido de un control de navegación centralizada siempre se desplazan de forma lateral en una dirección perpendicular a la dirección en la que se desplaza el control.

**Movimiento panorámico horizontal**

![Ejemplo de un control con movimiento panorámico horizontal](images/controls_hub_horizontal_pan.png)
**Movimiento panorámico vertical**

![Ejemplo de un control con movimiento panorámico vertical](images/controls_hub_vertical_pan.png)
**Movimiento panorámico horizontal con lista/cuadrícula de desplazamiento vertical**

![Ejemplo de un control con movimiento panorámico horizontal con una lista de desplazamiento vertical](images/controls_hub_horizontal_vertical_scroll.png)
**Movimiento panorámico vertical con lista/cuadrícula de desplazamiento horizontal**

![Ejemplo de navegación centralizada con movimiento panorámico horizontal](images/controls_hub_vertical_horizontal_scroll.png)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Hub">abrir la aplicación y ver la navegación centralizada en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

El control de navegación centralizada proporciona flexibilidad de diseño. Esto te permite diseñar aplicaciones que ofrecen una amplia variedad de experiencias atractivas y visualmente enriquecidas. Puedes usar una imagen o sección de contenido principal para el primer grupo; una imagen grande de la imagen principal se puede recortar vertical y horizontalmente sin perder el centro de interés. El siguiente ejemplo muestra de qué manera se puede recortar una imagen principal usada como prototipo, para obtener un ancho horizontal, vertical o estrecho.

![imagen principal recortada para distintos tamaños de ventana](images/hub_hero_cropped2.png)

En los dispositivos móviles, las secciones de navegación centralizada están visibles de una en una.

![Ejemplo de un patrón de navegación centralizada en una pantalla pequeña](images/phone_hub_example.png)

## <a name="recommendations"></a>Recomendaciones

-   Para que los usuarios sepan que hay más contenido en una sección del control de navegación centralizada, te recomendamos que recortes el contenido para que se vea parte de él.
-   En función de las necesidades de la aplicación, puedes añadir varias secciones de navegación centralizada al control de navegación centralizada, donde cada una de ellas ofrecerá su propio propósito funcional. Por ejemplo, una sección podría contener una serie de vínculos y controles, mientras que otra sección podría ser un repositorio de imágenes en miniatura. Un usuario puede desplazarse entre estas secciones usando la compatibilidad con gestos integrada en el control de navegación centralizada.
-   La mejor forma de acomodar distintos tamaños de ventana es que el contenido se redistribuya de manera dinámica.
-   Si tienes muchas secciones del control de navegación centralizada, considera la posibilidad de agregar zoom semántico. Esto también facilita la búsqueda de secciones cuando la aplicación cambia de tamaño a un ancho estrecho.
-   Te recomendamos que no tengas un elemento en una sección del control de navegación centralizada que lleve a otro control; en su lugar, puedes usar encabezados interactivos para navegar a otra sección o página del control de navegación centralizada.
-   El control de navegación centralizada es un punto de partida y está pensado para que se personalice y se ajuste a las necesidades de la aplicación. Puedes cambiar los siguientes aspectos de un control de navegación centralizada:
    -   Número de secciones
    -   Tipo de contenido en cada sección
    -   Ubicación y orden de secciones
    -   Tamaño de secciones
    -   Espaciado entre secciones
    -   Espaciado entre una sección y la parte superior o inferior del concentrador
    -   Tamaño y estilo de texto en encabezados y contenido
    -   Color de fondo, secciones, encabezados de secciones y contenido de secciones

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase Hub](https://msdn.microsoft.com/library/windows/apps/dn251843)
- [Conceptos básicos de navegación](../basics/navigation-basics.md)
- [Usar un control de navegación centralizada](https://msdn.microsoft.com/library/windows/apps/xaml/dn308518)
- [Ejemplo de control de navegación centralizada XAML](http://go.microsoft.com/fwlink/p/?LinkID=310072)
