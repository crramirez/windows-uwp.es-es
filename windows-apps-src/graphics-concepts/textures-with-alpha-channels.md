---
title: Texturas con canales alfa
description: Hay dos formas de codificar mapas de texturas que exhiben una transparencia más compleja.
ms.assetid: 768A774A-4F21-4DDE-B863-14211DA92926
keywords:
- Texturas con canales alfa
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88d150383d2be219e7f382e0e690771acbc9d2ee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651480"
---
# <a name="textures-with-alpha-channels"></a>Texturas con canales alfa


Hay dos formas de codificar mapas de texturas que exhiben una transparencia más compleja. En cada caso, un bloque que describe la transparencia precede el bloque de 64 bits ya descrito. La transparencia se representa como un mapa de bits de 4×4 con 4 bits por píxel (codificación explícita), o con menos bits y una interpolación lineal que es similar a la que se usa para la codificación del color.

El bloque de transparencia y el bloque de color se organizan tal como se muestra en la siguiente tabla.

| Dirección de Word | Bloque de 64 bits                      |
|--------------|-----------------------------------|
| 3:0          | Bloque de transparencia                |
| 7:4          | Bloque de 64 bits ya descrito |

 

## <a name="span-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanspan-idexplicit-texture-encodingspanexplicit-texture-encoding"></a><span id="Explicit-Texture-Encoding"></span><span id="explicit-texture-encoding"></span><span id="EXPLICIT-TEXTURE-ENCODING"></span>Codificación de textura explícita


Para la codificación de textura explícita (formato BC2), los componentes alfabéticos de los elementos de textura que describen la transparencia se codifican en un mapa de bits de 4 x 4, con 4 bits por elemento de textura. Estos 4 bits pueden lograrse mediante diversos medios, como la interpolación de colores o el uso de los 4 bits más importantes de los datos alfabéticos. Sea cual sea el modo en que se generen, se usan tal como están, sin ninguna forma de interpolación.

El siguiente diagrama muestra un bloque de transparencia de 64 bits.

![Diagrama de un bloque de transparencia de 64 bits](images/colors4.png)

**Tenga en cuenta**    los cuatro bits más significativos utiliza el método de compresión de Direct3D.

 

Las siguientes tablas muestran cómo se coloca la información alfabética en la memoria para cada palabra de 16 bits.

Diseño de la palabra 0:

| Bits          | Alfa      |
|---------------|------------|
| 3:0 (LSB\*)   | \[0\]\[0\] |
| 7:4           | \[0\]\[1\] |
| 11:8          | \[0\]\[2\] |
| 12:15 (MSB\*) | \[0\]\[3\] |

 

\*bit menos significativo, más importantes de bits (MSB)

Diseño de la palabra 1:

| Bits        | Alfa      |
|-------------|------------|
| 3:0 (LSB)   | \[1\]\[0\] |
| 7:4         | \[1\]\[1\] |
| 11:8        | \[1\]\[2\] |
| 15:12 (MSB) | \[1\]\[3\] |

 

Diseño de la palabra 2:

| Bits        | Alfa      |
|-------------|------------|
| 3:0 (LSB)   | \[2\]\[0\] |
| 7:4         | \[2\]\[1\] |
| 11:8        | \[2\]\[2\] |
| 15:12 (MSB) | \[2\]\[3\] |

 

Diseño de la palabra 3:

| Bits        | Alfa      |
|-------------|------------|
| 3:0 (LSB)   | \[3\]\[0\] |
| 7:4         | \[3\]\[1\] |
| 11:8        | \[3\]\[2\] |
| 15:12 (MSB) | \[3\]\[3\] |

 

En este formato no se utiliza la comparación de color usada en BC1 para determinar si el elemento de textura es transparente. Se supone que sin la comparación de color, los datos de color siempre se tratan como si estuvieran en modo de 4 colores.

