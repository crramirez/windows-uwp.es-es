---
title: Fase del ensamblador de entrada (IA)
description: La fase del ensamblador de entrada (IA) proporciona datos primitivos y de adyacen a la canalización, como triángulos, líneas y puntos, incluidos los identificadores semánticos para mejorar la eficacia de los sombreadores reduciendo el procesamiento a primitivos que aún no se han procesado.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- Fase del ensamblador de entrada (IA)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 12a7c7ebd250fec8d944c4cba467a92ff67bd33d
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219798"
---
# <a name="input-assembler-ia-stage"></a>Fase del ensamblador de entrada (IA)


La fase del ensamblador de entrada (IA) proporciona datos primitivos y de adyacen a la canalización, como triángulos, líneas y puntos, incluidos los identificadores semánticos para mejorar la eficacia de los sombreadores reduciendo el procesamiento a primitivos que aún no se han procesado.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Propósito y usos


El propósito de la fase del ensamblador de entrada (IA) es leer los datos primitivos (puntos, líneas y triángulos) de los búferes rellenados por el usuario y ensamblar los datos en primitivos que usarán las otras fases de canalización, y para adjuntar [valores generados](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) por el sistema para ayudar a que los sombreadores sean más eficaces. Los valores generados por el sistema son cadenas de texto que también se denominan semánticas. Las fases del sombreador programable se construyen a partir de un núcleo de sombreador común, que usa valores generados por el sistema (como un identificador primitivo, un identificador de instancia o un identificador de vértice), de modo que la fase del sombreador puede reducir el procesamiento a solo los primitivos, las instancias o los vértices que aún no se han procesado.

La fase IA puede ensamblar vértices en varios [tipos primitivos](primitive-topologies.md) diferentes (como listas de líneas, tiras de triángulos o primitivas con adyacencias). Los tipos primitivos, como una lista de triángulos con adyacencias y una lista de líneas con adyacencias, admiten la [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md).

La información de adyacencias solo es visible para una aplicación en un sombreador de geometría. Si se invocó un sombreador de geometría con un triángulo que incluye la adyacencia, por ejemplo, los datos de entrada contendrían 3 vértices para cada triángulo y 3 vértices para los datos de adyacencia por triángulo.

Cuando se solicita la fase IA para generar datos de adyacencias, los datos de entrada deben incluir datos de adyacencia. Esto puede requerir que se proporcione un vértice ficticio (formando un triángulo degenerado) o quizás mediante la marcación de uno de los atributos de vértice si el vértice existe o no. Esto también debe detectarse y administrarse mediante un sombreador de geometría, aunque la selección de geometría degenerada se producirá en la fase de rasterizador.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entradas


La fase IA Lee los datos de la memoria: datos primitivos (puntos, líneas o triángulos), de búferes rellenos por el usuario.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Genere


La fase de IA ensambla los datos en primitivos y asocia los valores generados por el sistema, y genera como primitivas que usarán la [fase del sombreador de vértices (vs)](vertex-shader-stage--vs-.md) y, a continuación, otras fases de canalización.

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
<td align="left"><p><a href="primitive-topologies.md">Topologías primitivas</a></p></td>
<td align="left"><p>Direct3D admite varias topologías primitivas, que definen cómo se interpretan y representan los vértices mediante la canalización, como las listas de puntos, las listas de líneas y las franjas de triángulo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">Uso de valores generados por el sistema</a></p></td>
<td align="left"><p>Los valores generados por el sistema se generan mediante la fase del ensamblador de entrada (IA) (según la <a href="/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics">semántica</a>de entrada proporcionada por el usuario) para permitir ciertas eficiencias en las operaciones del sombreador. Al adjuntar datos, como un identificador de instancia (visible para la <a href="vertex-shader-stage--vs-.md">fase del sombreador de vértices (vs)</a>), un identificador de vértice (visible para vs) o un identificador primitivo (visible para la fase del sombreador de píxeles de la <a href="geometry-shader-stage--gs-.md">fase GS)</a>, / <a href="pixel-shader-stage--ps-.md">Pixel Shader (PS) stage</a>una etapa del sombreador posterior puede buscar estos valores del sistema para optimizar el procesamiento en esa fase.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 