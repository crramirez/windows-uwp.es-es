---
ms.assetid: B5D915E4-4280-422C-BA0E-D574C534410B
description: En este artículo se describe cómo usar las clases SceneAnalysisEffect y FaceDetectionEffect para analizar el contenido de la secuencia de vista previa de captura de multimedia.
title: Efectos para analizar fotogramas de cámara
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 406af54cfaae8710cea2d989278a16f28c8dd619
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74256215"
---
# <a name="effects-for-analyzing-camera-frames"></a>Efectos para analizar fotogramas de cámara



En este artículo se describe cómo usar las clases [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) y [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) para analizar el contenido de la secuencia de vista previa de captura de multimedia.

## <a name="scene-analysis-effect"></a>Efecto de análisis de escena

La clase [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect) analiza los fotogramas de vídeo en la secuencia de vista previa de captura multimedia y recomienda opciones de procesamiento para mejorar el resultado de la captura. Actualmente, el efecto admite detectar si la captura podría mejorarse mediante el uso de procesamiento de alto intervalo dinámico (HDR).

Si el efecto recomienda usar HDR, puedes hacerlo de las siguientes maneras:

-   Usa la clase [**AdvancedPhotoCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) para capturar fotos con el algoritmo de procesamiento HDR integrado de Windows. Para obtener más información, consulta [Captura de fotos de alto rango dinámico (HDR)](high-dynamic-range-hdr-photo-capture.md).

-   Usa la clase [**HdrVideoControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.HdrVideoControl) para capturar vídeo con el algoritmo de procesamiento HDR integrado de Windows. Para obtener más información, consulta [Controles de dispositivo de captura para captura de vídeos](capture-device-controls-for-video-capture.md).

-   Usa [**VariablePhotoSequenceControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) para capturar una secuencia de fotogramas que después puedas componer mediante una implementación personalizada de HDR. Para obtener más información, consulta [Secuencia de fotos variable](variable-photo-sequence.md).

### <a name="scene-analysis-namespaces"></a>Espacios de nombres de análisis de escena

Para usar el análisis de la escena, tu aplicación debe incluir los siguientes espacios de nombres, además de los espacios de nombres necesarios para la captura multimedia básica.

[!code-cs[SceneAnalysisUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalysisUsing)]

### <a name="initialize-the-scene-analysis-effect-and-add-it-to-the-preview-stream"></a>Iniciar el análisis de la escena y añadirlo a la secuencia de vista previa

Efectos de vídeo se implementan mediante dos API, una definición de efecto, que proporciona la configuración que necesita el dispositivo de captura inicializar el efecto, y una instancia de efecto, que puede usarse para controlar el efecto. Dado que seguramente quieras obtener acceso a la instancia de efecto desde varias ubicaciones dentro del código, por lo general deberás declarar una variable de miembro que contenga el objeto.

[!code-cs[DeclareSceneAnalysisEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSceneAnalysisEffect)]

En la aplicación, después de inicializar el objeto **MediaCapture**, crea una nueva instancia de [**SceneAnalysisEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffectDefinition).

Registra el efecto con el dispositivo de captura llamando a [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) en tu objeto **MediaCapture** mediante **SceneAnalysisEffectDefinition** y especificando [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) para indicar que el efecto debe aplicarse en la secuencia de vista previa de vídeo, en lugar de la secuencia de captura. **AddVideoEffectAsync** devuelve una instancia del efecto agregado. Dado que este método puede usarse con varios tipos de efecto, se debe convertir la instancia devuelta a un objeto [**SceneAnalysisEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffect).

