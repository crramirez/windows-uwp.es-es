---
author: drewbatgit
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: Consulta de codificadores y descodificadores de audio y vídeo instalados en un dispositivo.
title: Consulta de códecs instalados
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, codec, encoder, decoder, query, códec, codificador, descodificador, consulta
ms.localizationpriority: medium
ms.openlocfilehash: b74ac269bcba6d15e7c4f5dcb4c34d53573deb5e
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5829548"
---
# <a name="query-for-codecs-installed-on-a-device"></a>Consulta de los códecs instalados en un dispositivo
La clase **[CodecQuery](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery)** te permite consultar los códecs instalados en el dispositivo actual. La lista de códecs que se incluyen con Windows 10 para las distintas familias de dispositivos se enumeran en el artículo [Códecs admitidos](supported-codecs.md), pero dado que los usuarios y aplicaciones pueden instalar códecs adicionales en un dispositivo, es aconsejable consultar la compatibilidad de códec en tiempo de ejecución para determinar qué códecs están disponibles en el dispositivo actual.

La API de CodecQuery es un miembro del espacio de nombres **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)**, por lo que tendrás que incluir este espacio de nombres en la aplicación.

La API de CodecQuery es un miembro del espacio de nombres **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)**, por lo que tendrás que incluir este espacio de nombres en la aplicación.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

Inicializa una nueva instancia de la clase **CodecQuery** con una llamada al constructor.

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

El método **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery.findallasync)** devuelve todos los códecs instalados que coincidan con los parámetros proporcionados. Estos parámetros incluyen un valor **[CodecKind](https://docs.microsoft.com/uwp/api/windows.media.core.codeckind)** que especifica si buscas códecs de audio o vídeo o ambos, un valor **[CodecCategory](https://docs.microsoft.com/uwp/api/windows.media.core.codeccategory)** que especifica si vas buscas codificadores o descodificadores y una cadena que representa el subtipo de codificación multimedia para el que realizas la consulta, como vídeo H.264 o audio MP3.

Especifica una cadena vacía o nula para que el valor de subtipo devuelva los códecs de todos los subtipos. En el siguiente ejemplo se indican todos los codificadores de vídeo instalados en el dispositivo.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

La cadena de subtipo que pasas al objeto **FindAllAsync** puede ser una representación de cadena del GUID de subtipo definido por el sistema o un código FOURCC para el subtipo. El conjunto de GUID de subtipo de medios admitidos se enumeran en los artículos [Audio Subtype GUIDs (GUID de subtipo de audio)](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) y [Video Subtype GUIDs (GUID de subtipo de vídeo)](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx), pero la clase **[CodecSubtypes](https://docs.microsoft.com/uwp/api/windows.media.core.codecsubtypes)** proporciona propiedades que devuelven los valores GUID para cada subtipo admitido. Para más información sobre códigos FOURCC, consulta [FOURCC Codes](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx) (Códigos FOURCC). 

En el siguiente ejemplo se especifica el código FOURCC "H264" para determinar si hay un descodificador de vídeo H.264 instalado en el dispositivo. Puedes realizar esta consulta antes de intentar reproducir contenido de vídeo H.264. También puedes controlar códecs no admitidos en tiempo de reproducción. Para más información, consulta [Controlar errores desconocidos y códecs no admitidos al abrir elementos multimedia](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items).

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

En el siguiente ejemplo se realiza una consulta para determinar si un descodificador de audio FLAC está instalado en el dispositivo actual y, si es así, se crea un objeto **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** para el subtipo que puede usarse para capturar el audio en un archivo o para transcodificar el audio de otro formato a un archivo de audio FLAC.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Temas relacionados

* [Reproducción de contenido multimedia](media-playback.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcodificar archivos multimedia](transcode-media-files.md)
* [Códecs admitidos](supported-codecs.md)
 

 




