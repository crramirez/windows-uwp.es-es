---
title: Formato BC6H
description: El formato de BC6H es un formato de compresión de texturas diseñado para admitir los espacios de color de alto rango dinámico (HDR) en los datos de origen.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- Formato BC6H
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: be88f06cd5893f2f67697a54754826440bdf7d18
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6039155"
---
# <a name="bc6h-format"></a>Formato BC6H


El formato de BC6H es un formato de compresión de texturas diseñado para admitir los espacios de color de alto rango dinámico (HDR) en los datos de origen.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>Acerca de BC6H/DXGI\_FORMAT\_BC6H


El formato BC6H proporciona una compresión de alta calidad para imágenes que usan tres canales de color HDR, con un valor de 16bits para cada canal de color del valor (16: 16:16). No existe compatibilidad para un canal alfa.

BC6H se especifica mediante los siguientes valores de enumeración DXGI\_FORMAT:

-   **DXGI\_FORMAT\_BC6H\_TYPELESS**.
-   **DXGI\_FORMAT\_BC6H\_UF16**. Este formato BC6H no usa un bit de signo en los valores de canal de color de punto flotante de 16bits.
-   **DXGI\_FORMAT\_BC6H\_SF16**. Este formato BC6H usa un bit de signo en los valores de canal de color de punto flotante de 16bits.

**Nota**  el formato de punto de canales de color flotante de 16 bits se conoce como formato de punto flotante "medio". Este formato tiene el siguiente diseño de bits:
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (float sin signo) | 5 bits de exponente + 11 bits de mantisa              |
| SF16 (float con signo)   | 1 bit de signo + 5 bits de exponente + 10 bits de mantisa |

 

 