Para recibir los resultados del análisis de la escena, debes registrar un controlador para el evento [**SceneAnalyzed**](https://docs.microsoft.com/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed).

Actualmente, el efecto de análisis de escena solo incluye el analizador de alto intervalo dinámico. Habilita el análisis de HDR estableciendo la propiedad [**HighDynamicRangeControl.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) del efecto en true.

[!code-cs[CreateSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateSceneAnalysisEffectAsync)]

### <a name="implement-the-sceneanalyzed-event-handler"></a>Implementa el controlador de eventos SceneAnalyzed

Los resultados del análisis de la escena se devuelven en el controlador de eventos **SceneAnalyzed**. El objeto [**SceneAnalyzedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalyzedEventArgs) pasado al controlador tiene un objeto [**SceneAnalysisEffectFrame**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.SceneAnalysisEffectFrame) que tiene un objeto [**HighDynamicRangeOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.HighDynamicRangeOutput). La propiedad [**Certainty**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangeoutput.certainty) de la salida de alto intervalo dinámico proporciona un valor entre 0 y 1,0, donde 0 indica que el procesamiento de HDR no podría ayudar a mejorar los resultados de la captura y 1,0 indica que el procesamiento de HDR podría ayudar. Puedes decidir el punto de umbral en el que quieres usar HDR, o mostrar los resultados al usuario y que el usuario decida.

[!code-cs[SceneAnalyzed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSceneAnalyzed)]

El objeto [**HighDynamicRangeOutput**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.HighDynamicRangeOutput) pasado al controlador también tiene una propiedad [**FrameControllers**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangeoutput.framecontrollers) que contiene los controladores de fotograma sugeridos para capturar una secuencia fotográfica variable para el procesamiento de HDR. Para obtener más información, consulta [Secuencia de fotos variable](variable-photo-sequence.md).

### <a name="clean-up-the-scene-analysis-effect"></a>Limpiar el efecto de análisis de la escena

Cuando la aplicación haya terminado de capturar, antes de eliminar el objeto **MediaCapture**, debes deshabilitar el efecto de análisis de la escena estableciendo la propiedad [**HighDynamicRangeAnalyzer.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.highdynamicrangecontrol.enabled) del efecto en "false", y anular el registro del controlador de eventos [**SceneAnalyzed**](https://docs.microsoft.com/uwp/api/windows.media.core.sceneanalysiseffect.sceneanalyzed). Llama a [**MediaCapture.ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync), especificando el flujo de vista previa del vídeo, ya que ese era el flujo al que se agregó el efecto. Por último, establece la variable de miembro en "null".

[!code-cs[CleanUpSceneAnalysisEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpSceneAnalysisEffectAsync)]

## <a name="face-detection-effect"></a>Efecto de detección de rostros

La clase [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect) identifica la ubicación de rostros dentro de la secuencia de vista previa de captura multimedia. El efecto te permite recibir una notificación siempre que se detecta en la secuencia de vista previa de una cara y proporciona el cuadro de límite para cada rostro detectado en el plazo de vista previa. En dispositivos compatibles, el efecto de detección de rostro también proporciona la exposición mejorada y centrarse en la cara más importante de la escena.

### <a name="face-detection-namespaces"></a>Espacios de nombres de detección de rostros

Para usar la detección de rostros, tu aplicación debe incluir los siguientes espacios de nombres además de los espacios de nombres necesarios para la captura multimedia básica.

[!code-cs[FaceDetectionUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetectionUsing)]

### <a name="initialize-the-face-detection-effect-and-add-it-to-the-preview-stream"></a>Iniciar el efecto de detección de rostros y añadirlo a la secuencia de vista previa

Efectos de vídeo se implementan mediante dos API, una definición de efecto, que proporciona la configuración que necesita el dispositivo de captura inicializar el efecto, y una instancia de efecto, que puede usarse para controlar el efecto. Dado que seguramente quieras obtener acceso a la instancia de efecto desde varias ubicaciones dentro del código, por lo general deberás declarar una variable de miembro que contenga el objeto.

[!code-cs[DeclareFaceDetectionEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareFaceDetectionEffect)]

En la aplicación, después de inicializar el objeto **MediaCapture**, crea una nueva instancia de [**FaceDetectionEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffectDefinition). Establecer la propiedad [**DetectionMode**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectdefinition.detectionmode) para priorizar la rapidez o la precisión de la detección de rostros. Establece [**SynchronousDetectionEnabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectdefinition.synchronousdetectionenabled) para especificar que las tramas entrantes no se retrasen esperando que se complete la detección de rostros, ya que esto puede provocar una experiencia de vista previa entrecortada.

Registra el efecto con el dispositivo de captura llamando a [**AddVideoEffectAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.addvideoeffectasync) en tu objeto **MediaCapture** mediante **FaceDetectionEffectDefinition** y especificando [**MediaStreamType.VideoPreview**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaStreamType) para indicar que el efecto debe aplicarse en la secuencia de vista previa de vídeo, en lugar de la secuencia de captura. **AddVideoEffectAsync** devuelve una instancia del efecto agregado. Dado que este método puede usarse con varios tipos de efecto, se debe convertir la instancia devuelta a un objeto [**FaceDetectionEffect**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectionEffect).

Habilita o deshabilita el efecto estableciendo la propiedad [**FaceDetectionEffect.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.enabled). Ajusta la frecuencia con que el efecto analiza los fotogramas estableciendo la propiedad [**FaceDetectionEffect.DesiredDetectionInterval**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.desireddetectioninterval). Estas dos propiedades se pueden ajustar mientras se está realizando la captura multimedia.

[!code-cs[CreateFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFaceDetectionEffectAsync)]

### <a name="receive-notifications-when-faces-are-detected"></a>Recibir notificaciones cuando se detectan rostros

Si quieres realizar alguna acción cuando se detectan rostros, como dibujar un cuadro alrededor de los rostros detectados en la vista previa del vídeo, puedes registrarte en los eventos [**FaceDetected**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.facedetected).

[!code-cs[RegisterFaceDetectionHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterFaceDetectionHandler)]

En el controlador para el evento, puedes obtener una lista de todos los rostros detectados en un marco si accedes a la propiedad [**FaceDetectionEffectFrame.DetectedFaces**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffectframe.detectedfaces) de la clase [**FaceDetectedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.FaceDetectedEventArgs). La propiedad [**FaceBox**](https://docs.microsoft.com/uwp/api/windows.media.faceanalysis.detectedface.facebox) es una estructura [**BitmapBounds**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBounds) que describe el rectángulo que contiene el rostro detectado en unidades con respecto a las dimensiones de la secuencia de vista previa. Para ver código de ejemplo que transforma las coordenadas de la secuencia de vista previa en coordenadas de pantalla, consulta [face detection UWP sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFaceDetection) (Muestra de UWP de detección de rostro).

[!code-cs[FaceDetected](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFaceDetected)]

### <a name="clean-up-the-face-detection-effect"></a>Limpiar el efecto de detección de rostro

Cuando la aplicación haya terminado capturar, antes de eliminar el objeto **MediaCapture**, deberías deshabilitar el efecto de detección de rostros con [**FaceDetectionEffect.Enabled**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.enabled) y anular el registro del controlador de eventos [**FaceDetected**](https://docs.microsoft.com/uwp/api/windows.media.core.facedetectioneffect.facedetected), si has registrado uno anteriormente. Llama a [**MediaCapture.ClearEffectsAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.cleareffectsasync), especificando el flujo de vista previa del vídeo, ya que ese era el flujo al que se agregó el efecto. Por último, establece la variable de miembro en "null".

[!code-cs[CleanUpFaceDetectionEffectAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpFaceDetectionEffectAsync)]

### <a name="check-for-focus-and-exposure-support-for-detected-faces"></a>Comprobar la compatibilidad del foco y la exposición de rostros detectados

No todos los dispositivos tienen un dispositivo de captura que puede ajustar el foco y la exposición en función de rostros detectados. Dado que la detección de rostro consume recursos del dispositivo, puede que solo desees habilitar la detección de rostro en dispositivos que pueden usar la característica para mejorar la captura. Para ver si la optimización de captura de rostros está disponible, obtén el [**VideoDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.VideoDeviceController) para tu [MediaCapture](capture-photos-and-video-with-mediacapture.md) inicializado y luego el [**RegionsOfInterestControl**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.RegionsOfInterestControl) del controlador de dispositivo de vídeo. Comprueba si [**MaxRegions**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.maxregions) admite al menos una región. Luego comprueba si [**AutoExposureSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.autoexposuresupported) o [**AutoFocusSupported**](https://docs.microsoft.com/uwp/api/windows.media.devices.regionsofinterestcontrol.autofocussupported) son true. Si se cumplen estas condiciones, el dispositivo puede aprovechar la detección de rostros para mejorar la captura.

[!code-cs[AreFaceFocusAndExposureSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetAreFaceFocusAndExposureSupported)]

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, vídeo y audio con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




