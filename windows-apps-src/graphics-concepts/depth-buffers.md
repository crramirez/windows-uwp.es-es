---
title: Búferes de profundidad
description: Un búfer de profundidad, o búfer Z, almacena la información de profundidad para controlar qué áreas de polígonos se representan y no se ocultan de la vista.
ms.assetid: BE83A1D7-D43D-4013-8817-EFD2B24DC58E
keywords:
- Búferes de profundidad
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fb7ff4b23f9735347ce75e2e565c1b420ec936d6
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7565732"
---
# <a name="depth-buffers"></a>Búferes de profundidad


Un *búfer de profundidad*, o *búfer Z*, almacena la información de profundidad para controlar qué áreas de polígonos se representan y no se ocultan de la vista.

## <a name="span-idoverviewspanspan-idoverviewspanspan-idoverviewspanoverview"></a><span id="Overview"></span><span id="overview"></span><span id="OVERVIEW"></span>Introducción


Un búfer de profundidad, a menudo llamado búfer Z o búfer W, es una propiedad del dispositivo que almacena la información de profundidad que debe usar Direct3D. Cuando Direct3D representa una escena en una superficie de destino, puede usar la memoria en una superficie de búfer de profundidad asociada, como un espacio de trabajo, para determinar cómo los píxeles de polígonos rasterizados se tapan entre sí. Direct3D usa una superficie de Direct3D fuera de la pantalla como destino a la que se escriben los valores de color finales. La superficie del búfer de profundidad que está asociada a la superficie de destino de representación se usa para almacenar información de profundidad que indica a Direct3D en qué profundidad está cada píxel en la escena.

Cuando una escena se rasteriza con el búfer de profundidad habilitado, se prueba cada punto de la superficie de representación. Los valores del búfer de profundidad pueden ser una coordenada Z de un punto o su coordenada W homogénea, desde la ubicación del punto (x,y,z,w) en el espacio de proyección. Un búfer de profundidad que usa valores Z se suele denominar búfer Z, y uno que usa valores W se llama búfer W. Cada tipo de búfer de profundidad tiene sus ventajas y desventajas, que se describen más adelante.

Al principio de la prueba, se establece el valor de profundidad del búfer de profundidad en el mayor valor posible de la escena. El valor de color de la superficie de representación se establece en el valor de color de fondo o el valor de color de la textura de fondo en ese momento. Cada polígono de la escena se prueba para ver si forma una intersección con la coordenada actual (x,y) en la superficie de representación.

Si no forma ninguna, el valor de profundidad (que será la coordenada Z en un búfer Z, y la coordenada W en un búfer de W) del punto actual se prueba para ver si es menor que el valor de profundidad almacenado en el búfer de profundidad. Si la profundidad del valor del polígono es menor, se almacena en el búfer de profundidad y el valor de color del polígono se escribe en el punto actual de la superficie de representación. Si el valor de profundidad del polígono en ese punto es mayor, se prueba el polígono siguiente de la lista. Este proceso se muestra en el siguiente diagrama.

![diagrama de valores de profundidad de prueba](images/zbuffer.png)

## <a name="span-idbufferingtechniquesspanspan-idbufferingtechniquesspanspan-idbufferingtechniquesspanbuffering-techniques"></a><span id="Buffering_techniques"></span><span id="buffering_techniques"></span><span id="BUFFERING_TECHNIQUES"></span>Técnicas de búfer


Aunque la mayoría de las aplicaciones no usan esta característica, puedes cambiar la comparación que Direct3D usa para determinar qué valores se colocan en el búfer de profundidad y, posteriormente, en la superficie de destino de representación. En determinado hardware, cambiar la función de comparación puede deshabilitar las pruebas z jerárquicas.

Casi todos los aceleradores del mercado admiten el búfer Z, lo que convierte a los búferes Z en el tipo más común de búfer de profundidad actualmente. Aunque estén en todas partes, los búferes Z presentan ciertas desventajas. Debido a las operaciones matemáticas implicadas, los valores Z generados en un búfer Z tienden a no distribuirse de forma uniforme en todo el intervalo del búfer Z (normalmente de 0,0 a 1,0, ambos inclusive).

En concreto, la relación entre los planos de recorte cercano y alejado afecta encarecidamente a la forma desigual en que están distribuidos los valores Z. Con una relación de 100 entre la distancia de plano alejado y la distancia de plano cercano, se emplea un 90 por ciento del intervalo de búfer de profundidad en el primer 10 por ciento del intervalo de profundidad de la escena. Las aplicaciones típicas de entretenimiento o simulaciones visuales con escenas exteriores a menudo requieren relaciones de planos cercanos o lejanos de cualquier sitio entre 1000 y 10000. En una relación de 1000, se emplea un 98 por ciento del intervalo en el primer 2 por ciento del intervalo de profundidad, y la distribución aún es peor con relaciones superiores. Esto puede generar defectos de la superficie ocultos en objetos distantes, especialmente cuando se usan búferes de profundidad de 16 bits, la profundidad de bits que más se admite habitualmente.

Un búfer de profundidad basado en W, por otro lado, suele distribuirse más equitativamente entre los planos de recorte cercano y alejado que un búfer Z. La principal ventaja es que la relación de las distancias de los planos de recorte alejado y cercano ya no resulta un problema. Esto permite que las aplicaciones admitan grandes intervalos máximos, a la vez que siguen obteniendo un búfer de profundidad relativamente preciso cerca del punto ocular. Un búfer de profundidad basado en W no es perfecto y, a veces, puede mostrar defectos de la superficie ocultos en objetos cercanos. Otro inconveniente del enfoque de búfer W está relacionado con la compatibilidad de hardware: el búfer W no se admite tanto en hardware como el búfer Z.

El uso de un búfer Z requiere una sobrecarga durante la representación. Pueden usarse varias técnicas para optimizar la representación al usar búferes Z. Las aplicaciones pueden aumentar el rendimiento al usar el búfer Z y las texturas asegurándose de que las escenas se representan de delante hacia atrás. Los primitivos con búfer Z con textura se prueban previamente con el búfer Z en base a una línea de digitalización. Si un polígono representado anteriormente oculta una línea de digitalización, el sistema lo rechaza de forma rápida y eficaz. El búfer Z puede mejorar el rendimiento, pero la técnica resulta más útil cuando una escena dibuja los mismos píxeles más de una vez. Es difícil de calcular exactamente, pero a menudo puedes hacer una aproximación cercana. Si los mismos píxeles se dibujan menos de dos veces, para lograr el mejor rendimiento, activa el búfer Z desactivado y representa la escena desde la parte posterior a la frontal.

La interpretación real de un valor de profundidad es específica del representador.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Búferes de profundidad y de galerías de símbolos](depth-and-stencil-buffers.md)

 

 




