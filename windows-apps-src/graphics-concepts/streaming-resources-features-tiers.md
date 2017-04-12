---
title: "Niveles de características de los recursos de streaming"
description: Direct3D admite los recursos de streaming en tres niveles de funcionalidades.
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords: "Niveles de características de los recursos de streaming"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 671c679938860057a5fe8f083eebfe47dc518e05
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
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
<td align="left"><p>[Nivel 1](tier-1.md)</p></td>
<td align="left"><p>Esta sección describe la compatibilidad del nivel 1.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Nivel 2](tier-2.md)</p></td>
<td align="left"><p>La compatibilidad de nivel 2 para los recursos de streaming agrega funcionalidades a las del nivel 1, como garantizar mapas MIP de texturas sin empaquetar cuando el tamaño es al menos una forma de icono estándar; instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; además, la lectura en iconos asignados NULL tratan el valor de muestra como cero.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Nivel 3](tier-3.md)</p></td>
<td align="left"><p>El nivel 3 agrega compatibilidad para Texture3D para los recursos de streaming, además de las funcionalidades del [nivel 2](tier-2.md).</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 




