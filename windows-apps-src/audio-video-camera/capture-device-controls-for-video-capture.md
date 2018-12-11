---
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición.
title: Controles manuales de la cámara para la captura de vídeo
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f144ef398fc55e79d2f0190c61214cdf1aa93b68
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8875027"
---
# <a name="manual-camera-controls-for-video-capture"></a>Controles manuales de la cámara para la captura de vídeo



Este artículo muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición.

Los controles de dispositivos de vídeo mencionados en este artículo se agregan a la aplicación con el mismo patrón. En primer lugar, comprueba si el control es compatible con el dispositivo actual en el que se ejecuta la aplicación. Si el control se admite, establece el modo deseado para él. Por lo general, si el dispositivo actual no admite un control en particular, debes deshabilitar u ocultar el elemento de la interfaz de usuario que permita al usuario habilitar esa característica.

Todas las API de control de dispositivos mencionadas en este artículo son miembros del espacio de nombres [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902).

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para la implementación de la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios de captura más avanzados. El código de este artículo supone que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## <a name="hdr-video"></a>Vídeo HDR

La característica de vídeo de alto rango dinámico (HDR) aplica el procesamiento HDR a la secuencia de vídeo del dispositivo de captura. Determina si se admite vídeo HDR seleccionando la propiedad [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682).

El control de vídeo HDR admite tres modos:, activado, desactivado y automático, lo que significa que el dispositivo determina dinámicamente si el procesamiento de vídeo HDR mejoraría la captura multimedia y, si es así, habilita el vídeo HDR. Para determinar si el dispositivo actual es compatible con un modo determinado, comprueba si la colección [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) contiene el modo deseado.

Habilitar o deshabilitar el procesamiento de vídeos HDR estableciendo [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) en el modo deseado.

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>Prioridad de exposición

[**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644), cuando se habilita, evalúa los fotogramas de vídeos desde el dispositivo de captura para determinar si el vídeo está capturando una escena de poca luz. Si es así, el control reduce la velocidad de fotogramas del vídeo capturado con el fin de aumentar el tiempo de exposición por cada fotograma y mejorar la calidad visual del vídeo capturado.

Determina si el control de la prioridad de exposición es compatible en el dispositivo actual comprobando la propiedad [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647).

Habilita o deshabilita el control de la prioridad de exposición estableciendo [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) en el modo deseado.

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="temporal-denoising"></a>Eliminación de ruido temporal
A partir de Windows 10, versión 1803, puedes habilitar la eliminación de ruido temporal para vídeo en dispositivos que lo admitan. Esta característica fusiona los datos de imagen de varios fotogramas adyacentes en tiempo real para producir fotogramas de vídeo que tengan menos ruido visual.

El [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol) permite a la aplicación determinar si se admite la eliminación de ruido temporal en el dispositivo actual y, de ser así, qué modos de eliminación de ruido se admiten. Los modos de eliminación de ruido disponibles son [**Desactivado**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode), [**Activado**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode) y [**Automático**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingmode). Un dispositivo no puede admitir todos los modos, pero cada dispositivo debe admitir **Auto** o **On** y **Off**.

El siguiente ejemplo usa una interfaz de usuario sencilla para proporcionar botones de radio que permiten al usuario pasar de un modo de eliminación de ruido a otro.

[!code-cs[SnippetDenoiseXAML](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetDenoiseXAML)]

En el siguiente método, la propiedad [**VideoTemporalDenoisingControl.Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.supported) se comprueba para ver si se admite la eliminación de ruido temporal en todo el dispositivo actual. De ser así, comprobamos para asegurarnos de que se admiten **Off** y **Auto** o **On**, en cuyo caso hacemos que nuestros botones de radio estén visibles. A continuación, los botones **Auto** y **On** se vuelven visibles si se admiten esos métodos.

[!code-cs[SnippetUpdateDenoiseCapabilities](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetUpdateDenoiseCapabilities)]

En el controlador de eventos **Checked** para los botones de radio, se comprueba el nombre del botón y se establece el modo correspondiente al establecer la propiedad [**VideoTemporalDenoisingControl.Mode**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol.mode).

[!code-cs[SnippetDenoiseButtonChecked](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseButtonChecked)]

### <a name="disabling-temporal-denoising-while-processing-frames"></a>Deshabilitar la eliminación de ruido temporal durante el procesamiento de fotogramas
El vídeo que se ha procesado mediante la eliminación de ruido temporal puede ser más agradable para el ojo humano. Sin embargo, dado que la eliminación de ruido temporal puede afectar a la coherencia de la imagen y disminuir la cantidad de detalles del fotograma, es posible que las aplicaciones que realizan el procesamiento de imágenes en los fotogramas, como el registro o el reconocimiento óptico de caracteres, deseen deshabilitar mediante programación la eliminación de ruido cuando el procesamiento de imágenes está habilitado.

El siguiente ejemplo determina qué modos de eliminación de ruido se admiten y almacena esta información en algunas variables de clase.

[!code-cs[SnippetDenoiseFrameReaderVars](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseFrameReaderVars)]

[!code-cs[SnippetDenoiseCapabilitiesForFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDenoiseCapabilitiesForFrameProcessing)]

Cuando la aplicación habilita el procesamiento de fotogramas, establece el modo de eliminación de ruido en **Off** si se admite ese modo para que el procesamiento de fotogramas pueda usar fotogramas sin procesar de los que no se haya eliminado el ruido.

[!code-cs[SnippetEnableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetEnableFrameProcessing)]

Cuando la aplicación deshabilita el procesamiento de fotogramas, establece el modo de eliminación de ruido en **On** o **Auto**, en función del modo que se admite.

[!code-cs[SnippetDisableFrameProcessing](./code/BasicMediaCaptureWin10/cs/MainPage.ManualControls.xaml.cs#SnippetDisableFrameProcessing)]

Para obtener más información acerca de la obtención de fotogramas de vídeo para el procesamiento de imágenes, consulta [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md).

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
*  [**VideoTemporalDenoisingControl**](https://docs.microsoft.com/uwp/api/windows.media.devices.videotemporaldenoisingcontrol)
 




