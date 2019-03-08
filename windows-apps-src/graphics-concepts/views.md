---
title: Vistas
description: El término \ 0034;vista \ 0034; se usa para referirse a \ 0034;datos en el formato necesario\ 0034;. Por ejemplo, una vista de búfer de constantes (CBV) serían los datos de búfer de constantes con el formato correcto. En esta sección se describen las vistas más comunes y útiles.
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- Vistas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: df106a400a7ba8c8f94aa6dd35325aabacd36eca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663300"
---
# <a name="views"></a>Vistas


El término "vista" se usa para referirse a "datos en el formato necesario". Por ejemplo, una vista de búfer de constantes (CBV) serían los datos de búfer de constantes con el formato correcto. En esta sección se describen las vistas más comunes y útiles.

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
<td align="left"><p><a href="constant-buffer-view--cbv-.md">Vista de búfer de constantes (CBV)</a></p></td>
<td align="left"><p>Los búferes de constantes contienen datos de constantes de sombreador. Su valor radica en que los datos persisten, y cualquier sombreador de la GPU puede acceder a ellos, hasta que sea necesario cambiar los datos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-buffer-view--vbv-.md">Vista de búfer de vértices (VBV) y la vista de índice de búfer (IBV)</a></p></td>
<td align="left"><p>Un búfer de vértices contiene los datos de una lista de vértices. Los datos para cada vértice pueden incluir posición, color, vector normal, coordenadas de textura, etc. Un búfer de índices contiene índices de enteros (desplazamientos) en un búfer de vértices y se usa para definir y representar un objeto que se compone de un subconjunto de la lista completa de vértices.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="shader-resource-view--srv-.md">Vista de recursos (SRV) de sombreador y vista de acceso desordenado (UAV)</a></p></td>
<td align="left"><p>Las vistas de recurso de sombreador normalmente encapsulan texturas en un formato que permite a los sombreadores acceder a ellas. Una vista de acceso desordenada proporciona una funcionalidad similar, pero permite la lectura de la textura y la escritura en ella (o en cualquier otro recurso) en cualquier orden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="sampler.md">muestra</a></p></td>
<td align="left"><p>El muestreo es el proceso de leer los valores de entrada de una textura u otros recursos. Una &quot;muestra&quot; es cualquier objeto que se lee desde recursos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-target-view--rtv-.md">Representar la vista de destino (RTV)</a></p></td>
<td align="left"><p>Los destinos de representación permiten representar una escena en un búfer intermedio temporal, en lugar de hacerlo en el búfer de reserva que se representará en la pantalla. Esta característica permite el uso de la escena compleja que podría representarse, quizás como una textura de reflexión u otro fin dentro de la canalización de gráficos, o quizás para agregar efectos de sombreador de píxeles adicionales a la escena antes de su representación.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="depth-stencil-view--dsv-.md">Vista de galería de símbolos de profundidad (DSV)</a></p></td>
<td align="left"><p>Una vista de galería de símbolos de profundidad proporciona el formato y el búfer para la información de profundidad y galería de símbolos. El búfer de profundidad se usa para eliminar el dibujo de píxeles que sería invisible en el visor, ya que quedan ocultos por un objeto más cercano. El búfer de galería de símbolos puede usarse para eliminar todos los dibujos fuera de una forma definida.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="stream-output-view--sov-.md">Vista de salida de Stream (SOV)</a></p></td>
<td align="left"><p>Las vistas de salida de lujo permiten que la información sobre los vértices que los sombreadores de vértices, teselación y geometría han obtenido se vuelva a transmitir a la aplicación para su uso posterior. Por ejemplo, un objeto que estos sombreadores hayan distarcionado puede volver a escribirse en la aplicación para proporcionar una entrada más precisa para una física u otro motor. Sin embargo, en la práctica, las vistas de salida de lujo son una característica poco usada de la canalización de gráficos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rasterizer-ordered-view--rov-.md">Rasterizador ordenada vista (ROV)</a></p></td>
<td align="left"><p>Las vistas ordenadas de rasterizador permiten corregir algunas de las limitaciones de un búfer de profundidad, en particular la de tener varias texturas con transparencia que se apliquen a los mismos píxeles.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

 

 




