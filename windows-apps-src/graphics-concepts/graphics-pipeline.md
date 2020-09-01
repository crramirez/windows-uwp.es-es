---
title: Canalización de gráficos
description: La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- Canalización de gráficos
- Etapas de canalización
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a562e1eb99447db263cd2bb4f87ec1a642ed8394
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175179"
---
# <a name="graphics-pipeline"></a>Canalización de gráficos


La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.

Todas las fases se pueden configurar mediante la API de Direct3D. Las fases que incluyen núcleos de sombreador comunes (los bloques rectangulares redondeados) se pueden programar mediante el lenguaje de programación [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl) . Esto hace que la canalización sea extremadamente flexible y adaptable.

El uso más común son la fase del sombreador de vértices (VS) y la fase del sombreador de píxeles (PS). Si no se proporcionan ni siquiera estas fases del sombreador, se usa un vértice de paso y un sombreador de píxeles predeterminados.

![diagrama del flujo de datos en la canalización programable de Direct3D 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Fase del ensamblador de entrada

|-|-| | Propósito | La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) proporciona datos primitivos y de adyacen a la canalización, como triángulos, líneas y puntos, incluidos los identificadores semánticos para mejorar la eficacia de los sombreadores reduciendo el procesamiento a primitivos que aún no se han procesado. | | Entrada | Datos primitivos (triángulos, líneas o puntos), de búferes rellenos por el usuario en la memoria. Y posiblemente datos de adyacencias. Un triángulo sería 3 vértices para cada triángulo y posiblemente tres vértices para datos de adyacencia por triángulo. | | Salida | Primitivas con valores asociados generados por el sistema (como un identificador primitivo, un identificador de instancia o un identificador de vértice). |

## <a name="vertex-shader-stage"></a>Fase del sombreador de vértices

|-|-| | Propósito | La [fase del sombreador de vértices (vs)](vertex-shader-stage--vs-.md) procesa los vértices, normalmente realizando operaciones como transformaciones, máscaras e iluminación. Un sombreador de vértices toma un solo vértice de entrada y genera un solo vértice de salida. Operaciones individuales por vértice, como transformaciones, máscaras, transformación y iluminación por vértice. | | Entrada | Un solo vértice, con los valores generados por el sistema VertexID y InstanceID. Cada vértice de entrada del sombreador de vértices puede estar formado por vectores de hasta 16 32 bits (hasta 4 componentes cada uno). | | Salida | Un solo vértice. Cada vértice de salida puede constar de un máximo de 16 32 vectores de componentes de 4 bits. |
 
## <a name="hull-shader-stage"></a>Fase del sombreador de casco
 
|-|-| | Propósito | La [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md) es una de las fases de teselación, que dividen de forma eficaz una sola superficie de un modelo en muchos triángulos. Un sombreador de casco se invoca una vez por revisión y transforma los puntos de control de entrada que definen una superficie de orden inferior en los puntos de control que componen una revisión. También realiza algunos cálculos por revisión para proporcionar datos para la etapa del teselador (TS) y la fase del sombreador de dominios (DS). | Entrada | Entre 1 y 32 puntos de control de entrada, que juntos definen una superficie de orden inferior. | | Salida | Entre 1 y 32 puntos de control de salida, que juntos forman una revisión. El sombreador de casco declara el estado de la fase del teselador (TS), incluido el número de puntos de control, el tipo de revisión y el tipo de creación de particiones que se va a usar cuando teselar. |

## <a name="tessellator-stage"></a>Fase del teselador

|-|-| | Propósito | La [etapa del teselador (TS)](tessellator-stage--ts-.md) crea un patrón de muestreo del dominio que representa la revisión geométrica y genera un conjunto de objetos más pequeños (triángulos, puntos o líneas) que conectan estos ejemplos. | | Entrada | Del teselador funciona una vez por revisión mediante los factores de teselación (que especifican el grado de teselación del dominio) y el tipo de creación de particiones (que especifica el algoritmo que se usa para segmentar una revisión) que se pasan desde la fase del sombreador de casco. | | Salida | Del teselador genera coordenadas UV (y, opcionalmente, w) y la topología de superficie en la etapa del sombreador de dominios.

## <a name="domain-shader-stage"></a>Fase del sombreador de dominios

|-|-| | Propósito | La [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md) calcula la posición del vértice de un punto subdividido en la revisión de salida; calcula la posición del vértice que corresponde a cada ejemplo de dominio. Un sombreador de dominio se ejecuta una vez por punto de salida de fase de del teselador y tiene acceso de solo lectura a la revisión de salida del sombreador de casco y a las constantes de la revisión de salida, y a las coordenadas UV de salida de la fase del teselador. | | Entrada | Un sombreador de dominios consume puntos de control de salida de la [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md). Entre las salidas del sombreador de casco se incluyen los puntos de control, los datos constantes de revisión y los factores de teselación (los factores de teselación pueden incluir los valores usados por el del teselador de funciones fijas, así como los valores sin formato, antes de redondear la teselación de enteros, lo que facilita la geotransformación, por ejemplo). Un sombreador de dominio se invoca una vez por cada coordenada de salida de la [fase del teselador (TS)](tessellator-stage--ts-.md). | Salida | La fase del sombreador de dominios (DS) genera la posición del vértice de un punto subdividido en la revisión de salida. |

