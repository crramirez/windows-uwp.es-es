---
author: drewbatgit
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "En este artículo se enumeran las opciones de codificación que pueden usarse con BitmapEncoder."
title: Referencia de opciones de BitmapEncoder
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 510cb363b258d20688ea212856af4b7ac0311e61

---

# Referencia de opciones de BitmapEncoder

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artículo enumera las opciones de codificación que pueden usarse con [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206). Una opción de codificación se define por su nombre, que es una cadena, y un valor en un determinado tipo de datos ([**Windows.Foundation.PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871)). Para obtener información sobre cómo trabajar con imágenes, consulta [Imágenes](imaging.md).

| Nombre                    | PropertyType | Notas de uso                                                                                        | Formatos válidos |
|-------------------------|--------------|----------------------------------------------------------------------------------------------------|---------------|
| ImageQuality            | single       | Valores válidos de 0 a 1.0. Los valores más altos indican una mayor calidad.                                 | JPEG, JPEG-XR |
| CompressionQuality      | single       | Valores válidos de 0 a 1.0. Los valores más altos indican un esquema de compresión más eficaz y más lento. | TIFF          |
| Lossless                | boolean      | Si se establece en true, se omite la opción ImageQuality.                                        | JPEG-XR       |
| InterlaceOption         | boolean      | Define si se entrelaza la imagen.                                                                    | PNG           |
| FilterOption            | uint8        | Usa la enumeración [**PngFilterMode**](https://msdn.microsoft.com/library/windows/apps/br226389).                                | PNG           |
| TiffCompressionMethod   | uint8        | Usa la enumeración [**TiffCompressionMode**](https://msdn.microsoft.com/library/windows/apps/br226399).                    | TIFF          |
| Luminance               | uint32Array  | Una matriz de 64 elementos que contiene constantes de cuantificación de luminancia.                               | JPEG          |
| Chrominance             | uint32Array  | Una matriz de 64 elementos que contiene constantes de cuantificación de crominancia.                             | JPEG          |
| JpegYCrCbSubsampling    | uint8        | Usa la enumeración [**JpegSubsamplingMode**](https://msdn.microsoft.com/library/windows/apps/br226386).                    | JPEG          |
| SuppressApp0            | boolean      | Define si se suprime la creación de un bloque de metadatos App0.                                        | JPEG          |
| EnableV5Header32bppBGRA | boolean      | Define si se debe codificar a una versión 5 BMP que admita alfa.                                         | BMP           |

 

## Temas relacionados

* [Imágenes](imaging.md)
 

 







<!--HONumber=Jun16_HO4-->


