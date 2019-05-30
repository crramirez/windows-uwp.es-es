---
Description: La navegación en las aplicaciones para la Plataforma universal de Windows (UWP) se basa en un modelo flexible de estructuras de navegación, elementos de navegación y características de nivel del sistema.
title: Conceptos básicos de navegación para las aplicaciones para UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1c764eeb57ec8046a93e7fb58e156fa68daea8df
ms.sourcegitcommit: 13fe5d04bdb43c75d0fc4de18c2c3d4ae58ff982
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "64564524"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Conceptos básicos del diseño de navegación para las aplicaciones para UWP

![Encabezado de conceptos básicos de navegación](images/nav/navigation-basics-header.jpg)

Si se piensa en una aplicación como si fuera una colección de páginas, el término *navegación* describe la acción de moverse entre las páginas y dentro de una página. Es el punto de partida de la experiencia del usuario, y es el modo en el que los usuarios encuentran el contenido y las características que les interesan. Es muy importante y puede ser difícil conseguir que sea la correcta.

Contamos con un número elevado de opciones entre las que elegir para la navegación. Podríamos:

:::row:::
    :::column:::
        ![navigation example 1](images/nav/nav-1.svg)

Los usuarios deben pasar por una serie de páginas en orden.
    :::column-end:::
    :::column:::
        ![navigation example 2](images/nav/nav-2.svg)

Proporcionar un menú que permite a los usuarios saltar directamente a cualquier página.
    :::column-end:::
    :::column:::
        ![navigation example 3](images/nav/nav-3.svg)

Todo lo que coloque en una sola página y proporcionar mecanismos de filtrado para ver el contenido.
    :::column-end:::
:::row-end:::

Aunque no hay ningún diseño de navegación único que funcione para todas las aplicaciones, existen principios y directrices para ayudarte a decidir el diseño adecuado para tu aplicación.

## <a name="principles-of-good-navigation"></a>Principios de buena navegación

Empecemos con los principios básicos de diseño de buena navegación:

- **Coherencia:** Cumplir las expectativas del usuario.
- **Simplicidad:** No más de lo necesario.
- **Mayor claridad:** Proporcionar opciones y trayectorias claras.

### <a name="consistency"></a>Consistency

