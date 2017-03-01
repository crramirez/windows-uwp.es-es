---
title: "Creación de grupos de mosaicos"
description: "Las aplicaciones pueden crear uno o varios grupos de mosaicos por cada dispositivo Direct3D. El tamaño total de cada grupo de mosaicos está restringido al límite del tamaño de recursos de Direct3D 11, que es aproximadamente 1/4 de la RAM de la unidad de procesamiento gráfico (GPU)."
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- "Creación de grupos de mosaicos"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 5289bcb57572ede45c6853b0077f5baa82af7ca2
ms.lasthandoff: 02/07/2017

---

# <a name="tile-pool-creation"></a>Creación de grupos de mosaicos


Las aplicaciones pueden crear uno o varios grupos de mosaicos por cada dispositivo Direct3D. El tamaño total de cada grupo de mosaicos está restringido al límite del tamaño de recursos de Direct3D 11, que es aproximadamente 1/4 de la RAM de la unidad de procesamiento gráfico (GPU).

Un grupo de mosaicos está compuesto por mosaicos de 64 KB, pero el sistema operativo (el controlador de pantalla) administra todo el grupo como una o varias asignaciones entre bastidores: el desglose no es visible para las aplicaciones. Para definir el contenido, los recursos de streaming apuntan a los mosaicos dentro de un grupo de mosaicos. Para desasignar un mosaicos desde un recurso de streaming, se selecciona dicho mosaico como **NULL**. Estos iconos sin asignar tienen reglas sobre el comportamiento de las lecturas o escrituras; consulta [Seguimiento de peligros frente a recursos de grupos de iconos](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Asignaciones en un grupo de mosaicos](mappings-are-into-a-tile-pool.md)

 

 





