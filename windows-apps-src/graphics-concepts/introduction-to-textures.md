---
title: Introducción a las texturas
description: Un recurso de textura es una estructura de datos para almacenar elementos de textura, que son las unidades más pequeñas de una textura que pueden leerse o en las que se puede escribir. Cuando un sombreador lee la textura, se puede filtrar por muestrarios de texturas.
ms.assetid: 6F3C76A8-F762-4296-AE02-BFBD6476A5A8
keywords:
- Introducción a las texturas
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f88cccc3f32449d09c01450bf159b3fca6a3d59f
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6991407"
---
# <a name="introduction-to-textures"></a>Introducción a las texturas


Un recurso de textura es una estructura de datos para almacenar elementos de textura, que son las unidades más pequeñas de una textura que pueden leerse o en las que se puede escribir. Cuando un sombreador lee la textura, se puede filtrar por muestrarios de texturas.

Un recurso de textura es una colección estructurada de datos diseñada para almacenar elementos de textura. Un elemento de textura representa la unidad más pequeña de una textura que puede leer una canalización o a la que puede escribir. A diferencia de los búferes, las texturas se pueden filtrar por muestrarios de texturas, ya que las leen las unidades del sombreador. El tipo de textura afecta a cómo se filtra la textura. Cada elemento de textura contiene de 1 a 4 componentes, organizados en uno de los formatos DXGI definidos por la enumeración DXGI\_FORMAT.

Las texturas se crean como un recurso estructurado con un tamaño conocido. Sin embargo, cada textura puede tener tipo o no cuando se crea el recurso, siempre que el tipo esté totalmente especificado con una vista cuando se enlace la textura a la canalización.

## <a name="span-idtexturetypesspanspan-idtexturetypesspanspan-idtexturetypesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span>Tipos de textura


Direct3D admite varias representaciones de punto flotante. Todos los cálculos de punto flotante funcionan en un subconjunto de las reglas de punto flotante IEEE 754 de precisión simple de 32 bits.

Hay varios tipos de texturas: 1D, 2D y 3D. Cada uno puede crearse con o sin mapas MIP. Direct3D también admite matrices de texturas y texturas de varias muestras.

