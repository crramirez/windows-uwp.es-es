---
title: Características de muestreo de texturas de recursos de streaming
description: Recursos de streaming las características de muestreo de textura incluyen la obtención de comentarios sobre el estado del sombreador sobre las áreas asignadas, la comprobación de si todos los datos a los que se tiene acceso se han asignado en el recurso, la fijación a los sombreadores de ayuda evita áreas en los recursos de streaming de mipmapped que se sabe que no están asignados y la detección de lo que el
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Características de muestreo de texturas de recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 823b7b6ba7835f62277e15fa41fb968c6f4a167f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173049"
---
# <a name="streaming-resources-texture-sampling-features"></a>Características de muestreo de texturas de recursos de streaming


Recursos de streaming las características de muestreo de textura incluyen la obtención de comentarios sobre el estado del sombreador sobre las áreas asignadas, la comprobación de si todos los datos a los que se tiene acceso se han asignado en el recurso, la fijación a los sombreadores de ayuda evita áreas en los recursos de streaming de mipmapped que se sabe que no están asignados y la detección de lo que el

## <a name="span-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanspan-idrequirements_of_streaming_resources_texture_sampling_featuresspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Requisitos de las características de muestreo de textura de recursos de streaming


Las características de muestreo de textura que se describen aquí requieren el nivel de nivel [2](tier-2.md) de compatibilidad con recursos de streaming.

## <a name="span-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanspan-idshader_status_feedback_about_mapped_areasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Comentarios sobre el estado del sombreador sobre las áreas asignadas


Cualquier instrucción del sombreador que lee y/o escribe en un recurso de streaming hace que se registre la información de estado. Este estado se expone como un valor de devolución adicional opcional en todas las instrucciones de acceso a recursos que entran en un registro temporal de 32 bits. El contenido del valor devuelto es opaco. Es decir, no se permite la lectura directa por parte del programa de sombreador. Sin embargo, puede usar la función [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) para extraer la información de estado.

## <a name="span-idfully_mapped_checkspanspan-idfully_mapped_checkspanspan-idfully_mapped_checkspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Comprobación totalmente asignada


La función [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) interpreta el estado devuelto desde un acceso a la memoria e indica si todos los datos a los que se tiene acceso se han asignado en el recurso. **CheckAccessFullyMapped** devuelve true (0xFFFFFFFF) si los datos se asignaron o false (0x00000000) si los datos se han desasignado.

Durante las operaciones de filtro, a veces el peso de un textura determinado termina en 0,0. Un ejemplo es una muestra lineal con coordenadas de textura que se encuentran directamente en un centro de textura: 3 otros textura (que pueden variar según el hardware) contribuyen al filtro pero con 0 peso. Estos 0 pesos textura no contribuyen al resultado del filtro, por lo que si se producen en iconos **nulos** , no se cuentan como accesos no asignados. Tenga en cuenta que la misma garantía se aplica a los filtros de textura que incluyen varios niveles de MIP. Si el textura de uno de los mapas de información no está asignado, pero el peso de esos textura es 0, esos textura no cuentan como un acceso sin asignar.

Cuando se realiza un muestreo de un formato que tiene menos de 4 componentes (como el formato de DXGI \_ \_ R8 \_ UNORM), cualquier textura que se encuentra en mosaicos **nulos** da como resultado el informado de un acceso asignado **nulo** , independientemente de los componentes que el sombreador examine realmente en el resultado. Por ejemplo, la lectura de R8 \_ UNORM y el enmascaramiento del resultado de lectura en el sombreador con. GBA/. YZW parecería que no necesitara leer la textura en absoluto. Pero si la dirección textura es un mosaico asignado **nulo** , la operación todavía cuenta como un acceso de asignación **null** .

El sombreador puede comprobar el estado y llevar a cabo cualquier curso de acción deseado en caso de error. Por ejemplo, un curso de acción puede ser el registro de "errores" (por ejemplo, a través de UAV Write) o la emisión de otra lectura fijada a un LOD más general que se sabe que está asignado. Es posible que una aplicación desee realizar un seguimiento de los accesos correctos también con el fin de obtener una idea de qué parte del conjunto de mosaicos asignados tiene acceso.

