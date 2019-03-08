---
title: Canalización de gráficos
description: La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Canalización de gráficos
- Fases de canalización
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 55621cec768e0aac680c3a84fd803e591459a97d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605440"
---
# <a name="graphics-pipeline"></a>Canalización de gráficos


La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.

Todas las fases pueden configurarse mediante la API de Direct3D. Las fases que ofrecen núcleos de sombreador comunes (bloques rectangulares redondeados) pueden programarse mediante el lenguaje de programación [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Esto hace que la canalización sea muy flexible y adaptable.

La más utilizadas son la fase del sombreador de vértices (VS) y la fase del sombreador de píxeles (PS). Si no proporcionas estas fases de sombreador, se usa el sombreador de píxeles y un vértice de paso a través no operativo predeterminado.

![diagrama del flujo de datos en la canalización programable de Direct3D 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Fase del ensamblador de entrada

|-|-| |Propósito|La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) proporciona datos primitivos y de adyacencia a la canalización, como triángulos, líneas y puntos, que incluyen los id. de semántica para reducir el procesamiento de primitivos que todavía no se procesaron con el fin de que los sombreadores sean más eficientes.| |Entrada|Datos primitivos (triángulos, líneas y/o puntos) de los búferes rellenados por el usuario en la memoria. Y posiblemente datos de adyacencia. Un triángulo tendría 3 vértices para cada triángulo y, posiblemente, 3 vértices para los datos de adyacencia de cada triángulo.| |Salida| Primitivos con valores adjuntos generados por el sistema (como un id. de primitivo, un id. de instancia o un id. de vértice).|

## <a name="vertex-shader-stage"></a>Fase del sombreador de vértices

|-|-| |Propósito|La [fase del sombreador de vértices (VS)](vertex-shader-stage--vs-.md) procesa vértices, normalmente realizando ciertas operaciones, como transformar, enmascarar e iluminar. Un sombreador de vértices toma un solo vértice de entrada y produce un solo vértice de salida. Operaciones por vértice individuales, como transformaciones, enmascaramientos e iluminaciones por vértice.| |Entrada|Un solo vértice, con los valores VertexID e InstanceID generados por el sistema. Cada vértice de entrada de sombreador de vértices puede comprender hasta 16 vectores de 32 bits (hasta 4 componentes cada uno).| |Salida|Un solo vértice. Cada vértice de salida puede comprender hasta 16 vectores de 4 componentes de 32 bits.|
 
## <a name="hull-shader-stage"></a>Fase del sombreador de envolvente
 
|-|-| |Propósito|La [fase del sombreador de envolvente (HS)](hull-shader-stage--hs-.md) es una de las fases de teselación, que divide de forma eficaz una sola superficie de un modelo en muchos triángulos. Un sombreador de casco se invoca una vez por revisión y transforma los puntos de control de entrada que definen una superficie de orden inferior en puntos de control que conforman una revisión. También hace algunos cálculos por revisión para proporcionar datos para la fase del teselador (TS) y la fase del sombreador de dominios (DS).| |Entrada|Entre 1 y 32 puntos de control de entrada, que definen una superficie de orden inferior.| |Salida| Entre 1 y 32 puntos de control de salida, que juntos forman una revisión. El sombreador de envolvente declara el estado de la fase del teselador (TS), incluido el número de puntos de control, el tipo de la cara de revisión y el tipo de partición que se usa al realizar la teselación.|

## <a name="tessellator-stage"></a>Fase del teselador

|-|-| |Propósito|La [fase del teselador (TS)](tessellator-stage--ts-.md) crea un patrón de muestreo del dominio que representa la revisión de geometría y genera un conjunto de objetos de menor tamaño (triángulos, líneas o puntos) que conectan estas muestras.| |Entrada|El teselador opera una vez por revisión con los factores de teselación (que especifican con qué precisión se teselará el dominio) y el tipo de partición (que especifica el algoritmo usado para dividir una revisión) que se pasan desde la fase del sombreador de envolvente. | |Salida|El teselador devuelve las coordenadas UV (y, opcionalmente, W) y la topología de superficie para la fase del sombreador de dominios.|

## <a name="domain-shader-stage"></a>Fase del sombreador de dominios

|-|-| |Propósito|La [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md) calcula la posición del vértice de un punto subdividido en la revisión de salida; calcula la posición del vértice correspondiente a cada muestra de dominio. Un sombreador de dominios se ejecuta una vez por punto de salida de la fase del teselador y tiene acceso de solo lectura a las constantes de revisión de salida y de revisión de salida del sombreador de envolvente, y a las coordenadas UV de salida de la fase del teselador.| |Entrada| Un sombreador de dominios consume puntos de control de salida de la [fase del sombreador de envolvente (HS)](hull-shader-stage--hs-.md). Entre los resultados del sombreador de casco se incluyen: Factores de teselación (los factores de teselación pueden incluir los valores utilizados por del teselador funciones fijas, así como los valores sin procesar - antes de redondearlo por teselación entero - que facilita geomorphing, por ejemplo, datos constantes de revisión y puntos de control ). Un sombreador de dominios se invoca una vez por coordenada de salida desde la [fase del teselador (TS)](tessellator-stage--ts-.md).| |Salida|La fase del sombreador de dominios (DS) devuelve la posición del vértice de un punto subdividido en la revisión de salida.|

## <a name="geometry-shader-stage"></a>Fase de sombreador de geometría

|-|-| |Propósito|La [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md) procesa primitivos completos: triángulos, líneas y puntos, junto con sus vértices adyacentes. Admite la amplificación y la desamplificación geométrica. Es útil para algoritmos, como Point Sprite Expansion, Dynamic Particle Systems, Fur/Fin Generation, Shadow Volume Generation, Single Pass Render-to-Cubemap, Per-Primitive Material Swapping y Per-Primitive Material Setup, que incluye la generación de coordenadas baricéntricas como datos primitivos, de modo que un sombreador de píxeles pueda realizar la interpolación de atributos personalizada. | |Entrada|A diferencia de los sombreadores de vértices, que trabajan en un solo vértice, las entradas del sombreador de geometría son los vértices para un primitivo completo (tres vértices para triángulos, dos vértices para líneas o un solo vértice para el punto).| |Salida|La fase del sombreador de geometría (GS) es capaz de presentar varios vértices que forman una sola topología seleccionada. Las topologías de salida del sombreador de geometría disponibles son <strong>tristrip</strong>, <strong>linestrip</strong> y <strong>pointlist</strong>. El número de primitivos emitido puede variar libremente dentro de cualquier invocación del sombreador de geometría, aunque el número máximo de vértices que podría emitirse debe declararse de forma estática. Las longitudes de franja emitidas desde una invocación del sombreador de geometría pueden ser arbitrarias, y pueden crearse nuevas franjas a través de la función HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660).|

