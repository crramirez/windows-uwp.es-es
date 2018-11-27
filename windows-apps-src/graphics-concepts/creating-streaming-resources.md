---
title: Creación de recursos de streaming
description: Los recursos de streaming se crean al especificar una marca cuando creas un recurso, lo que indica que el recurso es un recurso de streaming.
ms.assetid: B3F3E43C-54D4-458C-9E16-E13CB382C83F
keywords:
- Creación de recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec96f6245969d32357563c44107f539fb9043aac
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7712121"
---
# <a name="creating-streaming-resources"></a>Creación de recursos de streaming


Los recursos de streaming se crean al especificar una marca cuando creas un recurso, lo que indica que el recurso es un recurso de streaming.

Las restricciones relativas a cuándo puedes crear un recurso como recurso de streaming se describen en [Parámetros de creación de recursos de streaming](streaming-resource-creation-parameters.md).

El almacenamiento de un recurso que no es de streaming se asigna en el sistema de gráficos cuando se crea el recurso, como la asignación de una matriz de texturas 2D.

Cuando se crea un recurso de streaming, el sistema de gráficos no asigna el almacenamiento para el contenido del recurso. En su lugar, cuando una aplicación crea un recurso de streaming, el sistema de gráficos hace una reserva de espacio de direcciones para el área de la superficie con mosaicos únicamente y luego permite la asignación de los mosaicos para que los controle la aplicación. La "asignación" de un mosaico es simplemente la ubicación física en la memoria adonde apunta un mosaico lógico en un recurso (o **NULL** para un mosaico sin asignar).

No confundas este concepto con la noción de asignación de un recurso de Direct3D para el acceso de la CPU, lo que, a pesar de usar el mismo nombre, es completamente independiente. Podrás definir y cambiar la asignación de cada mosaico de manera individual, según sea necesario, sabiendo que no es necesario asignar a la vez todos los mosaicos para una superficie, lo que permite un uso eficaz de la cantidad de memoria disponible.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>En esta sección


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="mappings-are-into-a-tile-pool.md">Asignaciones a un grupo de mosaicos</a></p></td>
<td align="left"><p>Cuando un recurso se crea como un recurso de streaming, los mosaicos que conforman el recurso provienen de apuntar a ubicaciones de un grupo de mosaicos. Un grupo de mosaicos es un grupo de memoria (respaldada por una o varias asignaciones en segundo plano, que no ve la aplicación).</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-creation-parameters.md">Parámetros de creación de recursos de streaming</a></p></td>
<td align="left"><p>Existen algunas limitaciones en el tipo de recursos de Direct3D que puedes crear como recurso de streaming.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tile-pool-creation-parameters.md">Parámetros de creación de grupos de mosaicos</a></p></td>
<td align="left"><p>Usa los parámetros de esta sección para definir grupos de mosaicos al crear un búfer.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resource-cross-process-and-device-sharing.md">Uso compartido de dispositivos y entre procesos de recursos de streaming</a></p></td>
<td align="left"><p>Los grupos de mosaicos pueden compartirse con otros procesos al igual que los recursos tradicionales. Los recursos de streaming que hacen referencia a grupos de mosaicos no pueden compartirse en diferentes dispositivos y procesos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="operations-available-on-streaming-resources.md">Operaciones disponibles en recursos de streaming</a></p></td>
<td align="left"><p>En esta sección se enumeran las operaciones que puedes realizar en recursos de streaming.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="operations-available-on-tile-pools.md">Operaciones disponibles en grupos de mosaicos</a></p></td>
<td align="left"><p>Las operaciones en grupos de mosaicos incluyen cambiar el tamaño de un grupo de mosaicos, ofrecer recursos (ceder memoria temporalmente al sistema para todo el grupo de mosaicos) y recuperar recursos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="how-a-streaming-resource-s-area-is-tiled.md">Cómo se organiza en mosaico el área de un recurso de streaming</a></p></td>
<td align="left"><p>Cuando creas un recurso de streaming, las dimensiones, el tamaño del elemento de formato y el número de mapas MIP o los segmentos de matriz (si procede) determinan el número de mosaicos que son necesarios para cubrir toda la superficie.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 




