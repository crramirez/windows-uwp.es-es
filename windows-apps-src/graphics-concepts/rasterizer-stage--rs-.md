---
title: Fase del rasterizador (RS)
description: El rasterizador recorta primitivos que no están en la vista, los prepara para la fase del sombreador de píxeles (PS) y determina cómo invocar sombreadores de píxeles.
ms.assetid: 7E80724B-5696-4A99-91AF-49744B5CD3A9
keywords:
- Fase del rasterizador (RS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 17d58a05a57fa833003b7c229db91473f6cde3d4
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5972111"
---
# <a name="rasterizer-rs-stage"></a>Fase del rasterizador (RS)


El rasterizador recorta primitivos que no están en la vista, los prepara para la [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) y determina cómo invocar sombreadores de píxeles. La fase de rasterización convierte la información de vector (que se compone de formas o primitivos) en una imagen de trama (que se compone de píxeles) con el fin de mostrar gráficos 3D en tiempo real.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidad y usos


Durante la rasterización, cada primitivo se convierte en píxeles, mientras interpola los valores por vértice en todos los primitivos. La rasterización incluye vértices de recorte en el frustum de la vista, y realiza una división por Z para proporcionar la perspectiva, asigna primitivos a una ventanilla 2D y determina cómo invocar el sombreador de píxeles. Mientras que el uso de un sombreador de píxeles es opcional, la fase de rasterización siempre realiza el recorte, una división de perspectiva para transformar los puntos en un espacio homogéneo y la asignación de los vértices a la ventanilla.

Para deshabilitar la rasterización, puedes indicar a la canalización que no existe ningún sombreador de píxeles (establece la [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) en **NULL** y deshabilita las prueba de profundidad y galería de símbolos). Mientras esté deshabilitada, los contadores de la canalización relacionados con la rasterización no se actualizarán.

En el hardware que implementa las optimizaciones jerárquicas del búfer Z, puedes habilitar la precarga del búfer Z. Para ello, establece la fase del sombreador de píxeles (PS) en **NULL** mientras habilitas pruebas de profundidad y galería de símbolos.

Consulta [Reglas de rasterización](rasterization-rules.md).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Se supone que los vértices (x, y, z, w) que se incorporan a la fase de rasterización están en el espacio de recorte homogéneo. En este espacio de coordenadas, el eje X apunta a la derecha, el eje Y apunta arriba y el eje Z, fuera de la cámara.

La fase de rasterización (RS) de función fija se introduce a través de la fase de salida en secuencias (SO) o la fase de canalización anterior, como, por ejemplo, la [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md). Si no se utiliza GS, RS se introduce a través de la [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md). Si DS tampoco se utiliza, RS se introduce a través de la [fase del sombreador de vértices (VS)](vertex-shader-stage--vs-.md).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Salida


El uso de la fase del sombreador de píxeles (PS) es opcional; la fase de rasterización se puede generar directamente en la [fase de fusión de salida (OM)](output-merger-stage--om-.md) en su lugar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Reglas de rasterización](rasterization-rules.md)

[Canalización de gráficos](graphics-pipeline.md)

 

 




