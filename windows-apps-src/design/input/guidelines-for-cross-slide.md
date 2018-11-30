---
Description: Use cross-slide to support selection with the swipe gesture and drag (move) interactions with the slide gesture.
title: Instrucciones para el deslizamiento transversal
ms.assetid: 897555e2-c567-4bbe-b600-553daeb223d5
ms.date: 10/25/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f6a37af19c9e3e9beaebbadfcc71f3ad01e3087c
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8207099"
---
# <a name="guidelines-for-cross-slide"></a>Instrucciones para el deslizamiento transversal




**API importantes**

-   [**CrossSliding**](https://msdn.microsoft.com/library/windows/apps/br241942)
-   [**CrossSlideThresholds**](https://msdn.microsoft.com/library/windows/apps/br241941)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)

Usa el gesto de deslizamiento transversal para admitir la selección con el gesto de deslizar rápidamente y las interacciones de arrastrar (mover) con el gesto de deslizar.

## <a name="span-iddosanddontsspanspan-iddosanddontsspanspan-iddosanddontsspandos-and-donts"></a><span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Qué hacer y qué no hacer


-   Usa el deslizamiento transversal para listas y colecciones que se desplazan en una sola dirección.
-   Usa el deslizamiento transversal para la selección de elementos cuando la interacción de pulsar se utilice con otra finalidad.
-   No uses el deslizamiento transversal para agregar elementos a una cola.

## <a name="span-idadditionalusageguidancespanspan-idadditionalusageguidancespanspan-idadditionalusageguidancespanadditional-usage-guidance"></a><span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Instrucciones de uso adicionales


Las interacciones de seleccionar y arrastrar solo se pueden realizar dentro de un área de contenido que sea desplazable en una dirección (vertical u horizontal). Para que funcionen, una dirección de movimiento panorámico debe estar bloqueada y el gesto se debe realizar en la dirección perpendicular a la del movimiento panorámico.

Aquí te mostramos cómo seleccionar y arrastrar un objeto con un deslizamiento transversal. La imagen de la izquierda muestra que se selecciona un elemento si el gesto de deslizar rápidamente no cruza un umbral de distancia antes de que se levante el contacto y se suelte el objeto. En la imagen de la derecha se muestra que se arrastra el objeto si el gesto de deslizar cruza un umbral de distancia.

![diagrama en el que se muestran los procesos de seleccionar, y arrastrar y colocar.](images/crossslide-mechanism.png)

En el siguiente diagrama se muestran las distancias de umbral que se usan en la interacción deslizar transversalmente.

![captura de pantalla en la que se muestran los procesos de seleccionar, y arrastrar y colocar.](images/crossslide-threshold.png)

Para preservar la función de movimiento panorámico, se debe cruzar un pequeño umbral de 2,7 mm (aproximadamente 10 píxeles en la resolución de destino) antes de que se active la interacción de seleccionar o arrastrar. Este pequeño umbral ayuda al sistema a diferenciar entre deslizar transversalmente y mover panorámicamente. También garantiza que el gesto de pulsar se distingue de los gestos de deslizar transversalmente y mover panorámicamente.

En esta imagen se muestra cómo un usuario toca un elemento de la interfaz de usuario, pero mueve el dedo ligeramente hacia abajo al hacer contacto. Sin ningún umbral, la interacción se interpretaría como deslizar transversalmente a causa del movimiento vertical inicial. Con el umbral, el movimiento se interpreta correctamente como movimiento panorámico horizontal.

![captura de pantalla en la que se muestra el umbral de desambiguación de seleccionar y arrastrar y colocar.](images/crossslide-threshold2.png)

Aquí encontrarás algunas directrices que debes tener en cuenta a la hora de incluir la funcionalidad de deslizar transversalmente en tu aplicación.

Usa el deslizamiento transversal para listas y colecciones que se desplazan en una sola dirección. Para más información, consulta [Agregar controles ListView](https://msdn.microsoft.com/library/windows/apps/hh465382).

**Nota**en casos donde pueda desplazarse lateralmente en dos direcciones; por ejemplo, los exploradores web o lectores de e, el área de contenido se debe usar la interacción pulsar y sostener para invocar el menú contextual para objetos como imágenes e hipervínculos.

 

|                                                                                         |                                                                                         |
|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| ![movimiento panorámico horizontal, lista de dos dimensiones](images/groupedlistview1.png)                | ![movimiento panorámico vertical, lista de una dimensión](images/listviewlistlayout.png)                |
| Una lista de dos dimensiones con movimiento panorámico horizontal. Arrastra verticalmente para seleccionar o mover un elemento. | Una lista de una dimensión con movimiento panorámico vertical. Arrastra horizontalmente para seleccionar o mover un elemento. |

 

### <span id="selection"></span><span id="SELECTION"></span>

**Seleccionar**

Seleccionar es el marcado de uno o más objetos sin que haya inicio o activación. Esta acción es similar a un único clic con el mouse o la tecla Mayús y un clic del mouse sobre uno o varios objetos.

Para seleccionar mediante deslizar transversalmente, se toca un elemento y se suelta después de una interacción breve de arrastre. Este método de selección prescinde del modo de selección dedicada y de la interacción con intervalo de pulsar y sostener que requieren otras interfaces táctiles. Tampoco entra en conflicto con la interacción de pulsar para activación.

Además de la limitación del umbral de distancia, la selección mediante el deslizamiento transversal está restringida a un área de umbral de 90°, como se ve en el diagrama siguiente. Si el objeto se arrastra fuera del área, no queda seleccionado.

![diagrama en el que se muestra el área de umbral de selección.](images/crossslide-selection.png)

La interacción mediante el deslizamiento transversal se complementa con una interacción temporal de pulsar y sostener, lo que también se denomina interacción "reveladora". Esta interacción complementaria activa una animación que indica qué acción se puede realizar en el objeto. Si deseas obtener más información sobre la interfaz de usuario para desambiguación, consulta el tema sobre [directrices de comentarios visuales](guidelines-for-visualfeedback.md).

Las siguientes capturas de pantalla muestran el funcionamiento de la animación reveladora.

1.  Pulsa y sostén para iniciar la animación para la interacción reveladora. El estado seleccionado del elemento afecta a lo que la animación revela: una marca de verificación si no está seleccionado y ninguna marca de verificación si está seleccionado.

    ![captura de pantalla en la que se muestra un estado sin selección.](images/crossslide-selfreveal1.png)

2.  Selecciona el elemento utilizando el gesto de deslizar (arriba o abajo).

    ![captura de pantalla en la que se muestra la animación para la selección.](images/crossslide-selfreveal2.png)

3.  El elemento ya está seleccionado. Reemplaza el comportamiento de la selección mediante el gesto de deslizar para mover el elemento.

    ![captura de pantalla en la que se muestra la animación para arrastrar y colocar.](images/crossslide-selfreveal3.png)

Usa una sola pulsación para la selección en las aplicaciones en que esta es la única acción principal. La animación reveladora se muestra al deslizar transversalmente para eliminar la ambigüedad entre esta función y la interacción de pulsar estándar empleada para activar y navegar.

**Cesta de selección**

La cesta de selección es una representación dinámica y visualmente clara de elementos que se seleccionaron en la lista o colección primarias de la aplicación. Esta funciones se usa para realizar un seguimiento de los elementos seleccionados y debe usarse en las aplicaciones donde:

-   Los elementos se pueden seleccionar desde varias ubicaciones.
-   Se pueden seleccionar muchos elementos.
-   Una acción o un comando depende de la lista de selección.

El contenido de la cesta de selección se mantiene con todas las acciones y comandos. Por ejemplo, si seleccionas una serie de fotografías de una galería, aplicas una corrección de color a cada fotografía y compartes las fotografías de algún modo, la selección de elementos se mantiene.

Si en una aplicación no se usa cesta de selección, es necesario borrar la selección actual después de una acción o un comando. Por ejemplo, si seleccionas un tema de una lista de reproducción y le asignas una calificación, debes borrar la selección.

También es necesario borrar la selección actual cuando no se usa una cesta de selección y se activa otro elemento de la lista o colección. Por ejemplo, si seleccionas un mensaje de la bandeja de entrada, se actualiza el panel de vista previa. Si después seleccionas otro mensaje de la bandeja de entrada, la selección del mensaje anterior se cancela y se actualiza el panel de vista previa.

**Colas**

Una cola no equivale a la lista de la cesta de selección y no se debe tratar como tal. Entre las distinciones primarias se incluyen:

-   La lista de elementos de la cesta de selección solo es una representación visual; los elementos de una cola se ensamblan teniendo en cuenta una acción específica.
-   Los elementos se pueden representar únicamente en la cesta de selección pero varias veces en una cola.
-   El orden de los elementos en la cesta de selección representa el orden de la selección. El orden de los elementos en una cola está directamente relacionado con la funcionalidad.

Por estas razones, la interacción de selección mediante el deslizamiento transversal no debe usarse para agregar elementos a una cola. En cambio, para agregar elementos a una cola, se debe usar una acción de arrastrar.

### <span id="draganddrop"></span><span id="DRAGANDDROP"></span>

**Arrastrar**

Usa la acción de arrastrar para mover uno o más objetos de un lugar a otro.

Si es necesario mover más de un objeto, deja que los usuarios seleccionen varios elementos y los arrastren todos al mismo tiempo.

## <a name="span-idrelatedtopicsspanrelated-articles"></a><span id="related_topics"></span>Artículos relacionados


**Ejemplos**
* [Ejemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Muestra de entrada de latencia baja](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Ejemplo de modo de interacción del usuario](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Muestras de archivo**
* [Entrada: muestra de eventos de entrada de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: muestra de funcionalidades del dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: muestra de prueba de acceso táctil](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Muestra de desplazamiento, movimiento panorámico y zoom XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: ejemplo de entrada de lápiz simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: muestra de gestos de Windows 8](http://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: muestra de manipulaciones y gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Muestra de entrada táctil de DirectX](http://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