## <a name="stream-output-stage"></a>Fase de salida de flujo

|-|-| |Propósito|La [fase de salida de flujo (SO)](stream-output-stage--so-.md) devuelve (o transmite) continuamente datos de vértices de la fase activa anterior a uno o más búferes de la memoria. Los datos trasmitidos a la memoria pueden regresar a la canalización como datos de entrada o volver a leerse en la CPU.| |Entrada|Datos de los vértices de una fase de canalización anterior.| |Salida|La fase de salida de flujo (SO) devuelve (o transmite) continuamente datos de vértices de la fase activa anterior, como la fase del sombreador de geometría (GS), a uno o más búferes de la memoria. Si la fase del sombreador de geometría (GS) está inactiva y la fase de salida de flujo (SO) está activa, devuelve continuamente datos de vértices de la fase del sombreador de dominio (DS) a los búferes de la memoria (o si DS también está inactivo, de la fase del sombreador de vértices (VS)).|

## <a name="rasterizer-stage"></a>Fase del rasterizador

|-|-| |Propósito|La [fase del rasterizador (RS)](rasterizer-stage--rs-.md) recorta primitivos que no están en la vista, los prepara para la fase del sombreador de píxeles (PS) y determina cómo invocar sombreadores de píxeles. Convierte la información de vector (que se compone de formas o primitivos) en una imagen de trama (que se compone de píxeles) con el fin de mostrar gráficos 3D en tiempo real.| |Entrada|Se supone que los vértices (x, y, z, w) que se incorporan a la fase de rasterización están en el espacio de recorte homogéneo. En este espacio de coordenadas, el eje X apunta a la derecha, el eje Y apunta arriba y el eje Z, fuera de la cámara.| |Salida|Los píxeles reales que se deben representar. Incluye algunos atributos de vértice que debe usar el sombreador de píxeles en la interpolación.|

## <a name="pixel-shader-stage"></a>Fase del sombreador de píxeles
 
|-|-| |Propósito|La [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) recibe datos interpolados de un primitivo y genera datos por píxel, como el color.| |Entrada|Cuando se configura la canalización sin un sombreador de geometría, el sombreador de píxeles está limitado a 16 entradas de 4 componentes de 32 bits. De lo contrario, un sombreador de píxeles puede aceptar hasta 32 entradas de 4 componentes y 32 bits. Los datos de entrada del sombreador de píxeles incluyen atributos de vértice (que se pueden interpolar con o sin corrección de la perspectiva) o pueden tratarse como constantes por primitivo. Las entradas del sombreador de píxeles se interpolan desde los atributos de vértice del primitivo que se está rasterizando, según el modo de interpolación declarado. Si un primitivo se recorta antes de la rasterización, el modo de interpolación se respeta también durante el proceso de recorte. | |Salida|Un sombreador de píxeles puede producir hasta 8 colores de 4 componentes y 32 bits, o ningún color si el píxel se descarta. Los componentes de registro de salida del sombreador de píxeles deben declararse para que se puedan usar; se permite una máscara de escritura de salida diferente a cada registro.|

## <a name="output-merger-stage"></a>Fase de fusión de salida
 
|-|-| |Propósito|La [fase de fusión de salida (OM)](output-merger-stage--om-.md) combina varios tipos de datos de salida (valores de sombreador de píxeles, información de profundidad y galería de símbolos) con los contenidos del destino de representación y los búferes de profundidad o galería de símbolos para generar el resultado final de la canalización.| |Entrada|Las entradas de fusión de salida son el estado de canalización, los datos de píxeles generados por los sombreadores de píxeles, el contenido de los destinos de representación y el contenido de los búferes de profundidad o galería de símbolos.| |Salida|El color final del píxel representado.|

## <a name="related-topics"></a>Temas relacionados

- [Guía de aprendizaje de gráficos de Direct3D](index.md)

- [Proceso de canalización](compute-pipeline.md)