-   [Texturas 1D](#texture1d-resource)
-   [Matrices de texturas 1D](#texture1d-array-resource)
-   [Texturas 2D y matrices de texturas 2D](#texture2d-resource)
-   [Texturas 3D](#texture3d-resource)

### <a name="span-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1dresourcespanspan-idtexture1d-resourcespan1d-textures"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>Texturas 1D

Una textura 1D en su forma más sencilla contiene datos de texturas que se pueden abordar con una sola coordenada de textura. Se puede visualizar como una matriz de elementos de textura, como se muestra en la siguiente ilustración.

En la siguiente ilustración se muestra una textura 1D:

![una textura 1D](images/d3d10-1d-texture.png)

Cada elemento de textura contiene un número de componentes de color según el formato de los datos almacenados. Complicándolo más, puedes crear una textura 1D con niveles de mapas MIP, como se muestra en la siguiente ilustración.

![una textura 1D con niveles de mapas MIP](images/d3d10-resource-texture1d.png)

Un nivel de mapas MIP es una textura que es una potencia de dos menor que el nivel que tiene por encima. El nivel superior contiene más detalle y cada nivel posterior es menor. Para un mapa MIP 1D, el nivel más pequeño contiene un elemento de textura. Además, los niveles de MIP siempre se reducen hasta 1:1.

Cuando se generan mapas MIP para una textura de tamaño impar, el siguiente nivel inferior siempre es de tamaño par (excepto cuando el nivel más bajo llega a 1). Por ejemplo, en el diagrama se ilustra una textura de 5x1 cuyo siguiente nivel más bajo es una textura de 2x1, cuyo siguiente (y último) nivel MIP es una textura de tamaño 1x1. Un índice llamado LOD (nivel de detalle), que se usa para obtener acceso a la textura más pequeña al representar geometría que no está lo bastante cerca de la cámara, identifica los niveles.

### <a name="span-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1darrayresourcespanspan-idtexture1d-array-resourcespan1d-texture-arrays"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>Matrices de texturas 1D

Direct3D también admite matrices de texturas. Conceptualmente, una matriz de texturas 1D es similar a la siguiente ilustración.

![una matriz de texturas 1D](images/d3d10-resource-texture1darray.png)

Esta matriz de textura contiene tres texturas. Cada una de las tres texturas tiene un ancho de textura de 5 (que es el número de elementos que hay en la primera capa). Cada textura también contiene un mapa MIP de 3 capas.

Todas las matrices de textura de Direct3D son una matriz homogénea de texturas, lo que significa que cada textura de una matriz de texturas debe tener el mismo formato de datos y tamaño (incluido el ancho de textura y el número de niveles de mapas MIP). Puedes crear matrices de texturas de distintos tamaños, siempre que todas las texturas de cada matriz tengan el mismo tamaño.

### <a name="span-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2dresourcespanspan-idtexture2d-resourcespan2d-textures-and-2d-texture-arrays"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Texturas 2D y matrices de texturas 2D

Un recurso Texture2D contiene una cuadrícula 2D de elementos de textura. Cada elemento de textura es direccionable por un vector u, v. Dado que es un recurso de textura, puede contener niveles de mapas MIP y subrecursos. Un recurso de textura 2D totalmente rellenado es similar a la siguiente ilustración.

![un recurso de textura 2D](images/d3d10-resource-texture2d.png)

Este recurso de textura contiene una única textura de 3x5 con tres niveles de mapas MIP.

Un recurso de matriz de texturas es una matriz homogénea de texturas 2D. Es decir, cada textura tiene el mismo formato de datos y dimensiones (incluidos los niveles de mapas MIP). Tiene un diseño similar a la matriz de texturas 1D salvo que las texturas ahora contienen datos 2D, como se muestra en la siguiente ilustración.

![una matriz de texturas 2D](images/d3d10-resource-texture2darray.png)

Esta matriz de texturas contiene tres texturas y cada textura es de 3x5 con dos niveles de mapas MIP.

### <a name="span-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanspan-idtexture2darrayresourceasatexturecubespanusing-a-2d-texture-array-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Usar una matriz de texturas 2D como un cubo de texturas

Un cubo de texturas es una matriz de texturas 2D que contiene 6 texturas, una para cada cara del cubo. Un cubo de texturas totalmente rellenado es similar a la siguiente ilustración.

![una matriz de texturas 2D que representan un cubo de texturas](images/d3d10-resource-texturecube.png)

Una matriz de texturas 2D que contiene 6 texturas se pueden leer desde dentro de los sombreadores con las funciones intrínsecas del mapa de cubos, después de que se enlacen a la canalización con una vista de textura de cubo. Los cubos de texturas se tratan desde el sombreador con un vector 3D señalando desde el centro del cubo de texturas.

### <a name="span-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3dresourcespanspan-idtexture3d-resourcespan3d-textures"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Texturas 3D

Un recurso de textura 3D (también conocido como textura de volumen) contiene un volumen 3D de elementos de textura. Dado que es un recurso de textura, puede contener niveles de mapas MIP. Una textura 3D totalmente rellenada es similar a la siguiente ilustración.

![un recurso de textura 3D](images/d3d10-resource-texture3d.png)

Cuando se enlaza un segmento de mapa MIP de textura 3D como una salida de destino de representación (con una vista de destino de representación), la textura 3D se comporta igual que una matriz de texturas 2D con n segmentos. El sector de representación concreto se elige desde la fase del sombreador de geometría.

No hay ningún concepto de una matriz de texturas 3D. Por lo tanto, un subrecurso de textura 3D es un nivel de mapa MIP único.

Los sistemas de coordenadas de Direct3D están definidos para píxeles y elementos de textura.

## <a name="span-idpixelspanspan-idpixelspanspan-idpixelspanpixel-coordinate-system"></a><span id="Pixel"></span><span id="pixel"></span><span id="PIXEL"></span>Sistema de coordenadas de píxeles


El sistema de coordenadas de píxeles en Direct3D define el origen de un destino de representación en la esquina superior izquierda, como se muestra en el siguiente diagrama. Los centros de píxeles tienen un desplazamiento de (0,5f, 0,5f) desde ubicaciones de enteros.

![diagrama del sistema de coordenadas de píxeles en Direct3D 10](images/d3d10-coordspix10.png)

## <a name="span-idtexelspanspan-idtexelspanspan-idtexelspantexel-coordinate-system"></a><span id="Texel"></span><span id="texel"></span><span id="TEXEL"></span>Sistema de coordenadas de elementos de textura


El sistema de coordenadas de elementos de textura tiene su origen en la esquina superior izquierda de la textura, como se muestra en el siguiente diagrama. Esto hace que la representación de texturas alineadas a la pantalla sea trivial, dado que el sistema de coordenadas de píxeles se alinea con el sistema de coordenadas de elementos de textura.

![diagrama del sistema de coordenadas de elementos de textura](images/d3d10-coordstex10.png)

Las coordenadas de textura se representan con un número normalizado o escalado. Cada coordenada de textura se asigna a un elemento de textura específico de la siguiente manera:

Para obtener una coordenada normalizada:

-   Muestreo de punto: elemento de textura \# = floor(U \* ancho)
-   Muestreo lineal: elemento de textura izquierdo \# = floor(U \* ancho), elemento de textura derecho \# = elemento de textura izquierdo \# + 1

Para obtener una coordenada escalada:

-   Muestreo de punto: elemento de textura \# = floor(U)
-   Muestreo lineal: elemento de textura izquierdo \# = floor(U - 0,5), elemento de textura derecho \# = elemento de textura derecho \# + 1

Ancho es el ancho de la textura (en elementos de textura).

El ajuste de dirección de textura ocurre después de que se calcule la ubicación del elemento de textura.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)
