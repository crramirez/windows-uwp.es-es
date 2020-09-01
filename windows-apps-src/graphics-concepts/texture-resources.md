---
title: Recursos de texturas
description: Obtenga información sobre la representación con recursos de texturas de Direct3D y cómo admitir varias combinaciones de texturas mediante fases de textura.
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- Recursos de texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d82a5525601c98812d6aab97f5f5d4399ceddc91
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156189"
---
# <a name="texture-resources"></a>Recursos de texturas


Las texturas son un tipo de recurso que se usa para la representación.

## <a name="span-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>Representar con recursos de textura


Direct3D admite varias combinaciones de texturas mediante el concepto de fases de textura. Cada fase de textura contiene una textura y las operaciones que se pueden realizar en la textura. Las texturas de las fases de textura forman el conjunto de texturas actuales. Consulte [combinación de texturas](texture-blending.md). El estado de cada textura se encapsula en su fase de textura.

La aplicación también puede establecer los Estados de la perspectiva de textura y del filtrado de textura. Consulte [filtrado de textura](texture-filtering.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