El formato BC6H puede usarse para los recursos de textura [Texture2D](https://msdn.microsoft.com/library/windows/desktop/bb205277) (incluidas matrices), Texture3D o TextureCube (incluidas matrices). Del mismo modo, este formato se aplica a las superficies de mapas MIP asociadas a estos recursos.

BC6H usa un tamaño de bloque fijo de 16bytes (128bits) y un tamaño de mosaico fijo de elementos de textura de 4×4. Al igual que con los formatos de BC anteriores, las imágenes de textura mayores que el tamaño de mosaico compatible (4×4) se comprimen mediante el uso de varios bloques. Esta identidad de direccionamiento también se aplica a imágenes tridimensionales, mapas MIP, mapas de cubo y matrices de texturas. Todos los mosaicos de imágenes deben tener el mismo formato.

Advertencias con el formato BC6H:

-   BC6H admite la desnormalización del punto flotante, pero no admite INF (infinito) y NaN (no es un número). La excepción es el modo con signo de BC6H (DXGI\_FORMAT\_BC6H\_SF16), que es compatible con -INF (infinito negativo). Esta compatibilidad con -INF es simplemente un artefacto del formato en sí y no se admiten específicamente en codificadores para este formato. En general, cuando un codificador encuentra datos de entrada INF (positivo o negativo) o NaN, el codificador debe convertir esos datos en el valor máximo permitido de representación distinto de INF, y asignar 0 a NaN antes de la compresión.
-   BC6H no admite un canal alfa.
-   El descodificador BC6H realiza la descompresión antes de realizar el filtrado de texturas.
-   La descompresión de BC6H debe ser precisa en términos de bits; es decir, el hardware debe devolver resultados que sean idénticos al descodificador que se describe en esta documentación.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>Implementación de BC6H


Un bloque BC6H consta de bits de modo, extremos comprimidos, índices comprimidos y un índice de partición opcional. Este formato especifica 14 modos diferentes.

Un color de extremo se almacena como trío RGB. BC6H define una paleta de colores en una línea aproximada a lo largo de una serie de extremos de color definidos. Además, según el modo, un mosaico puede dividirse en dos regiones o tratarse como una sola región, donde un mosaico de dos regiones tiene un conjunto distinto de extremos de color para cada región. BC6H almacena un índice de paleta por elemento de textura.

En el caso de dos regiones, hay 32 particiones posibles.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>Descodificar el formato BC6H


El siguiente seudocódigo muestra los pasos para descomprimir el píxel en (x,y), dado el bloque BC6H de 16bytes.

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

La tabla siguiente contiene los valores y el número de bits para cada uno de los 14 formatos posibles para los bloques BC6H.

| Modo | Índices de partición | Partición | Extremos de color                  | Bits de modo      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 bits           | 5 bits    | 75 bits (10.555, 10.555, 10.555) | 2 bits (00)    |
| 2    | 46 bits           | 5 bits    | 75 bits (7666, 7666, 7666)       | 2 bits (01)    |
| 3    | 46 bits           | 5 bits    | 72 bits (11.555, 11.444, 11.444) | 5 bits (00010) |
| 4    | 46 bits           | 5 bits    | 72 bits (11.444, 11.555, 11.444) | 5 bits (00110) |
| 5    | 46 bits           | 5 bits    | 72 bits (11.444, 11.444, 11.555) | 5 bits (01010) |
| 6    | 46 bits           | 5 bits    | 72 bits (9555, 9555, 9555)       | 5 bits (01110) |
| 7    | 46 bits           | 5 bits    | 72 bits (8666, 8555, 8555)       | 5 bits (10010) |
| 8    | 46 bits           | 5 bits    | 72 bits (8555, 8666, 8555)       | 5 bits (10110) |
| 9    | 46 bits           | 5 bits    | 72 bits (8555, 8555, 8666)       | 5 bits (11010) |
| 10   | 46 bits           | 5 bits    | 72 bits (6666, 6666, 6666)       | 5 bits (11110) |
| 11   | 63 bits           | 0 bits    | 60 bits (10.10, 10.10, 10.10)    | 5 bits (00011) |
| 12   | 63 bits           | 0 bits    | 60 bits (11.9, 11.9, 11.9)       | 5 bits (00111) |
| 13   | 63 bits           | 0 bits    | 60 bits (12.8, 12.8, 12.8)       | 5 bits (01011) |
| 14   | 63 bits           | 0 bits    | 60 bits (16.4, 16.4, 16.4)       | 5 bits (01111) |

 

Cada formato de esta tabla puede identificase de manera exclusiva por los bits de modo. Los primeros diez modos se usan para los mosaicos de dos regiones, y el campo de bits de modo puede tener una longitud de dos o cinco bits. Estos bloques también tienen campos para los extremos de color comprimidos (72 o 75bits), la partición (5bits) y los índices de partición (46bits).

Para los extremos de color comprimidos, los valores en la tabla anterior dan cuenta de la precisión de los extremos RGB almacenados y del número de bits usados para cada valor de color. Por ejemplo, el modo 3 especifica un nivel de precisión del extremo de color de 11, y el número de bits usado para almacenar los valores delta de los extremos transformados para los colores rojo, azul y verde (5, 4 y 4, respectivamente). El modo 10 no usa la compresión delta y, en su lugar, almacena los cuatro extremos de color de forma explícita.

Los últimos cuatro modos de bloque se usan para mosaicos de una región, donde el campo de modo es de 5bits. Estos bloques tienen campos para los extremos (60bits) y los índices comprimidos (63bits). El modo 11 (al igual que el modo 10) no usa la compresión delta y, en su lugar, almacena ambos extremos de color de manera explícita.

Los modos 10011, 10111, 11011 y 11111 (no se muestran) están reservados. No los uses en el codificador. Si se pasan al hardware bloques con uno de estos modos especificados, el bloque descomprimido resultante debe contener todos ceros en todos los canales, excepto en el canal alfa.

Para BC6H, el canal alfa siempre debe devolver 1.0, independientemente del modo.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>Conjunto de partición de BC6H

Hay 32 conjuntos de partición posibles para un mosaico de dos regiones, y que se definen en la siguiente tabla. Cada bloque de 4×4 representa una sola forma.

![tabla de los conjuntos de particiones de bc6h](images/bc6h-partition-sets.png)

En esta tabla de conjuntos de particiones, la entrada en negrita y subrayada es la ubicación del índice de corrección del subconjunto1 (que se especifica con un bit menos). El índice de la corrección del subconjunto 0 es siempre el índice0, ya que la partición siempre se organiza de modo que ese índice0 esté siempre en el subconjunto0. El orden de las particiones va de la parte superior izquierda a la inferior derecha, y se mueve de izquierda a derecha y luego de arriba abajo.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>Formato de extremo comprimido BC6H


![campos de bits para formatos de extremo comprimidos bc6h](images/bc6h-headers-med.png)

En esta tabla se muestran los campos de bits para los extremos comprimidos como una función del formato de extremo, donde cada columna especifica una codificación y cada fila especifica un campo de bits. Este enfoque ocupa hasta 82bits para los mosaicos de dos regiones y 65bits para los mosaicos de una región. Por ejemplo, los primeros 5bits para la codificación de una región \[16 4\] anterior (en concreto, la columna de más a la derecha) son bits m\[4:0\], los siguientes 10bits son bits rw\[9:0\] y así sucesivamente con los últimos 6bits que contiene bw\[10:15\].

Los nombres de campo en la tabla anterior se definen como sigue:

| Campo | Variable          |
|-------|-------------------|
| m     | mode              |
| d     | shape index       |
| rw    | endpt\[0\].A\[0\] |
| rx    | endpt\[0\].B\[0\] |
| ry    | endpt\[1\].A\[0\] |
| rz    | endpt\[1\].B\[0\] |
| gw    | endpt\[0\].A\[1\] |
| gx    | endpt\[0\].B\[1\] |
| gy    | endpt\[1\].A\[1\] |
| gz    | endpt\[1\].B\[1\] |
| bw    | endpt\[0\].A\[2\] |
| bx    | endpt\[0\].B\[2\] |
| by    | endpt\[1\].A\[2\] |
| bz    | endpt\[1\].B\[2\] |

 

Endpt\[i\], donde i es 0 o 1, se refiere al conjunto de extremos 0 o 1, respectivamente.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>Extensión de signo para valores de extremos


Para los mosaicos de dos regiones, hay cuatro valores de extremos que pueden tener una extensión de signo. Endpt\[0\].A tiene signo solo si el formato es un formato con signo; los otros extremos tienen signo solo si el extremo se transformó o si el formato es con signo. El siguiente código muestra el algoritmo para extender el signo de valores de extremos de dos regiones.

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

Para los mosaicos de una región, el comportamiento es el mismo, solo que se quita endpt\[1\].

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>Transformación de inversión para los valores de extremos


Para los mosaicos de dos regiones, la transformación aplica la inversa de la codificación de diferencias, suma el valor base endpt\[0\].A a las otras tres entradas para un total de 9 operaciones de suma. En la imagen siguiente, el valor base se representa como "A0" y tiene la mayor precisión del punto flotante. "A1", "B0" y "B1" son todos deltas calculados desde el valor de anclaje, y estos valores delta se representan con una menor precisión. (A0 corresponde a endpt\[0\].A, B0 corresponde a endpt\[0\].B, A1 corresponde a endpt\[1\].A, y B1 corresponde a endpt\[1\].B).

![cálculo de valores de extremo de transformación de inversión](images/bc6h-transform-inverse.png)

Para los mosaicos de una región hay solo un desplazamiento de delta y, por tanto, solo 3 operaciones de suma.

El descompresor debe asegurarse de que los resultados de la transformación inversa no desborden la precisión de endpt\[0\].a. En caso de desbordamiento, los valores resultantes de la transformación inversa deben ajustarse en el mismo número de bits. Si la precisión de A0 es "p" bits, el algoritmo de transformación es:

`B0 = (B0 + A0) & ((1 << p) - 1)`

Para formatos con signo, también debe extenderse el signo de los resultados del cálculo delta. Si la operación de extensión de signo considera extender ambos signos, donde 0 es positivo y 1 es negativo, la extensión de signo de 0 se encarga del clamp anterior. De igual forma, después el clamp anterior, solo se debe extender el signo de un valor de 1 (negativo).

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Descuantificación de extremos de color


Dados los extremos sin comprimir, el siguiente paso es realizar una descuantificación inicial de los extremos de color. Esto implica tres pasos:

-   Una descuantificación de las paletas de colores
-   La interpolación de las paletas
-   La finalización de la descuantificación

Separar el proceso de descuantificación en dos partes (descuantificación de la paleta de colores antes de la interpolación y la descuantificación final después de la interpolación) reduce el número de operaciones de multiplicación necesarias en comparación con un proceso de descuantificación completo antes de la interpolación de la paleta.

El siguiente código muestra el proceso para recuperar las estimaciones de los valores de color de 16bits originales, para luego usar los valores de peso proporcionados para sumar 6 valores de color adicionales a la paleta. La misma operación se realiza en cada canal.

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

El siguiente código de ejemplo muestra el proceso de interpolación, con las observaciones siguientes:

-   Dado que el intervalo completo de los valores de color para la función **unquantize** (abajo) está entre -32768 y 65535, el interpolador se implementa mediante aritmética con signo de 17bits.
-   Después de la interpolación, los valores se pasan a la función **finish\_unquantize** (descrita en el tercer ejemplo de esta sección), que aplica el ajuste de escala final.
-   Todos los descompresores de hardware son necesarios para devolver resultados de bits precisos con estas funciones.

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

Se llama a **finish\_unquantize** después de la interpolación de la paleta. La función **unquantize** pospone el ajuste de escala en 31/32 para con signo, 31/64 para sin signo. Este comportamiento es necesario para obtener el valor final en el intervalo medio válido (-0x7BFF ~ 0x7BFF) una vez completada la interpolación de la paleta con el fin de reducir el número de multiplicaciones necesarias. **finish\_unquantize** aplica el ajuste de escala final y devuelve un valor **unsigned short** que se reinterpreta como **half**.

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Compresión de bloques de texturas](texture-block-compression.md)

 

 




