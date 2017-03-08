---
title: Uso de valores generados por el sistema
description: "La fase del ensamblador de entrada (IA) genera valores generados por el sistema (basándose en la semántica de entrada proporcionada por el usuario) para admitir determinadas eficiencias en las operaciones del sombreador."
ms.assetid: C7CBA81D-68CA-4E9A-95E3-8185C280C843
keywords:
- Uso de valores generados por el sistema
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 41a2fca9daf288f4c25f10500e332d2983e6a62d
ms.lasthandoff: 02/07/2017

---

# <a name="span-iddirect3dconceptsusingsystem-generatedvaluesspanusing-system-generated-values"></a><span id="direct3dconcepts.using_system-generated_values"></span>Uso de valores generados por el sistema


La [fase del ensamblador de entrada (IA](input-assembler-stage--ia-.md) genera valores generados por el sistema (basándose en la [semantics](https://msdn.microsoft.com/library/windows/desktop/bb509647) de entrada proporcionada por el usuario) para admitir determinadas eficiencias en las operaciones del sombreador. Al adjuntar datos, como un id. de instancia (visible para la [fase del sombreador de vértices (VS)](vertex-shader-stage--vs-.md)), un id. de vértice (visible para VS) o un id. de primitivo (visible para la [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md)/[fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md)), una fase del sombreador posterior puede ver estos valores del sistema para optimizar el procesamiento en esa fase.

Por ejemplo, la fase VS puede buscar el identificador de instancia para obtener datos adicionales por vértice para el sombreador o para realizar otras operaciones; por ejemplo, las fases GS y PS pueden usar el identificador de primitivo para obtener datos de cada primitivo de la misma manera.

## <a name="span-idvertexidspanspan-idvertexidspanspan-idvertexidspanvertexid"></a><span id="VertexID"></span><span id="vertexid"></span><span id="VERTEXID"></span>VertexID


En cada fase del sombreado se usa un identificador de vértice para identificar cada vértice. Es un número entero sin signo de 32 bits cuyo valor predeterminado es 0. Se asigna a un vértice cuando el primitivo se procesa mediante la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md). Adjunta la semántica de identificador de vértices a la declaración de entrada de sombreador para informar a la fase de IA de que debe generar un identificador por vértice.

El IA agrega un identificador de vértice a cada vértice para que lo usen las fases de sombreador. Para cada llamada de dibujo, el identificador de vértice se incrementa en 1. En las llamadas de dibujo indexadas, el recuento se restablece en el valor de inicio. Si el identificador de vértice se desborda (supera 2³²– 1), se ajusta a 0.

Para todos los tipos primitivos, los vértices tienen un identificador de vértice asociado a ellos (independientemente de la proximidad).

## <a name="span-idprimitiveidspanspan-idprimitiveidspanspan-idprimitiveidspanprimitiveid"></a><span id="PrimitiveID"></span><span id="primitiveid"></span><span id="PRIMITIVEID"></span>PrimitiveID


En cada fase del sombreado se usa un identificador de primitivo para identificar cada primitivo. Es un número entero sin signo de 32 bits cuyo valor predeterminado es 0. Se asigna a un primitivo cuando el primitivo se procesa mediante la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md). Para informar a la fase de IA de que debe generar un identificador de primitivo, adjunta la semántica de identificador de primitivos a la declaración de entrada de sombreador.

La fase de IA agregará un identificador de primitivo a cada primitivo para su uso por la [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md) o la [fase del sombreador de vértices (VS)](vertex-shader-stage--vs-.md) (cualquiera que sea la primera fase activa después de la fase de IA). Para cada llamada de dibujo indexada, el identificador de primitivo se incrementa en 1; sin embargo, se restablecerá el identificador de primitivo en 0 siempre que empiece una nueva instancia. Toaos las demás llamadas de dibujos no cambian el valor del identificador de instancia. Si el identificador de instancia se desborda (supera 2³²– 1), se ajusta a 0.

La [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) no tiene una entrada independiente para un identificador de primitivo; sin embargo, cualquier sombreador de píxeles de entrada que especifique un identificador de primitivo usa un modo de interpolación constante.

No hay ninguna compatibilidad para generar automáticamente un identificador de primitivo para primitivos adyacentes. Para los tipos primitivos con proximidad, como una franja de triángulos con proximidad, un identificador de primitivo se mantiene solo para los primitivos interiores (los primitivos no adyacentes), igual que el conjunto de primitivos en una franja de triángulos sin proximidad.

## <a name="span-idinstanceidspanspan-idinstanceidspanspan-idinstanceidspaninstanceid"></a><span id="InstanceID"></span><span id="instanceid"></span><span id="INSTANCEID"></span>InstanceID


Un identificador de instancia se usa en cada fase del sombreador para identificar la instancia de la geometría que se está procesando. Es un número entero sin signo de 32 bits cuyo valor predeterminado es 0.

La [fase de ensamblador de entrada (IA)](input-assembler-stage--ia-.md) agregará un identificador de instancia a cada vértice si la declaración de entrada del sombreador de vértices incluye la semántica del identificador de instancia. Para cada llamada de dibujo indexada, el identificador de instancia se incrementa en 1. Toaos las demás llamadas de dibujos no cambian el valor del identificador de instancia. Si el identificador de instancia se desborda (supera 2³²– 1), se ajusta a 0.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Por ejemplo:


La siguiente ilustración muestra cómo adjuntar valores del sistema a una franja de triángulos de instancias en la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md).

![ilustración de valores del sistema para una franja de triángulos de instancias](images/d3d10-ia-example.png)

Estas tablas muestran los valores del sistema generados para las dos instancias de la misma franja de triángulos. La primera instancia (instancia U) se muestra en azul, la segunda instancia (instancia V) se muestra en verde. Las líneas sólidas conectan los vértices en los primitivos, las líneas de guiones conectan los vértices adyacentes.

Las siguientes tablas muestran los valores generados por el sistema para la instancia U.

| Datos de vértices    | C,U | D,U | E,U | F, U | G,U | H, U | I,U | J,U | K,U | L,U |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |

 

La instancia de la franja de triángulos U tiene 3 primitivos de triángulo, con los siguientes valores generados por el sistema:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 0   | 0   | 0   |

 

Las siguientes tablas muestran los valores generados por el sistema para la instancia V.

| Datos de vértices    | C,V | D,V | E,V | F,V | G, V | H,V | I,V | J,V | K,V | L,V |
|----------------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| **VertexID**   | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |
| **InstanceID** | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   | 1   |

 

La instancia de la franja de triángulos V tiene 3 primitivos de triángulo, con los siguientes valores generados por el sistema:

|                 |     |     |     |
|-----------------|-----|-----|-----|
| **PrimitiveID** | 0   | 1   | 2   |
| **InstanceID**  | 1   | 1   | 1   |

 

La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) genera los identificadores (vértices, primitivo e instancia); ten en cuenta también que cada instancia recibe un identificador de instancia único. Los datos se acaban con el corte de franja, que separa cada instancia de la franja de triángulos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md)

 

 





