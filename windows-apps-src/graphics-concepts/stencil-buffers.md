---
title: Búferes de galerías de símbolos
description: Un búfer de galería de símbolos se usa para enmascarar píxeles en una imagen con el fin de producir efectos especiales.
ms.assetid: 544B3B9E-31E3-41DA-8081-CC3477447E94
keywords:
- Búferes de galerías de símbolos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 090dcab3b46b031a16be35973e1183220d5b1da0
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7305217"
---
# <a name="stencil-buffers"></a>Búferes de galerías de símbolos


Un *búfer de galería de símbolos* se usa para enmascarar píxeles en una imagen a fin de producir efectos especiales. La máscara controla si el píxel se dibuja o no. Estos efectos especiales incluyen la composición, el calco, las disoluciones, los fundidos y los deslizamientos, los contornos y las siluetas, y la galería de símbolos de dos caras. A continuación se muestran algunas de los efectos más habituales.

El búfer de la galería de símbolos habilita o deshabilita el dibujo en la superficie de destino de representación píxel a píxel. En su nivel más básico, permite que las aplicaciones enmascaren secciones de la imagen representada para que no se muestren. A menudo, las aplicaciones usan búferes de galerías de símbolos para efectos especiales, como disoluciones, calco y contornos.

La información del búfer de la galería de símbolos se inserta en los datos del búfer z.

## <a name="span-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanspan-idhowthestencilbufferworksspanhow-the-stencil-buffer-works"></a><span id="How_the_Stencil_Buffer_Works"></span><span id="how_the_stencil_buffer_works"></span><span id="HOW_THE_STENCIL_BUFFER_WORKS"></span>Cómo funciona el búfer de la galería de símbolos


Direct3D realiza una prueba del contenido del búfer de la galería de símbolos píxel a píxel. Para cada píxel de la superficie de destino, realiza una prueba empleando el valor correspondiente del búfer de la galería de símbolos, un valor de referencia de la galería de símbolos y un valor de máscara de la galería de símbolos. Si se pasa la prueba, Direct3D realiza una acción. La prueba se realiza mediante los siguientes pasos.

1.  Realizar una operación AND bit a bit del valor de referencia de la galería de símbolos con la máscara de la galería de símbolos.
2.  Realizar una operación AND bit a bit del valor del búfer de la galería de símbolos para el píxel actual con la máscara de la galería de símbolos.
3.  Comparar el resultado del paso 1 con el resultado del paso 2 mediante la función de comparación.

Los pasos anteriores se muestran en la siguiente línea de código:

```
(StencilRef & StencilMask) CompFunc (StencilBufferValue & StencilMask)
```

-   StencilRef representa el valor de referencia de la galería de símbolos.
-   StencilMask representa el valor de la máscara de la galería de símbolos.
-   CompFunc es la función de comparación.
-   StencilBufferValue es el contenido del búfer de la galería de símbolos para el píxel actual.
-   El símbolo & representa la operación AND bit a bit.

El píxel actual se escribe en la superficie de destino si se supera la prueba de la galería de símbolos y se omite en caso contrario. El comportamiento predeterminado de la comparación es escribir el píxel, independientemente de cómo cada operación bit a bit. Puedes cambiar este comportamiento cambiando el valor de un tipo enumerado para identificar la función de comparación deseada.

La aplicación puede personalizar la operación del búfer de la galería de símbolos. Puede establecer la función de comparación, la máscara de la galería de símbolos y el valor de referencia de la galería de símbolos. También puede controlar la acción que realiza Direct3D cuando pasa se supera o se suspende la prueba de la galería de símbolos.

## <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composición


La aplicación puede usar el búfer de la galería de símbolos para componer imágenes 2D o 3D en una escena 3D. Una máscara en el búfer de la galería de símbolos se usa para tapar un área de la superficie de destino de representación. La información 2D almacenada, como texto o mapas de bits, luego puede escribirse en el área que queda oculta. Como alternativa, la aplicación puede representar primitivos 3D adicionales en la región enmascarada por la galería de símbolos de la superficie de destino de representación. Incluso puede representar una escena completa.

