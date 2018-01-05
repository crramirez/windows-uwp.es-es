---
title: Transformaciones
description: "La parte de Direct3D que inserta geometría a través de la canalización de geometría de función fija es el motor de la transformación."
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords: Transformaciones
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e5eb63a41ad04577196b6a622736211c1d1576f9
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="transforms"></a>Transformaciones


La parte de Direct3D que inserta geometría a través de la canalización de geometría de función fija es el motor de la transformación. Localiza el modelo y el visor en el mundo, proyecta vértices para mostrar en la pantalla y sujeta vértices a la ventanilla. El motor de transformación también realiza cálculos de iluminación para determinar los componentes de difusión y resaltados especulares en cada vértice.

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
<td align="left"><p>[Introducción a las transformaciones](transform-overview.md)</p></td>
<td align="left"><p>Las transformaciones matriciales controlan muchos de los cálculos de bajo nivel de los gráficos 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Transformación de mundos](world-transform.md)</p></td>
<td align="left"><p>Una transformación de mundos cambia las coordenadas de un espacio de modelo, donde se definen los vértices con respecto al origen local de un modelo, a un espacio de mundo. En el espacio de mundo, se definen vértices en relación a un origen común a todos los objetos de una escena. La transformación de mundos coloca un modelo en el mundo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Transformación de vista](view-transform.md)</p></td>
<td align="left"><p>Una <em>Transformación de vista</em> localiza el visor en el espacio de mundo, transformando los vértices en espacio de cámara. En espacio de cámara, la cámara o el visor, están en el origen, mirando hacia la dirección positiva z. La matriz de vista reubica los objetos en el mundo alrededor de la posición de la cámara (el origen del espacio de cámara) y la orientación.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Transformación de proyección](projection-transform.md)</p></td>
<td align="left"><p>Una <em>transformación de proyección</em> controla los aspectos internos de la cámara, como la elección de un objetivo para una cámara. Este es el más complicado de los tres tipos de transformación.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




