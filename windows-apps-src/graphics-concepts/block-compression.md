---
title: Compresión de bloques
description: La compresión de bloque es una técnica de compresión de textura con pérdida para reducir el tamaño de la textura y la superficie de memoria, lo que permite aumentar el rendimiento. Una textura comprimida en bloque puede ser menor que una textura con 32 bits por color.
ms.assetid: 2FAD6BE8-C6E4-4112-AF97-419CD27F7C73
keywords:
- Compresión de bloques
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 02b65357b52740e9b56e0e2dc11ff22723825f20
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156459"
---
# <a name="block-compression"></a>Compresión de bloques

La compresión de bloque es una técnica de compresión de textura con pérdida para reducir el tamaño de la textura y la superficie de memoria, lo que permite aumentar el rendimiento. Una textura comprimida en bloque puede ser menor que una textura con 32 bits por color.

La compresión de bloque es una técnica de compresión de textura para reducir el tamaño de la textura. Cuando se comparan con una textura con 32 bits por color, una textura comprimida en bloque puede tener un tamaño de hasta un 75% inferior. Las aplicaciones normalmente ven un aumento del rendimiento cuando se usa la compresión de bloques debido a la menor superficie de memoria.

Aunque la pérdida, la compresión de bloque funciona bien y se recomienda para todas las texturas que se transforman y filtran por la canalización. Las texturas que se asignan directamente a la pantalla (elementos de la interfaz de usuario como iconos y texto) no son buenas opciones de compresión porque los artefactos son más evidentes.

Una textura comprimida en bloques debe crearse como un múltiplo del tamaño 4 en todas las dimensiones y no se puede usar como salida de la canalización.

## <a name="span-idbasicsspanspan-idbasicsspanspan-idbasicsspanhow-block-compression-works"></a><span id="Basics"></span><span id="basics"></span><span id="BASICS"></span>Cómo funciona la compresión por bloques

La compresión de bloque es una técnica para reducir la cantidad de memoria necesaria para almacenar los datos de color. Al almacenar algunos colores en su tamaño original y otros colores con un esquema de codificación, puede reducir drásticamente la cantidad de memoria necesaria para almacenar la imagen. Dado que el hardware descodifica automáticamente los datos comprimidos, no hay ninguna penalización de rendimiento para el uso de texturas comprimidas.

Para ver cómo funciona la compresión, consulte los dos ejemplos siguientes. En el primer ejemplo se describe la cantidad de memoria que se utiliza al almacenar datos sin comprimir. en el segundo ejemplo se describe la cantidad de memoria que se utiliza al almacenar datos comprimidos.

