---
title: Texturas alfa de 1 bit y opacas
description: El formato de textura BC1 es para las texturas que son opacas o tienen un único color transparente.
ms.assetid: 8C53ACDD-72ED-4307-B4F3-2FCF9A9F53EC
keywords:
- Texturas alfa de 1 bit y opacas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c08489055bfd4e867310c5bdec6fea655ddc6d41
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933016"
---
# <a name="span-iddirect3dconceptsopaque_and_1-bit_alpha_texturesspanopaque-and-1-bit-alpha-textures"></a><span id="direct3dconcepts.opaque_and_1-bit_alpha_textures"></span>Texturas alfa de 1 bit y opacas

El formato de textura BC1 es para las texturas que son opacas o tienen un único color transparente.

Para cada bloque alfa opaco o de 1 bit, se almacenan los valores de 2 16 bits (formato RGB 5:6:5) y un mapa de bits 4x4 con 2 bits por píxel. Se suman los 64 bits para 16 textura, o cuatro bits por textura. En el mapa de bits de bloque, hay 2 bits por textura para seleccionar entre los cuatro colores, dos de los cuales se almacenan en los datos codificados. Los otros dos colores se derivan de estos colores almacenados mediante la interpolación lineal. Este diseño se muestra en el diagrama siguiente.

![diagrama del diseño de mapa de bits](images/colors1.png)

El formato alfa de 1 bit se distingue del formato opaco mediante la comparación de los valores de color de 2 16 bits almacenados en el bloque. Se tratan como enteros sin signo. Si el primer color es mayor que el segundo, implica que solo se definen textura opacos. Esto significa que se usan cuatro colores para representar el textura. En la codificación de cuatro colores, hay dos colores derivados y los cuatro colores se distribuyen uniformemente en el espacio de colores RGB. Este formato es análogo al formato RGB 5:6:5. De lo contrario, para la transparencia alfa de 1 bit, se usan tres colores y el cuarto se reserva para representar textura transparentes.

En la codificación de tres colores, hay un color derivado y el cuarto código de 2 bits se reserva para indicar un textura transparente (información alfa). Este formato es análogo a RGBA 5:5:5:1, donde el bit final se usa para codificar la máscara alfa.

En el ejemplo de código siguiente se muestra el algoritmo para decidir si la codificación de tres o cuatro colores está seleccionada:

