---
title: Compresión de bloques
description: La compresión de bloques es una técnica de compresión de texturas con pérdida de información para reducir la superficie de memoria y el tamaño de la textura, lo que da un aumento del rendimiento. Una textura comprimida en bloques puede ser menor que una textura con 32bits por color.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- Compresión de bloques
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4c959ced5ada9145ca494dd023c9aa802d7dccc2
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5544588"
---
# <a name="block-compression"></a>Compresión de bloques


La compresión de bloques es una técnica de compresión de texturas con pérdida de información para reducir la superficie de memoria y el tamaño de la textura, lo que da un aumento del rendimiento. Una textura comprimida en bloques puede ser menor que una textura con 32bits por color.

La compresión de bloques es una técnica de compresión de texturas para reducir el tamaño de las texturas. Cuando se compara con una textura con 32bits por color, una textura comprimida en bloques puede ser hasta un 75% más pequeña. Las aplicaciones suelen ver un aumento en el rendimiento al usar la compresión de bloques debido a la menor superficie de memoria.

Si bien genera pérdida de información, la compresión de bloques funciona bien y se recomienda para todas las texturas que la canalización transforma y filtra. Las texturas que están asignadas directamente a la pantalla (elementos de la interfaz de usuario, como iconos y texto) no son buenas opciones para la compresión, ya que los artefactos son más perceptibles.

Una textura comprimida por bloques debe crearse como un múltiplo de tamaño 4 en todas las dimensiones y no puede usarse como salida de la canalización.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>Cómo funciona compresión de bloques


La compresión de bloques es una técnica para reducir la cantidad de memoria necesaria para almacenar los datos de color. Al almacenar algunos colores en su tamaño original y otros colores mediante un esquema de codificación, puedes reducir considerablemente la cantidad de memoria necesaria para almacenar la imagen. Dado que el hardware automáticamente descodifica los datos comprimidos, no hay ninguna disminución del rendimiento por el uso de texturas comprimidas.

Para ver cómo funciona la compresión, observa los dos ejemplos siguientes. En el primer ejemplo se describe la cantidad de memoria usada para almacenar datos sin comprimir; en el segundo ejemplo se describe la cantidad de memoria usada para almacenar datos comprimidos.

