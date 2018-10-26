---
title: Introducción a los búferes
description: Un recurso de búfer es una colección de datos completos, agrupados en elementos.
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- Introducción a los búferes
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 14e78aec9afa361b2627d62d92f0ee7d7ab0565b
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5558194"
---
# <a name="introduction-to-buffers"></a>Introducción a los búferes


Un recurso de búfer es una colección de datos completos, agrupados en elementos. Los búferes almacenan datos, como coordenadas de textura en un *búfer de vértices*, índices en una *búfer de índices*, datos de constantes del sombreador en un *búfer de constantes*, vectores de posición, vectores normales o el estado del dispositivo.

Un elemento de búfer se compone de 1 a 4 componentes. Los elementos de búfer pueden incluir valores de datos agrupados (por ejemplo, valores de superficie R8G8B8A8), enteros de 8 bits únicos o cuatro valores de punto flotante de 32 bits.

Un búfer se crea como un recurso no estructurado. Dado que es no estructurado, un búfer no puede contener cualquier niveles de mapas MIP, no se puede obtener filtrado cuando se leen y no puede ser varias muestras.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Tipos de búfer


La siguiente es los tipos de recurso de búfer compatibles con Direct3D 11.

-   [Búfer de vértices](#vertex-buffer)
-   [Búfer de índices](#index-buffer)
-   [Búfer de constantes](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Búfer de vértices

Un búfer de vértices contiene los datos de vértice que se usan para definir la geometría. Los datos de vértice incluyen coordenadas de posición, datos de color, datos de coordenadas de textura, datos normales, etc.

El ejemplo más simple de un búfer de vértices es aquella que solo contiene datos de posición. Se pueden visualizar como en la siguiente ilustración.

![ilustración de un búfer de vértices que contiene datos de posición](images/d3d10-resources-single-element-vb2.png)

Con más frecuencia, un búfer de vértices contiene todos los datos necesarios para especificar completamente los vértices 3D. Un ejemplo podría ser un búfer de vértices que contiene la posición por vértice y las coordenadas normales y de textura. Estos datos normalmente se organizan como conjuntos de elementos por vértice, como se muestra en la siguiente ilustración.

![Ilustración de un búfer de vértices que contiene la posición, la normal y datos de textura](images/d3d10-vertex-buffer-element.png)

Este búfer de vértices contiene datos de vértice; cada vértice almacena los tres elementos (posición, la normal y coordenadas de textura). La posición y la normal suelen especificarse con tres controles flotantes de 32 bits y las coordenadas de textura, con dos controles flotantes de 32 bits.

Para obtener acceso a datos de un búfer de vértices debes saber el vértice al acceso, además de los siguientes parámetros de búfer adicionales:

-   Offset: número de bytes desde el principio del búfer hasta los datos del primer vértice.
-   BaseVertexLocation: número de bytes desde el desplazamiento hasta el primer vértice utilizado por la llamada a draw correspondiente.

Antes de crear un búfer de vértices, debes definir su diseño. Después de crea el objeto de diseño de entrada, enlázalo a la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Búfer de índices

Los búferes de índices contienen desplazamientos de enteros en búferes de vértices y se usan para representar a primitivos de forma más eficaz. Un búfer de índices contiene un conjunto secuencial de índices de 16 o 32 bits; cada índice se utiliza para identificar un vértice en un búfer de vértices. Un búfer de índices se puede visualizar como en la siguiente ilustración.

![ilustración de un búfer de índices](images/d3d10-index-buffer.png)

Los índices secuenciales almacenados en un búfer de índices se encuentran con los siguientes parámetros:

-   Desplazamiento - el número de bytes desde la dirección base del búfer de índice.
-   StartIndexLocation - especifica el primer elemento de búfer de índice de la dirección base y el desplazamiento. La ubicación de inicio representa el primer índice para representar.
-   IndexCount: número de índices que se van a representar.

Inicio del búfer de índices = dirección de Base de búfer de índice, desplazamiento (bytes) + StartIndexLocation \ * ElementSize (bytes);

En este cálculo, ElementSize es el tamaño de cada elemento de búfer de índices, que es dos o cuatro bytes.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Búfer de constantes

Un búfer de constantes te permite suministrar de forma eficiente datos de constantes de sombreador a la canalización. Puedes usar un búfer de constantes para almacenar los resultados de la fase de salida de flujo. Conceptualmente, un búfer de constantes es similar a un búfer de vértices de elemento único, como se muestra en la siguiente ilustración.

![ilustración de un búfer de constantes del sombreador](images/d3d10-shader-resource-buffer.png)

Cada elemento almacena una constante de 1 a 4 componentes, que se determina por el formato de los datos almacenados.

Un búfer de constantes solo puede usar una marca de enlace único, lo que no puede combinarse con cualquier otra marca de enlace.

Para leer un búfer de constantes de sombreador de un sombreador, usa una función de la carga HLSL. Cada fase del sombreador permite un máximo de 15 búferes de constantes de sombreador; cada búfer puede contener hasta 4096 constantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Búferes de vértices e índices](vertex-and-index-buffers.md)

 

 




