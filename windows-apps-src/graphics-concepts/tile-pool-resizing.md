---
title: Cambio del tamaño de los grupos de mosaicos
description: Cambia el tamaño de un grupo de mosaicos para aumentar su tamaño si la aplicación necesita un mayor espacio de trabajo para los recursos de streaming que se le asignan, o para reducirlo si se necesita menos espacio.
ms.assetid: A54A06DC-BDDB-42DC-85E8-C64241100ED5
keywords:
- Cambio del tamaño de los grupos de mosaicos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e676b28750375a353bb41ce8e14ec1d4c3371c4c
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5922074"
---
# <a name="tile-pool-resizing"></a>Cambio del tamaño de los grupos de mosaicos


Cambia el tamaño de un grupo de mosaicos para aumentar su tamaño si la aplicación necesita un mayor espacio de trabajo para los recursos de streaming que se le asignan, o para reducirlo si se necesita menos espacio. Otra opción para las aplicaciones es asignar grupos de mosaicos adicionales para nuevos recursos de streaming. Pero si un recurso de streaming individual necesita más espacio del que estaba disponible inicialmente en su grupo de mosaicos, aumentar su tamaño es una buena opción. Un recurso de streaming no puede tener asignaciones en varios grupos de mosaicos al mismo tiempo.

Cuando se aumenta el tamaño de un grupo de mosaicos, el controlador de pantalla agrega mosaicos adicionales al final mediante una o varias asignaciones nuevas. Este desglose en asignaciones no es visible para la aplicación. La memoria existente en el grupo de mosaicos permanece intacta, al igual que las asignaciones de recursos de streaming en esa memoria.

Cuando se reduce el tamaño de un grupo de mosaicos, se quitan mosaicos del final. Se quitan iconos incluso por debajo del tamaño de la asignación inicial, hasta 0, lo que significa que no se pueden realizar asignaciones por encima del nuevo tamaño. Sin embargo, las asignaciones existentes que superen el final del nuevo tamaño permanecen intactas y utilizables. El controlador de pantalla conservará la memoria mientras permanezcan las asignaciones con cualquier parte de las asignaciones que usa el controlador para la memoria del grupo de mosaicos. Si tras la reducción de tamaño una parte de la memoria se ha mantenido porque existen asignaciones de mosaicos que apuntan a ella y vuelve a aumentarse el grupo de mosaicos (en cualquier cantidad), primero se reutiliza la memoria antes de que se produzca cualquier asignación adicional para cubrir el tamaño de la operación de aumento.

Con el fin de ahorrar memoria, una aplicación no solo debe reducir el tamaño de un grupo de mosaicos, sino que también debe quitar/reasignar las asociaciones existentes que superen el final del nuevo tamaño del grupo de mosaicos, que es menor.

El hecho de reducir el tamaño (y quitar asignaciones) no necesariamente produce ahorros de memoria inmediatos. La liberación de memoria depende de lo granulares que sean las asignaciones subyacente del controlador de pantalla para todo el grupo de mosaicos. Si la reducción de tamaño es suficiente para que no haya que utilizar una asignación del controlador de pantalla, dicho controlador de pantalla puede liberarla. Si se ha aumentado un grupo de mosaicos, reducirlo a tamaños anteriores (y quitar o volver a asignar las asignaciones de mosaicos que correspondan) es más probable que se produzcan ahorros de memoria, aunque no está garantizado en el caso de que los tamaños no se correspondan exactamente con los tamaños de asignación subyacente elegidos por el controlador de pantalla.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Asignaciones en un grupo de mosaicos](mappings-are-into-a-tile-pool.md)

 

 




