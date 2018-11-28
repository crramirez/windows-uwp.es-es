---
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: En este artículo se enumeran las opciones de codificación que pueden usarse con BitmapEncoder.
title: Referencia de opciones de BitmapEncoder
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07f5c6ef180cb4abe90a705e73be8d99ecbd2ca7
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7852543"
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

 