A menudo, los juegos componen varias escenas 3D juntas. Por ejemplo, los juegos de coches suelen mostrar un espejo retrovisor. El espejo contiene la vista de la escena 3D detrás del conductor. En esencia, es una segunda escena 3D compuesta con la vista hacia delante del conductor.

## <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Calco


Las aplicaciones Direct3D usan el calco para controlar qué píxeles de una imagen primitiva determinada se dibujan en la superficie de destino de representación. Las aplicaciones aplican los calcos a las imágenes de primitivos para permitir que los polígonos coplanares se representen correctamente.

Por ejemplo, al aplicar las marcas de rueda y líneas amarillas a una carretera, las marcas deben aparecer directamente en la parte superior de la carretera. Sin embargo, los valores z de las marcas y de la carretera son los mismos. Por lo tanto, el búfer de profundidad podría no producir una separación clara entre las dos. Algunos píxeles del primitivo trasero pueden representarse sobre el primitivo frontal y viceversa. La imagen resultante parece relucir de un fotograma a otro. Este efecto se denomina *rivalidad z* o *disputa*.

Para resolver este problema, usa una galería de símbolos para enmascarar la sección del primitivo trasero donde va a aparecer el calco. Desactiva el búfer z y representa la imagen del primitivo frontal en el área enmascarada de la superficie de destino de representación.

Aunque puede usarse la combinación de texturas múltiples para solucionar este problema, hacer eso limita el número de otros efectos especiales que puede generar la aplicación. El uso del búfer de la galería de símbolos para aplicar calcos libera fases de combinación de texturas para otros efectos.

## <a name="span-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspanspan-iddissolvesfadesandswipesspandissolves-fades-and-swipes"></a><span id="Dissolves__fades__and_swipes"></span><span id="dissolves__fades__and_swipes"></span><span id="DISSOLVES__FADES__AND_SWIPES"></span>Disoluciones, fundidos y deslizamientos


Cada vez más, las aplicaciones emplean efectos especiales que se usan habitualmente en películas y vídeo, como disoluciones, deslizamientos y fundidos.

En una disolución, una imagen se sustituye gradualmente por otra en una secuencia de fotogramas continua. Aunque Direct3D proporciona métodos para el uso de la combinación de varias texturas para lograr el mismo efecto, las aplicaciones que usan el búfer de la galería de símbolos para las disoluciones pueden emplear las funcionalidades de combinación de texturas para otros efectos mientras realizan una disolución.

Cuando la aplicación realiza una disolución, debe representar dos imágenes diferentes. Usa el búfer de la galería de símbolos para controlar qué píxeles de cada imagen se dibujan en la superficie de destino de representación. Puedes definir una serie de máscaras de la galería de símbolos y copiarlas en el búfer de la galería de símbolos en fotogramas sucesivos. Como alternativa, puedes definir una máscara base de la galería de símbolos para el primer fotograma y modificarlo de forma incremental.

Al principio de la disolución, la aplicación establece la función de la galería de símbolos y la máscara de la galería de símbolos para que la mayoría de los píxeles de la imagen inicial pasen la prueba de la galería de símbolos. La mayoría de los píxeles de la imagen final deberían suspender la prueba de la galería de símbolos. En los fotogramas sucesivos, se actualiza la máscara de la galería de símbolos para que cada vez menos píxeles de la imagen inicial pasen la prueba. A medida que avancen los fotogramas, cada vez menos píxeles de la imagen final no pasarán la prueba. De esta manera, la aplicación puede realizar una disolución con cualquier patrón de disolución arbitrario.

El fundido de entrada o salida es un caso especial de disolución. Al realizar un fundido de entrada, el búfer de la galería de símbolos se usa para disolver desde una imagen en blanco o negro a una representación de una escena en 3D. El fundido de salida es lo contrario: la aplicación empieza con una representación de una escena en 3D y se disuelve a blanco o negro. El fundido puede hacerse con cualquier patrón arbitrario que quieras emplear.

