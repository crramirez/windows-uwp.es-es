---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: "En este artículo se indican los perfiles Dynamic Adaptive Streaming over HTTP (DASH) compatibles con las aplicaciones para UWP."
title: Compatibilidad con los perfiles Dynamic Adaptive Streaming over HTTP (DASH)
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9ca8f2d1783a73ae38fed1d3ee1e58b809ddd2aa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Compatibilidad con los perfiles Dynamic Adaptive Streaming over HTTP (DASH)
En la tabla siguiente se enumeran los perfiles DASH que se admiten en las aplicaciones para UWP.



|Etiqueta | Tipo de manifiesto | Notas|Versión de julio de Windows 10|Windows 10, versión 1511|Windows 10, versión 1607 |Windows 10, versión 1607 |Windows 10, versión 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg:dash:profile:isoff-live:2011 | Estático |     |Se admite            |  Se admite              | Se admite        |Se admite| Se admite|
|urn:mpeg:dash:profile:isoff-main:2011 |        | Mejor esfuerzo | Se admite            |  Se admite              | Se admite        |Se admite| Se admite|
|urn:mpeg:dash:profile:isoff-live:2011 | Dinámico | $Time$ se admite, pero $Number$ no se admite en las plantillas de segmento | No se admite            | No se admite              | No se admite        |No se admite| Se admite|


## <a name="related-topics"></a>Temas relacionados

* [Reproducción de contenido multimedia](media-playback.md)
* [Streaming adaptable](adaptive-streaming.md)
 

 




