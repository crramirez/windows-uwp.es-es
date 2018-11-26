---
description: ''
title: Contenido como objetos
template: detail.hbs
ms.localizationpriority: medium
ms.openlocfilehash: 37ba5093f2d7cfe268be40413b889801daf00967
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7691406"
---
# <a name="content-as-objects"></a>Contenido como objetos

 

Puedes manipular la profundidad u orden z de elementos para crear una jerarquía visual que te ayude a que tu aplicación sea más fácil de usar.  

> Nota: este artículo es un primer borrador de una nueva característica de Windows10RS2. Los nombres de las características, la terminología y la funcionalidad no son definitivos. 

## <a name="why-visual-hierarchy-is-important"></a>Por qué es importante la jerarquía visual

Los usuarios están constantemente bombardeados con solicitudes de atención. Cada elemento de la pantalla exige que se examine, cada cadena de texto quiere que se lea y cada botón espera que se haga clic en él. A medida que el entorno visual va creciendo mezclado y caótico, supone más esfuerzo analizar la escena y averiguar qué está sucediendo.  

Por eso es tan importante que selecciones cuidadosamente los elementos de tu interfaz de usuario, y por eso es tan importante crear una distribución que establezca una jerarquía visual clara entre los elementos de tu interfaz de usuario. <!-- Every element is competing for the user's attention, and every time you add an element, you add a mental tax to the user. -->

Una jerarquía visual clara indica al usuario qué elementos son los más importantes y crea relaciones entre los elementos. Con una buena jerarquía visual, los usuarios entienden la distribución de la página de un solo vistazo y pueden centrarse en la tarea que están intentando realizar. 

<p></p>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p>Así que, ¿cómo puedes crear una jerarquía visual clara? Con las versiones anteriores de Windows 10, podías usar espacio en blanco, posición y tipografía para definir una jerarquía visual. </p>
  </div>
  <div class="side-by-side-content-right">
    ![Una distribución plana](images/content-as-objects/flat-layout.png)
    
  </div>
</div>
</div>

Con Windows 10 RS2, hemos agregado literalmente otra dimensión: la profundidad. 

![Profundidad en la distribución](images/content-as-objects/depth-in-layout2.png)


## <a name="use-depth-to-establish-a-hierarchy"></a>Usar profundidad para establecer una jerarquía 

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
     <p>Puedes usar la profundidad (orden z) con las demás herramientas de diseño (espacios en blanco, posición, tipografía) para establecer una jerarquía. Eleva tus elementos más importantes al nivel más avanzado; usa niveles inferiores para mostrar una interfaz de usuario menos crítica. 

    The relative importance of an element can change throughout an experience, so you can bring elements forward as they become more important and backward as they become less important. 
    </p>
  </div>
  <div class="side-by-side-content-right">
    ![Profundidad en la distribución](images/content-as-objects/elements-forward-backward.png) 
    
  </div>
</div>
</div>

## <a name="how-does-it-work"></a>¿Cómo funciona?
> TAREA: breve descripción de cómo puedes controlar el orden z de elementos. ¿Para ti codifica explícitamente el orden z o hay un sistema de clasificación semántica? ¿Cómo mover elementos de un nivel a otro? ¿Qué hace el sistema automáticamente y de qué deben preocuparse los diseñadores/desarrolladores? 

## <a name="the-four-layers-of-a-typical-app-layers"></a>Los cuatro niveles de una aplicación típica

<p>Una aplicación típica tiene 4 niveles.</p>
<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Fondo posterior** <br/>
Este nivel se encuentra detrás de la aplicación.  Cuando los elementos se muevan a este nivel, te recomendamos que los hagas no interactivos. Los elementos en este nivel tienen el efecto parallax más lento y se recortan en la ventana de la aplicación. TAREA: ¿escala este nivel? 

<p>Los elementos de fondo del ejemplo incluyen la imagen detrás del contenido, TAREA: ejemplo, TAREA: ejemplo.</p>
  </div>
  <div class="side-by-side-content-right">
    ![El nivel de fondo posterior de una aplicación](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Nivel pasivo** <br/>
Este es el nivel de base de la aplicación, donde se encuentran los elementos de la interfaz de usuario de manera predeterminada.  Los elementos se mueven en tiempo real a este nivel (sin parallax), se recortan en la ventana de la aplicación y se procesan al 100% de la escala. 

<p>Elementos de ejemplo: fondo de la aplicación, texto, interfaz de usuario secundaria, como la interfaz de usuario para navegar por la aplicación.</p>
  </div>
  <div class="side-by-side-content-right">
    ![El nivel pasivo de una aplicación](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Llama a la acción** <br/>
Este nivel es para elementos interactivos a los que des prioridad por encima de los elementos del nivel pasivo. Los elementos en este nivel tienen el efecto parallax medio y se recortan en la ventana de la aplicación. TAREA: ¿los elementos en este nivel escalan o tienen una sombra paralela?

<p>Elementos de ejemplo: listas, cuadrículas, comandos principales (TAREA: como...).</p> 
  </div>
  <div class="side-by-side-content-right">
    ![El nivel de llamada a la acción de una aplicación](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>

<p></p>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  **Nivel principal** <br/>
Este nivel es para el elemento de mayor prioridad en la pantalla en el momento.  Los elementos de este nivel pueden interrumpir los límites de la ventana de la aplicación, pueden escalar y pueden obtener automáticamente una sombra paralela.

<p>Elementos de ejemplo: elementos fotográficos, el elemento seleccionado actualmente.</p>  
  </div>
  <div class="side-by-side-content-right">
    ![El nivel principal de una aplicación](images/content-as-objects/elements-forward-backward.png)
    
  </div>
</div>
</div>



<!--
Depth is meaningful; it establishes visual and interactive hierarchy for users to efficiently complete tasks. Depth orients users in our system. 
-->

## <a name="example-tbd"></a>Ejemplo: TBD
> TAREA: mostrar cómo adaptar un patrón común de interfaz de usuario para usar el orden z. Debemos mostrar ilustraciones y código. 

## <a name="download-the-code-samples"></a>Descargar las muestras de código
>TAREA: vínculo a ejemplos que muestran esta característica. 


## <a name="related-articles"></a>Artículos relacionados
* [Conceptos básicos de contenido](../basics/content-basics.md)
