---
Description: La navegación en las aplicaciones de Windows se basa en un modelo flexible de estructuras de navegación, elementos de navegación y características de nivel del sistema.
title: Conceptos básicos de navegación para aplicaciones de Windows
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6fa0f8997db9a383097c7e0fece3d8a17d5875ab
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220098"
---
# <a name="navigation-design-basics-for-windows-apps"></a>Conceptos básicos de diseño de navegación para aplicaciones de Windows

![Encabezado de conceptos básicos de navegación](images/nav/navigation-basics-header.jpg)

Si se piensa en una aplicación como si fuera una colección de páginas, el término *navegación* describe la acción de moverse entre las páginas y dentro de una página. Se trata del punto de partida de la experiencia del usuario, además del modo en el que los usuarios encuentran el contenido y las características que les interesan. Es muy importante y puede ser algo difícil de conseguir.

Contamos con un número elevado de opciones entre las que elegir para la navegación. Podríamos:

:::row:::
    :::column:::
        ![ejemplo de navegación 1](images/nav/nav-1.svg)

Exigir que el usuario pase por una serie de páginas en orden.
    :::column-end:::
    :::column:::
        ![ejemplo de navegación 2](images/nav/nav-2.svg)

Ofrecer un menú que permitiera a los usuarios saltar directamente a cualquier página.
    :::column-end:::
    :::column:::
        ![ejemplo de navegación 3](images/nav/nav-3.svg)

Situarlo todo en una sola página y facilitar mecanismos de filtrado para ver el contenido.
    :::column-end:::
:::row-end:::

Aunque no hay ningún diseño de navegación único que funcione para todas las aplicaciones, existen principios y directrices para ayudarte a decidir el diseño adecuado para tu aplicación.

## <a name="principles-of-good-navigation"></a>Principios de buena navegación

Empecemos con los principios básicos de diseño de buena navegación:

- **Coherencia:** Cumplir las expectativas del usuario.
- **Simplicidad:** No hagas más de lo que necesites hacer.
- **Claridad:** Ofrece rutas de acceso y opciones claras.

### <a name="consistency"></a>Consistency

