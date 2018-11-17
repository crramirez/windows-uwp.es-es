---
author: jwmsft
ms.assetid: F912161D-3767-4F35-88C0-E1ECDED692A2
title: Mejorar el rendimiento de la recolección de elementos no usados
description: La memoria de las aplicaciones para la Plataforma universal de Windows (UWP) escritas en C# y Visual Basic se administra de manera automática mediante el recolector de elementos no usados de .NET. En esta sección se resume el comportamiento y los procesos recomendados de rendimiento del recolector de elementos no utilizados de .NET para las aplicaciones para UWP.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 31279de84b8f00e4489a7aae962caa231bb16dc1
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7150513"
---
# <a name="improve-garbage-collection-performance"></a>Mejorar el rendimiento de la recolección de elementos no usados


La memoria de las aplicaciones para la Plataforma universal de Windows (UWP) escritas en C# y Visual Basic se administra de manera automática mediante el recolector de elementos no usados de .NET. En esta sección se resume el comportamiento y los procesos recomendados de rendimiento del recolector de elementos no usados de .NET para las aplicaciones para UWP. Para más información sobre el funcionamiento del recolector de elementos no utilizados de .NET y las herramientas para depurar y analizar su rendimiento, consulta [Recolección de elementos no utilizados](https://msdn.microsoft.com/library/windows/apps/xaml/0xy59wtx.aspx).

**Nota**necesidad de intervenir en el comportamiento predeterminado del recolector de elementos no utilizados es una clara señal de problemas de memoria general con la aplicación. Para más información, consulta [Herramienta de uso de memoria durante la depuración en Visual Studio2015](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/13/memory-usage-tool-while-debugging-in-visual-studio-2015.aspx). Este tema solo se aplica a C# y Visual Basic.

 

El recolector de elementos no utilizados determina cuándo ejecutarse buscando un equilibrio entre el consumo de memoria del montón administrado y cantidad de trabajo que debe realizar la recolección de elementos no utilizados. Uno de los modos en que el recolector de elementos no utilizados hace esto es dividiendo el montón en generaciones y recolectando solo parte del montón la mayor parte del tiempo. Hay tres generaciones en el montón administrado:

-   Generación 0. Esta generación contiene los objetos asignados recientemente, a menos que su tamaño sea de 85KB o superior, en cuyo caso forman parte del montón de objetos grandes. El montón de objetos grandes se recolecta con las recolecciones de generación 2. Las recolecciones de generación 0 son el tipo de recolección más frecuente y limpian los objetos de corta duración, como las variables locales.
-   Generación 1. Esta generación contiene objetos que han sobrevivido a las recolecciones de generación 0. Sirve como búfer entre las generaciones 0 y 2. Las recolecciones de generación 1 tienen lugar con menor frecuencia que las de generación 0 y limpian los objetos temporales que se encontraban activos durante las recolecciones de generación 0 anteriores. Las recolecciones de generación 1 también recolectan la generación 0.
-   Generación 2. Esta generación contiene los objetos de larga duración que han sobrevivido a las recolecciones de las generaciones 0 y 1. Las colecciones de generación 2 son las menos frecuentes y recolectan todo el montón administrado, incluido el montón de objetos de gran tamaño que contiene objetos cuyo tamaño es de 85KB o superior.

Puedes medir el rendimiento del recolector de elementos no usados en relación con dos aspectos: el tiempo que tarda la recolección de elementos no usados y el consumo de memoria del montón administrado. Si tienes una aplicación pequeña con un tamaño de montón inferior a 100MB, céntrate en reducir el consumo de memoria. Si tienes una aplicación con un montón administrado superior a 100MB, céntrate solo en reducir el tiempo de la recolección de elementos no usados. A continuación se muestra cómo mejorar el rendimiento del recolector de elementos no usados de .NET.

## <a name="reduce-memory-consumption"></a>Reducir el consumo de memoria

### <a name="release-references"></a>Liberar las referencias

