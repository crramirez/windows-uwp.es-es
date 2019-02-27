---
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Conceptos básicos de navegación para las aplicaciones para UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: bba6d42e6ce494c0655c6b6c60db91c1e2e006e8
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116247"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Conceptos básicos del diseño de navegación para las aplicaciones para UWP

![Encabezado de conceptos básicos de navegación](images/nav/navigation-basics-header.jpg)

Si se piensa en una aplicación como si fuera una colección de páginas, el término *navegación* describe la acción de moverse entre las páginas y dentro de una página. Es el punto de partida de la experiencia del usuario, y es el modo en el que los usuarios encuentran el contenido y las características que les interesan. Es muy importante y puede ser difícil conseguir que sea la correcta.

Contamos con un número elevado de opciones entre las que elegir para la navegación. Podríamos:

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

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

La navegación debería ser coherente con las expectativas del usuario. Con [los controles estándar](#use-the-right-controls) que los usuarios están familiarizados con y siguientes convenciones estándares para los iconos, ubicación y estilos hará navegación intuitiva y predecible para los usuarios.

![imagen de componentes de la página](images/nav/page-components.svg)

> Los usuarios esperan encontrar determinados elementos de interfaz de usuario en las ubicaciones estándar.

### <a name="simplicity"></a>Simplicidad

Una menor cantidad de elementos de navegación simplifica la toma de decisiones para los usuarios. Ofrecer acceso sencillo a destinos importantes y ocultar los elementos menos importantes ayudará a los usuarios a ir donde quieran, con mayor rapidez.

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

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
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

        In a flat/lateral structure, pages exist side-by-side. You can go from one page to another in any order.

        We recommend using a flat structure when:

        - Las páginas se pueden ver en cualquier orden.
        - las páginas son claramente distintas entre sí y no tienen una relación primaria-secundaria obvia.
        - Hay menos de 8 páginas en el grupo. <br>
        (Si hay más páginas, a los usuarios les puede resultar difícil comprender el modo en que las páginas son únicas o entender su ubicación actual dentro del grupo. Si no crees que eso es un problema para tu aplicación, organiza las páginas como elementos del mismo nivel. De lo contrario, piensa en la posibilidad de usar una estructura jerárquica para separar las páginas en dos o más grupos pequeños).

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pages arranged in a hierarchy](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Hierarchical

        In a hierarchical structure, pages are organized into a tree-like structure. Each child page has one parent, but a parent can have one or more child pages. To reach a child page, you travel through the parent.

        Hierarchical structures are good for organizing complex content that spans lots of pages. The downside is some navigation overhead: the deeper the structure, the more clicks it takes to get from page to page.

        We recommend a hierarchical structure when:
        
        - Las páginas deben recorrerse en un orden específico.
        - Hay una relación primaria-secundaria clara entre las páginas.
        - Hay más de 7 páginas en el grupo.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![an app with a hybrid structure](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### Combining structures

        You don't have choose to one structure or the other; many well-design apps use both. An app can use flat structures for top-level pages that can be viewed in any order, and hierarchical structures for pages that have more complex relationships.

        If your navigation structure has multiple levels, we recommend that peer-to-peer navigation elements only link to the peers within their current subtree. Consider the adjacent illustration, which shows a navigation structure that has two levels:

        - En el nivel 1, el elemento de navegación punto a punto debe proporcionar acceso a las páginas A, B, C y D.
        - En el nivel 2, los elementos de navegación punto a punto de las páginas A2 solo deben vincularse a otras páginas A2. No se deben vincular a páginas de nivel 2 del subárbol C.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Uso de los controles correctos

Cuando hayas decidido la estructura de las páginas, tendrás que decidir cómo navegarán los usuarios a través de esas páginas. La UWP proporciona una variedad de controles de navegación para ayudar a garantizar una experiencia de navegación coherente y fiable en la aplicación.

:::row:::
    :::column:::
        ![Frame image](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)

        With few exceptions, any app that has multiple pages uses a frame. Typically, an app has a main page that contains the frame and a primary navigation element, such as a navigation view control. When the user selects a page, the frame loads and displays it.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

        Displays a horizontal list of links to pages at the same level. The [NavigationView](../controls-and-patterns/navigationview.md) control implements the top navigation and tabs patterns.
        
        Use top navigation when:

        - Quieres mostrar todas las opciones de exploración en la pantalla.
        - Desean más espacio para el contenido de la aplicación.
        - Iconos claramente no describen las categorías de navegación.
        
        Usa pestañas cuando:

        - Desea conservar el estado de página y el historial de navegación.
        - Espera que los usuarios cambien entre las pestañas con frecuencia.

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

        Displays a vertical list of links to top-level pages. Use when:
        
        - Las páginas existen en el nivel superior.
        - Hay muchos elementos de navegación (más de 5)
        - No se espera que los usuarios cambien entre las páginas con frecuencia.
        
:::row-end:::

:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

        Displays a list (master view) of items. Selecting an item displays its corresponding page in the details section. Use when:
        
        - Se espera que los usuarios cambien entre elementos secundarios con frecuencia;
        - si desea permitir al usuario realizar operaciones de alto nivel, como eliminar u ordenar, en elementos individuales o grupos de elementos, y también quieres que el usuario pueda ver o actualizar los detalles de cada elemento.

        El patrón de maestro y detalles es ideal para las bandejas de entrada de correo electrónico, las listas de contactos y la entrada de datos.
:::row-end:::

:::row:::
    :::column:::
        ![Hyperlinks and buttons image](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

        Embedded navigation elements can appear in a page's content. Unlike other navigation elements, which should be consistent across the pages, content-embedded navigation elements are unique from page to page.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>A continuación: agrega código de navegación a la aplicación

En el siguiente artículo, [Implementación de la navegación básica](navigate-between-two-pages.md), se muestra el código necesario para usar un control Frame para habilitar en tu aplicación la navegación básica entre dos páginas.
