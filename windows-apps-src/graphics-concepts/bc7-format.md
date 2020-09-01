---
title: Formato BC7
description: El formato BC7 es un formato de compresión de textura que se usa para la compresión de alta calidad de datos RGB y RGBA.
ms.assetid: 788B6E8C-9A1F-45F9-BE49-742285E8D8A6
keywords:
- Formato BC7
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4ca22bc897acf0d4cbd924002de96d13f2ba8549
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159029"
---
# <a name="bc7-format"></a>Formato BC7


El formato BC7 es un formato de compresión de textura que se usa para la compresión de alta calidad de datos RGB y RGBA.

Para obtener información sobre los modos de bloqueo del formato BC7, consulte [Referencia del modo de formato BC7](/windows/desktop/direct3d11/bc7-format-mode-reference).

## <a name="span-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanspan-idabout-bc7-dxgi-format-bc7spanabout-bc7dxgi_format_bc7"></a><span id="About-BC7-DXGI-FORMAT-BC7"></span><span id="about-bc7-dxgi-format-bc7"></span><span id="ABOUT-BC7-DXGI-FORMAT-BC7"></span>Acerca del formato BC7/DXGI \_ \_ BC7


BC7 se especifica mediante los siguientes \_ valores de enumeración de formato de DXGI:

-   **DXGI \_ DAR formato a \_ BC7 \_ sin tipo**.
-   **DXGI \_ DAR formato a \_ BC7 \_ UNORM**.
-   **DXGI \_ FORMATO \_ BC7 \_ UNORM \_ sRGB**.