## <a name="span-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanspan-idthree-bit-linear-alpha-interpolationspanthree-bit-linear-alpha-interpolation"></a><span id="Three-Bit-Linear-Alpha-Interpolation"></span><span id="three-bit-linear-alpha-interpolation"></span><span id="THREE-BIT-LINEAR-ALPHA-INTERPOLATION"></span>Interpolación lineal alfa tres bits


La codificación de la transparencia para el formato BC3 se basa en un concepto similar a la codificación lineal usada para el color. En los primeros 8 bytes del bloque se almacenan 2 valores alfa de 8 bits y un mapa de bits de 4 x 4 con 3 bits por píxel. Los valores alfa representativos se usan para interpolar los valores alfa intermedios. Existe información adicional disponible sobre la forma en que se almacenan los 2 valores alfa. Si alfa\_0 es mayor que alfa\_1, entonces seis valores alfabéticos intermedios se crean mediante la interpolación. De lo contrario, se interpolan 4 valores alfa intermedios entre los extremos alfa especificados. Los 2 valores alfa implícitos adicionales son 0 (totalmente transparente) y 255 (totalmente opaco).

El siguiente ejemplo de código ilustra este algoritmo.

```
// 8-alpha or 6-alpha block?    
if (alpha_0 > alpha_1) {    
    // 8-alpha block:  derive the other six alphas.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (6 * alpha_0 + 1 * alpha_1 + 3) / 7;    // bit code 010
    alpha_3 = (5 * alpha_0 + 2 * alpha_1 + 3) / 7;    // bit code 011
    alpha_4 = (4 * alpha_0 + 3 * alpha_1 + 3) / 7;    // bit code 100
    alpha_5 = (3 * alpha_0 + 4 * alpha_1 + 3) / 7;    // bit code 101
    alpha_6 = (2 * alpha_0 + 5 * alpha_1 + 3) / 7;    // bit code 110
    alpha_7 = (1 * alpha_0 + 6 * alpha_1 + 3) / 7;    // bit code 111  
}    
else {  
    // 6-alpha block.    
    // Bit code 000 = alpha_0, 001 = alpha_1, others are interpolated.
    alpha_2 = (4 * alpha_0 + 1 * alpha_1 + 2) / 5;    // Bit code 010
    alpha_3 = (3 * alpha_0 + 2 * alpha_1 + 2) / 5;    // Bit code 011
    alpha_4 = (2 * alpha_0 + 3 * alpha_1 + 2) / 5;    // Bit code 100
    alpha_5 = (1 * alpha_0 + 4 * alpha_1 + 2) / 5;    // Bit code 101
    alpha_6 = 0;                                      // Bit code 110
    alpha_7 = 255;                                    // Bit code 111
}
```

El diseño de la memoria del bloque alfa es la siguiente:

| Byte | Alfa                                                          |
|------|----------------------------------------------------------------|
| 0    | Alfa\_0                                                       |
| 1    | Alfa\_1                                                       |
| 2    | \[0\]\[2\] (2 MSBs), \[0\]\[1\], \[0\]\[0\]                    |
| 3    | \[1\]\[1\] (1 MSB), \[1\]\[0\], \[0\]\[3\], \[0\] \[2\] (1 LSB) |
| 4    | \[1\]\[3\], \[1\]\[2\], \[1\]\[1\] (2 LSBs)                    |
| 5    | \[2\]\[2\] (2 MSBs), \[2\]\[1\], \[2\]\[0\]                    |
| 6    | \[3\]\[1\] (1 MSB), \[3\]\[0\], \[2\]\[3\], \[2\] \[2\] (1 LSB) |
| 7    | \[3\]\[3\], \[3\]\[2\], \[3\]\[1\] (2 LSBs)                    |

 

Con estos formatos no se utiliza la comparación de color usada en BC1 para determinar si el elemento de textura es transparente. Se supone que sin la comparación de color, los datos de color siempre se tratan como si estuvieran en modo de 4 colores.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de textura comprimido](compressed-texture-resources.md)

 

 




