---
title: Cadenas de intercambio
description: Una cadena de intercambio es un conjunto de búferes que se usan para mostrar fotogramas al usuario.
ms.assetid: A38E8BB7-1E77-4D93-B321-D3572A80D5DD
keywords:
- Cadenas de intercambio
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 486eb4adc1151bac1bf6a04a8f54b67530b426a3
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7692866"
---
# <a name="swap-chains"></a>Cadenas de intercambio


Una cadena de intercambio es una colección de búferes que se usan para mostrar fotogramas al usuario. Cada vez que una aplicación presenta un nuevo marco para mostrar, el primer búfer de la cadena de intercambio toma el lugar del búfer que se muestra. Este proceso se denomina *intercambio* o *desplazamiento*.

Un adaptador de gráficos mantiene un puntero en una superficie que representa la imagen que se muestra en el monitor, llamado búfer frontal. Cuando se actualiza el monitor, la tarjeta gráfica envía el contenido del búfer frontal al monitor para que se muestre. Sin embargo, esto produce un problema de "división" a la hora de representar gráficos en tiempo real. El problema principal es que las frecuencias de actualización de los monitores son muy lentas en comparación con el resto del equipo. Las frecuencias de actualización habituales van de los 60Hz (60 veces por segundo) a los 100Hz.

Si la aplicación actualiza el búfer frontal mientras el monitor está en medio de una actualización, la imagen que se muestra se cortará por la mitad, y la mitad superior de la pantalla contendrá la imagen anterior y la mitad inferior contendrá la imagen nueva. Este problema se conoce como *división*.

## <a name="span-idavoidingtearingspanspan-idavoidingtearingspanspan-idavoidingtearingspanavoiding-tearing"></a><span id="Avoiding_tearing"></span><span id="avoiding_tearing"></span><span id="AVOIDING_TEARING"></span>Evitar las divisiones


Direct3D implementa dos opciones para evitar las divisiones:

-   Una opción para permitir solamente las actualizaciones del monitor en la operación de retrazado vertical (o sincronización vertical). Un monitor normalmente actualiza su imagen moviendo un punto de luz horizontalmente, en zigzag desde la parte superior izquierda del monitor hasta la parte inferior derecha. Cuando el punto de luz llega al final, el monitor vuelve a calibrar dicho punto de la luz moviéndolo hacia atrás hasta la parte superior izquierda, para que el proceso pueda empezar de nuevo.

    Esta recalibración se denomina sincronización vertical. Durante una sincronización vertical, el monitor no dibuja nada, por lo que no se ven las actualizaciones en el búfer frontal hasta que el monitor comienza a dibujar otra vez. La sincronización vertical es relativamente lenta; sin embargo, no es tan lenta como para representar una escena compleja mientras se espera. Lo que se necesita para evitar las divisiones y poder representar escenas complejas es un proceso denominado almacenamiento en búfer de reserva.

-   Una opción es usar una técnica llamada almacenamiento en búfer de reserva. El almacenamiento en búfer de reserva es el proceso de dibujar una escena en una superficie fuera de la pantalla, denominada búfer de reserva. Cualquier superficie que no sea el búfer frontal se conoce como superficie fuera de la pantalla, ya que el monitor nunca la ve directamente.

    Mediante el uso de un búfer de reserva, una aplicación tiene la libertad de representar una escena siempre que el sistema esté inactivo (es decir, que no haya mensajes de ventanas en espera) sin necesidad de tener en cuenta la frecuencia de actualización del monitor. El almacenamiento en búfer de reserva produce una complicación: cómo y cuándo se debe pasar el búfer de reserva al búfer frontal.

## <a name="span-idsurfaceflippingspanspan-idsurfaceflippingspanspan-idsurfaceflippingspansurface-flipping"></a><span id="Surface_flipping"></span><span id="surface_flipping"></span><span id="SURFACE_FLIPPING"></span>Desplazamiento de la superficie


El proceso de mover el búfer de reserva al búfer frontal se denomina desplazamiento de la superficie. Como la tarjeta gráfica simplemente usa un puntero en una superficie para representar el búfer frontal, un sencillo cambio del puntero es todo lo que se necesita para establecer el búfer de reserva en el búfer frontal. Cuando una aplicación pide a Direct3D presentar el búfer de reserva en el búfer frontal, Direct3D simplemente "desplaza" los dos punteros de superficie. El resultado es que el búfer de reserva ahora es el nuevo búfer frontal, y el antiguo búfer frontal es el nuevo búfer de reserva.

Se invoca un desplazamiento de las superficies siempre que una aplicación solicita al dispositivo de Direct3D que presente el búfer de reserva; sin embargo, Direct3D puede configurarse para poner en cola las solicitudes hasta que se produzca una sincronización vertical. Esta opción se conoce como el intervalo de presentación del dispositivo de Direct3D. Los datos del nuevo búfer de reserva no pueden ser reutilizables, dependiendo de cómo una aplicación especifica el modo en que Direct3D debe administrar el desplazamiento de las superficies.

El desplazamiento de superficies es clave en el software multimedia, de animación y de juegos; es el equivalente a la manera en que se realiza animación con un bloc de notas. En cada página, el artista cambia las figuras ligeramente, para que cuando se desplacen rápidamente las hojas, el dibujo parezca animado.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Dispositivos](devices.md)

 

 




