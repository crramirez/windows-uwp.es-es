---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: En este artículo se enumeran las opciones de codificación que pueden usarse con BitmapEncoder.
title: Referencia de opciones de BitmapEncoder
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 212f405c943354618a4ae2bf6f2ab9c4925e7085
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161089"
---
# <a name="bitmapencoder-options-reference"></a>Referencia de opciones de BitmapEncoder


En este artículo se enumeran las opciones de codificación que se pueden usar con [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Una opción de codificación se define por su nombre, que es una cadena, y un valor en un determinado tipo de datos ([**Windows.Foundation.PropertyType**](/uwp/api/Windows.Foundation.PropertyType)). Para obtener más información sobre el trabajo con imágenes, consulta [Crear, editar y guardar imágenes de mapa de bits](imaging.md).

| Nombre                    | PropertyType | Notas de uso                                                                                        | Formatos válidos |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | sola       | Valores válidos de 0 a 1.0. Los valores más altos indican una mayor calidad.                                 | JPEG, JPEG-XR |
| CompressionQuality      | sola       | Valores válidos de 0 a 1.0. Los valores más altos indican un esquema de compresión más eficaz y más lento. | TIFF          |
| Lossless                | boolean      | Si se establece en true, se omite la opción ImageQuality.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Define si se entrelaza la imagen.                                                                    | PNG           |
| FilterOption            | uint8        | Usar la enumeración [**PngFilterMode**](/uwp/api/Windows.Graphics.Imaging.PngFilterMode)                                | PNG           |
| TiffCompressionMethod   | uint8        | Usa la enumeración [**TiffCompressionMode**](/uwp/api/Windows.Graphics.Imaging.TiffCompressionMode).                    | TIFF          |
| Luminance               | uint32Array  | Una matriz de 64 elementos que contiene constantes de cuantificación de luminancia.                               | JPEG          |
| Chrominance             | uint32Array  | Una matriz de 64 elementos que contiene constantes de cuantificación de crominancia.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Usa la enumeración [**JpegSubsamplingMode**](/uwp/api/Windows.Graphics.Imaging.JpegSubsamplingMode).                    | JPEG          |
| SuppressApp0            | boolean      | Define si se suprime la creación de un bloque de metadatos App0.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Define si se debe codificar a una versión de 5 BMP que admita alfa.                                         | BMP           |

 

## <a name="related-topics"></a>Temas relacionados

* [Crear, editar y guardar imágenes de mapa de bits](imaging.md)
* [Códecs admitidos](supported-codecs.md)

 