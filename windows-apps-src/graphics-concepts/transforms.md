---
title: Transformaciones
description: La parte de Direct3D que inserta geometría a través de la canalización de geometría de función fija es el motor de la transformación.
ms.assetid: 0DF2A99A-335C-4D14-9720-6D7996DD635A
keywords:
- Transformaciones
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a29d42a9254ca47402a38ea71c8c1ef69de5c6c7
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8344365"
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
<td align="left"><p><a href="transform-overview.md">Introducción a las transformaciones</a></p></td>
<td align="left"><p>Las transformaciones matriciales controlan muchos de los cálculos de bajo nivel de los gráficos 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="world-transform.md">Transformación de mundos</a></p></td>
<td align="left"><p>Una transformación de mundos cambia las coordenadas de un espacio de modelo, donde se definen los vértices con respecto al origen local de un modelo, a un espacio de mundo. En el espacio de mundo, se definen vértices en relación a un origen común a todos los objetos de una escena. La transformación de mundos coloca un modelo en el mundo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="view-transform.md">Transformación de vista</a></p></td>
<td align="left"><p>Una <em>Transformación de vista</em> localiza el visor en el espacio de mundo, transformando los vértices en espacio de cámara. En espacio de cámara, la cámara o el visor, están en el origen, mirando hacia la dirección positiva z. La matriz de vista reubica los objetos en el mundo alrededor de la posición de la cámara (el origen del espacio de cámara) y la orientación.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="projection-transform.md">Transformación de proyección</a></p></td>
<td align="left"><p>Una <em>transformación de proyección</em> controla los aspectos internos de la cámara, como la elección de un objetivo para una cámara. Este es el más complicado de los tres tipos de transformación.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




