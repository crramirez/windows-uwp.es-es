---
title: Vectores normales de caras y vértices
description: Cada cara de una malla tiene un vector normal de unidad perpendicular. La dirección del vector viene determinada por el orden en el que se definen los vértices y por si el sistema de coordenadas es una mano derecha o izquierda.
ms.assetid: 02333579-9749-4612-B121-23F97898A3E0
keywords:
- Vectores normales de caras y vértices
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ef0d3ea5a3bc0f5c4ac6b6b660dc543919d297ec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168169"
---
# <a name="face-and-vertex-normal-vectors"></a>Vectores normales de caras y vértices


Cada cara de una malla tiene un vector normal de unidad perpendicular. La dirección del vector viene determinada por el orden en el que se definen los vértices y por si el sistema de coordenadas es una mano derecha o izquierda.

## <a name="span-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanspan-idperpendicular_unit_normal_vector_for_a_front_facespanperpendicular-unit-normal-vector-for-a-front-face"></a><span id="Perpendicular_unit_normal_vector_for_a_front_face"></span><span id="perpendicular_unit_normal_vector_for_a_front_face"></span><span id="PERPENDICULAR_UNIT_NORMAL_VECTOR_FOR_A_FRONT_FACE"></span>Vector normal de unidad perpendicular para una cara frontal


Cada cara de una malla tiene un vector normal de unidad perpendicular. La dirección del vector viene determinada por el orden en el que se definen los vértices y por si el sistema de coordenadas es una mano derecha o izquierda. La cara normal apunta fuera del lado frontal de la cara. En Direct3D, solo está visible la parte frontal de una cara. Una cara frontal es aquella en la que los vértices se definen en el orden de las agujas del reloj.

En la ilustración siguiente se muestra un vector normal para una cara frontal:

![un vector normal para una cara frontal](images/nrmlvect.png)

## <a name="span-idculling_back_facesspanspan-idculling_back_facesspanspan-idculling_back_facesspanculling-back-faces"></a><span id="Culling_back_faces"></span><span id="culling_back_faces"></span><span id="CULLING_BACK_FACES"></span>Selección de caras atrás


Cualquier cara que no sea una cara frontal es una cara trasera. Direct3D no siempre representa caras inversas; se dice que las caras atrás se seleccionan. La selección de la cara posterior significa eliminar caras de la representación. Si lo desea, puede cambiar el modo de selección de datos para que represente caras. Consulte [Estado de selección](/windows/desktop/direct3d9/culling-state) de datos para obtener más información.

## <a name="span-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanspan-idvertex_unit_normalsspanvertex-unit-normals"></a><span id="Vertex_unit_normals"></span><span id="vertex_unit_normals"></span><span id="VERTEX_UNIT_NORMALS"></span>Normales de unidad de vértice


Direct3D usa las normales de unidad de vértices para los efectos de sombreado Gouraud, iluminación y textura.

En la ilustración siguiente se muestran las normales de vértice:

![normales de vértices](images/vertnrml.png)

Al aplicar el sombreado Gouraud a un polígono, Direct3D usa las normales de vértice para calcular el ángulo entre la fuente de luz y la superficie. Calcula los valores de color y intensidad de los vértices y los interpola para cada punto de todas las superficies primitivas. Direct3D calcula el valor de la intensidad de la luz mediante el ángulo. Cuanto mayor sea el ángulo, menos luz se iluminará en la superficie.

## <a name="span-idflat_surfacesspanspan-idflat_surfacesspanspan-idflat_surfacesspanflat-surfaces"></a><span id="Flat_surfaces"></span><span id="flat_surfaces"></span><span id="FLAT_SURFACES"></span>Superficies planas


Si va a crear un objeto plano, establezca el valor normal del vértice en el punto perpendicular a la superficie.

En la ilustración siguiente se muestra una superficie plana compuesta por dos triángulos con normales de vértice:

![superficie plana compuesta por dos triángulos con normales de vértice](images/flatvert.png)

