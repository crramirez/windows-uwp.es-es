---
title: Conceptos básicos de texturas
description: Las primeras imágenes 3D generadas por PC, si bien en general eran avanzadas para su época, solían tener una apariencia de plástico brillante.
ms.assetid: 3CA3905D-E837-48EB-A81F-319AA1C6537E
keywords:
- Conceptos básicos de texturas
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c48b7b007c1af1eaf6013d5f6e99468713d50745
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5881678"
---
# <a name="basic-texturing-concepts"></a>Conceptos básicos de texturas


Las primeras imágenes en 3D generadas por PC, aunque en general eran avanzadas para su época, solían tener una apariencia plástica y brillante. Carecían de los tipos de marcas (como rayas, fisuras, huellas digitales y manchas) que dan a los objetos 3D una complejidad visual realista. Las texturas se han vuelvo populares gracias a que mejoran el realismo de las imágenes 3D generadas por PC.

En su uso cotidiano, la palabra "textura" hace referencia a la suavidad o rugosidad de un objeto. Sin embargo, en los gráficos para PC, una textura es un mapa de bits de colores de píxeles que dan a un objeto la apariencia de textura.

Dado que las texturas de Direct3D son mapas de bits, puede aplicarse cualquier mapa de bits a un primitivo de Direct3D. Por ejemplo, las aplicaciones pueden crear y manipular objetos que parezcan tener una veta de madera. Hierba, suciedad y rocas pueden aplicarse a un conjunto de primitivos 3D que forman una colina. El resultado es un ladera de aspecto realista. También puedes usar las texturas para crear efectos, como señales a lo largo de una carretera, estratos de roca en un acantilado o la apariencia de mármol en un piso.

Además, Direct3D admite técnicas más avanzadas de texturas, como la combinación de texturas (con o sin transparencia) y los mapas de luz. Consulta [Combinación de texturas](texture-blending.md) y [Mapas de luz con texturas](light-mapping-with-textures.md).

Si tu aplicación crea un dispositivo de capa de abstracción de hardware (HAL) o un dispositivo de software, puede usar texturas de 8, 24, 16, 32, 64 o 128bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




