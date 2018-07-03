---
author: drewbatgit
ms.assetid: 3E0FBB43-F6A4-4558-AA89-20E7760BA73F
description: En este artículo se indican los perfiles Dynamic Adaptive Streaming over HTTP (DASH) compatibles con las aplicaciones para UWP.
title: Compatibilidad con los perfiles Dynamic Adaptive Streaming over HTTP (DASH)
ms.author: drewbat
ms.date: 02/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c411d479430f793d85863f66c64758155b8a5758
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842342"
---
# <a name="dynamic-adaptive-streaming-over-http-dash-profile-support"></a>Compatibilidad con los perfiles Dynamic Adaptive Streaming over HTTP (DASH)


## <a name="supported-dash-profiles"></a>Perfiles DASH compatibles
En la tabla siguiente se enumeran los perfiles DASH que se admiten en las aplicaciones para UWP.

|Etiqueta | Tipo de manifiesto | Notas|Versión de julio de Windows 10|Windows 10, versión 1511|Windows 10, versión 1607 |Windows 10, versión 1607 |Windows 10, versión 1703|
|----------------|------|-------|-----------|--------------|---------|-------|--------|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Estático |     |Se admite            |  Se admite              | Se admite        |Se admite| Compatible|
|urn:mpeg&#58;dash:profile:isoff-main:2011 |        | Mejor esfuerzo | Se admite            |  Se admite              | Se admite        |Se admite| Compatible|
|urn:mpeg&#58;dash:profile:isoff-live:2011 | Dinámico | $Time$ se admite, pero $Number$ no se admite en las plantillas de segmento | No se admite            | No se admite              | No se admite        |No se admite| Compatible|


## <a name="unsupported-dash-profiles"></a>Perfiles DASH no compatibles
Los perfiles que no figuran en la tabla anterior no se admiten, incluidos, sin limitaciones, a los perfiles siguientes:

* urn:mpeg&#58;dash:profile:full:2011
* urn:mpeg&#58;dash:profile:isoff-on-demand:2011
* urn:mpeg&#58;dash:profile:mp2t-main:2011
* urn:mpeg&#58;dash:profile:mp2t-simple:2011


## <a name="related-topics"></a>Temas relacionados

* [Reproducción de contenido multimedia](media-playback.md)
* [Streaming adaptable](adaptive-streaming.md)
 

 




