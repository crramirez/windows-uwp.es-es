---
title: Ajuste de texturas
description: El ajuste de texturas cambia la forma básica en la que Direct3D rasteriza los polígonos texturizados mediante las coordenadas de textura especificadas para cada vértice.
ms.assetid: C28FB369-9A91-4D57-A96D-4A5D36484B35
keywords:
- Ajuste de texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6618b7573be7cd39f703299b9418d1575297120e
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8732609"
---
# <a name="texture-wrapping"></a>Ajuste de texturas


El ajuste de texturas cambia la forma básica en la que Direct3D rasteriza los polígonos texturizados mediante las coordenadas de textura especificadas para cada vértice. Durante la rasterización de un polígono, el sistema interpola entre las coordenadas de textura en cada uno de los vértices del polígono para determinar los elementos de textura que deberían usarse para cada píxel del polígono. Normalmente, el sistema trata la textura como un plano en 2D e interpola nuevos elementos de textura, para lo que toma la ruta más corta desde el punto A dentro de una textura al punto B. Si el punto A representa la posición u, v (0.8,0.1) y el punto B se encuentra en (0.1,0.1), la línea de interpolación es similar al siguiente diagrama.

![Diagrama de una línea de la interpolación entre dos puntos](images/interp1.png)

Ten en cuenta que la distancia más corta entre A y B en la siguiente ilustración discurre aproximadamente por el medio de la textura. Si se habilita el ajuste de las coordenadas de la textura u o la textura v, cambia el modo en que Direct3D percibe la ruta más corta entre las coordenadas de textura en la dirección u y la dirección v. Por definición, el ajuste de textura causa que el rasterizador tome la ruta más corta entre conjuntos de coordenadas de textura, y asume que 0.0 y 1.0 son coincidentes. El último bit es la parte complicada: puedes imaginar que habilitar el ajuste de textura en una dirección hace que el sistema trate una textura como si se ajustara alrededor de un cilindro. Por ejemplo, piensa en el siguiente diagrama.

![Diagrama de una textura y dos puntos ajustados alrededor de un cilindro](images/interp2.png)

La ilustración anterior muestra cómo el ajuste de la dirección u afecta al modo en que el sistema interpola coordenadas de textura. Si se usan los mismos puntos que en el ejemplo de texturas normales o sin ajustar, puedes ver que la ruta más corta entre los puntos A y B ya no está en medio de la textura, sino que ahora cruza el borde donde coexisten 0.0 y 1.0. El ajuste en la dirección v es similar, salvo que ajusta la textura alrededor de un cilindro que se apoya sobre un lado. El ajuste en la dirección u y la dirección v es más complejo. En esta situación, puedes concebir la textura como un anillo o una rosquilla.

La aplicación práctica más habitual para el ajuste de texturas es realizar la asignación del entorno. Normalmente, un objeto texturizado con una asignación de entorno parece muy reflectante y muestra una imagen reflejada de lo que rodea al objeto en la escena. Para los fines de este artículo, imagina una sala con cuatro paredes, cada una de ellas pintada con una letra (R, G, B e Y) y con los colores correspondientes (rojo, verde, azul y amarillo). La asignación de entorno para dicha habitación sencilla podría parecerse a la siguiente ilustración.

![Ilustración de bandas verticales de rojo, verde, azul y amarillo](images/envmap.png)

Imagina que el techo de la habitación está sostenido por una columna perfectamente reflectante de cuatro lados. Asignar la textura de la asignación de entorno a la columna es muy sencillo, pero hacer que dicha columna parezca que refleje las letras y los colores no es tan fácil. El siguiente diagrama muestra un marco de conexiones de la columna con las coordenadas de textura aplicables indicadas cerca de los vértices superiores. El límite donde el ajuste cruzará los bordes de la textura se muestra con una línea de puntos.

![Diagrama de un rectángulo con una línea de puntos bisectriz](images/seam.png)

Con el ajuste habilitado en la dirección u, la columna texturizada muestra los colores y los símbolos de la asignación de entorno correctamente y, en el límite de la parte delantera de la textura, el rasterizador elige correctamente la ruta más corta entre las coordenadas de textura, asumiendo que las coordenadas 0.0 y 1.0 comparten la misma ubicación. La columna texturizada es similar a la siguiente ilustración.

![Ilustración de una columna que consta de cuadrantes rojos, verdes, azules y amarillos](images/tex-seam.png)

Si el ajuste de texturas no está habilitado, el rasterizador no interpola en la dirección necesaria para generar una imagen reflejada creíble. En su lugar, el área situada en la parte delantera de la columna contiene una versión comprimida en horizontal de los elementos de textura entre las coordenadas u 0.175 y 0.875, cuando pasan a través del centro de la textura. El efecto del ajuste se arruina.

No confundas el ajuste de texturas con los modos de direccionamiento de las texturas con nombre similar. El ajuste de texturas se realiza antes del direccionamiento de las texturas. Asegúrate de que el ajuste de los datos de la textura no contiene coordenadas de textura fuera del intervalo \[0.0, 1.0\], ya esto generará resultados indefinidos. Para obtener más información acerca de direccionamiento de las texturas, consulta [Modos de direccionamiento de las texturas](texture-addressing-modes.md).

## <a name="span-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspanspan-iddisplacementmapwrappingspandisplacement-map-wrapping"></a><span id="Displacement_Map_Wrapping"></span><span id="displacement_map_wrapping"></span><span id="DISPLACEMENT_MAP_WRAPPING"></span>Ajuste de la asignación de desplazamiento


Las asignaciones de desplazamiento las interpola el motor de teselación. Como el modo de ajuste no se puede especificar para el motor de teselación, el ajuste de texturas no se puede realizar con asignaciones de desplazamiento. Una aplicación puede usar un conjunto de vértices obligue a la interpolación a ajustar en cualquier dirección. La aplicación también puede especificar que la interpolación se realice como una interpolación lineal simple.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