La navegación debería ser coherente con las expectativas del usuario. Uso de [controles estándar](#use-the-right-controls) que los usuarios siguientes y está familiarizadas con convenciones estándar para los iconos, ubicación, y estilo hará navegación predecible e intuitiva para los usuarios.

![imagen de componentes de la página](images/nav/page-components.svg)

> Los usuarios esperan encontrar determinados elementos de interfaz de usuario en las ubicaciones estándar.

### <a name="simplicity"></a>Simplicidad

Una menor cantidad de elementos de navegación simplifica la toma de decisiones para los usuarios. Ofrecer acceso sencillo a destinos importantes y ocultar los elementos menos importantes ayudará a los usuarios a ir donde quieran, con mayor rapidez.

:::row:::
    :::column:::
        ![do example](images/nav/do.svg)

        ![navview good](images/nav/navview-good.svg)

Presentar los elementos de navegación en un menú de navegación conocida.
    :::column-end:::
    :::column:::
        ![don't example](images/nav/dont.svg)

        ![navview bad](images/nav/navview-bad.svg)

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

2. Evite las jerarquías de exploración profunda. Si vas más allá de tres niveles de navegación, existe el riesgo de hacer encallar al usuario en una jerarquía profunda de la que tenga dificultades para salir.

3. Evita el "pogo-sticking". El "pogo-sticking" se produce cuando hay contenido relacionado, pero navegar hasta él requiere que el usuario suba un nivel y después vuelva a bajar.

## <a name="use-the-right-structure"></a>Uso de la estructura correcta

Ahora que ya conoces los principios generales de navegación, ¿cómo deberías estructurar la aplicación? Hay dos estructuras generales: plana y jerárquica.

:::row:::
    :::column:::
        ![Pages arranged in a flat structure](images/nav/flat-lateral-structure.svg)
    :::column-end:::
    :::column span="2":::
        ### Flat/lateral

En una estructura plana o lateral, las páginas existen en paralelo. Puedes ir de una página a otra en cualquier orden.

Te recomendamos que uses una estructura plana en los siguientes casos:

- las páginas se pueden ver en cualquier orden;
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

En una estructura jerárquica, las páginas se organizan en una estructura parecida a un árbol. Cada página secundaria tiene un solo elemento primario, pero un elemento primario puede tener una o más páginas secundarias. Para llegar a una página secundaria, hay que moverse a través del elemento primario.

Las estructuras jerárquicas sirven para organizar un contenido complejo que ocupe una gran cantidad de páginas. La desventaja es que se produce cierta sobrecarga de navegación: cuanto más profunda es la estructura, más clics se necesitan para que los usuarios vayan de una página otra.

Se recomienda jerárquica estructura cuando:
        
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

No se opta por una estructura o la otra; muchas aplicaciones de diseño también usan ambos. Una aplicación puede usar estructuras planas para las páginas de nivel superior, que pueden verse en cualquier orden, y estructuras jerárquicas para las páginas que tienen relaciones más complejas.

Si la estructura de navegación tiene varios niveles, te recomendamos que los elementos de navegación punto a punto solo se vinculen con los elementos del mismo nivel dentro de su subárbol. Tenga en cuenta la ilustración adyacente, que muestra una estructura de navegación que tiene dos niveles:

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

Con algunas excepciones, cualquier aplicación que tiene varias páginas usa un marco. Por lo general, una aplicación tiene una página principal que contiene el marco y un elemento de navegación primario, como un control de vista de navegación. Cuando el usuario selecciona una página, el marco la carga y la muestra.
:::row-end:::

:::row:::
    :::column:::
        ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
    :::column span="2":::
        [**Top navigation and tabs**](../controls-and-patterns/navigationview.md)

Muestra una lista horizontal de vínculos a páginas del mismo nivel. El [NavigationView](../controls-and-patterns/navigationview.md) control implementa el panel de navegación superior y patrones de pestañas.
        
Usar la navegación superior cuando:

- Desea mostrar todas las opciones de navegación en la pantalla.
- Desean más espacio para el contenido de la aplicación.
- Los iconos no pueden describir claramente las categorías de exploración.
        
Use las pestañas cuando:

- Desea conservar el estado de página y el historial de navegación.
- Se espera que los usuarios cambiar entre pestañas con frecuencia.

:::row-end:::

:::row:::
    :::column:::
         ![tabs and pivot image](images/nav/thumbnail-tabs-pivot.svg)
    :::column-end:::
        :::column span="2":::
    [**Pivot**](../controls-and-patterns/pivot.md)
    
Similar a [vista de navegación](../controls-and-patterns/navigationview.md), pero con compatibilidad adicional para tecnología táctil y el comportamiento de navegación ligeramente diferente.
    
Usar una tabla dinámica cuando:
- Quiere que su aplicación para permitir la interacción de deslizar rápidamente entre las categorías
- Desea que las opciones de navegación en carrusel infintely
- No es necesario un amplio control sobre el comportamiento de navegación entre las categorías

:::row-end:::

:::row:::
    :::column:::
        ![navview image](images/nav/thumbnail-navview.svg)
    :::column-end:::
    :::column span="2":::
        [**Left navigation**](../controls-and-patterns/navigationview.md)

Muestra una lista vertical de vínculos a páginas de nivel superior. Úsalo en estos casos:
        
- Las páginas existan en el nivel superior.
- Hay muchos elementos de navegación (más de 5)
- No se espera que los usuarios cambien entre las páginas con frecuencia.

:::row-end:::
        
:::row:::
    :::column:::
        ![Master details image](images/nav/thumbnail-master-detail.svg)
    :::column-end:::
    :::column span="2":::
        [**Master/details**](../controls-and-patterns/master-details.md)

Muestra una lista (vista maestra) de elementos. Al seleccionar un elemento, se muestra su página correspondiente en la sección de detalles. Úsalo en estos casos:
        
- no se espera que los usuarios cambien entre elementos secundarios con frecuencia;
- si desea permitir al usuario realizar operaciones de alto nivel, como eliminar u ordenar, en elementos individuales o grupos de elementos, y también quieres que el usuario pueda ver o actualizar los detalles de cada elemento.

El patrón de maestro y detalles es ideal para las bandejas de entrada de correo electrónico, las listas de contactos y la entrada de datos.
:::row-end:::

:::row:::
    :::column:::
        ![Hyperlinks and buttons image](images/nav/thumbnail-hyperlinks-buttons.svg)
    :::column-end:::
    :::column span="2":::
        [**Hyperlinks**](../controls-and-patterns/hyperlinks.md)

En el contenido de una página pueden aparecer elementos de navegación incrustados. A diferencia de otros elementos de navegación, que deben ser coherentes en todas las páginas, los elementos de navegación incrustados de contenido son exclusivos en cada página.
:::row-end:::

## <a name="next-add-navigation-code-to-your-app"></a>Siguiente: Agregue el código de navegación a la aplicación

En el siguiente artículo, [Implementación de la navegación básica](navigate-between-two-pages.md), se muestra el código necesario para usar un control Frame para habilitar en tu aplicación la navegación básica entre dos páginas.
