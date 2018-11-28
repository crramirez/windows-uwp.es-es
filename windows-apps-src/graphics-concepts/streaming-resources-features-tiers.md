---
title: Niveles de características de los recursos de streaming
description: Direct3D admite los recursos de streaming en tres niveles de funcionalidades.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- Niveles de características de los recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c872d289c67161e414671d3d509401f0539a7675
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7840881"
---
# <a name="streaming-resources-features-tiers"></a>Niveles de características de los recursos de streaming


Direct3D admite los recursos de streaming en tres niveles de funcionalidades.

El nivel 1 proporciona funcionalidades básicas para los recursos de streaming.

El nivel 2 agrega funcionalidades a las del nivel 1, como garantizar mapas MIP de texturas sin empaquetar cuando el tamaño es al menos una forma de icono estándar; instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; además, la lectura en iconos asignados NULL tratan el valor de muestra como cero.

El nivel 3 agrega capacidades de Texture3D, además de las del nivel 2.

Existen funciones de consulta disponibles en las versiones de Direct3D para validar la compatibilidad del hardware y los controladores para los recursos de streaming y en qué nivel.

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
<td align="left"><p><a href="tier-1.md">Nivel 1</a></p></td>
<td align="left"><p>Esta sección describe la compatibilidad del nivel 1.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">Nivel 2</a></p></td>
<td align="left"><p>La compatibilidad de nivel 2 para los recursos de streaming agrega funcionalidades a las del nivel 1, como garantizar mapas MIP de texturas sin empaquetar cuando el tamaño es al menos una forma de icono estándar; instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; además, la lectura en iconos asignados NULL tratan el valor de muestra como cero.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">Nivel 3</a></p></td>
<td align="left"><p>El nivel 3 agrega compatibilidad para Texture3D para los recursos de streaming, además de las funcionalidades del <a href="tier-2.md">nivel 2</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 




