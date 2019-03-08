---
title: Cambio del tamaño de los grupos de iconos
description: Cambia el tamaño de un grupo de iconos para aumentar un grupo de iconos si la aplicación necesita más espacio de trabajo para los recursos de streaming asignados. También puedes reducirlo si se necesita menos espacio.
ms.assetid: A54A06DC-BDDB-42DC-85E8-C64241100ED5
keywords:
- Cambio del tamaño de los grupos de iconos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e08447c575e99178e503e99eb651cd5e225a898
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607780"
---
# <a name="tile-pool-resizing"></a>Cambio del tamaño de los grupos de iconos


Cambia el tamaño de un grupo de iconos para aumentar un grupo de iconos si la aplicación necesita más espacio de trabajo para los recursos de streaming asignados. También puedes reducirlo si se necesita menos espacio. Otra opción para las aplicaciones es asignar grupos de mosaicos adicionales para nuevos recursos de streaming. Pero si un recurso de streaming individual necesita más espacio del que estaba disponible inicialmente en su grupo de mosaicos, aumentar su tamaño es una buena opción. Un recurso de streaming no puede tener asignaciones en varios grupos de mosaicos al mismo tiempo.

Cuando se aumenta el tamaño de un grupo de mosaicos, el controlador de pantalla agrega mosaicos adicionales al final mediante una o varias asignaciones nuevas. Este desglose en asignaciones no es visible para la aplicación. La memoria existente en el grupo de mosaicos permanece intacta, al igual que las asignaciones de recursos de streaming en esa memoria.

Cuando se reduce el tamaño de un grupo de mosaicos, se quitan mosaicos del final. Se quitan iconos incluso por debajo del tamaño de la asignación inicial, hasta 0, lo que significa que no se pueden realizar asignaciones por encima del nuevo tamaño. Sin embargo, las asignaciones existentes que superen el final del nuevo tamaño permanecen intactas y utilizables. El controlador de pantalla conservará la memoria mientras permanezcan las asignaciones con cualquier parte de las asignaciones que usa el controlador para la memoria del grupo de mosaicos. Si tras la reducción de tamaño una parte de la memoria se ha mantenido porque existen asignaciones de mosaicos que apuntan a ella y vuelve a aumentarse el grupo de mosaicos (en cualquier cantidad), primero se reutiliza la memoria antes de que se produzca cualquier asignación adicional para cubrir el tamaño de la operación de aumento.

Con el fin de ahorrar memoria, una aplicación no solo debe reducir el tamaño de un grupo de mosaicos, sino que también debe quitar/reasignar las asociaciones existentes que superen el final del nuevo tamaño del grupo de mosaicos, que es menor.

El hecho de reducir el tamaño (y quitar asignaciones) no necesariamente produce ahorros de memoria inmediatos. La liberación de memoria depende de lo granulares que sean las asignaciones subyacente del controlador de pantalla para todo el grupo de mosaicos. Si la reducción de tamaño es suficiente para que no haya que utilizar una asignación del controlador de pantalla, dicho controlador de pantalla puede liberarla. Si se ha aumentado un grupo de mosaicos, reducirlo a tamaños anteriores (y quitar o volver a asignar las asignaciones de mosaicos que correspondan) es más probable que se produzcan ahorros de memoria, aunque no está garantizado en el caso de que los tamaños no se correspondan exactamente con los tamaños de asignación subyacente elegidos por el controlador de pantalla.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Las asignaciones son en un grupo de icono](mappings-are-into-a-tile-pool.md)

 

 




