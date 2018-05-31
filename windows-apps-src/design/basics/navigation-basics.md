---
author: serenaz
Description: Navigation in Universal Windows Platform (UWP) apps is based on a flexible model of navigation structures, navigation elements, and system-level features.
title: Conceptos básicos de navegación para las aplicaciones para UWP
ms.assetid: B65D33BA-AAFE-434D-B6D5-1A0C49F59664
label: Navigation design basics
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3edb7dc28561b5d316a908df951e3052eafc995b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654274"
---
# <a name="navigation-design-basics-for-uwp-apps"></a>Conceptos básicos del diseño de navegación para las aplicaciones para UWP

Si se piensa en una aplicación como si fuera una colección de páginas, el término *navegación* describe la acción de moverse entre las páginas y dentro de una página. Es el punto de partida de la experiencia del usuario, y es el modo en el que los usuarios encuentran el contenido y las características que les interesan. Es muy importante y puede ser difícil conseguir que sea la correcta.

Es difícil porque, como diseñadores de la aplicación, tenemos un gran número de elecciones que realizar. Podríamos requerir al usuario recorrer una serie de páginas en orden. O podríamos proporcionar un menú que permitiera a los usuarios saltar directamente a cualquier página. También podríamos poner todo en una sola página y proporcionar mecanismos de filtrado para ver el contenido.
 
Si bien no hay ningún diseño de navegación que funcione para todas las aplicaciones, existen una serie de principios y directrices que te ayudarán a decidir el diseño correcto para tu aplicación. 

<figure class="wdg-figure">
  <img alt="Navigation diagram for an app" src="images/navigation_diagram.png" />
  <figcaption>Diagrama de la navegación de una aplicación.</figcaption>
</figure> 

## <a name="principles-of-good-navigation"></a>Principios de una buena navegación
Vamos a empezar con los principios básicos del diseño de una buena navegación: 

* Coherencia: Cumple con las expectativas del usuario.
* Simplicidad: No hagas más de lo que necesites.
* Claridad: Proporciona rutas y opciones claras.

