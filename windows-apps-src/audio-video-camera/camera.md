---
ms.assetid: 370f2c14-4f1e-47b3-9197-24205ab255a3
description: Este artículo muestra las características de cámara que están disponibles para aplicaciones para UWP, así como los vínculos a los artículos de procedimientos que muestran cómo usarlos.
title: Cámara
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e28a4a7abaacd8b2de60c6163055bd9d667ba412
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8946916"
---
# <a name="camera"></a>Cámara

Esta sección proporciona instrucciones para crear aplicaciones para la Plataforma universal de Windows (UWP) que usen la cámara o el micrófono para capturar fotos, vídeos o audio.

## <a name="use-the-windows-built-in-camera-ui"></a>Usar la interfaz de usuario de la cámara integrada de Windows

| Tema | Descripción |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturar fotografías y vídeos con la cámara de interfaz de usuario integrada de Windows](capture-photos-and-video-with-cameracaptureui.md) | Muestra cómo usar la clase [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI) para capturar fotos o vídeos con la interfaz de usuario de la cámara integrada en Windows. Si, simplemente, quieres permitir que el usuario capture una foto o vídeo y devuelva el resultado a la aplicación, esta es la forma más rápida y sencilla de hacerlo.  |

## <a name="basic-mediacapture-tasks"></a>Tareas básicas de MediaCapture

| Tema | Descripción |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Mostrar la vista previa de la cámara](simple-camera-preview-access.md) | Describe cómo mostrar rápidamente la secuencia de vista previa de cámara en una página XAML en una aplicación para UWP. |
| [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) | Muestra la forma más sencilla para capturar fotos y vídeo mediante la clase [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). La clase **MediaCapture** expone un eficaz conjunto de API que proporciona control de bajo nivel sobre la canalización de captura y habilita escenarios de captura avanzados, pero este artículo está pensado para ayudar a agregar la captura multimedia básica a la aplicación de forma rápida y fácil. |
| [Características de la interfaz de usuario de la cámara para dispositivos móviles](camera-ui-features-for-mobile-devices.md) | Muestra cómo sacar provecho de las características especiales de la interfaz de usuario de la cámara que solo están presentes en los dispositivos móviles.  |
                                                                                                               
## <a name="advanced-mediacapture-tasks"></a>Tareas de MediaCapture avanzadas   
                                                                                                               
| Tema                                                                                             | Descripción                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Controlar la orientación de pantalla y del dispositivo con MediaCapture](handle-device-orientation-with-mediacapture.md) | Muestra cómo controlar la orientación del dispositivo al capturar fotos y vídeos con una clase auxiliar. | 
| [Descubrir y seleccionar las funcionalidades de cámara con los perfiles de cámara](camera-profiles.md) | Muestra cómo usar perfiles de cámara para detectar y administrar las funcionalidades de los diferentes dispositivos de captura de vídeo. Esto incluye tareas como la selección de perfiles que admiten resoluciones específicas o velocidades de fotogramas, perfiles que admiten el acceso simultáneo con varias cámaras y perfiles que admiten HDR. |
| [Establecer el formato, la resolución y la velocidad de fotogramas para MediaCapture](set-media-encoding-properties.md) | Muestra cómo usar la interfaz [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) para establecer la resolución y la velocidad de fotogramas de la secuencia de vista previa de la cámara, así como de las fotos y los vídeos capturados. También se muestra cómo asegurarse de que la relación de aspecto de la secuencia de vista previa coincida con la de la secuencia multimedia capturada. |
| [Captura de fotos HDR y con poca luz](high-dynamic-range-hdr-photo-capture.md) | Muestra cómo usar la clase [**AdvancedPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture) para capturar fotos de alto rango dinámico (HDR) y con poca luz. |
| [Controles manuales de la cámara para la captura de fotos y vídeos](capture-device-controls-for-photo-and-video-capture.md) | Muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de fotos y vídeo mejorados, como la estabilización de imagen óptica y el zoom suave. |
| [Controles manuales de la cámara para la captura de vídeos.](capture-device-controls-for-video-capture.md) | Muestra cómo usar los controles de dispositivo manuales para permitir escenarios de captura de vídeo mejorados, como vídeo HDR y prioridad de exposición.  |
| [Efecto de estabilización de vídeo para la captura de vídeos](effects-for-video-capture.md) | Muestra cómo usar el efecto de estabilización de vídeo.  |
| [Análisis de la escena para MediaCapture](scene-analysis-for-media-capture.md) | Muestra cómo usar las clases [**SceneAnalysisEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.SceneAnalysisEffect) y [**FaceDetectionEffect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.FaceDetectionEffect) para analizar el contenido de la secuencia de vista previa de captura de multimedia.  |
| [Capturar una secuencia de fotos con VariablePhotoSequence](variable-photo-sequence.md) | Muestra cómo capturar una secuencia de fotos variable, que permite capturar varios fotogramas de imágenes en sucesión rápida y configurarlos individualmente que usen una configuración de foco, flash, ISO, exposición y compensación de la exposición diferente.  |
| [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md) | Muestra cómo usar [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) con [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) para obtener fotogramas multimedia de uno o más orígenes disponibles, lo que incluye cámaras a color, de profundidad y de infrarrojos, dispositivos de audio o incluso orígenes de fotogramas personalizados, como los que producen fotogramas de seguimiento estructurales. Esta característica se diseñó para que la usen las aplicaciones que realizan procesamiento en tiempo real de fotogramas multimedia, como las aplicaciones de realidad aumentada y las de cámara con reconocimiento de profundidad.  |
| [Obtener un marco de vista previa](get-a-preview-frame.md) | Muestra cómo obtener un marco de vista previa único de la secuencia de vista previa de captura multimedia.  |                                                                                                   


## <a name="uwp-app-samples-for-camera"></a>Muestras de aplicaciones para UWP para la cámara

* [Muestra de detección de rostro de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=619486&clcid=0x409)
* [Muestra de marco de vista previa de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=620516&clcid=0x409)
* [Muestra de HDR de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=620517&clcid=0x409)
* [Muestra de controles manuales de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=627611&clcid=0x409)
* [Muestra de perfil de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=620518&clcid=0x409)
* [Muestra de resolución de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=624252&clcid=0x409)
* [Kit de inicio de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=619479&clcid=0x409)
* [Muestra de estabilización de vídeo de la cámara](http://go.microsoft.com/fwlink/p/?LinkID=620519&clcid=0x409)

## <a name="related-topics"></a>Temas relacionados

* [Audio, vídeo y cámara](index.md)
 

 




