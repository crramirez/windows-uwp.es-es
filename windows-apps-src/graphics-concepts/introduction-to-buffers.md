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
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 677eea4757220320bc364fef20bf5a5c8d4daaa2
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044344"
---
# <a name="introduction-to-buffers"></a>Introducción a los búferes


Un recurso de búfer es una colección de datos completos, agrupados en elementos. Los búferes almacenan datos, como coordenadas de textura en un *búfer de vértices*, índices en una *búfer de índices*, datos de constantes del sombreador en un *búfer de constantes*, vectores de posición, vectores normales o el estado del dispositivo.

Un elemento del búfer se compone de componentes de 1 a 4. Los elementos de búfer pueden incluir valores de datos agrupados (por ejemplo, valores de superficie R8G8B8A8), enteros de 8 bits únicos o cuatro valores de punto flotante de 32 bits.

Un búfer se crea como un recurso no estructurado. Dado que es no estructurado, un búfer no puede contener los niveles de mipmap, no se puede obtener filtrado cuando se leen y no puede ser de muestreo múltiple.

## <a name="span-idbuffertypesspanspan-idbuffertypesspanspan-idbuffertypesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Tipos de búfer


Los siguientes son los tipos de recurso de búfer compatibles con Direct3D 11.

-   [Búfer de vértices](#vertex-buffer)
-   [Búfer de índices](#index-buffer)
-   [Constante búfer](#shader-constant-buffer)

### <a name="span-idvertexbufferspanspan-idvertexbufferspanspan-idvertexbufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Búfer de vértice

Un búfer de vértice contiene los datos de vértice que se usa para definir la geometría. Los datos de vértice incluyen coordenadas de posición, datos de color, datos de coordenadas de textura, datos normales, etc.

El ejemplo más sencillo de un búfer de vértice es aquella que sólo contiene datos de posición. Se pueden visualizar como en la siguiente ilustración.

![ilustración de un búfer de vértices que contiene datos de posición](images/d3d10-resources-single-element-vb2.png)

Con más frecuencia, un búfer de vértices contiene todos los datos necesarios para especificar completamente los vértices 3D. Un ejemplo podría ser un búfer de vértices que contiene la posición por vértice y las coordenadas normales y de textura. Estos datos normalmente se organizan como conjuntos de elementos por vértice, como se muestra en la siguiente ilustración.

![Ilustración de un búfer de vértices que contiene la posición, la normal y datos de textura](images/d3d10-vertex-buffer-element.png)

Este búfer de vértice contiene datos por vértice; cada vértice almacena tres elementos (posición, normal y coordenadas de textura). La posición y la normal suelen especificarse con tres controles flotantes de 32 bits y las coordenadas de textura, con dos controles flotantes de 32 bits.

Para obtener acceso a datos desde un búfer de vértice necesita saber qué vértice en access, además de los parámetros de búfer adicional siguiente:

-   Offset: número de bytes desde el principio del búfer hasta los datos del primer vértice.
-   BaseVertexLocation: número de bytes desde el desplazamiento hasta el primer vértice utilizado por la llamada a draw correspondiente.

Antes de crear un búfer de vértice, debe definir su diseño. Una vez creado el objeto de diseño de entrada, enlazarla a la [fase de entrada Assembler (IA)](input-assembler-stage--ia-.md).

### <a name="span-idindexbufferspanspan-idindexbufferspanspan-idindexbufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Búfer de índice

Búferes de índice contienen desplazamientos de entero en los búferes de vértice y se utilizan para representar a Fundamentos de forma más eficaz. Un búfer de índices contiene un conjunto secuencial de índices de 16 o 32 bits; cada índice se utiliza para identificar un vértice en un búfer de vértices. Un búfer de índices se puede visualizar como en la siguiente ilustración.

![ilustración de un búfer de índices](images/d3d10-index-buffer.png)

Los índices secuenciales almacenados en un búfer de índices se encuentran con los siguientes parámetros:

-   Desplazamiento - el número de bytes de la dirección base del búfer de índice.
-   StartIndexLocation - especifica el primer elemento de búfer de índice de la dirección base y el desplazamiento. La ubicación de inicio representa el primer índice para representar.
-   IndexCount: número de índices que se van a representar.

Inicio del búfer de índice = dirección Base del búfer de índice + desplazamiento (bytes) + StartIndexLocation \ * ElementSize (bytes);

En este cálculo, ElementSize es el tamaño de cada elemento de búfer de índice, que es de dos o cuatro bytes.

### <a name="span-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshaderconstantbufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Constante búfer

Un búfer constante permite proporcionar con eficacia datos de constantes de sombreador de vértices a la canalización. Puede usar un constante búfer para almacenar los resultados de la región de resultados de la secuencia. Conceptualmente, un búfer de constante es similar a un búfer de vértice de un solo elemento, tal como se muestra en la siguiente ilustración.

![ilustración de un búfer de constantes del sombreador](images/d3d10-shader-resource-buffer.png)

Cada elemento almacena una constante de 1 a 4 componentes, que se determina por el formato de los datos almacenados.

Un búfer constante sólo puede utilizar una marca enlazar único, que no se puede combinar con cualquier otro marcador de enlace.

Para leer un búfer de constante de sombreador de vértices de un sombreado, utilizar una función de carga HLSL. Cada fase del sombreador permite un máximo de 15 búferes de constantes de sombreador; cada búfer puede contener hasta 4096 constantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Búferes de vértices e índices](vertex-and-index-buffers.md)

 

 