La navegación debería ser coherente con las expectativas del usuario. El uso de [controles estándar](#use-the-right-controls) que los usuarios conozcan y de las siguientes convenciones estándar para iconos, ubicación y estilo hará que la navegación sea predecible e intuitiva para los usuarios.

![imagen de componentes de la página](images/nav/page-components.svg)

> Los usuarios esperan encontrar determinados elementos de interfaz de usuario en las ubicaciones estándar.

### <a name="simplicity"></a>Simplicidad

Una menor cantidad de elementos de navegación simplifica la toma de decisiones para los usuarios. Ofrecer acceso sencillo a destinos importantes y ocultar los elementos menos importantes ayudará a los usuarios a ir donde quieran, con mayor rapidez.

:::row:::
    :::column:::
        ![ejemplo de cosas para hacer](images/nav/do.svg)

        ![vista de navegación correcta](images/nav/navview-good.svg)

Presentar los elementos de navegación en un menú de navegación conocido.
    :::column-end:::
    :::column:::
        ![ejemplo de cosas que evitar](images/nav/dont.svg)

        ![vista de navegación incorrecta](images/nav/navview-bad.svg)

Abrumar a los usuarios con muchas opciones de navegación.
    :::column-end:::
:::row-end:::

### <a name="clarity"></a>Claridad

Las rutas de acceso claras permiten la navegación lógica para los usuarios. Realizar opciones de navegación obvias y aclarar las relaciones entre páginas debe evitar que se pierdan los usuarios.

![ejemplo de cosas para hacer](images/nav/clarity-image.svg)

> Los destinos están etiquetados con claridad para que los usuarios sepan dónde se encuentran.

## <a name="general-recommendations"></a>Recomendaciones generales

Ahora vamos a tomar nuestros principios de diseño, coherencia, simplicidad y claridad, y los vamos a usar para elaborar algunas recomendaciones generales.

1. Piensa en tus usuarios. Traza rutas habituales que podrían tomar en la aplicación, y en cada página, piensa en por qué está ahí el usuario y dónde podría querer ir.

2. Evita las jerarquías de navegación profundas. Si vas más allá de tres niveles de navegación, existe el riesgo de hacer encallar al usuario en una jerarquía profunda de la que tenga dificultades para salir.

3. Evita el "pogo-sticking". El "pogo-sticking" se produce cuando hay contenido relacionado, pero navegar hasta él requiere que el usuario suba un nivel y después vuelva a bajar.

## <a name="use-the-right-structure"></a>Uso de la estructura correcta

Ahora que ya conoces los principios generales de navegación, ¿cómo deberías estructurar la aplicación? Hay dos estructuras generales: plana y jerárquica.

:::row:::
    :::column:::
        ![Páginas organizadas en una estructura plana](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="flatlateral"></a>Plana o lateral

En una estructura plana o lateral, las páginas existen en paralelo. Puedes ir de una página a otra en cualquier orden.

Te recomendamos que uses una estructura plana en los siguientes casos:

- las páginas se pueden ver en cualquier orden;
- las páginas son claramente distintas entre sí y no tienen una relación primaria-secundaria obvia.
- Hay menos de 8 páginas en el grupo. <br>
(Si hay más páginas, a los usuarios les puede resultar difícil comprender el modo en que las páginas son únicas o entender su ubicación actual dentro del grupo. Si no crees que eso es un problema para tu aplicación, organiza las páginas como elementos del mismo nivel. Por otro lado, puedes tener en cuenta la posibilidad de usar una estructura jerárquica para separar las páginas en dos o más grupos pequeños.

    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Páginas organizadas en una jerarquía](images/nav/hierarchical-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="hierarchical"></a>Jerárquica

En una estructura jerárquica, las páginas se organizan en una estructura parecida a un árbol. Cada página secundaria tiene un solo elemento primario, pero un elemento primario puede tener una o más páginas secundarias. Para llegar a una página secundaria, hay que moverse a través del elemento primario.

Las estructuras jerárquicas sirven para organizar un contenido complejo que ocupe una gran cantidad de páginas. La desventaja es que se produce cierta sobrecarga de navegación: cuanto más profunda es la estructura, más clics se necesitan para que los usuarios vayan de una página otra.

Te recomendamos una estructura jerárquica en los siguientes casos:
        
- Las páginas deben recorrerse en un orden específico.
- Hay una relación primaria-secundaria clara entre las páginas.
- Hay más de 7 páginas en el grupo.
        
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![una aplicación con una estructura híbrida](images/nav/combining-structures.svg)
    :::column-end:::
    :::column span="2":::
        ### <a name="combining-structures"></a>Combinación de estructuras

No es necesario que elijas entre una estructura y otra, ya que muchas aplicaciones bien diseñadas usan ambas. Una aplicación puede usar estructuras planas para las páginas de nivel superior, que pueden verse en cualquier orden, y estructuras jerárquicas para las páginas que tienen relaciones más complejas.

Si la estructura de navegación tiene varios niveles, te recomendamos que los elementos de navegación punto a punto solo se vinculen con los elementos del mismo nivel dentro de su subárbol. Ten en cuenta la ilustración adyacente, que muestra una estructura de navegación con dos niveles:

- En el nivel 1, el elemento de navegación punto a punto debe proporcionar acceso a las páginas A, B, C y D.
- En el nivel 2, los elementos de navegación punto a punto de las páginas A2 solo deben vincularse a otras páginas A2. No se deben vincular a páginas de nivel 2 del subárbol C.
    :::column-end:::
:::row-end:::

## <a name="use-the-right-controls"></a>Uso de los controles correctos

Cuando hayas decidido la estructura de las páginas, tendrás que decidir cómo navegarán los usuarios a través de esas páginas. UWP ofrece diversos controles de navegación para ayudar a garantizar una experiencia de navegación coherente y fiable en la aplicación.

:::row:::
    :::column:::
        ![Imagen de marco](images/nav/thumbnail-frame.svg)
    :::column-end:::
    :::column span="2":::
        [**Marco**](/uwp/api/Windows.UI.Xaml.Controls.Frame)

Con algunas excepciones, cualquier aplicación que tiene varias páginas usa un marco. Por lo general, una aplicación tiene una página principal que contiene el marco y un elemento de navegación primario, como un control de vista de navegación. Cuando el usuario selecciona una página, el marco la carga y la muestra.
:::row-end:::

:::row:::
    :::column:::
        ![imagen de pestañas y tabla dinámica](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Navegación superior**](../controls-and-patterns/navigationview.md)

Muestra una lista horizontal de vínculos a páginas del mismo nivel. El control [NavigationView](../controls-and-patterns/navigationview.md) implementa el patrón de navegación superior.
        
Usa navegación superior en estos casos:

- Quieres mostrar todas las opciones de navegación en la pantalla.
- Quieres más espacio para el contenido de tu aplicación.
- Los iconos no pueden describir claramente las categorías de exploración.

:::row-end:::

:::row:::
    :::column:::
        ![imagen de pestañas y tabla dinámica](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Pestañas**](../controls-and-patterns/tab-view.md)

Muestra un conjunto horizontal de pestañas y su contenido respectivo. El control [TabView](../controls-and-patterns/tab-view.md) es útil para mostrar varias páginas (o documentos), a la vez que proporciona a los usuarios la capacidad de reorganizar, abrir o cerrar nuevas pestañas.
    
Usa pestañas en estos casos:

- Si quieres que los usuarios puedan abrir, cerrar o reorganizar las pestañas dinámicamente.
- Esperas que haya un gran número de pestañas abiertas a la vez.
- Esperas que los usuarios puedan mover fácilmente las pestañas entre las ventanas de la aplicación que usan pestañas, de manera similar a los exploradores web, como Microsoft Edge.

:::row-end:::

:::row:::
    :::column:::
         ![imagen de pestañas y tabla dinámica](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Tabla dinámica**](../controls-and-patterns/pivot.md)
    
Similar a [Vista de navegación](../controls-and-patterns/navigationview.md), pero con soporte adicional de función táctil y comportamiento de navegación ligeramente diferente.
    
Usa una tabla dinámica en estos casos:
- Quieres que tu aplicación permita el deslizamiento táctil rápido entre categorías
- Quieres que las opciones de navegación se muestren infinitamente en carrusel
- No necesitas un amplio control sobre el comportamiento de navegación entre las categorías

:::row-end:::

:::row:::
    :::column:::
        ![imagen de vista de navegación](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Navegación izquierda**](../controls-and-patterns/navigationview.md)

Muestra una lista vertical de vínculos a páginas de nivel superior. Úsala en estos casos:
        
- Las páginas existan en el nivel superior.
- Existen muchos elementos de navegación (más de 5).
- No se espera que los usuarios cambien entre las páginas con frecuencia.

:::row-end:::
        
:::row:::
    :::column:::
        ![Imagen de detalles de maestro](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Maestro/detalles**](../controls-and-patterns/master-details.md)

Muestra una lista (vista maestra) de elementos. Al seleccionar un elemento, se muestra su página correspondiente en la sección de detalles. Úsala en estos casos:
        
- no se espera que los usuarios cambien entre elementos secundarios con frecuencia;
- si desea permitir al usuario realizar operaciones de alto nivel, como eliminar u ordenar, en elementos individuales o grupos de elementos, y también quieres que el usuario pueda ver o actualizar los detalles de cada elemento.

El patrón de maestro y detalles es ideal para las bandejas de entrada de correo electrónico, las listas de contactos y la entrada de datos.
:::row-end:::

:::row:::
    :::column:::
        ![Imagen de hipervínculos y botones](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hipervínculos**](../controls-and-patterns/hyperlinks.md)

En el contenido de una página pueden aparecer elementos de navegación incrustados. A diferencia de otros elementos de navegación, que deben ser coherentes en todas las páginas, los elementos de navegación de contenido incrustado son exclusivos en cada página.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Siguiente: Agregar código de navegación a la aplicación

En el siguiente artículo, [Implementación de la navegación básica](navigate-between-two-pages.md), se muestra el código necesario para usar un control Frame para habilitar en tu aplicación la navegación básica entre dos páginas.
