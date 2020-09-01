---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición.
title: Controles manuales de la cámara para la captura de vídeo.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d484571f69025ceb1ce8c6eeb827c46cacfa15ed
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160979"
---
# <a name="manual-camera-controls-for-video-capture"></a>Controles manuales de la cámara para la captura de vídeo.



Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición.

Los controles de dispositivos de vídeo mencionados en este artículo se agregan a la aplicación con el mismo patrón. En primer lugar, comprueba si el control es compatible con el dispositivo actual en el que se ejecuta la aplicación. Si el control se admite, establece el modo deseado para él. Por lo general, si el dispositivo actual no admite un control en particular, debes deshabilitar u ocultar el elemento de la interfaz de usuario que permita al usuario habilitar esa característica.

Todas las API de control de dispositivos mencionadas en este artículo son miembros del espacio de nombres [**Windows.Media.Devices**](/uwp/api/Windows.Media.Devices).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## <a name="hdr-video"></a>Vídeo HDR

La característica de vídeo de alto rango dinámico (HDR) aplica el procesamiento HDR a la secuencia de vídeo del dispositivo de captura. Determina si se admite vídeo HDR seleccionando la propiedad [**HdrVideoControl.Supported**](/uwp/api/windows.media.devices.hdrvideocontrol.supported).

El control de vídeo HDR admite tres modos:, activado, desactivado y automático, lo que significa que el dispositivo determina dinámicamente si el procesamiento de vídeo HDR mejoraría la captura multimedia y, si es así, habilita el vídeo HDR. Para determinar si el dispositivo actual es compatible con un modo determinado, comprueba si la colección [**HdrVideoControl.SupportedModes**](/uwp/api/windows.media.devices.hdrvideocontrol.supportedmodes) contiene el modo deseado.

Habilitar o deshabilitar el procesamiento de vídeos HDR estableciendo [**HdrVideoControl.Mode**](/uwp/api/windows.media.devices.hdrvideocontrol.mode) en el modo deseado.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>Prioridad de exposición

[**ExposurePriorityVideoControl**](/uwp/api/Windows.Media.Devices.ExposurePriorityVideoControl), cuando se habilita, evalúa los fotogramas de vídeos desde el dispositivo de captura para determinar si el vídeo está capturando una escena de poca luz. Si es así, el control reduce la velocidad de fotogramas del vídeo capturado con el fin de aumentar el tiempo de exposición por cada fotograma y mejorar la calidad visual del vídeo capturado.

Determina si el control de la prioridad de exposición es compatible en el dispositivo actual comprobando la propiedad [**ExposurePriorityVideoControl.Supported**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.supported).

Habilita o deshabilita el control de la prioridad de exposición estableciendo [**ExposurePriorityVideoControl.Enabled**](/uwp/api/windows.media.devices.exposurepriorityvideocontrol.enabled) en el modo deseado.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="temporal-denoising"></a>Denoising temporal
A partir de Windows 10, versión 1803, puede habilitar denoising temporales para vídeo en los dispositivos que lo admiten. Esta característica funde los datos de imagen de varios fotogramas adyacentes en tiempo real para producir fotogramas de vídeo con menos ruido visual.

[**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) permite a la aplicación determinar si se admite el denoising temporal en el dispositivo actual y, en ese caso, qué modos denoising se admiten. Los modos de denoising disponibles son [**OFF**](/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**on**](/uwp/api/windows.media.devices.videotemporaldenoisingmode)y [**auto**](/uwp/api/windows.media.devices.videotemporaldenoisingmode). Es posible que un dispositivo no admita todos los modos, pero todos los dispositivos deben ser compatibles con el modo **automático** o **activado** y **desactivado**.

En el ejemplo siguiente se usa una interfaz de usuario simple para proporcionar botones de radio que permiten al usuario cambiar entre los modos denoising.

[!code-xml[SnippetDenoiseXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetDenoiseXAML)]

En el método siguiente, se comprueba la propiedad [**VideoTemporalDenoisingControl. Supported**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) para ver si se admite denoising temporal en el dispositivo actual. Si es así, comprobamos que se admita **OFF** y **auto** o **on** , en cuyo caso hacemos que los botones de radio estén visibles. A continuación, los botones **automático** y **activado** se hacen visibles si se admiten esos métodos.

[!code-cs[SnippetUpdateDenoiseCapabilities](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetUpdateDenoiseCapabilities)]

En el controlador de eventos **Checked** de los botones de radio, se activa el nombre del botón y el modo correspondiente se establece mediante el establecimiento de la propiedad [**VideoTemporalDenoisingControl. Mode**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode) .

[!code-cs[SnippetDenoiseButtonChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseButtonChecked)]

### <a name="disabling-temporal-denoising-while-processing-frames"></a>Deshabilitar denoising temporales al procesar fotogramas
El vídeo que se ha procesado con denoising temporales puede ser más agradable para el ojo humano. Sin embargo, dado que los denoising temporales pueden afectar a la coherencia de la imagen y reducir la cantidad de detalles en el marco, es posible que las aplicaciones que realizan el procesamiento de imágenes en los fotogramas, como el registro o el reconocimiento óptico de caracteres, deseen deshabilitar denoising mediante programación cuando se habilita el procesamiento de imágenes.

En el ejemplo siguiente se determinan los modos denoising que se admiten y se almacena esta información en algunas variables de clase.

[!code-cs[SnippetDenoiseFrameReaderVars](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseFrameReaderVars)]

[!code-cs[SnippetDenoiseCapabilitiesForFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseCapabilitiesForFrameProcessing)]

Cuando la aplicación habilita el procesamiento de fotogramas, establece el modo de denoising en **OFF** si se admite ese modo, de modo que el procesamiento de fotogramas pueda usar fotogramas sin ruido que no se hayan desconectado.

[!code-cs[SnippetEnableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEnableFrameProcessing)]

Cuando la aplicación deshabilita Frame prcessing, establece el modo denoising en **on** o **auto**, en función del modo que se admita.

[!code-cs[SnippetDisableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDisableFrameProcessing)]

Para obtener más información sobre cómo obtener fotogramas de vídeo para el procesamiento de imágenes, consulte [procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md).

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 