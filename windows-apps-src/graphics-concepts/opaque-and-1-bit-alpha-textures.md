---
title: Texturas alfa de 1 bit y opacas
description: "El formato de textura BC1 es para texturas opacas o que tienen un único color transparente."
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords: Texturas alfa de 1 bit y opacas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 56a63e5536523eaf290465bba73a436008bee2f7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptsopaqueand1-bitalphatexturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>Texturas alfa de 1 bit y opacas


El formato de textura BC1 es para texturas opacas o que tienen un único color transparente.

Por cada bloque opaco o de alfa de 1 bit, se almacenan dos valores de 16 bits (en formato RGB 5:6:5) y un mapa de bits de 4x4 con 2 bits por píxel. Esto hace un total de 64 bits de 16 elementos de textura o cuatro bits por elemento de textura. En el mapa de bits de bloque, hay 2 bits por elemento de textura para seleccionar entre los cuatro colores, dos de los cuales se almacenan en los datos codificados. Los otros dos colores se derivan de estos colores almacenados por interpolación lineal. El diseño se muestra en el siguiente diagrama.

![diagrama del diseño del mapa de bits](images/colors1.png)

El formato alfa de 1 bit se distingue del formato opaco comparando los dos valores de color de 16 bits almacenados en el bloque. Se tratan como enteros sin signo. Si el primer color es mayor que el segundo, implica que solo se definen los elementos de textura opacos. Esto significa que se usan cuatro colores para representar los elementos de textura. En la codificación de cuatro colores, hay dos colores derivados y los cuatro colores se distribuyen por igual en el espacio de colores RGB. Este formato es similar al formato RGB 5:6:5. De lo contrario, para la transparencia de alfa de 1 bit, se usan tres colores y el cuarto está reservado para representar los elementos de textura transparentes.

En la codificación de tres colores, hay un color derivado y el cuarto código de 2 bits está reservado para indicar un elemento de textura transparente (información alfa). Este formato es similar al formato RGBA 5:5:5:1, en el que el bit final se usa para la codificación de la máscara alfa.

En el siguiente ejemplo de código se muestra el algoritmo para decidir si se seleccionó la codificación de tres o cuatro colores:

```
if (color_0 > color_1) 
{
    // Four-color block: derive the other two colors. 
    
    // 00 = color_0, 01 = color_1, 10 = color_2, 11 = color_3
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block.
    color_2 = (2 * color_0 + color_1 + 1) / 3;
    color_3 = (color_0 + 2 * color_1 + 1) / 3;
}    
else
{ 
    // Three-color block: derive the other color.
    // 00 = color_0,  01 = color_1,  10 = color_2,  
    // 11 = transparent.
    // These 2-bit codes correspond to the 2-bit fields 
    // stored in the 64-bit block. 
    color_2 = (color_0 + color_1) / 2;    
    color_3 = transparent;    

}
```

Se recomienda establecer los componentes RGBA del píxel de transparencia a cero antes de combinar.

En las siguientes tablas se muestra el diseño de la memoria para el bloque de 8 bytes. Se supone que el primer índice corresponde a la coordenada Y y el segundo, a la coordenada X. Por ejemplo, Texel\[1\]\[2\] hace referencia al píxel del mapa de textura de la posición (x,y) = (2,1).

El siguiente es el diseño de la memoria para el bloque de 8 bytes (64 bits):

| Dirección de Word | Palabra de 16 bits    |
|--------------|----------------|
| 0            | Color\_0       |
| 1            | Color\_1       |
| 2            | Bitmap Word\_0 |
| 3            | Bitmap Word\_1 |

 

Color\_0 y Color\_1, los colores de los dos extremos, tienen el siguiente diseño:

| Bits        | Color                 |
|-------------|-----------------------|
| 4:0 (LSB\*) | Componente de color azul  |
| 10:5        | Componente de color verde |
| 15:11       | Componente de color rojo   |

 

\*bit menos importante

Bitmap Word\_0 tiene el siguiente diseño:

| Bits          | Elemento de textura           |
|---------------|-----------------|
| 1:0 (LSB)     | Texel\[0\]\[0\] |
| 3:2           | Texel\[0\]\[1\] |
| 5:4           | Texel\[0\]\[2\] |
| 7:6           | Texel\[0\]\[3\] |
| 9:8           | Texel\[1\]\[0\] |
| 11:10         | Texel\[1\]\[1\] |
| 13:12         | Texel\[1\]\[2\] |
| 15:14 (MSB\*) | Texel\[1\]\[3\] |

 

\*bit más importante (MSB)

Bitmap Word\_1 tiene el siguiente diseño:

| Bits        | Elemento de textura           |
|-------------|-----------------|
| 1:0 (LSB)   | Texel\[2\]\[0\] |
| 3:2         | Texel\[2\]\[1\] |
| 5:4         | Texel\[2\]\[2\] |
| 7:6         | Texel\[2\]\[3\] |
| 9:8         | Texel\[3\]\[0\] |
| 11:10       | Texel\[3\]\[1\] |
| 13:12       | Texel\[3\]\[2\] |
| 15:14 (MSB) | Texel\[3\]\[3\] |

 

## <a name="span-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanspan-idexampleofopaquecolorencodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>Ejemplo de codificación de color opaco


Como ejemplo de codificación opaca, supongamos que los colores rojo y negro se encuentran en los extremos. Rojo es color\_0 y negro es color\_1. Hay cuatro colores interpolados que forman el degradado uniformemente distribuido entre ellos. Para determinar los valores del mapa de bits de 4x4, se usan los siguientes cálculos:

```
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

El mapa de bits tiene el aspecto del siguiente diagrama.

![diagrama del diseño del mapa de bits expandido](images/colors2.png)

Tiene el aspecto de la siguiente serie ilustrada de colores.

**Nota** en una imagen, los píxeles (0,0) aparecen en la esquina superior izquierda.

 

![ilustración de un degradado codificado opaco](images/redsquares.png)

## <a name="span-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanspan-idexampleof1bitalphaencodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>Ejemplo de codificación alfa de 1 bit


Este formato se selecciona cuando el entero de 16 bits sin signo, color\_0, es menor que el entero de 16 bits sin signo, color\_1. Un ejemplo de dónde se puede usar este formato es en hojas de un árbol, que se muestran contra un cielo azul. Algunos elementos de textura se pueden marcar como transparentes, a la vez que hay disponibles tres tonos de verde disponibles para las hojas. Dos colores fijan los extremos y el tercero es un color interpolado.

La siguiente ilustración es un ejemplo de esta imagen.

![ilustración de codificación alfa de 1 bit](images/greenthing.png)

Donde la imagen se muestra en blanco, el elemento de textura está codificado como transparente. Los componentes RGBA de los elementos de textura transparentes deben establecerse en cero antes de combinar.

La codificación del mapa de bits para los colores y la transparencia se determina con los siguientes cálculos.

```
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

El mapa de bits tiene el aspecto del siguiente diagrama.

![diagrama del diseño del mapa de bits expandido](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de texturas comprimidas](compressed-texture-resources.md)

 

 




