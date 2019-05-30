---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: En este artículo se muestra cómo capturar una secuencia de fotos variable, que permite capturar varios fotogramas de imágenes en sucesión rápida y configurar cada uno para que use una configuración de foco, flash, ISO, exposición y compensación de la exposición diferente.
title: Secuencia de fotos variable
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 506ea5f7fe199c3df0b6089da73a108d53d98ef3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361375"
---
# <a name="variable-photo-sequence"></a>Secuencia de fotos variable



En este artículo se muestra cómo capturar una secuencia de fotos variable, que permite capturar varios fotogramas de imágenes en sucesión rápida y configurar cada uno para que use una configuración de foco, flash, ISO, exposición y compensación de la exposición diferente. Esta característica permite escenarios como crear imágenes de alto rango dinámico (HDR).

Si deseas capturar imágenes HDR pero no quieres implementar tu propio algoritmo de procesamiento, puedes usar la API [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) para usar las funcionalidades HDR integradas en Windows. Para obtener más información, consulta [Captura de fotos de alto rango dinámico (HDR)](high-dynamic-range-hdr-photo-capture.md).

> [!NOTE] 
> Este artículo se basa en los conceptos y el código analizados en [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md), donde se describen los pasos para implementar la captura básica de fotos y vídeo. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>Configurar la aplicación para usar la captura de secuencia de fotos variable

Además de los espacios de nombres necesarios para la captura de multimedia básico, la implementación de una captura de secuencia fotográfica variable requiere los siguientes espacios de nombres.

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

Declara una variable de miembro para almacenar el objeto [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture), que se usa para iniciar la captura de secuencia de fotos. Declara una matriz de objetos [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) para almacenar cada imagen capturada en la secuencia. Además, declara una matriz para almacenar el objeto [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) por cada fotograma. Puede usarse por su algoritmo de procesamiento de imagen para determinar qué configuración se usó para capturar cada fotograma. Por último, declara un índice que se usará para dar seguimiento a la imagen en la secuencia que se está capturando en el momento.

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>Preparar la captura de secuencia de fotos variable

Después de inicializar tu objeto [MediaCapture](capture-photos-and-video-with-mediacapture.md), asegúrate de que las secuencias de fotos variables son compatibles con el dispositivo actual. Para ello, obtén una instancia del objeto [**VariablePhotoSequenceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) de la captura multimedia [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController) y comprueba la propiedad [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported).

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

Obtén un objeto [**FrameControlCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) desde el controlador de secuencia fotográfica variable. Este objeto tiene una propiedad para cada configuración que pueda configurarse por fotograma de una secuencia de fotos. Entre ellos se incluyen los siguientes:

-   [**Exposición**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**Flash**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**Foco**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

En este ejemplo se establece un valor de compensación de la exposición diferente para cada fotograma. Para comprobar que la compensación de la exposición es compatible con secuencias de fotos en el dispositivo actual, comprueba la propiedad [**Supported**](https://docs.microsoft.com/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) del objeto [**FrameExposureCompensationCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) al que se accede a través de la propiedad **ExposureCompensation**.

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

Crea un nuevo objeto [**FrameController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameController) por cada fotograma que quieras capturar. En este ejemplo se capturan tres fotogramas. Establece los valores para los controles que desees variar para cada fotograma. A continuación, borra la colección [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) de **VariablePhotoSequenceController** y agrega el controlador de cada fotograma a la colección.

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

Crea un objeto [**ImageEncodingProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) para establecer la codificación que desees usar para las imágenes capturadas. Llama al método estático [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync) y pasa las propiedades de codificación. Este método devuelve un objeto [**VariablePhotoSequenceCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture). Por último, registra controladores de eventos para los eventos [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) y [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped).

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>Inicia la captura de una secuencia de fotos variable

Para iniciar la captura de la secuencia de fotos variable, llama a [**VariablePhotoSequenceCapture.StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync). Asegúrate de inicializar las matrices para almacenar las imágenes capturadas y los valores de control de fotograma, y establece el índice actual en 0. Establece la variable de estado de grabación de la aplicación y actualiza la interfaz de usuario para deshabilitar el inicio de otra captura mientras esta está en curso.

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>Recibe los fotogramas capturados

El evento [**PhotoCaptured**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) se genera para cada fotograma capturado. Guarda los valores de control y la imagen capturada para el fotograma y, a continuación, incrementa el índice del fotograma actual. Este ejemplo muestra cómo obtener una representación [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) de cada fotograma. Para obtener más información acerca del uso de **SoftwareBitmap**, consulta [Imagen](imaging.md).

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>Administra la finalización de la captura de secuencia de fotos variable

El evento [**Stopped**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) se produce cuando todos los fotogramas de la secuencia han sido capturados. Actualizar el estado de grabación de la aplicación y actualizar la interfaz de usuario para permitir al usuario iniciar nuevas capturas. En este punto, puedes pasar las imágenes capturadas y los valores de control de fotograma al código de procesamiento de la imagen.

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>Actualiza los controladores de fotograma

Si deseas realizar otra captura de secuencia de fotos variable con una configuración diferente para cada fotograma, no necesitas reiniciar completamente **VariablePhotoSequenceCapture**. Puedes borrar la colección [**DesiredFrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) y agregar nuevos controladores de fotograma, o puedes modificar los valores del controlador de fotograma existente. El siguiente ejemplo comprueba el objeto [**FrameFlashCapabilities**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) para comprobar que el dispositivo actual es compatible con el flash y la potencia del flash para los fotogramas de una secuencia de fotos variable. Si es así, se actualiza cada fotograma para habilitar el flash al 100% de energía. Los valores de compensación de exposición que anteriormente estaban establecidos por cada fotograma siguen activos.

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>Limpia la captura de una secuencia de fotos variable

Cuando hayas terminado de capturar secuencias de fotos variables o la aplicación esté suspendida, limpia el objeto de secuencia de fotos variable con una llamada a [**FinishAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync). Anula el registro de controladores de eventos del objeto y establécelo en null.

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Capturar básica de fotos, vídeo y audio con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