El formato BC7 se puede usar para los recursos de textura [Texture2D](/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (incluidas las matrices), Texture3D o TextureCube (incluidas las matrices). Del mismo modo, este formato se aplica a las superficies de mapa MIP asociadas a estos recursos.

BC7 usa un tamaño de bloque fijo de 16 bytes (128 bits) y un tamaño de mosaico fijo de textura 4x4. Al igual que con los formatos BC anteriores, las imágenes de textura mayores que el tamaño de mosaico compatible (4x4) se comprimen mediante varios bloques. Esta identidad de direccionamiento también se aplica a imágenes de tres dimensiones y matrices de mapas MIP, cubemaps y Texture. Todos los mosaicos de imagen deben tener el mismo formato.

BC7 comprime las imágenes de datos de punto fijo de tres canales (RGB) y de cuatro canales (RGBA). Normalmente, los datos de origen son de 8 bits por componente de color (canal), aunque el formato es capaz de codificar los datos de origen con más bits por componente de color. Todos los mosaicos de imagen deben tener el mismo formato.

El descodificador de BC7 realiza la descompresión antes de aplicar el filtrado de textura.

El hardware de descompresión de BC7 debe ser preciso. es decir, el hardware debe devolver resultados idénticos a los resultados devueltos por el descodificador descrito en este documento.

## <a name="span-idbc7-implementationspanspan-idbc7-implementationspanspan-idbc7-implementationspanbc7-implementation"></a><span id="BC7-Implementation"></span><span id="bc7-implementation"></span><span id="BC7-IMPLEMENTATION"></span>Implementación de BC7


Una implementación de BC7 puede especificar uno de los ocho modos, con el modo especificado en el bit menos significativo del bloque de 16 bytes (128 bits). El modo está codificado por cero o más bits con un valor de 0 seguido de 1.

Un bloque BC7 puede contener varios pares de extremos. Se puede hacer referencia al conjunto de índices que corresponden a un par de puntos de conexión como "subconjunto". Además, en algunos modos de bloqueo, la representación del punto de conexión se codifica en una forma a la que se puede hacer referencia como "RBGP", donde el bit "P" representa un bit menos significativo compartido para los componentes de color del punto de conexión. Por ejemplo, si la representación del punto de conexión para el formato es "RGB 5.5.5.1", el punto de conexión se interpreta como un valor de 6.6.6 RGB, en el que el estado del bit P define el bit menos significativo de cada componente. De forma similar, para los datos de origen con un canal alfa, si la representación para el formato es "RGBAP 5.5.5.5.1", el punto de conexión se interpreta como RGBA 6.6.6.6. Dependiendo del modo de bloqueo, puede especificar el bit menos significativo compartido para ambos extremos de un subconjunto individualmente (2 P bits por subconjunto) o compartirlos entre los puntos de conexión de un subconjunto (1 P bits por subconjunto).

En el caso de los bloques BC7 que no codifican explícitamente el componente alfa, un bloque BC7 está formado por bits de modo, bits de partición, puntos de conexión comprimidos, índices comprimidos y un bit P opcional. En estos bloques, los puntos de conexión tienen una representación solo de RGB y el componente alfa se descodifica como 1,0 para todos los textura de datos de origen.

En el caso de los bloques BC7 que tienen componentes de color y alfa combinados, un bloque consta de bits de modo, puntos de conexión comprimidos, índices comprimidos y bits de partición opcionales y un bit P. En estos bloques, los colores del punto de conexión se expresan en formato RGBA y los valores de los componentes alfa se interpolan junto con los valores del componente de color.

En el caso de los bloques BC7 que tienen componentes de color y alfa independientes, un bloque consta de bits de modo, bits de rotación, puntos de conexión comprimidos, índices comprimidos y un bit de selector de índice opcional. Estos bloques tienen un vector RGB efectivo \[ R, G, B \] y un canal alfa escalar \[ a una \] codificación separada.

En la tabla siguiente se enumeran los componentes de cada tipo de bloque.

| El bloque BC7 contiene...     | bits de modo | bits de rotación | bit de selector de índice | bits de partición | puntos de conexión comprimidos | Bit P    | índices comprimidos |
|---------------------------|-----------|---------------|--------------------|----------------|----------------------|----------|--------------------|
| solo componentes de color     | requerido  | N/D           | N/D                | requerido       | requerido             | opcional | requerido           |
| color + alfa combinado    | requerido  | N/D           | N/D                | opcional       | requerido             | opcional | requerido           |
| color y con separación alfa | requerido  | requerido      | opcional           | N/D            | requerido             | N/D      | requerido           |

 

BC7 define una paleta de colores en una línea aproximada entre dos extremos. El valor de modo determina el número de pares de puntos de conexión de interpolación por bloque. BC7 almacena un índice de paleta por textura.

Para cada subconjunto de índices que corresponde a un par de puntos de conexión, el codificador corrige el estado de un bit de los datos de índice comprimidos para ese subconjunto. Para ello, elige un orden de punto de conexión que permita que el índice del índice designado "Fix-up" establezca el bit más significativo en 0 y que se pueda descartar, lo que ahorra un bit por subconjunto. En el caso de los modos de bloqueo con un solo subconjunto, el índice de corrección siempre es el índice 0.

## <a name="span-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspanspan-iddecoding-the-bc7-formatspandecoding-the-bc7-format"></a><span id="Decoding-the-BC7-Format"></span><span id="decoding-the-bc7-format"></span><span id="DECODING-THE-BC7-FORMAT"></span>Descodificar el formato BC7


En el siguiente pseudocódigo se describen los pasos para descomprimir el píxel en (x, y) dado el bloque BC7 de 16 bytes.

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

El pseudocódigo al describe los pasos para descodificar completamente los componentes alfa y de color del punto de conexión para cada subconjunto dado un bloque BC7 de 16 bytes.

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

Para generar cada componente interpolado para cada subconjunto, use el siguiente algoritmo: permita que "c" sea el componente que se va a generar. permita que "E0" sea el componente del punto de conexión 0 del subconjunto; y permiten que "E1" sea el componente del punto de conexión 1 del subconjunto.

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

En el siguiente pseudocódigo se muestra cómo extraer índices y recuentos de bits para los componentes de color y alfa. Los bloques con color y alfa independientes también tienen dos conjuntos de datos de índice: uno para el canal vectorial y otro para el canal escalar. En el modo 4, estos índices son de distintos anchos (2 o 3 bits) y hay un selector de un bit que especifica si los datos de vectores o escalares usan los índices de 3 bits. (La extracción del recuento de bits alfa es similar a la extracción del recuento de bits de color, pero con un comportamiento inverso basado en el bit **idxMode** ).

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


Esta sección contiene una lista de los 8 modos de bloque y las asignaciones de bits para los bloques de formato de compresión de textura BC7.

Los colores de cada subconjunto dentro de un bloque se representan mediante dos colores de extremo explícitos y un conjunto de colores interpolados entre ellos. Dependiendo de la precisión del índice del bloque, cada subconjunto puede tener 4, 8 o 16 colores posibles.

### <a name="span-idmode-0spanspan-idmode-0spanspan-idmode-0spanmode-0"></a><span id="Mode-0"></span><span id="mode-0"></span><span id="MODE-0"></span>Modo 0

El modo de BC7 0 tiene las siguientes características:

-   Solo componentes de color (no alfa)
-   3 subconjuntos por bloque
-   Puntos de conexión de RGBP 4.4.4.1 con un bit P único por punto de conexión
-   índices de 3 bits
-   16 particiones

![diseño de bits de modo 0](images/bc7-mode0.png)

### <a name="span-idmode-1spanspan-idmode-1spanspan-idmode-1spanmode-1"></a><span id="Mode-1"></span><span id="mode-1"></span><span id="MODE-1"></span>Modo 1

El modo de BC7 1 tiene las siguientes características:

-   Solo componentes de color (no alfa)
-   2 subconjuntos por bloque
-   RGBP extremos de 6.6.6.1 con un bit P compartido por subconjunto)
-   índices de 3 bits
-   64 particiones

