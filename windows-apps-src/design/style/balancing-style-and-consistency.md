---
description: Sugerencias para crear aplicaciones coherentes y utilizables que también demuestren originalidad y creatividad.
title: Equilibrio entre estilo y coherencia (diseño de aplicaciones de Windows)
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecfa771d8976b3c9e8cce8c8e9c45a5db5fb9b0a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220278"
---
# <a name="balancing-style-and-consistency"></a>Equilibrio entre coherencia y estilo

 

> Nota: Este artículo es un primer borrador para Windows 10 RS2. Los nombres de características, la terminología y la funcionalidad no son definitivos.

Cuando diseñas un producto, tú eres el promotor del cliente. Todos hacemos todo lo posible por crear el mejor diseño que sea apropiado y fiel a nuestras intenciones. En este artículo se explica el equilibrio entre las siguientes convenciones para crear una experiencia de usuario coherente en contraposición con la creación de características y experiencias únicas que diferencian tu aplicación. 

 
## <a name="the-importance-of-consistency"></a>La importancia de la coherencia
¿Por qué es importante la coherencia? La coherencia puede hacer que una aplicación sea más fácil de usar. Una parte fundamental de un diseño adecuado es la capacidad de aprendizaje; tener un diseño coherente entre las distintas aplicaciones reduce la cantidad de veces que el usuario final tiene que "volver a aprender" a usarlas. Mira los ejemplos del mundo real: el acelerador siempre está a la derecha, el picaporte siempre se gira hacia la izquierda para abrirlo, una señal de STOP siempre es roja. 

Sin embargo, hacer que una aplicación sea coherente no significa que todas las aplicaciones se vean y comportan de forma idéntica entre sí. Volviendo a los ejemplos del mundo real, hay un número casi infinito de diseños de sillas, pero muy pocos requieren volver a aprender cómo sentarse. Eso se debe a que los elementos importantes —la superficie para sentarse— son lo suficientemente coherentes como para que el usuario los entienda. 

Uno de los retos al crear el diseño de una buena aplicación es comprender dónde es importante ser coherente y dónde es importante ser original. 

## <a name="the-consistency-spectrum"></a>El espectro de coherencia
 Resulta útil pensar en la coherencia como un espectro con dos extremos opuestos:


![El espectro de coherencia](images/consistency/consistency-spectrum.png)

Entre los elementos de la experiencia familiar se incluyen:
-   Paradigmas establecidos de la interfaz de usuario (comportamiento al hacer clic con el mouse, mantener presionado en un elemento, iconos que tienen un aspecto similar)
-   Elementos de tu marca que quieres aplicar en tus productos (tipografía, colores)

Entre los diferenciadores se incluyen:
-   Los elementos que forman el "alma" única de los productos
-   Los elementos que te ayudan a adaptar la experiencia al factor de forma previsto

Vamos a crear un modelo de diseño tomando este espectro y aplicándolo a los elementos más importantes de una aplicación. 

![El modelo de coherencia del diseño](images/consistency/design-consistency-model.png)

En este modelo, las capas base ofrecen unos cimientos probados y demostrados de coherencia, mientras que las capas superiores se centran en la flexibilidad y la personalización.  

1. Los conceptos básicos de la aplicación conforman la primera capa de nuestro modelo: la cuadrícula de diseño, la [paleta de colores](color.md), las [convenciones tipográficas](typography.md) y los [estilos de los iconos](icons.md). Estas características fundamentales deben aplicarse de manera coherente. 

2. Para la segunda capa, UWP proporciona un conjunto básico de [controles comunes](../controls-and-patterns/index.md) que equilibra la eficiencia con la flexibilidad. También hemos creado directrices que ayudan a establecer una voz y un tono coherentes, usando las mismas palabras para describir y guiar a las personas por las experiencias de la aplicación. Hemos creado un conjunto de patrones que las aplicaciones pueden usar para su diseño, para asegurarnos de que los diseños puedan escalarse a dispositivos de distintos tamaños y entradas. 
3. La tercera capa es donde personalizas la navegación a distintos dispositivos y contextos. Por ejemplo, el modo en que te desplazas con la entrada táctil en un teléfono probablemente sea diferente que en un monitor de 32" con teclado y mouse, o en HoloLens, con gestos y 100 puntos de entrada táctil en una superficie de 84".
4. La cuarta capa es donde defines la personalidad de tu marca. ¿Qué elementos de diseño propio refuerzan la marca y la distinguen de la competencia? También es donde adaptas tu aplicación a diferentes usuarios finales. ¿Tu aplicación se dirige a jugadores, a trabajadores de la información o a profesores y alumnos de primaria y secundaria? ¿Cuáles son las necesidades propias de estos diferentes clientes y cómo podemos hacer que el diseño funcione mejor para ellos? No hagas más de lo mismo, sigue buscando crear más valor para los diferentes clientes.  


## <a name="design-principles"></a>Principios de diseño
Para usar este modelo de manera eficaz, necesitamos un conjunto de principios de diseño que nos ayuden a lograr el equilibrio adecuado. He aquí nuestros principios funcionales de coherencia en el diseño:

**Si se ve igual, haz que funcione igual**
-   Cuando el usuario ve un cuadro de texto o el control de hamburguesa, es muy probable que espere que se comporte de la misma manera en diferentes dispositivos. Si tienes una buena razón para desviarte de un comportamiento establecido, cumple con las expectativas del usuario haciendo que también se vea diferente.

**Si un elemento se ve de manera muy similar a un elemento o una convención existentes, considera la posibilidad de que haga lo mismo**
-   Necesitas un icono de "nuevo documento". Por qué diseñar uno nuevo que sea apenas un poco diferente, cuando puedes usar uno que el usuario ya reconozca.

**La facilidad de uso bate a la coherencia**
-   Es mejor que se pueda usar a que sea coherente. En algunos casos, puede que tengas que crear nuevos controles o comportamientos para facilitar el uso. El uso del teléfono con una sola mano presenta sus propios desafíos. Al igual que trabajar con una pantalla de 80". Un buen diseño hace que el usuario se sienta experto. 

**El atractivo es importante**
-   No hagas algo aburrido. Si todo es plano y tiene el mismo color y formas cuadradas, ¿querrán los clientes elegirlo y usarlo? Crea algo placentero. Introduce nuevos elementos que sorprendan sin interrumpir la capacidad de aprendizaje. 

**Los comportamientos evolucionan**
-   Este es un poco engañoso: a medida que el sector evoluciona, se establecen nuevas convenciones. Es posible que los comportamientos actuales se desvanezcan y que nuestros comportamientos coherentes tengan que adoptar nuevos estándares. Mira los gestos de acercar y alejar los dedos. Antes, lo normal era acercar/alejar con la interfaz de usuario +/-, pero en la interfaz de usuario moderna se espera acercar y alejar los dedos para aplicar zoom. Observa los nuevos paradigmas de experiencia y evoluciona. 
