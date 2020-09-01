---
title: Mapas en un grupo de iconos
description: Cuando se crea un recurso como un recurso de streaming, los mosaicos que componen el recurso proceden de apuntar a ubicaciones de un grupo de mosaicos. Un grupo de mosaicos es un grupo de memoria (respaldado por una o varias asignaciones en segundo plano, no detectadas por la aplicación).
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- Mapas en un grupo de iconos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b021fc47eee6063761635422a780bb5f9f341f4d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171919"
---
# <a name="mappings-are-into-a-tile-pool"></a>Mapas en un grupo de iconos


Cuando se crea un recurso como un recurso de streaming, los mosaicos que componen el recurso proceden de apuntar a ubicaciones de un grupo de mosaicos. Un grupo de mosaicos es un grupo de memoria (respaldado por una o varias asignaciones en segundo plano, no detectadas por la aplicación). El sistema operativo y el controlador de pantalla administran este grupo de memoria y la superficie de memoria se entiende fácilmente por parte de una aplicación. Los recursos de streaming asignan regiones de 64 KB apuntando a ubicaciones de un grupo de mosaicos. Una Fallout de esta configuración permite que varios recursos compartan y reutilicen los mismos mosaicos, y que también se reutilicen los mismos mosaicos en ubicaciones diferentes dentro de un recurso si se desea.

El costo de la flexibilidad de rellenar los iconos de un recurso de un grupo de mosaicos es que el recurso tiene que realizar el trabajo de definir y mantener la asignación de los mosaicos del grupo de mosaicos que representan los mosaicos necesarios para el recurso. Se pueden cambiar las asignaciones de mosaicos. Además, no es necesario asignar todos los mosaicos de un recurso a la vez; un recurso puede tener asignaciones **null** . Una asignación **null** define un icono como no disponible desde el punto de vista del recurso que tiene acceso a él.

Se pueden crear varios grupos de mosaicos y cualquier número de recursos de streaming puede asignarse a un grupo de mosaicos determinado al mismo tiempo. También se pueden incrementar o reducir los grupos de mosaicos. Para obtener más información, consulte [cambio de tamaño del grupo de mosaicos](tile-pool-resizing.md). Una restricción que existe para simplificar la implementación del controlador de pantalla y del tiempo de ejecución es que un recurso de streaming determinado solo puede tener asignaciones a un grupo de mosaicos como máximo a la vez (en lugar de tener una asignación simultánea a varios grupos de mosaicos).

La cantidad de almacenamiento que está asociada a un recurso de streaming en sí (es decir, memoria de grupo de mosaicos independiente) es aproximadamente proporcional al número de mosaicos asignados realmente al grupo en un momento dado. En el hardware, este hecho reduce el tamaño de la superficie de memoria para el almacenamiento de tabla de páginas aproximadamente con la cantidad de mosaicos que se asignan (por ejemplo, con un esquema de tabla de páginas multinivel según corresponda).

El grupo de mosaicos puede considerarse como una abstracción de software totalmente que permite que las aplicaciones de Direct3D puedan programar eficazmente las tablas de páginas en la unidad de procesamiento de gráficos (GPU) sin tener que conocer los detalles de implementación de bajo nivel (o tratar directamente las direcciones de puntero). Los grupos de mosaicos no aplican ningún nivel de direccionamiento indirecto adicional en el hardware. Las optimizaciones de una tabla de páginas de un solo nivel mediante construcciones como los directorios de páginas son independientes del concepto de grupo de mosaicos.

Vamos a explorar qué almacenamiento podría requerir la tabla de páginas en el peor de los casos (aunque en las implementaciones de la práctica solo se requiere almacenamiento aproximadamente proporcional a lo que se asigna).

Suponga que cada entrada de la tabla de páginas es 64 bits.

En el peor de los casos, el tamaño de la tabla de páginas se ha alcanzado para una sola superficie, dados los límites de recursos en Direct3D 11, supongamos que se crea un recurso de streaming con un formato de 128 bits por elemento (por ejemplo, un valor Float de 64 KB), por lo que un mosaico de 64 KB solo contiene 4096 píxeles. El tamaño máximo admitido de [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) de 16384 \* 16384 \* 2048 (pero con un solo mipmap) requeriría aproximadamente 1 GB de almacenamiento en la tabla de la página si se rellenaron completamente (sin incluir los mapas MIP) mediante entradas de la tabla de 64 bits. Agregar mapas de bits aumentaría el almacenamiento de la tabla de páginas de asignación completa (el peor de los casos) en unos tercios a aproximadamente 1,3 GB.

Este caso daría acceso a aproximadamente 10,6 terabytes de memoria direccionable. Sin embargo, puede haber un límite en la cantidad de memoria direccionable, lo que reduciría estos importes, quizás en torno al intervalo de terabytes.

Otro caso a tener en cuenta es un único recurso de streaming de [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) de 16384 \* 16384 con un formato de bit por elemento de 32, incluidos los mapas de bits. El espacio necesario en una tabla de páginas completa sería aproximadamente 170KB con las entradas de la tabla de 64 bits.

Por último, considere la posibilidad de usar un formato BC, por ejemplo BC7 con 128 bits por mosaico de 4x4 píxeles. Es un byte por píxel. Un [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) de 16384 \* 16384 \* 2048, incluidos los mapas de mapas, necesitaría aproximadamente 85MB para rellenar por completo esta memoria en una tabla de páginas. Esto no es malo considerar esto permite que un recurso de streaming abarque 550 Gigapixels (512 GB de memoria en este caso).

En la práctica, no se definió ninguna parte cercana a estas asignaciones completas, dado que la cantidad de memoria física disponible no permitiría en todo momento cerca de que se asignara y se haga referencia a ella a la vez. Sin embargo, con un grupo de mosaicos, las aplicaciones pueden optar por reutilizar los mosaicos (por ejemplo, reutilizar un icono de color "negro" para las regiones de color negro de gran tamaño de una imagen) de forma eficaz mediante el grupo de mosaicos (es decir, las asignaciones de tabla de páginas) como herramienta para la compresión de memoria.

El contenido inicial de la tabla de páginas es **null** para todas las entradas. Las aplicaciones tampoco pueden pasar los datos iniciales para el contenido de la memoria de la superficie desde que se inicia sin necesidad de hacer una copia de seguridad de la memoria.

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
<td align="left"><p><a href="tile-pool-creation.md">Creación de grupos de iconos</a></p></td>
<td align="left"><p>Las aplicaciones pueden crear uno o varios grupos de mosaicos por dispositivo Direct3D. El tamaño total de cada grupo de mosaicos está restringido al límite de tamaño de los recursos de Direct3D 11, que es aproximadamente 1/4 de RAM de GPU.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-pool-resizing.md">Cambio del tamaño de los grupos de iconos</a></p></td>
<td align="left"><p>Cambiar el tamaño de un grupo de mosaicos para aumentar un grupo de mosaicos si la aplicación necesita más espacio de trabajo para la asignación de recursos de streaming en él, o para reducir si se necesita menos espacio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="hazard-tracking-versus-tile-pool-resources.md">Seguimiento de peligros frente a recursos de grupos de iconos</a></p></td>
<td align="left"><p>En el caso de los recursos que no son de streaming, Direct3D puede evitar ciertas condiciones de riesgo durante la representación, pero como el seguimiento de los peligros sería un nivel de mosaico para los recursos de streaming, el seguimiento de las condiciones de riesgo durante la representación de los recursos de streaming puede ser demasiado caro.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Creación de recursos de streaming](creating-streaming-resources.md)

 

 