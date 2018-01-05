---
title: Recursos de streaming
description: "Los recursos de streaming son grandes recursos lógicos que usan pequeñas cantidades de memoria física. En lugar de pasar un recurso grande completo, se transmiten pequeñas partes del recurso según son necesarias. Los recursos de streaming anteriormente se denominaban recursos en mosaico."
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- Recursos de streaming
- recursos, streaming
- recursos, en mosaico
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 54441d62107f08abe18715a91920d5cb30c75470
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="streaming-resources"></a>Recursos de streaming


Los *recursos de streaming* son grandes recursos lógicos que usan pequeñas cantidades de memoria física. En lugar de pasar un recurso grande completo, se transmiten pequeñas partes del recurso según son necesarias. Los recursos de streaming anteriormente se denominaban *recursos en mosaico*.

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
<td align="left"><p>[La necesidad de los recursos de streaming](the-need-for-streaming-resources.md)</p></td>
<td align="left"><p>Los recursos de streaming son necesarios para que no se malgaste memoria de la GPU al almacenar regiones de superficies a las que no se accederá, así como para indicar al hardware cómo filtrar entre mosaicos adyacentes.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Creación de recursos de streaming](creating-streaming-resources.md)</p></td>
<td align="left"><p>Los recursos de streaming se crean especificando una marca al crear un recurso, que indica que el recurso es un recurso de streaming.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Acceso de canalización a los recursos de streaming](pipeline-access-to-streaming-resources.md)</p></td>
<td align="left"><p>Los recursos de streaming pueden usarse en las vistas de recurso de sombreador (SRV), vistas de destino de representación (RTV), vistas de galería de símbolos de profundidad (origen de datos) y vistas de acceso sin ordenar (UAV), así como en algunos puntos de enlace donde no se usan vistas, como los enlaces de búfer de vértices.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Niveles de características de los recursos de streaming](streaming-resources-features-tiers.md)</p></td>
<td align="left"><p>Direct3D admite los recursos de streaming en tres niveles de funcionalidades.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

[Recursos](resources.md)

 

 




