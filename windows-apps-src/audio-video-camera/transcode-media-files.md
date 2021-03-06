---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Puedes usar las API Windows.Media.Transcoding para transcodificar archivos de vídeo de un formato a otro.
title: Transcodificar archivos multimedia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 28ed15ab49f3ec33e382d28c14ffd43bb5c8779d
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363748"
---
# <a name="transcode-media-files"></a>Transcodificar archivos multimedia



Puedes usar las API [**Windows.Media.Transcoding**](/uwp/api/Windows.Media.Transcoding) para transcodificar archivos de vídeo de un formato a otro.

La *transcodificación* es la conversión de un archivo multimedia digital, como un archivo de vídeo o audio, de un formato a otro. Para ello, generalmente, se descodifica el archivo y después se lo vuelve a codificar. Por ejemplo, puedes convertir un archivo de Windows Media a MP4 para que pueda reproducirse en un dispositivo portátil compatible con este formato. O bien, puedes convertir un archivo de vídeo de alta definición a una resolución más baja. En ese caso, el archivo que se volvió a codificar podría usar el mismo códec que el archivo original, pero tendría un perfil de codificación diferente.

## <a name="set-up-your-project-for-transcoding"></a>Configurar el proyecto de la transcodificación

Además de los espacios de nombres que hacen referencia a la plantilla de proyecto de manera predeterminada, debes hacer referencia a estos espacios de nombres para transcodificar los archivos multimedia con el código de este artículo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetUsing":::

## <a name="select-source-and-destination-files"></a>Seleccionar los archivos de origen y de destino

La forma en que la aplicación determina los archivos de origen y destino de la transcodificación depende de la implementación. En este ejemplo se usa una clase [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) y una clase [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) para permitir al usuario seleccionar un origen y un archivo de destino.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeGetFile":::

## <a name="create-a-media-encoding-profile"></a>Crear un perfil de codificación de multimedia

El perfil de codificación contiene todas las opciones de configuración que determinan el modo en que se codificará el archivo de destino. Se trata del lugar donde tienes el mayor número de opciones al transcodificar un archivo.

La clase [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) proporciona métodos estáticos para crear perfiles de codificación predefinidos:

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Métodos para crear perfiles de codificación solo de audio

Método  |Perfil  |
---------|---------|
[**CreateAlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Audio de códec de audio sin pérdida de Apple (ALAC)         |
[**CreateFlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Audio de códec de audio sin pérdida de servicio (FLAC).         |
[**CreateM4a**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |Audio AAC (M4A)         |
[**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |Audio MP3         |
[**CreateWav**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |Audio WAV         |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Audio de Windows Media (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Métodos para crear perfiles de codificación de audio/vídeo

Método  |Perfil  |
---------|---------|
[**CreateAvi**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |Vídeo de codificación de vídeo de alta eficacia (HEVC), también conocido como vídeo H. 265 |
[**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |Vídeo MP4 (vídeo H.264 más audio AAC) |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Vídeo de Windows Media (WMV) |


Con el siguiente código se crea un perfil para vídeo MP4.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeMediaProfile":::

El método [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) estático crea un perfil de codificación MP4. El parámetro de este método proporciona la resolución de destino para el vídeo. En este caso, [**VideoEncodingQuality.hd720p**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality) significa 1280 x 720 píxeles a 30 fotogramas por segundo. ("720p" significa 720 líneas de escaneo progresivo por fotograma). Todos los demás métodos para crear perfiles predefinidos siguen este modelo.

Como alternativa, puedes crear un perfil que coincida con un archivo multimedia existente mediante el método [**MediaEncodingProfile.CreateFromFileAsync**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync). O bien, si conoces la configuración de codificación exacta que deseas, puedes crear un nuevo objeto [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) y rellenar los detalles del perfil.

## <a name="transcode-the-file"></a>Transcodificar el archivo

Para transcodificar el archivo, crea un nuevo objeto [**MediaTranscoder**](/uwp/api/Windows.Media.Transcoding.MediaTranscoder) y llama al método [**MediaTranscoder.PrepareFileTranscodeAsync**](/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync). Pasa el archivo de origen, el archivo de destino y el perfil de codificación. Después llama al método [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) en el objeto [**PrepareTranscodeResult**](/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) que devolvió la operación de transcodificación asincrónica.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeTranscodeFile":::

## <a name="respond-to-transcoding-progress"></a>Responder al progreso de la transcodificación

Puedes registrar eventos para responder cuando el progreso de la transcodificación asincrónica [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) cambie. Estos eventos forman parte del marco de programación asincrónica para aplicaciones para la Plataforma universal de Windows (UWP) y no son específicos para la API de transcodificación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetTranscodeCallbacks":::