![diseño de bits de modo 1](images/bc7-mode1.png)

### <a name="span-idmode-2spanspan-idmode-2spanspan-idmode-2spanmode-2"></a><span id="Mode-2"></span><span id="mode-2"></span><span id="MODE-2"></span>Modo 2

El modo 2 de BC7 tiene las siguientes características:

-   Solo componentes de color (no alfa)
-   3 subconjuntos por bloque
-   Extremos 5.5.5 RGB
-   índices de 2 bits
-   64 particiones

![diseño de bits de modo 2](images/bc7-mode2.png)

### <a name="span-idmode-3spanspan-idmode-3spanspan-idmode-3spanmode-3"></a><span id="Mode-3"></span><span id="mode-3"></span><span id="MODE-3"></span>Modo 3

El modo de BC7 3 tiene las siguientes características:

-   Solo componentes de color (no alfa)
-   2 subconjuntos por bloque
-   Puntos de conexión de RGBP 7.7.7.1 con un bit P único por subconjunto)
-   índices de 2 bits
-   64 particiones

![diseño de modo de 3 bits](images/bc7-mode3.png)

### <a name="span-idmode-4spanspan-idmode-4spanspan-idmode-4spanmode-4"></a><span id="Mode-4"></span><span id="mode-4"></span><span id="MODE-4"></span>Modo 4

El modo de BC7 4 tiene las siguientes características:

-   Componentes de color con componentes alfa independientes
-   1 subconjunto por bloque
-   Extremos de color RGB 5.5.5
-   puntos de conexión alfa de 6 bits
-   16 índices de 2 bits
-   16 índices de 3 bits
-   rotación de componentes de 2 bits
-   Selector de índice de 1 bit (si se usan los índices de 2 o 3 bits)

![diseño de modo 4 bits](images/bc7-mode4.png)

