---
title: Propiedades de luz
description: Las propiedades de la luz describen el tipo de la fuente de luz (puntual, direccional o foco), la atenuación, el color, la dirección, la posición y el intervalo.
ms.assetid: E832C3FD-9921-41C4-87B8-056E16B61B77
keywords:
- Propiedades de luz
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 86b8627461251a5d43762facc18c8a414a117fc9
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7695295"
---
# <a name="light-properties"></a>Propiedades de luz


Las propiedades de la luz describen el tipo de la fuente de luz (puntual, direccional o foco), la atenuación, el color, la dirección, la posición y el intervalo. Según el tipo de luz que se usa, una luz puede tener propiedades de atenuación e intervalo o efectos de resaltado. No todos los tipos de luces usan todas las propiedades.

Las propiedades de posición, intervalo y atenuación definen la ubicación de la luz en el espacio global y cómo se comporta la luz que emite con la distancia.

## <a name="span-idlightattenuationspanspan-idlightattenuationspanspan-idlightattenuationspanlight-attenuation"></a><span id="Light_Attenuation"></span><span id="light_attenuation"></span><span id="LIGHT_ATTENUATION"></span>Atenuación de la luz


La atenuación controla cómo disminuye la intensidad de la luz hacia la máxima distancia especificada por la propiedad de intervalo. A veces, se usan tres valores de punto flotante para representar la atenuación de la luz: Attenuation0, Attenuation1 y Attenuation2. Estos valores de punto flotante comprenden entre 0,0 y infinito y controlan la atenuación de la luz. Algunas aplicaciones establecen el miembro Attenuation1 en 1,0 y los demás, en 0,0, lo que resulta en un cambio en la intensidad de la luz de 1 / D, donde D es la distancia desde la fuente de luz hasta el vértice. La intensidad de la luz máxima está en el origen y disminuye en 1 / (intervalo de luz) en el intervalo de la luz.

Aunque, por lo general, una aplicación establece Attenuation0 en 0,0, Attenuation1 en un valor de constante y Attenuation2 en 0,0, de modo que se pueden lograr varios efectos de luz si se modifica esto. Puedes combinar los valores de atenuación para obtener efectos de atenuación más complejos. O bien, puedes establecerlos en valores fuera del intervalo normal para crear efectos de atenuación más extraños. Sin embargo, no se permiten los valores de atenuación negativos. Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md).

## <a name="span-idlightcolorspanspan-idlightcolorspanspan-idlightcolorspanlight-color"></a><span id="Light_Color"></span><span id="light_color"></span><span id="LIGHT_COLOR"></span>Color de la luz


Las luces en Direct3D emiten tres colores que se usan de forma independiente en los cálculos de iluminación del sistema: un color difuso, un color de ambiente y un color especular. El módulo de iluminación de Direct3D los incorpora e interactúa con un equivalente del material actual, para producir un color final que se usa en la representación. El color difuso interactúa con la propiedad de reflectancia difusa del material actual y el color especular, con la propiedad de reflectancia especular del material, etc. Para obtener información específica sobre cómo aplica Direct3D estos colores, consulta [Cálculos de iluminación](mathematics-of-lighting.md).

En una aplicación de Direct3D, normalmente hay tres valores de colores (difuso, de ambiente y especular) que definen el color que se emite.

El tipo de color que afecta más a los cálculos del sistema es el color difuso. El color difuso más común es el blanco (R:1,0 G:1,0 B:1,0), pero puedes crear colores según necesites para lograr los efectos que quieres. Por ejemplo, puedes usar luz roja para una chimenea o puedes usar luz verde para una señal de tráfico, como un semáforo en verde.

Por lo general, estableces los componentes de colores de luz en valores entre 0,0 y 1,0, incluidos, pero no es un requisito. Por ejemplo, puedes establecer todos los componentes en 2,0 para crear una luz "más brillante que el blanco". Este tipo de configuración puede ser especialmente útil cuando usas una configuración de atenuación que no sea constante.

Aunque Direct3D usa valores RGBA para las luces, no se usa el componente de color alfa.

Por lo general, se usan colores de materiales para la iluminación. Sin embargo, puedes especificar que los colores de vértices difusos o especulares invaliden los colores de los materiales (de emisión, de ambiente, difusos y especulares).

El valor alfa o transparencia siempre proviene solo del canal alfa del color difuso.

El valor de niebla siempre proviene solo del canal alfa del color especular.

## <a name="span-idlightdirectionspanspan-idlightdirectionspanspan-idlightdirectionspanlight-direction"></a><span id="Light_Direction"></span><span id="light_direction"></span><span id="LIGHT_DIRECTION"></span>Dirección de la luz


La propiedad de dirección de la luz determina la dirección en la que se desplaza la luz que emite el objeto, en el espacio global. Solo las luces direccionales y los focos de luz usan la dirección, que se describe con un vector.

Establece la dirección de la luz como un vector. Los vectores de dirección se describen como distancias desde un origen lógico, independientemente de la posición de la luz de una escena. Por lo tanto, un foco de luz que apunta directamente a una escena (a lo largo del eje Z positivo) tiene un vector de dirección de &lt;0,0,1&gt; independientemente de dónde se defina su posición. Del mismo modo, puedes simular la luz del sol brillando directamente en una escena con una luz direccional cuya dirección sea &lt;0,-1,0&gt;. No tienes que crear luces que brillen a lo largo de los ejes de coordenadas: puedes combinar y hacer coincidir valores para crear luces que brillen en ángulos más interesantes.

Aunque no es necesario normalizar un vector de dirección de la luz, asegúrate siempre de que tiene magnitud. En otras palabras, no uses un vector de dirección &lt;0,0,0&gt;.

## <a name="span-idlightpositionspanspan-idlightpositionspanspan-idlightpositionspanlight-position"></a><span id="Light_Position"></span><span id="light_position"></span><span id="LIGHT_POSITION"></span>Posición de la luz


La posición de la luz se describe con una estructura de vectores. Se supone que las coordenadas X, Y y Z están en un espacio global. Las luces direccionales son el único tipo de luz que no usa la propiedad de posición.

## <a name="span-idlightrangespanspan-idlightrangespanspan-idlightrangespanlight-range"></a><span id="Light_Range"></span><span id="light_range"></span><span id="LIGHT_RANGE"></span>Intervalo de luz


La propiedad de intervalo de la luz determina la distancia, en el espacio global, a la que las mallas de una escena dejan de recibir la luz que emite el objeto. Las luces direccionales no usan la propiedad de intervalo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Luces y materiales](lights-and-materials.md)

 

 




