---
title: Mapas en un grupo de iconos
description: Cuando un recurso se crea como un recurso de streaming, los mosaicos que conforman el recurso provienen de apuntar a ubicaciones de un grupo de mosaicos. Un grupo de iconos es un grupo de memoria (respaldada por una o varias asignaciones en segundo plano, que no ve la aplicación).
ms.assetid: 58B8DBD5-62F5-4B94-8DD1-C7D57A812185
keywords:
- Mapas en un grupo de iconos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a0474345e21161e76fbfeebe0086e5d433b2d219
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8731655"
---
# <a name="mappings-are-into-a-tile-pool"></a>Mapas en un grupo de iconos


Cuando un recurso se crea como un recurso de streaming, los mosaicos que conforman el recurso provienen de apuntar a ubicaciones de un grupo de mosaicos. Un grupo de iconos es un grupo de memoria (respaldada por una o varias asignaciones en segundo plano, que no ve la aplicación). El sistema operativo y el controlador de pantalla administran este grupo de memoria y una aplicación comprende fácilmente la superficie de memoria. Los recursos de streaming asignan áreas de 64KB apuntando a ubicaciones de un grupo de iconos. Una ventaja de esta configuración es que permite que varios recursos compartan y reutilicen los mismos iconos, y que los mismos iconos vuelvan a usarse en distintas ubicaciones dentro de un recurso, si lo prefieres.

El coste de la flexibilidad de rellenar los iconos de un recurso a partir de un grupo de iconos es que el recurso tiene que hacer el trabajo de definición y mantenimiento de la asignación de los iconos del grupo de iconos que representan los iconos necesarios para el recurso. Se pueden cambiar las asignaciones de los iconos. Además, no todos los iconos de un recurso deben asignarse simultáneamente, un recurso puede tener asignaciones **nulas**. Una asignación **nula** define un icono que no está disponible desde el punto de vista de que el recurso no puede obtener acceso a él.

Se pueden crear varios grupos de iconos y se puede asignar cualquier cantidad de recursos de streaming a cualquier grupo de iconos determinado al mismo tiempo. También se puede ampliar o reducir los grupos de iconos. Para obtener más información, consulta [Cambio del tamaño de los grupos de iconos](tile-pool-resizing.md). Una restricción que existe para simplificar el controlador de pantalla y la implementación del tiempo de ejecución es que un determinado recurso de streaming solo puede tener asignaciones en un máximo de un grupo de iconos a la vez (en lugar de tener asignación simultánea en varios grupos de iconos).

La cantidad de almacenamiento asociada a un recurso de streaming (es decir, la memoria del grupo de iconos independiente) es aproximadamente proporcional al número de iconos que realmente están asignados al grupo en un momento determinado. En hardware, este hecho se reduce al escalado de la superficie de memoria para el almacenamiento de la tabla de página aproximadamente con la cantidad de iconos que se asignan (por ejemplo, con un esquema de tabla de página de varios niveles, según corresponda).

El grupo de iconos se puede considerar como toda una abstracción de software que permite que las aplicaciones de Direct3D puedan programar eficazmente las tablas de página de la unidad de procesamiento gráfico (GPU) sin tener que conocer los detalles de implementación de nivel bajo (o tratar directamente con las direcciones de los punteros). Los grupos de iconos no aplican niveles adicionales de direccionamiento indirecto en hardware. Las optimizaciones de una tabla de página de un solo nivel con construcciones como los directorios de página son independientes del concepto del grupo de iconos.

Nos gustaría explorar qué almacenamiento podría necesitar la tabla de página en el peor de los casos (aunque en implementaciones de práctica solo necesita almacenamiento aproximadamente proporcional a lo que está asignado).

Supongamos que cada entrada de la tabla de página es de 64 bits.

La tabla de página peor para una sola superficie, dada los límites de recursos en Direct3D11, supongamos que un recurso de streaming se crea con un formato de 128 bits por elemento (por ejemplo, un flotante RGBA), por lo tanto, un icono de 64KB contiene solo 4096 píxeles. El tamaño máximo admitido de [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) de 16384\*16384\*2048 (pero con solo un mapa MIP único) necesitaría aproximadamente 1GB de almacenamiento en la tabla de página si se llena del todo (sin incluir los mapas MIP) con entradas de tabla de 64 bits. Si se agregan mapas MIP, aumentará el almacenamiento de tabla de página (en el peor de los casos) completamente asignado en aproximadamente un tercio, 1,3GB.

Este caso daría acceso a aproximadamente 10,6 terabytes de memoria direccionable. Puede haber un límite en la cantidad de memoria direccionable, pero podría reducir estas cantidades, quizás alrededor del rango de terabytes.

Otro caso a tener en cuenta es un único recurso de streaming de [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) de 16384\*16384 con un formato de 32 bits por elemento, incluidos mapas MIP. El espacio necesario en una tabla de página completa sería aproximadamente de 170KB con entradas de tabla de 64 bits.

Por último, veamos un ejemplo con un formato BC, por ejemplo, BC7 con 128 bits por icono de 4x4 píxeles. Es un byte por píxel. Un objeto [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) de 16384\*16384\*2048 con mapas MIP necesitaría aproximadamente 85MB para rellenar completamente esta memoria en una tabla de página. No está mal teniendo en cuenta que esto permite que un recurso de streaming expanda 550 gigapíxeles (512GB de memoria en este caso).

En la práctica, no se definiría nada parecido a estas asignaciones completas, dado que la cantidad de memoria física disponible no permitiría nada cercano a la cantidad asignada y de referencia a la vez. Pero, con un grupo de iconos, las aplicaciones pueden optar por volver a usar los iconos (como un ejemplo simple, reutilizar un icono de color "negro" para grandes regiones en negro de una imagen). Es eficaz si se usa el grupo de iconos (es decir, asignaciones de tabla de página) como una herramienta de compresión de memoria.

El contenido inicial de la tabla de página es **nulo** para todas las entradas. Las aplicaciones tampoco pueden pasar datos iniciales para el contenido de la memoria de superficie, ya que se inicia sin respaldo de memoria.

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
<td align="left"><p>Las aplicaciones pueden crear uno o varios grupos de iconos por dispositivo de Direct3D. El tamaño total de cada grupo de iconos está restringido al límite de tamaño de recursos del Direct3D11, que es aproximadamente 1/4 de RAM de GPU.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-pool-resizing.md">Cambio del tamaño de los grupos de iconos</a></p></td>
<td align="left"><p>Cambia el tamaño de un grupo de iconos para aumentar un grupo de iconos si la aplicación necesita más espacio de trabajo para los recursos de streaming asignados. También puedes reducirlo si se necesita menos espacio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="hazard-tracking-versus-tile-pool-resources.md">Seguimiento de peligros frente a recursos de grupos de iconos</a></p></td>
<td align="left"><p>Para recursos que no son de streaming, Direct3D puede impedir ciertas condiciones de riesgo durante la representación, pero, ya que el seguimiento de riesgos sería a nivel de iconos para los recursos de streaming, las condiciones de riesgo de seguimiento durante la representación de los recursos de streaming pueden ser demasiado altas.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Creación de recursos de streaming](creating-streaming-resources.md)

 

 




