---
title: Tipos de recursos
description: Los distintos tipos de recursos tienen un diseño distinto (o superficie de memoria).
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- Tipos de recursos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b300747027f0c9466e7ca04b4c4b571882f57a6e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156439"
---
# <a name="resource-types"></a>Tipos de recursos


Los distintos tipos de recursos tienen un diseño distinto (o superficie de memoria). Todos los recursos utilizados por la canalización de Direct3D derivan de dos tipos de recursos básicos: [búferes](#buffer-resources) y [texturas](#texture-resources). Un búfer es una colección de datos sin procesar (elementos); una textura es una colección de textura (elementos de textura).

Hay dos maneras de especificar por completo el diseño (o la superficie de memoria) de un recurso:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Elemento</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Con tipo</p></td>
<td align="left"><p>Especifique totalmente el tipo cuando se cree el recurso.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>Sin tipo</p></td>
<td align="left"><p>Especifique totalmente el tipo cuando el recurso esté enlazado a la canalización.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>Recursos de búfer


Un recurso de búfer es una colección de datos totalmente tipados; internamente, un búfer contiene elementos. Un elemento se compone de 1 a 4 componentes. Algunos ejemplos de tipos de datos de elementos son: un valor de datos empaquetado (como R8G8B8A8), un solo entero de 8 bits, valores Float de 4 32 bits. Estos tipos de datos se usan para almacenar datos, como un vector de posición, un vector normal, una coordenada de textura en un búfer de vértices, un índice en un búfer de índice o el estado del dispositivo.

Un búfer se crea como un recurso no estructurado. Dado que no está estructurado, un búfer no puede contener ningún nivel de mipmap, no se filtrará cuando se lea y no se podrá realizar un muestreo multimuestreado.

### <a name="span-idbuffer_typesspanspan-idbuffer_typesspanspan-idbuffer_typesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Tipos de búfer

-   [Búfer de vértices](#vertex-buffer)
-   [Búfer de índice](#index-buffer)
-   [Búfer de constantes](#shader-constant-buffer)

### <a name="span-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Búfer de vértices

Un búfer es una colección de elementos; un búfer de vértice contiene datos por vértice. El ejemplo más sencillo es un búfer de vértices que contiene un tipo de datos, como los datos de posición. Se puede visualizar como la siguiente ilustración.

![Ilustración de un búfer de vértices que contiene datos de posición](images/d3d10-resources-single-element-vb2.png)

Con mayor frecuencia, un búfer de vértice contiene todos los datos necesarios para especificar por completo los vértices 3D. Un ejemplo de esto podría ser un búfer de vértices que contenga la posición por vértice, las coordenadas normales y de textura. Normalmente, estos datos se organizan como conjuntos de elementos por vértice, tal como se muestra en la siguiente ilustración.

![Ilustración de un búfer de vértices que contiene datos de posición, normal y de textura](images/d3d10-vertex-buffer-element.png)

Este búfer de vértice contiene datos por vértice para ocho vértices; cada vértice almacena tres elementos (coordenadas de posición, normal y de textura). La posición y la normal se especifican normalmente con los flotantes de 3 32 bits y las coordenadas de textura con los flotantes de 2 32 bits.

Para tener acceso a los datos de un búfer de vértices, debe saber qué vértice tiene acceso y estos otros parámetros de búfer:

-   *Offset* : el número de bytes desde el inicio del búfer hasta los datos del primer vértice.
-   *BaseVertexLocation* : el número de bytes desde el desplazamiento hasta el primer vértice usado por la llamada a Draw adecuada.

Antes de crear un búfer de vértices, debe definir su diseño mediante la creación de un objeto de diseño de entrada. Una vez creado el objeto de diseño de entrada, enlácelo a la fase del ensamblador de entrada (IA).

### <a name="span-idindex_bufferspanspan-idindex_bufferspanspan-idindex_bufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Búfer de índice

Un búfer de índice contiene un conjunto secuencial de índices de 16 o 32 bits; cada índice se usa para identificar un vértice en un búfer de vértice. El uso de un búfer de índice con uno o varios búferes de vértices para proporcionar datos a la fase de IA se denomina indexación. Un búfer de índice se puede visualizar como la siguiente ilustración.

![Ilustración de un búfer de índice](images/d3d10-index-buffer.png)

Los índices secuenciales almacenados en un búfer de índice se encuentran con los parámetros siguientes:

-   *Offset* : el número de bytes desde el inicio del búfer hasta el primer índice.
-   *StartIndexLocation* : el número de bytes desde el desplazamiento hasta el primer vértice usado por la llamada a Draw adecuada.
-   *IndexCount* : número de índices que se van a representar.

Un búfer de índice puede unir varias bandas de líneas o triángulos ([topologías primitivas](primitive-topologies.md)) separando cada una de ellas con un índice de corte de franja. Un índice de corte de banda permite dibujar varias franjas de líneas o triángulos con una sola llamada a Draw. Un índice de corte de banda es el valor máximo posible para el índice (0xFFFF para un índice de 16 bits, 0xFFFFFFFF para un índice de 32 bits). El índice de corte de franja restablece el orden de bobinado en los primitivos indizados y se puede usar para quitar la necesidad de triángulos degenerados que, de lo contrario, podría ser necesario para mantener el orden de bobinado adecuado en una franja de triángulo. En la ilustración siguiente se muestra un ejemplo de un índice de corte de banda.

![Ilustración de un índice de corte de franja](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Búfer de constantes

Direct3D tiene un búfer para proporcionar constantes de sombreador denominadas búfer de constantes de sombreador o simplemente un búfer de constantes. Conceptualmente, se parece a un búfer de vértice de un solo elemento, tal como se muestra en la siguiente ilustración.

![Ilustración de un búfer de constantes de sombreador](images/d3d10-shader-resource-buffer.png)

Cada elemento almacena una constante de componente de 1 a 4, determinada por el formato de los datos almacenados.

Los búferes de constantes reducen el ancho de banda necesario para actualizar las constantes de sombreador al permitir que las constantes de sombreador se agrupen y se confirmen al mismo tiempo, en lugar de realizar llamadas individuales para confirmar cada constante por separado.

Un sombreador sigue leyendo variables en un búfer de constantes directamente por nombre de variable de la misma manera que se leen las variables que no forman parte de un búfer de constantes.

Cada fase del sombreador permite hasta 15 búferes de constantes de sombreador; cada búfer puede contener hasta 4096 constantes.

Use un búfer de constantes para almacenar los resultados de la fase de flujo de salida.

Consulte [constantes de sombreador (DirectX HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) para obtener un ejemplo de cómo declarar un búfer de constantes en un sombreador.

## <a name="span-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>Recursos de textura


Un recurso de textura es una colección estructurada de datos diseñada para almacenar textura. A diferencia de los búferes, las texturas se pueden filtrar por muestras de texturas, ya que las unidades de sombreador las leen. El tipo de textura afecta al modo en que se filtra la textura. Un textura representa la unidad más pequeña de una textura que se puede leer o escribir en la canalización. Cada textura contiene de 1 a 4 componentes, organizados en uno de los formatos de DXGI (consulte [** \_ formato de dxgi**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

Las texturas se crean como un recurso estructurado para que se conozca su tamaño. Sin embargo, cada textura puede ser de tipo o sin tipo en el momento de la creación de recursos, siempre que el tipo se especifique por completo utilizando una vista cuando la textura esté enlazada a la canalización.

-   [Tipos de textura](#texture-types)
-   [Subrecursos](#subresources)
-   [Strong frente a tipos débiles](#typed)

### <a name="span-idtexture_typesspanspan-idtexture_typesspanspan-idtexture_typesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>Tipos de textura

Hay varios tipos de texturas: 1D, 2D y 3D, cada una de las cuales se puede crear con o sin mapas MIP. Direct3D también admite matrices de texturas y texturas multimuestreadas.

-   [Textura 1D](#texture1d-resource)
-   [matriz de textura 1D](#texture1d-array-resource)
-   [Textura 2D y matriz de textura 2D](#texture2d-resource)
-   [Textura 3D](#texture3d-resource)

### <a name="span-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>Textura 1D

Una textura 1D en su forma más simple contiene datos de textura que se pueden direccionar con una sola coordenada de textura. se puede visualizar como una matriz de textura, tal como se muestra en la siguiente ilustración.

![Ilustración de una textura 1D](images/d3d10-1d-texture.png)

Cada textura contiene una serie de componentes de color dependiendo del formato de los datos almacenados. Agregar más complejidad, puede crear una textura 1D con niveles de mipmap, tal como se muestra en la siguiente ilustración.

![Ilustración de una textura 1D con niveles de mipmap](images/d3d10-resource-texture1d.png)

Un nivel de mipmap es una textura que es una potencia de dos menor que el nivel superior. El nivel superior contiene la mayoría de los detalles, cada nivel posterior es más pequeño; en el caso de un mipmap 1D, el nivel más pequeño contiene un textura. Los distintos niveles se identifican mediante un índice denominado LOD (nivel de detalle); puede usar el LOD para tener acceso a una textura más pequeña al representar geometría que no es tan cercana a la cámara.

### <a name="span-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>matriz de textura 1D

Direct3D 10 también tiene una nueva estructura de datos para una matriz de texturas. Una matriz de texturas 1D es conceptualmente similar a la siguiente ilustración.

![Ilustración de una matriz de textura 1D](images/d3d10-resource-texture1darray.png)

Esta matriz de texturas contiene tres texturas. Cada una de las tres texturas tiene un ancho de textura de 5 (que es el número de elementos de la primera capa). Cada textura también contiene un mipmap de tres niveles.

Todas las matrices de texturas en Direct3D son una matriz de texturas homogénea; Esto significa que todas las texturas de una matriz de textura deben tener el mismo formato de datos y el mismo tamaño (incluido el ancho de la textura y el número de niveles de mipmap). Puede crear matrices de texturas de diferentes tamaños, siempre que todas las texturas de cada matriz coincidan en tamaño.

### <a name="span-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Textura 2D y matriz de textura 2D

Un recurso Texture2D contiene una cuadrícula 2D de textura. Cada textura es direccionable por un vector u, v. Dado que se trata de un recurso de textura, puede contener niveles de mipmap y Subrecursos. Un recurso de textura 2D totalmente rellenado tiene un aspecto similar al de la siguiente ilustración.

![Ilustración de un recurso de textura 2D](images/d3d10-resource-texture2d.png)

Este recurso de textura contiene una sola textura 3x5 con tres niveles de mipmap.

Un recurso Texture2DArray es una matriz homogénea de texturas 2D; es decir, cada textura tiene el mismo formato de datos y dimensiones (incluidos los niveles de mipmap). Tiene un diseño similar al de la matriz de texturas 1D, salvo que las texturas contienen ahora datos 2D y, por tanto, tienen un aspecto similar al de la siguiente ilustración.

![Ilustración de una matriz de recursos de textura 2D](images/d3d10-resource-texture2darray.png)

Esta matriz de texturas contiene tres texturas; cada textura se 3x5 con dos niveles de mipmap.

### <a name="span-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Usar un Texture2DArray como un cubo de textura

Un cubo de textura es una matriz de textura 2D que contiene 6 texturas, una para cada una de las caras del cubo. Un cubo de textura totalmente rellenado tiene un aspecto similar al de la siguiente ilustración.

![Ilustración de una matriz de recursos de textura 2D que representan un cubo de textura](images/d3d10-resource-texturecube.png)

Una matriz de textura 2D que contiene 6 texturas se puede leer desde los sombreadores con las funciones intrínsecas de mapa de cubo, una vez enlazadas a la canalización con una vista de textura de cubo. Los cubos de textura se dirigen desde el sombreador con un vector 3D que señala hacia fuera del centro del cubo de textura.

### <a name="span-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Textura 3D

Un recurso Texture3D (también conocido como textura de volumen) contiene un volumen 3D de textura. Dado que se trata de un recurso de textura, puede contener niveles de mipmap. Una textura 3D totalmente rellenada es similar a la siguiente ilustración.

![Ilustración de un recurso de textura 3D](images/d3d10-resource-texture3d.png)

Cuando un segmento MIP de textura 3D se enlaza como salida de destino de representación (con una vista de destino de representación), la textura 3D se comporta de forma idéntica a una matriz de textura 2D con n segmentos. El segmento de representación determinado se elige en la fase de sombreador de geometría.

No hay ningún concepto de una matriz de textura 3D; por lo tanto, un subrecurso de textura 3D es un solo nivel de mipmap.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>Metadata

La API de Direct3D hace referencia a los recursos o subconjuntos de recursos completos. Para especificar parte de los recursos, Direct3D ha acuñó el término *Subrecursos*, lo que significa que es un subconjunto de un recurso.

Un búfer se define como un subrecurso único. Las texturas son un poco más complicadas, ya que existen distintos tipos de textura (1D, 2D, etc.), algunos de los cuales admiten niveles de mipmap o matrices de texturas. A partir del caso más simple, una textura 1D se define como un subrecurso único, tal como se muestra en la siguiente ilustración.

![Ilustración de una textura 1D](images/d3d10-1d-texture.png)

Esto significa que la matriz de textura que componen una textura 1D está contenida en un solo subrecurso.

Si expande una textura 1D con tres niveles de mipmap, se puede visualizar como esto.

![Ilustración de una textura 1D con niveles de mipmap](images/d3d10-resource-texture1d.png)

Piense en esto como una textura única que se compone de tres subtexturas. Cada subtextura se cuenta como un subrecurso, por lo que esta textura 1D contiene 3 Subrecursos. Una subtextura (o subrecurso) se puede indizar mediante el nivel de detalle (LOD) para una sola textura. Cuando se usa una matriz de texturas, el acceso a una subtextura determinada requiere el LOD y la textura determinada. Como alternativa, la API combina estos dos fragmentos de información en un único índice de subrecurso basado en cero, tal como se muestra aquí.

![Ilustración de un índice de subrecurso basado en cero](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselecting_subresourcesspanspan-idselecting_subresourcesspanspan-idselecting_subresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>Selección de Subrecursos

Algunas API acceden a un recurso completo y otras acceden a una parte de un recurso. Las API que tienen acceso a una parte de un recurso suelen usar una descripción de la vista para especificar los Subrecursos a los que se va a tener acceso.

Estas figuras muestran los términos utilizados por una descripción de la vista al obtener acceso a una matriz de texturas.

### <a name="span-idarray_slicespanspan-idarray_slicespanspan-idarray_slicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>Segmento de matriz

Dada una matriz de texturas, cada textura con los mapas MIP, un segmento de matriz (representado por el rectángulo blanco) incluye una textura y todas sus subtexturas, tal y como se muestra en la siguiente ilustración.

![Ilustración de una inserción de matriz](images/d3d10-resource-array-slice.png)

### <a name="span-idmip_slicespanspan-idmip_slicespanspan-idmip_slicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Segmento MIP

Un segmento de MIP (representado por el rectángulo blanco) incluye un nivel de mipmap para cada textura de una matriz, como se muestra en la siguiente ilustración.

![Ilustración de una inserción de MIP](images/d3d10-resource-mip-slice.png)

### <a name="span-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>Selección de un único subrecurso

Puede usar estos dos tipos de segmentos para elegir un solo subrecurso, tal como se muestra en la siguiente ilustración.

![Ilustración de la elección de un subrecurso mediante el uso de un segmento de matriz y un empalme de MIP](images/d3d10-resource-subresources-1.png)

### <a name="span-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>Seleccionar varios Subrecursos

También puede usar estos dos tipos de segmentos con el número de niveles de mipmap y/o el número de texturas para elegir varios Subrecursos.

![Ilustración de la elección de varios Subrecursos](images/d3d10-resource-subresources-2.png)

Independientemente del tipo de textura que esté utilizando, con o sin mapas MIP, con o sin una matriz de textura, a menudo hay funciones auxiliares que se proporcionan para calcular el índice de un subrecurso determinado.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Strong frente a tipos débiles

Al crear un recurso totalmente tipado, se restringe el recurso al formato con el que se creó. Esto permite al motor en tiempo de ejecución optimizar el acceso, especialmente si el recurso se crea con marcas que indican que la aplicación no puede asignarlo. Los recursos creados con un tipo específico no se pueden reinterpretar mediante el mecanismo de vista.

En un recurso sin tipo, el tipo de datos es desconocido cuando el recurso se crea por primera vez. La aplicación debe elegir entre los formatos sin tipo disponibles. Debe especificar el tamaño de la memoria que se va a asignar y si el tiempo de ejecución necesitará generar las subtexturas en un mipmap.

Sin embargo, el formato de datos exacto (si la memoria se interpreta como enteros, valores de punto flotante, enteros sin signo, etc.) no se determina hasta que el recurso se enlaza a la canalización con una vista. Dado que el formato de textura sigue siendo flexible hasta que la textura se enlaza a la canalización, el recurso se denomina almacenamiento débilmente tipado. El almacenamiento débilmente tipado tiene la ventaja de que se puede reutilizar o reinterpretar (en otro formato) siempre que el bit del componente del nuevo formato coincida con el recuento de bits del formato anterior.

Un solo recurso se puede enlazar a varias fases de canalización, siempre y cuando cada una tenga una vista única, que califique por completo los formatos en cada ubicación. Por ejemplo, un recurso creado con un formato sin tipo podría usarse como un formato FLOAT y un formato UINT en diferentes ubicaciones de la canalización simultáneamente.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos](resources.md)