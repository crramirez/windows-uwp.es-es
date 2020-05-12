---
title: Tamaños de pantalla y puntos de interrupción de diseño adaptativo
description: En lugar de optimizar tu interfaz de usuario de los muchos dispositivos en el ecosistema de Windows 10, te recomendamos diseñar para unas cuantas categorías de ancho clave denominadas puntos de interrupción.
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f79b7247a7e1a1889c530a16c280f490db51042e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970750"
---
#  <a name="screen-sizes-and-breakpoints"></a>Tamaños de pantalla y puntos de interrupción

Las aplicaciones de Windows pueden ejecutarse en cualquier dispositivo que ejecute Windows 10, lo que incluye teléfonos, tabletas, equipos de escritorio, televisores, etc. Con un elevado número de destinos de dispositivo y tamaños de pantalla en el ecosistema de Windows 10, en lugar de optimizar tu interfaz de usuario para cada dispositivo, te recomendamos diseñar unas cuantas categorías de ancho clave (también denominadas "puntos de interrupción"): 
- Pequeño (menos de 640 píxeles)
- Medio (de 641 a 1007 píxeles)
- Grande (1008 píxeles o más)

> [!TIP]
> Cuando diseñes para puntos de interrupción específicos, diseña para la cantidad de espacio en pantalla disponible para tu aplicación (ventana de la aplicación), no el tamaño de la pantalla. Cuando la aplicación se ejecuta a pantalla completa, la ventana de la aplicación tiene el mismo tamaño que la pantalla, pero cuando la aplicación no está a pantalla completa, la ventana es menor que la pantalla.

## <a name="breakpoints"></a>Puntos de interrupción
En esta tabla se describen las distintas clases de tamaño y puntos de interrupción.

![Puntos de interrupción con diseño dinámico](images/breakpoints/size-classes.svg)

<table>
<thead>
<tr class="header">
<th align="left">Clase de tamaño</th>
<th align="left">Puntos de interrupción</th>
<th align="left">Tamaño de pantalla habitual (diagonal)</th>
<th align="left">Dispositivos</th>
<th align="left">Tamaños de la ventana</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Pequeña</td>
<td style="vertical-align:top;">640 píxeles o menos</td>
<td style="vertical-align:top;">4&quot; a 6&quot;; 20&quot; a 65&quot;</td>
<td style="vertical-align:top;">Teléfonos, televisores</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Medio</td>
<td style="vertical-align:top;">De 641 a 1007 píxeles</td>
<td style="vertical-align:top;">7&quot; a 12&quot;</td>
<td style="vertical-align:top;">Tabléfono, tabletas</td>
<td style="vertical-align:top;">960 x 540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Grande</td>
<td style="vertical-align:top;">1008 píxeles o más</td>
<td style="vertical-align:top;">13 pulgadas o más</td>
<td style="vertical-align:top;">PC, portátiles, Surface Hub</td>
<td style="vertical-align:top;">1024 x 640, 1366 x 768, 1920 x 1080</td>
</tr>
</tbody>
</table>

## <a name="why-are-tvs-considered-small"></a>¿Por qué los televisores se consideran "pequeños"? 

Aunque la mayoría de los televisores son físicamente bastante grandes (de 40 a 65 pulgadas es lo habitual) y tienen resoluciones altas (HD o 4k), el diseño para una televisión de 1080 píxeles que se ve a 3 m de distancia es diferente del diseño de un monitor de 1080 píxeles cuando se está sentado a una distancia de 30 cm en el escritorio. Cuando tienes en cuenta la distancia, los 1080 píxeles de los televisores son más similares al monitor 540 píxeles que está mucho más cerca.

El sistema de píxeles eficaz de UWP tiene en cuenta automáticamente la distancia de visualización. Al especificar un tamaño para un control o un intervalo de punto de interrupción, estás usando realmente píxeles "efectivos". Por ejemplo, si creas código dinámico para 1080 píxeles y superior, un monitor de 1080 usará ese código, pero un televisor de 1080 píxeles no lo hará porque, aunque un televisor de 1080 píxeles tiene 1080 píxeles físicos, solo tiene 540 píxeles efectivos. Lo que hace que el diseño para un televisor sea similar al diseño para un teléfono.

## <a name="effective-pixels-and-scale-factor"></a>Píxeles efectivos y factor de escala

Las aplicaciones para UWP escalan automáticamente tu interfaz de usuario para garantizar que tu aplicación sea legible en todos los dispositivos Windows 10. Windows escala automáticamente para cada pantalla en función de su valor de PPP (puntos por pulgada) y la distancia de visualización del dispositivo. Los usuarios pueden anular el valor predeterminado desde la página **Configuración** > **Pantalla** > **Escala y distribución**. 


## <a name="general-recommendations"></a>Recomendaciones generales

### <a name="small"></a>Pequeña
- Establece los márgenes de la ventana de la izquierda y derecha en 12 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.
- Acopla las [barras de la aplicación](../controls-and-patterns/app-bars.md) a la parte inferior de la ventana para una mejor accesibilidad.
- Usa una columna o región a la vez.
- Usa un icono para representar la búsqueda (no mostrar un cuadro de búsqueda).
- Coloca el [panel de navegación](../controls-and-patterns/navigationview.md) en modo de superposición para ahorrar espacio en pantalla.
- Si usas el [patrón de maestro y detalles](../controls-and-patterns/master-details.md), elige el modo de presentación apilada para ahorrar espacio en pantalla.

### <a name="medium"></a>Medio
- Establece los márgenes izquierdo y derecho de la ventana en 24 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.
- Sitúa los elementos de comando, como las [barras de la aplicación](../controls-and-patterns/app-bars.md) en la parte superior de la ventana de la aplicación.
- Usa hasta dos columnas o regiones.
- Muestra el cuadro de búsqueda.
- Coloca el [panel de navegación](../controls-and-patterns/navigationview.md) en modo de franja para que siempre se muestre en una tira estrecha de iconos.
- Considera la posibilidad de realizar más adaptaciones para las [experiencias con televisores](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN).

### <a name="large"></a>Grande
- Establece los márgenes izquierdo y derecho de la ventana en 24 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.
- Sitúa los elementos de comando, como las [barras de la aplicación](../controls-and-patterns/app-bars.md) en la parte superior de la ventana de la aplicación.
- Usa hasta tres columnas o regiones.
- Muestra el cuadro de búsqueda.
- Coloca el [panel de navegación](../controls-and-patterns/navigationview.md) en modo acoplado para que siempre se muestre.

>[!TIP] 
> Con [**Continuum para teléfonos**](https://docs.microsoft.com/windows-hardware/design/device-experiences/continuum-phone?redirectedfrom=MSDN), los usuarios pueden conectar dispositivos móviles compatibles con Windows 10 a un monitor, un ratón y un teclado para que los teléfonos funcionen como un portátil. Ten en cuenta esta nueva funcionalidad al diseñar para puntos de interrupción específicos, ya que un teléfono móvil no se mantendrá siempre en la clase de tamaño.


