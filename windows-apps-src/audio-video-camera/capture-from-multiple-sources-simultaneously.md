---
author: drewbatgit
ms.assetid: 
description: "En este artículo se muestra cómo capturar vídeo desde varios orígenes a la vez en un solo archivo con varias pistas de vídeo incrustadas."
title: "Captura desde varios orígenes con MediaFrameSourceGroup"
ms.author: drewbat
ms.date: 09/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, captura, vídeo"
localizationpriority: medium
ms.openlocfilehash: c6097c6b4fd7439101086f1c0bcf7564bacc63f5
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2017
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Captura desde varios orígenes con MediaFrameSourceGroup

En este artículo se muestra cómo capturar vídeo desde varios orígenes a la vez en un solo archivo con varias pistas de vídeo incrustadas. A partir de RS3, puedes especificar varios objetos **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** para un solo objeto **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**. Esto permite codificar varias secuencias a la vez en un solo archivo. Las secuencias de vídeo codificadas en esta operación deben incluirse en un solo **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** que especifique un conjunto de cámaras del dispositivo actual que se puedan usar al mismo tiempo. 

Para obtener información sobre el uso de **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** con la clase **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** para permitir escenarios de visión del equipo en tiempo real que usen varias cámaras, consulta [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md).

El resto de este artículo explica los pasos para grabar vídeo desde dos cámaras de color en un solo archivo con varias pistas de vídeo.

## <a name="find-available-sensor-groups"></a>Buscar grupos de sensores disponibles
Un **MediaFrameSourceGroup** representa una colección de orígenes de fotogramas, normalmente cámaras, a las que se puede acceder a la vez. El conjunto de grupos de orígenes de fotogramas disponible es diferente para cada dispositivo, por lo que es el primer paso de este ejemplo es obtener la lista de grupos de orígenes de fotogramas disponibles y encontrar uno que contenga las cámaras necesarias para el escenario, lo que en este caso requiere dos cámaras de color.

El método **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup#Windows_Media_Capture_Frames_MediaFrameSourceGroup_FindAllAsync)** devuelve todos los grupos de orígenes disponibles en el dispositivo actual. Cada **MediaFrameSourceGroup** tiene una lista de objetos **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** que describe cada origen de fotogramas del grupo. Se usa una consulta Linq para encontrar un grupo de orígenes que contenga dos cámaras de color, una en el panel frontal y otra en la parte posterior. Se devuelve un objeto anónimo que contiene el **MediaFrameSourceGroup** y el **MediaFrameSourceInfo** seleccionados para cada cámara de color. En lugar de usar la sintaxis de Linq, podrías realizar un bucle en cada grupo y luego en cada **MediaFrameSourceInfo** para buscar un grupo que cumpla los requisitos.

Ten en cuenta que no todos los dispositivos contendrá un grupo de orígenes que contenga dos cámaras de color, por lo que debes asegurarte de que se ha encontrado un grupo de orígenes antes de intentar la captura de vídeo.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>Inicializar el objeto MediaCapture
La clase **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** es la clase principal que se usa para la mayoría de las operaciones de captura de audio, vídeo y fotos en las aplicaciones para UWP. Para inicializar este objeto, llama a **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_InitializeAsync)** y pasa un objeto **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** que contenga los parámetros de inicialización. En este ejemplo, el único valor especificado es la propiedad **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings#Windows_Media_Capture_MediaCaptureInitializationSettings_SourceGroup)**, que se establece en el **MediaFrameSourceGroup** que se ha recuperado en el ejemplo de código anterior.

Para obtener información sobre otras operaciones que se pueden realizar con **MediaCapture** y otras características de las aplicaciones para UWP para capturar contenido multimedia, consulta [Cámara](camera.md).

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>Crear un MediaEncodingProfile
La clase **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** indica al proceso de captura de contenido multimedia cómo se deben codificar el audio y el vídeo a medida que se escriben en un archivo. En los escenarios típicos de captura y transcodificación, esta clase proporciona un conjunto de métodos estáticos para crear perfiles comunes, como **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAvi_Windows_Media_MediaProperties_VideoEncodingQuality_)** y **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp3_Windows_Media_MediaProperties_AudioEncodingQuality_)**. Para este ejemplo, se crea manualmente un perfil de codificación mediante un contenedor Mpeg4 y una codificación de vídeo H264. La configuración de la codificación de vídeo se especifica con un objeto **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)**. Para cada cámara de color que se usa en este escenario, se configura un objeto **VideoStreamDescriptor**. El descriptor se construye con el objeto **VideoEncodingProperties** que especifica la codificación. La propiedad **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor#Windows_Media_Core_VideoStreamDescriptor_Label)** de **VideoStreamDescriptor** debe establecerse en el identificador del origen de los fotogramas multimedia que se va a capturar en la secuencia. Así es como el proceso de captura sabe qué descriptor de la secuencia y qué propiedades de codificación deben usarse para cada cámara. El identificador del origen de los fotogramas lo exponen los objetos **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** que se han encontrado en la sección anterior, cuando se ha seleccionado un **MediaFrameSourceGroup**.


A partir de RS3, puedes establecer varias propiedades de codificación en un **MediaEncodingProfile** llamando a **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_SetVideoTracks_Windows_Foundation_Collections_IIterable_Windows_Media_Core_VideoStreamDescriptor__)**. Puedes recuperar la lista de descriptores de secuencias de vídeo mediante una llamada a **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_GetVideoTracks)**. Ten en cuenta que si estableces la propiedad **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile#Windows_Media_MediaProperties_MediaEncodingProfile_Video)**, que almacena un descriptor de secuencia individual, la lista de descriptores establecida mediante la llamada a **SetVideoTracks** se sustituirá por una lista que contendrá el descriptor único especificado.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Grabación mediante MediaEncodingProfile multisecuencia
El último paso de este ejemplo es iniciar la captura de vídeo mediante una llamada a **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_)**, pasando el **StorageFile** en el que se escribirá el contenido multimedia capturado y el **MediaEncodingProfile** creado en el ejemplo de código anterior. Tras esperar unos segundos, la grabación se detiene con una llamada a **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture#Windows_Media_Capture_MediaCapture_StopRecordAsync)**.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

Una vez completada la operación, se habrá creado un archivo de vídeo que contendrá el vídeo capturado desde cada cámara codificado como una secuencia independiente dentro del archivo. Para obtener información sobre la reproducción de archivos multimedia que contengan varias pistas de vídeo, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md)


 

 




