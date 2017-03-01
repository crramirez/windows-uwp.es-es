---
title: "Introducción a los búferes"
description: "Un recurso de búfer es una colección de datos completos, agrupados en elementos."
ms.assetid: 494FDF57-0FBE-434C-B568-06F977B40263
keywords:
- "Introducción a los búferes"
author: mtoepke
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ec2d181daca8f9ece8e1d364cd58234feb85977a
ms.lasthandoff: 02/07/2017

---

# <a name="introduction-to-buffers"></a>Introducción a los búferes


Un recurso de búfer es una colección de datos completos, agrupados en elementos. Los búferes almacenan datos, como coordenadas de textura en un *búfer de vértices*, índices en una *búfer de índices*, datos de constantes del sombreador en un *búfer de constantes*, vectores de posición, vectores normales o el estado del dispositivo.

Un elemento de búfer se compone de 1 a 4 componentes. Los elementos de búfer pueden incluir valores de datos agrupados (por ejemplo, valores de superficie R8G8B8A8), enteros de 8 bits únicos o cuatro valores de punto flotante de 32 bits.

Un búfer se crea como un recurso no estructurado. Dado que no es estructurado, un búfer no puede contener niveles de mapas MIP, no se puede filtrar cuando se lee y no se pueden realizar muestras múltiples en él.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Tipos de búfer


Los siguientes son los tipos de recurso de búfer que admite Direct3D 11.

-   [Búfer de vértices](#vertex-buffer)
-   [Búfer de índices](#index-buffer)
-   [Búfer de constantes](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Búfer de vértices

Un búfer de vértices contiene los datos de vértice que se usan para definir la geometría. Los datos de vértice incluyen coordenadas de posición, datos de color, datos de coordenadas de textura, datos normales, etc.

El ejemplo más sencillo de un búfer de vértices es el que solo contiene datos de posición. Se puede visualizar como en la siguiente ilustración.

![ilustración de un búfer de vértices que contiene datos de posición](images/d3d10-resources-single-element-vb2.png)

Con más frecuencia, un búfer de vértices contiene todos los datos necesarios para especificar completamente los vértices 3D. Un ejemplo podría ser un búfer de vértices que contiene la posición por vértice y las coordenadas normales y de textura. Estos datos normalmente se organizan como conjuntos de elementos por vértice, como se muestra en la siguiente ilustración.

![ilustración de un búfer de vértices que contiene datos normales, de posición y de textura](images/d3d10-vertex-buffer-element.png)

Este búfer de vértices contiene datos por vértice, cada vértice almacena tres elementos (coordenadas normales, de posición y de textura). Las de posición y las normales se especifican normalmente cada una con tres flotantes de 32 bits y las coordenadas de textura, con dos flotantes de 32 bits.

Para obtener acceso a datos desde un búfer de vértices, necesitas saber a qué vértices tienes que obtener acceso, además de los siguientes parámetros de búfer adicionales:

-   Offset: el número de bytes desde el inicio del búfer hasta los datos para el primer vértice.
-   BaseVertexLocation: el número de bytes desde el desplazamiento hasta el primer vértice que usa la llamada de dibujo apropiada.

Antes de crear un búfer de vértices, tienes que definir su diseño. Después de crear el objeto de diseño de entrada, se enlaza a la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Búfer de índices

Los búferes de índices contienen desplazamientos de enteros en búferes de vértices y se usan para representar primitivos de forma más eficiente. Un búfer de índices contiene un conjunto secuencial de índices de 32 bits o 16 bits y cada índice se usa para identificar un vértice en un búfer de vértices. Un búfer de índices se puede visualizar como en la siguiente ilustración.

![ilustración de un búfer de índices](images/d3d10-index-buffer.png)

Los índices secuenciales almacenados en un búfer de índices se encuentran con los siguientes parámetros:

-   Offset: el número de bytes desde la dirección base del búfer de índices.
-   StartIndexLocation: especifica el primer elemento del búfer de índices desde la dirección base y el desplazamiento. La ubicación de inicio representa el primer índice que se representará.
-   IndexCount: el número de índices que se representará.

Inicio del búfer de índices = dirección base del búfer de índices + desplazamiento (bytes) + StartIndexLocation \ * ElementSize (bytes);

En este cálculo, ElementSize es el tamaño de cada elemento del búfer de índices, que es dos o cuatro bytes.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Búfer de constantes

Un búfer de constantes te permite suministrar de forma eficiente datos de constantes del sombreador a la canalización. Puedes usar un búfer de constantes para almacenar los resultados de la fase de salida en secuencias. Conceptualmente, un búfer de constantes es similar a un búfer de vértices de un solo elemento, como se muestra en la siguiente ilustración.

![ilustración de un búfer de constantes del sombreador](images/d3d10-shader-resource-buffer.png)

Cada elemento almacena de 1 a 4 constantes del componente, en función del formato de los datos almacenados.

Un búfer de constantes solo puede usar una sola marca de enlace, que no se pueden combinar con ninguna otra.

Para leer un búfer de constantes del sombreador desde un sombreador, usa una función de carga HLSL. Cada fase del sombreador permite hasta 15 búferes de constantes del sombreador y cada búfer puede contener hasta 4096 constantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Búferes de vértices e índices](vertex-and-index-buffers.md)

 

 





