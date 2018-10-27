---
title: Características de muestreo de texturas de los recursos de streaming
description: Las características de muestreo de texturas de los recursos de streaming incluyen la obtención de los comentarios de estado del sombreador sobre las áreas asignadas, la comprobación de si todos los datos a los que se accede se han asignado en el recurso, la compresión para ayudar a los sombreadores a evitar áreas de recursos de streaming con mapas MIP que se sabe que no se han asignado y la detección de que el LOD mínimo se ha asignado completamente para la totalidad de una superficie de filtro de texturas.
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Características de muestreo de texturas de los recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0066d38aaa3f5802ff5b1d380d405e60d90cad49
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698708"
---
# <a name="streaming-resources-texture-sampling-features"></a>Características de muestreo de texturas de los recursos de streaming


Las características de muestreo de texturas de los recursos de streaming incluyen la obtención de los comentarios de estado del sombreador sobre las áreas asignadas, la comprobación de si todos los datos a los que se accede se han asignado en el recurso, la compresión para ayudar a los sombreadores a evitar áreas de recursos de streaming con mapas MIP que se sabe que no se han asignado y la detección de que el LOD mínimo se ha asignado completamente para la totalidad de una superficie de filtro de texturas.

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Requisitas de muestreo de texturas de los recursos de streaming


Las características de muestreo de texturas descritas aquí requieren el [nivel 2](tier-2.md) de compatibilidad de recursos de streaming.

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Comentarios de estado de sombreadores sobre las áreas asignadas


Cualquier instrucción de sombreador que lee o escribe en un recurso de streaming hace que se registre información sobre el estado. Este estado se expone como un valor de retorno adicional opcional en cada instrucción de acceso de recurso que entra en un registro temporal de 32bits. El contenido del valor devuelto es opaco. Es decir, no se permite la lectura directa por parte del programa sombreador. No obstante, puedes usar la función [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) para extraer la información de estado.

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Comprobación de la asignación completa


La función [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) interpreta el estado devuelto desde un acceso a la memoria e indica si todos los datos a los que se tiene acceso se han asignado en el recurso. **CheckAccessFullyMapped** devuelve el valor true (0xFFFFFFFF) si los datos estaban asignados o false (0x00000000) si los datos estaban sin asignar.

Durante las operaciones de filtro, a veces el peso de un elemento de textura determinado termina siendo 0.0. Un ejemplo es una muestra lineal con coordenadas de textura que caigan directamente en el centro de un elemento de texto: otros 3 elementos de textura (que pueden variar según el hardware) contribuyen al filtro, pero con un peso de 0. Estos elementos de textura de peso O no contribuyen al resultado del filtro, por lo que si se producen, se colocan en iconos **NULL** y no cuentan como un acceso sin asignar. Ten en cuenta que la misma garantía se aplica a los filtros de textura que incluyen varios niveles de MIP; si los elementos de textura de uno de los mapas MIP no está asignado, pero el peso de esos elementos de textura es 0, estos no cuentan como un acceso sin asignar.

Al realizar un muestreo de un formato que tenga menos de 4 componentes (por ejemplo, DXGI\_FORMAT\_R8\_UNORM), cualquier elemento de textura que se sitúe en iconos **NULL** da como resultado la notificación de un acceso asignado **NULL**, independientemente de los componentes que el sombreador busque realmente en el resultado. Por ejemplo, la lectura desde R8\_UNORM y el enmascaramiento el resultado de la lectura en el sombreador con .gba/.yzw no parecerían necesitar leer la textura en absoluto. Sin embargo, si la dirección del elemento de textura es un icono asignado **NULL**, la operación aún se cuenta como un acceso de asignación **NULL**.

El sombreador puede comprobar el estado y tomar cualquier curso de acción en caso de error. Por ejemplo, un curso de acción puede ser registrar los "elementos faltantes"(por ejemplo, a través de escritura UAV) o emitir otra lectura comprimida a un LOD más amplio que se sepa que está asignado. Una aplicación podría querer realizar el seguimiento de los accesos correctos también, para hacerse una idea de a qué parte del conjunto asignado de los iconos se ha accedido.

Una complicación para el registro es que no existe ningún mecanismo para notificar el conjunto exacto de los iconos a los que se habría accedido. La aplicación puede hacer suposiciones conservadoras basadas en el conocimiento de las coordenadas que usa para acceder, así como usar la instrucción LOD; por ejemplo, [**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)) devuelve el cálculo del LOD de hardware.

