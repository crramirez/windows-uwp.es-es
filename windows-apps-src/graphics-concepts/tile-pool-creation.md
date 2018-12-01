---
title: Creación de grupos de mosaicos
description: Las aplicaciones pueden crear uno o varios grupos de iconos por dispositivo de Direct3D. El tamaño total de cada grupo de iconos está restringido al límite de tamaño de recursos del Direct3D11, que es aproximadamente 1/4 de la unidad de procesamiento gráfico (GPU) RAM.
ms.assetid: BD51EDD3-4AD3-4733-B014-DD77B9D743BB
keywords:
- Creación de grupos de mosaicos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5ce3824ab2d435b42df9586a6c229b68db10a0c9
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335751"
---
# <a name="tile-pool-creation"></a>Creación de grupos de mosaicos


Las aplicaciones pueden crear uno o varios grupos de iconos por dispositivo de Direct3D. El tamaño total de cada grupo de iconos está restringido al límite de tamaño de recursos del Direct3D11, que es aproximadamente 1/4 de la unidad de procesamiento gráfico (GPU) RAM.

Un grupo de mosaicos está compuesto por mosaicos de 64KB, pero el sistema operativo (el controlador de pantalla) administra todo el grupo como una o varias asignaciones entre bastidores: el desglose no es visible para las aplicaciones. Para definir el contenido, los recursos de streaming apuntan a los mosaicos dentro de un grupo de mosaicos. Para desasignar un mosaicos desde un recurso de streaming, se selecciona dicho mosaico como **NULL**. Estos iconos sin asignar tienen reglas sobre el comportamiento de las lecturas o escrituras; consulta [Seguimiento de peligros frente a recursos de grupos de iconos](hazard-tracking-versus-tile-pool-resources.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Asignaciones en un grupo de mosaicos](mappings-are-into-a-tile-pool.md)

 

 