- [Almacenar datos sin comprimir](#storing-uncompressed-data)
- [Almacenar datos comprimidos](#storing-compressed-data)

### <a name="span-idstoring_uncompressed_dataspanspan-idstoring_uncompressed_dataspanspan-idstoring_uncompressed_dataspanspan-idstoring-uncompressed-dataspanstoring-uncompressed-data"></a><span id="Storing_Uncompressed_Data"></span><span id="storing_uncompressed_data"></span><span id="STORING_UNCOMPRESSED_DATA"></span><span id="storing-uncompressed-data"></span>Almacenar datos sin comprimir

En la ilustración siguiente se representa una textura de 4 × 4 sin comprimir. Supongamos que cada color contiene un componente de color único (rojo por ejemplo) y se almacena en un byte de memoria.

![textura 4x4 sin comprimir](images/d3d10-block-compress-1.png)

Los datos sin comprimir se colocan en la memoria secuencialmente y requieren 16 bytes, tal como se muestra en la siguiente ilustración.

![datos sin comprimir en memoria secuencial](images/d3d10-block-compress-2.png)

### <a name="span-idstoring_compressed_dataspanspan-idstoring_compressed_dataspanspan-idstoring_compressed_dataspanspan-idstoring-compressed-dataspanstoring-compressed-data"></a><span id="Storing_Compressed_Data"></span><span id="storing_compressed_data"></span><span id="STORING_COMPRESSED_DATA"></span><span id="storing-compressed-data"></span>Almacenar datos comprimidos

Ahora que ha visto cuánta memoria utiliza una imagen sin comprimir, eche un vistazo a cuánta memoria guarda una imagen comprimida. El formato de compresión [BC1](#bc1) almacena 2 colores (1 byte cada uno) y índices de 16 3 bits (48 bits o 6 bytes) que se usan para interpolar los colores originales en la textura, como se muestra en la siguiente ilustración.

![el formato de compresión BC1](images/d3d10-block-compress-3.png)

El espacio total necesario para almacenar los datos comprimidos es de 8 bytes, lo que supone un ahorro de memoria del 50% sobre el ejemplo sin comprimir. El ahorro es incluso mayor cuando se usa más de un componente de color.

El importante ahorro de memoria que proporciona la compresión por bloques puede dar lugar a un aumento del rendimiento. Este rendimiento se produce a costa de la calidad de la imagen (debido a la interpolación de color). sin embargo, la calidad inferior no suele ser apreciable.

En la sección siguiente se muestra cómo Direct3D habilita el uso de la compresión de bloque en una aplicación.

## <a name="span-idusing_block_compressionspanspan-idusing_block_compressionspanspan-idusing_block_compressionspanusing-block-compression"></a><span id="Using_Block_Compression"></span><span id="using_block_compression"></span><span id="USING_BLOCK_COMPRESSION"></span>Usar la compresión de bloque

Cree una textura comprimida en bloque como una textura sin comprimir, salvo que especifique un formato comprimido en bloques.

A continuación, cree una vista para enlazar la textura a la canalización, ya que una textura con compresión de bloque solo se puede usar como entrada para una fase de sombreador y desea crear una vista de recursos de sombreador.

Use una textura comprimida de bloque del mismo modo que usaría una textura sin comprimir. Si la aplicación va a obtener un puntero de memoria para bloquear datos comprimidos, debe tener en cuenta el relleno de memoria en un mipmap que hace que el tamaño declarado difiera del tamaño real.

- [Tamaño virtual frente al tamaño físico](#virtual-size-versus-physical-size)

### <a name="span-idvirtual_sizespanspan-idvirtual_sizespanspan-idvirtual_sizespanspan-idvirtual-size-versus-physical-sizespanvirtual-size-versus-physical-size"></a><span id="Virtual_Size"></span><span id="virtual_size"></span><span id="VIRTUAL_SIZE"></span><span id="virtual-size-versus-physical-size"></span>Tamaño virtual frente al tamaño físico

Si tiene código de aplicación que usa un puntero de memoria para recorrer la memoria de una textura comprimida de bloque, hay una consideración importante que puede requerir una modificación en el código de la aplicación. Una textura comprimida por bloques debe ser un múltiplo de 4 en todas las dimensiones, ya que los algoritmos de compresión de bloque operan en bloques textura de 4x4. Esto supondrá un problema para un mipmap cuyas dimensiones iniciales son divisibles en 4, pero los niveles subdivididos no lo son. En el diagrama siguiente se muestra la diferencia en el área entre el tamaño virtual (declarado) y el tamaño físico (real) de cada nivel de mipmap.

![niveles de mipmap sin comprimir y comprimidos](images/d3d10-block-compress-pad.png)

En el lado izquierdo del diagrama se muestran los tamaños de nivel de mipmap que se generan para una textura de 60 × 40 sin comprimir. El tamaño de nivel superior se toma de la llamada API que genera la textura; cada nivel subsiguiente es la mitad del tamaño del nivel anterior. En el caso de una textura sin comprimir, no hay ninguna diferencia entre el tamaño virtual (declarado) y el tamaño físico (real).

En el lado derecho del diagrama se muestran los tamaños de nivel de mipmap que se generan para la misma textura de 60 × 40 con compresión. Tenga en cuenta que tanto el segundo como el tercer nivel tienen un relleno de memoria para que los factores de tamaño de 4 se conviertan en todos los niveles. Esto es necesario para que los algoritmos puedan funcionar en 4 × 4 bloques textura. Esto es especialmente evidente si se tienen en cuenta los niveles de mipmap inferiores a 4 × 4; el tamaño de estos niveles de mipmap pequeños se redondeará al factor más cercano de 4 cuando se asigne memoria de textura.

El hardware de muestreo utiliza el tamaño virtual; Cuando se muestrea la textura, se omite el relleno de memoria. En el caso de los niveles de mipmap inferiores a 4 × 4, solo se usarán los cuatro primeros textura para un mapa 2 × 2 y solo el primer textura se usará en un bloque 1 × 1. Sin embargo, no hay ninguna estructura de API que exponga el tamaño físico (incluido el relleno de memoria).

En Resumen, tenga cuidado de utilizar bloques de memoria alineados al copiar regiones que contienen datos comprimidos en bloques. Para hacer esto en una aplicación que obtiene un puntero de memoria, asegúrese de que el puntero usa el paso de la superficie para tener en cuenta el tamaño de la memoria física.

## <a name="span-idcompression_algorithmsspanspan-idcompression_algorithmsspanspan-idcompression_algorithmsspancompression-algorithms"></a><span id="Compression_Algorithms"></span><span id="compression_algorithms"></span><span id="COMPRESSION_ALGORITHMS"></span>Algoritmos de compresión

Las técnicas de compresión en bloque de Direct3D dividen los datos de textura sin comprimir en 4 × 4 bloques, comprimen cada bloque y, a continuación, almacenan los datos. Por esta razón, se espera que las texturas se compriman deben tener dimensiones de textura que sean múltiplos de 4.

![compresión de bloque](images/d3d10-compression-1.png)

En el diagrama anterior se muestra una textura particionada en bloques textura. El primer bloque muestra el diseño de los 16 textura etiquetados como-p, pero cada bloque tiene la misma organización de datos.

Direct3D implementa varios esquemas de compresión, cada uno de ellos implementa un equilibrio diferente entre el número de componentes almacenados, el número de bits por componente y la cantidad de memoria consumida. Use esta tabla para ayudar a elegir el formato que mejor se adapte al tipo de datos y la resolución de los datos que mejor se adapta a su aplicación.

| Datos de origen                     | Resolución de compresión de datos (en bits) | Elegir este formato de compresión |
|---------------------------------|---------------------------------------|--------------------------------|
| Color de tres componentes y alfa | Color (5:6:5), alfa (1) o no alfa  | [BC1](#bc1)                    |
| Color de tres componentes y alfa | Color (5:6:5), alfa (4)              | [BC2](#bc2)                    |
| Color de tres componentes y alfa | Color (5:6:5), alfa (8)              | [BC3](#bc3)                    |
| Color de un componente             | Un componente (8)                     | [LAS texturas BC4](#bc4)                    |
| Color de dos componentes             | Dos componentes (8:8)                  | [BC5](#bc5)                    |

- [BC1](#bc1)
- [BC2](#bc2)
- [BC3](#bc3)
- [LAS texturas BC4](#bc4)
- [BC5](#bc5)

### <a name="span-idbc1spanspan-idbc1spanbc1"></a><span id="BC1"></span><span id="bc1"></span>BC1

Use el primer formato de compresión de bloque (BC1) (el formato de DXGI \_ \_ BC1 \_ sin tipo, \_ el formato de DXGI \_ BC1 \_ UNORM o dxgi \_ BC1 \_ UNORM \_ sRGB) para almacenar datos de color de tres componentes mediante un color de 5:6:5 (5 bits rojo, 6 bits verde, 5 bits azul). Esto es así incluso si los datos también contienen alfa de 1 bit. Suponiendo una textura de 4 × 4 con el formato de datos más grande posible, el formato BC1 reduce la cantidad de memoria necesaria de 48 bytes (16 colores × 3 componentes/color × 1 byte/componente) a 8 bytes de memoria.

El algoritmo funciona en 4 × 4 bloques de textura. En lugar de almacenar 16 colores, el algoritmo guarda 2 colores de referencia (color \_ 0 y color \_ 1) y índices de color de 16 2 bits (bloques a – p), tal como se muestra en el diagrama siguiente.

![el diseño de la compresión BC1](images/d3d10-compression-bc1.png)

Los índices de color (a – p) se usan para buscar los colores originales de una tabla de colores. La tabla de colores contiene 4 colores. Los dos primeros colores (color \_ 0 y color \_ 1) son los colores mínimo y máximo. Los otros dos colores, color \_ 2 y color \_ 3, son colores intermedios calculados con interpolación lineal.

```cpp
color_2 = 2/3*color_0 + 1/3*color_1
color_3 = 1/3*color_0 + 2/3*color_1
```

A los cuatro colores se les asignan valores de índice de 2 bits que se guardarán en bloques a – p.

```cpp
color_0 = 00
color_1 = 01
color_2 = 10
color_3 = 11
```

Por último, cada uno de los colores de los bloques a – p se comparan con los cuatro colores de la tabla de colores y el índice del color más cercano se almacena en los bloques de 2 bits.

Este algoritmo se presta a los datos que contienen también Alpha de 1 bit. La única diferencia es que el color \_ 3 se establece en 0 (que representa un color transparente) y \_ el color 2 es una mezcla lineal del color \_ 0 y el color \_ 1.

```cpp
color_2 = 1/2*color_0 + 1/2*color_1;
color_3 = 0;
```

### <a name="span-idbc2spanspan-idbc2spanbc2"></a><span id="BC2"></span><span id="bc2"></span>BC2

Use el formato BC2 (ya sea el formato de DXGI \_ \_ BC2 \_ sin tipo, el formato de dxgi \_ \_ BC2 \_ UNORM o dxgi \_ BC2 \_ UNORM \_ sRGB) para almacenar datos que contengan datos de color y alfa con baja coherencia (use [BC3](#bc3) para datos alfa altamente coherentes). El formato BC2 almacena los datos RGB como un color 5:6:5 (5 bits rojo, 6 bits verde, 5 bits azul) y alfa como un valor de 4 bits independiente. Suponiendo una textura de 4 × 4 con el formato de datos más grande posible, esta técnica de compresión reduce la cantidad de memoria necesaria de 64 bytes (16 colores × 4 componentes/color × 1 byte/componente) a 16 bytes de memoria.

El formato BC2 almacena los colores con el mismo número de bits y diseño de datos que el formato [BC1](#bc1) . sin embargo, BC2 requiere una memoria adicional de 64 bits para almacenar los datos alfa, tal como se muestra en el diagrama siguiente.

![el diseño de la compresión BC2](images/d3d10-compression-bc2.png)

### <a name="span-idbc3spanspan-idbc3spanbc3"></a><span id="BC3"></span><span id="bc3"></span>BC3

Use el formato BC3 (ya sea el formato de DXGI \_ \_ BC3 \_ sin tipo, el formato de dxgi \_ \_ BC3 \_ UNORM o dxgi \_ BC3 \_ UNORM \_ sRGB) para almacenar datos de color muy coherentes (use [BC2](#bc2) con datos alfa menos coherentes). El formato BC3 almacena los datos de color con color 5:6:5 (5 bits rojo, 6 bits verde, 5 bits azul) y datos alfa usando un byte. Suponiendo una textura de 4 × 4 con el formato de datos más grande posible, esta técnica de compresión reduce la cantidad de memoria necesaria de 64 bytes (16 colores × 4 componentes/color × 1 byte/componente) a 16 bytes de memoria.

El formato BC3 almacena los colores con el mismo número de bits y diseño de datos que el formato [BC1](#bc1) . sin embargo, BC3 requiere una memoria adicional de 64 bits para almacenar los datos alfa. El formato BC3 controla el valor alfa al almacenar dos valores de referencia y interpolar entre ellos (de forma similar al modo en que BC1 almacena el color RGB).

El algoritmo funciona en 4 × 4 bloques de textura. En lugar de almacenar 16 valores alfa, el algoritmo almacena 2 alfas de referencia (alfa \_ 0 y alfa \_ 1) y los índices de color de 16 3 bits (alfa a a p), tal como se muestra en el diagrama siguiente.

![el diseño de la compresión BC3](images/d3d10-compression-bc3.png)

El formato BC3 usa los índices alfa (a – p) para buscar los colores originales de una tabla de búsqueda que contiene 8 valores. Los dos primeros valores (alfa \_ 0 y alfa \_ 1) son los valores mínimo y máximo; los otros seis valores intermedios se calculan mediante la interpolación lineal.

El algoritmo determina el número de valores alfa interpolados examinando los dos valores alfa de referencia. Si el \_ valor alfa 0 es mayor que el \_ valor alfa 1, BC3 interpola 6 valores alfa; de lo contrario, interpola 4. Cuando BC3 interpola solo 4 valores alfa, establece dos valores alfa adicionales (0 para totalmente transparente y 255 para totalmente opaco). BC3 comprime los valores alfa en el área textura de 4 × 4 almacenando el código de bit correspondiente a los valores alfa interpolados que más se aproxime al alfa original de un textura determinado.

```cpp
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

### <a name="span-idbc4spanspan-idbc4spanbc4"></a><span id="BC4"></span><span id="bc4"></span>LAS texturas BC4

Use el formato las texturas BC4 para almacenar datos de color de un componente usando 8 bits para cada color. Como resultado de la mayor precisión (en comparación con [BC1](#bc1)), las texturas BC4 es ideal para almacenar los datos de punto flotante en el intervalo de \[ 0 a 1 \] mediante el formato de dxgi \_ \_ las texturas BC4 \_ UNORM y \[ -1 a + 1 \] con el formato dxgi \_ \_ las texturas BC4 \_ SNORM. Suponiendo una textura de 4 × 4 con el formato de datos más grande posible, esta técnica de compresión reduce la cantidad de memoria necesaria de 16 bytes (16 colores × 1 componentes/color × 1 byte/componente) a 8 bytes.

El algoritmo funciona en 4 × 4 bloques de textura. En lugar de almacenar 16 colores, el algoritmo almacena 2 colores de referencia (rojo \_ 0 y rojo \_ 1) y índices de color de 16 3 bits (rojo a a rojo p), tal como se muestra en el diagrama siguiente.

![el diseño de la compresión las texturas BC4](images/d3d10-compression-bc4.png)

El algoritmo utiliza los índices de 3 bits para buscar colores de una tabla de colores que contiene 8 colores. Los dos primeros colores, rojo \_ 0 y rojo \_ 1, son los colores mínimo y máximo. El algoritmo calcula el resto de los colores mediante la interpolación lineal.

El algoritmo determina el número de valores de color interpolados examinando los dos valores de referencia. Si el rojo \_ 0 es mayor que rojo \_ 1, las texturas BC4 interpola los valores de 6 colores; de lo contrario, interpola 4. Cuando las texturas BC4 interpola solo 4 valores de color, establece dos valores de color adicionales (0.0 f para totalmente transparente y 1,0 f para totalmente opaco). LAS texturas BC4 comprime los valores alfa en el área textura de 4 × 4 almacenando el código de bit correspondiente a los valores alfa interpolados que más se aproxime al alfa original de un textura determinado.

- [LAS texturas BC4 \_ UNORM](#bc4-unorm)
- [LAS texturas BC4 \_ SNORM](#bc4-snorm)

### <a name="span-idbc4_unormspanspan-idbc4_unormspanspan-idbc4-unormspanbc4_unorm"></a><span id="BC4_UNORM"></span><span id="bc4_unorm"></span><span id="bc4-unorm"></span>LAS texturas BC4 \_ UNORM

La interpolación de los datos de un solo componente se realiza como en el ejemplo de código siguiente.

```cpp
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

A los colores de referencia se les asignan índices de 3 bits (000 – 111, ya que hay 8 valores), que se guardarán en bloques rojo a a rojo p durante la compresión.

### <a name="span-idbc4_snormspanspan-idbc4_snormspanspan-idbc4-snormspanbc4_snorm"></a><span id="BC4_SNORM"></span><span id="bc4_snorm"></span><span id="bc4-snorm"></span>LAS texturas BC4 \_ SNORM

El formato de DXGI \_ \_ las texturas BC4 \_ SNORM es exactamente el mismo, salvo que los datos se codifican en el intervalo SNORM y cuando se interpolan 4 valores de color. La interpolación de los datos de un solo componente se realiza como en el ejemplo de código siguiente.

```cpp
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

A los colores de referencia se les asignan índices de 3 bits (000 – 111, ya que hay 8 valores), que se guardarán en bloques rojo a a rojo p durante la compresión.

### <a name="span-idbc5spanspan-idbc5spanbc5"></a><span id="BC5"></span><span id="bc5"></span>BC5

Use el formato BC5 para almacenar datos de color de dos componentes mediante 8 bits para cada color. Como resultado de la mayor precisión (en comparación con [BC1](#bc1)), BC5 es ideal para almacenar los datos de punto flotante en el intervalo de \[ 0 a 1 \] mediante el formato de dxgi \_ \_ BC5 \_ UNORM y \[ -1 a + 1 \] con el formato dxgi \_ \_ BC5 \_ SNORM. Suponiendo una textura de 4 × 4 con el formato de datos más grande posible, esta técnica de compresión reduce la cantidad de memoria necesaria de 32 bytes (16 colores × 2 componentes/color × 1 byte/componente) a 16 bytes.

- [BC5 \_ UNORM](#bc5-unorm)
- [BC5 \_ SNORM](#bc5-snorm)

El algoritmo funciona en 4 × 4 bloques de textura. En lugar de almacenar 16 colores para ambos componentes, el algoritmo almacena 2 colores de referencia para cada componente (rojo \_ 0, rojo \_ 1, verde \_ 0 y verde \_ 1) y los índices de color de 16 3 bits para cada componente (de rojo a a rojo p y de verde a a verde p), tal y como se muestra en el diagrama siguiente.

![el diseño de la compresión BC5](images/d3d10-compression-bc5.png)

El algoritmo utiliza los índices de 3 bits para buscar colores de una tabla de colores que contiene 8 colores. Los dos primeros colores, rojo \_ 0 y rojo \_ 1 (o verde \_ 0 y verde \_ 1), son los colores mínimo y máximo. El algoritmo calcula el resto de los colores mediante la interpolación lineal.

El algoritmo determina el número de valores de color interpolados examinando los dos valores de referencia. Si el rojo \_ 0 es mayor que rojo \_ 1, BC5 interpola los valores de 6 colores; de lo contrario, interpola 4. Cuando BC5 interpola solo 4 valores de color, establece los dos valores de color restantes en 0,0 f y 1,0 f.

### <a name="span-idbc5_unormspanspan-idbc5_unormspanspan-idbc5-unormspanbc5_unorm"></a><span id="BC5_UNORM"></span><span id="bc5_unorm"></span><span id="bc5-unorm"></span>BC5 \_ UNORM

La interpolación de los datos de un solo componente se realiza como en el ejemplo de código siguiente. Los cálculos de los componentes verdes son similares.

```cpp
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

A los colores de referencia se les asignan índices de 3 bits (000 – 111, ya que hay 8 valores), que se guardarán en bloques rojo a a rojo p durante la compresión.

### <a name="span-idbc5_snormspanspan-idbc5_snormspanspan-idbc5-snormspanbc5_snorm"></a><span id="BC5_SNORM"></span><span id="bc5_snorm"></span><span id="bc5-snorm"></span>BC5 \_ SNORM

El formato de DXGI \_ \_ BC5 \_ SNORM es exactamente el mismo, salvo que los datos se codifican en el intervalo SNORM y cuando se interpolan 4 valores de datos, los dos valores adicionales son: 1,0 f y 1,0 f. La interpolación de los datos de un solo componente se realiza como en el ejemplo de código siguiente. Los cálculos de los componentes verdes son similares.

```cpp
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

A los colores de referencia se les asignan índices de 3 bits (000 – 111, ya que hay 8 valores), que se guardarán en bloques rojo a a rojo p durante la compresión.

## <a name="span-iddifferencesspanspan-iddifferencesspanspan-iddifferencesspanformat-conversion"></a><span id="Differences"></span><span id="differences"></span><span id="DIFFERENCES"></span>Conversión de formato

Direct3D permite copias entre texturas con tipo preestructurado y texturas con compresión de bloque de los mismos anchos de bits.

Puede copiar recursos entre algunos tipos de formato. Este tipo de operación de copia realiza un tipo de conversión de formato que reinterpreta los datos de recursos como un tipo de formato diferente. Considere este ejemplo que muestra la diferencia entre reinterpretar los datos con la manera en que se comporta un tipo más típico de conversión:

```cpp
FLOAT32 f = 1.0f;
UINT32 u;
```

Para reinterpretar ' f ' como el tipo de ' u ', use [memcpy](/cpp/c-runtime-library/reference/memcpy-wmemcpy):

```cpp
memcpy( &u, &f, sizeof( f ) ); // 'u' becomes equal to 0x3F800000.
```

En la reinterpretación anterior, el valor subyacente de los datos no cambia; [memcpy](/cpp/c-runtime-library/reference/memcpy-wmemcpy) reinterpreta el Float como un entero sin signo.

Para realizar el tipo más típico de conversión, use la asignación:

```cpp
u = f; // 'u' becomes 1.
```

En la conversión anterior, cambia el valor subyacente de los datos.

En la tabla siguiente se enumeran los formatos de origen y destino permitidos que se pueden usar en este tipo de reinterpretación de conversión de formato. Debe codificar los valores correctamente para que la reinterpretación funcione según lo previsto.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Ancho de bits</th>
<th align="left">Recurso sin comprimir</th>
<th align="left">Recurso con compresión de bloque</th>
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
<td align="left"><p>DXGI_FORMAT_BC1_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC4_UNORM</p>
<p>DXGI_FORMAT_BC4_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left">128</td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_UINT</p>
<p>DXGI_FORMAT_R32G32B32A32_SINT</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC3_UNORM [_SRGB]</p>
<p>DXGI_FORMAT_BC5_UNORM</p>
<p>DXGI_FORMAT_BC5_SNORM</p></td>
</tr>
</tbody>
</table>

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados

[Recursos de texturas comprimidas](compressed-texture-resources.md)