Si un objeto de tu aplicación tiene una referencia, dicho objeto y todos los objetos a los que hace referencia no pueden recolectarse. El compilador de .NET realiza un buen trabajo al detectar cuándo una variable ya no se usa. De este modo, los objetos que contienen esa variable se habilitarán para la recolección. Pero, en algunos casos, es posible que no sea evidente que algunos objetos tienen una referencia a otros objetos, porque parte del gráfico de objeto puede pertenecer a bibliotecas que usa tu aplicación. Para conocer las herramientas y las técnicas que te permiten averiguar qué objetos sobreviven a una recolección de elementos no usados, consulta [Recolección de elementos no usados y rendimiento](https://msdn.microsoft.com/library/windows/apps/xaml/ee851764.aspx).

### <a name="induce-a-garbage-collection-if-its-useful"></a>Inducir una recolección de elementos no utilizados si es útil

Induce una recolección de elementos no utilizados solamente si has medido el rendimiento de tu aplicación y si has determinado que esto lo mejorará.

Para inducir una recolección de elementos no usados de una generación, llama a [**GC.Collect(n)**](https://msdn.microsoft.com/library/windows/apps/xaml/y46kxc5e.aspx), donde n es la generación que quieres recolectar (0, 1 o 2).

**Nota**se recomienda no forzar una recolección en tu aplicación, ya que el recolector de elementos no usados usa muchas medidas heurísticas para determinar el mejor momento para realizar una recolección y forzar una colección es en muchos casos, un uso innecesario de la CPU. Pero si sabes que tienes una gran cantidad de objetos en la aplicación que ya no se usan y quieres devolver esta memoria al sistema, puede resultar conveniente forzar una recolección de elementos no utilizados. Por ejemplo, puedes inducir una recolección al final de una secuencia de carga en un juego para liberar memoria antes de comenzar la partida.
 
Para evitar inducir accidentalmente demasiadas recolecciones de elementos no utilizados, puedes establecer el valor de [**GCCollectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/bb495757.aspx) en **Optimized**. Esto indica al recolector de elementos no utilizados que debe iniciar una recolección solo si determina que será lo suficientemente productiva como para justificar su ejecución.

## <a name="reduce-garbage-collection-time"></a>Reducir el tiempo de recolección de elementos no utilizados

Esta sección se aplica si has analizado tu aplicación y has observado que tarda mucho en recolectar los elementos no usados. El tiempo de pausa relacionado con la recolección de elementos no usados abarca: el tiempo que se tarda en ejecutar una sola fase de recolección de elementos no usado y el tiempo total que tarda la aplicación en realizar las recolecciones de elementos no usados. El tiempo que se tarda en realizar una recolección depende de la cantidad de datos activos que debe analizar el recolector. El tamaño de las generaciones 0 y 1 es limitado, pero la generación 2 crece a medida que se activan objetos de larga duración en la aplicación. Esto significa que el tiempo de recolección para las generaciones 0 y 1 son limitados, mientras que las recolecciones de generación 2 pueden tardar más tiempo. La frecuencia con la que se ejecutan las recolecciones de elementos no usados depende principalmente de la cantidad de memoria que se asigna, porque la recolección de elementos no usados libera memoria para cumplir con las solicitudes de asignación.

En ocasiones, el recolector de elementos no utilizados pausa la aplicación para llevar a cabo el trabajo, pero no necesariamente la pausa todo el tiempo que tarda en realizar la recolección. Los tiempos de pausa no suelen ser perceptibles para el usuario en la aplicación, especialmente en las recolecciones de las generaciones 0 y 1. La característica de [recolección de elementos no utilizados en segundo plano](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#background-garbage-collection) del recolector de elementos no utilizados de .NET permite que las recolecciones de generación 2 se realicen al mismo tiempo que se ejecuta la aplicación y, de este modo, la aplicación solo se pausa por períodos cortos. Pero no siempre es posible realizar una recolección de generación 2 como una recolección en segundo plano. En ese caso, el usuario puede percibir la pausa si tienes un montón demasiado grande (más de 100 MB).

Las recolecciones de elementos no usados frecuentes pueden contribuir a un mayor consumo de CPU (y, por lo tanto, de energía), tiempos de carga más prolongados o una menor velocidad de fotogramas en la aplicación. A continuación se ofrecen algunas técnicas que puedes usar para reducir el tiempo de recolección de elementos no usados y las pausas relacionadas con la recolección en tu aplicación para UWP.

### <a name="reduce-memory-allocations"></a>Reducir las asignaciones de memoria

Si no asignas ningún objeto, el recolector de elementos no utilizados no se ejecutará a menos que se detecte una situación de memoria insuficiente en el sistema. La reducción de la cantidad de memoria asignada se traduce directamente en recolecciones de elementos no utilizados menos frecuentes.

Si en algunas secciones de tu aplicación no quieres que haya ningún tipo de pausa, puedes preasignar los objetos necesarios con antelación durante un momento en que el rendimiento no sea tan importante. Por ejemplo, un juego podría asignar todos los objetos necesarios para la partida durante la pantalla de carga de un nivel en lugar de realizar las asignaciones durante la partida propiamente dicha. Esto evita pausas mientras el usuario está jugando y da como resultado una velocidad de fotogramas más alta y coherente.

### <a name="reduce-generation-2-collections-by-avoiding-objects-with-a-medium-length-lifetime"></a>Reducir recolecciones de generación 2 al evitar objetos de mediana duración

Las recolecciones de elementos no utilizados generacionales se realizan más eficazmente cuando la aplicación cuenta con objetos cuya duración es realmente corta o realmente larga. Los objetos de corta duración se recolectan en las recolecciones de las generaciones 0 y 1, que consumen menos recursos, y los objetos de larga duración se promueven a la generación 2, que se recolecta con menor frecuencia. Los objetos de larga duración son los que se usan durante todo el ciclo de vida de la aplicación, o durante una período significativo, por ejemplo, durante una página o nivel de juego específicos.

Si con frecuencia creas objetos que tienen una duración temporal, pero duran lo suficiente como para avanzar a la generación 2, se producirán más recolecciones de generación 2, las cuales consumen más recursos. Es posible que puedas reducir el tiempo de las recolecciones de generación 2 si reciclas los objetos existentes o si los eliminas con mayor rapidez.

Un ejemplo común de objetos de mediana duración son los objetos que se usan para mostrar elementos en una lista por la que se desplaza un usuario. Si se crean objetos cuando aparecen los elementos de la lista y se deja de hacer referencia a ellos tan pronto como se desplazan fuera de la vista, tu aplicación tendrá una gran cantidad de recolecciones de generación 2. En este tipo de situaciones, puedes preasignar un conjunto de objetos y reutilizarlos para los datos que se muestran al usuario de manera activa y usar objetos de corta duración para cargar la información a medida que aparecen los elementos de la lista.

### <a name="reduce-generation-2-collections-by-avoiding-large-sized-objects-with-short-lifetimes"></a>Reducir recolecciones de generación 2 al evitar objetos de gran tamaño con duraciones cortas

Los objetos de 85KB o mayores se asignan al montón de objetos grandes (LOH) y se recolectan como parte de la generación 2. Si tienes variables temporales como, por ejemplo, búferes, cuyo tamaño supera los 85KB, una recolección de generación 2 las eliminará. Al limitar las variables temporales a menos de 85KB, se reduce el número de recolecciones de generación 2 en la aplicación. Una técnica común consiste en crear una grupo de búferes y volver a usar los objetos del grupo para evitar asignaciones temporales de gran tamaño.

### <a name="avoid-reference-rich-objects"></a>Evitar objetos con muchas referencias

El recolector de elementos no utilizados sigue las referencias entre los objetos desde las raíces de la aplicación para determinar qué objetos están activos. Para más información, consulta el tema que explica [lo que sucede durante una recolección de elementos no utilizados](https://msdn.microsoft.com/library/windows/apps/xaml/ee787088.aspx#what-happens-during-a-garbage-collection). Si un objeto contiene muchas referencias, el recolector de elementos no utilizados deberá realizar una mayor cantidad de trabajo. Una técnica común (especialmente con los objetos grandes) consiste en convertir los objetos con muchas referencias en objetos sin referencias (por ejemplo, en lugar de almacenar una referencia, almacena un índice). Obviamente, esta técnica solo funciona cuando es posible hacerlo de forma lógica.

El reemplazo de referencias de objeto por índices puede implicar una modificación complicada y perjudicial en la aplicación, y es más eficaz en objetos grandes con una gran cantidad de referencias. Hazlo solamente si notas tiempos de recolección de elementos no utilizados prolongados en la aplicación relacionados con objetos con muchas referencias.

 

 




