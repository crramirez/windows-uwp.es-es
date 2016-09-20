---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: "Puedes usar las API Windows.Media.Transcoding para transcodificar archivos de vídeo de un formato a otro."
title: Transcodificar archivos multimedia
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 06c452291f10acd35dde9659c08a386ea38fa90a

---

# Transcodificar archivos multimedia

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Puedes usar las API [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) para transcodificar archivos de vídeo de un formato a otro.


            La *transcodificación* es la conversión de un archivo multimedia digital, como un archivo de vídeo o audio, de un formato a otro. Para ello, generalmente, se descodifica el archivo y después se lo vuelve a codificar. Por ejemplo, puedes convertir un archivo de Windows Media a MP4 para que pueda reproducirse en un dispositivo portátil compatible con este formato. O bien, puedes convertir un archivo de vídeo de alta definición a una resolución más baja. En ese caso, el archivo que se volvió a codificar podría usar el mismo códec que el archivo original, pero tendría un perfil de codificación diferente.

## Configurar el proyecto de la transcodificación

Además de los espacios de nombres que hacen referencia a la plantilla de proyecto de manera predeterminada, debes hacer referencia a estos espacios de nombres para transcodificar los archivos multimedia con el código de este artículo.

[!code-cs[Uso](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## Seleccionar los archivos de origen y de destino

La forma en que la aplicación determina los archivos de origen y destino de la transcodificación depende de la implementación. En este ejemplo se usa una clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) y una clase [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir al usuario seleccionar un origen y un archivo de destino.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## Crear un perfil de codificación de multimedia

El perfil de codificación contiene todas las opciones de configuración que determinan el modo en que se codificará el archivo de destino. Se trata del lugar donde tienes el mayor número de opciones al transcodificar un archivo.

La clase [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) proporciona métodos estáticos para crear perfiles de codificación predefinidos:

-   Wav
-   Audio AAC (M4A)
-   Audio MP3
-   Audio de Windows Media (WMA)
-   Avi
-   Vídeo MP4 (vídeo H.264 más audio AAC)
-   Vídeo de Windows Media (WMV)

Los cuatro primeros perfiles de esta lista contienen solo audio. Los otros tres contienen vídeo y audio.

Con el siguiente código se crea un perfil para vídeo MP4.

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

El método [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) estático crea un perfil de codificación MP4. El parámetro de este método proporciona la resolución de destino para el vídeo. En este caso, [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) significa 1280 x 720 píxeles a 30 fotogramas por segundo. ("720p" significa 720 líneas de escaneo progresivo por fotograma). Todos los demás métodos para crear perfiles predefinidos siguen este modelo.

Como alternativa, puedes crear un perfil que coincida con un archivo multimedia existente mediante el método [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047). O bien, si conoces la configuración de codificación exacta que deseas, puedes crear un nuevo objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) y rellenar los detalles del perfil.

## Transcodificar el archivo

Para transcodificar el archivo, crea un nuevo objeto [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) y llama al método [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936). Pasa el archivo de origen, el archivo de destino y el perfil de codificación. Después llama al método [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) en el objeto [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) que devolvió la operación de transcodificación asincrónica.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## Responder al progreso de la transcodificación

Puedes registrar eventos para responder cuando el progreso de la transcodificación asincrónica [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) cambie. Estos eventos forman parte del marco de programación asincrónica para aplicaciones para la Plataforma universal de Windows (UWP) y no son específicos para la API de transcodificación.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 







<!--HONumber=Jun16_HO4-->


