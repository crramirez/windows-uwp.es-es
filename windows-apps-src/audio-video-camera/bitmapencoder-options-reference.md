---
author: laurenhughes
ms.assetid: 98BD79B3-F420-43C5-98D3-52EBDDB479A0
description: "En este artículo se enumeran las opciones de codificación que pueden usarse con BitmapEncoder."
title: Referencia de opciones de BitmapEncoder
translationtype: Human Translation
ms.sourcegitcommit: fd5b52a1d431b9396a4b162077d4f8d6246cd597
ms.openlocfilehash: 4edf119d0f8830fec9ece34f1a7eb8f3ffbe67dd

---

# Referencia de opciones de BitmapEncoder

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

 

## Temas relacionados

* [Crear, editar y guardar imágenes de mapa de bits](imaging.md)
* [Códecs admitidos](supported-codecs.md)

 







<!--HONumber=Nov16_HO1-->


