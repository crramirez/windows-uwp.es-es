---
description: En este artículo se explica cómo usar la clase LowLightFusion para procesar mapas de bits.
title: Procesamiento de mapas de bits con la API de fusión de baja luz
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP, fusión de baja luz, mapas de bits, procesamiento de imágenes
ms.localizationpriority: medium
ms.openlocfilehash: 4e82eb780efe83125a09417f349f84ee9451c1f0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363838"
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Procesar mapas de bits con la API LowLightFusion

Las imágenes de poca luz son difíciles de capturar con buena calidad de imagen, especialmente en dispositivos móviles con una abertura y un tamaño de sensor fijos. Para compensar la iluminación baja, los dispositivos pueden aumentar el tiempo de exposición o la ganancia del sensor, lo que puede provocar un desenfoque de movimiento y un mayor ruido en las imágenes. 

La [clase LowLightFusion](/uwp/api/windows.media.core.lowlightfusion) mejora la calidad de las imágenes de poca luz mediante el muestreo de información de píxeles de varios fotogramas en proximidad temporal de cierre, es decir, imágenes de ráfagas cortas, para reducir el desenfoque de ruido y de movimiento. Esta es una funcionalidad útil para agregar a una aplicación de edición de fotografías, por ejemplo.

Esta característica también está disponible a través de la [clase AdvancedPhotoCapture](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture), que aplica el algoritmo de fusión de baja luz a una secuencia de imágenes directamente después de capturar las imágenes, si es necesario. Vea la captura [de fotos de baja luz](./high-dynamic-range-hdr-photo-capture.md#low-light-photo-capture) para obtener información sobre cómo implementar esta característica.

## <a name="prepare-the-images-for-processing"></a>Preparar las imágenes para su procesamiento

En este ejemplo, demostraremos cómo usar la [clase LowLightFusion](/uwp/api/windows.media.core.lowlightfusion), así como el [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir que un usuario seleccione varias imágenes para realizar la fusión con poca luz activada.

En primer lugar, es necesario determinar el número de imágenes (también conocidas como fotogramas) que el algoritmo acepta y crear una lista que contenga estos fotogramas.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetMaxLLFFrames":::

Una vez que determinamos el número de fotogramas que acepta el algoritmo de fusión de poca luz, podemos usar la [FileOpenPicker](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir al usuario elegir qué imágenes se deben usar en el algoritmo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetGetFrames":::

Ahora que tenemos el número correcto de fotogramas seleccionados, es necesario descodificar los fotogramas en [SoftwareBitmaps](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) y asegurarse de que el SoftwareBitmaps está en el formato correcto para LowLightFusion.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetDecodeFrames":::


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Confundir los mapas de bits en un único mapa de bits

Ahora que tenemos un número correcto de fotogramas en un formato aceptable, podemos usar el método **[FuseAsync](/uwp/api/windows.media.core.lowlightfusion.fuseasync)** para aplicar el algoritmo de fusión de poca luz. El resultado será la imagen procesada, con una mayor claridad, en forma de un SoftwareBitmap. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetFuseFrames":::

Por último, limpiaremos el SoftwareBitmap resultante mediante la codificación y el guardado en una imagen "normal" de usuario, similar a las imágenes de entrada con las que comenzamos.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/LowLightFusionSample/cs/MainPage.xaml.cs" id="SnippetEncodeFrame":::


## <a name="before-and-after"></a>Antes y después

A continuación se muestra un ejemplo de una imagen de entrada y la imagen de salida resultante después de aplicar el algoritmo de fusión de baja luz.

> [!div class="mx-tableFixed"] 
| Fotograma de entrada | Salida de fusión de poca luz | 
|-------------|-------------------------|
| ![Fotograma de entrada al algoritmo de fusión de baja luz](./images/LLF-Input.png) | ![Marco de resultados del algoritmo de fusión de baja luz](./images/LLF-Output.png) |

Puede ver en el marco de entrada que se han mejorado la iluminación y la claridad de las sombras que rodean el banner.

## <a name="related-topics"></a>Temas relacionados 
[Clase LowLightFusion](/uwp/api/windows.media.core.lowlightfusion)  
[Clase LowLightFusionResult](/uwp/api/windows.media.core.lowlightfusionresult)
