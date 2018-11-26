---
title: Introducción a la iluminación
description: Cuando usas la iluminación de Direct3D, permites que Direct3D controle los detalles de iluminación en tu lugar. Los usuarios avanzados pueden realizar la iluminación ellos mismos, si lo prefieren.
ms.assetid: FCBF6A92-4EAC-4CCC-A76C-79985AF348AE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e90e460cf5f5bda7d90447440d76cf6898a83747
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7694994"
---
# <a name="lighting-overview"></a>Introducción a la iluminación

Cuando usas la iluminación de Direct3D, permites que Direct3D controle los detalles de iluminación en tu lugar. Los usuarios avanzados pueden realizar la iluminación ellos mismos, si lo prefieren.

Cuando se habilita la iluminación, Direct3D calcula el color de cada vértice de objeto en función de una combinación de los siguientes elementos:

-   Los elementos de textura de un mapa de texturas asociado.
-   Los colores de difusión especulares del vértice, si se especifican.
-   El color y la intensidad de la luz producida por las fuentes de luz en la escena o el nivel de luz de ambiente de la escena.

Cómo trabajas con la iluminación y los materiales marca la diferencia en la apariencia de la escena representada. Los materiales definen cómo se refleja la luz en una superficie. Los niveles de luz directa y de ambiente definen la luz que se refleja. Debes usar materiales para representar una escena si se habilita la iluminación.

No son necesarias las luces para representar una escena, pero no se ven los detalles de una escena representada sin luz. Como mucho, representar una escena sin luz da como resultado una silueta de los objetos de la escena. No es suficiente detalle para la mayoría de los casos.

## <a name="span-iddirectlightvsambientlightspanspan-iddirectlightvsambientlightspandirect-light-vs-ambient-light"></a><span id="direct_light_vs._ambient_light"></span><span id="DIRECT_LIGHT_VS._AMBIENT_LIGHT"></span>Luz directa frente a luz de ambiente


Aunque la luz directa y la de ambiente iluminan los objetos de una escena, son independientes la una de la otra, tienen efectos muy diferentes y necesitan que trabajes con ellas de formas totalmente distintas.

*La luz directa* golpea el objeto directamente. La luz directa siempre tiene dirección y color y es un factor para los algoritmos de sombreado, como el sombreado Gouraud. Diferentes tipos de luz emiten luz directa de maneras diferentes y crear efectos de atenuación especiales.

*La luz de ambiente* es eficaz en cualquier parte de una escena. La luz de ambiente es un nivel general de luz que llena toda una escena, independientemente de los objetos y de sus ubicaciones en la escena. La luz de ambiente no tiene posición o dirección, solo color e intensidad. Cada luz se agrega a la luz de ambiente global de una escena.

El color de la luz de ambiente adopta la forma de un valor RGBA, donde cada componente es un valor entero de 0 a 255. Esto es diferente a la mayoría de valores de color de Direct3D.

Los componentes de rojo, verde y azul se combinan para hacer el color final de la luz de ambiente. El componente alfa controla la transparencia del color. Al usar la aceleración de hardware o la emulación RGB, se omite el componente alfa.

## <a name="span-iddirect3dlightmodelvsnaturespanspan-iddirect3dlightmodelvsnaturespandirect3d-light-model-vs-nature"></a><span id="direct3d_light_model_vs._nature"></span><span id="DIRECT3D_LIGHT_MODEL_VS._NATURE"></span>Modelo de luz de Direct3D frente a la naturaleza


En la naturaleza, cuando una fuente emite luz, se refleja en cientos, miles o millones de objetos antes de llegar a la vista del usuario. Cada vez que se refleja, una superficie absorbe algo de luz, se distribuye luz en direcciones aleatorias y el resto va a otra superficie o a la vista del usuario. Este proceso continúa hasta que la luz se reduce a nada o un usuario la percibe.

Obviamente, los cálculos necesarios para simular perfectamente el comportamiento natural de la luz consumen demasiado tiempo para usarlos en los gráficos de Direct3D en tiempo real. Por lo tanto, con la velocidad en mente, el modelo de luz de Direct3D se aproxima a cómo funciona luz en el mundo natural. Direct3D describe la luz en componentes de rojo, verde y azul que se combinan para crear un color final.

