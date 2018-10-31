---
author: laurenhughes
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: En este artículo se enumeran las opciones de codificación que pueden usarse con BitmapEncoder.
title: Referencia de opciones de BitmapEncoder
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 13f19ce909703b6748ab00aec1026e30d5c70a64
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5824111"
---
# <a name="bitmapencoder-options-reference"></a>Referencia de opciones de BitmapEncoder


Este artículo enumera las opciones de codificación que pueden usarse con [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206). Una opción de codificación se define por su nombre, que es una cadena, y un valor en un determinado tipo de datos ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). Para obtener más información sobre el trabajo con imágenes, consulta [Crear, editar y guardar imágenes de mapa de bits](imaging.md).

| Nombre                    | PropertyType | Notas de uso                                                                                        | Formatos válidos |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | Valores válidos de 0 a 1.0. Los valores más altos indican una mayor calidad.                                 | JPEG, JPEG-XR |
| CompressionQuality      | único       | Valores válidos de 0 a 1.0. Los valores más altos indican un esquema de compresión más eficaz y más lento. | TIFF          |
| Lossless                | booleano      | Si se establece en true, se omite la opción ImageQuality.                                        | JPEG-XR       |
| InterlaceOption         | booleano      | Define si se entrelaza la imagen.                                                                    | PNG           |
| FilterOption            | uint8        | Usa la enumeración [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389).                                | PNG           |
| TiffCompressionMethod   | uint8        | Usa la enumeración [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399).                    | TIFF          |
| Luminance               | uint32Array  | Una matriz de 64 elementos que contiene constantes de cuantificación de luminancia.                               | JPEG          |
| Chrominance             | uint32Array  | Una matriz de 64 elementos que contiene constantes de cuantificación de crominancia.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Usa la enumeración [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386).                    | JPEG          |
| SuppressApp0            | booleano      | Define si se suprime la creación de un bloque de metadatos App0.                                        | JPEG          |
| EnableV5Header32bppBGRA | booleano      | Define si se debe codificar a una versión de 5 BMP que admita alfa.                                         | BMP           |

 

## <a name="related-topics"></a>Temas relacionados

* [Crear, editar y guardar imágenes de mapa de bits](imaging.md)
* [Códecs admitidos](supported-codecs.md)

 




