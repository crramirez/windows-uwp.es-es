---
title: Copia y acceso a datos de recursos
description: Los indicadores de uso indican cómo la aplicación pretende utilizar los datos de recursos para colocar recursos en el área de mayor rendimiento de memoria posible. Los datos de recursos se copian en los recursos para que la CPU o la GPU puedan acceder a estos sin que ello afecte al rendimiento.
ms.assetid: 6A09702D-0FF2-4EA6-A353-0F95A3EE34E2
keywords:
- Copia y acceso a datos de recursos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e7b0f06711b4a908f8990dfb16968400c685c15f
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6861218"
---
# <a name="copying-and-accessing-resource-data"></a>Copia y acceso a datos de recursos


Los indicadores de uso indican cómo la aplicación pretende utilizar los datos de recursos para colocar recursos en el área de mayor rendimiento de memoria posible. Los datos de recursos se copian en los recursos para que la CPU o la GPU puedan acceder a estos sin que ello afecte al rendimiento.

No es necesario pensar en si los recursos se crean en la memoria de vídeo o en la memoria del sistema, ni decidir si el tiempo de ejecución debe administrar la memoria o no. Con la arquitectura del WDDM (modelo de controladores de pantalla de Windows), las aplicaciones crean recursos de Direct3D con distintos indicadores de uso para mostrar cómo la aplicación pretende usar los datos de recursos. Este modelo de controladores virtualiza la memoria usada por los recursos; es responsabilidad del administrador del sistema operativo, del controlador o de la memoria incluir recursos en el área de memoria de mayor rendimiento posible, dado el uso esperado.

El caso predeterminado corresponde a que los recursos estén disponibles para la GPU. Hay ocasiones en que los datos de recursos deben estar disponible para la CPU. Copiar los datos de recursos para que el procesador adecuado pueda acceder a ellos sin afectar al rendimiento requiere ciertos conocimientos acerca de cómo funcionan los métodos API.

## <a name="span-idcopyingspanspan-idcopyingspanspan-idcopyingspancopying-resource-data"></a><span id="Copying"></span><span id="copying"></span><span id="COPYING"></span>Copiar datos de recursos


Los recursos se crean en la memoria cuando Direct3D ejecuta una llamada Create. Se pueden crear en la memoria de vídeo, en la memoria del sistema o en cualquier otro tipo de memoria. Dado que WDDM virtualiza esta memoria, las aplicaciones ya no necesitan hacer un seguimiento de en qué tipo de memoria se crean los recursos.

Idealmente, todos los recursos estarían ubicados en la memoria de vídeo para que la GPU pueda acceder a ellos de inmediato. Sin embargo, en ocasiones es necesario que la CPU lea los datos de recursos o que la GPU acceda a los datos de recursos que ha escrito la CPU. Para controlar estas distintas situaciones, Direct3D solicita que la aplicación especifique un uso y luego ofrece varios métodos para copiar los datos de recursos cuando sea necesario.

Según cómo se haya creado el recurso, no siempre es posible acceder directamente a los datos subyacentes. Esto puede significar que los datos de recursos se deban copiar desde el recurso de origen a otro recurso al que el procesador adecuado pueda acceder. En cuanto a Direct3D, la GPU puede acceder directamente a los recursos predeterminados, y la CPU puede acceder directamente a los recursos dinámicos y provisionales.

Una vez que se ha creado un recurso, no se puede cambiar su uso. En su lugar, copia el contenido de un recurso a otro recurso que se haya creado con otro uso. Puedes copien los datos de recursos de un recurso a otra o copiar los datos de la memoria a un recurso.

Existen dos tipos principales de recursos: asignables y no asignables. Los recursos creados con usos dinámicos o provisionales son asignables, mientras que los recursos creados con los usos predeterminados o inmutables no son asignables.

Copiar datos entre recursos no asignables es muy rápido, ya que este es el caso más común y se ha optimizado para tener un buen rendimiento. Dado que la CPU no accede directamente a estos recursos, están optimizados para que la GPU pueda manipularlos rápidamente.

Copiar datos entre recursos asignables es más problemático, ya que el rendimiento dependerá del uso para el que se creó el recurso. Por ejemplo, la GPU puede leer un recurso dinámico bastante rápido, pero no puede escribir en él, y no puede leer ni escribir en recursos provisionales directamente.

