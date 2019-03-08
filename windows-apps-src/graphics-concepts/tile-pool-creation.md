---
title: Creación de grupos de iconos
description: Las aplicaciones pueden crear uno o varios grupos de iconos por dispositivo de Direct3D. El tamaño total de cada grupo de mosaico está restringido al límite de tamaño de recurso de Direct3D 11, que es aproximadamente 1/4 de la unidad de procesamiento gráfico (GPU) de RAM.
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- Creación de grupos de iconos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ce3824ab2d435b42df9586a6c229b68db10a0c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612810"
---
# <a name="tile-pool-creation"></a>Creación de grupos de iconos


Las aplicaciones pueden crear uno o varios grupos de iconos por dispositivo de Direct3D. El tamaño total de cada grupo de mosaico está restringido al límite de tamaño de recurso de Direct3D 11, que es aproximadamente 1/4 de la unidad de procesamiento gráfico (GPU) de RAM.

Un grupo de mosaicos está compuesto por mosaicos de 64 KB, pero el sistema operativo (el controlador de pantalla) administra todo el grupo como una o varias asignaciones entre bastidores: el desglose no es visible para las aplicaciones. Para definir el contenido, los recursos de streaming apuntan a los mosaicos dentro de un grupo de mosaicos. Para desasignar un mosaicos desde un recurso de streaming, se selecciona dicho mosaico como **NULL**. Estos iconos sin asignar tienen reglas sobre el comportamiento de las lecturas o escrituras; consulta [Seguimiento de peligros frente a recursos de grupos de iconos](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Las asignaciones son en un grupo de icono](mappings-are-into-a-tile-pool.md)

 

 




