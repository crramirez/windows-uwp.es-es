---
ms.assetid: ''
description: En este artículo se muestra cómo capturar vídeo de varios orígenes simulataneously en un único archivo con varias pistas de vídeo insertadas.
title: Captura desde varios orígenes con MediaFrameSourceGroup
ms.date: 09/12/2017
ms.topic: article
keywords: Windows 10, UWP, captura, vídeo
ms.localizationpriority: medium
ms.openlocfilehash: 6d40e75dd88b84eb5d7244a2ad3ed3d605c17e0b
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362879"
---
# <a name="capture-from-multiple-sources-using-mediaframesourcegroup"></a>Captura desde varios orígenes con MediaFrameSourceGroup

En este artículo se muestra cómo capturar vídeo de varios orígenes simultáneamente en un solo archivo con varias pistas de vídeo insertadas. A partir de RS3, puede especificar varios objetos **[VideoStreamDescriptor](/uwp/api/windows.media.core.videostreamdescriptor)** para un único **[MediaEncodingProfile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)**. Esto le permite codificar varias secuencias simultáneamente en un único archivo. Las secuencias de vídeo que se codifican en esta operación deben incluirse en una única **[MediaFrameSourceGroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** que especifique un conjunto de cámaras en el dispositivo actual que se pueden usar al mismo tiempo. 

Para obtener información sobre el uso de **[MediaFrameSourceGroup](/uwp/api/windows.media.capture.frames.mediaframesourcegroup)** con la clase **[MediaFrameReader](/uwp/api/windows.media.capture.frames.mediaframereader)** para habilitar escenarios de visión de equipos en tiempo real que usan varias cámaras, consulte [procesamiento de fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md).

El resto de este artículo le guiará a través de los pasos para grabar vídeo de dos cámaras de color a un solo archivo con varias pistas de vídeo.

## <a name="find-available-sensor-groups"></a>Buscar grupos de sensores disponibles
Un **MediaFrameSourceGroup** representa una colección de orígenes de fotogramas, normalmente cámaras, a la que se puede tener acceso simulataneously. El conjunto de grupos de orígenes de fotogramas disponibles es diferente para cada dispositivo, por lo que el primer paso de este ejemplo es obtener la lista de grupos de orígenes de fotogramas disponibles y encontrar uno que contenga las cámaras necesarias para el escenario, que en este caso requiere dos cámaras de color.

El método **[MediaFrameSourceGroup. FindAllAsync](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.FindAllAsync)** devuelve todos los grupos de origen disponibles en el dispositivo actual. Cada **MediaFrameSourceGroup** devuelto tiene una lista de objetos **[MediaFrameSourceInfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** que describe cada origen de fotogramas del grupo. Una consulta LINQ se usa para buscar un grupo de origen que contenga dos cámaras de color, una en el panel frontal y otra en la parte trasera. Se devuelve un objeto anónimo que contiene el **MediaFrameSourceGroup** seleccionado y el **MediaFrameSourceInfo** para cada cámara de color. En lugar de usar la sintaxis de LINQ, puede recorrer en su lugar cada grupo y, a continuación, cada **MediaFrameSourceInfo** para buscar un grupo que cumpla sus requisitos.

Tenga en cuenta que no todos los dispositivos contendrán un grupo de origen que contenga dos cámaras de color, por lo que debe comprobar para asegurarse de que se ha encontrado un grupo de origen antes de intentar capturar el vídeo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordFindSensorGroups":::

## <a name="initialize-the-mediacapture-object"></a>Inicializar el objeto MediaCapture
La clase **[MediaCapture](/uwp/api/windows.media.capture.mediacapture)** es la clase principal que se usa para la mayoría de las operaciones de captura de audio, vídeo y foto en aplicaciones UWP. Inicialice el objeto llamando a **[InitializeAsync](/uwp/api/windows.media.capture.mediacapture.InitializeAsync)** y pasando un objeto **[MediaCaptureInitializationSettings](/uwp/api/windows.media.capture.mediacaptureinitializationsettings)** que contenga los parámetros de inicialización. En este ejemplo, la única configuración especificada es la propiedad **[SourceGroup](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.SourceGroup)** , que se establece en el valor de **MediaFrameSourceGroup** que se recuperó en el ejemplo de código anterior.

Para obtener información sobre otras operaciones que puede realizar con **MediaCapture** y otras características de aplicaciones para UWP para capturar medios, consulte [cámara](camera.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordInitMediaCapture":::

## <a name="create-a-mediaencodingprofile"></a>Creación de un MediaEncodingProfile
La clase **[MediaEncodingProfile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)** indica a la canalización de captura multimedia cómo se debe codificar el audio y el vídeo capturados a medida que se escriben en un archivo. En los escenarios de captura y transcodificación típicos, esta clase proporciona un conjunto de métodos estáticos para crear perfiles comunes, como **[CreateAvi](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi)** y **[CreateMp3](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)**. En este ejemplo, se crea un perfil de codificación de forma manual mediante un contenedor MPEG4 y una codificación de vídeo H264. La configuración de la codificación de vídeo se especifica mediante un objeto **[VideoEncodingProperties](/uwp/api/windows.media.mediaproperties.videoencodingproperties)** . Para cada cámara de color utilizada en este escenario, se configura un objeto **VideoStreamDescriptor** . El descriptor se construye con el objeto **VideoEncodingProperties** que especifica la codificación. La propiedad **[Label](/uwp/api/windows.media.core.videostreamdescriptor.Label)** de **VideoStreamDescriptor** debe establecerse en el identificador del origen del marco multimedia que se capturará en la secuencia. Así es como la canalización de captura sabe qué propiedades de descriptor de secuencia y de codificación deben usarse para cada cámara. Los objetos **[MediaFrameSourceInfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)** que se encontraron en la sección anterior exponen el identificador del origen del marco, cuando se selecciona un **MediaFrameSourceGroup** .


A partir de Windows 10, versión 1709, puede establecer varias propiedades de codificación en un **MediaEncodingProfile** llamando a **[SetVideoTracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.setvideotracks)**. Puede recuperar la lista de descriptores de flujo de vídeo llamando a **[GetVideoTracks](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.GetVideoTracks)**. Tenga en cuenta que si establece la propiedad **[vídeo](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.Video)** , que almacena un único descriptor de secuencia, la lista de descriptores que se establece mediante una llamada a **SetVideoTracks** se reemplazará por una lista que contenga el único descriptor especificado.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordMediaEncodingProfile":::

### <a name="encode-timed-metadata-in-media-files"></a>Codificar metadatos con tiempo en archivos multimedia

A partir de Windows 10, versión 1803, además de audio y vídeo, puede codificar los metadatos con tiempo en un archivo multimedia para el que se admite el formato de datos. Por ejemplo, los metadatos de GoPro (GPMD) se pueden almacenar en archivos MP4 para transmitir la ubicación geográfica correlacionada con una secuencia de vídeo. 

La codificación de metadatos usa un patrón que es paralelo a la codificación de audio o vídeo. La clase [**TimedMetadataEncodingProperties**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties) describe las propiedades type, SubType y Encoding de los metadatos, como **VideoEncodingProperties** , para vídeo. [**TimedMetadataStreamDescriptor**](/uwp/api/windows.media.core.timedmetadatastreamdescriptor) identifica una secuencia de metadatos, igual que **VideoStreamDescriptor** , para las secuencias de vídeo.  

En el ejemplo siguiente se muestra cómo inicializar un objeto **TimedMetadataStreamDescriptor** . En primer lugar, se crea un objeto **TimedMetadataEncodingProperties** y el **subtipo** se establece en un GUID que identifica el tipo de metadatos que se incluirán en la secuencia. En este ejemplo se usa el GUID para metadatos de GoPro (GPMD). Se llama al método [**SetFormatUserData**](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) para establecer datos específicos de formato. En el caso de los archivos MP4, los datos específicos del formato se almacenan en el cuadro SampleDescription (stsd). A continuación, se crea un nuevo **TimedMetadataStreamDescriptor** a partir de las propiedades de codificación. Las propiedades **etiqueta** y **nombre** se establecen para identificar la secuencia que se va a codificar. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetGetStreamDescriptor":::

Llame a [**MediaEncodingProfile. SetTimedMetadataTracks**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks) para agregar el descriptor de flujo de metadatos al perfil de codificación. En el ejemplo siguiente se muestra un método auxiliar que toma dos descriptores de flujo de vídeo, un descriptor de flujo de audio y un descriptor de flujo de metadatos con tiempo, y devuelve un **MediaEncodingProfile** que se puede usar para codificar las secuencias.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetGetMediaEncodingProfile":::

## <a name="record-using-the-multi-stream-mediaencodingprofile"></a>Registro mediante el MediaEncodingProfile de varios flujos
El último paso de este ejemplo es iniciar la captura de vídeo llamando a **[StartRecordToStorageFileAsync](/uwp/api/windows.media.capture.mediacapture.startrecordtostoragefileasync)**, pasando el elemento **StorageFile** en el que se escriben los medios capturados y **MediaEncodingProfile** creado en el ejemplo de código anterior. Después de esperar unos segundos, la grabación se detiene con una llamada a **[StopRecordAsync](/uwp/api/windows.media.capture.mediacapture.StopRecordAsync)**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.MultiRecord.xaml.cs" id="SnippetMultiRecordToFile":::

Una vez completada la operación, se creará un archivo de vídeo que contendrá el vídeo capturado de cada cámara codificada como una secuencia independiente dentro del archivo. Para obtener información sobre la reproducción de archivos multimedia que contienen varias pistas de vídeo, consulte [elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md)


 

 
