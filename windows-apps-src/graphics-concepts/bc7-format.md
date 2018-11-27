---
title: Formato BC7
description: El formato BC7 es un formato de compresión de texturas usado para la compresión de alta calidad de datos RGB y RGBA.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- Formato BC7
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2c55a12dfa7757a48874b6857c95af592e818c2b
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7711936"
---
# <a name="bc7-format"></a>Formato BC7


El formato BC7 es un formato de compresión de texturas usado para la compresión de alta calidad de datos RGB y RGBA.

Para obtener información sobre los modos de bloques del formato BC7, consulta [Referencia al modo de formato BC7](https://msdn.microsoft.com/library/windows/desktop/hh308954).

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgiformatbc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>Acerca de BC7/DXGI\_FORMAT\_BC7


BC7 se especifica mediante los siguientes valores de enumeración DXGI\_FORMAT:

-   **DXGI\_FORMAT\_BC7\_TYPELESS**.
-   **DXGI\_FORMAT\_BC7\_UNORM**.
-   **DXGI\_FORMAT\_BC7\_UNORM\_SRGB**.

El formato BC7 puede usarse para los recursos de textura [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (incluidas matrices), Texture3D o TextureCube (incluidas matrices). Del mismo modo, este formato se aplica a las superficies de mapas MIP asociadas a estos recursos.

BC7 usa un tamaño de bloque fijo de 16bytes (128bits) y un tamaño de mosaico fijo de elementos de textura de 4×4. Al igual que con los formatos de BC anteriores, las imágenes de textura mayores que el tamaño de mosaico compatible (4×4) se comprimen mediante el uso de varios bloques. Esta identidad de direccionamiento también se aplica a imágenes tridimensionales, mapas MIP, mapas de cubo y matrices de texturas. Todos los mosaicos de imágenes deben tener el mismo formato.

BC7 comprime tanto imágenes de datos de punto fijo de tres canales (RGB) como de cuatro canales (RGBA). Por lo general, los datos de origen tienen 8bits por componente de color (canal), aunque el formato es capaz de codificar datos de origen con mayor cantidad de bits por componente de color. Todos los mosaicos de imágenes deben tener el mismo formato.

El descodificador BC7 realiza la descompresión antes de que se aplique un filtro de texturas.

El hardware de descompresión BC7 debe ser precisa en cuanto a los bits; es decir, el hardware debe devolver resultados que sean idénticos a los resultados devueltos por el descodificador descrito en este documento.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>Implementación de BC7


Una implementación de BC7 puede especificar uno de 8 modos, donde el modo se especifica en el bit menos significativo del bloque de 16bytes (128bits). El modo se codifica mediante cero o más bits con un valor de 0, seguido de un 1.

Un bloque BC7 puede contener varios pares de extremos. El conjunto de índices que corresponden a un par de extremos puede denominase "subconjunto". Además, en algunos modos de bloque, la representación del extremo se codifica en una forma que se puede denominar "RBGP", donde el bit "P" representa un bit compartido menos significativo para los componentes de color del extremo. Por ejemplo, si la representación de extremo para el formato es "RGB5.5.5.1", el extremo se interpreta como un valor RGB6.6.6, donde el estado del bit P define el bit menos significativo de cada componente. Del mismo modo, para los datos de origen con un canal alfa, si la representación para el formato es "RGBAP5.5.5.5.1", el extremo se interpreta como RGBA6.6.6.6. Según el modo del bloque, puedes especificar el bit compartido menos significativo para cualquiera de ambos extremos de un subconjunto individualmente (2 bits P por subconjunto), o compartido entre los extremos de un subconjunto (1 bit P por subconjunto).

Para los bloques BC7 que no codifican de forma explícita el componente alfa, un bloque BC7 consta de bits de modo, bits de partición, extremos comprimidos, índices comprimidos y un bit P opcional. En estos bloques, los extremos tienen una representación solo RGB y el componente alfa se descodifica como 1.0 para todos los elementos de textura en los datos de origen.

Para los bloques BC7 donde se han combinado los componentes de color y alfa, un bloque consiste en bits de modo, extremos comprimidos, índices comprimidos, bits de partición opcionales y un bit P. En estos bloques, los colores de extremo se expresan en formato RGBA, y los valores de los componentes alfa se interpolan junto con los valores de los componentes de color.

Para los bloques BC7 que tienen componentes de color y alfa independientes, un bloque consta de bits de modo, bits de rotación, extremos comprimidos, índices comprimidos y un bit selector de índice opcional. Estos bloques tienen un vector RGB eficaz \[R, G, B\] y un canal alfa escalar \[A\] codificado por separado.

La siguiente tabla enumera los componentes de cada tipo de bloque.

| El bloque BC7 contiene...     | bits de modo | bits de rotación | bit selector de índice | bits de partición | extremos comprimidos | bit P    | índices comprimidos |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| solo componentes de color     | obligatorio  | N/C           | N/C                | obligatorio       | obligatorio             | opcional | obligatorio           |
| color y alfa combinados    | obligatorio  | N/C           | N/C                | opcional       | obligatorio             | opcional | obligatorio           |
| color y alfa separados | obligatorio  | obligatorio      | opcional           | N/C            | obligatorio             | N/C      | obligatorio           |

 

BC7 define una paleta de colores en una línea aproximada entre dos extremos. El valor del modo determina el número de pares de extremos de interpolación por bloque. BC7 almacena un índice de paleta por elemento de textura.

Para cada subconjunto de índices que corresponde a un par de extremos, el codificador fija el estado de un bit de los datos de índices comprimidos para ese subconjunto. Para ello, elige un orden de los extremos que permite que el índice para el índice de "fijación" designado establezca su bit más significativo en 0, y que luego puede descartarse, lo que ahorra un bit por subconjunto. Para los modos de bloque con solo un subconjunto único, el índice de fijación es siempre el índice 0.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>Descodificar el formato BC7


El siguiente seudocódigo detalla los pasos para descomprimir el píxel en (x,y), dado el bloque BC7 de 16bytes.

``` syntax
decompress_bc7(x, y, block)
{
    mode = extract_mode(block);
    
    //decode partition data from explicit partition bits
    subset_index = 0;
    num_subsets = 1;
    
    if (mode.type == 0 OR == 1 OR == 2 OR == 3 OR == 7)
    {
        num_subsets = get_num_subsets(mode.type);
        partition_set_id = extract_partition_set_id(mode, block);
        subset_index = get_partition_index(num_subsets, partition_set_id, x, y);
    }
    
    //extract raw, compressed endpoint bits
    UINT8 endpoint_array[num_subsets][4] = extract_endpoints(mode, block);
    
    //decode endpoint color and alpha for each subset
    fully_decode_endpoints(endpoint_array, mode, block);
    
    //endpoints are now complete.
    UINT8 endpoint_start[4] = endpoint_array[2 * subset_index];
    UINT8 endpoint_end[4]   = endpoint_array[2 * subset_index + 1];
        
    //Determine the palette index for this pixel
    alpha_index     = get_alpha_index(block, mode, x, y);
    alpha_bitcount  = get_alpha_bitcount(block, mode);
    color_index     = get_color_index(block, mode, x, y);
    color_bitcount  = get_color_bitcount(block, mode);

    //determine output
    UINT8 output[4];
    output.rgb = interpolate(endpoint_start.rgb, endpoint_end.rgb, color_index, color_bitcount);
    output.a   = interpolate(endpoint_start.a,   endpoint_end.a,   alpha_index, alpha_bitcount);
    
    if (mode.type == 4 OR == 5)
    {
        //Decode the 2 color rotation bits as follows:
        // 00 – Block format is Scalar(A) Vector(RGB) - no swapping
        // 01 – Block format is Scalar(R) Vector(AGB) - swap A and R
        // 10 – Block format is Scalar(G) Vector(RAB) - swap A and G
        // 11 - Block format is Scalar(B) Vector(RGA) - swap A and B
        rotation = extract_rot_bits(mode, block);
        output = swap_channels(output, rotation);
    }
    
}
```

El seudocódigo siguiente describe los pasos para descodificar completamente los componentes de color y alfa del extremo para cada subconjunto, dado un bloque BC7 de 16bytes.

``` syntax
fully_decode_endpoints(endpoint_array, mode, block)
{
    //first handle modes that have P-bits
    if (mode.type == 0 OR == 1 OR == 3 OR == 6 OR == 7)
    {
        for each endpoint i
        {
            //component-wise left-shift
            endpoint_array[i].rgba = endpoint_array[i].rgba << 1;
        }
        
        //if P-bit is shared
        if (mode.type == 1) 
        {
            pbit_zero = extract_pbit_zero(mode, block);
            pbit_one = extract_pbit_one(mode, block);
            
            //rgb component-wise insert pbits
            endpoint_array[0].rgb |= pbit_zero;
            endpoint_array[1].rgb |= pbit_zero;
            endpoint_array[2].rgb |= pbit_one;
            endpoint_array[3].rgb |= pbit_one;  
        }
        else //unique P-bit per endpoint
        {  
            pbit_array = extract_pbit_array(mode, block);
            for each endpoint i
            {
                endpoint_array[i].rgba |= pbit_array[i];
            }
        }
    }

    for each endpoint i
    {
        // Color_component_precision & alpha_component_precision includes pbit
        // left shift endpoint components so that their MSB lies in bit 7
        endpoint_array[i].rgb = endpoint_array[i].rgb << (8 - color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a << (8 - alpha_component_precision(mode));

        // Replicate each component's MSB into the LSBs revealed by the left-shift operation above
        endpoint_array[i].rgb = endpoint_array[i].rgb | (endpoint_array[i].rgb >> color_component_precision(mode));
        endpoint_array[i].a = endpoint_array[i].a | (endpoint_array[i].a >> alpha_component_precision(mode));
    }
        
    //If this mode does not explicitly define the alpha component
    //set alpha equal to 1.0
    if (mode.type == 0 OR == 1 OR == 2 OR == 3)
    {
        for each endpoint i
        {
            endpoint_array[i].a = 255; //i.e. alpha = 1.0f
        }
    }
}
```

Para generar cada componente interpolado para cada subconjunto, usa el algoritmo siguiente: se denominará "c" al componente que se va a generar; se denominará "e0" al componente del extremo 0 del subconjunto; y se denominará "e1" al componente del extremo 1 del subconjunto.

``` syntax
UINT16 aWeight2[] = {0, 21, 43, 64};
UINT16 aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
UINT16 aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

UINT8 interpolate(UINT8 e0, UINT8 e1, UINT8 index, UINT8 indexprecision)
{
    if(indexprecision == 2)
        return (UINT8) (((64 - aWeights2[index])*UINT16(e0) + aWeights2[index]*UINT16(e1) + 32) >> 6);
    else if(indexprecision == 3)
        return (UINT8) (((64 - aWeights3[index])*UINT16(e0) + aWeights3[index]*UINT16(e1) + 32) >> 6);
    else // indexprecision == 4
        return (UINT8) (((64 - aWeights4[index])*UINT16(e0) + aWeights4[index]*UINT16(e1) + 32) >> 6);
}
```

El seudocódigo siguiente ilustra cómo extraer los índices y los números de bits para los componentes de color y alfa. Los bloques con color y alfa independientes también tienen dos conjuntos de datos de índice: uno para el canal vectorial y otro para el canal escalar. Para el modo 4, estos índices tienen anchos diferentes (2 o 3bits) y hay un selector de un bit que especifica si los datos vectoriales o escalares usan los índices de 3bits. (Extraer el número de bits de alfa es similar a extraer el número de bits de color, pero con el comportamiento inverso según el bit **idxMode**).

``` syntax
bitcount get_color_bitcount(block, mode)
{
    if (mode.type == 0 OR == 1)
        return 3;
    
    if (mode.type == 2 OR == 3 OR == 5 OR == 7)
        return 2;
    
    if (mode.type == 6)
        return 4;
        
    //The only remaining case is Mode 4 with 1-bit index selector
    idxMode = extract_idxMode(block);
    if (idxMode == 0)
        return 2;
    else
        return 3;
}
```

## <a name="span-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanspan-idbc7-format-mode-referencespanbc7-format-mode-reference"></a><span id="BC7-format-mode-reference"></span><span id="bc7-format-mode-reference"></span><span id="BC7-FORMAT-MODE-REFERENCE"></span>Referencia del modo de formato BC7


En esta sección se incluye una lista de los 8 modos de bloque y las asignaciones de bits para los bloques de formato de compresión de texturas BC7.

Los colores para cada subconjunto dentro de un bloque se representan mediante dos colores de extremo explícitos y un conjunto de colores interpolados entre ellos. Según la precisión del índice del bloque, cada subconjunto puede tener 4, 8 o 16colores posibles.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>Modo 0

El Modo 0 de BC7 tiene las siguientes características:

-   Solo componentes de color (sin alfa)
-   3 subconjuntos por bloque
-   Extremos RGBP 4.4.4.1 con un único bit P por extremo
-   Índices de 3 bits
-   16 particiones

![diseño del modo de 0 bits](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>Modo 1

El Modo 1 de BC7 tiene las siguientes características:

-   Solo componentes de color (sin alfa)
-   2 subconjuntos por bloque
-   Extremos de RGBP 6.6.6.1 con un bit P compartido por subconjunto
-   Índices de 3 bits
-   64 particiones

![diseño del modo de 1 bit](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>Modo 2

El Modo 2 de BC7 tiene las siguientes características:

-   Solo componentes de color (sin alfa)
-   3 subconjuntos por bloque
-   Extremos de RGB 5.5.5
-   Índices de 2 bits
-   64 particiones

![diseño del modo de 2 bits](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>Modo 3

El Modo 3 de BC7 tiene las siguientes características:

-   Solo componentes de color (sin alfa)
-   2 subconjuntos por bloque
-   Extremos de RGBP 7.7.7.1 con un bit P único por subconjunto
-   Índices de 2 bits
-   64 particiones

![diseño del modo de 3 bits](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>Modo 4

El Modo 4 de BC7 tiene las siguientes características:

-   Componentes de color con el componente alfa independiente
-   1 subconjunto por bloque
-   Extremos de color RGB 5.5.5
-   Extremos de alfa de 6 bits
-   16 índices de 2 bits
-   16 índices de 3 bits
-   Rotación de componente de 2 bits
-   Selector de índice de 1 bit (si se usan los índices de 2 o 3 bits)

![diseño del modo de 4 bits](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>Modo 5

El Modo 5 de BC7 tiene las siguientes características:

-   Componentes de color con el componente alfa independiente
-   1 subconjunto por bloque
-   Extremos de color RGB 7.7.7
-   Extremos de alfa de 6 bits
-   16 índices de color de 2 bits
-   16 índices de alfa de 2 bits
-   Rotación de componente de 2 bits

![diseño del modo de 5 bits](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>Modo 6

El Modo 6 de BC7 tiene las siguientes características:

-   Componentes de color y alfa combinados
-   Un subconjunto por bloque
-   Extremos de color (y alfa) RGBAP 7.7.7.7.1 (bit P único por extremo)
-   16 índices de 4 bits

![diseño del modo de 6 bits](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>Modo 7

El Modo 7 de BC7 tiene las siguientes características:

-   Componentes de color y alfa combinados
-   2 subconjuntos por bloque
-   Extremos de color (y alfa) RGBAP 5.5.5.5.1 (bit P único por extremo)
-   Índices de 2 bits
-   64 particiones

![diseño del modo de 7 bits](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Observaciones

El modo de 8 (el byte menos significativo se establece en 0x00) es reservado. No lo uses en el codificador. Si pasas este modo al hardware, se devuelve un bloque inicializado con todos ceros.

En BC7, puedes codificar el componente alfa en una de las siguientes maneras:

-   Tipos de bloque sin codificación explícita del componente alfa. En estos bloques, los extremos de color tienen codificación solo RGB, y el componente alfa está decodificado en 1.0 par todos los elementos de textura.
-   Tipos de bloque con componentes de color y alfa combinados. En estos bloques, los valores de color del extremo se especifican en el formato RGBA, y los valores de los componentes alfa se interpolan junto con los valores de color.
-   Tipos de bloque con componentes de color y alfa separados. En estos bloques, los valores de color y alfa se especifican por separado, cada uno con su propio conjunto de índices. Como resultado, tienen un vector eficaz y un canal escalar codificados por separado, donde el vector habitualmente especifica los canales de color \[R, G, B\] y el escalar especifica el canal alfa \[A\]. Para admitir este enfoque, se proporciona un campo de 2 bits independiente con la codificación, lo que permite la especificación de la codificación del canal independiente como valor escalar. Como consecuencia, el bloque puede tener una de las diferentes cuatro representaciones siguientes de esta codificación de alfa (como se indica en el campo de 2 bits):
    -   RGB|A: canal alfa independiente
    -   AGB|R: canal de color "rojo" independiente
    -   RAB|G: canal de color "verde" independiente
    -   RGA|B: canal de color "azul" independiente

    El descodificador reordena el orden de canal nuevamente en RGBA después de la descodificación, por lo que el formato del bloque interno es invisible para el desarrollador. Los negros con componentes de color y alfa independientes también tienen dos conjuntos de datos de índice: uno para el conjunto de vectores de canales y otro para el canal escalar. (En el caso del Modo 4, estos índices tienen anchos diferentes \[2 o 3 bits\]. El Modo 4 también contiene un selector de 1 bit que especifica si el vector o el canal escalar usa los índices de 3 bits).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Compresión de bloques de texturas](texture-block-compression.md)

 

 




