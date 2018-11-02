---
author: drewbatgit
ms.assetid: 66a9cfe2-b212-4c73-8a36-963c33270099
description: En este artículo se enumeran las etiquetas del protocolo HTTP Live Streaming (HLS) que se admiten en las aplicaciones para UWP.
title: Compatibilidad con etiquetas HTTP Live Streaming (HLS)
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6d8e90f98dd79150cf19727fe31e51278a88a198
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5976606"
---
# <a name="http-live-streaming-hls-tag-support"></a>Compatibilidad con etiquetas HTTP Live Streaming (HLS)
En la tabla siguiente se enumeran las etiquetas HLS que se admiten en las aplicaciones para UWP.

> [!NOTE] 
> Se puede acceder a las etiquetas personalizadas que comienzan por "X-" como metadatos temporizados, como se describe en el artículo [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

|Etiqueta |Introducida en la versión del protocolo HLS|Versión de borrador del documento del protocolo HLS|Obligatoria en el cliente|Versión de julio de Windows 10|Windows 10, versión 1511|Windows 10, versión 1607 |
|---------------------|-----------|--------------|---------|--------------|-----|-----|
|4.3.1.  Etiquetas básicas                 |             |                   |         |             |     |    |
| 4.3.1.1.  EXTM3U |1|0|OBLIGATORIA|Se admite|Se admite|Se admite|
| 4.3.1.2.  EXT-X-VERSION |2|3|OBLIGATORIA|Se admite|Se admite|Se admite
|4.3.2.  Etiquetas de segmento multimedia                 |             |                   |         |             |     |    | 
| 4.3.2.1.  EXTINF  |1|0|OBLIGATORIA|Se admite|Se admite|Se admite
| 4.3.2.2.  EXT-X-BYTERANGE |4|7|OPCIONAL|Se admite|Se admite|Se admite|
| 4.3.2.3.  EXT-X-DISCONTINUITY |1|2|OPCIONAL|Se admite|Se admite|Se admite|
| 4.3.2.4.  EXT-X-KEY |1|0|OPCIONAL|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp; METHOD|1|0|Atributo|"NONE, AES-128"|"NONE, AES-128"|"NONE, AES-128, SAMPLE-AES"|
|&nbsp;&nbsp;&nbsp; URI|1|0|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp; IV|2|3|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp; KEYFORMAT|5|9|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp; KEYFORMATVERSIONS|5|9|Atributo|No se admite|No se admite|No se admite|
| 4.3.2.5.  EXT-X-MAP |5|9|OPCIONAL|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp; URI|5|9|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp; BYTERANGE|5|9|Atributo|No se admite|No se admite|No se admite|
| 4.3.2.6.  EXT-X-PROGRAM-DATE-TIME |1|0|OPCIONAL|No se admite|No se admite|No se admite|
|4.3.3.  Etiquetas de lista de reproducción de multimedia                 |             |                   |         |             |     |    | 
| 4.3.3.1.  EXT-X-TARGETDURATION  |1|0|OBLIGATORIA|Se admite|Se admite|Se admite|
| 4.3.3.2.  EXT-X-MEDIA-SEQUENCE  |1|0|OPCIONAL|Se admite|Se admite|Se admite|
| 4.3.3.3.  EXT-X-DISCONTINUITY-SEQUENCE|6|12|OPCIONAL|No se admite|No se admite|No se admite|
| 4.3.3.4.  EXT-X-ENDLIST |1|0|OPCIONAL|Se admite|Se admite|Se admite|
| 4.3.3.5.  EXT-X-PLAYLIST-TYPE |3|6|OPCIONAL|Se admite|Se admite|Se admite|
| 4.3.3.6.  EXT-X-I-FRAMES-ONLY |4|7|OPCIONAL|No se admite|No se admite|No se admite|
|4.3.4.  Etiquetas de lista de reproducción maestra                 |             |                   |         |             |     |    |
| 4.3.4.1.  EXT-X-MEDIA |4|7|OPCIONAL|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  TYPE|4|7|Atributo|"AUDIO, VIDEO"|"AUDIO, VIDEO"|"AUDIO, VIDEO, SUBTITLES"|
|&nbsp;&nbsp;&nbsp;  URI|4|7|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  GROUP-ID|4|7|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  LANGUAGE|4|7|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  ASSOC-LANGUAGE|6|13|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp;  NAME|4|7|Atributo|No se admite|No se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  DEFAULT|4|7|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp;  AUTOSELECT|4|7|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp;  FORCED|5|9|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp;  INSTREAM-ID|6|12|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp;  CHARACTERISTICS|5|9|Atributo|No se admite|No se admite|No se admite|
| 4.3.4.2.  EXT-X-STREAM-INF  |1|0|OBLIGATORIA|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  BANDWIDTH|1|0|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  PROGRAM-ID|1|0|Atributo|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AVERAGE-BANDWIDTH|7|14|Atributo|No se admite|No se admite|No se admite|
|&nbsp;&nbsp;&nbsp;  CODECS|1|0|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  RESOLUTION|2|3|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  FRAME-RATE|7|15|Atributo|NA|NA|NA|
|&nbsp;&nbsp;&nbsp;  AUDIO|4|7|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  VIDEO|4|7|Atributo|Se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  SUBTITLES|5|9|Atributo|No se admite|No se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  CLOSED-CAPTIONS|6|12|Atributo|No se admite|No se admite|No se admite|
| 4.3.4.3.  EXT-X-I-FRAME-STREAM-INF  |4|7|OPCIONAL|No se admite|No se admite|No se admite|
| 4.3.4.4.  EXT-X-SESSION-DATA  |7|14|OPCIONAL|No se admite|No se admite|No se admite|
| 4.3.4.5.  EXT-X-SESSION-KEY |7|17|OPCIONAL|No se admite|No se admite|No se admite|
|4.3.5.  Etiquetas de la lista de reproducción maestra o multimedia                  |             |                   |         |             |     |    |
| 4.3.5.1.  EXT-X-INDEPENDENT-SEGMENTS |6|13|OPCIONAL|No se admite|Se admite|Se admite|
| 4.3.5.2.  EXT-X-START  |6|12|OPCIONAL|No se admite|Se admite parcialmente|Se admite parcialmente|
|&nbsp;&nbsp;&nbsp;  TIME-OFFSET|6|12|Atributo|No se admite|Se admite|Se admite|
|&nbsp;&nbsp;&nbsp;  PRECISE|6|12|Atributo|No se admite|Se admite "NO" predeterminado|Se admite "NO" predeterminado|



## <a name="related-topics"></a>Temas relacionados

* [Reproducción de contenido multimedia](media-playback.md)
* [Streaming adaptable](adaptive-streaming.md)
 

 




