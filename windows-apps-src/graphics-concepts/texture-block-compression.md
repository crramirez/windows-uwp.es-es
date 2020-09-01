---
title: Compresión de bloques de texturas
description: La compatibilidad con la compresión de bloques (BC) para las texturas se ha ampliado en Direct3D 11 para incluir los algoritmos BC6H y BC7.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- Compresión de bloques de texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06c132f23d22dc688f82ff7976bcb93108f2ee1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158819"
---
# <a name="texture-block-compression"></a>Compresión de bloques de texturas


La compatibilidad con la compresión de bloques (BC) para las texturas se ha ampliado en Direct3D 11 para incluir los algoritmos BC6H y BC7. BC6H es compatible con los datos de origen de color de alto nivel dinámico y BC7 proporciona una compresión de calidad mejor que la media con menos artefactos para los datos de origen RGB estándar.

Para obtener información más específica acerca de la compatibilidad con algoritmos de compresión de bloque antes de Direct3D 11, incluida la compatibilidad con los formatos de BC1 a BC5, vea [compresión de bloques (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression).

**Nota sobre los formatos de archivo:  ** Los formatos de compresión de textura BC6H y BC7 usan el formato de archivo DDS para almacenar los datos de textura comprimidos. Para obtener más información, consulte la [Guía de programación para DDS](/windows/desktop/direct3ddds/dx-graphics-dds-pguide) .

## <a name="span-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Formatos de compresión de bloque admitidos en Direct3D 11


| Datos de origen                                  | Resolución de compresión de datos mínima requerida                              | Formato recomendado | Nivel mínimo de características admitidas |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| Color de tres canales con canal alfa       | Tres canales de color (5 bits: 6 bits: 5 bits), con 0 o 1 bit (s) de alfa  | BC1                | Direct3D 9,1                    |
| Color de tres canales con canal alfa       | Tres canales de color (5 bits: 6 bits: 5 bits), con 4 bits de alfa         | BC2                | Direct3D 9,1                    |
| Color de tres canales con canal alfa       | Tres canales de color (5 bits: 6 bits: 5 bits) con 8 bits de alfa          | BC3                | Direct3D 9,1                    |
| Color de un canal                            | Un canal de color (8 bits)                                                | LAS texturas BC4                | Direct3D 10                     |
| Color de dos canales                            | Dos canales de color (8 bits: 8 bits)                                        | BC5                | Direct3D 10                     |
| Color de intervalo dinámico alto (HDR) de tres canales | Tres canales de color (16 bits: 16 bits: 16 bits) en el punto flotante "medio"\* | BC6H               | Direct3D 11                     |
| Color de tres canales, canal alfa opcional  | Tres canales de color (de 4 a 7 bits por canal) con de 0 a 8 bits de alfa  | BC7                | Direct3D 11                     |

 

\*El punto flotante "medio" es un valor de 16 bits que consta de un bit de signo opcional, un exponente sesgado de 5 bits y una mantisa de 10 o 11 bits.
## <a name="span-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>Formatos BC1, BC2 y B3


Los formatos BC1, BC2 y BC3 son equivalentes a los formatos de compresión de textura de Direct3D 9 DXTn y son los mismos que los correspondientes formatos de Direct3D 10 BC1, BC2 y BC3. La compatibilidad con estos tres formatos es necesaria para todos los niveles de características (nivel de característica de D3D \_ \_ \_ 9 \_ 1, nivel de característica de D3D \_ \_ \_ 9 \_ 2, nivel de característica de D3D \_ \_ 9 3, nivel de característica de D3D \_ \_ \_ \_ \_ 10 \_ 0, \_ \_ nivel de característica de D3D \_ 10 \_ 1 y \_ nivel de característica \_ \_ 11 0 de D3D \_ .

| Formato de compresión de bloque | Formato de DXGI                                                                           | Formato equivalente de Direct3D 9                               | Bytes por bloque de píxeles 4x4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI \_ format \_ BC1 \_ UNORM, \_ formato dxgi \_ BC1 \_ UNORM \_ sRGB, dxgi \_ format \_ BC1 \_ sin tipo | D3DFMT \_ DXT1, FourCC = "DXT1"                                | 8                         |
| BC2                      | DXGI \_ format \_ BC2 \_ UNORM, \_ formato dxgi \_ BC2 \_ UNORM \_ sRGB, dxgi \_ format \_ BC2 \_ sin tipo | D3DFMT \_ DXT2 \* , FourCC = "DXT2", D3DFMT \_ DXT3, FourCC = "DXT3" | 16                        |
| BC3                      | DXGI \_ format \_ BC3 \_ UNORM, \_ formato dxgi \_ BC3 \_ UNORM \_ sRGB, dxgi \_ format \_ BC3 \_ sin tipo | D3DFMT \_ DXT4 \* , FourCC = "DXT4", D3DFMT \_ DXT5, FourCC = "DXT5" | 16                        |

 

\*Estos esquemas de compresión (DXT2 y DXT4) no realizan ninguna distinción entre los formatos alfa de Direct3D 9 premultiplicados y los formatos alfa estándar. Los sombreadores programables deben controlar esta distinción en el momento de la representación.

## <a name="span-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>Formatos las texturas BC4 y BC5


| Formato de compresión de bloque | Formato de DXGI                                                                     | Formato equivalente de Direct3D 9 | Bytes por bloque de píxeles 4x4 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| LAS texturas BC4                      | Formato de DXGI \_ \_ las texturas BC4 \_ UNORM, \_ formato de dxgi \_ las texturas BC4 \_ SNORM, formato de dxgi \_ \_ las texturas BC4 \_ sin tipo | FourCC = "ATI1"                | 8                         |
| BC5                      | Formato de DXGI \_ \_ BC5 \_ UNORM, \_ formato de dxgi \_ BC5 \_ SNORM, formato de dxgi \_ \_ BC5 \_ sin tipo | FourCC = "ATI2"                | 16                        |

 

## <a name="span-idbc6h_formatspanspan-idbc6h_formatspanspan-idbc6h_formatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>Formato BC6H


Para obtener información más detallada sobre este formato, vea la documentación de [BC6H Format](/windows/desktop/direct3d11/bc6h-format) .

| Formato de compresión de bloque | Formato de DXGI                                                                      | Formato equivalente de Direct3D 9 | Bytes por bloque de píxeles 4x4 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | Formato de DXGI \_ \_ BC6H \_ UF16, \_ formato de dxgi \_ BC6H \_ SF16, formato de dxgi \_ \_ BC6H \_ sin tipo | N/D                          | 16                        |

 

El formato BC6H puede seleccionar diferentes modos de codificación para cada bloque de píxeles de 4x4. Hay disponible un total de 14 modos de codificación diferentes, cada uno con un equilibrio ligeramente diferente en la calidad visual resultante de la textura mostrada. La elección de los modos permite la descodificación rápida por el hardware con el nivel de calidad seleccionado o adaptado según el contenido de origen, pero también aumenta en gran medida la complejidad del espacio de búsqueda.

## <a name="span-idbc7_formatspanspan-idbc7_formatspanspan-idbc7_formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>Formato BC7


Para obtener información más detallada sobre este formato, vea la documentación de [BC7 Format](/windows/desktop/direct3d11/bc7-format) .

| Formato de compresión de bloque | Formato de DXGI                                                                           | Formato equivalente de Direct3D 9 | Bytes por bloque de píxeles 4x4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI \_ format \_ BC7 \_ UNORM, \_ formato dxgi \_ BC7 \_ UNORM \_ sRGB, dxgi \_ format \_ BC7 \_ sin tipo | N/D                          | 16                        |

 

El formato BC7 puede seleccionar diferentes modos de codificación para cada bloque de píxeles de 4x4. Hay disponible un total de 8 modos de codificación diferentes, cada uno con un equilibrio ligeramente diferente en la calidad visual resultante de la textura mostrada. La elección de los modos permite la descodificación rápida por el hardware con el nivel de calidad seleccionado o adaptado según el contenido de origen, pero también aumenta en gran medida la complejidad del espacio de búsqueda.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Apéndices](appendix.md)

[Texturas](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 