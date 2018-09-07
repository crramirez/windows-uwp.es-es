---
author: QuinnRadich
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Conceptos básicos de navegación para las aplicaciones para UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: quradic
ms.date: 7/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b731910f53a6152554b74e946374234b827f4a86
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "3417663"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Conceptos básicos del diseño de navegación para las aplicaciones para UWP

![Encabezado de conceptos básicos de navegación](images/nav/navigation-basics-header.jpg)

Si se piensa en una aplicación como si fuera una colección de páginas, el término *navegación* describe la acción de moverse entre las páginas y dentro de una página. Es el punto de partida de la experiencia del usuario, y es el modo en el que los usuarios encuentran el contenido y las características que les interesan. Es muy importante y puede ser difícil conseguir que sea la correcta.

Contamos con un número elevado de opciones entre las que elegir para la navegación. Podríamos:

:::row:::
    :::column:::
        ![ejemplo de navegación 1](images/nav/nav-1.svg)

        Require users to go through a series of pages in order.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

        Provide a menu that allows users to jump directly to any page.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

        Place everything on a single page and provide filtering mechanisms for viewing content.
    :::column-end:::
:::row-end:::

Aunque no hay ningún diseño de navegación único que funcione para todas las aplicaciones, existen principios y directrices para ayudarte a decidir el diseño adecuado para tu aplicación.

## <a name="principles-of-good-navigation"></a>Principios de buena navegación

Empecemos con los principios básicos de diseño de buena navegación:

- **Coherencia**: satisface las expectativas del usuario.
- **Simplicidad**: no hagas más de lo que necesites hacer.
- **Claridad**: proporciona opciones y rutas claras.

### <a name="consistency"></a>Coherencia

La navegación debería ser coherente con las expectativas del usuario. Con [los controles estándar](#use-the-right-controls) que los usuarios están familiarizados con y siguientes convenciones estándares para los iconos, ubicación y el estilo hará navegación intuitiva y predecible para los usuarios.

![imagen de componentes de la página](images/nav/page-components.svg)

> Los usuarios esperan encontrar determinados elementos de interfaz de usuario en las ubicaciones estándar.

### <a name="simplicity"></a>Simplicidad

Una menor cantidad de elementos de navegación simplifica la toma de decisiones para los usuarios. Ofrecer acceso sencillo a destinos importantes y ocultar los elementos menos importantes ayudará a los usuarios a ir donde quieran, con mayor rapidez.

:::row:::
    :::column:::
        ![ejemplo de cosas para hacer](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

        Present navigation items in a familiar navigation menu.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

        Overwhelm users with many navigation options.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Claridad

Las rutas de acceso claras permiten la navegación lógica para los usuarios. Realizar opciones de navegación obvias y aclarar las relaciones entre páginas debe evitar que se pierdan los usuarios.

![ejemplo de cosas para hacer](images/nav/clarity-image.svg)

> Los destinos están etiquetados con claridad para que los usuarios sepan dónde se encuentran.

## <a name="general-recommendations"></a>Recomendaciones generales

Ahora vamos a tomar nuestros principios de diseño, coherencia, simplicidad y claridad, y los vamos a usar para elaborar algunas recomendaciones generales.

1. Piensa en tus usuarios. Traza rutas habituales que podrían tomar en la aplicación, y en cada página, piensa en por qué está ahí el usuario y dónde podría querer ir.

2. Evita las jerarquías de navegación detallado. Si vas más allá de tres niveles de navegación, existe el riesgo de hacer encallar al usuario en una jerarquía profunda de la que tenga dificultades para salir.

3. Evita el "pogo-sticking". El "pogo-sticking" se produce cuando hay contenido relacionado, pero navegar hasta él requiere que el usuario suba un nivel y después vuelva a bajar.

## <a name="use-the-right-structure"></a>Uso de la estructura correcta

Ahora que ya conoces los principios generales de navegación, ¿cómo deberías estructurar la aplicación? Hay dos estructuras generales: plana y jerárquica.

:::row:::
    :::column:::
        ![Páginas organizadas en una estructura plana](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    ::: column span = "2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from one page to another in any order.

        We recommend using a flat structure when:

        - The pages can be viewed in any order.
        - The pages are clearly distinct from each other and don't have an obvious parent/child relationship.
        - There are less than 8 pages in the group. <br>
        (When there are more pages, it might be difficult for users to understand how the pages are unique or to understand their current location within the group. If you don't think that's an issue for your app, go ahead and make the pages peers. Otherwise, consider using a hierarchical structure to break the pages into two or more smaller groups.)

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Páginas organizadas en una jerarquía](images/nav/hierarchical-structure.svg)
    :::column-end:::
    ::: column span = "2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page.

        We recommend a hierarchical structure when:
        
        - Pages should be traversed in a specific order.
        - There is a clear parent-child relationship between pages.
        - There are more than 7 pages in the group.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![una aplicación con una estructura híbrida](images/nav/combining-structures.svg)
    :::column-end:::
    ::: column span = "2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - At level 1, the peer-to-peer navigation element should provide access to pages A, B, C, and D.
        - At level 2, the peer-to-peer navigation elements for the A2 pages should only link to the other A2 pages. They should not link to level 2 pages in the C subtree.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Uso de los controles correctos

Cuando hayas decidido la estructura de las páginas, tendrás que decidir cómo navegarán los usuarios a través de esas páginas. La UWP proporciona una variedad de controles de navegación para ayudar a garantizar una experiencia de navegación coherente y fiable en la aplicación.

:::row:::
    :::column:::
        ![Imagen de trama](images/nav/thumbnail-frame.svg)
    :::column-end:::
    ::: column span = "2"::: [ **fotograma**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row:::
    :::column:::
        ![imagen de pestañas y](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    ::: column span = "2"::: [ **pestañas y navegación superior**](../controls-and-patterns/navigationview.md)

        Displays a horizontal list of links to pages at the same level. The [NavigationView](../controls-and-patterns/navigationview.md) control implements the top navigation and tabs patterns.
        
        Use top navigation when:

        - You want to show all navigation options on the screen.
        - You desire more space for your app's content.
        - Icons cannot clearly describe your navigation categories.
        
        Use tabs when:

        - You want to preserve navigation history and page state.
        - You expect users to switch between tabs frequently.

:::row-end:::

:::row:::
    :::column:::
        ![imagen de navview](images/nav/thumbnail-navview.svg)
    :::column-end:::
    ::: column span = "2"::: [ **navegación izquierda**](../controls-and-patterns/navigationview.md)

        Displays a vertical list of links to top-level pages. Use when:
        
        - The pages exist at the top level.
        - There are many navigation items (more than 5)
        - You don't expect users to switch between pages frequently.
        
:::row-end:::

:::row:::
    :::column:::
        ![Imagen de maestro y detalles](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    ::: column span = "2"::: [ **maestro y detalles**](../controls-and-patterns/master-details.md)

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        
        - You expect users to switch between child items frequently.
        - You want to enable the user to perform high-level operations, such as deleting or sorting, on individual items or groups of items, and also want to enable the user to view or update the details for each item.

        Master/details is well suited for email inboxes, contact lists, and data entry.
:::row-end:::

:::row:::
    :::column:::
        ![Imagen de hipervínculos y botones](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    ::: column span = "2"::: [ **hipervínculos**](../controls-and-patterns/hyperlinks.md)

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>A continuación: agrega código de navegación a la aplicación

En el siguiente artículo, [Implementación de la navegación básica](navigate-between-two-pages.md), se muestra el código necesario para usar un control Frame para habilitar en tu aplicación la navegación básica entre dos páginas.