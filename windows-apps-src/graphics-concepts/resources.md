---
title: Recursos
description: Un recurso es una área en la memoria a la que puede obtener acceso la canalización de Direct3D.
ms.assetid: 2E68E5A8-83DA-4DC8-B7F3-B8988CF8090C
keywords:
- Recursos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a56b03de29e110a2768ebe71f4e61d8099ca1cf8
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6460968"
---
# <a name="resources"></a>Recursos


Un recurso es una área en la memoria a la que puede obtener acceso la canalización de Direct3D. Para que la canalización pueda obtener acceso a la memoria de forma eficiente, los datos que se proporcionan a la canalización (por ejemplo, geometría de entrada, recursos del sombreador y texturas) deben almacenarse en un recurso. Hay dos tipos de recursos de los que derivan todos los recursos de Direct3D: un búfer o una textura. Se pueden activar hasta 128 recursos para cada fase de la canalización.

Por lo general, cada aplicación creará muchos recursos. Algunos ejemplos de recursos son: búferes de vértices, búfer de índices, búfer de constantes, texturas y recursos de sombreador. Existen varias opciones que determinan cómo se pueden utilizar los recursos. Puedes crear recursos fuertemente tipados o sin tipos; puedes controlar si los recursos tienen acceso de lectura y escritura; y puedes hacer que los recursos solo sean accesibles para la CPU, la GPU o ambas. Naturalmente, habrá compensación entre velocidad y funcionalidad: cuanta más funcionalidad permitas que tenga un recurso, menos rendimiento podrás esperar.

Dado que una aplicación suele utilizar muchas texturas, Direct3D también tiene el concepto de una matriz de texturas para simplificar la administración de texturas. Una matriz de texturas contiene uno o más texturas (todas del mismo tipo y con las mismas dimensiones) que se pueden indexar desde una aplicación o a través de los sombreadores. Las matrices de texturas permiten utilizar una única interfaz con varios índices para acceder a muchas texturas. Puedes crear tantas matrices de textura como necesites para administrar distintos tipos de texturas.

Una vez que hayas creado los recursos que usará la aplicación, debes conectar o enlazar cada recurso a las fases de la canalización que los usarán. Esto se logra mediante una llamada a una API de enlace, que lleva un puntero al recurso. Dado que más de una fase de la canalización puede necesitar acceder al mismo recurso, Direct3D presenta el concepto de vista de recursos. Una vista identifica la parte de un recurso a la que se puede acceder. Puedes crear *m* vistas o un recurso y enlazarlo a *n* fases de la canalización, suponiendo que sigas las reglas de enlace para el recurso compartido (de lo contrario, el tiempo de ejecución generará errores en tiempo de compilación).

Una vista de recursos proporciona un modelo general para acceder a un recurso (como texturas o búferes). Dado que puedes usar una vista para indicar al tiempo de ejecución los datos a los que quieres acceder y cómo hacerlo, las vistas de recursos permiten crear recursos sin tipos. Es decir, puedes crear recursos para un tamaño determinado en tiempo de compilación y, a continuación, declarar el tipo de datos dentro del recurso cuando este se enlace a la canalización. Las vistas exponen muchas funcionalidades para el uso de recursos, como la capacidad de leer superficies de reserva de profundidad o galería de símbolos en el sombreador, que genera mapas de cubos dinámicos en una sola fase y los representa simultáneamente en varios segmentos de un volumen.

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
<td align="left"><p><a href="resource-types.md">Tipos de recursos</a></p></td>
<td align="left"><p>Los distintos tipos de recursos tienen un diseño (o una superficie de memoria) diferente. Todos los recursos que utiliza la canalización de Direct3D derivan de dos tipos de recursos básicos: <a href="resource-types.md#buffer-resources">búferes</a> y <a href="resource-types.md#texture-resources">texturas</a>. Un búfer es una colección de datos sin procesar (elementos); una textura es una colección de elementos de textura.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="choosing-a-resource.md">Elección de un recurso</a></p></td>
<td align="left"><p>Un recurso es una colección de datos que se usa en la canalización 3D. La creación de recursos y la definición de su comportamiento constituyen el primer paso para la programación de la aplicación. En esta guía se tratan los aspectos básicos para elegir los recursos necesarios para la aplicación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="copying-and-accessing-resource-data.md">Copia y acceso a datos de recursos</a></p></td>
<td align="left"><p>Los indicadores de uso indican cómo la aplicación pretende utilizar los datos de recursos para colocar recursos en el área de mayor rendimiento de memoria posible. Los datos de recursos se copian en los recursos para que la CPU o la GPU puedan acceder a estos sin que ello afecte al rendimiento.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-views.md">Vistas de texturas</a></p></td>
<td align="left"><p>En Direct3D, se accede a los recursos de texturas con una vista, que es un mecanismo para la interpretación de hardware de un recurso en memoria. Una vista permite que una fase de la canalización concreta acceda solo a los <a href="resource-types.md">subrecursos</a> que necesita, en la representación que desea la aplicación.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas](coordinate-systems.md)

[Guía de aprendizaje de gráficos de Direct3D](index.md)

[Reglas de punto flotante](floating-point-rules.md)

[Conversión de tipos de datos](data-type-conversion.md)
