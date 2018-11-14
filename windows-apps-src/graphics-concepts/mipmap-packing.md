---
title: Empaquetado de mapas MIP
description: Se pueden empaquetar algunos MIP (por segmento de matriz) en un determinado número de iconos, según las dimensiones, el formato, el número de mapas MIP y los segmentos de matriz de un recurso de streaming.
ms.assetid: 906C3CAC-4E84-4947-B508-06788551BE85
keywords:
- Empaquetado de mapas MIP
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ec3d091d7cc5aca82aeef9a3e7f29a8d363705a3
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6649442"
---
# <a name="mipmap-packing"></a>Empaquetado de mapas MIP


Se pueden empaquetar algunos MIP (por segmento de matriz) en un determinado número de iconos, según las dimensiones, el formato, el número de mapas MIP y los segmentos de matriz de un recurso de streaming.

En función del [nivel](streaming-resources-features-tiers.md) que admitan los recursos de streaming, los mapas MIP con determinadas dimensiones no siguen las formas de icono estándar y se considera que están todos empaquetados juntos entre sí de manera que resultan opacos para la aplicación. Los niveles superiores de compatibilidad tienen más garantías sobre los tipos de dimensiones de superficie que caben en las formas de icono estándar (y, por lo tanto, las aplicaciones las pueden asignar individualmente).

Lo que puede variar entre las implementaciones es que (dadas las dimensiones, el formato, el número de mapas MIP y los segmentos de matriz de un recurso de streaming) un número M de MIP (por segmento de matriz) se pueda empaquetar en un número N de iconos. Cuando obtienes la información de icono del recurso para un dispositivo, el controlador notifica a la aplicación qué son M y N (entre otros detalles sobre la superficie que son estándar y no varían según el proveedor de hardware). El conjunto de iconos para el MIP empaquetado sigue siendo 64KB y se puede asignar individualmente en ubicaciones distintas de un grupo de iconos.

Sin embargo, la forma del píxel de los iconos y cómo se ajustan los mapas MIP en el conjunto de iconos es específico del proveedor de hardware y demasiado complejo para exponerlo. Por lo tanto, es necesario que las aplicaciones asignen cada vez todos los iconos que se indican como empaquetados o ninguno de ellos. De lo contrario, el comportamiento de acceso al recurso de streaming no está definido.

Para las superficies de una matriz, el conjunto de MIP empaquetados y el número de iconos empaquetados que almacenan esos MIP (M y N descritos anteriormente) se aplica por separado para cada segmento de matriz.

Las API dedicadas para copiar iconos no pueden obtener acceso a MIP empaquetados. Las aplicaciones que quieran copiar datos desde MIP empaquetados (y hacia ellos) pueden hacerlo con todas las API que no son específicas de los recursos de streaming para copiar y representar en superficies.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cómo se organiza en mosaico el área de un recurso de streaming](how-a-streaming-resource-s-area-is-tiled.md)

 

 




