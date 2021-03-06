---
title: Formato BC6H
description: El formato BC6H es un formato de compresión de textura diseñado para admitir los espacios de colores de intervalos dinámicos (HDR) en los datos de origen.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- Formato BC6H
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5f53ebf6a7326bd9e6a99272c01d9eeb5c03f580
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165079"
---
# <a name="bc6h-format"></a>Formato BC6H


El formato BC6H es un formato de compresión de textura diseñado para admitir los espacios de colores de intervalos dinámicos (HDR) en los datos de origen.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgi_format_bc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>Acerca del formato BC6H/DXGI \_ \_ BC6H


El formato BC6H proporciona compresión de alta calidad para las imágenes que usan tres canales de color HDR, con un valor de 16 bits para cada canal de color del valor (16:16:16). No se admiten canales alfa.

BC6H se especifica mediante los siguientes \_ valores de enumeración de formato de DXGI:

-   **DXGI \_ DAR formato a \_ BC6H \_ sin tipo**.
-   **DXGI \_ DAR formato a \_ BC6H \_ UF16**. Este formato BC6H no usa un bit de signo en los valores de canal de color de punto flotante de 16 bits.
-   **DXGI \_ DAR formato a \_ BC6H \_ SF16**. Este formato BC6H usa un bit de signo en los valores de canal de color de punto flotante de 16 bits.

**Nota:**    El formato de punto flotante de 16 bits para los canales de color a menudo se conoce como formato de punto flotante "medio". Este formato tiene el siguiente diseño de bits:
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (unsigned float) | 5 bits de exponente + 11 bits de mantisa              |
| SF16 (valor Float con signo)   | 1 bit de signo + 5 bits de exponente + 10 bits de mantisa |

 

 

