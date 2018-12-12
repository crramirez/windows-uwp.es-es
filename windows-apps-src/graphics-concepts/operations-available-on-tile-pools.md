---
title: Operaciones disponibles en grupos de iconos
description: Las operaciones en grupos de iconos incluyen cambiar el tamaño de un grupo de iconos, ofrecer recursos (ceder memoria temporalmente al sistema para todo el grupo de iconos) y recuperar recursos.
ms.assetid: 90347F7F-C991-4287-BD70-494533ECDC8A
keywords:
- Operaciones disponibles en grupos de iconos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b8b0c6f4fa578e4ec483492b320dc9bc346ab66
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929741"
---
# <a name="operations-available-on-tile-pools"></a>Operaciones disponibles en grupos de iconos


Las operaciones en grupos de iconos incluyen cambiar el tamaño de un grupo de iconos, ofrecer recursos (ceder memoria temporalmente al sistema para todo el grupo de iconos) y recuperar recursos.

-   La duración de los grupos de iconos funciona como cualquier otro recurso de Direct3D, respaldado por recuento de referencias, incluido el seguimiento de asignaciones de recursos de streaming en este caso. El sistema operativo anulará la asignación del grupo de iconos cuando la aplicación ya no haga referencia a un grupo de iconos, desaparezcan las asignaciones de iconos en la memoria y se completen los accesos a la unidad de procesamiento de gráficos (GPU).
-   Las API relacionadas con sincronizar y compartir la superficie funcionan para grupos de iconos (pero no directamente en recursos de streaming). Similar al comportamiento de grupos de iconos ofrecidos, los comandos de Direct3D que obtienen acceso a los recursos de streaming que apuntan a un grupo de iconos se anulan si se compartió el grupo de iconos y otro dispositivo y proceso lo adquiere actualmente.
-   Cambiar el tamaño de un grupo de iconos.
-   Ofrecer y reclamar recursos: estas operaciones para ceder memoria al sistema temporalmente operan en todo el grupo de iconos (y no están disponibles para los recursos de streaming individuales). Si un recurso de streaming apunta a un icono en un grupo de iconos ofrecido, el recurso de streaming se comporta como si fuera ofrecido (por ejemplo, el tiempo de ejecución anula los comandos que le hacen referencia).

No se pueden copiar datos directamente al grupo de iconos o desde él. Los accesos a la memoria se realizan siempre con recursos de streaming.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Creación de recursos de streaming](creating-streaming-resources.md)

 

 




