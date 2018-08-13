---
title: Ventanillas y recortes
description: Una ventanilla es un rectángulo bidimensional (2D) en el que se proyecta una escena 3D.
ms.assetid: D0DD646E-13AE-452A-AD22-8C35000D0BA9
keywords:
- Ventanillas y recortes
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: defea1e9adbb4d0f9edb24c936069191944b94be
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044724"
---
# <a name="viewports-and-clipping"></a>Ventanillas y recortes


Una *ventanilla* es un rectángulo bidimensional (2D) en el que se proyecta una escena 3D. En Direct3D, el rectángulo existe como coordenadas dentro de una superficie de Direct3D que el sistema usa como destino de representación. La transformación de proyección convierte los vértices en el sistema de coordenadas usado para la ventanilla. Una ventanilla también se usa para especificar el intervalo de valores de profundidad en una superficie de destino de representación en la que se representará una escena (normalmente 0,0 a 1,0).

## <a name="span-idtheviewingfrustumspanspan-idtheviewingfrustumspanspan-idtheviewingfrustumspanthe-viewing-frustum"></a><span id="The_Viewing_Frustum"></span><span id="the_viewing_frustum"></span><span id="THE_VIEWING_FRUSTUM"></span>El tronco de visualización


Un tronco de visualización es el volumen 3D en una escena colocada en relación con la cámara de la ventanilla. La forma del volumen afecta a cómo se proyectan los modelos desde el espacio de la cámara a la pantalla. El tipo más común de proyección, una proyección de perspectiva, es responsable de hacer que los objetos cerca de la cámara parezcan mayores que los objetos en la distancia. Para ver el punto de vista, se puede visualizar el tronco de visualización como una pirámide, con la cámara situada en la punta tal como se muestra en la ilustración siguiente. Esta pirámide está cruzada por un plano de recorte frontal y trasero. El volumen dentro de la pirámide entre los planos de recorte frontal y trasero es el tronco de visualización. Los objetos están visibles solo cuando están en este volumen.

![ilustración de un tronco de visualización con un plano de recorte frontal y trasero](images/frustum.png)

Si te imaginas que estás de pie en una habitación oscura y mirando por una ventana cuadrada, estás visualizando un tronco de visualización. En esta analogía, el plano de recorte cercano es la ventana y el plano de recorte trasero es lo que finalmente interrumpe la vista: el rascacielos al otro lado de la calle, las montañas en la distancia o nada en absoluto. Puedes ver todo el contenido dentro de la pirámide truncada que comienza en la ventana y termina con cualquier cosa que interrumpa la vista y no podrás ver nada más.

El tronco de visualización está definido por fov (campo de visión) y por las distancias de los planos de recorte de la parte delantera y trasera, especificado en coordenadas -z, tal como se muestra en el siguiente diagrama.

![diagrama del tronco de visualización](images/fovdiag.png)

En este diagrama, la variable D es la distancia desde la cámara al origen del espacio que se ha definido en la última parte de la canalización de geometría: la transformación de visualización. Este es el espacio alrededor del cual se organizan los límites de tu tronco de visualización. Para obtener información sobre cómo se utiliza esta variable D para crear la matriz de proyección, consulta la [transformación de proyección](projection-transform.md)

## <a name="span-idviewportrectanglespanspan-idviewportrectanglespanspan-idviewportrectanglespanviewport-rectangle"></a><span id="Viewport_Rectangle"></span><span id="viewport_rectangle"></span><span id="VIEWPORT_RECTANGLE"></span>Rectángulo de ventanilla


Una estructura de ventanilla contiene cuatro miembros (X, Y, ancho, alto) que definen el área de la superficie de destino de representación en la que se va a representar una escena. Estos valores corresponden al rectángulo de destino, o el rectángulo de ventanilla, como se muestra en el siguiente diagrama.

![diagrama del rectángulo de ventanilla](images/destrect.png)

Los valores especificados para los miembros X, Y, ancho y alto son las coordenadas de pantalla con respecto a la esquina superior izquierda de la superficie de destino de la representación. La estructura define a dos miembros adicionales (MinZ y MaxZ) que indican los intervalos de profundidad en los que se va a representar la escena.