El formato BC6H se puede usar para los recursos de textura [Texture2D](/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (incluidas las matrices), Texture3D o TextureCube (incluidas las matrices). Del mismo modo, este formato se aplica a las superficies de mapa MIP asociadas a estos recursos.

BC6H usa un tamaño de bloque fijo de 16 bytes (128 bits) y un tamaño de mosaico fijo de textura 4x4. Al igual que con los formatos BC anteriores, las imágenes de textura mayores que el tamaño de mosaico compatible (4x4) se comprimen mediante varios bloques. Esta identidad de direccionamiento se aplica también a imágenes tridimensionales, mapas MIP, mapas de cubos y matrices de texturas. Todos los mosaicos de imagen deben tener el mismo formato.

Advertencias con el formato BC6H:

-   BC6H admite la desnormalización de punto flotante, pero no admite INF (infinito) y NaN (no un número). La excepción es el modo con signo de BC6H (DXGI \_ format \_ BC6H \_ SF16), que admite-INF (infinito negativo). Esta compatibilidad con-INF es simplemente un artefacto del mismo formato y no es compatible específicamente con los codificadores para este formato. En general, cuando un codificador encuentra datos de entrada de INF (positivos o negativos) o NaN, el codificador debe convertir los datos al valor de representación de no INF máximo permitido y asignar NaN a 0 antes de la compresión.
-   BC6H no es compatible con un canal alfa.
-   El descodificador de BC6H realiza la descompresión antes de realizar el filtrado de textura.
-   La descompresión de BC6H debe ser precisa de bits; es decir, el hardware debe devolver resultados idénticos al descodificador descrito en esta documentación.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>Implementación de BC6H


Un bloque BC6H consta de bits de modo, puntos de conexión comprimidos, índices comprimidos y un índice de partición opcional. Este formato especifica 14 modos diferentes.

Un color de punto de conexión se almacena como un tripledo RGB. BC6H define una paleta de colores en una línea aproximada en varios extremos de color definidos. Además, en función del modo, un icono se puede dividir en dos regiones o tratarse como una sola región, en la que un mosaico de dos regiones tiene un conjunto independiente de extremos de color para cada región. BC6H almacena un índice de paleta por textura.

En el caso de dos regiones, hay 32 particiones posibles.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>Descodificar el formato BC6H


En el siguiente pseudocódigo se muestran los pasos para descomprimir el píxel en (x, y) dado el bloque BC6H de 16 bytes.

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

La tabla siguiente contiene el recuento de bits y los valores para cada uno de los 14 formatos posibles para los bloques BC6H.

| Mode | Índices de partición | Partition | Extremos de color                  | Bits de modo      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 bits           | 5 bits    | 75 bits (10,555, 10,555, 10,555) | 2 bits (00)    |
| 2    | 46 bits           | 5 bits    | 75 bits (7666, 7666, 7666)       | 2 bits (01)    |
| 3    | 46 bits           | 5 bits    | 72 bits (11,555, 11,444, 11,444) | 5 bits (00010) |
| 4    | 46 bits           | 5 bits    | 72 bits (11,444, 11,555, 11,444) | 5 bits (00110) |
| 5    | 46 bits           | 5 bits    | 72 bits (11,444, 11,444, 11,555) | 5 bits (01010) |
| 6    | 46 bits           | 5 bits    | 72 bits (9555, 9555, 9555)       | 5 bits (01110) |
| 7    | 46 bits           | 5 bits    | 72 bits (8666, 8555, 8555)       | 5 bits (10010) |
| 8    | 46 bits           | 5 bits    | 72 bits (8555, 8666, 8555)       | 5 bits (10110) |
| 9    | 46 bits           | 5 bits    | 72 bits (8555, 8555, 8666)       | 5 bits (11010) |
| 10   | 46 bits           | 5 bits    | 72 bits (6666, 6666, 6666)       | 5 bits (11110) |
| 11   | 63 bits           | 0 bits    | 60 bits (10,10, 10,10, 10,10)    | 5 bits (00011) |
| 12   | 63 bits           | 0 bits    | 60 bits (11,9, 11,9, 11,9)       | 5 bits (00111) |
| 13   | 63 bits           | 0 bits    | 60 bits (12,8, 12,8, 12,8)       | 5 bits (01011) |
| 14   | 63 bits           | 0 bits    | 60 bits (16,4, 16,4, 16,4)       | 5 bits (01111) |

 

Los bits de modo pueden identificar de forma única cada formato de esta tabla. Los primeros diez modos se usan para los mosaicos de dos regiones y el campo de bits de modo puede tener dos o cinco bits de longitud. Estos bloques también tienen campos para los puntos de conexión de color comprimidos (bits 72 o 75), la partición (5 bits) y los índices de partición (46 bits).

En el caso de los extremos de color comprimido, los valores de la tabla anterior tienen en cuenta la precisión de los puntos de conexión RGB almacenados y el número de bits usados para cada valor de color. Por ejemplo, el modo 3 especifica un nivel de precisión de punto de conexión de color de 11, y el número de bits que se usa para almacenar los valores Delta de los puntos de conexión transformados para los colores rojo, azul y verde (5, 4 y 4, respectivamente). El modo 10 no utiliza la compresión Delta y, en su lugar, almacena explícitamente los cuatro extremos de color.

Los cuatro últimos modos de bloqueo se usan para los mosaicos de una región, donde el campo de modo es 5 bits. Estos bloques tienen campos para los puntos de conexión (60 bits) y los índices comprimidos (63 bits). El modo 11 (como el modo 10) no utiliza la compresión Delta y, en su lugar, almacena explícitamente ambos extremos de color.

Los modos 10011, 10111, 11011 y 11111 (no mostrados) están reservados. No los use en el codificador. Si el hardware pasa bloques con uno de estos modos especificados, el bloque descomprimido resultante debe contener todos los ceros en todos los canales excepto el canal alfa.

En el caso de BC6H, el canal alfa siempre debe devolver 1,0 independientemente del modo.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>Conjunto de particiones BC6H

Hay 32 posibles conjuntos de particiones para un mosaico de dos regiones y que se definen en la tabla siguiente. Cada bloque 4x4 representa una sola forma.

![tabla de conjuntos de particiones de bc6h](images/bc6h-partition-sets.png)

En esta tabla de conjuntos de particiones, la entrada en negrita y en subrayado es la ubicación del índice de corrección del subconjunto 1 (que se especifica con un bit menos). El índice de corrección del subconjunto 0 siempre es el índice 0, ya que las particiones siempre están organizadas de forma que el índice 0 siempre está en el subconjunto 0. El orden de las particiones va de arriba a izquierda a la derecha, de izquierda a derecha y de arriba abajo.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>Formato de punto de conexión comprimido BC6H


![campos de bits para formatos de punto de conexión comprimidos de bc6h](images/bc6h-headers-med.png)

En esta tabla se muestran los campos de bits de los puntos de conexión comprimidos como una función del formato del extremo, donde cada columna especifica una codificación y cada fila especifica un campo de bits. Este enfoque ocupa 82 bits para los mosaicos de dos regiones y 65 bits para los mosaicos de una región. Por ejemplo, los primeros cinco bits para la codificación de una región \[ 16 4 \] anterior (en concreto, la columna situada más a la derecha) son \[ los bits m 4:0 \] , los 10 bits siguientes son bits RW \[ 9:0 \] y así sucesivamente con los 6 últimos bits que contienen BW \[ 10:15 \] .

Los nombres de campo de la tabla anterior se definen de la siguiente manera:

| Campo | Variable          |
|-------|-------------------|
| m     | mode              |
| d     | Índice de la forma       |
| RW    | endpt \[ 0 \] . Un \[ 0\] |
| CSO    | endpt \[ 0 \] . B \[ 0\] |
| r    | endpt \[ 1 \] . Un \[ 0\] |
| RZ    | endpt \[ 1 \] . B \[ 0\] |
| GW    | endpt \[ 0 \] . \[1\] |
| GX    | endpt \[ 0 \] . B \[ 1\] |
| GY (    | endpt \[ 1 \] . \[1\] |
| GZ    | endpt \[ 1 \] . B \[ 1\] |
| ancho    | endpt \[ 0 \] . A \[ 2\] |
| BX    | endpt \[ 0 \] . B \[ 2\] |
| by    | endpt \[ 1 \] . A \[ 2\] |
| BZ    | endpt \[ 1 \] . B \[ 2\] |

 

Endpt \[ i \] , donde i es 0 o 1, hace referencia al conjunto de extremos 0 o 1, respectivamente.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>Extensión de signo para los valores de punto de conexión


En el caso de los mosaicos de dos regiones, hay cuatro valores de punto de conexión que se pueden firmar. Endpt \[ 0 \] . Un solo está firmado si el formato es un formato con signo; los otros puntos de conexión solo se firman si se transformó el punto de conexión, o si el formato es un formato con signo. En el código siguiente se muestra el algoritmo para extender el signo de los valores de punto de conexión de dos regiones.

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

En el caso de los mosaicos de una región, el comportamiento es el mismo, solo con endpt \[ 1 \] quitado.

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

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>Transformación de la inversión para los valores de punto de conexión


En el caso de los mosaicos de dos regiones, la transformación aplica el inverso de la codificación de diferencia, agregando el valor base en endpt \[ 0 \] . A las otras tres entradas para un total de 9 operaciones de adición. En la imagen siguiente, el valor base se representa como "a0" y tiene la precisión de punto flotante más alta. "A1", "B0" y "B1" son deltas calculados a partir del valor de delimitador y estos valores Delta se representan con una precisión inferior. (A0 corresponde a endpt \[ 0 \] . A, B0 corresponde a endpt \[ 0 \] . B, a1 corresponde a endpt \[ 1 \] . Y B1 corresponde a endpt \[ 1 \] . B).

![cálculo de valores de punto de conexión de inversión de transformación](images/bc6h-transform-inverse.png)

En el caso de los mosaicos de una región solo hay un desplazamiento Delta y, por lo tanto, solo 3 operaciones de adición.

El descompresor debe asegurarse de que los resultados de la transformación inversa no desbordarán la precisión de endpt \[ 0 \] . a. En el caso de un desbordamiento, los valores resultantes de la transformación inversa deben ajustarse en el mismo número de bits. Si la precisión de a0 es "p" bits, el algoritmo de transformación es:

`B0 = (B0 + A0) & ((1 << p) - 1)`

En el caso de los formatos con signo, los resultados del cálculo Delta también se deben firmar. Si la operación de extensión de signo considera la ampliación de ambos signos, donde 0 es positivo y 1 es negativo, la extensión de signo de 0 se encarga de la abrazadera anterior. De igual forma, después de la abrazadera anterior, solo es necesario firmar el valor 1 (negativo).

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Descuantificación de los extremos de color


Dados los puntos de conexión sin comprimir, el paso siguiente consiste en realizar una descuantificación inicial de los extremos de color. Esto implica tres pasos:

-   Una descuantificación de las paletas de colores
-   Interpolación de las paletas
-   Finalización de la descuantificación

La separación del proceso de descuantificación en dos partes (descuantificación de la paleta de colores antes de la interpolación y la descuantificación final después de la interpolación) reduce el número de operaciones de multiplicación necesarias cuando se compara con un proceso de descuantificación completo antes de la interpolación de la paleta.

En el código siguiente se muestra el proceso para recuperar las estimaciones de los valores de color de 16 bits originales y, a continuación, usar los valores de peso proporcionados para agregar 6 valores de color adicionales a la paleta. La misma operación se realiza en cada canal.

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

En el siguiente ejemplo de código se muestra el proceso de interpolación, con las siguientes observaciones:

-   Puesto que el intervalo completo de valores de color para la función de **descuantificación** (a continuación) está comprendido entre-32768 y 65535, el interpolador se implementa mediante la aritmética con signo de 17 bits.
-   Después de la interpolación, los valores se pasan a la función **Finish \_ uncuantificate** (descrita en el tercer ejemplo de esta sección), que aplica el ajuste de escala final.
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

**la \_ descuantificación finalizar** se llama después de la interpolación de la paleta. La función **uncuantificate** pospone el ajuste de escala en 31/32 para signed, 31/64 para sin signo. Este comportamiento es necesario para obtener el valor final en el intervalo medio válido (-0x7BFF ~ 0x7BFF) después de que se complete la interpolación de la paleta con el fin de reducir el número de multiplicaciones necesarias. **finalizar \_ descuantificar** aplica el ajuste de escala final y devuelve un valor **Short sin signo** que se reinterpreta en la **mitad**.

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

 

 