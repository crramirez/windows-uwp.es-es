---
title: Uso de valores generados por el sistema
description: Los valores generados por el sistema se generan mediante la fase del ensamblador de entrada (IA) (según la semántica de entrada proporcionada por el usuario) para permitir ciertas eficiencias en las operaciones del sombreador.
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- Uso de valores generados por el sistema
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7217b52c6e9f9882997649c5f843eb119d741e0b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156159"
---
# <a name="span-iddirect3dconceptsusing_system-generated_valuesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>Uso de valores generados por el sistema


Los valores generados por el sistema se generan mediante la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) (según la [semántica](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics)de entrada proporcionada por el usuario) para permitir ciertas eficiencias en las operaciones del sombreador. Al adjuntar datos, como un identificador de instancia (visible para la [fase del sombreador de vértices (vs)](vertex-shader-stage--vs-.md)), un identificador de vértice (visible para vs) o un identificador primitivo (visible para la fase del sombreador de píxeles de la [fase GS)](geometry-shader-stage--gs-.md), / [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md)una etapa del sombreador posterior puede buscar estos valores del sistema para optimizar el procesamiento en esa fase.

Por ejemplo, la fase de VS puede buscar el identificador de instancia para captar datos adicionales por vértice para el sombreador o para realizar otras operaciones; las fases GS y PS pueden utilizar el identificador primitivo para capturar datos por primitivos de la misma manera.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


Cada fase del sombreador usa un identificador de vértice para identificar cada vértice. Es un entero de 32 bits sin signo cuyo valor predeterminado es 0. Se asigna a un vértice cuando la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md)procesa la primitiva. Adjunte la semántica del identificador de vértice a la declaración de entrada del sombreador para informar a la fase del IA que genere un identificador por vértice.

El IA agregará un identificador de vértice a cada vértice para que lo usen las fases del sombreador. Para cada llamada a Draw, el identificador de vértice se incrementa en 1. En las llamadas a Draw indizadas, el recuento vuelve a establecer el valor de inicio. Si el identificador de vértice desborda (supera 2 ³ ² – 1), se ajusta a 0.

En todos los tipos primitivos, los vértices tienen un identificador de vértice asociado a ellos (independientemente de la adyacencia).

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


Cada fase del sombreador usa un identificador primitivo para identificar cada primitiva. Es un entero de 32 bits sin signo cuyo valor predeterminado es 0. Se asigna a un primitivo cuando la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md)procesa el primitivo. Para informar a la fase del IA que genere un identificador primitivo, adjunte la semántica de identificador primitivo a la declaración de entrada del sombreador.

La fase IA agregará un identificador primitivo a cada primitivo para su uso en la fase del [sombreador de geometría (GS)](geometry-shader-stage--gs-.md) o en la [fase del sombreador de vértices (vs)](vertex-shader-stage--vs-.md) (lo que sea la primera fase activa después de la fase del IA). En cada llamada a Draw indizada, el identificador primitivo se incrementa en 1; sin embargo, el identificador primitivo se restablece en 0 cada vez que se inicia una nueva instancia. Todas las demás llamadas a Draw no cambian el valor del identificador de instancia. Si el identificador de instancia se desborda (supera 2 ³ ² – 1), se ajusta a 0.

La [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) no tiene una entrada independiente para un identificador primitivo; sin embargo, cualquier entrada del sombreador de píxeles que especifique un identificador primitivo utiliza un modo de interpolación constante.

No se admite la generación automática de un identificador primitivo para primitivos adyacentes. En el caso de los tipos primitivos con adyacencias, como una franja de triángulo con adyacencias, solo se mantiene un identificador primitivo para los primitivos interiores (los primitivos no adyacentes), al igual que el conjunto de primitivas en una franja de triángulo sin adyacencias.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>ID


Cada fase del sombreador usa un identificador de instancia para identificar la instancia de la geometría que se está procesando actualmente. Es un entero de 32 bits sin signo cuyo valor predeterminado es 0.

La [etapa del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) agregará un identificador de instancia a cada vértice si la declaración de entrada del sombreador de vértices incluye la semántica del identificador de instancia. Para cada llamada a Draw indizada, el ID. de instancia se incrementa en 1. Todas las demás llamadas a Draw no cambian el valor de ID. de instancia. Si el identificador de instancia se desborda (supera 2 ³ ² – 1), se ajusta a 0.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


En la siguiente ilustración se muestra cómo se adjuntan los valores del sistema a una franja de triángulo de instancia en la [etapa del ensamblador de entrada (IA)](input-assembler-stage--ia-.md).

![Ilustración de los valores del sistema para una franja de triángulo con instancias](images/d3d10-ia-example.png)

En estas tablas se muestran los valores del sistema generados para ambas instancias de la misma franja de triángulo. La primera instancia (instancia U) se muestra en azul; la segunda instancia (instancia V) se muestra en verde. Las líneas sólidas conectan los vértices de los primitivos, las líneas discontinuas conectan los vértices adyacentes.

En las tablas siguientes se muestran los valores generados por el sistema para la instancia U.

| Datos de vértices    | C, U | D, U | E, U | F, U | G, U | H, U | I, U | J, U | K, U | L, U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

Triangular instancia U tiene tres primitivos triángulos, con los siguientes valores generados por el sistema:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

En las tablas siguientes se muestran los valores generados por el sistema para la instancia V.

| Datos de vértices    | C, V | D, V | E, V | F, V | G, V | H, V | I, V | J, V | K, V | L, V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

La instancia de franja de triángulo V tiene tres primitivas triángulos, con los siguientes valores generados por el sistema:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) genera los identificadores (vértice, primitivo e instancia); Observe también que a cada instancia se le asigna un identificador de instancia único. Los datos finalizan con el corte de franja, que separa cada instancia de la franja de triángulo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md)

 

 