---
title: Fase de salida de flujo (SO)
description: La fase de salida de flujo (SO) saca (o transmite) continuamente datos de vértices desde la fase activa anterior a uno o más búferes de la memoria. Los datos trasmitidos a la memoria pueden regresar a la canalización como datos de entrada o volver a leerse desde la CPU.
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- Fase de salida de flujo (SO)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a86aa5a78bc4df9deaeea239356345c33736d942
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821899"
---
# <a name="stream-output-so-stage"></a>Fase de salida de flujo (SO)


La fase de salida de flujo (SO) saca (o transmite) continuamente datos de vértices desde la fase activa anterior a uno o más búferes de la memoria. Los datos trasmitidos a la memoria pueden regresar a la canalización como datos de entrada o volver a leerse desde la CPU.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidad y usos


![Diagrama de ubicación de la fase de salida de flujo en la canalización](images/d3d10-pipeline-stages-so.png)

La fase de salida de flujo transmite datos de primitivos de desde la canalización a la memoria en tránsito hacia el rasterizador. Los datos de la etapa anterior pueden trasmitirse a la memoria o pasarse al rasterizador. Los datos trasmitidos a la memoria pueden regresar a la canalización como datos de entrada o volver a leerse desde la CPU.

Los datos trasmitidos a la memoria pueden volver a leerse en la canalización en un pase de representación posterior o pueden copiarse en un recurso de almacenamiento provisional (para que pueda leerlos la CPU). La cantidad de datos trasmitidos puede variar; Direct3D está diseñado para administrar los datos sin necesidad de consultar (a la GPU) sobre la cantidad de datos que se escriben.--&gt;

Existen dos formas de transmitir datos de salida de flujo a la canalización:

-   Los datos de la secuencia de flujo pueden devolverse a la fase del ensamblador de entrada (IA).
-   Los datos de la secuencia de flujo pueden ser leerlos sombreadores programables mediante el uso de funciones [Load](https://msdn.microsoft.com/library/windows/desktop/bb509694).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Los datos de vértices procedentes de una fase anterior del sombreador.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Resultados


La fase de salida de flujo (SO) saca (o transmite) continuamente datos de vértices desde la fase activa anterior, como la fase del sombreador de geometría (GS). Si la fase del sombreador de geometría (GS) está inactiva, la fase de salida de flujo (SO) transmite continuamente los datos de vértices procedentes de la fase del sombreador de dominio (DS) a los búferes de la memoria (o si la fase DS también está inactiva, desde la fase del sombreador de vértices [VS]).

Cuando una franja de triángulos o líneas está enlazada a la fase del ensamblador de entrada (IA), cada franja se convierte en una lista antes de transmitirse. Los vértices siempre se escriben como primitivos completos (por ejemplo, 3 vértices a la vez de triángulos); los primitivos incompletos nunca se transmiten. Los tipos primitivos con proximidad descartan los datos de adyacencia antes de transmitir los datos.

La fase de salida de flujo admite hasta 4 búferes al mismo tiempo.

-   Si transmites datos a varios búferes, cada búfer únicamente puede capturar un elemento individual (hasta 4 componentes) de datos por vértice, con un intervalo de datos implícito igual al ancho del elemento en cada búfer (compatible con la forma en que se pueden enlazar los búferes de elemento único para la entrada en las fases de sombreador). Además, si los búferes tienen diferentes tamaños, la escritura se detiene en cuanto uno de los búferes está lleno.
-   Si transmites datos a un único búfer, este búfer puede capturar hasta 64 componentes escalares de datos por vértice (256 bytes o menos) o el intervalo del vértice puede tener hasta 2048 bytes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




