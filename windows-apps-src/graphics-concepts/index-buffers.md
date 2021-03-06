---
title: Búferes de índices
description: Los búferes de índices son búferes de memoria que contienen datos de índice, que son desplazamientos de enteros en búferes de vértices y se usan para representar primitivos.
ms.assetid: 14D3DEC5-CF74-488B-BE41-16BF5E3201BE
keywords:
- Búferes de índices
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 36d08006fa2f32812f97daef5135a98dce16c4e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594620"
---
# <a name="index-buffers"></a>Búferes de índices


Los *búferes de índices* son búferes de memoria que contienen datos de índice, que son desplazamientos de enteros en búferes de vértices y se usan para representar primitivos.

Los búferes de índices son búferes de memoria que contienen datos de índice. Los datos de índice, o índices, son desplazamientos de enteros en búferes de vértices y se usan para representar primitivos.

Un búfer de vértices contiene vértices; por lo tanto, puedes dibujar un búfer de vértices con o sin primitivos indizados. Sin embargo, como un búfer de índices contiene índices, no puedes usar un búfer de índices sin un búfer de vértices correspondiente.

## <a name="span-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanspan-idindexbufferdescriptionspanindex-buffer-description"></a><span id="Index_Buffer_Description"></span><span id="index_buffer_description"></span><span id="INDEX_BUFFER_DESCRIPTION"></span>Descripción de búfer de índice


Un búfer de índices se describe en términos de sus funcionalidades, como su ubicación en la memoria, si admite la lectura y escritura y el tipo y la cantidad de índices que puede contener.

Las descripciones del búfer de índices indican a la aplicación cómo se creó un búfer existente. Proporcionas una estructura de descripción vacía para que el sistema la rellene con las capacidades de un búfer de índices creado anteriormente.

## <a name="span-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanspan-idindexprocessingrequirementsspanindex-processing-requirements"></a><span id="Index_Processing_Requirements"></span><span id="index_processing_requirements"></span><span id="INDEX_PROCESSING_REQUIREMENTS"></span>Requisitos de procesamiento de índice


El rendimiento de las operaciones de procesamiento de índices depende en gran medida de que el búfer de índices exista en la memoria y del tipo de dispositivo de representación que se usa. Las aplicaciones controlan la asignación de memoria para los búferes de índices cuando se crean.

La aplicación puede escribir directamente índices en un búfer de índices asignado en la memoria óptima para el controlador. Esta técnica impide que se haga una operación de copia redundante más adelante. Esta técnica no funciona bien si la aplicación vuelve a leer datos de un búfer de índices, ya que las operaciones de lectura que lleva a cabo el host de la memoria óptima para el controlador pueden ser muy lentas. Por lo tanto, si la aplicación tiene que leer durante el procesamiento o escribe datos en el búfer de forma errática, un búfer de índices de memoria del sistema es una elección mejor.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Búferes de vértices e índices](vertex-and-index-buffers.md)

 

 




