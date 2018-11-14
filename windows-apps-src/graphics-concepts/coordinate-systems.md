---
title: Sistemas de coordenadas
description: 'Por lo general, las aplicaciones de gráficos 3D usan uno de dos tipos de sistemas de coordenadas cartesianas: diestro o zurdo. En ambos sistemas de coordenadas, el eje x positivo apunta a la derecha y el eje y positivo apunta arriba.'
ms.assetid: 138D9B81-146F-4E9F-B742-1EDED8FBF2AE
keywords:
- Sistemas de coordenadas
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1b8faed0719419d4be8ac1e5d493610ec2660598
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6275920"
---
# <a name="coordinate-systems"></a>Sistemas de coordenadas


Por lo general, las aplicaciones de gráficos 3D usan uno de dos tipos de sistemas de coordenadas cartesianas: diestro o zurdo. En ambos sistemas de coordenadas, el eje x positivo apunta a la derecha y el eje y positivo apunta arriba.

## <a name="span-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanspan-idleftandrighthandedcoordinatesspanleft-and-right-handed-coordinates"></a><span id="Left_and_right_handed_coordinates"></span><span id="left_and_right_handed_coordinates"></span><span id="LEFT_AND_RIGHT_HANDED_COORDINATES"></span>Coordenadas zurdas y diestras


Para recordar en qué dirección apunta el eje z positivo, apunta con los dedos de la mano izquierda o derecha en la dirección x positiva y dóblalos en la dirección y positiva. La dirección a la que apunta el pulgar (acercándose o alejándose de ti) es la dirección que el eje z positivo apunta para ese sistema de coordenadas. La siguiente ilustración muestra estos dos sistemas de coordenadas.

![ilustración de los sistemas de coordenadas cartesianas zurdo y diestro](images/leftrght.png)

Direct3D usa un sistema de coordenadas zurdo. Aunque los sistemas de coordenadas zurdo y diestro son los más comunes, hay una variedad de otros sistemas de coordenadas que se usan en software 3D. Por ejemplo, no es raro que las aplicaciones de modelado 3D usen un sistema de coordenadas en el que el eje y se acerque o se aleje del espectador y el eje z apunte hacia arriba.

## <a name="span-idverticesandvectorsspanspan-idverticesandvectorsspanspan-idverticesandvectorsspanvertices-and-vectors"></a><span id="Vertices_and_vectors"></span><span id="vertices_and_vectors"></span><span id="VERTICES_AND_VECTORS"></span>Vértices y vectores


Dado el sistema de coordenadas, las coordenadas x, y, z pueden definir un punto en el espacio (un "vértice") o una dirección 3D (un "vector").

Puede usarse una colección de vértices para definir líneas y formas. Los objetos más simples que pueden definirse mediante vértices se denominan [primitivos](primitives.md), y un objeto más complejo definido por un conjunto de primitivos se denomina "malla".

Las operaciones esenciales que se realizan en las mallas definidas en un sistema de coordenadas 3D son la traslación, la rotación y el escalado. Puedes combinar estas transformaciones básicas para crear una matriz de transformación. Para obtener más información, consulta [Transformaciones](transforms.md).

Cuando se combinan estas operaciones, los resultados no son conmutativos; el orden en el que se multiplican las matrices es importante.

## <a name="span-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanspan-idportingfromaright-handedcoordinatesystemspanporting-from-a-right-handed-coordinate-system"></a><span id="Porting_from_a_right-handed_coordinate_system"></span><span id="porting_from_a_right-handed_coordinate_system"></span><span id="PORTING_FROM_A_RIGHT-HANDED_COORDINATE_SYSTEM"></span>Migrar de un sistema de coordenadas diestro


Si vas a migrar una aplicación que se basa en un sistema de coordenadas diestro, debes hacer dos cambios en los datos que se pasan a Direct3D:

-   Invierte el orden de los vértices del triángulo, de modo que el sistema los recorra en el sentido de las agujas del reloj desde el frente. En otras palabras, si los vértices son v0, v1, v2, pásalos a Direct3D como v0 v2, v1.
-   Usa la matriz de vista para escalar el espacio global por -1 en la dirección z. Para ello, invierte el signo del miembro \_31, \_32, \_33 y \_34 de la estructura matricial que usas para la matriz de vista.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




