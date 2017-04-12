---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: "Puedes usar las API Windows.Media.Transcoding para transcodificar archivos de vídeo de un formato a otro."
title: Transcodificar archivos multimedia
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: c9e6a6437e427ca6b5bd063d467fd713526ea8ee
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="transcode-media-files"></a>Transcodificar archivos multimedia

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Puedes usar las API [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) para transcodificar archivos de vídeo de un formato a otro.

La *transcodificación* es la conversión de un archivo multimedia digital (como un archivo de vídeo o audio) de un formato a otro. Para ello, generalmente, se descodifica el archivo y después se lo vuelve a codificar. Por ejemplo, puedes convertir un archivo de Windows Media a MP4 para que pueda reproducirse en un dispositivo portátil compatible con este formato. O bien, puedes convertir un archivo de vídeo de alta definición a una resolución más baja. En ese caso, el archivo que se volvió a codificar podría usar el mismo códec que el archivo original, pero tendría un perfil de codificación diferente.

## <a name="set-up-your-project-for-transcoding"></a>Configurar el proyecto de la transcodificación

Además de los espacios de nombres que hacen referencia a la plantilla de proyecto de manera predeterminada, debes hacer referencia a estos espacios de nombres para transcodificar los archivos multimedia con el código de este artículo.

[!code-cs[Uso](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Seleccionar los archivos de origen y de destino

La forma en que la aplicación determina los archivos de origen y destino de la transcodificación depende de la implementación. En este ejemplo se usa una clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) y una clase [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir al usuario seleccionar un origen y un archivo de destino.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Crear un perfil de codificación de multimedia

El perfil de codificación contiene todas las opciones de configuración que determinan el modo en que se codificará el archivo de destino. Se trata del lugar donde tienes el mayor número de opciones al transcodificar un archivo.

La clase [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) proporciona métodos estáticos para crear perfiles de codificación predefinidos:

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Métodos para crear perfiles de codificación de solo audio

Método  |Perfil  |
---------|---------|
[CreateAlac](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAlac_Windows_Media_MediaProperties_AudioEncodingQuality_)     |Audio Apple Lossless Audio Codec (ALAC)         |
[CreateFlac](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateFlac_Windows_Media_MediaProperties_AudioEncodingQuality_)     |Audio Free Lossless Audio Codec (FLAC)         |
[CreateM4a](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateM4a_Windows_Media_MediaProperties_AudioEncodingQuality_)     |Audio AAC (M4A)         |
[CreateMp3](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp3_Windows_Media_MediaProperties_AudioEncodingQuality_)     |Audio MP3         |
[CreateWav](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateWav_Windows_Media_MediaProperties_AudioEncodingQuality_)     |Audio WAV         |
[CreateWmv](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateWmv_Windows_Media_MediaProperties_VideoEncodingQuality_)     |Audio de Windows Media (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Métodos para crear perfiles de codificación de audio y vídeo

Método  |Perfil  |
---------|---------|
[CreateAvi](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateAvi_Windows_Media_MediaProperties_VideoEncodingQuality_)     |AVI         |
[CreateHevc](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateHevc_Windows_Media_MediaProperties_VideoEncodingQuality_)     |Vídeo High Efficiency Video Coding (HEVC), también conocido como vídeo H.265         |
[CreateMp4](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateMp4_Windows_Media_MediaProperties_VideoEncodingQuality_)     |Vídeo MP4 (vídeo H.264 más audio AAC)         |

[CreateWmv](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile#Windows_Media_MediaProperties_MediaEncodingProfile_CreateWmv_Windows_Media_MediaProperties_VideoEncodingQuality_)     |Windows Media Video (WMV)         |


Con el siguiente código se crea un perfil para vídeo MP4.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

El método [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) estático crea un perfil de codificación MP4. El parámetro de este método proporciona la resolución de destino para el vídeo. En este caso, [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) significa 1280 x 720 píxeles a 30 fotogramas por segundo. ("720p" significa 720 líneas de escaneo progresivo por fotograma). Todos los demás métodos para crear perfiles predefinidos siguen este modelo.

Como alternativa, puedes crear un perfil que coincida con un archivo multimedia existente mediante el método [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047). O bien, si conoces la configuración de codificación exacta que deseas, puedes crear un nuevo objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) y rellenar los detalles del perfil.

## <a name="transcode-the-file"></a>Transcodificar el archivo

Para transcodificar el archivo, crea un nuevo objeto [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) y llama al método [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936). Pasa el archivo de origen, el archivo de destino y el perfil de codificación. Después llama al método [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) en el objeto [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) que devolvió la operación de transcodificación asincrónica.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Responder al progreso de la transcodificación

Puedes registrar eventos para responder cuando el progreso de la transcodificación asincrónica [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) cambie. Estos eventos forman parte del marco de programación asincrónica para aplicaciones para la Plataforma universal de Windows (UWP) y no son específicos para la API de transcodificación.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 




