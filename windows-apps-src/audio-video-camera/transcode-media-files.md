---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Puedes usar las API Windows.Media.Transcoding para transcodificar archivos de vídeo de un formato a otro.
title: Transcodificar archivos multimedia
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1a6eb19ca5954b3ce71ecbaefe3339bee78f8717
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7836283"
---
# <a name="transcode-media-files"></a>Transcodificar archivos multimedia



Puedes usar las API [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) para transcodificar archivos de vídeo de un formato a otro.

La *transcodificación* es la conversión de un archivo multimedia digital (como un archivo de vídeo o audio) de un formato a otro. Para ello, generalmente, se descodifica el archivo y después se lo vuelve a codificar. Por ejemplo, puedes convertir un archivo de Windows Media a MP4 para que pueda reproducirse en un dispositivo portátil compatible con este formato. O bien, puedes convertir un archivo de vídeo de alta definición a una resolución más baja. En ese caso, el archivo que se volvió a codificar podría usar el mismo códec que el archivo original, pero tendría un perfil de codificación diferente.

## <a name="set-up-your-project-for-transcoding"></a>Configurar el proyecto de la transcodificación

Además de los espacios de nombres que hacen referencia a la plantilla de proyecto de manera predeterminada, debes hacer referencia a estos espacios de nombres para transcodificar los archivos multimedia con el código de este artículo.

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Selecciona los archivos de origen y de destino

La forma en que la aplicación determina los archivos de origen y destino de la transcodificación depende de la implementación. En este ejemplo se usa una clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) y una clase [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir al usuario seleccionar un origen y un archivo de destino.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Crear un perfil de codificación de multimedia

El perfil de codificación contiene todas las opciones de configuración que determinan el modo en que se codificará el archivo de destino. Se trata del lugar donde tienes el mayor número de opciones al transcodificar un archivo.

La clase [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) proporciona métodos estáticos para crear perfiles de codificación predefinidos:

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Métodos para crear perfiles de codificación de solo audio

Método  |Perfil  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Audio Apple Lossless Audio Codec (ALAC)         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Audio Free Lossless Audio Codec (FLAC)         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |Audio AAC (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |Audio MP3         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |Audio WAV         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Audio de Windows Media (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Métodos para crear perfiles de codificación de audio y vídeo

Método  |Perfil  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |Vídeo High Efficiency Video Coding (HEVC), también conocido como vídeo H.265 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |Vídeo MP4 (vídeo H.264 más audio AAC) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Vídeo de Windows Media (WMV) |


Con el siguiente código se crea un perfil para vídeo MP4.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

El método [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) estático crea un perfil de codificación MP4. El parámetro de este método proporciona la resolución de destino para el vídeo. En este caso, [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) significa 1280 x 720 píxeles a 30 fotogramas por segundo. ("720p" significa 720 líneas de escaneo progresivo por fotograma). Todos los demás métodos para crear perfiles predefinidos siguen este modelo.

Como alternativa, puedes crear un perfil que coincida con un archivo multimedia existente mediante el método [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047). O bien, si conoces la configuración de codificación exacta que deseas, puedes crear un nuevo objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) y rellenar los detalles del perfil.

## <a name="transcode-the-file"></a>Transcodificar el archivo

Para transcodificar el archivo, crea un nuevo objeto [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) y llama al método [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936). Pasa el archivo de origen, el archivo de destino y el perfil de codificación. Después llama al método [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) en el objeto [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) que devolvió la operación de transcodificación asincrónica.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Responder al progreso de la transcodificación

Puedes registrar eventos para responder cuando el progreso de la transcodificación asincrónica [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) cambie. Estos eventos forman parte del marco de programación asincrónica para aplicaciones para la Plataforma universal de Windows (UWP) y no son específicos para la API de transcodificación.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>Codificar una secuencia de metadatos
A partir de Windows 10, versión 1803, puedes incluir metadatos temporizados cuando archivos multimedia de transcodificación. A diferencia de los ejemplos de transcodificación vídeo anteriores, que usa lo métodos de creación del perfil, como [**MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), de codificación de multimedia integrado debe crear manualmente el perfil de codificación de metadatos para admitir el tipo de metadatos que se va a codificar .

Este primer paso para crear un perfil de incoding de metadatos es crear un objeto [**TimedMetadataEncodingProperties**] que describe la codificación de los metadatos que pueden transformar. La propiedad Subtype es un GUID que especifica el tipo de los metadatos. Los detalles de codificación para cada tipo de metadatos es propietarios y no se proporciona por Windows. En este ejemplo, se usa el GUID para metadatos GoPro (gprs). A continuación, se llama [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) para establecer un blob binario de datos que describen el formato de secuencia es específico para el formato de metadatos. A continuación, un **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) se crea a partir de las propiedades de codificación, y una etiqueta de pista y un nombre permitir que una aplicación que se lee de la secuencia de endcoded para identificar la secuencia de metadatos y, opcionalmente, mostrar el nombre de la secuencia en la interfaz de usuario. 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

Después de crear el **TimedMetadataStreamDescriptor**, puedes crear un **MediaEncodingProfile** que describe el vídeo, audio y metadatos codificación en el archivo. El **TimedMetadataStreamDescriptor** creado en el ejemplo anterior se pasa a esta función auxiliar de ejemplo y se agrega a la **MediaEncodingProfile** mediante una llamada a [**SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks).

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




