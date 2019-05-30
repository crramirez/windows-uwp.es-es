---
title: Fase del sombreador de píxeles (PS)
description: La fase del sombreador de píxeles (PS) recibe datos interpolados de un primitivo y genera datos por píxel, como el color.
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- Fase del sombreador de píxeles (PS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 81b2bc5e78087b19d8829df4dab4b03e4db76467
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370987"
---
# <a name="pixel-shader-ps-stage"></a>Fase del sombreador de píxeles (PS)


La fase del sombreador de píxeles (PS) recibe datos interpolados de un primitivo y genera datos por píxel, como el color.

Se trata de una fase del sombreador programable; se muestra como un bloque redondeado en el diagrama de [canalización de gráficos](graphics-pipeline.md). Esta fase del sombreador expone su propia funcionalidad única, integrada en Shader Model 4.0 [Common-Shader Core](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core).

La fase del sombreador de píxeles (PS) habilita completas técnicas de sombreado, como el posprocesamiento y la iluminación por píxel. Un sombreador de píxeles es un programa que combina variables de constantes, datos de textura, valores interpolados por vértice y otros datos para producir salidas por píxel. La [fase del rasterizador (RS)](rasterizer-stage--rs-.md) invoca un sombreador de píxeles una vez para cada píxel incluido en un primitivo, aunque se puede especificar un sombreador **NULL** para impedir que se ejecute un sombreador.

Al aplicar el muestreo múltiple a una textura, un sombreador de píxeles se invoca una vez por píxel incluido mientras se realiza una prueba de profundidad o galería de símbolos para cada muestra múltiple incluida. Las muestras que pasan la prueba de profundidad o galería de símbolos se actualizan con el color de salida del sombreador de píxeles.

Las funciones intrínsecas del sombreador de píxeles generan o usan derivados de cantidades con respecto al espacio de pantalla x e y. El uso más común de los derivados consiste en realizar cálculos de nivel de detalle para el muestreo de texturas y, en el caso del filtrado anisotrópico, en seleccionar muestras a lo largo del eje de anisotropía. Por lo general, una implementación de hardware ejecuta un sombreador de píxeles en varios píxeles (por ejemplo, una cuadrícula de 2 x 2) al mismo tiempo, de modo que los derivados de cantidades que se han calculado en el sombreador de píxeles pueden aproximarse de manera razonable como deltas de los valores en el mismo punto de ejecución en píxeles adyacentes.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>Entradas


Cuando se configura la canalización sin un sombreador de geometría, un sombreador de píxeles se limita a las entradas de 4 componentes de 16 y 32 bits. De lo contrario, un sombreador de píxeles puede aceptar hasta 32 entradas de 4 componentes y 32 bits.

Los datos de entrada del sombreador de píxeles incluyen atributos de vértice (que se pueden interpolar con o sin corrección de la perspectiva) o pueden tratarse como constantes por primitivo. Las entradas del sombreador de píxeles se interpolan desde los atributos de vértice del primitivo que se está rasterizando, según el modo de interpolación declarado. Si un primitivo se recorta antes de la rasterización, el modo de interpolación se respeta también durante el proceso de recorte.

Los atributos de vértice se interpolan (o evalúan) en las ubicaciones del centro del sombreador de píxeles. Los modos de interpolación de atributos del sombreador de píxeles se declaran en una declaración del registro de entrada, por elemento, ya sea en un [argumento](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-function-parameters) o una [estructura de entrada](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-struct). Los atributos se pueden interpolar de forma lineal o con el muestreo de centroide. Consulta la sección "Muestreo centroide de atributos durante el suavizado de contorno multimuestra" en [Reglas de rasterización](rasterization-rules.md). La evaluación centroide es importante solamente durante el muestreo múltiple para resolver casos donde un píxel está incluido en un tipo, pero un centro de píxel podría no estarlo; la evaluación centroide se produce lo más cerca posible del centro de píxeles (no incluido).

Las entradas también se pueden declarar con una [semántica de valor del sistema](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics), que marca un parámetro que se utiliza en otras fases de la canalización. Por ejemplo, una posición de píxel debe marcarse con la VP\_la semántica de posición. El [etapa del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) puede generar uno escalar de un sombreador de píxeles (mediante SV\_PrimitiveID); el [etapa del rasterizador (RS)](rasterizer-stage--rs-.md) también se puede generar uno escalar de un sombreador de píxeles (mediante SV\_ IsFrontFace).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>Salidas


Un sombreador de píxeles puede producir hasta 8 colores de 4 componentes y 32 bits, o ningún color si el píxel se descarta. Los componentes de registro de salida del sombreador de píxeles deben declararse para que se puedan utilizar; se permite una máscara de escritura de salida diferente a cada registro.

Usa el estado depth-write-enable [en la [fase de fusión de salida (OM)](output-merger-stage--om-.md)] para controlar si los datos de profundidad se escriben en un búfer de profundidad (o utilice la instrucción discard para descartar los datos de ese píxel). Un sombreador de píxeles puede generar también un valor de profundidad de 32 bits, componente de 1, punto flotante, opcional para las pruebas de profundidad (mediante la VP\_profundidad semántica). El valor de profundidad es genera en el registro de oDepth y reemplaza el valor de profundidad interpolado de las pruebas de profundidad (suponiendo que las pruebas de profundidad estén habilitadas). No hay ninguna manera de cambiar dinámicamente entre utilizar el registro oDepth del sombreador o la profundidad de función fija.

Un sombreador de píxeles no puede generar un valor de la galería de símbolos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




