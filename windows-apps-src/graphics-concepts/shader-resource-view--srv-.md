---
title: Vista de recursos del sombreador (SRV) y de acceso desordenado (UAV)
description: Las vistas de recurso de sombreador normalmente encapsulan texturas en un formato que permite a los sombreadores acceder a ellas. Una vista de acceso desordenada proporciona una funcionalidad similar, pero permite la lectura de la textura y la escritura en ella (o en cualquier otro recurso) en cualquier orden.
ms.assetid: 4505BCD2-0EDA-40F2-887C-EC081FE32E8F
keywords:
- Vista de recursos de sombreador (SRV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bd6cca08c0845e2da5420fc1f040cc285c87953d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636050"
---
# <a name="shader-resource-view-srv-and-unordered-access-view-uav"></a>Vista de recursos del sombreador (SRV) y de acceso desordenado (UAV)


Las vistas de recurso de sombreador normalmente encapsulan texturas en un formato que permite a los sombreadores acceder a ellas. Una vista de acceso desordenada proporciona una funcionalidad similar, pero permite la lectura de la textura y la escritura en ella (o en cualquier otro recurso) en cualquier orden.

Encapsular una única textura es probablemente la forma más sencilla de una vista de recursos de sombreador. Ejemplos más complejos serían una colección de subrecursos (matrices individuales, planos o colores de una textura con mapas MIP), texturas 3D, degradados de color de textura 1D, etc.

Las vistas de acceso desordenado son un poco más costosas en términos de rendimiento, pero permiten, por ejemplo, escribir una textura a la vez que se lee desde ella. Esto permite a la canalización de gráficos reutilizar la textura actualizada para otros propósitos. Las vistas de recurso de sombreador son para uso de solo lectura (que es el uso más común de los recursos).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




