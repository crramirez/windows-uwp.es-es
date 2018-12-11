---
title: Filtrado de texturas con mapas MIP
description: Un mapa MIP es una secuencia de texturas, cada una de las cuales es una representación de la misma imagen con una resolución progresivamente inferior. El alto y ancho de cada imagen, o nivel, en el mapa MIP es una potencia de dos inferior al nivel anterior.
ms.assetid: 28E863A2-C776-40E4-8551-9851DF7EC93E
keywords:
- Filtrado de texturas con mapas MIP
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 474f97f32439c389be8283bb10e0c0ed716b3f69
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8875627"
---
# <a name="texture-filtering-with-mipmaps"></a>Filtrado de texturas con mapas MIP


Un *mapa MIP* es una secuencia de texturas, cada una de las cuales es una representación de la misma imagen con una resolución progresivamente inferior. El alto y ancho de cada imagen, o nivel, en el mapa MIP es una potencia de dos inferior al nivel anterior. Los mapas MIP no tienen que ser cuadrados.

Para los objetos que están cerca del usuario se usa una imagen de mapa MIP de alta resolución. Cuando el objeto aparece más alejado, se usan imágenes de menor resolución. Los mapas MIP mejoran la calidad de las texturas representadas a costa de usar más memoria.

Direct3D representa los mapas MIP como una cadena de superficies adjuntas. La textura de resolución más alta está al principio de la cadena y tiene el siguiente nivel del mapa MIP como adjunto. A su vez, ese nivel tiene un adjunto que es el siguiente nivel en del mapa MIP, y así sucesivamente, hasta el nivel de resolución más bajo del mapa MIP.

En las ilustraciones siguientes se muestra un ejemplo de estos niveles. Las texturas de mapas de bits representan una señal en un contenedor en un juego en primera persona en 3D. Cuando se crea como mapa MIP, la textura de resolución más alta es la primera del conjunto. Cada textura posterior del conjunto del mapa MIP es más pequeña en alto y ancho en una potencia de 2. En este caso, el mapa MIP de resolución máxima es de 256 x 256 píxeles. La siguiente textura es de 128 x 128. La última textura de la cadena es de 64 x 64.

Esta señal tiene una distancia máxima desde la que es visible. Si el usuario comienza lejos de la señal, el juego muestra la textura más pequeña de la cadena de mapa MIP, que en este caso es la textura de 64 x 64.

![Ilustración de una textura de 64 x 64 de una señal de peligro](images/mip1.jpg)

A medida que el usuario mueve el punto de vista más cerca de la señal, se usan texturas con una resolución cada vez mayor de la cadena de mapa MIP. La resolución de la siguiente ilustración es de 128 x 128.

![Ilustración de una textura de 128 x 128 de una señal de peligro](images/mip2.jpg)

La textura de mayor resolución se usa cuando el punto de vista del usuario está a la distancia mínima permitida de la señal, como se muestra en la siguiente ilustración.

![Ilustración de una textura de 256 x 256 de una señal de peligro](images/mip3.jpg)

Esta es una manera más eficaz de simular perspectivas para las texturas. En lugar de representar una única textura con varias resoluciones, es más rápido usar varias texturas con resoluciones variables.

Direct3D puede evaluar qué textura de un conjunto de mapas MIP es la solución más cercana a los resultados deseados y pueden asignar píxeles a su espacio de elementos de textura. Si la resolución de la imagen final se encuentra entre las resoluciones de las texturas en el conjunto de mapas MIP, Direct3D puede examinar los elementos de textura en ambos mapas MIP y combinar sus valores de color.

Para usar mapas MIP, la aplicación debe crear un conjunto de mapas MIP. Para aplicar mapas MIP, las aplicaciones seleccionan el mapa MIP establecido como primera textura en el conjunto de texturas actuales. Consulta [Combinación de texturas](texture-blending.md).

A continuación, la aplicación debe establecer el método de filtrado que Direct3D usará para el muestreo de los elementos de textura. El método más rápido de filtrado de mapas MIP es que Direct3D seleccione el elemento de textura más cercano. Usa el valor enumerado D3DTEXF\_POINT para seleccionarlo. Direct3D puede producir los mejores resultados de filtrado si la aplicación usa el valor enumerado D3DTEXF\_LINEAR. Esto selecciona el mapa MIP más cercano y, a continuación, calcula la media ponderada de los elementos de textura que rodean la ubicación en la textura a la que el píxel actual se asigna.

Las texturas de mapas MIP se usan en escenas en 3D para disminuir el tiempo necesario para representarlas. También mejoran el realismo de la escena. Sin embargo, a menudo requieren grandes cantidades de memoria.

**Nota**  cada superficie de una cadena de mapas MIP tiene dimensiones que son la mitad que la superficie anterior de la cadena. Si el mapa MIP de nivel superior tiene unas dimensiones de 256 x 128, las dimensiones del mapa MIP de segundo nivel son de 128 x 64, las del tercer nivel son de 64 x 32, y así sucesivamente hasta 1 x 1. No puedes solicitar un número de niveles de mapa MIP en niveles que haría que el ancho o el alto de cualquier mapa MIP de la cadena fuera inferior a 1. En el caso sencillo de una superficie de mapa MIP de nivel superior de 4 x 2, el valor máximo permitido para los niveles es tres. Las dimensiones del nivel superior son 4 x 2, las dimensiones del segundo nivel son 2 x 1 y las dimensiones del tercer nivel son 1 x 1. Un valor mayor de 3 en los niveles da como resultado un valor fraccional en la altura del mapa MIP del segundo nivel, por lo que no se permite.

 

Direct3D puede realizar automáticamente el filtrado de texturas de los mapas MIP. Las aplicaciones pueden recorrer manualmente una cadena de mapas MIP para cargar datos de mapa de bits en cada superficie de la cadena. Esta suele ser la única razón para recorrer la cadena. La generación automática de mapas MIP en el tiempo de creación de la textura aprovecha las ventajas del filtrado por hardware porque el mapa MIP reside en la memoria de vídeo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Filtrado de texturas](texture-filtering.md)

 

 




