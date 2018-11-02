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
ms.localizationpriority: medium
ms.openlocfilehash: d4a362a3a7be06e48c64ce3e4d43ff917b9b24c5
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5929642"
---
# <a name="graphics-pipeline"></a>Canalización de gráficos


La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.

Todas las fases pueden configurarse mediante la API de Direct3D. Las fases que ofrecen núcleos de sombreador comunes (bloques rectangulares redondeados) pueden programarse mediante el lenguaje de programación [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Esto hace que la canalización sea muy flexible y adaptable.

La más utilizadas son la fase del sombreador de vértices (VS) y la fase del sombreador de píxeles (PS). Si no proporcionas estas fases de sombreador, se usa el sombreador de píxeles y un vértice de paso a través no operativo predeterminado.

![diagrama del flujo de datos en la canalización programable de Direct3D 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Fase del ensamblador de entrada

|-|-| | Propósito | La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) proporciona primitivos y datos de adyacencia a la canalización, como triángulos, líneas y puntos, incluyen semántica identificadores para ayudar a que los sombreadores sean más eficientes reducir el procesamiento de primitivos que ya no se han procesa. | | Entrada | Datos primitivos (triángulos, líneas o puntos) de los búferes rellenados por el usuario en la memoria. Y posiblemente datos de adyacencia. Un triángulo sería 3 vértices para cada triángulo y posiblemente 3 vértices para datos de adyacencia por triángulo. | | Salida | Primitivos con valores adjuntos generados por el sistema (por ejemplo, un identificador de primitivo, un identificador de instancia o un identificador de vértice). |

## <a name="vertex-shader-stage"></a>Fase del sombreador de vértices

|-|-| | Propósito | La [fase del sombreador de vértices (VS)](vertex-shader-stage--vs-.md) procesa vértices, normalmente realizando ciertas operaciones como transformar, enmascarar e iluminar. Un sombreador de vértices toma un solo vértice de entrada y produce un solo vértice de salida. Operaciones por vértice individuales, como transformaciones, enmascaramientos e iluminaciones por vértice. | | Entrada | Un solo vértice, con los valores generados por el sistema VertexID y InstanceID. Cada vértice de entrada del sombreador de vértices puede comprender hasta 16 vectores de 32 bits (hasta 4 componentes cada uno). | | Salida | Un solo vértice. Cada vértice de salida puede comprender hasta 16 vectores de 4 componentes de 32 bits. |
 
## <a name="hull-shader-stage"></a>Fase del sombreador de casco
 
|-|-| | Propósito | La [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md) es una de las fases de teselación, que división de forma eficaz una sola superficie de un modelo en muchos triángulos. Un sombreador de casco se invoca una vez por revisión y transforma los puntos de control de entrada que definen una superficie de orden inferior en puntos de control que conforman una revisión. También hace algunos cálculos por revisión para proporcionar datos de la fase de Tessellator (TS) y la fase del sombreador de dominios (DS). | | Entrada | Entre 1 y puntos de control de entrada 32, que en conjunto definen una superficie de orden. | | Salida | Entre 1 y 32 puntos de control de salida, que juntos forman una revisión. El sombreador de casco declara el estado de la fase de Tessellator (TS), incluido el número de puntos de control, el tipo de cara de revisión y el tipo de partición que se usa durante la teselación. |

## <a name="tessellator-stage"></a>Fase del teselador

|-|-| | Propósito | La [fase del Teselador (TS)](tessellator-stage--ts-.md) crea un patrón de muestreo del dominio que representa la revisión de geometría y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estas muestras. | | Entrada | El tessellator funciona una vez por revisión usando los factores de teselación (que especifican cómo con precisión el dominio se se tesela) y el tipo de partición (que especifica el algoritmo usado para dividir una revisión) que se pasan desde la fase del sombreador de casco. | | Salida | El tessellator envía uv (y, opcionalmente, w) coordenadas y la topología de superficie a la fase del sombreador de dominios. |

## <a name="domain-shader-stage"></a>Fase del sombreador de dominios

|-|-| | Propósito | La [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md) calcula la posición del vértice de un punto subdividido en la revisión de salida; calcula la posición del vértice correspondiente a cada muestra de dominio. Un sombreador de dominios se ejecuta una vez por cada fase del teselador punto de salida de la fase y tiene acceso de solo lectura para el sombreador de casco revisión de salida y constantes de revisión de salida y las coordenadas de UV de salida de la fase de tessellator. | | Entrada | Un sombreador de dominios consume puntos de control de salida de la [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md). Las salidas del sombreador de casco incluyen: puntos de control, datos constantes de revisión y factores de teselación (los factores de teselación pueden incluir los valores que usa el teselador de función fija, así como los valores sin procesar [antes del redondeo por teselación de entero], lo que facilita la geotransformación, por ejemplo). Un sombreador de dominios se invoca una vez por coordenada de salida de la [fase del Teselador (TS)](tessellator-stage--ts-.md). | | Salida | La fase del sombreador de dominios (DS) devuelve la posición del vértice de un punto subdividido en la revisión de salida. |

## <a name="geometry-shader-stage"></a>Fase de sombreador de geometría

|-|-| | Propósito | La [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md) procesa primitivos completos: triángulos, líneas y puntos, junto con sus vértices adyacentes. Admite la amplificación y la desamplificación geométrica. Es útil para algoritmos, como Point Sprite Expansion, Dynamic Particle Systems, Fur/Fin Generation, Shadow Volume Generation, Single Pass Render-to-Cubemap, Per-Primitive Material Swapping y Per-Primitive Material Setup, que incluye la generación de coordenadas baricéntricas como datos primitivos, de modo que un sombreador de píxeles pueda realizar la interpolación de atributos personalizada. | | Entrada | A diferencia de los sombreadores de vértices, que trabajan en un solo vértice, entradas del sombreador de geometría son los vértices para un primitivo completo (tres vértices para triángulos, dos vértices para líneas o solo vértice de punto). | | Salida | La fase del sombreador de geometría (GS) es capaz de presentar varios vértices que forman una sola topología seleccionada. Las topologías de salida del sombreador de geometría disponibles son <strong>tristrip</strong>, <strong>linestrip</strong> y <strong>pointlist</strong>. El número de primitivos emitido puede variar libremente dentro de cualquier invocación del sombreador de geometría, aunque el número máximo de vértices que podría emitirse debe declararse de forma estática. Longitudes de franja emitidas desde una invocación del sombreador de geometría pueden ser arbitrarias, y pueden crearse nuevas franjas a través de la función HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) . |

