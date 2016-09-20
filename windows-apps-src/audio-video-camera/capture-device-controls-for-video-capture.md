---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: "En este artículo se muestra cómo los controles de dispositivo de vídeo permiten escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición."
title: "Controles de dispositivo de captura para captura de vídeos"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 65883f1be1a014b6c7e211e2e060ae97fbd9eb0d

---

# Controles de dispositivo de captura para captura de vídeos

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artículo muestra cómo los controles de dispositivo de vídeo permiten escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición.

Los controles de dispositivo de vídeo mencionados en este artículo se agregan a la aplicación con el mismo patrón. En primer lugar, se comprueba si el control es compatible con el dispositivo actual en el que se ejecuta la aplicación. Si se admite el control, establece el modo deseado para el control. Por lo general, si el dispositivo actual no admite un control en particular, debes deshabilitar u ocultar el elemento de interfaz de usuario que permita al usuario habilitar la característica.

Todas las API de control de dispositivo mencionadas en este artículo son miembros del espacio de nombres [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

**Nota**  
Este artículo se basa en los conceptos y el código analizados en [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md), donde se describen los pasos para la implementación de la captura básica de fotografías y vídeos. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código de este artículo supone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## Vídeo HDR

La característica de vídeo de alto rango dinámico (HDR) aplica el procesamiento HDR a la secuencia de vídeo del dispositivo de captura. Determina si se admite vídeo HDR comprobando la propiedad [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682).

El control de vídeo HDR admite tres modos:, activado, desactivado y automático, lo que significa que el dispositivo dinámicamente determina si el procedimiento de vídeos HDR mejora la captura multimedia y, si es así, permite vídeo HDR. Para determinar si el dispositivo actual es compatible con un modo determinado, comprueba si la colección [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) contiene el modo deseado.

Habilitar o deshabilitar el procesamiento de vídeos HDR estableciendo [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) en el modo deseado.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## Prioridad de exposición

[**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644), cuando se habilita, evalúa los fotogramas de vídeos desde el dispositivo de captura para determinar si el vídeo está capturando una escena de poca luz. Si es así, el control reduce la velocidad de fotogramas del vídeo capturado con el fin de aumentar el tiempo de exposición por cada fotograma y mejorar la calidad visual del vídeo capturado.

Determina si el control de la prioridad de exposición es compatible en el dispositivo actual comprobando la propiedad [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647).

Habilitar o deshabilitar el control de la prioridad de exposición estableciendo [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) en el modo deseado.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## Temas relacionados

* [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