### <a name="span-idmode-5spanspan-idmode-5spanspan-idmode-5spanmode-5"></a><span id="Mode-5"></span><span id="mode-5"></span><span id="MODE-5"></span>Modo 5

El modo de BC7 5 tiene las siguientes características:

-   Componentes de color con componentes alfa independientes
-   1 subconjunto por bloque
-   Extremos de color RGB 7.7.7
-   puntos de conexión alfa de 6 bits
-   16 índices de color de 2 bits
-   16 índices alfa de 2 bits
-   rotación de componentes de 2 bits

![diseño de bits de modo 5](images/bc7-mode5.png)

### <a name="span-idmode-6spanspan-idmode-6spanspan-idmode-6spanmode-6"></a><span id="Mode-6"></span><span id="mode-6"></span><span id="MODE-6"></span>Modo 6

El modo de BC7 6 tiene las siguientes características:

-   Componentes de color y alfa combinados
-   Un subconjunto por bloque
-   Puntos de conexión de color (y alfa) de RGBAP 7.7.7.7.1 (único P bits por extremo)
-   16 índices x 4 bits

![diseño de modo de 6 bits](images/bc7-mode6.png)

### <a name="span-idmode-7spanspan-idmode-7spanspan-idmode-7spanmode-7"></a><span id="Mode-7"></span><span id="mode-7"></span><span id="MODE-7"></span>Modo 7

El modo de BC7 7 tiene las siguientes características:

-   Componentes de color y alfa combinados
-   2 subconjuntos por bloque
-   Puntos de conexión de color (y alfa) de RGBAP 5.5.5.5.1 (único P bits por extremo)
-   índices de 2 bits
-   64 particiones

![diseño de bits de modo 7](images/bc7-mode7.png)

### <a name="span-idremarksspanspan-idremarksspanspan-idremarksspanremarks"></a><span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Sección

El modo 8 (el byte menos significativo está establecido en 0x00). No lo use en el codificador. Si pasa este modo al hardware, se devuelve un bloque inicializado con ceros.

En BC7, puede codificar el componente alfa de una de las siguientes maneras:

-   Tipos de bloque sin codificación de componentes alfa explícita. En estos bloques, los extremos de color tienen una codificación solo RGB, con el componente alfa descodificado en 1,0 para todos los textura.
-   Tipos de bloque con componentes de color y alfa combinados. En estos bloques, los valores de color del punto de conexión se especifican en el formato RGBA y los valores del componente alfa se interpolan junto con los valores de color.
-   Tipos de bloque con componentes de color y alfa separados. En estos bloques, los valores de color y alfa se especifican por separado, cada uno con su propio conjunto de índices. Como resultado, tienen un vector efectivo y un canal escalar codificados por separado, donde el vector especifica normalmente los canales de color \[ R, G, B \] y el valor escalar especifica el canal alfa \[ a \] . Para admitir este enfoque, se proporciona un campo de 2 bits independiente en la codificación, que permite la especificación de la codificación de canal independiente como un valor escalar. Como resultado, el bloque puede tener una de las siguientes cuatro representaciones diferentes de esta codificación alfa (como se indica en el campo de 2 bits):
    -   RGB | R: canal alfa independiente
    -   AGB | R: independiente del canal de color "rojo"
    -   RAB | G: canal de color "verde" independiente
    -   RGA | B: canal de color "azul" independiente

    El descodificador vuelve a ordenar el orden del canal a RGBA después de la descodificación, por lo que el formato de bloque interno no es visible para el desarrollador. Los negros con componentes de color y alfa independientes también tienen dos conjuntos de datos de índice: uno para el conjunto de canales de vectores y otro para el canal escalar. (En el caso del modo 4, estos índices son de distintos anchos \[ 2 o 3 bits \] . El modo 4 también contiene un selector de 1 bit que especifica si el vector o el canal escalar utiliza los índices de 3 bits).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Compresión de bloques de texturas](texture-block-compression.md)

 

 