Las aplicaciones de Direct3D usan una técnica similar para los deslizamientos. Por ejemplo, cuando una aplicación realiza un deslizamientos de izquierda a derecha, la imagen final parece deslizarse gradualmente sobre la imagen inicial de izquierda a derecha. Al igual que en una disolución, debes definir una serie de máscaras de la galería de símbolos que se carguen en el búfer de la galería de símbolos en fotogramas sucesivos o modificar sucesivamente la máscara inicial de la galería de símbolos. Las máscaras de galerías de símbolos se usan para deshabilitar la escritura de píxeles desde la imagen inicial y para habilitar la escritura de píxeles desde la imagen final.

Un deslizamiento es un poco más complejo que una disolución, ya que la aplicación debe leer los píxeles de la imagen final en el orden inverso del deslizamiento. Es decir, si el deslizamiento se mueve de izquierda a derecha, la aplicación debe leer los píxeles de la imagen final de derecha a izquierda.

## <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanoutlines-and-silhouettes"></a><span id="Outlines_and_silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span>Contornos y siluetas


Puedes usar el búfer de la galería de símbolos para efectos más abstractos, como los contornos y las siluetas.

Si la aplicación aplica una máscara de la galería de símbolos a la imagen de un primitivo que tiene la misma forma pero es ligeramente más pequeña, la imagen resultante contiene únicamente el contorno del primitivo. Luego la aplicación puede llenar el área de la imagen enmascarada por la galería de símbolos con un color sólido, lo que da al primitivo un aspecto de relieve.

Si la máscara de la galería de símbolos tiene el mismo tamaño y forma que el primitivo que estás representando, la imagen resultante contiene un hueco donde debería estar el primitivo. Luego la aplicación puede rellenar el espacio con color negro para producir una silueta del primitivo.

## <a name="span-idtwo-sidedstencilspanspan-idtwo-sidedstencilspanspan-idtwo-sidedstencilspantwo-sided-stencil"></a><span id="Two-sided_stencil"></span><span id="two-sided_stencil"></span><span id="TWO-SIDED_STENCIL"></span>Galería de símbolos de dos caras


Los volúmenes de sombras se usan para dibujar sombras con el búfer de la galería de símbolos. La aplicación calcula los volúmenes de sombra que genera la geometría que tapa la luz; para ello, calcula los bordes de las siluetas y los extruye desde la luz hasta un conjunto de volúmenes 3D. Luego estos volúmenes se representan dos veces en el búfer de la galería de símbolos.

La primera representación dibuja polígonos orientados hacia el frente y aumenta los valores del búfer de la galería de símbolos. La segunda representación dibuja los polígonos orientados hacia atrás del volumen de sombra y reduce los valores del búfer de la galería de símbolos. Normalmente, todos los valores incrementados y reduce aumentados. Sin embargo, la escena ya estaba representada con la geometría normal, por lo que algunos píxeles no supera la prueba de búfer z a medida que se representa el volumen de sombra. Los valores que quedan en el búfer de la galería de símbolos corresponden a los píxeles que se encuentran en la sombra. Estos contenidos del búfer de la galería de símbolos que quedan se usan como máscara, para la combinación alfa de un cuadrado negro grande, que abarca todo en la escena. Con el búfer de la galería de símbolos como máscara, el resultado es oscurecer los píxeles que se encuentran en las sombras.

Esto significa que la geometría de las sombras se dibuja dos veces por fuente de luz, lo que ejerce presión en el rendimiento de vértices de la GPU. La característica de galería de símbolos a doble cara se ha diseñado para mitigar esta situación. En este enfoque, existen dos conjuntos de estado de la galería de símbolos (nombrados a continuación), un conjunto cada uno para los triángulos orientados al frente y otro para los triángulos orientados hacia atrás. De esta forma, solo se dibuja un paso por volumen de sombra y por luz.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Búferes de profundidad y de galerías de símbolos](depth-and-stencil-buffers.md)

 

 




