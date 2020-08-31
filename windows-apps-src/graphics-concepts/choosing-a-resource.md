---
title: Elección de un recurso
description: Obtenga información sobre cómo identificar y seleccionar los mejores recursos que se van a enlazar con diferentes fases de la canalización de gráficos 3D en la aplicación.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- Elección de un recurso
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 69527915988136854e201b82332c17f3e0134290
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094473"
---
# <a name="choosing-a-resource"></a>Elección de un recurso


Un recurso es una colección de datos utilizada por la canalización 3D. La creación de recursos y la definición de su comportamiento es el primer paso para programar la aplicación. En esta guía se tratan los temas básicos para elegir los recursos necesarios para la aplicación.

## <a name="span-ididentify_bindingspanspan-ididentify_bindingspanspan-ididentify_bindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>Identificación de las fases de canalización que necesitan recursos


El primer paso es elegir la fase de [canalización de gráficos](graphics-pipeline.md) (o fases) que usará un recurso. Es decir, identifique cada fase que va a leer los datos de un recurso, así como las fases que escribirán los datos en un recurso. Conocer las fases de canalización en las que se usarán los recursos determina las API a las que se llamará para enlazar el recurso a la fase.

En esta tabla se enumeran los tipos de recursos que se pueden enlazar a cada fase de canalización. Incluye si el recurso se puede enlazar como entrada o como salida.

| Fase de canalización  | Dentro/fuera | Resource               | Tipo de recurso                           |
|-----------------|--------|------------------------|-----------------------------------------|
| Ensamblador de entrada | En     | Búfer de vértices          | Buffer                                  |
| Ensamblador de entrada | En     | Búfer de índice           | Buffer                                  |
| Fases del sombreador   | En     | Sombreador: ResourceView    | Buffer, Texture1D, Texture2D, Texture3D |
| Fases del sombreador   | En     | Shader: búfer de constantes | Buffer                                  |
| Salida de flujo   | Fuera    | Buffer                 | Buffer                                  |
| Fusión de salida   | Fuera    | Representación: vista de destino     | Buffer, Texture1D, Texture2D, Texture3D |
| Fusión de salida   | Fuera    | Vista de profundidad/estarcido     | Texture1D, Texture2D                    |

 

## <a name="span-ididentify_usagespanspan-ididentify_usagespanspan-ididentify_usagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>Identificar cómo se utilizará cada recurso


Una vez que haya elegido las fases de canalización que va a usar la aplicación (y, por lo tanto, los recursos que necesitará cada fase), el siguiente paso es determinar cómo se usará cada recurso, es decir, si se puede tener acceso a un recurso mediante la CPU o la GPU.

El hardware en el que se ejecuta la aplicación tendrá como mínimo una CPU y una GPU. Para elegir un valor de uso, tenga en cuenta qué tipo de procesador debe leer o escribir en el recurso de las siguientes opciones.

| Resource Usage | Se puede actualizar mediante                    | Frecuencia de actualización |
|----------------|--------------------------------------|---------------------|
| Valor predeterminado        | GPU                                  | con poca frecuencia        |
| Dinámica        | CPU                                  | mayor          |
| Ensayo        | GPU                                  | N/D                 |
| Inmutable      | CPU (solo en el momento de creación de recursos) | N/D                 |

 

El uso predeterminado se debe usar para un recurso que se espera que la CPU actualice con poca frecuencia (menos de una vez por fotograma). Idealmente, la CPU nunca escribiría directamente en un recurso con uso predeterminado para evitar posibles penalizaciones de rendimiento.

El uso dinámico debe usarse en un recurso que la CPU actualice con relativa frecuencia (una o más por fotograma). Un escenario típico de un recurso dinámico sería crear búferes dinámicos de vértices y de índices que se rellenarían en tiempo de ejecución con datos sobre la geometría visible desde el punto de vista del usuario para cada fotograma. Estos búferes se utilizarían para representar solo la geometría visible para el usuario para ese marco.

El uso del almacenamiento provisional debe usarse para copiar datos en otros recursos y desde ellos. Un escenario típico sería copiar los datos de un recurso con uso predeterminado (al que la CPU no puede acceder) a un recurso con uso de almacenamiento provisional (al que la CPU puede tener acceso).

Los recursos inmutables deben usarse cuando los datos del recurso nunca cambien.

Otra forma de ver la misma idea es pensar en lo que hace una aplicación con un recurso.

| Cómo usa la aplicación el recurso     | Resource Usage       |
|---------------------------------------|----------------------|
| Cargar una vez y no actualizar nunca            | Inmutable o predeterminado |
| La aplicación rellena recursos repetidamente | Dinámica              |
| Representar en textura                     | Valor predeterminado              |
| Acceso de CPU de los datos de GPU                | Ensayo              |

 

Si no está seguro de qué uso elegir, empiece con el uso predeterminado, ya que se espera que sea el caso más común. Un búfer de constantes de sombreador es el tipo de recurso que siempre debería tener el uso predeterminado.

## <a name="span-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanspan-idresource_types_and_pipeline_stagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>Enlazar recursos a fases de canalización


Un recurso se puede enlazar a más de una fase de canalización al mismo tiempo, siempre que se cumplan las restricciones especificadas cuando se creó el recurso. Estas restricciones se especifican como marcas de uso, marcas de enlace o marcas de acceso a la CPU. Más concretamente, un recurso se puede enlazar como una entrada y una salida simultáneamente siempre y cuando no se pueda realizar la lectura y la escritura de una parte de un recurso al mismo tiempo.

Al enlazar un recurso, piense en cómo la GPU y la CPU tendrán acceso al recurso. Los recursos que están diseñados para un solo propósito (no se usan varias marcas de acceso de uso, enlace y CPU) serán más que probablemente resulten un mejor rendimiento.

Por ejemplo, considere el caso de un destino de representación que se usa como textura varias veces. Es posible que sea más rápido tener dos recursos: un destino de representación y una textura utilizada como un recurso de sombreador. Cada recurso usaría solo una marca de enlace, que indica "destino de representación" o "recurso de sombreador". Los datos se copiarán de la textura de representación-destino a la textura del sombreador.

Esta técnica en este ejemplo puede mejorar el rendimiento aislando la escritura de destino de representación de la lectura del sombreador-textura. La única manera de asegurarse es implementar ambos enfoques y medir la diferencia de rendimiento en una aplicación concreta.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos](resources.md)

 

 




