---
title: Recursos de texturas
description: Las texturas son un tipo de recurso usado para la representación.
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- Recursos de texturas
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 72f58521e01d46437ba44453b94d12a82bb3e639
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5599181"
---
# <a name="texture-resources"></a>Recursos de texturas


Las texturas son un tipo de recurso usado para la representación.

## <a name="span-idrenderingwithtextureresourcesspanspan-idrenderingwithtextureresourcesspanspan-idrenderingwithtextureresourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>Representación mediante recursos de textura


Direct3D admite la fusión de texturas múltiples mediante el concepto de fases de textura. Cada fase de textura contiene una textura y operaciones que pueden realizarse en dicha textura. Las texturas de las fases de textura forman el conjunto de texturas actuales. Consulta [Combinación de texturas](texture-blending.md). El estado de cada textura se encapsula en su fase de textura.

La aplicación también puede establecer los estados de la perspectiva de la textura y el filtrado de texturas. Consulta [Filtrado de texturas](texture-filtering.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




