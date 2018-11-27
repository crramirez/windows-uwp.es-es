---
title: Combinación de texturas
description: Direct3D puede combinar hasta ocho texturas en primitivos en un solo pase.
ms.assetid: 9AD388FA-B2B9-44A9-B73E-EDBD7357ACFB
keywords:
- Combinación de texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c40c7d3bd080bd927fc52cb7f740e1dc4a6358c0
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7702626"
---
# <a name="texture-blending"></a>Combinación de texturas


Direct3D puede combinar hasta ocho texturas en primitivos en un solo pase. El uso de la combinación de texturas múltiples puede aumentar profundamente la velocidad de fotogramas de una aplicación de Direct3D. Una aplicación emplea la combinación de texturas múltiples para aplicar texturas, sombras, iluminación especular, iluminación difusa y otros efectos especiales en una solo pase.

Para usar la combinación de texturas, la aplicación debe comprobar antes si el hardware del usuario la admite.

## <a name="span-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespanspan-idtexture-stages-and-the-texture-blending-cascadespantexture-stages-and-the-texture-blending-cascade"></a><span id="Texture-Stages-and-the-Texture-Blending-Cascade"></span><span id="texture-stages-and-the-texture-blending-cascade"></span><span id="TEXTURE-STAGES-AND-THE-TEXTURE-BLENDING-CASCADE"></span>Fases de textura y la cascada de la combinación de texturas


Direct3D admite la combinación de texturas múltiples de un solo paso mediante el uso de fases de textura. Una fase de textura toma dos argumentos y realiza una operación de combinación en ellas, y luego pasa el resultado para su procesamiento posterior o su rasterización. Puedes visualizar una fase de textura como se muestra en el siguiente diagrama.

![Diagrama de una fase de textura](images/texstg.png)

Como se muestra en el diagrama anterior, las fases de la textura combinan dos argumentos mediante el operador especificado. Las operaciones comunes incluyen la modulación simple o la incorporación de los componentes de color o alfa de los argumentos, pero se admiten más de dos docenas de operaciones. Los argumentos de una fase pueden ser una textura asociada, el color o el alfa en iteración (repetidos durante el sombreado Gouraud), un color y un alfa arbitrarios o el resultado de la fase de textura anterior.

**Nota**  Direct3D distingue la combinación de Alfas de color. Las aplicaciones establecen las operaciones y los argumentos de combinación para el color y el alfa individualmente, y los resultados de estos parámetros de configuración son independientes uno del otro.

 

La combinación de argumentos y operaciones usados por varias fases de combinación definen un lenguaje de combinación sencillo basado en el flujo. Los resultados de una fase pasan a la siguiente fase, de esa fase a la siguiente, y así sucesivamente. El concepto de resultados que pasan de una fase a otra para finalmente rasterizarse en un polígono se suele denominar "cascada de combinación de texturas". El siguiente diagrama muestra cómo las diversas fases individuales conforman la cascada de combinación de texturas.

![Diagrama de las fases de textura en la cascada de combinación de texturas](images/tcascade.png)

Cada fase de un dispositivo tiene un índice basado en cero. Direct3D permite hasta ocho fases combinación, aunque siempre se debe comprobar las funcionalidades del dispositivo para determinar cuántas fases admite el hardware actual. La primera fase de combinación está en el índice 0, la segunda en el 1, y así sucesivamente, hasta el índice 7. El sistema combina las fases en orden ascendente de índice.

Usa únicamente la cantidad de fases que necesites; las fases de combinación no utilizadas se deshabilitan de manera predeterminada. Por lo tanto, si la aplicación solo usa las dos primeras fases, únicamente necesita establecer operaciones y los argumentos para las fases 0 y 1. El sistema combina las dos fases y omite las fases deshabilitadas.

Si la aplicación varía la cantidad de fases que usa para diferentes situaciones (como cuatro fases para algunos objetos y solo dos para otras), no es necesario deshabilitar explícitamente todas las fases utilizadas anteriormente. Una opción es deshabilitar la operación de color para la primera fase no utilizada, lo que hará que no se aplique ninguna fase con un índice mayor. Otra opción consiste en deshabilitar la asignación de texturas por completo, para lo cual hay que establecer la operación de color para la primera fase de textura (fase 0) en un estado deshabilitado.

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
<td align="left"><p><a href="blending-stages.md">Fases de combinación</a></p></td>
<td align="left"><p>Una fase de fusión es un conjunto de operaciones de textura y sus argumentos que definen la forma en que se combinan las texturas.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multipass-texture-blending.md">Combinación de texturas multipase</a></p></td>
<td align="left"><p>Las aplicaciones de Direct3D pueden lograr numerosos efectos especiales mediante la aplicación de varias texturas a un primitivo a lo largo de varios pases de representación. El término habitual para esto es <em>combinación de texturas multipase</em>. Un uso habitual de la combinación de texturas multipase es emular los efectos de modelos complejos de iluminación y sombreado con la aplicación de varios colores de distintas texturas diferentes. Esto se denomina <em>mapa de luces</em>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