## <a name="geometry-shader-stage"></a>Fase del sombreador de geometría

|-|-| | Propósito | La [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md) procesa los primitivos completos: triángulos, líneas y puntos, junto con sus vértices adyacentes. Admite amplificación geométrica y desamplificación. Resulta útil para los algoritmos, como la expansión de sprites de punto, sistemas de partículas dinámicas, generación de peletería y fin, generación de volúmenes de instantáneas, representación de un solo paso a mapa, intercambio de material por primitivos y configuración de materiales por primitivos, incluida la generación de coordenadas Barycentric como datos primitivos para que un sombreador de píxeles pueda realizar la interpolación de | | Entrada | A diferencia de los sombreadores de vértices, que operan en un solo vértice, las entradas del sombreador de geometría son los vértices de un primitivo completo (tres vértices para triángulos, dos vértices para las líneas o un solo vértice para el punto). | | Salida | La fase del sombreador de geometría (GS) es capaz de generar varios vértices que formen una sola topología seleccionada. Las topologías de salida del sombreador de geometría disponibles son <strong>trifranja</strong>, <strong>linestrip</strong>y <strong>PointList</strong>. El número de primitivas emitidas puede variar libremente dentro de cualquier invocación del sombreador de geometría, aunque el número máximo de vértices que podrían emitirse se debe declarar de forma estática. Las longitudes de franjas emitidas desde una invocación del sombreador de geometría pueden ser arbitrarias y se pueden crear nuevas bandas a través de la función HLSL [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) |

## <a name="stream-output-stage"></a>Fase de salida de flujo

|-|-| | Propósito | La [fase de salida de la secuencia (SO)](stream-output-stage--so-.md) genera continuamente datos de vértices (o flujos) de la fase activa anterior en uno o varios búferes de la memoria. Los datos que se transmiten a la memoria pueden volver a reproducirse en la canalización como datos de entrada o a la lectura de la CPU. | Entrada | Datos de vértices de una fase de canalización anterior. | | Salida | La fase de salida de la secuencia (SO) envía continuamente los datos de vértices (o flujos) de la fase activa anterior, como la fase del sombreador de geometría (GS), a uno o varios búferes de la memoria. Si la fase del sombreador de geometría (GS) está inactiva y la fase de salida de la secuencia (SO) está activa, genera continuamente datos de vértices de la fase del sombreador de dominios (DS) en los búferes de la memoria (o si el DS también está inactivo, desde la fase del sombreador de vértices).

## <a name="rasterizer-stage"></a>Fase de rasterización

|-|-| | Propósito | La [fase de rasterizador (RS)](rasterizer-stage--rs-.md) recorta los primitivos que no están en la vista, prepara los primitivos para la fase del sombreador de píxeles (PS) y determina cómo invocar los sombreadores de píxeles. Convierte la información de vectores (formada por formas o tipos primitivos) en una imagen de trama (compuesta de píxeles) con el fin de mostrar gráficos 3D en tiempo real. | Entrada | Se supone que los vértices (x, y, z, w) que entran en la fase de rasterizador están en el espacio de recorte homogéneo. En este espacio de coordenadas, el eje X apunta hacia la derecha, y apunta hacia arriba y a la Z puntos de la cámara. | | Salida | Los píxeles reales que se deben representar. Incluye algunos atributos de vértice para su uso en la interpolación por parte del sombreador de píxeles.

## <a name="pixel-shader-stage"></a>Fase del sombreador de píxeles
 
|-|-| | Propósito | La [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) recibe datos interpolados para un primitivo y genera datos por píxel, como el color. | | Entrada | Cuando la canalización se configura sin un sombreador de geometría, un sombreador de píxeles se limita a 16, 32 bits, 4-entradas de componentes. De lo contrario, un sombreador de píxeles puede tardar hasta 32, 32 bits, 4-entradas de componente. Los datos de entrada del sombreador de píxeles incluyen atributos de vértices (que se pueden interpolar con o sin corrección de perspectiva) o se pueden tratar como constantes por primitiva. Las entradas del sombreador de píxeles se interpolan a partir de los atributos de vértice de la primitiva que se está rasterizando, según el modo de interpolación declarado. Si una primitiva se recorta antes de la rasterización, también se respeta el modo de interpolación durante el proceso de recorte. | | Salida | Un sombreador de píxeles puede generar un resultado de hasta 8, 32 bits, colores de cuatro componentes o ningún color si se descarta el píxel. Los componentes del registro de salida del sombreador de píxeles se deben declarar antes de poder usarlos. se permite a cada registro una máscara de escritura de salida distinta. |

## <a name="output-merger-stage"></a>Fase de fusión de salida
 
|-|-| | Propósito | La [fase de fusión de salida (OM)](output-merger-stage--om-.md) combina varios tipos de datos de salida (valores del sombreador de píxeles, información de profundidad y estarcido) con el contenido de los búferes de destino de representación y de profundidad/estarcido para generar el resultado final de la canalización. | | Entrada | Las entradas de fusión de salida son el estado de canalización, los datos de píxeles generados por los sombreadores de píxeles, el contenido de los destinos de representación y el contenido de los búferes de profundidad/estarcido. | | Salida | Color de píxel representado final. |

## <a name="related-topics"></a>Temas relacionados

- [Guía de aprendizaje de gráficos de Direct3D](index.md)

- [Canalización del proceso](compute-pipeline.md)