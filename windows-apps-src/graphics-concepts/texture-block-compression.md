---
title: Compresión de bloques de texturas
description: La compatibilidad de la compresión de bloques (BC) para las texturas se ha ampliado en Direct3D 11 para incluir los algoritmos BC6H y BC7.
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- Compresión de bloques de texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dec33768eff90b9bd35a3ea60f3158fce663345e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640190"
---
# <a name="texture-block-compression"></a>Compresión de bloques de texturas


La compatibilidad de la compresión de bloques (BC) para las texturas se ha ampliado en Direct3D 11 para incluir los algoritmos BC6H y BC7. BC6H admite datos de origen de color de alto rango dinámico, y BC7 proporciona una compresión de calidad mejor que el promedio con menos artefactos para los datos de origen RGB estándar.

Para obtener información más específica sobre la compatibilidad de los algoritmos de compresión por bloques anteriores a Direct3D 11, incluida la compatibilidad de los formatos de BC1 a BC5, consulta [Compresión por bloques (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb694531) (en inglés).

**Nota sobre los formatos de archivo:  ** Los formatos de compresión de textura BC6H y BC7 utilizan el formato de archivo de dominios de detección para almacenar los datos de textura comprimido. Para obtener más información, consulta la [Guía de programación para DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991) (en inglés) para obtener más información.

## <a name="span-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Bloquear los formatos de compresión admitidos en Direct3D 11


| Datos de origen                                  | Resolución mínima requerida para la compresión de datos                              | Formato recomendado | Nivel de características mínimo admitido |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| Color de tres canales con canal alfa       | Tres canales de color (5 bits:6 bits:5 bits), con 0 o 1 bits de alfa  | BC1                | Direct3D 9.1                    |
| Color de tres canales con canal alfa       | Tres canales de color (5 bits:6 bits:5 bits), con 4 bits de alfa         | BC2                | Direct3D 9.1                    |
| Color de tres canales con canal alfa       | Tres canales de color (5 bits:6 bits:5 bits), con 8 bits de alfa          | BC3                | Direct3D 9.1                    |
| Color de un canal                            | un canal de color (8 bits)                                                | BC4                | Direct3D 10                     |
| Color de dos canales                            | Dos canales de color (8 bits:8 bits)                                        | BC5                | Direct3D 10                     |
| Color de tres canales de alto rango dinámico (HDR) | En "medio" punto flotante de tres colores canales (16 bits: 16 bits: 16 bits)\* | BC6H               | Direct3D 11                     |
| Color de tres canales, canal alfa opcional  | Tres canales de color (de 4 a 7 bits por canal), con de 0 a 8 bits de alfa  | BC7                | Direct3D 11                     |

 

\*"Medio" punto flotante es un valor de 16 bits que consta de un bit de signo opcional, un poco 5 inclina exponente y un 10 o 11 bits mantisa.
## <a name="span-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1, BC2 y B3 formatos


Los formatos BC1, BC2 y BC3 son equivalentes a los formatos de compresión de textura de Direct3D 9 DXTn y son iguales que los formatos BC1, BC2 y BC3 correspondientes de Direct3D 10. Se requiere compatibilidad con estos tres formatos por todos los niveles de características (D3D\_característica\_nivel\_9\_1, D3D\_característica\_nivel\_9\_2, D3D\_ CARACTERÍSTICA\_nivel\_9\_3, D3D\_característica\_nivel\_10\_0, D3D\_característica\_nivel\_10 \_1 y D3D\_característica\_nivel\_11\_0).

| Formato de compresión por bloques | Formato DXGI                                                                           | Formato equivalente en Direct3D 9                               | Bytes por bloque de píxeles de 4 x 4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI\_FORMATO\_BC1\_UNORM, DXGI\_FORMATO\_BC1\_UNORM\_SRGB, DXGI\_FORMATO\_BC1\_TYPELESS | D3DFMT\_DXT1, FourCC = "DXT1"                                | 8                         |
| BC2                      | DXGI\_FORMATO\_BC2\_UNORM, DXGI\_FORMATO\_BC2\_UNORM\_SRGB, DXGI\_FORMATO\_BC2\_TYPELESS | D3DFMT\_DXT2\*, FourCC = "DXT2" D3DFMT\_DXT3, FourCC = "DXT3" | 16                        |
| BC3                      | DXGI\_FORMATO\_BC3\_UNORM, DXGI\_FORMATO\_BC3\_UNORM\_SRGB, DXGI\_FORMATO\_BC3\_TYPELESS | D3DFMT\_DXT4\*, FourCC = "DXT4" D3DFMT\_DXT5, FourCC = "DXT5" | 16                        |

 

\*Estos esquemas de compresión (DXT2 y DXT4) no Asegúrese de distinción entre los formatos de alfa premultiplicados de Direct3D 9 y los formatos estándar de alfa. Esta distinción deben administrarla los sombreadores programables en el tiempo de representación.

## <a name="span-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>Las texturas BC4 y formatos BC5


| Formato de compresión por bloques | Formato DXGI                                                                     | Formato equivalente en Direct3D 9 | Bytes por bloque de píxeles de 4 x 4 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI\_FORMATO\_LAS TEXTURAS BC4\_UNORM, DXGI\_FORMATO\_LAS TEXTURAS BC4\_SNORM, DXGI\_FORMATO\_LAS TEXTURAS BC4\_TYPELESS | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI\_FORMATO\_BC5\_UNORM, DXGI\_FORMATO\_BC5\_SNORM, DXGI\_FORMATO\_BC5\_TYPELESS | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6hformatspanspan-idbc6hformatspanspan-idbc6hformatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>Formato BC6H


Para obtener más información sobre este formato, consulta la documentación del enlace [Formato BC6H](https://msdn.microsoft.com/library/windows/desktop/hh308952) (en inglés).

| Formato de compresión por bloques | Formato DXGI                                                                      | Formato equivalente en Direct3D 9 | Bytes por bloque de píxeles de 4 x 4 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI\_FORMATO\_BC6H\_UF16, DXGI\_FORMATO\_BC6H\_SF16, DXGI\_FORMATO\_BC6H\_TYPELESS | N/D                          | 16                        |

 

El formato BC6H puede seleccionar diferentes modos de codificación para cada bloque de 4 x 4 píxeles. Están disponibles un total de 14 modos de codificación diferentes, cada uno con ventajas e inconvenientes ligeramente distintos en la calidad visual resultante de la textura mostrada. La elección de los modos permite una descodificación rápida por parte del hardware con el nivel de calidad seleccionado o adaptado según el contenido de origen, pero también aumenta la complejidad del espacio de búsqueda.

## <a name="span-idbc7formatspanspan-idbc7formatspanspan-idbc7formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>Formato BC7


Para obtener más información sobre este formato, consulta la documentación del enlace [Formato BC7](https://msdn.microsoft.com/library/windows/desktop/hh308953) (en inglés).

| Formato de compresión por bloques | Formato DXGI                                                                           | Formato equivalente en Direct3D 9 | Bytes por bloque de píxeles de 4 x 4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI\_FORMATO\_BC7\_UNORM, DXGI\_FORMATO\_BC7\_UNORM\_SRGB, DXGI\_FORMATO\_BC7\_TYPELESS | N/D                          | 16                        |

 

El formato BC7 puede seleccionar diferentes modos de codificación para cada bloque de 4 x 4 píxeles. Están disponibles un total de 8 modos de codificación diferentes, cada uno con ventajas e inconvenientes ligeramente distintos en la calidad visual resultante de la textura mostrada. La elección de los modos permite una descodificación rápida por parte del hardware con el nivel de calidad seleccionado o adaptado según el contenido de origen, pero también aumenta la complejidad del espacio de búsqueda.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Apéndices](appendix.md)

[Texturas](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




