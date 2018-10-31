---
title: Fase de Tessellator (TS)
description: La fase de Tessellator (TS) crea un patrón de muestreo del dominio que representa la revisión de geometría y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estas muestras.
ms.assetid: 2F006F3D-5A04-4B3F-8BC7-55567EFCFA6C
keywords:
- Fase de Tessellator (TS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d57c60e8cba9be75e936c55800bac93f8df3e30
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5835006"
---
# <a name="tessellator-ts-stage"></a>Fase de Tessellator (TS)


La fase de Tessellator (TS) crea un patrón de muestreo del dominio que representa la revisión de geometría y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estas muestras.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidad y usos


El siguiente diagrama resalta las fases de la canalización de gráficos de Direct3D.

![Diagrama de la canalización de Direct3D 11 que destaca las fases de sombreador de casco, Tessellator y sombreador de dominios](images/d3d11-pipeline-stages-tessellation.png)

El siguiente diagrama muestra el progreso a través de las fases de la teselación.

![Diagrama de progreso de la teselación](images/tess-prog.png)

El progreso comienza con la superficie de subdivisión de bajo nivel de detalle. A continuación, el progreso destaca la revisión de entrada con la revisión de geometría correspondiente, las muestras de dominio y los triángulos que conectan estos ejemplos. Por último, el progreso destaca los vértices que corresponden a estas muestras.

El tiempo de ejecución de Direct3D admite tres fases que implementan la teselación, lo que convierte las superficies de subdivisión de bajo nivel de detalle en primitivos de mayor nivel de detalle en la GPU. La teselación descompone (o desglosa) las superficies de orden superior en estructuras adecuadas para la representación.

En conjunto, las fases de teselación convierten las superficies de orden superior (que permiten que el modelo sea sencillo y eficaz) en muchos triángulos para la representación detallada dentro de la canalización de gráficos de Direct3D.

La teselación usa la GPU para calcular una superficie más detallada a partir de una superficie construida desde revisiones de cuadrados, revisiones de triángulos o isolíneas. Para aproximarse a la superficie de orden superior, cada revisión se subdivide en triángulos, puntos o líneas mediante factores de teselación. La canalización de gráficos de Direct3D implementa la teselación mediante tres fases de canalización:

-   [Fase del sombreador de casco (HS)](hull-shader-stage--hs-.md): Una fase del sombreador programable que produce una revisión de la geometría (y constantes de revisión) que se corresponde con cada revisión de entrada (cuadrado, triángulo o línea).
-   Fase de Tessellator (TS): Una fase de canalización de función fija que crea un patrón de muestreo del dominio que representa la revisión de geometría y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estas muestras.
-   [Fase del sombreador de dominio (DS)](domain-shader-stage--ds-.md): Una fase del sombreador programable que calcula la posición del vértice correspondiente a cada muestra del dominio.

Al implementar la teselación en el hardware, una canalización de gráficos puede evaluar modelos de menor nivel de detalle (con un recuento de polígonos más bajo) y representar con mayor detalle. Aunque es posible realizar teselación de software, la teselación implementada por hardware puede generar una cantidad increíble de detalles visuales (incluida la compatibilidad con la asignación de desplazamientos) sin agregar dichos detalles visuales a los tamaños de modelo y paralizar las frecuencias de actualización.

Ventajas de la teselación:

-   La teselación ahorra una gran cantidad de memoria y de ancho de banda, lo que permite que una aplicación represente superficies con mayor detalle a partir de modelos de baja resolución. La técnica de teselación implementada en la canalización de gráficos de Direct3D también admite la asignación de desplazamientos, lo que puede producir unas cantidades sorprendentes de detalles de la superficie.
-   La teselación admite técnicas de representación escalable, como niveles de detalle continuos o dependientes de la vista que se pueden calcular sobre la marcha.
-   La teselación mejora el rendimiento ya que realiza cálculos costosos con menor frecuencia (realiza los cálculos en un modelo de menor nivel de detalle). Esto podría incluir los cálculos de fusión mediante el uso de formas de fusión o destinos de morfología para los cálculos de animaciones o física realistas para la detección de colisiones o la dinámica de cuerpos blandos.

La canalización de gráficos de Direct3D implementa la teselación en el hardware, lo que descarga el trabajo de la CPU a la GPU. Esto puede provocar enormes mejoras del rendimiento si una aplicación implementa un gran número de destinos de morfología o modelos de enmascaramiento/deformación más sofisticados.

El Tessellator es una etapa de función fija que se inicializa mediante el enlace de un [sombreador de casco](hull-shader-stage--hs-.md) a la canalización. (Consulta [Procedimientos: Inicializar la fase de Tessellator](https://msdn.microsoft.com/library/windows/desktop/ff476341)). El propósito de la fase de Tessellator es subdividir un dominio (cuadrado, triángulo o línea) en muchos objetos más pequeños (triángulos, puntos o líneas). El Tessellator descompone un dominio canónico en un sistema de coordenadas normalizado (de cero a uno). Por ejemplo, un dominio cuadrado se tesela en un cuadrado de la unidad.

### <a name="span-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanspan-idphasesinthetessellatortsstagespanphases-in-the-tessellator-ts-stage"></a><span id="Phases_in_the_Tessellator__TS__stage"></span><span id="phases_in_the_tessellator__ts__stage"></span><span id="PHASES_IN_THE_TESSELLATOR__TS__STAGE"></span>Fases de la fase de Tessellator (TS)

La fase de Tessellator (TS) funciona en dos fases:

-   La primera fase procesa los factores de teselación, para lo que soluciona problemas de redondeo, administra factores muy pequeños, reduce y combina factores y usa la aritmética de punto flotante de 32bits.
-   La segunda fase genera listas de puntos o topología en función del tipo de partición seleccionado. Esta es la tarea principal del escenario del Tessellator y usa fracciones de 16bits con aritmética de punto fijo. La aritmética de punto fijo permite la aceleración de hardware a la vez que se mantiene una precisión aceptable. Por ejemplo, dada una revisión ancho de 64metros, esta precisión puede colocar puntos con una resolución de 2mm.

    | Tipo de particiones | Intervalo                       |
    |----------------------|-----------------------------|
    | Fractional\_odd      | \[1...63\]                  |
    | Fractional\_even     | Intervalo de TessFactor: \[2..64\] |
    | Integer              | Intervalo de TessFactor: \[1..64\] |
    | Pow2                 | Intervalo de TessFactor: \[1..64\] |

     

La teselación se implementa con dos fases de sombreadores programables: un [sombreador de casco](hull-shader-stage--hs-.md) y un [sombreador de dominios](domain-shader-stage--ds-.md). Estas fases de sombreador se programan con código HLSL, que se define en el modelo de sombreador 5. Los destinos de sombreador son: hs\_5\_0 y ds\_5\_0. El título crea el sombreador y luego el código para el hardware se extrae de los sombreadores compilados pasados al tiempo de ejecución cuando los sombreadores se enlazan a la canalización.

### <a name="span-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanspan-idenablingdisablingtessellationspanenablingdisabling-tessellation"></a><span id="Enabling_disabling_tessellation"></span><span id="enabling_disabling_tessellation"></span><span id="ENABLING_DISABLING_TESSELLATION"></span>Habilitar o deshabilitar la teselación

Para habilitar la teselación, crea un sombreador de casco y enlázalo a la fase del sombreador de casco (esto configura automáticamente la fase de Tessellator). Para generar las posiciones finales de los vértices a partir de las revisiones teseladas, también debes crear un [sombreador de dominios](domain-shader-stage--ds-.md) y enlazarlo a la fase del sombreador de dominios. Una vez habilitada la teselación, la entrada de datos en la fase del ensamblador de entrada (IA) deben ser datos de revisión. La topología de ensamblador de entrada debe ser una topología constante de revisión.

Para deshabilitar la teselación, establece el sombreador de casco y el sombreador de dominios en **NULL**. Ni la [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md) ni la [fase de salida de flujo (SO)](stream-output-stage--so-.md) pueden leer puntos de control de salida del sombreador de casco ni datos de revisión.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


El Tessellator funciona una vez por revisión usando los factores de teselación (que especifican la precisión de teselado del dominio) y el tipo de partición (que especifica el algoritmo usado para dividir una revisión), que se pasan desde la fase del sombreador de casco.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Resultados


El Tessellator envía coordenadas uv (y, opcionalmente, w) y la topología de superficie a la fase del sombreador de dominios.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




