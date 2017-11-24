---
author: laurenhughes
description: "En este artículo se explica cómo usar la clase LowLightFusion para procesar mapas de bits."
title: Procesar mapas de bits la API Low Light Fusion
ms.author: lahugh
ms.date: 11/02/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, fusión de poca luz, low light fusion, mapas de bits, procesamiento de imágenes"
localizationpriority: medium
ms.openlocfilehash: 989571063603a7133f39961b4b32bdc60fc373dc
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2017
---
# <a name="process-bitmaps-with-the-lowlightfusion-api"></a>Procesar mapas de bits la API LowLightFusion

Las imágenes con poca luz son difíciles de capturar con una buena calidad, especialmente en dispositivos móviles con un tamaño fijo de apertura y sensor. Para compensar la baja iluminación, los dispositivos pueden aumentar el tiempo de exposición o la ganancia del sensor, lo que puede provocar un desenfoque del movimiento y una mayor ruido en las imágenes. 

La [clase LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion) mejora la calidad de las imágenes con poca luz al realizar un muestreo de la información de píxeles de varios fotogramas en proximidad temporal, por ejemplo, imágenes de ráfagas, para reducir el ruido y el desenfoque del movimiento. Esta es una funcionalidad útil para agregarla a una aplicación de edición de fotos, por ejemplo.

Esta característica también está disponible a través de la [clase AdvancedPhotoCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture), que aplica el algoritmo de Low Light Fusion a una secuencia de imágenes directamente tras capturarlas, si es necesario. Consulta [Captura de fotos con poca luz](https://docs.microsoft.com/windows/uwp/audio-video-camera/high-dynamic-range-hdr-photo-capture#low-light-photo-capture) para obtener información sobre cómo implementar esta característica.

## <a name="prepare-the-images-for-processing"></a>Preparación de las imágenes para su procesamiento

En este ejemplo mostraremos cómo usar la [clase LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion), así como [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker), para permitir a un usuario seleccionar varias imágenes en las que realizar la fusión de poca luz (Low Light Fusion).

En primer lugar tendremos que determinar cuántas imágenes (también conocidas como fotogramas) acepta el algoritmo y luego crearemos una lista que contenga estos fotogramas.

[!code-cs[SnippetGetMaxLLFFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetMaxLLFFrames)]

Una vez que determinemos cuántos fotogramas acepta el algoritmo de Low Light Fusion, podremos usar [FileOpenPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir al usuario elegir qué imágenes se deben utilizar en el algoritmo.

[!code-cs[SnippetGetFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetGetFrames)]

Ahora que hemos seleccionado el número correcto de fotogramas, debemos descodificar dichos fotogramas en [SoftwareBitmaps](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) y asegurarnos de que estos SoftwareBitmaps están en el formato correcto para LowLightFusion.

[!code-cs[SnippetDecodeFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetDecodeFrames)]


## <a name="fuse-the-bitmaps-into-a-single-bitmap"></a>Fusión de los mapas de bits en un único mapa de bits

Ahora que tenemos un número correcto de fotogramas en un formato aceptable, podemos usar el método **[FuseAsync](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion#Windows_Media_Core_LowLightFusion_FuseAsync_Windows_Foundation_Collections_IIterable_Windows_Graphics_Imaging_SoftwareBitmap__)** para aplicar el algoritmo de Low Light Fusion. El resultado será la imagen procesada, con la claridad mejorada, con la forma de un SoftwareBitmap. 

[!code-cs[SnippetFuseFrames](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetFuseFrames)]

Por último, limpiaremos el SoftwareBitmap resultante mediante codificación y lo guardaremos en una imagen "normal" fácil de usar, similar a las imágenes de entrada con las que hemos empezamos el procedimiento.

[!code-cs[SnippetEncodeFrame](./code/LowLightFusionSample/cs/MainPage.xaml.cs#SnippetEncodeFrame)]


## <a name="before-and-after"></a>Antes y después

Este es un ejemplo de dos imágenes de entrada y la imagen de salida resultante después de aplicar el algoritmo de Low Light Fusion.

> [!div class="mx-tableFixed"] 
| Fotograma 1 | Fotograma 2 | Salida de fusión de poca luz | 
|---------|---------|-------------------------|
| ![Primer fotograma de entrada para el algoritmo de Low Light Fusion](./images/LLF-Input1.png) | ![Segundo fotograma de entrada para el algoritmo de Low Light Fusion](./images/LLF-Input2.png) | ![Fotograma resultante del algoritmo de Low Light Fusion](./images/LLF-Output.png) |

En los fotogramas de entrada y el fotograma de salida puede ver que se han mejorado la iluminación y la claridad de la copa, así como la claridad del entorno, como la moldura junto a la alfombra.

## <a name="related-topics"></a>Temas relacionados 
[Clase LowLightFusion](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusion)  
[Clase LowLightFusionResult](https://docs.microsoft.com/uwp/api/windows.media.core.lowlightfusionresult)