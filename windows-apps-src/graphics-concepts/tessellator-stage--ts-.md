---
title: Fase de Tessellator (TS)
description: La fase del teselador (TS) crea un patrón de muestreo del dominio que representa la revisión geométrica y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estos ejemplos.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- Fase de Tessellator (TS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 90dfb8d28be786cb542e72fde5a24bed4de68f78
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167979"
---
# <a name="tessellator-ts-stage"></a>Fase de Tessellator (TS)


La fase del teselador (TS) crea un patrón de muestreo del dominio que representa la revisión geométrica y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estos ejemplos.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Propósito y usos


En el diagrama siguiente se resaltan las fases de la canalización de gráficos Direct3D.

![diagrama de la canalización de Direct3D 11 que resalta las fases de sombreador de casco, del teselador y sombreador de dominio](images/d3d11-pipeline-stages-tessellation.png)

En el diagrama siguiente se muestra la progresión a través de las fases de teselación.

![diagrama de progresión de teselación](images/tess-prog.png)

La progresión comienza con la superficie de subdivisión de menor detalle. La progresión resalta la revisión de entrada con la revisión geométrica correspondiente, ejemplos de dominio y triángulos que conectan estos ejemplos. Finalmente, la progresión resalta los vértices que corresponden a estos ejemplos.

El tiempo de ejecución de Direct3D admite tres fases que implementan Teselación, que convierte las superficies de subdivisiones de bajo detalle en primitivas más detalladas en la GPU. Mosaicos de teselación (o rotura) de superficies de orden superior en estructuras adecuadas para la representación.

Las fases de teselación funcionan juntas para convertir superficies de orden superior (que mantienen el modelo de forma sencilla y eficaz) en muchos triángulos para la representación detallada dentro de la canalización de gráficos de Direct3D.

La teselación usa la GPU para calcular una superficie más detallada de una superficie construida a partir de las cuádruples revisiones, parches de triángulo o isolíneas. Para aproximar la superficie de orden superior, cada revisión se divide en triángulos, puntos o líneas mediante factores de teselación. La canalización de gráficos de Direct3D implementa la teselación mediante tres fases de canalización:

-   [Fase de sombreador de casco (HS)](hull-shader-stage--hs-.md) : una fase de sombreador programable que genera una revisión de geometría (y constantes de revisión) que se corresponden con cada revisión de entrada (cuádruple, triángulo o línea).
-   Fase del teselador (TS): una fase de canalización de función fija que crea un patrón de muestreo del dominio que representa la revisión de geometría y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estos ejemplos.
-   [Fase de sombreador de dominios (DS)](domain-shader-stage--ds-.md) : una fase de sombreador programable que calcula la posición del vértice que corresponde a cada ejemplo de dominio.

Mediante la implementación de teselación en hardware, una canalización de gráficos puede evaluar modelos de menor detalle (recuento de polígonos inferiores) y representar con mayor detalle. Aunque se puede hacer la teselación de software, la teselación implementada por el hardware puede generar una cantidad increíble de detalles visuales (incluida la compatibilidad con la asignación de desplazamiento) sin agregar los detalles visuales a los tamaños del modelo y las tasas de actualización de paralyzing.

Ventajas de la teselación:

-   La teselación ahorra gran cantidad de memoria y ancho de banda, lo que permite que una aplicación represente superficies más detalladas de modelos de baja resolución. La técnica de teselación implementada en la canalización de gráficos de Direct3D también admite la asignación de desplazamiento, que puede generar sorprendentes cantidades de detalles de la superficie.
-   Teselación admite técnicas de representación escalables, como Continuous o View dependent Level-of-detail que se pueden calcular sobre la marcha.
-   La teselación mejora el rendimiento al realizar cálculos caros con una frecuencia menor (realizar cálculos en un modelo de detalles más bajo). Esto podría incluir los cálculos de mezcla mediante formas de mezcla o destinos de transformación para la animación realista o cálculos físicos para la detección de colisiones o la dinámica del cuerpo flexible.

La canalización de gráficos Direct3D implementa la teselación en el hardware, que no carga el trabajo de la CPU a la GPU. Esto puede dar lugar a mejoras de rendimiento muy grandes si una aplicación implementa un gran número de destinos transformativos y/o modelos más sofisticados.

Del teselador es una fase de función fija que se inicializa mediante el enlace de un [sombreador de casco](hull-shader-stage--hs-.md) a la canalización. (vea [Cómo: inicializar la fase del teselador](/windows/desktop/direct3d11/direct3d-11-advanced-stages-tessellator-initialize)). El propósito de la fase del teselador es subdividir un dominio (cuádruple, Tri o line) en muchos objetos más pequeños (triángulos, puntos o líneas). Del teselador coloca en mosaico un dominio canónico en un sistema de coordenadas normalizado (cero a uno). Por ejemplo, un dominio cuádruple se Tesela en un cuadrado de unidad.

### <a name="span-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanspan-idphases_in_the_tessellator__ts__stagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>Fases en la etapa del teselador (TS)

La fase del teselador (TS) funciona en dos fases:

-   En la primera fase se procesan los factores de teselación, se corrigen los problemas de redondeo, se controlan los factores muy pequeños, se reducen y se combinan los factores mediante la aritmética de punto flotante de 32 bits.
-   La segunda fase genera listas de puntos o topologías según el tipo de partición seleccionado. Esta es la tarea principal de la fase del teselador y usa fracciones de 16 bits con aritmética de punto fijo. La aritmética de punto fijo permite la aceleración de hardware manteniendo una precisión aceptable. Por ejemplo, dada una revisión de ancho de 64, esta precisión puede colocar puntos en una resolución de 2 mm.

    | Tipo de creación de particiones | Intervalo                       |
    |----------------------|-----------------------------|
    | Fraccionario \_ impar      | \[1... 63\]                  |
    | Separadores fraccionarios \_     | Intervalo de TessFactor: \[ 2.. 64\] |
    | Entero              | Intervalo de TessFactor: \[ 1.. 64\] |
    | Pow2                 | Intervalo de TessFactor: \[ 1.. 64\] |

     

La teselación se implementa con dos fases del sombreador programable: un [sombreador de casco](hull-shader-stage--hs-.md) y un [sombreador de dominios](domain-shader-stage--ds-.md). Estas fases del sombreador se programan con código HLSL que se define en el modelo de sombreador 5. Los destinos del sombreador son: HS \_ 5 \_ 0 y DS \_ 5 \_ 0. El título crea el sombreador y, a continuación, se extrae el código del hardware de los sombreadores compilados que se pasan al tiempo de ejecución cuando los sombreadores se enlazan a la canalización.

### <a name="span-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanspan-idenabling_disabling_tessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>Habilitar o deshabilitar la teselación

Habilite la teselación creando un sombreador de casco y enlazándolo a la fase del sombreador de casco (Esto configura automáticamente la fase del teselador). Para generar las posiciones finales del vértice a partir de las revisiones teselas, también debe crear un [sombreador de dominios](domain-shader-stage--ds-.md) y enlazarlo a la fase del sombreador del dominio. Una vez habilitada la teselación, la entrada de datos en la etapa del ensamblador de entrada (IA) debe ser datos de revisión. La topología del ensamblador de entrada debe ser una topología de constantes de revisión.

Para deshabilitar la teselación, establezca el sombreador de casco y el sombreador de dominios en **null**. Ni la fase del [sombreador de geometría (GS)](geometry-shader-stage--gs-.md) ni la de [salida de flujo (SO)](stream-output-stage--so-.md) pueden leer puntos de control de salida del sombreador de casco o datos de revisión.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entradas


Del teselador funciona una vez por revisión mediante los factores de teselación (que especifican el grado de teselación del dominio) y el tipo de creación de particiones (que especifica el algoritmo que se usa para segmentar una revisión) que se pasan desde la fase del sombreador de casco.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Genere


Del teselador genera coordenadas UV (y, opcionalmente, w) y la topología de superficie en la etapa del sombreador de dominio.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 