En Direct3D, cuando la luz se refleja en una superficie, el color de la luz interactúa matemáticamente con la propia superficie para crear el color que finalmente se muestra en la pantalla. Para obtener información específica sobre los algoritmos que usa Direct3D, consulta [Cálculos de iluminación](mathematics-of-lighting.md).

El modelo de luz de Direct3D generaliza la luz en dos tipos: luz de ambiente y luz directa. Cada una tiene diferentes atributos y cada una interactúa con el material de una superficie de manera diferente. La luz de ambiente es luz que se distribuyó tanto, que su dirección y su origen son indeterminados: mantiene un nivel bajo de intensidad en todas partes. La iluminación indirecta que usan los fotógrafos es un buen ejemplo de la luz de ambiente.

La luz de ambiente en Direct3D, como en la naturaleza, no tiene dirección ni origen real, solo color e intensidad. De hecho, el nivel de luz de ambiente es completamente independiente de los objetos de una escena que generan luz. La luz de ambiente no contribuye al reflejo especular.

La luz directa es la luz que genera una fuente en una escena, siempre tiene color e intensidad y viaja en una dirección concreta. La luz directa interactúa con el material de una superficie para crear resaltados especulares y su dirección se usa como factor en algoritmos de sombreado, incluido el sombreado Gouraud. Cuando se refleja la luz directa, no contribuye al nivel de luz de ambiente de una escena. Las fuentes de una escena que generan luz directa tienen diferentes características que afectan a cómo iluminan una escena.

Además, el material de un polígono tiene propiedades que afectan a cómo ese polígono refleja la luz que recibe. Estableces un solo rasgo de reflectancia que describe cómo refleja el material la luz de ambiente y estableces rasgos individuales para determinar la reflectancia especular y difusa del material.

## <a name="span-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspanspan-idcolorvaluesforlightsandmaterialsspancolor-values-for-lights-and-materials"></a><span id="Color_Values_for_Lights_and_Materials"></span><span id="color_values_for_lights_and_materials"></span><span id="COLOR_VALUES_FOR_LIGHTS_AND_MATERIALS"></span>Valores de colores para luces y materiales


Direct3D describe el color como cuatro componentes (rojo, verde, azul y alfa) que se combinan para crear un color final. Cada componente oscila entre 0,0 y 1,0. Aunque las luces y los materiales usan la misma estructura para describir el color, las luces usan los valores de un modo ligeramente distinto al de los materiales.

Los valores de colores de las fuentes de luz representan la cantidad que emite un componente de luz concreto. Dado que las luces no usan un componente alfa, solo los componentes de rojo, verde y azul del color son pertinentes. Puedes visualizar los tres componentes como las lentes de rojo, verde y azul de una televisión de proyección. Cada lente puede estar desactivada (un valor de 0,0 en el miembro correspondiente), puede tener el máximo brillo posible (un valor de 1,0) o puede que esté en algún punto entre ambos valores.

Los colores que llegan a través de las lentes se combinan para crear el color final de la luz. Una combinación como R(1,0), G(1,0), B(1,0) crea una luz blanca, mientras que B(0,0) R(0,0), G(0,0), no emite ninguna luz. Puedes hacer una luz que emita un solo componente, lo que provocaría una luz roja, verde o azul pura; o bien, la luz podría usar combinaciones para emitir colores como amarillo o púrpura. Puedes incluso establecer valores del componente de color negativos para crear una "luz oscura" que quite luz de una escena. O bien, puede establecer los componentes en un valor mayor que 1,0 para crear una luz muy brillante.

Por otro lado, con los materiales, los valores de colores representan la cantidad de un componente de luz que refleja una superficie representada con ese material. Un material cuyos componentes de color son R(1,0), G(1,0), B(1,0), A(1,0) refleja toda la luz que le llega. De igual modo, un material con R(0,0), G(1,0), B(0,0), A(1,0) refleja toda la luz verde que se dirige a él. Los materiales tienen varios valores de reflectancia para crear varios tipos de efectos.

Consulta [Tipos de luz](light-types.md) y [Propiedades de luz](light-properties.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Luces y materiales](lights-and-materials.md)

 

 




