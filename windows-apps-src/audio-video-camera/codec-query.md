---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: Consultar los codificadores y descodificadores de audio y vídeo instalados en un dispositivo.
title: Consulta de códecs instalados
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, códec, encoder, descodificador, consulta
ms.localizationpriority: medium
ms.openlocfilehash: ff1199651ceb9cf6ab3ef88cfdfa7dfbf25b67ee
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175739"
---
# <a name="query-for-codecs-installed-on-a-device"></a>Consultar los códecs instalados en un dispositivo
La clase **[CodecQuery](/uwp/api/windows.media.core.codecquery)** permite consultar los códecs instalados en el dispositivo actual. La lista de códecs que se incluyen con Windows 10 para diferentes familias de dispositivos se enumeran en el artículo [códecs admitidos](supported-codecs.md), pero como los usuarios y las aplicaciones pueden instalar códecs adicionales en un dispositivo, es posible que desee consultar la compatibilidad con códecs en tiempo de ejecución para determinar qué códecs están disponibles en el dispositivo actual.

La API de CodecQuery es un miembro del espacio de nombres **[Windows. Media. Core](/uwp/api/windows.media.core)** , por lo que tendrá que incluir este espacio de nombres en la aplicación.

La API de CodecQuery es un miembro del espacio de nombres **[Windows. Media. Core](/uwp/api/windows.media.core)** , por lo que tendrá que incluir este espacio de nombres en la aplicación.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

Inicialice una nueva instancia de la clase **CodecQuery** llamando al constructor.

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

El método **[FindAllAsync](/uwp/api/windows.media.core.codecquery.findallasync)** devuelve todos los códecs instalados que coinciden con los parámetros proporcionados. Estos parámetros incluyen un valor **[CodecKind](/uwp/api/windows.media.core.codeckind)** que especifica si está consultando códecs de audio o vídeo o ambos, un valor **[CodecCategory](/uwp/api/windows.media.core.codeccategory)** que especifica si está consultando los codificadores o descodificadores, y una cadena que representa el subtipo de codificación multimedia para el que está consultando, como vídeo H. 264 o audio MP3.

Especifique una cadena vacía o null para que el valor del subtipo devuelva códecs para todos los subtipos. En el ejemplo siguiente se enumeran todos los codificadores de vídeo instalados en el dispositivo.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

La cadena de subtipo que se pasa a **FindAllAsync** puede ser una representación de cadena del GUID del subtipo definido por el sistema o un código FourCC para el subtipo. El conjunto de GUID de subtipo de medios admitidos se enumera en los artículos GUID de subtipo de [audio](/windows/desktop/medfound/audio-subtype-guids) y GUID de subtipo de [vídeo](/windows/desktop/medfound/video-subtype-guids), pero la clase **[CodecSubtypes](/uwp/api/windows.media.core.codecsubtypes)** proporciona propiedades que devuelven los valores GUID para cada subtipo compatible. Para obtener más información sobre los códigos FOURCC, consulte [códigos FourCC](/windows/desktop/DirectShow/fourcc-codes) 

En el ejemplo siguiente se especifica el código FOURCC "H264" para determinar si hay un descodificador de vídeo H. 264 instalado en el dispositivo. Puede realizar esta consulta antes de intentar reproducir el contenido de vídeo H. 264. También puede administrar códecs no admitidos en el momento de la reproducción. Para obtener más información, consulte [administrar códecs no admitidos y errores desconocidos al abrir elementos multimedia](./media-playback-with-mediasource.md#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items).

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

En el ejemplo siguiente se realiza una consulta para determinar si un codificador de audio FLAC está instalado en el dispositivo actual y, en ese caso, se crea un **[MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** para el subtipo que se puede usar para capturar audio en un archivo o transcodificar el audio de otro formato en un archivo de audio FLAC.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Temas relacionados

* [Reproducción de multimedia](media-playback.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcodificar archivos multimedia](transcode-media-files.md)
* [Códecs admitidos](supported-codecs.md)
 

 