### <a name="consistency"></a>Coherencia
La navegación debería ser coherente con las expectativas del usuario. Si se usan [controles estándar](#use-the-right-controls) que los usuarios conozcan y se siguen convenciones estándar para los iconos, la ubicación, y el estilo, se conseguirá que la navegación resulte intuitiva y predecible para los usuarios.

<figure class="wdg-figure">
<img src="images/nav/nav-component-layout.png" alt="Preferred location for navigation elements"/>
  <figcaption>Los usuarios esperan encontrar determinados elementos de la interfaz de usuario en ubicaciones estándar.</figcaption>
</figure> 

### <a name="simplicity"></a>Simplicidad
Una menor cantidad de elementos de navegación simplifica la toma de decisiones para los usuarios. Si proporcionas un acceso fácil a los destinos importantes y ocultas los elementos menos importantes, ayudarás a los usuarios a ir donde quieran y con mayor rapidez.

<figure class="wdg-figure">
<img src="images/nav/nav-simple-menus.png" alt="A simple versus a complex menu"/>
  <figcaption> El menú de la izquierda será más fácil de comprender y utilizar para los usuarios, porque hay menos elementos.
</figcaption>
</figure> 

### <a name="clarity"></a>Claridad
Unas rutas de acceso claras permiten que los usuarios naveguen de forma lógica. Si se hace que las opciones de navegación sean obvias y se clarifican las relaciones entre las páginas, se debería impedir que los usuarios se pierdan.

<figure class="wdg-figure">
<img src="images/nav/nav-pages.png" alt="Clear paths and destinations"/>
  <figcaption> Los destinos están etiquetados con claridad para que los usuarios sepan dónde se encuentran.
</figcaption>
</figure> 

## <a name="general-recommendations"></a>Recomendaciones generales
Ahora vamos a tomar nuestros principios de diseño, coherencia, simplicidad y claridad, y los vamos a usar para elaborar algunas recomendaciones generales.

1. Piensa en tus usuarios. Traza rutas habituales que podrían tomar en la aplicación, y en cada página, piensa en por qué está ahí el usuario y dónde podría querer ir. 

2. Evita las jerarquías de navegación profundas. Si vas más allá de tres niveles de navegación, existe el riesgo de hacer encallar al usuario en una jerarquía profunda de la que tenga dificultades para salir.

3. Evita el "pogo-sticking". El "pogo-sticking" se produce cuando hay contenido relacionado, pero navegar hasta él requiere que el usuario suba un nivel y después vuelva a bajar.

## <a name="use-the-right-structure"></a>Uso de la estructura correcta 
Ahora que ya conoces los principios generales de navegación, ¿cómo deberías estructurar la aplicación? Hay dos estructuras generales: plana y jerárquica. 

### <a name="flatlateral"></a>Plana o lateral
![Páginas organizadas en una estructura plana](images/nav/nav-pages-peer.png)

En una estructura plana o lateral, las páginas existen en paralelo. Puedes ir de una página a otra en cualquier orden. 

Te recomendamos que uses una estructura plana en los siguientes casos: 
<ul>
<li>Las páginas se pueden ver en cualquier orden.</li>
<li>las páginas son claramente distintas entre sí y no tienen una relación primaria-secundaria obvia.</li>
<li>Hay menos de 8 páginas en el grupo.<br/>
(Si hay más páginas, a los usuarios les puede resultar difícil comprender el modo en que las páginas son únicas o entender su ubicación actual dentro del grupo. Si no crees que eso es un problema para tu aplicación, organiza las páginas como elementos del mismo nivel. De lo contrario, piensa en la posibilidad de usar una estructura jerárquica para separar las páginas en dos o más grupos pequeños).</li>
</ul>

### <a name="hierarchical"></a>Jerárquica
![Páginas organizadas en una jerarquía](images/nav/nav-pages-hiearchy.png)

En una estructura jerárquica, las páginas se organizan en una estructura parecida a un árbol. Cada página secundaria tiene un solo elemento primario, pero un elemento primario puede tener una o más páginas secundarias. Para llegar a una página secundaria, hay que pasar por el elemento primario.

Las estructuras jerárquicas sirven para organizar un contenido complejo que ocupe una gran cantidad de páginas. La desventaja es que se produce cierta sobrecarga de navegación: cuanto más profunda es la estructura, más clics se necesitan para que los usuarios vayan de una página otra. 

Te recomendamos una estructura jerárquica en los siguientes casos: 
<ul>
<li>Las páginas deben recorrerse en un orden específico.</li>
<li>Hay una relación primaria-secundaria clara entre las páginas.</li>
<li>Hay más de 7 páginas en el grupo.</li>
</ul>

### <a name="combining-structures"></a>Combinación de estructuras
![una aplicación con una estructura híbrida](images/nav/nav-hybridstructure.png.png)

No tienes que elegir una estructura u otra; muchas aplicaciones bien diseñadas usan ambas. Una aplicación puede usar estructuras planas para las páginas de nivel superior, que pueden verse en cualquier orden, y estructuras jerárquicas para las páginas que tienen relaciones más complejas. 

Si la estructura de navegación tiene varios niveles, te recomendamos que los elementos de navegación punto a punto solo se vinculen con los elementos del mismo nivel dentro de su subárbol. Ten en cuenta la siguiente ilustración, que muestra una estructura de navegación con tres niveles:

![una aplicación con dos subárboles](images/nav/nav-subtrees.png)
- En el nivel 1, el elemento de navegación punto a punto debe proporcionar acceso a las páginas A, B, C y D.
- En el nivel 2, los elementos de navegación punto a punto de las páginas A2 solo deben vincularse a otras páginas A2. No se deben vincular a páginas de nivel 2 del subárbol C.

![una aplicación con dos subárboles](images/nav/nav-subtrees2.png)

## <a name="use-the-right-controls"></a>Uso de los controles correctos
Cuando hayas decidido la estructura de las páginas, tendrás que decidir cómo navegarán los usuarios a través de esas páginas. La UWP proporciona una variedad de controles de navegación para ayudar a garantizar una experiencia de navegación coherente y fiable en la aplicación. 

Te recomendamos seleccionar un control de navegación basado en el número de elementos de navegación que haya en la aplicación. Si tienes cinco elementos de navegación o menos, usa la navegación de nivel superior, como el modelo de [pestañas y controles dinámicos](../controls-and-patterns/tabs-pivot.md). Si tiene seis elementos de navegación o más, usa la navegación a la izquierda, como la [vista de navegación](../controls-and-patterns/navigationview.md) o el patrón de [maestro y detalles](../controls-and-patterns/master-details.md).

<div class="mx-responsive-img">

<table>
<tr>
    <th>Control</th>
    <th>Descripción</th>
</tr>
<tr>
    <td><a href="https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Frame">Marco</a><br/><br/>
    <img src="images/frame.png" alt="Frame" /></td>
    <td>Muestra páginas <p>Con algunas excepciones, cualquier aplicación que tiene varias páginas usa un marco. Por lo general, una aplicación tiene una página principal que contiene el marco y un elemento de navegación primario, como un control de vista de navegación. Cuando el usuario selecciona una página, el marco la carga y la muestra.</p></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/tabs-pivot.md">Pestañas y controles dinámicos</a><br/><br/>
    <img src="images/nav/nav-tabs-sm-300.png" alt="Tab-based navigation" /></td>
    <td>Muestra una lista horizontal de vínculos a páginas del mismo nivel.
<p>Úsalo en estos casos:</p>
<ul>
<li>Hay entre 2 y 5 páginas. (Se pueden usar pestañas/controles dinámicos cuando hay más de 5 páginas pero puede ser difícil ajustarlos todos en la pantalla.)</li>
<li>Se espera que los usuarios cambien entre las páginas con frecuencia.</li>
</ul></td>
</tr>
<tr>
    <td><a href="../controls-and-patterns/navigationview.md">Vista de navegación</a><br/><br/>
    <img src="images/nav/nav-navpane-4page-thumb.png" alt="A navigation pane" /></td>
    <td>Muestra una lista vertical de vínculos a páginas de nivel superior.
<p>Úsalo en estos casos:</p>
<ul>
<li>Las páginas existen en el nivel superior.</li>
<li>Existen muchos elementos de navegación (más de 5)..</li>
<li>No se espera que los usuarios cambien entre las páginas con frecuencia.</li>

</ul></td>
</tr>
<tr>
<td><a href="../controls-and-patterns/master-details.md">Maestro/detalles</a><br/><br/>
<img src="images/higsecone-masterdetail-thumb.png" alt="Master/details" /></td>
<td>Muestra una lista (vista maestra) de elementos. Al seleccionar un elemento, se muestra su página correspondiente en la sección de detalles.
<p>Úsalo en estos casos:</p>
<ul>
<li>Se espera que los usuarios cambien entre elementos secundarios con frecuencia;</li>
<li>si desea permitir al usuario realizar operaciones de alto nivel, como eliminar u ordenar, en elementos individuales o grupos de elementos, y también quieres que el usuario pueda ver o actualizar los detalles de cada elemento.</li>
</ul>
<p>El patrón de maestro y detalles es ideal para las bandejas de entrada de correo electrónico, las listas de contactos y la entrada de datos.</p>
</td>
<tr>
<td><a href="../controls-and-patterns/hub.md">Centralizada</a><br/><br/>
<img src="images/hub.png" alt="Hub" /></td>
<td> Muestra secciones de elementos ordenados en cuadrículas. 
<p>Úsalo en estos casos:</p>
<ul>
<li>Quieres crear una navegación visual con imágenes prominentes.</li>
</ul>
<p>La navegación centralizada es ideal para las experiencias de exploración, visualización o compra.</p>
</td>
</tr>
<tr>
<td><a href="../controls-and-patterns/hyperlinks.md">Hipervínculos</a> y <a href="../controls-and-patterns/buttons.md">botones</a></td>
<td> En el contenido de una página pueden aparecer elementos de navegación incrustados. A diferencia de otros elementos de navegación, que deben ser coherentes en todas las páginas, los elementos de navegación incrustados de contenido son exclusivos en cada página.</td>
</tr>
</table>
</div>

## <a name="next-add-navigation-code-to-your-app"></a>A continuación: agrega código de navegación a la aplicación
En el siguiente artículo, [Implementación de la navegación básica](navigate-between-two-pages.md), se muestra el código necesario para usar un control Frame para habilitar en tu aplicación la navegación básica entre dos páginas. 