Direct3D asume que los intervalos del volumen de recorte de ventanilla son entre -1,0 y 1,0 en X y entre 1.0 y -1.0 en Y. Estos eran los valores usados con más frecuencia por las aplicaciones en el pasado. Puedes ajustar la relación de aspecto de la ventanilla antes de recortar usando la [transformación de proyección](projection-transform.md).

**Nota** MinZ y MaxZ indican los intervalos de profundidad en los que se va a representar la escena y no se usan para el recorte. La mayoría de las aplicaciones establecen estos valores a 0.0 y 1.0, para permitir que el sistema represente todo el rango de valores de profundidad en el búfer de profundidad. En algunos casos, puedes lograr efectos especiales mediante el uso de otros intervalos de profundidad. Por ejemplo, para representar una pantalla de visualización frontal en un juego, puedes establecer los valores en 0.0 para forzar al sistema a representar objetos en una escena en primer plano, o puedes establecerlos en 1.0 para representar un objeto que debería estar siempre en segundo plano.

 

La dimensión que se usa en los miembros X, Y, ancho, alto de una estructura de ventanilla definen la ubicación y las dimensiones de la ventanilla en la superficie de destino de representación. Estos valores están en coordenadas de pantalla, con respecto a la esquina superior izquierda de la superficie.

Direct3D usa la ubicación de la ventanilla y las dimensiones para escalar los vértices de forma que se ajusten a una escena representada en la ubicación adecuada en la superficie de destino. Internamente, Direct3D inserta estos valores en la siguiente matriz que se aplica a cada vértice.

![ecuación de la matriz que se aplica a cada vértice](images/vpscale.png)

Esta matriz escala vértices según las dimensiones de la ventanilla y el intervalo de profundidad deseado y los traslada a la ubicación adecuada en la superficie de destino de representación. La matriz también cambia la coordenada -y para reflejar el origen de la pantalla en la esquina superior izquierda con el aumento de y hacia abajo. Después de aplicar esta matriz, los vértices siguen siendo homogéneos, es decir, siguen existiendo como vértices \[x,y,z,w\], y se deben convertir en coordenadas no homogéneas antes de ser enviadas al rasterizador.

**Nota**   Las aplicaciones normalmente establecen MinZ y MaxZ en 0.0 y 1 respectivamente para hacer que el sistema represente el intervalo de profundidad completo. Sin embargo, puedes usar otros valores para lograr algunos efectos. Por ejemplo, puedes establecer ambos valores en 0.0 para forzar a todos los objetos en primer plano, o establecer ambos en 1.0 para representar todos los objetos en segundo plano.

 

## <a name="span-idclearingaviewportspanspan-idclearingaviewportspanspan-idclearingaviewportspanclearing-a-viewport"></a><span id="Clearing_a_Viewport"></span><span id="clearing_a_viewport"></span><span id="CLEARING_A_VIEWPORT"></span>Borrar una ventanilla


Borrar la ventanilla restablece el contenido del rectángulo de la ventanilla en la superficie de destino de representación. También puede borrar el rectángulo en las superficies de búfer de galería de símbolos y profundidad.

## <a name="span-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanspan-idsetuptheviewportforclippingspanset-up-the-viewport-for-clipping"></a><span id="Set_Up_the_Viewport_for_Clipping"></span><span id="set_up_the_viewport_for_clipping"></span><span id="SET_UP_THE_VIEWPORT_FOR_CLIPPING"></span>Configurar la ventanilla para recortes


Los resultados de la matriz de proyección determinan el volumen de recorte en el espacio de proyección como:

-w<sub>c</sub>&lt;= x<sub>c</sub>&lt;= w<sub>c</sub>

-w<sub>c</sub>&lt;= y<sub>c</sub>&lt;= w<sub>c</sub>

0 &lt;= z<sub>c</sub>&lt;= w<sub>c</sub>

Donde x, y, z y w representan las coordenadas de vértices después de aplicar la transformación de proyección. Se recortan los vértices que tienen componentes- x, -y o z fuera de estos intervalos, si el recorte está habilitado (comportamiento predeterminado).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