-   [Almacenamiento de datos sin comprimir](#storing-uncompressed-data)
-   [Almacenamiento de datos comprimidos](#storing-compressed-data)

### <a name="span-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoringuncompresseddataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>Almacenamiento de datos sin comprimir

La siguiente ilustración representa una textura de 4×4 sin comprimir. Supongamos que cada color contiene un único componente de color (por ejemplo, rojo) y se almacena en un solo byte de memoria.

![una textura de 4×4 sin comprimir](images/d3d10-block-compress-1.png)

Los datos sin comprimir se disponen secuencialmente en la memoria y requieren 16bytes, como se muestra en la ilustración siguiente.

![datos sin comprimir en la memoria secuencial](images/d3d10-block-compress-2.png)

### <a name="span-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoringcompresseddataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>Almacenamiento de datos comprimidos

Ahora que has visto cuánta memoria usa una imagen sin comprimir, echa un vistazo a cuánta memoria ahorra una imagen comprimida. El formato de compresión [BC1](#bc1) almacena 2 colores (de 1byte cada uno) y 16índices de 3bits (48bits o 6bytes) que se usan para interpolar los colores originales en la textura, como se muestra en la siguiente ilustración.

![el formato de compresión bc1](images/d3d10-block-compress-3.png)

El espacio total necesario para almacenar los datos comprimidos es de 8bytes, lo que supone un ahorro de memoria del 50% en comparación con el ejemplo sin comprimir. El ahorro es aún mayor cuando se usa más de un componente de color.

El ahorro considerable de memoria que se logra con la compresión de bloques puede provocar un aumento en el rendimiento. Este rendimiento viene en perjuicio de la calidad de imagen (debido a la interpolación de colores); sin embargo, la calidad inferior a menudo no es perceptible.

En la siguiente sección se muestra cómo Direct3D permite el uso de la compresión de bloques en una aplicación.

## <a name="span-idusingblockcompressionspanspan-idusingblockcompressionspanspan-idusingblockcompressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>Uso de la compresión de bloques


Crea una textura comprimida por bloques igual que una textura sin comprimir, salvo que especificarás un formato comprimido por bloques.

Luego crea una vista para enlazar la textura a la canalización. Dado que una textura comprimida por bloques puede usarse solo como entrada para una fase de sombreador, querrás crear una vista de recursos del sombreador.

Usa una textura comprimida por bloques de la misma manera que usarías una textura sin comprimir. Si la aplicación obtendrá un puntero de memoria para los datos comprimidos por bloques, debes tener en cuenta el relleno de memoria en un mapa MIP que hace que el tamaño declarado sea diferente del tamaño real.

-   [Tamaño virtual frente a tamaño físico](#virtual-size-versus-physical-size)

### <a name="span-idvirtualsizespanspan-idvirtualsizespanspan-idvirtualsizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>Tamaño virtual frente a tamaño físico

Si tienes un código de aplicación que usa un puntero de memoria para recorrer la memoria de una textura comprimida por bloques, hay una consideración importante que probablemente requiera una modificación en el código de la aplicación. Una textura comprimida por bloques debe ser un múltiplo de 4 en todas las dimensiones, porque los algoritmos de compresión por bloques operan en bloques de elementos de textura de 4×4. Esto será un problema para un mapa MIP cuyas dimensiones iniciales sean divisibles por 4, pero no sus niveles subdivididos. En el siguiente diagrama se muestra la diferencia en el área entre el tamaño virtual (declarado) y el tamaño físico (real) de cada nivel del mapa MIP.

![niveles del mapa MIP sin comprimir y comprimidos](images/d3d10-block-compress-pad.png)

En el lado izquierdo del diagrama se muestran los tamaños de los niveles del mapa MIP que se generan para una textura de 60×40 sin comprimir. El tamaño de nivel superior se toma de la llamada API que genera la textura; cada nivel subsiguiente tiene la mitad del tamaño del nivel anterior. Para una textura sin comprimir, no existe ninguna diferencia entre el tamaño virtual (declarado) y el tamaño físico (real).

En el lado derecho del diagrama se muestran los tamaños de los niveles del mapa MIP que se generan para la misma textura 60×40 con compresión. Ten en cuenta que tanto el segundo como el tercer nivel tienen rellenos de memoria para que los tamaños sean múltiplos de 4 en cada nivel. Esto es necesario para que los algoritmos puedan operar con bloques de elementos de textura de 4×4. Esto es más evidente si piensas en los niveles del mapa MIP que son menores que 4×4; el tamaño de estos niveles del mapa MIP muy pequeños se redondearán hacia arriba al múltiplo de 4 más cercano cuando se asigne la memoria de textura.

El muestreo de hardware usa el tamaño virtual; cuando se muestrea la textura, se omite el relleno de memoria. Para los niveles del mapa MIP menores que 4×4, solo se usarán los cuatro primeros elementos de textura para un mapa de 2×2, y un bloque de 1×1 solo usará el primer elemento de textura. Sin embargo, no hay ninguna estructura de API que exponga el tamaño físico (incluido el relleno de memoria).

En resumen, ten cuidado de usar bloques de memoria alineados al copiar regiones que contienen datos comprimidos por bloques. Para hacer esto en una aplicación que obtiene un puntero de memoria, asegúrate de que el puntero utilice el pitch de la superficie para tener en cuenta el tamaño de la memoria física.

## <a name="span-idcompressionalgorithmsspanspan-idcompressionalgorithmsspanspan-idcompressionalgorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>Algoritmos de compresión


Las técnicas de compresión de bloques en Direct3D descomponen los datos de texturas sin comprimir en bloques de 4×4, comprimen cada bloque y luego almacenan los datos. Por este motivo, se espera que las texturas que se van a comprimir tengan dimensiones de textura que sean múltiplos de 4.

![compresión de bloques](images/d3d10-compression-1.png)

En el diagrama anterior se muestra una textura dividida en bloques de elementos de textura. El primer bloque muestra el diseño de los 16 elementos de textura etiquetados de "a" a "p", pero cada bloque tiene la misma organización de datos.

Direct3D implementa varios esquemas de compresión; cada uno implementa un equilibrio diferente entre el número de componentes almacenados, el número de bits por componente y la cantidad de memoria consumida. Usa esta tabla como ayuda para elegir el formato que funcione mejor con el tipo de datos y la resolución de datos que mejor se adapte a tu aplicación.

| Datos de origen                     | Resolución de compresión de datos (en bits) | Elige este formato de compresión |
|---------------------------------|---------------------------------------|--------------------------------|
| Color y alfa de tres componentes | Color (5:6:5), alfa (1) o sin alfa  | [BC1](#bc1)                    |
| Color y alfa de tres componentes | Color (5:6:5), alfa (4)              | [BC2](#bc2)                    |
| Color y alfa de tres componentes | Color (5:6:5), alfa (8)              | [BC3](#bc3)                    |
| Color de un componente             | Un componente (8)                     | [BC4](#bc4)                    |
| Color de dos componentes             | Dos componentes (8:8)                  | [BC5](#bc5)                    |

 

-   [BC1](#bc1)
-   [BC2](#bc2)
-   [BC3](#bc3)
-   [BC4](#bc4)
-   [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

Usa el formato de compresión del primer bloque (BC1) (DXGI\_FORMAT\_BC1\_TYPELESS, DXGI\_FORMAT\_BC1\_UNORM o DXGI\_BC1\_UNORM\_SRGB) para almacenar los datos de color de tres componentes usando un color 5:6:5 (rojo de 5bits, verde de 6bits, azul de 5bits). Esto se cumple incluso si los datos también contienen un alfa de 1bit. Si se supone una textura de 4×4 con el formato de datos más amplio posible, el formato de BC1 reduce la memoria necesaria de 48bytes (16 colores × 3 componentes/color × 1 byte/componente) a 8bytes de memoria.

El algoritmo funciona en bloques de elementos de textura de 4×4. En lugar de almacenar 16colores, el algoritmo guarda 2colores de referencia (color\_0 y color\_1) y 16índices de color de 2bits (bloques "a" a "p"), como se muestra en el siguiente diagrama.

![el diseño de compresión bc1](images/d3d10-compression-bc1.png)

Los índices de color ("a" a "p") se usan para buscar los colores originales de una tabla de colores. La tabla de colores contiene 4 colores. Los primeros dos colores (color\_0 y color\_1) son los colores mínimos y máximos. Los otros dos colores (color\_2 y color\_3) son colores intermedios que se calculan con una interpolación lineal.

```
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

A los cuatro colores se les asignan los valores de índice de 2bits que se guardarán en los bloques "a" a "p".

```
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

Por último, cada uno de los colores de los bloques "a" a "p" se comparan con los cuatro colores de la tabla de colores, y el índice para el color más cercano se almacena en bloques de 2bits.

Este algoritmo también se presta para los datos que contienen un alfa de 1bit. La única diferencia es que color\_3 se establece en 0 (que representa un color transparente) y color\_2 es una combinación lineal de color\_0 y color\_1.

```
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

Usa el formato BC2 (DXGI\_FORMAT\_BC2\_TYPELESS, DXGI\_FORMAT\_BC2\_UNORM o DXGI\_BC2\_UNORM\_SRGB) para almacenar los datos que contienen los datos de color y alfa con baja coherencia (usa [BC3](#bc3) para datos de alfa de alta coherencia). El formato BC2 almacena datos RGB como un color 5:6:5 (rojo de 5bits, verde de 6bits, azul de 5bits) y alfa como un valor de 4bits independiente. Si se supone una textura de 4×4 con el formato de datos más amplio posible, esta técnica de compresión reduce la memoria necesaria de 64bytes (16 colores × 4 componentes/color × 1 byte/componente) a 16bytes de memoria.

El formato BC2 almacena colores con el mismo número de bits y diseño de datos que el formato [BC1](#bc1); sin embargo, BC2 requiere 64bits adicionales de memoria para almacenar los datos de alfa, como se muestra en el siguiente diagrama.

![el diseño de compresión bc2](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

Usa el formato BC3 (DXGI\_FORMAT\_BC3\_TYPELESS, DXGI\_FORMAT\_BC3\_UNORM o DXGI\_BC3\_UNORM\_SRGB) para almacenar los datos de color de alta coherencia (usa [BC2](#bc2) con datos de alfa de menos coherencia). El formato BC3 almacena datos de color con un color 5:6:5 (rojo de 5bits, verde de 6bits, azul de 5bits) y datos de alfa de un byte. Si se supone una textura de 4×4 con el formato de datos más amplio posible, esta técnica de compresión reduce la memoria necesaria de 64bytes (16 colores × 4 componentes/color × 1 byte/componente) a 16bytes de memoria.

El formato BC3 almacena colores con el mismo número de bits y diseño de datos que el formato [BC1](#bc1); sin embargo, BC3 requiere 64bits adicionales de memoria para almacenar los datos de alfa. Para administrar el alfa, el formato BC3 almacena dos valores de referencia y los interpola (de forma similar a cómo BC1 almacena el color RGB).

El algoritmo funciona en bloques de elementos de textura de 4×4. En lugar de almacenar 16valores de alfa, el algoritmo almacena 2alfas de referencia (alpha\_0 y alpha\_1) y 16índices de color de 3bits (alfa "a" a "p"), como se muestra en el siguiente diagrama.

![el diseño de compresión bc3](images/d3d10-compression-bc3.png)

El formato BC3 usa los índices de alfa ("a" a "p") para buscar los colores originales de una tabla de búsqueda que contiene 8 valores. Los dos primeros valores (alpha\_0 y alpha\_1) son los valores mínimos y máximos; los otros seis valores intermedios se calculan mediante interpolación lineal.

El algoritmo determina el número de valores de alfa interpolados tras examinar los dos valores de alfa de referencia. Si alpha\_0 es mayor que alpha\_1, BC3 interpola 6 valores de alfa; de lo contrario, interpola 4. Cuando BC3 interpola solo 4 valores de alfa, establece dos valores de alfa adicionales (0 para totalmente transparente y 255 para totalmente opaco). Para comprimir los valores de alfa en el área de elementos de textura de 4×4, BC3 almacena el código de bits correspondiente a los valores de alfa interpolados que más se acerquen al alfa original para un elemento de textura determinado.

```
if( alpha_0 > alpha_1 )
{
  // 6 interpolated alpha values.
  alpha_2 = 6/7*alpha_0 + 1/7*alpha_1; // bit code 010
  alpha_3 = 5/7*alpha_0 + 2/7*alpha_1; // bit code 011
  alpha_4 = 4/7*alpha_0 + 3/7*alpha_1; // bit code 100
  alpha_5 = 3/7*alpha_0 + 4/7*alpha_1; // bit code 101
  alpha_6 = 2/7*alpha_0 + 5/7*alpha_1; // bit code 110
  alpha_7 = 1/7*alpha_0 + 6/7*alpha_1; // bit code 111
}
else
{
  // 4 interpolated alpha values.
  alpha_2 = 4/5*alpha_0 + 1/5*alpha_1; // bit code 010
  alpha_3 = 3/5*alpha_0 + 2/5*alpha_1; // bit code 011
  alpha_4 = 2/5*alpha_0 + 3/5*alpha_1; // bit code 100
  alpha_5 = 1/5*alpha_0 + 4/5*alpha_1; // bit code 101
  alpha_6 = 0;                         // bit code 110
  alpha_7 = 255;                       // bit code 111
}
```

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>BC4

Usa el formato BC4 para almacenar datos de color de un componente mediante 8bits para cada color. Como resultado de la mayor precisión (en comparación con [BC1](#bc1)), BC4 es perfecto para almacenar datos de punto flotante en el intervalo de \[0 a 1\] con el formato DXGI\_FORMAT\_BC4\_UNORM y de \[-1 a + 1\] con el formato DXGI\_FORMAT\_BC4\_SNORM. Si se supone una textura de 4×4 con el formato de datos más amplio posible, esta técnica de compresión reduce la memoria necesaria de 16bytes (16 colores × 1 componentes/color × 1 byte/componente) a 8bytes.

El algoritmo funciona en bloques de elementos de textura de 4×4. En lugar de almacenar 16colores, el algoritmo almacena 2colores de referencia (red\_0 y red\_1) y 16índices de color de 3bits (de rojo "a" a rojo "p"), como se muestra en el siguiente diagrama.

![el diseño de compresión bc4](images/d3d10-compression-bc4.png)

El algoritmo usa los índices de 3bits para buscar los colores en una tabla de colores que contiene 8colores. Los primeros dos colores (red\_0 y red\_1) son los colores mínimos y máximos. El algoritmo calcula los colores restantes mediante interpolación lineal.

El algoritmo determina el número de valores de color interpolados tras examinar los dos valores de referencia. Si red\_0 es mayor que red\_1, BC4 interpola 6 valores de color; de lo contrario, interpola 4. Cuando BC4 interpola solo 4 valores de color, establece dos valores de color adicionales (0.0f para totalmente transparente y 1.0f para totalmente opaco). Para comprimir los valores de alfa en el área de elementos de textura de 4×4, BC4 almacena el código de bits correspondiente a los valores de alfa interpolados que más se acerquen al alfa original para un elemento de textura determinado.

-   [BC4\_UNORM](#bc4-unorm)
-   [BC4\_SNORM](#bc4-snorm)

### <a name="span-idbc4unormspanspan-idbc4unormspanspan-idbc4-unormspanbc4unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>BC4\_UNORM

La interpolación de los datos de componente único se logra como se muestra en el siguiente ejemplo de código.

```
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

A los colores de referencia se les asignan índices de 3bits (000–111, dado que hay 8valores), que se guardarán en los bloques rojo "a" a rojo "p" durante la compresión.

### <a name="span-idbc4snormspanspan-idbc4snormspanspan-idbc4-snormspanbc4snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>BC4\_SNORM

DXGI\_FORMAT\_BC4\_SNORM funciona exactamente igual, salvo que los datos se codifican en el intervalo SNORM y cuando se interpolan 4 valores de color. La interpolación de los datos de componente único se logra como se muestra en el siguiente ejemplo de código.

```
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                     // bit code 110
  red_7 =  1.0f;                     // bit code 111
}
```

A los colores de referencia se les asignan índices de 3bits (000–111, dado que hay 8valores), que se guardarán en los bloques rojo "a" a rojo "p" durante la compresión.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

Usa el formato BC5 para almacenar datos de color de dos componentes mediante 8bits para cada color. Como resultado de la mayor precisión (en comparación con [BC1](#bc1)), BC5 es perfecto para almacenar datos de punto flotante en el intervalo de \[0 a 1\] con el formato DXGI\_FORMAT\_BC5\_UNORM y de \[-1 a + 1\] con el formato DXGI\_FORMAT\_BC5\_SNORM. Si se supone una textura de 4×4 con el formato de datos más amplio posible, esta técnica de compresión reduce la memoria necesaria de 32bytes (16 colores × 2 componentes/color × 1 byte/componente) a 16bytes.

-   [BC5\_UNORM](#bc5-unorm)
-   [BC5\_SNORM](#bc5-snorm)

El algoritmo funciona en bloques de elementos de textura de 4×4. En lugar de almacenar 16colores para ambos componentes, el algoritmo almacena 2 colores de referencia para cada componente (red\_0, red\_1, green\_0 y green\_1) y 16índices de color de 3bits para cada componente (de rojo "a" a rojo "p" y de verde "a" a verde "p"), como se muestra en el siguiente diagrama.

![el diseño de compresión bc5](images/d3d10-compression-bc5.png)

El algoritmo usa los índices de 3bits para buscar los colores en una tabla de colores que contiene 8colores. Los primeros dos colores (red\_0 y red\_1 o green\_0 y green\_1) son los colores mínimos y máximos. El algoritmo calcula los colores restantes mediante interpolación lineal.

El algoritmo determina el número de valores de color interpolados tras examinar los dos valores de referencia. Si red\_0 es mayor que red\_1, BC5 interpola 6 valores de color; de lo contrario, interpola 4. Cuando BC5 interpola solo 4 valores de color, establece los dos valores de color restantes en 0.0f y 1.0f.

### <a name="span-idbc5unormspanspan-idbc5unormspanspan-idbc5-unormspanbc5unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5\_UNORM

La interpolación de los datos de componente único se logra como se muestra en el siguiente ejemplo de código. Los cálculos para los componentes verdes son similares.

```
unsigned word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = 0.0f;                     // bit code 110
  red_7 = 1.0f;                     // bit code 111
}
```

A los colores de referencia se les asignan índices de 3bits (000–111, dado que hay 8valores), que se guardarán en los bloques rojo "a" a rojo "p" durante la compresión.

### <a name="span-idbc5snormspanspan-idbc5snormspanspan-idbc5-snormspanbc5snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5\_SNORM

DXGI\_FORMAT\_BC5\_SNORM funciona exactamente igual, salvo que los datos se codifican en el intervalo SNORM y cuando se interpolan 4 valores de datos, los dos valores adicionales son -1.0f y 1.0f. La interpolación de los datos de componente único se logra como se muestra en el siguiente ejemplo de código. Los cálculos para los componentes verdes son similares.

```
signed word red_0, red_1;

if( red_0 > red_1 )
{
  // 6 interpolated color values
  red_2 = (6*red_0 + 1*red_1)/7.0f; // bit code 010
  red_3 = (5*red_0 + 2*red_1)/7.0f; // bit code 011
  red_4 = (4*red_0 + 3*red_1)/7.0f; // bit code 100
  red_5 = (3*red_0 + 4*red_1)/7.0f; // bit code 101
  red_6 = (2*red_0 + 5*red_1)/7.0f; // bit code 110
  red_7 = (1*red_0 + 6*red_1)/7.0f; // bit code 111
}
else
{
  // 4 interpolated color values
  red_2 = (4*red_0 + 1*red_1)/5.0f; // bit code 010
  red_3 = (3*red_0 + 2*red_1)/5.0f; // bit code 011
  red_4 = (2*red_0 + 3*red_1)/5.0f; // bit code 100
  red_5 = (1*red_0 + 4*red_1)/5.0f; // bit code 101
  red_6 = -1.0f;                    // bit code 110
  red_7 =  1.0f;                    // bit code 111
}
```

A los colores de referencia se les asignan índices de 3bits (000–111, dado que hay 8valores), que se guardarán en los bloques rojo "a" a rojo "p" durante la compresión.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>Conversión de formato


Direct3D permite copias entre escrito texturas con tipos preestructurados y texturas comprimidas por bloques con los mismo anchos de bits.

Puedes copiar recursos entre algunos tipos de formato. Este tipo de operación de copia realiza un tipo de conversión de formato que reinterpreta los datos de recursos como un tipo de formato distinto. Ten en cuenta este ejemplo, que muestra la diferencia entre la reinterpretación de los datos y el comportamiento de un tipo más habitual de conversión:

```
FLOAT32 f = 1.0f;
UINT32 u;
```

Para reinterpretar 'f' como el tipo de 'u', usa [memcpy](http://msdn.microsoft.com/library/dswaw1wk.aspx):

```
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

En la reinterpretación anterior, no cambia el valor subyacente de los datos; [memcpy](http://msdn.microsoft.com/library/dswaw1wk.aspx) reinterpreta el float como un entero sin signo.

Para realizar el tipo de conversión más habitual, usa la asignación:

```
u = f; // 'u' becomes 1.
```

En la conversión anterior, cambia el valor subyacente de los datos.

En la siguiente tabla se enumeran los formatos permitidos de origen y destino que puedes usar en la reinterpretación de este tipo de conversión de formato. Debes codificar los valores correctamente para que la reinterpretación funcione según lo esperado.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ancho de bits</th>
<th align="left">Recursos sin comprimir</th>
<th align="left">Recurso comprimido por bloques</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">32</td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p>
<p>DXGI_FORMAT_R32_SINT</p></td>
<td align="left">DXGI_FORMAT_R9G9B9E5_SHAREDEXP</td>
</tr>
<tr class="even">
<td align="left">64</td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UINT</p>
<p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<p>DXGI_FORMAT_R32G32_UINT</p>
<p>DXGI_FORMAT_R32G32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM[_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de texturas comprimidas](compressed-texture-resources.md)