```cpp
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

Se recomienda establecer los componentes RGBA del píxel de transparencia en cero antes de la fusión.

En las tablas siguientes se muestra el diseño de memoria para el bloque de 8 bytes. Se supone que el primer índice corresponde a la coordenada y y el segundo corresponde a la coordenada x. Por ejemplo, textura \[ 1 \] \[ 2 \] hace referencia al píxel del mapa de texturas en (x, y) = (2, 1).

A continuación se encuentra el diseño de memoria para el bloque de 8 bytes (64 bits):

| Dirección de palabra | Palabra de 16 bits    |
|--------------|----------------|
| 0            | Color \_ 0       |
| 1            | Color \_ 1       |
| 2            | Bitmap (palabra \_ 0) |
| 3            | Mapa de bits (palabra \_ 1) |

 

\_El color 0 y \_ el color 1, los colores de los dos extremos, se disponen de la siguiente manera:

| Bits        | Color                 |
|-------------|-----------------------|
| 4:0 (LSB \* ) | Componente de color azul  |
| 10:5        | Componente de color verde |
| 15:11       | Componente de color rojo   |

 

\*bit menos significativo

El mapa \_ de bits palabra 0 está diseñado de la siguiente manera:

| Bits          | Textura           |
|---------------|-----------------|
| 1:0 (LSB)     | Textura \[ 0 \] \[ 0\] |
| 3:2           | Textura \[ 0 \] \[ 1\] |
| 5:4           | Textura \[ 0 \] \[ 2\] |
| 7:6           | Textura \[ 0 \] \[ 3\] |
| 9:8           | Textura \[ 1 \] \[ 0\] |
| 11:10         | Textura \[ 1 \] \[ 1\] |
| 13:12         | Textura \[ 1 \] \[ 2\] |
| 15:14 (MSB \* ) | Textura \[ 1 \] \[ 3\] |

 

\*bit más significativo (MSB)

El mapa \_ de bits de la palabra 1 se dispone de la siguiente manera:

| Bits        | Textura           |
|-------------|-----------------|
| 1:0 (LSB)   | Textura \[ 2 \] \[ 0\] |
| 3:2         | Textura \[ 2 \] \[ 1\] |
| 5:4         | Textura \[ 2 \] \[ 2\] |
| 7:6         | Textura \[ 2 \] \[ 3\] |
| 9:8         | Textura \[ 3 \] \[ 0\] |
| 11:10       | Textura \[ 3 \] \[ 1\] |
| 13:12       | Textura \[ 3 \] \[ 2\] |
| 15:14 (MSB) | Textura \[ 3 \] \[ 3\] |

 

## <a name="span-idexample_of_opaque_color_encodingspanspan-idexample_of_opaque_color_encodingspanspan-idexample_of_opaque_color_encodingspanexample-of-opaque-color-encoding"></a><span id="Example_of_Opaque_Color_Encoding"></span><span id="example_of_opaque_color_encoding"></span><span id="EXAMPLE_OF_OPAQUE_COLOR_ENCODING"></span>Ejemplo de codificación de color opaca


Como ejemplo de codificación opaca, supongamos que los colores rojo y negro están en el extremo. Rojo es el color \_ 0 y el negro es el color \_ 1. Hay cuatro colores interpolados que forman el degradado distribuido uniformemente entre ellos. Para determinar los valores para el mapa de bits 4x4, se usan los cálculos siguientes:

```cpp
00 ? color_0
01 ? color_1
10 ? 2/3 color_0 + 1/3 color_1
11 ? 1/3 color_0 + 2/3 color_1
```

El mapa de bits es similar al diagrama siguiente.

![Diagrama del diseño de mapa de bits expandido para rojo y negro.](images/colors2.png)

Es similar a la siguiente serie de colores ilustrada.

**Nota:**    En una imagen, el píxel (0,0) aparece en la parte superior izquierda.

 

![Ilustración de un degradado opaco codificado](images/redsquares.png)

## <a name="span-idexample_of_1_bit_alpha_encodingspanspan-idexample_of_1_bit_alpha_encodingspanspan-idexample_of_1_bit_alpha_encodingspanexample-of-1-bit-alpha-encoding"></a><span id="Example_of_1_Bit_Alpha_Encoding"></span><span id="example_of_1_bit_alpha_encoding"></span><span id="EXAMPLE_OF_1_BIT_ALPHA_ENCODING"></span>Ejemplo de codificación alfa de 1 bit


Este formato se selecciona cuando el entero de 16 bits sin signo, el color \_ 0, es menor que el entero de 16 bits sin signo, el color \_ 1. Un ejemplo de dónde se puede usar este formato es la salida de un árbol, que se muestra con un cielo azul. Algunos textura se pueden marcar como transparentes, mientras que tres tonalidades verdes siguen estando disponibles para las hojas. Dos colores corrigen los extremos y el tercero es un color interpolado.

La siguiente ilustración es un ejemplo de este tipo de imagen.

![Ilustración de la codificación alfa de 1 bit](images/greenthing.png)

Cuando la imagen se muestra en blanco, textura se codificaría como transparente. Los componentes RGBA del textura transparente deben establecerse en cero antes de la combinación.

La codificación de mapa de bits para los colores y la transparencia se determina mediante los cálculos siguientes.

```cpp
00 ? color_0
01 ? color_1
10 ? 1/2 color_0 + 1/2 color_1
11   ?   Transparent
```

El mapa de bits es similar al diagrama siguiente.

![Diagrama del diseño de mapa de bits expandido para iluminar verde y más oscuro verde.](images/colors3.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de texturas comprimidas](compressed-texture-resources.md)

 

 




