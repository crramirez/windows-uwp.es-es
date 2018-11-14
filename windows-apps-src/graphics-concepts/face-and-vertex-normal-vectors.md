---
title: Vectores normales de caras y vértices
description: Cada cara de una malla tiene un vector normal a la unidad. La dirección del vector está determinada por el orden en que se definen los vértices y por si el sistema de coordenadas es diestro o zurdo.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- Vectores normales de caras y vértices
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2081e8c09a6f6fd75f460af3f339902bcb80bac6
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "6449446"
---
# <a name="face-and-vertex-normal-vectors"></a>Vectores normales de caras y vértices


Cada cara de una malla tiene un vector normal a la unidad. La dirección del vector está determinada por el orden en que se definen los vértices y por si el sistema de coordenadas es diestro o zurdo.

## <a name="span-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanspan-idperpendicularunitnormalvectorforafrontfacespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Vector normal de unidad perpendicular de una cara anterior.


Cada cara de una malla tiene un vector normal de unidad perpendicular. La dirección del vector está determinada por el orden en que se definen los vértices y por si el sistema de coordenadas es diestro o zurdo. El vector normal de cara apunta hacia el lado frontal de la cara. En Direct3D, solo está visible la parte frontal de una cara. Una cara anterior es una en que los vértices se definen en el orden de las agujas del reloj.

En la siguiente ilustración se muestra un vector normal para una cara anterior:

![un vector normal para una cara anterior](images/nrmlvect.png)

## <a name="span-idcullingbackfacesspanspan-idcullingbackfacesspanspan-idcullingbackfacesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>Selección de caras posteriores


Cualquier cara que no es anterior es posterior. Direct3D no siempre representa caras posteriores; se dice que las caras posteriores deben seleccionarse. La selección de cara posterior significa eliminar las caras posteriores de la representación. Puedes cambiar el modo de selección para representar caras posteriores si quieres. Consulta [Culling State (Estado de selección)](https://msdn.microsoft.com/library/windows/desktop/bb204882) para obtener más información.

## <a name="span-idvertexunitnormalsspanspan-idvertexunitnormalsspanspan-idvertexunitnormalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>Vectores normales de unidad de vértice


Direct3D usa los vectores normales de unidad de vértice para la iluminación, los efectos de texturas y el sombreado Gouraud.

En la siguiente ilustración se muestran vectores normales de vértice:

![vectores normales de vértice](images/vertnrml.png)

Al aplicar el sombreado Gouraud a un polígono, Direct3D usa los vectores normales de vértice para calcular el ángulo entre la fuente de luz y la superficie. Calcula los valores de color e intensidad de los vértices y los interpola para cada punto a través de todas las superficies del primitivo. Direct3D calcula el valor de la intensidad de la luz utilizando el ángulo. Cuanto mayor sea el ángulo, menos luz brillará en la superficie.

## <a name="span-idflatsurfacesspanspan-idflatsurfacesspanspan-idflatsurfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>Superficies planas


Si vas a crear un objeto que es plano, establece vectores normales de vértice que apunten de forma perpendicular a la superficie.

En la siguiente ilustración se muestra una superficie plana formada por dos triángulos con vectores normales de vértice:

![superficie plana formada por dos triángulos con vectores normales de vértice](images/flatvert.png)

## <a name="span-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspanspan-idsmoothshadingonanon-flatobjectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>Sombreado suave en un objeto no plano


En lugar de que el objeto sea plano, lo más probable es que el objeto se componga de franjas de triángulos y los triángulos no son coplanos. Una forma sencilla de lograr sombreado suave entre todos los triángulos de la franja consiste en calcular primero el vector normal de superficie para cada cara poligonal con la que el vértice esté asociado. El vector normal de vértice puede establecerse para que cree un ángulo igual con cada superficie normal. Sin embargo, es posible que este método no sea lo suficientemente eficaz para los primitivos complejos.

Este método se ilustra en el diagrama siguiente, que muestra dos superficies, S1 y S2, con el ángulo visto desde arriba. Los vectores normales de S1 y S2 se muestran en color azul. El vector normal de vértice se muestra en rojo. El ángulo que el vector normal de vértice crea con el vector normal de superficie de S1 es el mismo que el ángulo entre el vector normal de vértice y de superficie de S2. Cuando estas dos superficies se iluminan y se sombrean con sombreado Gouraud, el resultado es un borde redondeado y sombreado suavemente entre ellos.

En la siguiente ilustración se muestran dos superficies (S1 y S2) y sus vectores normales y el vector normal de vértice:

![dos superficies (S1 y S2) y sus vectores normales y el vector normal de vértice](images/gvert.png)

Si el vector normal de vértice se inclina hacia una de las caras con las que está asociado, la intensidad de la luz aumenta o disminuye para los puntos de esa superficie, en función del ángulo que crea con la fuente de luz. En el diagrama siguiente se muestra un ejemplo. Estas superficies se muestran con el ángulo en la parte superior. El vector normal de vértice se inclina hacia S1, lo que produce que el ángulo con la fuente de luz sea menor que si el vector normal de vértice tuviera ángulos iguales con los vectores normales de superficie.

En la siguiente ilustración se muestran dos superficies (S1 y S2) con un vector normal de vértice que se inclina hacia una cara:

![dos superficies (S1 y S2) con un vector normal de vértice que se inclina hacia una cara](images/gvert2.png)

## <a name="span-idsharpedgesspanspan-idsharpedgesspanspan-idsharpedgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>Contornos afilados


Puedes usar el sombreado Gouraud para mostrar algunos objetos en una escena en 3D con contornos afilados. Para ello, duplica los vectores normales de vértice en cualquier intersección de caras donde se requiera un contorno afilado.

En la siguiente ilustración se muestran vectores normales de vértice duplicados en contornos afilados:

![vectores normales de vértice duplicados en contornos afilados](images/shade1.png)

Si usas los métodos DrawPrimitive para representar la escena, define el objeto con contornos afilados como una lista de triángulos, en lugar de una franja de triángulos. Cuando defines un objeto como una franja de triángulos, Direct3D lo trata como un polígono único compuesto por varias caras triangulares. El sombreado Gouraud se aplica tanto a través de cada cara del polígono como entre las caras adyacentes.

El resultado es un objeto sombreado suavemente de una cara a la otra. Dado que una lista de triángulos es un polígono compuesto por una serie de caras triangulares no contiguas, Direct3D aplica el sombreado Gouraud en cada cara del polígono. Sin embargo, no se aplica de una cara a la otra. Si dos o más triángulos de una lista de triángulos son adyacentes, parecerá que tienen un contorno afilado entre ellos.

Otra alternativa es cambiar a sombreado plano al representar objetos con contornos afilados. Este es el método más eficaz desde el punto de vista computacional, pero puede provocar que algunos objetos de la escena no se representen de la misma forma realista que los objetos que tienen sombreado Gouraud.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




