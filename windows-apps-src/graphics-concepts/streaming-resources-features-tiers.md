---
title: Niveles de características de recursos de streaming
description: Obtenga acceso a artículos sobre los tres niveles de funcionalidades de características para los recursos de streaming de Direct3D, denominados previamente recursos en mosaico.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- Niveles de características de recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ee27244c4d4c2797b71c9d5c8c2c5185a99596b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173059"
---
# <a name="streaming-resources-features-tiers"></a>Niveles de características de recursos de streaming


Direct3D admite recursos de streaming en tres niveles de funcionalidad.

El nivel 1 proporciona capacidades básicas para los recursos de streaming.

El nivel 2 agrega funcionalidades más allá del nivel 1, como garantizar el mipmap de textura no empaquetada cuando el tamaño es al menos una forma de mosaico estándar. instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; Además, la lectura de los mosaicos asignados con valores NULL trata el valor muestreado como cero.

El nivel 3 agrega funcionalidades de Texture3D, más allá del nivel 2.

Las funciones de consulta están disponibles en las versiones de Direct3D, para validar la compatibilidad de hardware y controladores con los recursos de streaming y en qué nivel de nivel.

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
<td align="left"><p>En esta sección se describe la compatibilidad de nivel 1.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">Nivel 2</a></p></td>
<td align="left"><p>La compatibilidad de nivel 2 con recursos de streaming agrega funcionalidades más allá del nivel 1, como garantizar el mipmap de textura no empaquetada cuando el tamaño es al menos una forma de mosaico estándar. instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; Además, la lectura de los mosaicos asignados con valores NULL trata el valor muestreado como cero.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">Nivel 3</a></p></td>
<td align="left"><p>El nivel 3 agrega compatibilidad con Texture3D para los recursos de streaming, además de las capacidades del <a href="tier-2.md">nivel 2</a> .</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 