Las aplicaciones que deseen copiar datos desde un recurso con el uso predeterminado a un recurso con el uso provisional (para permitir que la CPU lea los datos, es decir, el problema de lectura de la GPU) deben hacerlo con cuidado. Consulta [Acceso a datos de recursos](#accessing) a continuación.

## <a name="span-idaccessingspanspan-idaccessingspanspan-idaccessingspanaccessing-resource-data"></a><span id="Accessing"></span><span id="accessing"></span><span id="ACCESSING"></span>Acceso a datos de recursos


El acceso a un recurso requiere asignar el recurso; "asignar" básicamente significa que la aplicación intenta darle a la CPU acceso a la memoria. El proceso de asignar un recurso para que la CPU pueda acceder a la memoria subyacente puede provocar algunos cuellos de botella en el rendimiento y, por este motivo, debe tenerse cuidado en cuanto al cómo y cuándo se realiza esta tarea.

Si la aplicación intenta asignar un recurso en el momento incorrecto, el rendimiento puede paralizarse. Si la aplicación intenta acceder a los resultados de una operación antes de que termine la operación, se detendrá la canalización.

Realizar una operación de asignación en el momento incorrecto podría provocar una caída grave del rendimiento al obligar a la GPU y a la CPU a sincronizarse entre sí. Esta sincronización se producirá si la aplicación quiere acceder a un recurso antes de que la GPU termine de copiarlo en un recurso que la CPU pueda asignar.

### <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Consideraciones de rendimiento

Es mejor pensar en un equipo como una máquina que funciona como una arquitectura en paralelo con dos tipos principales de procesadores: una CPU o más y una GPU o más. Al igual que en cualquier arquitectura en paralelo, el mejor rendimiento se logra cuando cada procesador tiene programadas suficientes tareas para evitar que quede inactivo y cuando el trabajo de un procesador no espera el trabajo del otro.

La peor situación para la paralelismo de la CPU/GPU es la necesidad de forzar un procesador para que espere los resultados del trabajo que hace el otro. Direct3D elimina este inconveniente haciendo que los métodos de copia sean asincrónicos; la copia no se ejecuta necesariamente en el momento en que el método devuelve.

La ventaja de esto es que la aplicación no paga el coste de rendimiento de copiar realmente los datos hasta que la CPU accede a los datos, que es cuando se llama a Map. Si se llama al método Map después de que en verdad se han copiado los datos, no se produce ninguna pérdida de rendimiento. Por otro lado, si se llama al método Map antes de que se hayan copiado los datos, se detendrá la canalización.

Las llamadas asincrónicas en Direct3D (que son la amplia mayoría de los métodos y, en particular, las llamadas de representación) se almacenan en lo que se denomina un *búfer de comandos*. Este búfer es interno para el controlador de gráficos y se usa para procesar por lotes las llamadas al hardware subyacente, para que el cambio costoso del modo de usuario al modo de kernel en Microsoft Windows se produzca con la menor frecuencia posible.

El búfer de comandos se vacía, lo que provoca un cambio de modo de usuario a kernel, en una de cuatro situaciones, que son las siguientes.

1.  Se llama a Present.
2.  Se llama a Flush.
3.  El búfer de comandos está lleno; su tamaño es dinámico y se controla mediante el sistema operativo y el controlador de gráficos.
4.  La CPU requiere acceso a los resultados de un comando que está a la espera de ejecución en el búfer de comandos.

De las cuatro situaciones anteriores, la cuarta es la que más afecta al rendimiento. Si la aplicación emite una llamada para copiar un recurso (o subrecurso), esta llamada se pone en cola en el búfer de comandos.

Si la aplicación luego intenta asignar el recurso provisional que era el destino de la llamada de copia antes de que se haya vaciado el búfer de comandos, se detendrá la canalización, porque no solo es necesario ejecutar la llamada al método Copy, sino que también deben ejecutarse todos los demás comandos en el búfer de comandos. Esto hará que la GPU y la CPU se sincronicen, porque la CPU estará esperando acceder al recurso provisional mientras la GPU está vaciando el búfer de comandos y, finalmente, rellenando el recurso que necesita la CPU. Una vez que la GPU finalice la copia, la CPU empezará a acceder al recurso provisional, pero, durante este tiempo, la GPU se encontrará inactiva.

Si esto sucede con frecuencia en tiempo de ejecución, el rendimiento se degradará enormemente. Por ese motivo, la asignación de recursos creados con el uso predeterminado debe hacerse con cuidado. La aplicación debe esperar lo suficiente para que el búfer de comando se vacíe y, por consiguiente, finalice la ejecución de todos esos comandos antes de intentar asignar el recurso provisional correspondiente.

¿Cuánto tiempo debe esperar la aplicación? Al menos dos fotogramas, ya que esto permitirá aprovechar al máximo el paralelismo entre la CPU y la GPU. La manera en que funciona la GPU hace que, mientras la aplicación está procesando el fotograma N mediante el envío de llamadas al búfer de comandos, la GPU está ocupada ejecutando las llamadas del fotograma anterior, N-1.

Por lo tanto, si una aplicación quiere asignar un recurso que se origina en la memoria de vídeo y se copia un recurso en el fotograma N, esta llamada realmente comenzará a ejecutarse en el fotograma N+1, cuando la aplicación esté enviando llamadas para el siguiente fotograma. La copia debería haber finalizado cuando la aplicación esté procesando el fotograma N+2.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Fotograma</th>
<th align="left">Estado de la GPU/CPU</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">N</td>
<td align="left"><ul>
<li>La CPU emite llamadas de representación para el fotograma actual.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+1</td>
<td align="left"><ul>
<li>La GPU ejecuta las llamadas enviadas desde la CPU durante el fotograma N.</li>
<li>La CPU emite llamadas de representación para el fotograma actual.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+2</td>
<td align="left"><ul>
<li>La GPU terminó de ejecutar las llamadas enviadas desde la CPU durante el fotograma N. Los resultados están listos.</li>
<li>La GPU ejecuta las llamadas enviadas desde la CPU durante el fotograma N+1.</li>
<li>La CPU emite llamadas de representación para el fotograma actual.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">N+3</td>
<td align="left"><ul>
<li>La GPU terminó de ejecutar las llamadas enviadas desde la CPU durante el fotograma N+1. Los resultados están listos.</li>
<li>La GPU ejecuta las llamadas enviadas desde la CPU durante el fotograma N+2.</li>
<li>La CPU emite llamadas de representación para el fotograma actual.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">N+4</td>
<td align="left">...</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos](resources.md)

 

 




