---
title: Canalización de gráficos
description: La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Canalización de gráficos
- Fases de canalización
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6cb50af5facdbf4271c9911f0e1f536dead0b8b8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044914"
---
# <a name="graphics-pipeline"></a>Canalización de gráficos


La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.

Todas las fases pueden configurarse mediante la API de Direct3D. Las fases que ofrecen núcleos de sombreador comunes (bloques rectangulares redondeados) pueden programarse mediante el lenguaje de programación [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Esto hace que la canalización sea muy flexible y adaptable.

La más utilizadas son la fase del sombreador de vértices (VS) y la fase del sombreador de píxeles (PS). Si no proporcionas estas fases de sombreador, se usa el sombreador de píxeles y un vértice de paso a través no operativo predeterminado.

![diagrama del flujo de datos en la canalización programable de Direct3D 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Fase del ensamblador de entrada

|-|-| | Propósito | Los suministros de [entrada Assembler (IA) fase](input-assembler-stage--ia-.md) primitivos y datos de la proximidad a la canalización, como triángulos, líneas y puntos, incluidos semántica identificadores para ayudar a que sea más eficaz mediante la reducción del procesamiento fundamentos que ya no se han sombreados procesar. | | Entrada | Datos primitivo (triángulos, líneas y puntos), de búferes de relleno de usuario en la memoria. Y posiblemente datos de adyacencia. Un triángulo sería 3 vértices para cada triángulo y, posiblemente, 3 vértices para los datos de la proximidad por triángulo. | | Salida | Fundamentos con adjuntos valores generados por el sistema (como un identificador primitivo, un identificador de instancia o un identificador de vértice). |

## <a name="vertex-shader-stage"></a>Fase del sombreador de vértices

|-|-| | Propósito | La [fase de sombreador de vértices (VS)](vertex-shader-stage--vs-.md) procesa los vértices, normalmente realizar operaciones como transformaciones, aplicación de aspectos e iluminación. Un sombreador de vértices toma un solo vértice de entrada y produce un solo vértice de salida. Las operaciones individuales por vértice, como las transformaciones, máscaras, Morphing e iluminación por vértices. | | Entrada | Un solo vértice, con los valores generados por el sistema VertexID y InstanceID. Cada vértice de entrada de sombreador de vértices puede estar formado por un máximo de 16 vectores de 32 bits (hasta 4 componentes cada). | | Salida | Un solo vértice. Cada vértice de salida puede estar formado por 16 como máximo 32-bit 4-componente vectores. |
 
## <a name="hull-shader-stage"></a>Fase del sombreador de casco
 
|-|-| | Propósito | La [fase de casco sombreador de vértices (SA)](hull-shader-stage--hs-.md) es una de las fases de mosaico, dividir eficazmente una única superficie de un modelo en muchos triángulos. Un sombreador de casco se invoca una vez por revisión y transforma los puntos de control de entrada que definen una superficie de orden inferior en puntos de control que conforman una revisión. También realiza algunos cálculos por revisión para proporcionar datos para la fase de Tessellator (TS) y la etapa de sombreador de vértices de dominio (DS). | | Entrada | Entre 1 y puntos de control de entrada 32, que conjuntamente definen una superficie de orden inferior. | | Salida | Entre 1 y 32 puntos de control de salida, que forman parte de una revisión. El sombreado de casco declara el estado de la fase de Tessellator (TS), incluido el número de puntos de control, el tipo de imagen de revisión y el tipo de creación de particiones para usar cuando tessellating. |

## <a name="tessellator-stage"></a>Fase del teselador

|-|-| | Propósito | La [fase de Tessellator (TS)](tessellator-stage--ts-.md) crea un patrón de muestreo del dominio que representa la revisión de la geometría y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que se conectan a estos ejemplos. | | Entrada | La tessellator funciona una vez por cada revisión con los factores de mosaico (que especifican cómo de teselados de dominio) y el tipo de creación de particiones (que especifica el algoritmo utilizado para una revisión de división) que se pasan en de la región de casco sombreador de vértices. | | Salida | El tessellator da como resultado uv (y, opcionalmente, w) las coordenadas y la topología de la superficie a la fase de dominio sombreador de vértices. |

## <a name="domain-shader-stage"></a>Fase del sombreador de dominios

|-|-| | Propósito | La [fase de sombreador de vértices de dominio (DS)](domain-shader-stage--ds-.md) se calcula la posición del vértice de un punto dividida en la revisión de salida; calcula la posición del vértice que corresponde a cada ejemplo de dominio. Un sombreado de dominio se ejecuta una vez por tessellator punto de salida de fase y tiene acceso de solo lectura para el sombreado de casco de resultados de revisión y constantes de revisión de salida, y coordenadas UV de resultados de la fase de tessellator. | | Entrada | Un sombreado de dominio consume puntos de control de salida de la [fase de casco sombreador de vértices (SA)](hull-shader-stage--hs-.md). Las salidas del sombreador de casco incluyen: puntos de control, datos constantes de revisión y factores de teselación (los factores de teselación pueden incluir los valores que usa el teselador de función fija, así como los valores sin procesar [antes del redondeo por teselación de entero], lo que facilita la geotransformación, por ejemplo). Un sombreado de dominio se invoca una vez por cada coordenada de salida de la [fase de Tessellator (TS)](tessellator-stage--ts-.md). | | Salida | La fase de sombreador de vértices de dominio (DS) da como resultado la posición del vértice de un punto dividida en la revisión de salida. |

## <a name="geometry-shader-stage"></a>Fase de sombreador de geometría

|-|-| | Propósito | La [fase de sombreador de vértices de geometría (GS)](geometry-shader-stage--gs-.md) procesa Fundamentos todo: triángulos, líneas y puntos, junto con sus vértices adyacentes. Admite la amplificación y la desamplificación geométrica. Es útil para algoritmos, como Point Sprite Expansion, Dynamic Particle Systems, Fur/Fin Generation, Shadow Volume Generation, Single Pass Render-to-Cubemap, Per-Primitive Material Swapping y Per-Primitive Material Setup, que incluye la generación de coordenadas baricéntricas como datos primitivos, de modo que un sombreador de píxeles pueda realizar la interpolación de atributos personalizada. | | Entrada | A diferencia de los sombreados de vértice, que actúan en un solo vértice, entradas del sombreado de geometría son los vértices de un primitivo completo (tres vértices de triángulos, dos vértices para las líneas o vértice único punto). | | Salida | La fase de sombreador de vértices de geometría (GS) es capaz de generar varios vértices que forman una única topología seleccionada. Las topologías de salida del sombreador de geometría disponibles son <strong>tristrip</strong>, <strong>linestrip</strong> y <strong>pointlist</strong>. El número de primitivos emitido puede variar libremente dentro de cualquier invocación del sombreador de geometría, aunque el número máximo de vértices que podría emitirse debe declararse de forma estática. Longitudes de franja emitidos desde una invocación de sombreador de vértices de geometría pueden ser arbitrarios y descubrir nuevos se puede crear a través de la función [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL. |

## <a name="stream-output-stage"></a>Fase de salida de flujo

|-|-| | Propósito | La [fase de salida de la secuencia (así)](stream-output-stage--so-.md) continuamente da como resultado (o secuencias) datos de vértice de la región activa anterior a uno o más búferes de memoria. Datos transmite en secuencias a la memoria pueden ser sometidos en la canalización como datos de entrada, o la devolución de lectura de la CPU. | | Entrada | Datos de vértice de una fase de canalización anterior. | | Salida | La fase de salida de la secuencia (así) continuamente da como resultado (o secuencias) datos de vértice de la región activa anterior, como la etapa de sombreador de vértices de geometría (GS), en los búferes de uno o más de memoria. Si la región de sombreador de vértices de geometría (GS) está inactiva y la fase de salida de la secuencia (así) está activa, envía continuamente los datos de vértice de la región de sombreador de vértices de dominio (DS) en los búferes de memoria (o si el DS también está inactivo, de la región de sombreador de vértices (VS)). |

## <a name="rasterizer-stage"></a>Fase del rasterizador

|-|-| | Propósito | La [fase de rasterizador (RS)](rasterizer-stage--rs-.md) clips de los fundamentos que no están en la vista, prepara los fundamentos para la etapa de sombreador de píxeles (PS) y determina cómo invocar a sombreadores de píxeles. Convierte vectoriales información (consta de las formas o Fundamentos) en una imagen de mapa de bits (se compone de píxeles) con el fin de mostrar gráficos 3D en tiempo real. | | Entrada | Los vértices (x, y, z, w) ingresa el rasterizador etapa se supone que en el espacio de clip homogéneo. En este espacio de coordenadas del eje X señala hacia la derecha, Y señala hacia arriba y Z apunta fuera de la cámara. | | Salida | Los píxeles real que se deben representar. Incluye algunos atributos de vértice para su uso en interpolación por el sombreador de píxeles. |

## <a name="pixel-shader-stage"></a>Fase del sombreador de píxeles
 
|-|-| | Propósito | La [fase de sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) recibe datos interpoladas para un tipo primitivo y genera datos por píxel como el color. | | Entrada | Cuando se configura la canalización sin un sombreado de geometría, un sombreador de píxeles está limitado a 16, entradas de 32 bits, 4-componente. De lo contrario, un sombreador de píxeles puede aceptar hasta 32 entradas de 4 componentes y 32 bits. Los datos de entrada del sombreador de píxeles incluyen atributos de vértice (que se pueden interpolar con o sin corrección de la perspectiva) o pueden tratarse como constantes por primitivo. Las entradas del sombreador de píxeles se interpolan desde los atributos de vértice del primitivo que se está rasterizando, según el modo de interpolación declarado. Si un primitivo se recorta antes de la rasterización, el modo de interpolación se respeta también durante el proceso de recorte. | | Salida | Un sombreador de píxeles puede salida hasta 8, 32 bits, 4-componente colores o sin color si se descarta el píxel. Componentes de registro de salida de píxel sombreador de vértices se deben declarar antes de que se pueden usar; cada registro se permite una máscara de escritura de salida distinct. |

## <a name="output-merger-stage"></a>Fase de fusión de salida
 
|-|-| | Propósito | La [fase de fusión de salida (OM)](output-merger-stage--om-.md) combina varios tipos de datos de salida (valores de sombreador de píxeles, la información de profundidad y la Galería de símbolos) con el contenido de los búferes de destino y profundidad/Galería de símbolos de render para generar el resultado final de canalización. | | Entrada | Entradas de fusión de salida son el estado de la canalización, los datos de píxeles generados por los sombreadores de píxeles, el contenido de los destinos de representación y el contenido de los búferes de profundidad/Galería de símbolos. | | Salida | El final representada color del píxel. |

## <a name="related-topics"></a>Temas relacionados

- [Guía de aprendizaje de gráficos de Direct3D](index.md)

- [Canalización del proceso](compute-pipeline.md)
