---
title: Tipos de recursos
description: Los distintos tipos de recursos tienen un diseño (o una superficie de memoria) diferente.
ms.assetid: BCDDF227-1837-44DA-ABD4-E39BCFF2B8EF
keywords:
- Tipos de recursos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9712b4498b03460568d20d4c8e27172ad5c14360
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210101"
---
# <a name="resource-types"></a>Tipos de recursos


Los distintos tipos de recursos tienen un diseño (o una superficie de memoria) diferente. Todos los recursos que utiliza la canalización de Direct3D derivan de dos tipos de recursos básicos: [búferes](#buffer-resources) y [texturas](#texture-resources). Un búfer es una colección de datos sin procesar (elementos); una textura es una colección de elementos de textura.

Existen dos maneras de especificar completamente el diseño (o la superficie de memoria) de un recurso:

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
<td align="left"><p>Se especifica el tipo completamente al crear el recurso.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="Typeless"></span><span id="typeless"></span><span id="TYPELESS"></span>Sin tipo</p></td>
<td align="left"><p>Se especifica el tipo completamente cuando el recurso está enlazado a la canalización.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer_resourcesspanspan-idbuffer-resourcesspanbuffer-resources"></a><span id="Buffer_Resources"></span><span id="buffer_resources"></span><span id="BUFFER_RESOURCES"></span><span id="buffer-resources"></span>Recursos de búfer


Un recurso de búfer es una colección de datos completamente tipificados; internamente, un búfer contiene elementos. Un elemento está formado por entre 1 y 4 componentes. Algunos ejemplos de tipos de datos de elemento son: un valor de los datos empaquetados (por ejemplo, R8G8B8A8), un único entero de 8 bits o cuatro valores flotantes de 32 bits. Estos tipos de datos se utilizan para almacenar datos, como un vector de posición, un vector normal, una coordenada de textura en un búfer de vértices, un índice en un búfer de índices o el estado del dispositivo.

Un búfer se crea como un recurso no estructurado. Dado que no está estructurado, un búfer no puede contener ningún nivel de mapas MIP, no se filtra cuando se lee y no puede ser de varias muestras.

### <a name="span-idbuffer_typesspanspan-idbuffer_typesspanspan-idbuffer_typesspanbuffer-types"></a><span id="Buffer_Types"></span><span id="buffer_types"></span><span id="BUFFER_TYPES"></span>Tipos de búfer

-   [Búfer de vértices](#vertex-buffer)
-   [Búfer de índice](#index-buffer)
-   [Búfer de constantes](#shader-constant-buffer)

### <a name="span-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex_bufferspanspan-idvertex-bufferspanvertex-buffer"></a><span id="Vertex_Buffer"></span><span id="vertex_buffer"></span><span id="VERTEX_BUFFER"></span><span id="vertex-buffer"></span>Búfer de vértices

Un búfer es una colección de elementos; un búfer de vértices contiene los datos por vértice. El ejemplo más simple es un búfer de vértices que contiene un tipo de datos, como datos de posición. Se puede visualizar como en la siguiente ilustración.

![ilustración de un búfer de vértices que contiene datos de posición](images/d3d10-resources-single-element-vb2.png)

Con más frecuencia, un búfer de vértices contiene todos los datos necesarios para especificar completamente los vértices 3D. Un ejemplo podría ser un búfer de vértices que contiene la posición por vértice y las coordenadas normales y de textura. Estos datos normalmente se organizan como conjuntos de elementos por vértice, como se muestra en la siguiente ilustración.

![ilustración de un búfer de vértices que contiene datos normales, de posición y de textura](images/d3d10-vertex-buffer-element.png)

Este búfer de vértices contiene los datos por vértice de ocho vértices; cada vértice almacena tres elementos (posición, normal y coordenadas de textura). Las de posición y las normales se especifican normalmente cada una con tres flotantes de 32 bits y las coordenadas de textura, con dos flotantes de 32 bits.

Para acceder a datos de un búfer de vértices, necesitas saber el vértice al que debes acceder y estos otros parámetros de búfer:

-   *Offset*: número de bytes desde el principio del búfer hasta los datos del primer vértice.
-   *BaseVertexLocation*: número de bytes desde el desplazamiento hasta el primer vértice utilizado por la llamada a draw correspondiente.

Antes de crear un búfer de vértices, debes definir su diseño mediante la creación de un objeto de diseño de entrada. Una vez creado el objeto de diseño de entrada, enlázalo a la fase del ensamblador de entrada (IA).

### <a name="span-idindex_bufferspanspan-idindex_bufferspanspan-idindex_bufferspanspan-idindex-bufferspanindex-buffer"></a><span id="Index_Buffer"></span><span id="index_buffer"></span><span id="INDEX_BUFFER"></span><span id="index-buffer"></span>Búfer de índice

Un búfer de índices contiene un conjunto secuencial de índices de 16 o 32 bits; cada índice se utiliza para identificar un vértice en un búfer de vértices. El uso de un búfer de índices con uno o más búferes de vértices para suministrar datos a la fase IA se denomina indexación. Un búfer de índices se puede visualizar como en la siguiente ilustración.

![ilustración de un búfer de índices](images/d3d10-index-buffer.png)

Los índices secuenciales almacenados en un búfer de índices se encuentran con los siguientes parámetros:

-   *Offset*: número de bytes desde el principio del búfer hasta el primer índice.
-   *StartIndexLocation*: número de bytes desde el desplazamiento hasta el primer vértice utilizado por la llamada a draw correspondiente.
-   *IndexCount*: número de índices que se van a representar.

Un búfer de índices puede unir varias series de líneas o triángulos ([topologías primitivas](primitive-topologies.md)) mediante la separación de cada una de ellas con un índice de corte en series. Un índice de corte en series permite dibujar varias series de líneas o triángulos con una sola llamada a draw. Un índice de franja en series es el máximo valor posible del índice (0xffff para un índice de 16 bits, 0xffffffff para un índice de 32 bits). El índice de corte en series restablece el orden de generación de los primitivos indexados y puede usarse para eliminar la necesidad de triángulos degenerados que, de otro modo, se necesitarían para mantener el orden de generación correcto en una serie de triángulos. En la siguiente ilustración se muestra un ejemplo de un índice de corte en series.

![Ilustración de un índice de corte en series](images/d3d10-ia-strips-cut-value.png)

### <a name="span-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader_constant_bufferspanspan-idshader-constant-bufferspanconstant-buffer"></a><span id="Shader_Constant_Buffer"></span><span id="shader_constant_buffer"></span><span id="SHADER_CONSTANT_BUFFER"></span><span id="shader-constant-buffer"></span>Búfer de constantes

Direct3D tiene un búfer para suministrar constantes de sombreador que se denomina "búfer de constantes de sombreador" o simplemente "búfer de constantes". Conceptualmente, se parece a un búfer de vértices de un solo elemento, como se muestra en la siguiente ilustración.

![ilustración de un búfer de constantes del sombreador](images/d3d10-shader-resource-buffer.png)

Cada elemento almacena de 1 a 4 constantes del componente, en función del formato de los datos almacenados.

Los búferes de constantes reducen el ancho de banda necesario para actualizar las constantes de sombreador. Para ello, permite agrupar y confirmar las constantes de sombreador al mismo tiempo, en lugar de realizar llamadas individuales para confirmar cada constante por separado.

Un sombreador continúa leyendo variables en un búfer de constantes directamente por el nombre de variable, igual que se leen las variables que no forman parte de un búfer de constantes.

Cada fase del sombreador permite hasta 15 búferes de constantes del sombreador y cada búfer puede contener hasta 4096 constantes.

Usa un búfer de constantes para almacenar los resultados de la fase de salida en secuencias.

Consulta [Shader Constants (DirectX HLSL)](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants) [Constantes de sombreador (DirectX HLSL)] para ver un ejemplo de declaración de un búfer de constantes en un sombreador.

## <a name="span-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture_resourcesspanspan-idtexture-resourcesspantexture-resources"></a><span id="Texture_Resources"></span><span id="texture_resources"></span><span id="TEXTURE_RESOURCES"></span><span id="texture-resources"></span>Recursos de textura


Un recurso de textura es una colección estructurada de datos diseñada para almacenar elementos de textura. A diferencia de los búferes, las texturas se pueden filtrar por muestrarios de texturas, ya que las leen las unidades del sombreador. El tipo de textura afecta a cómo se filtra la textura. Un elemento de textura representa la unidad más pequeña de una textura que puede leer una canalización o a la que puede escribir. Cada textura contiene de 1 a 4 componentes, organizados en uno de los formatos de DXGI (consulte [**formato de\_de dxgi**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

Las texturas se crean como un recurso estructurado para que se conozca su tamaño. Sin embargo, cada textura puede ser con o sin tipos en el momento de creación del recurso, siempre que el tipo esté totalmente especificado con una vista cuando la textura esté enlazada a la canalización.

-   [Tipos de textura](#texture-types)
-   [Metadata](#subresources)
-   [Strong frente a tipos débiles](#typed)

### <a name="span-idtexture_typesspanspan-idtexture_typesspanspan-idtexture_typesspanspan-idtexture-typesspantexture-types"></a><span id="Texture_Types"></span><span id="texture_types"></span><span id="TEXTURE_TYPES"></span><span id="texture-types"></span>Tipos de textura

Hay varios tipos de texturas: 1D, 2D y 3D. Cada uno puede crearse con o sin mapas MIP. Direct3D también admite matrices de texturas y texturas de varias muestras.

-   [Textura 1D](#texture1d-resource)
-   [matriz de textura 1D](#texture1d-array-resource)
-   [Textura 2D y matriz de textura 2D](#texture2d-resource)
-   [Textura 3D](#texture3d-resource)

### <a name="span-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d_resourcespanspan-idtexture1d-resourcespan1d-texture"></a><span id="Texture1D_Resource"></span><span id="texture1d_resource"></span><span id="TEXTURE1D_RESOURCE"></span><span id="texture1d-resource"></span>Textura 1D

Una textura 1D en su forma más sencilla contiene datos de texturas que se pueden abordar con una sola coordenada de textura. Se puede visualizar como una matriz de elementos de textura, como se muestra en la siguiente ilustración.

![Ilustración de una textura 1D](images/d3d10-1d-texture.png)

Cada elemento de textura contiene un número de componentes de color según el formato de los datos almacenados. Complicándolo más, puedes crear una textura 1D con niveles de mapas MIP, como se muestra en la siguiente ilustración.

![Ilustración de una textura 1D con niveles de mapas MIP](images/d3d10-resource-texture1d.png)

Un nivel de mapas MIP es una textura que es una potencia de dos menor que el nivel que tiene por encima. El nivel superior contiene el máximo detalle, que es menor en cada nivel posterior; para un mapa MIP 1D, el nivel inferior contiene un elemento de textura. Los diferentes niveles se identifican con un índice denominado LOD (nivel de detalle); puedes usar el LOD para acceder a una textura inferior al representar geometría que está tan cerca de la cámara.

### <a name="span-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d_array_resourcespanspan-idtexture1d-array-resourcespan1d-texture-array"></a><span id="Texture1D_Array_Resource"></span><span id="texture1d_array_resource"></span><span id="TEXTURE1D_ARRAY_RESOURCE"></span><span id="texture1d-array-resource"></span>matriz de textura 1D

Direct3D 10 también tiene una nueva estructura de datos para las matrices de texturas. Conceptualmente, una matriz de texturas 1D es similar a la siguiente ilustración.

![Ilustración de una matriz de texturas 1D](images/d3d10-resource-texture1darray.png)

Esta matriz de textura contiene tres texturas. Cada una de las tres texturas tiene un ancho de textura de 5 (que es el número de elementos que hay en la primera capa). Cada textura también contiene un mapa MIP de 3 niveles.

Todas las matrices de textura de Direct3D son una matriz homogénea de texturas, lo que significa que cada textura de una matriz de texturas debe tener el mismo formato de datos y tamaño (incluido el ancho de textura y el número de niveles de mapas MIP). Puedes crear matrices de texturas de distintos tamaños, siempre que todas las texturas de cada matriz tengan el mismo tamaño.

### <a name="span-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d_resourcespanspan-idtexture2d-resourcespan2d-texture-and-2d-texture-array"></a><span id="Texture2D_Resource"></span><span id="texture2d_resource"></span><span id="TEXTURE2D_RESOURCE"></span><span id="texture2d-resource"></span>Textura 2D y matriz de textura 2D

Un recurso Texture2D contiene una cuadrícula 2D de elementos de textura. Cada elemento de textura es direccionable por un vector u, v. Dado que es un recurso de textura, puede contener niveles de mapas MIP y subrecursos. Un recurso de textura 2D totalmente rellenado es similar a la siguiente ilustración.

![Ilustración de un recurso de textura 2D](images/d3d10-resource-texture2d.png)

Este recurso de textura contiene una única textura de 3x5 con tres niveles de mapas MIP.

Un recurso Texture2DArray es una matriz homogénea de texturas 2D; es decir, cada textura tiene las mismas dimensiones y el mismo formato de datos (incluidos los niveles de mapas MIP). Tiene un diseño similar que la matriz de textura 1D, salvo en que las texturas contienen ahora datos 2D y, por tanto, su aspecto es similar al de la siguiente ilustración.

![Ilustración de una matriz de recursos de textura 2D](images/d3d10-resource-texture2darray.png)

Esta matriz de texturas contiene tres texturas y cada textura es de 3x5 con dos niveles de mapas MIP.

### <a name="span-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanspan-idtexture2darray_resource_as_a_texture_cubespanusing-a-texture2darray-as-a-texture-cube"></a><span id="Texture2DArray_Resource_as_a_Texture_Cube"></span><span id="texture2darray_resource_as_a_texture_cube"></span><span id="TEXTURE2DARRAY_RESOURCE_AS_A_TEXTURE_CUBE"></span>Usar un Texture2DArray como un cubo de textura

Un cubo de texturas es una matriz de texturas 2D que contiene 6 texturas, una para cada cara del cubo. Un cubo de texturas totalmente rellenado es similar a la siguiente ilustración.

![Ilustración de una matriz de recursos de textura 2D que representan un cubo de texturas](images/d3d10-resource-texturecube.png)

Una matriz de texturas 2D que contiene 6 texturas se pueden leer desde dentro de los sombreadores con las funciones intrínsecas del mapa de cubos, después de que se enlacen a la canalización con una vista de textura de cubo. Los cubos de texturas se tratan desde el sombreador con un vector 3D señalando desde el centro del cubo de texturas.

### <a name="span-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d_resourcespanspan-idtexture3d-resourcespan3d-texture"></a><span id="Texture3D_Resource"></span><span id="texture3d_resource"></span><span id="TEXTURE3D_RESOURCE"></span><span id="texture3d-resource"></span>Textura 3D

Un recurso Texture3D (también conocido como "textura de volumen") contiene un volumen 3D de elementos de textura. Dado que es un recurso de textura, puede contener niveles de mapas MIP. Una textura 3D totalmente rellenada es similar a la siguiente ilustración.

![Ilustración de un recurso de textura 3D](images/d3d10-resource-texture3d.png)

Cuando se enlaza un segmento de mapa MIP de textura 3D como una salida de destino de representación (con una vista de destino de representación), la textura 3D se comporta igual que una matriz de texturas 2D con n segmentos. El sector de representación concreto se elige desde la fase del sombreador de geometría.

No hay ningún concepto de una matriz de texturas 3D. Por lo tanto, un subrecurso de textura 3D es un nivel de mapa MIP único.

### <a name="span-idsubresourcesspanspan-idsubresourcesspanspan-idsubresourcesspansubresources"></a><span id="Subresources"></span><span id="subresources"></span><span id="SUBRESOURCES"></span>Metadata

La API de Direct3D hace referencia a recursos completos o a subconjuntos de recursos. Para especificar parte de los recursos, Direct3D ha acuñado el término *subrecursos*, que hace referencia a un subconjunto de un recurso.

Un búfer se define como un subrecurso único. Las texturas son un poco más complejas, ya que hay varios tipos de textura diferentes (1D, 2D, etc.), algunos de los cuales admiten niveles de mapas MIP o matrices de texturas. Para empezar por el caso más simple, una textura 1D se define como un subrecurso único, como se muestra en la siguiente ilustración.

![Ilustración de una textura 1D](images/d3d10-1d-texture.png)

Esto significa que la matriz de elementos de textura que conforman una textura 1D se encuentran en un único subrecurso.

Si se expande una textura 1D con tres niveles de mapas MIP, se puede visualizar de esta manera.

![Ilustración de una textura 1D con niveles de mapas MIP](images/d3d10-resource-texture1d.png)

Considérala una textura única que se compone de tres subtexturas. Cada subtextura se considera un subrecurso, por lo que esta textura 1D contiene 3 subrecursos. Una subtextura (o un subrecurso) se puede indexar con el nivel de la detalle (LOD) de una única textura. Cuando se utiliza una matriz de texturas, el acceso a una subtextura concreta requiere el LOD y la textura determinada. Como alternativa, la API combina estos dos fragmentos de información en un único índice de subrecursos basado en cero, como se muestra aquí.

![Ilustración de un índice de subrecursos basado en cero](images/d3d10-resource-texture1darray-sub-indexing.png)

### <a name="span-idselecting_subresourcesspanspan-idselecting_subresourcesspanspan-idselecting_subresourcesspanselecting-subresources"></a><span id="Selecting_Subresources"></span><span id="selecting_subresources"></span><span id="SELECTING_SUBRESOURCES"></span>Selección de Subrecursos

Algunas API acceden a un recurso completo, mientras que otras personas acceden a una parte de un recurso. Las API que acceden a una parte de un recurso suelen utilizar una descripción de la vista para especificar los subrecursos a los que van a acceder.

Estas figuras muestran los términos que se utilizan en una descripción de la vista al acceder a una matriz de texturas.

### <a name="span-idarray_slicespanspan-idarray_slicespanspan-idarray_slicespanarray-slice"></a><span id="Array_Slice"></span><span id="array_slice"></span><span id="ARRAY_SLICE"></span>Segmento de matriz

Dada una matriz de texturas, cada textura con mapas MIP, un segmento de matriz (representado por el rectángulo blanco) incluye una textura y todas sus subtexturas, como se muestra en la siguiente ilustración.

![Ilustración de un segmento de matriz](images/d3d10-resource-array-slice.png)

### <a name="span-idmip_slicespanspan-idmip_slicespanspan-idmip_slicespanmip-slice"></a><span id="Mip_Slice"></span><span id="mip_slice"></span><span id="MIP_SLICE"></span>Segmento MIP

Un segmento de mapa MIP (representado por el rectángulo blanco) incluye un nivel de mapa MIP para cada textura de una matriz, como se muestra en la siguiente ilustración.

![Ilustración de un segmento de mapa MIP](images/d3d10-resource-mip-slice.png)

### <a name="span-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanspan-idselecting_a_single_subresourcespanselecting-a-single-subresource"></a><span id="Selecting_a_Single_Subresource"></span><span id="selecting_a_single_subresource"></span><span id="SELECTING_A_SINGLE_SUBRESOURCE"></span>Selección de un único subrecurso

Puedes usar estos dos tipos de segmentos para elegir un único subrecurso, como se muestra en la siguiente ilustración.

![Ilustración de selección de un subrecurso mediante un segmento de matriz y segmento de mapa MIP](images/d3d10-resource-subresources-1.png)

### <a name="span-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanspan-idselecting_multiple_subresourcesspanselecting-multiple-subresources"></a><span id="Selecting_Multiple_Subresources"></span><span id="selecting_multiple_subresources"></span><span id="SELECTING_MULTIPLE_SUBRESOURCES"></span>Seleccionar varios Subrecursos

O bien, puedes utilizar estos dos tipos de segmentos con el número de niveles de mapas MIP o el número de texturas para seleccionar varios subrecursos.

![Ilustración de selección de varios subrecursos](images/d3d10-resource-subresources-2.png)

Independientemente del tipo de textura que utilices, con o sin mapas MIP, con o sin una matriz de textura, suelen proporcionarse funciones auxiliares para calcular el índice de un subrecurso particular.

### <a name="span-idtypedspanspan-idtypedspanspan-idtypedspanstrong-vs-weak-typing"></a><span id="Typed"></span><span id="typed"></span><span id="TYPED"></span>Strong frente a tipos débiles

La creación de un recurso completamente tipificado restringe el recurso al formato en el que se creó. Esto permite que el tiempo de ejecución optimice el acceso, especialmente si el recurso se crea con marcas que indican que la aplicación no lo puede asignar. Los recursos creados con un tipo específico no se pueden reinterpretar mediante el mecanismo de vista.

En un recurso sin tipos, el tipo de datos es desconocido cuando el recurso se crea por primera vez. La aplicación debe elegir entre los formatos sin tipos disponibles. Debes especificar el tamaño de la memoria que se va a asignar y si el tiempo de ejecución deberá generar las subtexturas en un mapa MIP.

Sin embargo, el formato de datos exacto (si la memoria se interpretará como enteros, valores de punto flotante, enteros sin signo, etc.) no se determina hasta que el recurso se enlaza a la canalización con una vista. Dado que el formato de textura sigue siendo flexible hasta que la textura se enlaza a la canalización, el recurso se denomina "almacenamiento con tipificación flexible". El almacenamiento con tipificación flexible tiene la ventaja de que se puede reutilizar o reinterpretar (en otro formato), siempre que los bits de componente del nuevo formato coincidan con el recuento de bits del formato antiguo.

Un único recurso se puede enlazar a varias fases de la canalización, siempre que cada una de ellas tenga una vista única que se ajuste a los formatos de cada ubicación. Por ejemplo, un recurso creado con un formato sin tipos puede usarse como un formato FLOAT y un formato UINT en diferentes ubicaciones de la canalización al mismo tiempo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos](resources.md)
