---
title: Fase de salida de flujo (SO)
description: La fase de salida de la secuencia (SO) genera continuamente datos de vértices (o flujos) de la fase activa anterior en uno o varios búferes de la memoria. Los datos que se transmiten a la memoria pueden volver a reproducirse en la canalización como datos de entrada o de lectura desde la CPU.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Fase de salida de flujo (SO)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f56036ecc083d72f552954860d04750c1c83b8b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156209"
---
# <a name="stream-output-so-stage"></a>Fase de salida de flujo (SO)


La fase de salida de la secuencia (SO) genera continuamente datos de vértices (o flujos) de la fase activa anterior en uno o varios búferes de la memoria. Los datos que se transmiten a la memoria pueden volver a reproducirse en la canalización como datos de entrada o de lectura desde la CPU.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Propósito y usos


![diagrama de la ubicación de la fase de salida de flujo en la canalización](images/d3d10-pipeline-stages-so.png)

La fase Stream-Output transmite los datos primitivos de la canalización a la memoria de su camino al rasterizador. Los datos de la fase anterior pueden transmitirse a la memoria o pasarse al rasterizador. Los datos que se transmiten a la memoria pueden volver a reproducirse en la canalización como datos de entrada o de lectura desde la CPU.

Los datos que se transmiten a la memoria se pueden volver a leer en la canalización en una fase de representación subsiguiente, o bien se pueden copiar en un recurso de almacenamiento provisional (de modo que la CPU pueda leerlos). La cantidad de datos transmitidos puede variar; Direct3D está diseñado para controlar los datos sin necesidad de consultar (la GPU) la cantidad de datos escritos.--&gt;

Hay dos maneras de alimentar los datos de salida de flujo en la canalización:

-   Los datos de salida de flujo se pueden volver a alimentar en la fase del ensamblador de entrada (IA).
-   Los datos de salida de flujo se pueden leer mediante sombreadores programables mediante el uso de funciones de [carga](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load) .

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entradas


Datos de vértices de una fase de sombreador anterior.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Genere


La fase de salida de la secuencia (SO) envía continuamente los datos de vértices (o flujos) de la fase activa anterior, como la fase del sombreador de geometría (GS), a uno o varios búferes de la memoria. Si la fase del sombreador de geometría (GS) está inactiva, la fase de salida de la secuencia (SO) genera continuamente los datos de vértices de la fase del sombreador de dominios (DS) en los búferes de la memoria (o si DS también está inactivo, desde la fase del sombreador de vértices (VS)).

Cuando un triángulo o una franja de líneas está enlazada a la fase del ensamblador de entrada (IA), cada franja se convierte en una lista antes de que se transmitan. Los vértices siempre se escriben como tipos primitivos completos (por ejemplo, 3 vértices a la vez para triángulos). los primitivos incompletos nunca se transmiten por secuencias. Los tipos primitivos con adyacencias descartan los datos de adyacencia antes de la transmisión de datos.

La fase Stream-Output admite hasta 4 búferes simultáneamente.

-   Si está transmitiendo datos en varios búferes, cada búfer solo puede capturar un único elemento (hasta 4 componentes) de datos por vértice, con un intervalo de datos implícito igual al ancho del elemento en cada búfer (compatible con la forma en que se pueden enlazar los búferes de un solo elemento para la entrada en fases del sombreador). Además, si los búferes tienen tamaños diferentes, la escritura se detiene en cuanto uno de los búferes esté lleno.
-   Si está transmitiendo datos en un solo búfer, el búfer puede capturar hasta 64 componentes escalares de datos por vértice (256 bytes o menos) o el intervalo de vértices puede tener hasta 2048 bytes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 