Una complicación para el registro es que no existe ningún mecanismo para informar del conjunto exacto de mosaicos al que se habría tenido acceso. La aplicación puede hacer conjeturas conservadoras en función de conocer las coordenadas utilizadas para el acceso, así como de utilizar la instrucción LOD; por ejemplo, [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)) devuelve el cálculo de LOD de hardware.

Otra complicación es que muchos de los accesos serán a los mismos iconos, por lo que se producirá una gran cantidad de registros redundantes y posiblemente contención en la memoria. Podría ser conveniente si el hardware pudiera tener la opción de no molestarse en el acceso al icono de informe si se notificaba en otro lugar antes. Quizás el estado de este seguimiento podría restablecerse desde la API (probablemente en los límites del marco).

## <a name="span-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanspan-idper-sample_minlod_clampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Abrazadera MinLOD por muestra


Para ayudar a los sombreadores a evitar áreas en los recursos de streaming de mipmapped que se sabe que no están asignados, la mayoría de las instrucciones de sombreador que implican el uso de una muestra (filtrado) tienen un modo que permite al sombreador pasar un parámetro de la abrazadera de float32 MinLOD adicional al ejemplo de textura. Este valor se encuentra en el espacio de número de mipmap de la vista, en lugar del recurso subyacente.

El hardware realiza ` max(fShaderMinLODClamp,fComputedLOD) ` en el mismo lugar en el cálculo de LOD en el que se produce la abrazadera MinLOD por recurso, que también es un [**Max**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-max)().

Si el resultado de aplicar la abrazadera de LOD por muestra y cualquier otra abrazadera de LOD definida en la muestra es un conjunto vacío, el resultado es el mismo que el resultado de acceso fuera del límite como la abrazadera minLOD por recurso: 0 para los componentes en el formato de superficie y los valores predeterminados para los componentes que faltan.

La instrucción LOD (por ejemplo, [**tex2Dlod**](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)), que preasigna la abrazadera de minLOD por muestra que se describe aquí, devuelve un LOD con compresión y no fija. El LOD fijado devuelto por esta instrucción LOD refleja todas las abrazaderas, incluida la abrazadera por recurso, pero no una abrazadera por muestra. De todos modos, el sombreador controla y conoce la abrazadera por muestra, por lo que el autor del sombreador puede aplicar manualmente esa abrazadera al valor devuelto de la instrucción LOD si lo desea.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrado de reducción mín./máx.


Las aplicaciones pueden optar por administrar sus propias estructuras de datos que les informan del aspecto de las asignaciones para un recurso de streaming. Un ejemplo sería una superficie que contiene un textura para contener información para cada mosaico en un recurso de streaming. Podría almacenar el primer LOD que está asignado en una ubicación de mosaico determinada. Al realizar un muestreo cuidadoso de esta estructura de datos de forma similar a como se pretende que se muestree el recurso de streaming, es posible que descubra cuál será el valor de LOD mínimo que se asigna por completo para una superficie de filtro de textura completa. Para facilitar este proceso, Direct3D 11,2 presenta un nuevo modo de muestra de uso general, filtrado mínimo y máximo.

La utilidad de filtrado mínimo/máximo para el seguimiento de LOD puede ser útil para otros propósitos, como, quizás el filtrado de superficies de profundidad.

El filtrado de reducción mínimo/máximo es un modo en los muestreadores que capturan el mismo conjunto de textura que un filtro de textura normal debería obtener. Pero en lugar de combinar los valores para generar una respuesta, devuelve el valor mínimo () o máximo () de textura recuperado, por componente (por ejemplo, el valor mínimo de todos los valores de R, independientemente del valor mínimo de todos los valores G, etc.).

Las operaciones Min/Max siguen las reglas de precisión aritmética de Direct3D. No importa el orden de las comparaciones.

Durante las operaciones de filtro que no son Min/Max, a veces el peso de un textura determinado termina siendo 0,0. Un ejemplo es una muestra lineal con coordenadas de textura que se encuentran directamente en un centro de textura; en ese caso, 3 otros textura (que pueden variar según el hardware) contribuyen al filtro, pero con 0 peso. En el caso de cualquiera de estos textura que sería 0 peso en un filtro distinto de Min/Max, si el filtro es Min/Max, estos textura todavía no contribuyen al resultado (y los pesos no afectan a la operación de filtro min/max).

La compatibilidad con esta característica depende de la compatibilidad de [nivel 2](tier-2.md) con los recursos de streaming.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 