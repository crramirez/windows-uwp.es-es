---
title: Recursos de texturas
description: "Las texturas son un tipo de recurso usado para la representación."
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords: Recursos de texturas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: c82079666f2ee32a4e858414c824142d2bcff932
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="texture-resources"></a>Recursos de texturas


Las texturas son un tipo de recurso usado para la representación.

## <a name="span-idrenderingwithtextureresourcesspanspan-idrenderingwithtextureresourcesspanspan-idrenderingwithtextureresourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>Representación mediante recursos de textura


Direct3D admite la fusión de texturas múltiples mediante el concepto de fases de textura. Cada fase de textura contiene una textura y operaciones que pueden realizarse en dicha textura. Las texturas de las fases de textura forman el conjunto de texturas actuales. Consulta [Combinación de texturas](texture-blending.md). El estado de cada textura se encapsula en su fase de textura.

La aplicación también puede establecer los estados de la perspectiva de la textura y el filtrado de texturas. Consulta [Filtrado de texturas](texture-filtering.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 



