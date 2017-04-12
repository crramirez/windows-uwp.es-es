---
author: drewbatgit
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: "Consulta de codificadores y descodificadores de audio y vídeo instalados en un dispositivo."
title: "Consulta de códecs instalados"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, codec, encoder, decoder, query, códec, codificador, descodificador, consulta"
ms.openlocfilehash: 1bde2c24db6faa843d9118f330867e7f66b22e7e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="query-for-installed-codecs"></a>Consulta de códecs instalados
En este artículo se muestra cómo consultar los codificadores y descodificadores de audio y vídeo instalados en un dispositivo. En el artículo [Códecs admitidos](supported-codecs.md) se indican los códecs que se incluyen en el sistema operativo para cada familia de dispositivos. En un dispositivo individual, el usuario o una aplicación puede instalar códecs adicionales. La clase [CodecQuery](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery) se puede usar para enumerar el conjunto actual de codificadores y descodificadores instalados en el dispositivo en el que se ejecuta tu aplicación.

La clase **CodecQuery** se incluye en el espacio de nombres [windows.media.core](https://docs.microsoft.com/en-us/uwp/api/windows.media.core) y proporciona un constructor que no acepta argumentos.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

El método [FindAllAsync](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery#Windows_Media_Core_CodecQuery_FindAllAsync_Windows_Media_Core_CodecKind_Windows_Media_Core_CodecCategory_System_String_) de la clase **CodecQuery** devuelve todos los códecs instalados que satisfacen los parámetros especificados. Estos parámetros incluyen un valor de la enumeración [CodecKind](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeckind) que especifica si se deben devolver códecs de audio o vídeo y un valor [CodecCategory](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeccategory) que especifica si se deben devolver códecs de audio o vídeo.

Un parámetro final es una cadena que representa un subtipo multimedia, tal como vídeo H264 o audio FLAC, que todo códec devuelto del método **FindAllAsync** debe admitir. Si quieres que se devuelvan códecs para todos los subtipos multimedia, puedes especificar una cadena nula o vacía para este parámetro. En el siguiente ejemplo se indican todos los codificadores de vídeo instalados en el dispositivo actual.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

Si especificas un subtipo multimedia, debes usar la representación de cadena de uno de los GUID de subtipo que figuran en [GUID de subtipo de audio](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) o [GUID de subtipo de vídeo](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx). La clase [CodecSubtypes](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecsubtypes) proporciona propiedades para la mayoría de los subtipos multimedia admitidos que devuelven la representación de cadena del GUID de subtipo. También puedes especificar un código FOURCC para el parámetro de subtipo multimedia. Para más información, consulta [Códigos FOURCC](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx). 

En el siguiente ejemplo se usa el código FOURCC para consultar los descodificadores de vídeo que admiten vídeo H.264. Un escenario de ejemplo para esta operación es comprobar si un formato de vídeo se admite antes de intentar reproducir un archivo de vídeo.

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

En el siguiente ejemplo se hace una consulta de los codificadores de audio que admiten el subtipo multimedia FLAC. Luego, en el ejemplo se crea un objeto AudioEncodinAudioEncodingProperties y MediaEncodingProfile para el subtipo multimedia. Esto se puede usar para grabar audio en el formato especificado o transcodificar el audio a partir de otro formato. Para más información, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md) y [Transcodificar archivos multimedia](transcode-media-files.md).

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Temas relacionados

* [Códecs admitidos](supported-codecs.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcodificar archivos multimedia](transcode-media-files.md)
 

 




