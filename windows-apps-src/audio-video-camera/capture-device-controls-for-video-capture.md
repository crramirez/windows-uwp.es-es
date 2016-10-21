---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: "Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición."
title: "Controles manuales de la cámara para la captura de vídeo."
translationtype: Human Translation
ms.sourcegitcommit: daeb92e51a005825f1e410da9c924afc723297f1
ms.openlocfilehash: 5a51ee9c67eb421c2478ca46f415879afb609210

---

# Controles manuales de la cámara para la captura de vídeo.

\[ Actualizado para aplicaciones para UWP en Windows10. Para leer más artículos sobre Windows8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición.

Los controles de dispositivos de vídeo mencionados en este artículo se agregan a la aplicación con el mismo patrón. En primer lugar, comprueba si el control es compatible con el dispositivo actual en el que se ejecuta la aplicación. Si el control se admite, establece el modo deseado para él. Por lo general, si el dispositivo actual no admite un control en particular, debes deshabilitar u ocultar el elemento de la interfaz de usuario que permita al usuario habilitar esa característica.

Todas las API de control de dispositivos mencionadas en este artículo son miembros del espacio de nombres [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para la implementación de la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. El código de este artículo supone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## Vídeo HDR

La característica de vídeo de alto rango dinámico (HDR) aplica el procesamiento HDR a la secuencia de vídeo del dispositivo de captura. Determina si se admite vídeo HDR seleccionando la propiedad [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682).

El control de vídeo HDR admite tres modos:, activado, desactivado y automático, lo que significa que el dispositivo determina dinámicamente si el procesamiento de vídeo HDR mejoraría la captura multimedia y, si es así, habilita el vídeo HDR. Para determinar si el dispositivo actual es compatible con un modo determinado, comprueba si la colección [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) contiene el modo deseado.

Habilitar o deshabilitar el procesamiento de vídeos HDR estableciendo [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) en el modo deseado.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## Prioridad de exposición

[**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644), cuando se habilita, evalúa los fotogramas de vídeos desde el dispositivo de captura para determinar si el vídeo está capturando una escena de poca luz. Si es así, el control reduce la velocidad de fotogramas del vídeo capturado con el fin de aumentar el tiempo de exposición por cada fotograma y mejorar la calidad visual del vídeo capturado.

Determina si el control de la prioridad de exposición es compatible en el dispositivo actual comprobando la propiedad [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647).

Habilita o deshabilita el control de la prioridad de exposición estableciendo [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) en el modo deseado.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 







<!--HONumber=Aug16_HO3-->