Otra complicación es que un gran número de accesos será a los mismos iconos, por lo que se producirá mucho registro redundante y posiblemente contención en la memoria. Podría resultar conveniente si al hardware se le pudiera dar la opción de no molestarse en informar sobre los accesos a iconos si ya se han notificado antes en otra parte. Quizá el estado de dicho seguimiento podría restablecerse desde la API (probablemente en los límites del marco).

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Compresión MinLOD por muestra


Para ayudar a los sombreadores a evitar áreas en los recursos de streaming con mapas MIP que se sepa que no están asignadas, mayoría de las instrucciones de sombreador que implican el uso de una muestra (filtrado) tienen un modo que permite al sombreador pasar un parámetro de compresión MinLOD float32 adicional a la muestra de textura. Este valor está en el espacio de números de mapa MIP de la vista, en contra de lo que sucede en el recurso subyacente.

El hardware realiza` max(fShaderMinLODClamp,fComputedLOD) `en el mismo lugar en el cálculo de LOD donde se produce la compresión MinLOD por recurso, que es también un [**max**](https://msdn.microsoft.com/library/windows/desktop/bb509624)().

Si el resultado de aplicar la compresión LOD por muestra y otras compresiones LOD definidas en la muestra es un conjunto vacío, el resultado es el mismo resultado de accesos fuera de límites que en la compresión minLOD por recurso: 0 para los componentes en el formato de superficie y valores predeterminados para los componentes que faltan.

La instrucción LOD (por ejemplo, [**tex2Dlod**](https://msdn.microsoft.com/library/windows/desktop/bb509680)), que precede a la compresión minLOD por muestra descrita aquí, devuelve un LOD comprimido y un LOD sin comprimir. El LOD comprimido devuelto desde esta instrucción LOD refleja toda la compresión, incluida la compresión por recurso, pero no una compresión por muestra. La compresión por muestra la controla el sombreador y la conoce en cualquier caso, para que el creador del sombreador pueda aplicar manualmente esa compresión al valor devuelto de la instrucción LOD si lo desea.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrado de reducción mínimo/máximo


Las aplicaciones pueden elegir administrar sus propias estructuras de datos que les notifique la apariencia de las asignaciones correspondientes a un recurso de streaming. Un ejemplo sería una superficie que contenga un elemento de textura para contener información relativa a cada icono de un recurso de streaming. Se podría almacenar el primer LOD asignado en una ubicación de icono determinada. Al realizar un muestreo detallado de esta estructura de datos de manera similar al modo en que se pretende realizar el muestreo del recurso de streaming, se podría detectar cuál será el LOD mínimo que está asignado completamente para la totalidad de una superficie de filtro de textura. Para ayudar a facilitar este proceso, Direct3D 11.2 presenta un nuevo modo de muestra de propósito general, el filtrado mínimo/máximo.

La utilidad del filtrado mínimo/máximo para el seguimiento del LOD puede resultar útil para otros fines, como, quizá, el filtrado de las superficies de profundidad.

El filtrado reducción mínimo/máximo es un modo presente en las muestras que obtiene el mismo conjunto de elementos textura que capturaría un filtro de texturas normal. Sin embargo, en lugar fusionar los valores para generar una respuesta, devuelve el min() o max() de los elementos de textura capturados, componente a componente (por ejemplo, el mínimo de todos los valores de R por separado del mínimo de todos los valores de G, y así sucesivamente).

Las operaciones de mínimo/máximo siguen las reglas de precisión aritmética de Direct3D. El orden de las comparaciones no importa.

Durante las operaciones de filtro que no son de mínimo/máximo, a veces el peso de un determinado elemento de textura termina siendo 0,0. Un ejemplo es una muestra lineal con coordenadas de textura que caigan directamente en el centro de un elemento de texto; es ese caso, otros 3 elementos de textura (que pueden variar según el hardware) contribuyen al filtro, pero con un peso de 0. Para cualquiera de estos elementos de textura cuyo peso sería 0 en un filtro de no mínimo/máximo, estos elementos de textura siguen sin contribuir al resultado (y los pesos no se afectan de ningún otro modo a la operación de filtro mínimo/máximo).

La compatibilidad de esta característica depende de la compatibilidad de [nivel 2](tier-2.md) de los recursos de streaming.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




