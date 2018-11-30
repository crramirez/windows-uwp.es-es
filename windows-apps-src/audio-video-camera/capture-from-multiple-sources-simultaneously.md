---
ms.assetid: ''
description: En este artículo se muestra cómo capturar vídeo desde varios orígenes a la vez en un solo archivo con varias pistas de vídeo incrustadas.
title: Captura desde varios orígenes con MediaFrameSourceGroup
ms.date: 09/12/2017
ms.topic: article
keywords: windows 10, uwp, captura, vídeo
ms.localizationpriority: medium
ms.openlocfilehash: a654739490043b9f821e7906fa8cf9e3e7259fed
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8325578"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Captura desde varios orígenes con MediaFrameSourceGroup

En este artículo se muestra cómo capturar vídeo desde varios orígenes a la vez en un solo archivo con varias pistas de vídeo incrustadas. A partir de RS3, puedes especificar varios objetos **[VideoStreamDescriptor](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor)** para un solo objeto **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**. Esto permite codificar varias secuencias a la vez en un solo archivo. Las secuencias de vídeo codificadas en esta operación deben incluirse en un solo **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** que especifique un conjunto de cámaras del dispositivo actual que se puedan usar al mismo tiempo. 

Para obtener información sobre el uso de **[MediaFrameSourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** con la clase **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** para permitir escenarios de visión del equipo en tiempo real que usen varias cámaras, consulta [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md).

El resto de este artículo explica los pasos para grabar vídeo desde dos cámaras de color en un solo archivo con varias pistas de vídeo.

## <a name="find-available-sensor-groups"></a>Buscar grupos de sensores disponibles
Un **MediaFrameSourceGroup** representa una colección de orígenes de fotogramas, normalmente cámaras, a las que se puede acceder a la vez. El conjunto de grupos de orígenes de fotogramas disponible es diferente para cada dispositivo, por lo que es el primer paso de este ejemplo es obtener la lista de grupos de orígenes de fotogramas disponibles y encontrar uno que contenga las cámaras necesarias para el escenario, lo que en este caso requiere dos cámaras de color.

El método **[MediaFrameSourceGroup.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** devuelve todos los grupos de orígenes disponibles en el dispositivo actual. Cada **MediaFrameSourceGroup** tiene una lista de objetos **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** que describe cada origen de fotogramas del grupo. Se usa una consulta Linq para encontrar un grupo de orígenes que contenga dos cámaras de color, una en el panel frontal y otra en la parte posterior. Se devuelve un objeto anónimo que contiene el **MediaFrameSourceGroup** y el **MediaFrameSourceInfo** seleccionados para cada cámara de color. En lugar de usar la sintaxis de Linq, podrías realizar un bucle en cada grupo y luego en cada **MediaFrameSourceInfo** para buscar un grupo que cumpla los requisitos.

Ten en cuenta que no todos los dispositivos contendrá un grupo de orígenes que contenga dos cámaras de color, por lo que debes asegurarte de que se ha encontrado un grupo de orígenes antes de intentar la captura de vídeo.

[!code-cs[MultiRecordFindSensorGroups](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordFindSensorGroups)]

## <a name="initialize-the-mediacapture-object"></a>Inicializar el objeto MediaCapture
La clase **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** es la clase principal que se usa para la mayoría de las operaciones de captura de audio, vídeo y fotos en las aplicaciones para UWP. Para inicializar este objeto, llama a **[InitializeAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.InitializeAsync)** y pasa un objeto **[MediaCaptureInitializationSettings](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** que contenga los parámetros de inicialización. En este ejemplo, el único valor especificado es la propiedad **[SourceGroup](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)**, que se establece en el **MediaFrameSourceGroup** que se ha recuperado en el ejemplo de código anterior.

Para obtener información sobre otras operaciones que se pueden realizar con **MediaCapture** y otras características de las aplicaciones para UWP para capturar contenido multimedia, consulta [Cámara](camera.md).

[!code-cs[MultiRecordInitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordInitMediaCapture)]

## <a name="create-a-mediaencodingprofile"></a>Crear un MediaEncodingProfile
La clase **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** indica al proceso de captura de contenido multimedia cómo se deben codificar el audio y el vídeo a medida que se escriben en un archivo. En los escenarios típicos de captura y transcodificación, esta clase proporciona un conjunto de métodos estáticos para crear perfiles comunes, como **[CreateAvi](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** y **[CreateMp3](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**. Para este ejemplo, se crea manualmente un perfil de codificación mediante un contenedor Mpeg4 y una codificación de vídeo H264. La configuración de la codificación de vídeo se especifica con un objeto **[VideoEncodingProperties](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties)**. Para cada cámara de color que se usa en este escenario, se configura un objeto **VideoStreamDescriptor**. El descriptor se construye con el objeto **VideoEncodingProperties** que especifica la codificación. La propiedad **[Label](https://docs.microsoft.com/uwp/api/windows.media.core.videostreamdescriptor.Label)** de **VideoStreamDescriptor** debe establecerse en el identificador del origen de los fotogramas multimedia que se va a capturar en la secuencia. Así es como el proceso de captura sabe qué descriptor de la secuencia y qué propiedades de codificación deben usarse para cada cámara. El identificador del origen de los fotogramas lo exponen los objetos **[MediaFrameSourceInfo](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** que se han encontrado en la sección anterior, cuando se ha seleccionado un **MediaFrameSourceGroup**.


A partir de Windows 10, version 1709, puedes establecer varias propiedades de codificación en un **MediaEncodingProfile** llamando a **[SetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)**. Puedes recuperar la lista de descriptores de secuencias de vídeo mediante una llamada a **[GetVideoTracks](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)**. Ten en cuenta que si estableces la propiedad **[Video](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)**, que almacena un descriptor de secuencia individual, la lista de descriptores establecida mediante la llamada a **SetVideoTracks** se sustituirá por una lista que contendrá el descriptor único especificado.


[!code-cs[MultiRecordMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordMediaEncodingProfile)]

### <a name="encode-timed-metadata-in-media-files"></a>Codificar metadatos temporizados en archivos multimedia

A partir de Windows 10, versión 1803, además de audio y vídeo, puede codificar metadatos temporizados en un archivo multimedia para el que se admite el formato de datos. Por ejemplo, los metadatos de GoPro (gpmd) pueden guardarse en archivos MP4 para transmitir la ubicación geográfica correlacionada con una secuencia de vídeo. 

La codificación de metadatos usa un patrón que es paralelo a la codificación de audio o vídeo. La clase [**TimedMetadataEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) describe el tipo, el subtipo y las propiedades de codificación de los metadatos, como **VideoEncodingProperties** lo hace para el vídeo. El [**TimedMetadataStreamDescriptor**](https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) identifica una secuencia de metadatos, al igual que lo hace el **VideoStreamDescriptor** para secuencias de vídeo.  

El siguiente ejemplo muestra cómo inicializar un objeto **TimedMetadataStreamDescriptor**. Primero, se crea un objeto **TimedMetadataEncodingProperties** y el **Subtipo** se establece en un GUID que identifica el tipo de metadatos que se incluirá en la secuencia. Este ejemplo usa el GUID para metadatos GoPro (gpmd). Se llama al método [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) para establecer datos específicos de formato. Para los archivos MP4, los datos específicos del formato se almacenan en el cuadro SampleDescription (stsd). Luego, se crea un nuevo **TimedMetadataStreamDescriptor** a partir de las propiedades de codificación. Las propiedades **Label** y **Name** se establecen para identificar la secuencia que se va a codificar. 

[!code-cs[GetStreamDescriptor](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetStreamDescriptor)]

Llama a [MediaEncodingProfile.SetTimedMetadataTracks](**https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks**) para agregar el descriptor de la secuencia de metadatos en el perfil de codificación. En el siguiente ejemplo se muestra un método auxiliar que toma dos descriptores de secuencia de vídeo, un descriptor de secuencia de audio y un descriptor de secuencia de metadatos temporizado, y devuelve un **MediaEncodingProfile** que se puede usar para codificar las secuencias.

[!code-cs[GetMediaEncodingProfile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetGetMediaEncodingProfile)]

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Grabación mediante MediaEncodingProfile multisecuencia
El último paso de este ejemplo es iniciar la captura de vídeo mediante una llamada a **[StartRecordToStorageFileAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)**, pasando el **StorageFile** en el que se escribirá el contenido multimedia capturado y el **MediaEncodingProfile** creado en el ejemplo de código anterior. Tras esperar unos segundos, la grabación se detiene con una llamada a **[StopRecordAsync](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)**.

[!code-cs[MultiRecordToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs#SnippetMultiRecordToFile)]

Una vez completada la operación, se habrá creado un archivo de vídeo que contendrá el vídeo capturado desde cada cámara codificado como una secuencia independiente dentro del archivo. Para obtener información sobre la reproducción de archivos multimedia que contengan varias pistas de vídeo, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md)


 

 