## <a name="stream-output-stage"></a>Fase de salida de flujo

|-|-| | Propósito | La [fase de salida de flujo (SO)](stream-output-stage--so-.md) continuamente salidas (o transmite) datos de vértices de la fase activa anterior a uno o más búferes de memoria. Los datos trasmitidos a la memoria pueden regresar regresar a la canalización como datos de entrada o volver de la CPU. | | Entrada | Datos de vértices de una fase de canalización anterior. | | Salida | La fase de salida de flujo (SO) continuamente salidas (o transmite) datos de vértices de la fase activa anterior, como la fase del sombreador de geometría (GS), a uno o más búferes de memoria. Si la fase del sombreador de geometría (GS) está inactiva y la fase de salida de flujo (SO) está activa, devuelve continuamente datos de vértices de la fase del sombreador de dominios (DS) en los búferes de memoria (o si DS también está inactivo, desde la fase del sombreador de vértices (VS)). |

## <a name="rasterizer-stage"></a>Fase del rasterizador

|-|-| | Propósito | La [fase del rasterizador (RS)](rasterizer-stage--rs-.md) recorta primitivos que no están en la vista, los prepara para la fase del sombreador de píxeles (PS) y determina cómo invocar a sombreadores de píxeles. Convierte vector información (que se compone de formas o primitivos) en una imagen de trama (que se compone de píxeles) con el fin de mostrar gráficos 3D en tiempo real. | | Entrada | Los vértices (x, y, z, w) entra en el rasterizador fase se supone que en el espacio de recorte homogéneo. En este espacio de coordenadas del eje X apunta a la derecha, Y apunta hacia arriba y Z puntos fuera de la cámara. | | Salida | Píxeles reales que se deben representar. Incluye algunos atributos de vértice para su uso en la interpolación el sombreador de píxeles. |

## <a name="pixel-shader-stage"></a>Fase del sombreador de píxeles
 
|-|-| | Propósito | La [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) recibe datos interpolados de un primitivo y genera datos por píxel, como el color. | | Entrada | Cuando se configura la canalización sin un sombreador de geometría, un sombreador de píxeles se limita a las entradas de 16 y 32 bits, 4 componentes. De lo contrario, un sombreador de píxeles puede aceptar hasta 32 entradas de 4 componentes y 32 bits. Los datos de entrada del sombreador de píxeles incluyen atributos de vértice (que se pueden interpolar con o sin corrección de la perspectiva) o pueden tratarse como constantes por primitivo. Las entradas del sombreador de píxeles se interpolan desde los atributos de vértice del primitivo que se está rasterizando, según el modo de interpolación declarado. Si un primitivo se recorta antes de la rasterización, el modo de interpolación se respeta también durante el proceso de recorte. | | Salida | Un sombreador de píxeles puede producir hasta 8, 32 bits, los colores de 4 componentes, o ningún color si el píxel se descarta. Componentes de registro de salida de sombreador de píxeles deben declararse para que puedan usarse; cada registro se permite una máscara de escritura de salida diferente. |

## <a name="output-merger-stage"></a>Fase de fusión de salida
 
|-|-| | Propósito | La [fase de fusión de salida (OM)](output-merger-stage--om-.md) combina varios tipos de datos de salida (valores de sombreador de píxeles, información de profundidad y Galería de símbolos) con el contenido de los búferes de profundidad o galería de símbolos y de destino de representación para generar el resultado final de la canalización. | | Entrada | Las entradas de fusión de salida son el estado de canalización, los datos de píxel generados por los sombreadores de píxeles, el contenido de los destinos de representación y el contenido de los búferes de profundidad o galería de símbolos. | | Salida | Color de píxel de representación final. |

## <a name="related-topics"></a>Temas relacionados

- [Guía de aprendizaje de gráficos de Direct3D](index.md)

- [Canalización del proceso](compute-pipeline.md)
