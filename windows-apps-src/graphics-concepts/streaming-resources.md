---
title: Recursos de streaming
description: Los recursos de streaming son grandes recursos lógicos que usan pequeñas cantidades de memoria física. En lugar de pasar un recurso grande completo, se transmiten pequeñas partes del recurso según son necesarias. Los recursos de streaming anteriormente se denominaban recursos en mosaico.
ms.assetid: 04F0486E-4B71-4073-88DA-2AF505F4F0C1
keywords:
- Recursos de streaming
- recursos, streaming
- recursos, en mosaico
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dac89fc678e35b1e3a39d26d836f03c18d3c4684
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6974780"
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
<td align="left"><p><a href="the-need-for-streaming-resources.md">La necesidad de los recursos de streaming</a></p></td>
<td align="left"><p>Los recursos de streaming son necesarios para que no se malgaste memoria de la GPU al almacenar regiones de superficies a las que no se accederá, así como para indicar al hardware cómo filtrar entre mosaicos adyacentes.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="creating-streaming-resources.md">Creación de recursos de streaming</a></p></td>
<td align="left"><p>Los recursos de streaming se crean especificando una marca al crear un recurso, que indica que el recurso es un recurso de streaming.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="pipeline-access-to-streaming-resources.md">Acceso de canalización a los recursos de streaming</a></p></td>
<td align="left"><p>Los recursos de streaming pueden usarse en las vistas de recurso de sombreador (SRV), vistas de destino de representación (RTV), vistas de galería de símbolos de profundidad (origen de datos) y vistas de acceso sin ordenar (UAV), así como en algunos puntos de enlace donde no se usan vistas, como los enlaces de búfer de vértices.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="streaming-resources-features-tiers.md">Niveles de características de los recursos de streaming</a></p></td>
<td align="left"><p>Direct3D admite los recursos de streaming en tres niveles de funcionalidades.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

[Recursos](resources.md)

 

 