## <a name="span-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspanspan-idsmooth_shading_on_a_non-flat_objectspansmooth-shading-on-a-non-flat-object"></a><span id="Smooth_shading_on_a_non-flat_object"></span><span id="smooth_shading_on_a_non-flat_object"></span><span id="SMOOTH_SHADING_ON_A_NON-FLAT_OBJECT"></span>Sombreado suave en un objeto no plano


En lugar de un objeto plano, es más probable que el objeto esté formado por franjas de triángulo y que los triángulos no sean coplanos. Una manera sencilla de lograr un sombreado suave en todos los triángulos de la franja es calcular primero el vector normal de la superficie para cada cara poligonal a la que está asociado el vértice. El vértice normal se puede establecer para crear un ángulo igual con cada superficie normal. Sin embargo, es posible que este método no sea lo suficientemente eficaz para primitivas complejas.

Este método se muestra en el siguiente diagrama, que muestra dos superficies, S1 y S2 que se han encontrado en el borde anterior. Los vectores normales para S1 y S2 se muestran en azul. El vector normal del vértice se muestra en rojo. El ángulo que el vector normal de vértice realiza con la superficie normal de S1 es el mismo que el del vértice normal y el normal de la superficie de S2. Cuando estas dos superficies están iluminadas y sombreadas con el sombreado Gouraud, el resultado es un borde con un sombreado suave y con una curvatura suave.

En la ilustración siguiente se muestran dos superficies (S1 y S2) y sus vectores normales y vector normal de vértice:

![dos superficies (S1 y S2) y sus vectores normales y vector normal de vértice](images/gvert.png)

Si el vértice normal se inclina hacia uno de los rostros con el que está asociado, hace que la intensidad de la luz aumente o disminuya para los puntos de la superficie, dependiendo del ángulo que realice con la fuente de luz. En el diagrama siguiente se muestra un ejemplo. Estas superficies se ven perimetrales. El vértice normal se inclina hacia S1, lo que hace que tenga un ángulo más pequeño con la fuente de luz que si el vértice normal tuviera ángulos iguales con las normales de la superficie.

En la ilustración siguiente se muestran dos superficies (S1 y S2) con un vector normal de vértice que se inclina hacia una cara:

![dos superficies (S1 y S2) con un vector normal de vértice que se inclina hacia una cara](images/gvert2.png)

## <a name="span-idsharp_edgesspanspan-idsharp_edgesspanspan-idsharp_edgesspansharp-edges"></a><span id="Sharp_edges"></span><span id="sharp_edges"></span><span id="SHARP_EDGES"></span>Bordes nítidos


Puede usar el sombreado Gouraud para mostrar algunos objetos en una escena 3D con bordes nítidos. Para ello, duplique los vectores normales de vértices en cualquier intersección de caras en las que se requiera un borde nítido.

En la ilustración siguiente se muestran vectores normales de vértices duplicados en bordes nítidos:

![vectores normales de vértices duplicados en bordes agudos](images/shade1.png)

Si usa los métodos DrawPrimitive para representar la escena, defina el objeto con bordes nítidos como una lista de triángulos, en lugar de una franja de triángulo. Al definir un objeto como una franja de triángulo, Direct3D lo trata como un único polígono compuesto por varias caras triangulares. El sombreado Gouraud se aplica tanto en cada cara del polígono como entre caras adyacentes.

El resultado es un objeto que se sombrea suavemente de la superficie a la superficie. Dado que una lista de triángulos es un polígono compuesto por una serie de caras triangular discontinuas, Direct3D aplica sombreado Gouraud en cada cara del polígono. Sin embargo, no se aplica de la esfera a la superficie. Si dos o más triángulos de una lista de triángulo son adyacentes, parecen tener un borde nítido entre ellos.

Otra alternativa es cambiar a sombreado plano al representar objetos con bordes nítidos. Este es el método más eficaz, pero puede dar lugar a que los objetos de la escena no se representen de forma realista como los objetos con sombreado Gouraud.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 