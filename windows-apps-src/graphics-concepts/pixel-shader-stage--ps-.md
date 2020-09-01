---
title: Fase del sombreador de píxeles (PS)
description: La fase del sombreador de píxeles (PS) recibe datos interpolados para un primitivo y genera datos por píxel, como el color.
ms.assetid: 0AEBFDFB-0AD8-4633-AE4E-A44004B57745
keywords:
- Fase del sombreador de píxeles (PS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d35fa06ac415b1d4197c1b1bcbf101f1d4cf9b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156289"
---
# <a name="pixel-shader-ps-stage"></a>Fase del sombreador de píxeles (PS)


La fase del sombreador de píxeles (PS) recibe datos interpolados para un primitivo y genera datos por píxel, como el color.

Se trata de una fase de sombreador programable; se muestra como un bloque redondeado en el diagrama de [canalización de gráficos](graphics-pipeline.md) . Esta fase del sombreador expone su propia funcionalidad única, basada en el modelo de sombreador 4,0 [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core).

La fase del sombreador de píxeles (PS) permite técnicas de sombreado enriquecidas, como la iluminación por píxel y el procesamiento posterior. Un sombreador de píxeles es un programa que combina variables constantes, datos de textura, valores interpolados por vértice y otros datos para generar salidas por píxel. La [fase de rasterizador (RS)](rasterizer-stage--rs-.md) invoca un sombreador de píxeles una vez para cada píxel incluido en una primitiva; sin embargo, es posible especificar un sombreador **nulo** para evitar la ejecución de un sombreador.

Cuando se realiza un muestreo múltiple de una textura, se invoca un sombreador de píxeles una vez por cada uno de ellos, mientras que una prueba de profundidad/estarcido se produce para cada Multimuestra. Los ejemplos que superan la prueba de profundidad/estarcido se actualizan con el color de salida del sombreador de píxeles.

Las funciones intrínsecas del sombreador de píxeles producen o utilizan derivados de cantidades con respecto al espacio de pantalla x e y. El uso más común de los derivados es calcular los cálculos de nivel de detalle para el muestreo de textura y, en el caso del filtrado anisotrópico, seleccionar muestras a lo largo del eje de anisotropía. Normalmente, una implementación de hardware ejecuta un sombreador de píxeles en varios píxeles (por ejemplo, una cuadrícula de 2x2) simultáneamente, de modo que los derivados de cantidades calculadas en el sombreador de píxeles se pueden aproximar razonablemente como deltas de los valores en el mismo punto de ejecución en píxeles adyacentes.

## <a name="span-idinputsspanspan-idinputsspanspan-idinputsspaninputs"></a><span id="Inputs"></span><span id="inputs"></span><span id="INPUTS"></span>Comentarios


Cuando la canalización se configura sin un sombreador de geometría, un sombreador de píxeles se limita a 16, 32 bits, 4-entradas de componentes. De lo contrario, un sombreador de píxeles puede tardar hasta 32, 32 bits, 4-entradas de componente.

Los datos de entrada del sombreador de píxeles incluyen atributos de vértices (que se pueden interpolar con o sin corrección de perspectiva) o se pueden tratar como constantes por primitiva. Las entradas del sombreador de píxeles se interpolan a partir de los atributos de vértice de la primitiva que se está rasterizando, según el modo de interpolación declarado. Si una primitiva se recorta antes de la rasterización, también se respeta el modo de interpolación durante el proceso de recorte.

Los atributos de vértice se interpolan (o evalúan) en las ubicaciones del centro de sombreador de píxeles. Los modos de interpolación de atributo del sombreador de píxeles se declaran en una declaración de registro de entrada, por elemento, en un [argumento](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-function-parameters) o en una [estructura de entrada](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-struct). Los atributos se pueden interpolar linealmente o con el muestreo centroide. Vea la sección "muestreo de centroide de atributos cuando el suavizado de contorno de muestreo múltiple" en [reglas de rasterización](rasterization-rules.md). La evaluación de centroide solo es relevante durante el muestreo múltiple para cubrir los casos en los que un píxel está cubierto por un primitivo, pero un centro de píxeles puede no ser; la evaluación de centroide se produce lo más cerca posible del centro de píxeles (no incluido).

Las entradas también se pueden declarar con una [semántica de valor del sistema](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics), que marca un parámetro consumido por otras fases de canalización. Por ejemplo, una posición en píxeles se debe marcar con la \_ semántica de la posición VP. La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) puede generar un valor escalar para un sombreador de píxeles (mediante \_ la VP PrimitiveID); la [fase de rasterizador (RS)](rasterizer-stage--rs-.md) también puede generar un escalar para un sombreador de píxeles (con la VP \_ IsFrontFace).

## <a name="span-idoutputsspanspan-idoutputsspanspan-idoutputsspanoutputs"></a><span id="Outputs"></span><span id="outputs"></span><span id="OUTPUTS"></span>Salidas


Un sombreador de píxeles puede generar un resultado de hasta 8, 32 bits, colores de cuatro componentes o ningún color si se descarta el píxel. Los componentes del registro de salida del sombreador de píxeles se deben declarar antes de poder usarlos. cada registro tiene permitida una máscara de salida-escritura distinta.

Use el estado de profundidad-escritura-habilitar (en la [fase de combinación de salida (OM)](output-merger-stage--om-.md)) para controlar si los datos de profundidad se escriben en un búfer de profundidad (o use la instrucción de descarte para descartar los datos de ese píxel). Un sombreador de píxeles también puede generar un valor de profundidad de punto flotante y de un componente de 32 bits opcional para las pruebas de profundidad (con la semántica de profundidad de la VP \_ ). El valor de profundidad se genera en el registro oDepth y reemplaza el valor de profundidad interpolada para las pruebas de profundidad (suponiendo que la prueba de profundidad está habilitada). No hay ninguna manera de cambiar de forma dinámica entre el uso de la profundidad de funciones fijas o del sombreador oDepth.

Un sombreador de píxeles no puede generar un valor de estarcido.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 