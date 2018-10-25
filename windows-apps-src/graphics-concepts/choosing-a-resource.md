---
title: Elección de un recurso
description: Un recurso es una colección de datos que se usa en la canalización 3D.
ms.assetid: 6BAD6287-2930-42F8-BF51-69A379D1D2C3
keywords:
- Elección de un recurso
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8ddac5d69ce0c562129255832adfc49380946510
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5572384"
---
# <a name="choosing-a-resource"></a>Elección de un recurso


Un recurso es una colección de datos que se usa en la canalización 3D. La creación de recursos y la definición de su comportamiento constituyen el primer paso para la programación de la aplicación. En esta guía se tratan los aspectos básicos para elegir los recursos necesarios para la aplicación.

## <a name="span-ididentifybindingspanspan-ididentifybindingspanspan-ididentifybindingspanidentify-pipeline-stages-that-need-resources"></a><span id="Identify_Binding"></span><span id="identify_binding"></span><span id="IDENTIFY_BINDING"></span>Identificar las fases de canalización que necesitan recursos


El primer paso es elegir la o las fases de [canalización de gráficos](graphics-pipeline.md) que usarán un recurso. Es decir, identificar cada fase que va a leer datos de un recurso, así como las fases que escribirán datos en un recurso. Conocer las fases de canalización donde se usarán los recursos determina las API a las que se llamará para enlazar el recurso con la fase.

En esta tabla se enumeran los tipos de recursos que pueden enlazarse en cada fase de canalización. Incluye si el recurso puede enlazarse como una entrada o una salida.

| Fase de canalización  | Entrada/salida | Recurso               | Tipo de recurso                           |
|-----------------|--------|------------------------|-----------------------------------------|
| Ensamblador de entrada | Entrada     | Búfer de vértices          | Búfer                                  |
| Ensamblador de entrada | Entrada     | Búfer de índices           | Búfer                                  |
| Fases de sombreador   | Entrada     | Vista del recurso de sombreador    | Buffer, Texture1D, Texture2D, Texture3D |
| Fases de sombreador   | Entrada     | Búfer de constantes del sombreador | Búfer                                  |
| Salida de secuencia   | Salida    | Búfer                 | Búfer                                  |
| Fusión de salida   | Salida    | Vista de destino de representación     | Buffer, Texture1D, Texture2D, Texture3D |
| Fusión de salida   | Salida    | Vista de galería de símbolos de profundidad     | Texture1D, Texture2D                    |

 

## <a name="span-ididentifyusagespanspan-ididentifyusagespanspan-ididentifyusagespanidentify-how-each-resource-will-be-used"></a><span id="Identify_Usage"></span><span id="identify_usage"></span><span id="IDENTIFY_USAGE"></span>Identificar cómo se usará cada recurso


Una vez que hayas elegido las fases de canalización que usará la aplicación (y, por lo tanto, los recursos que requiere cada fase), el siguiente paso es determinar cómo se usará cada recurso, es decir, si la CPU o la GPU puede acceder a un recurso.

El hardware en el que se ejecuta la aplicación tendrá un mínimo de una CPU y una GPU. Para elegir un valor de uso, considera qué tipo de procesador necesita leer o escribir en el recurso de las siguientes opciones.

| Uso de recursos | Puede actualizarse mediante                    | Frecuencia de actualización |
|----------------|--------------------------------------|---------------------|
| Predeterminado        | GPU                                  | con poca frecuencia        |
| Dinámico        | CPU                                  | con frecuencia          |
| Provisional        | GPU                                  | n/d                 |
| Inmutable      | CPU (solo en tiempo de creación de recursos) | n/d                 |

 

El uso predeterminado debe usarse para un recurso que se espera sea actualizado por la CPU con poca frecuencia (menos de una vez por fotograma). En teoría, la CPU nunca escribiría directamente en un recurso con el uso predeterminado a fin de evitar posibles disminuciones de rendimiento.

El uso dinámico debe usarse para un recurso que la CPU actualiza con relativa frecuencia (una vez o más por fotograma). Una situación típica para un recurso dinámico sería crear búferes dinámicos de vértices e índices que se completarán en tiempo de ejecución con datos sobre la geometría visibles desde el punto de vista del usuario para cada fotograma. Estos búferes se usarían para representar solo la geometría visible para el usuario para ese fotograma.

El uso provisional debe usarse para copiar datos desde y hacia otros recursos. Una situación típica sería copiar datos en un recurso con el uso predeterminado (al cual la CPU no puede acceder) en un recurso con el uso provisional (al cual puede acceder la CPU).

Los recursos inmutables deberían usarse cuando los datos en el recurso no cambiarán nunca.

Otra forma de ver este concepto es pensar en lo que hace una aplicación con un recurso.

| Cómo la aplicación usa el recurso     | Uso de recursos       |
|---------------------------------------|----------------------|
| Cargar una vez y no actualizar nunca            | Inmutable o predeterminado |
| La aplicación rellena el recurso repetidamente | Dinámico              |
| Representar en la textura                     | Predeterminado              |
| Acceso de CPU a los datos de GPU                | Provisional              |

 

Si no estás seguro de qué uso elegir, empieza con el uso predeterminado, ya que se espera que se el caso más común. Un búfer de constantes de sombreador es el tipo de recurso que siempre debería tener un uso predeterminado.

## <a name="span-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanspan-idresourcetypesandpipelinestagesspanbinding-resources-to-pipeline-stages"></a><span id="Resource_Types_and_Pipeline_stages"></span><span id="resource_types_and_pipeline_stages"></span><span id="RESOURCE_TYPES_AND_PIPELINE_STAGES"></span>Enlace de recursos a fases de canalización


Un recurso puede enlazarse a más de una fase de canalización al mismo tiempo, siempre que se cumplan las restricciones especificadas cuando se creó el recurso. Estas restricciones se especifican como marcas de uso, marcas de enlace o marcas de acceso a la CPU. En concreto, un recurso puede enlazarse como entrada y salida simultáneamente, siempre que la parte de lectura y escritura de un recurso no se produzcan al mismo tiempo.

Al enlazar un recurso, piensa en cómo la CPU y la GPU accederán al recurso. Los recursos que están diseñados para un solo propósito (no uses las marcas de uso múltiple, de enlace y de acceso a la CPU) muy probablemente generarán un mejor rendimiento.

Por ejemplo, considera el caso de un destino de representación que se use como textura varias veces. Puede ser más rápido tener dos recursos: un destino de representación y una textura usada como recurso del sombreador. Cada recurso usaría solo una marca de enlace, indicado como "destino de representación" o "recurso del sombreador". Los datos se copiarían de la textura de destino de representación a la textura del sombreador.

La técnica que se incluye en este ejemplo puede mejorar el rendimiento al aislar la escritura del destino de representación de la lectura de la textura del sombreador. La única manera de estar seguros es implementar ambos enfoques y medir la diferencia de rendimiento en la aplicación en particular.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos](resources.md)

 

 




