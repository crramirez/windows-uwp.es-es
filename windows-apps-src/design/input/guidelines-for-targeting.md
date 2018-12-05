---
Description: This topic describes the use of contact geometry for touch targeting and provides best practices for targeting in Windows Runtime apps.
title: Selección de destinos
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6e8425232512650d5c80bf6fee9745b261aee8d9
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8738971"
---
# <a name="guidelines-for-targeting"></a>Directrices para la selección del destino


La selección táctil del destino en Windows usa toda el área de contacto de cada dedo que es detectado por un digitalizador táctil. El conjunto más grande y más complejo de datos de entrada notificado por el digitalizador se usa para aumentar la precisión cuando se determina el destino previsto (o con más probabilidades) del usuario.

> **API importantes**: [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

En este tema se describe el uso de la geometría de contacto para la selección táctil del destino y se ofrecen procedimientos recomendados para la selección del destino en aplicaciones para UWP.

## <a name="measurements-and-scaling"></a>Medidas y escalas


Para mantener la coherencia con distintos tamaños de pantalla y densidades de píxeles, todos los tamaños de destinos se representan en unidades físicas (milímetros). Puedes convertir las unidades físicas a píxeles mediante la siguiente ecuación:

Píxeles = densidad de píxeles × medida

En el ejemplo siguiente se usa esta fórmula para calcular el tamaño en píxeles de un destino de 9 mm en una pantalla de 135 píxeles por pulgada (PPP) y un nivel predefinido de escalado de 1x:

Píxeles = 135 PPP × 9 mm

Píxeles = 135 PPP × (0,03937 pulgadas por mm x 9 mm)

Píxeles = 135 PPP × 0,35433 pulgadas

Píxeles = 48 píxeles

Debes ajustar el resultado en función de cada nivel predefinido de escalado definido por el sistema.

## <a name="thresholds"></a>Umbrales


Los umbrales de distancia y tiempo pueden usarse para determinar el resultado de una interacción.

Por ejemplo, cuando se detecta un contacto táctil, se registra una pulsación si el objeto se arrastra menos de 2,7 mm desde el punto del contacto táctil y el toque se levanta al cabo de 0,1 segundos o menos desde el contacto táctil. Si se mueve el dedo más allá de este umbral de 2,7 mm, el objeto se arrastra y se selecciona o se mueve (para obtener más información, consulta las [instrucciones del deslizamiento transversal](guidelines-for-cross-slide.md)). Según tu aplicación, mantener presionado el dedo durante más de 0,1 segundos puede provocar que el sistema realice una interacción reveladora (para obtener más información, consulta las [instrucciones para comentarios visuales](guidelines-for-visualfeedback.md)).

## <a name="target-sizes"></a>Tamaños de los destinos


En general, establece el tamaño del destino táctil en 9 mm cuadrados o más (48 x 48 píxeles en una pantalla de 135 PPP a un nivel predefinido de escalado de 1.0x). Evita usar destinos táctiles de menos de 7 mm cuadrados.

En el diagrama siguiente se muestra que el tamaño del destino suele ser una combinación de un destino visual, el tamaño de destino real y el relleno entre el destino real y otros posibles destinos.

![diagrama en el que se muestran los tamaños recomendados para el destino visual, el destino real y el espaciado.](images/targeting-size.png)

En la tabla siguiente se ofrece una lista del tamaño mínimo y el tamaño recomendado para los componentes de un destino táctil.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Componente del destino</th>
<th align="left">Tamaño mínimo</th>
<th align="left">Tamaño recomendado</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Espaciado interno</td>
<td align="left">2 mm</td>
<td align="left">No es aplicable.</td>
</tr>
<tr class="even">
<td align="left">Tamaño del destino visual</td>
<td align="left">&lt;60% del tamaño real</td>
<td align="left">90-100% del tamaño real
<p>La mayoría de los usuarios no se darán cuenta de que un destino visual es táctil si tiene menos de 4,2 mm cuadrados (60% del tamaño del destino mínimo recomendado de 7 mm).</p></td>
</tr>
<tr class="odd">
<td align="left">Tamaño real del destino</td>
<td align="left">7 mm cuadrados</td>
<td align="left">Mayor que o igual a 9 mm cuadrados (48 x 48 px en 1x)</td>
</tr>
<tr class="even">
<td align="left">Tamaño total del destino</td>
<td align="left">11 x 11 mm (aproximadamente 60 px: tres unidades de cuadrícula de 20 px a 1x)</td>
<td align="left">13,5 x 13,5 mm (72 x 72 px a 1x)
<p>Esto implica que el tamaño del destino real y el espaciado combinados debe ser superior a sus respectivos mínimos.</p></td>
</tr>
</tbody>
</table>

 

Estas recomendaciones del tamaño de destino pueden ajustarse según sea necesario para un escenario en particular. Algunas de las consideraciones que se ofrecen con estas recomendaciones son las siguientes:

-   Frecuencia de interacciones táctiles: considera hacer que los destinos que se presionan de manera repetida o frecuente sean más grandes que el tamaño mínimo.
-   Consecuencia del error: los destinos que tienen consecuencias graves si se tocan por error deberán tener un mayor espaciado y colocarse más lejos del extremo del área de contenido. Esto se aplica especialmente a destinos que se tocan con frecuencia.
-   Posición en el área de contenido
-   Factor de forma y tamaño de la pantalla
-   Postura del dedo
-   Visualizaciones táctiles
-   Digitalizadores táctiles y de hardware

## <a name="targeting-assistance"></a>Asistencia para la selección de destinos


Windows ofrece asistencia para la selección de destinos en situaciones en las que no se pueden aplicar las recomendaciones respecto del tamaño mínimo o el espaciado que se señalan aquí; por ejemplo, hipervínculos en una página web, controles de calendario, listas desplegables y cuadros combinados o selección de texto.

Las mejoras en la plataforma para selección de destinos y los comportamientos de la interfaz de usuario se combinan con la información visual (interfaz de usuario para desambiguación) para aumentar la precisión y la confianza del usuario. Para obtener más información, consulta las [directrices para información visual](guidelines-for-visualfeedback.md).

Si un elemento táctil debe ser más pequeño que el tamaño mínimo recomendado para los destinos, puedes usar las técnicas siguientes para reducir al mínimo los problemas que puedan presentarse al seleccionar destinos.

## <a name="tethering"></a>Tethering


Tethering es una indicación visual (un conector desde un punto de contacto hasta el rectángulo de límite de un objeto) que se usa para indicar al usuario que está conectado a un objeto e interactúa con él, aunque el contacto de entrada no esté en contacto directo con el objeto. Esto puede ocurrir cuando:

-   Se detectó por primera vez un contacto táctil dentro de un umbral de proximidad a un objeto y este objeto se identificó como el destino más probable del contacto.
-   Se movió un contacto táctil alejándose de un objeto pero el contacto sigue estando dentro de un umbral de proximidad.

Esta función no se expone a los desarrolladores de aplicaciones para UWP con JavaScript.

## <a name="scrubbing"></a>Arrastrar


Arrastrar significa tocar en cualquier punto de un campo de destinos y deslizar el dedo para seleccionar el destino deseado sin levantarlo hasta llevarlo encima del destino buscado. Esta interacción también se denomina "activación de despegue", donde el objeto que se activa es el que se tocó último antes de levantar el dedo de la pantalla.

Usa las siguientes directrices cuando diseñes interacciones de arrastre:

-   El arrastre se usa en combinación con la interfaz de usuario de para desambiguación. Para obtener más información, consulta las [directrices para información visual](guidelines-for-visualfeedback.md).
-   El tamaño mínimo recomendado para un destino táctil de arrastre es 20 px (3,75 mm en tamaño de 1x).
-   El arrastre tiene prioridad cuando se ejecuta en una superficie que puede moverse panorámicamente, como una página web.
-   Los destinos de arrastre deben estar cerca.
-   Cuando el usuario aleja el dedo de un destino de arrastre arrastrándolo, la acción se cancela.
-   El tethering a un destino de arrastre se especifica si las acciones ejecutadas por el destino no son destructivas, por ejemplo cambiar de fecha en un calendario.
-   El tethering se especifica en una sola dirección, horizontal o vertical.

## <a name="related-articles"></a>Artículos relacionados


**Ejemplos**
* [Ejemplo de entrada básica](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Muestra de entrada de latencia baja](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Muestra de modo de interacción del usuario](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Muestra de elementos visuales de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Muestras de archivo**
* [Entrada: muestra de eventos de entrada de usuario de XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: muestra de funcionalidades del dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: muestra de prueba de acceso táctil](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [Muestra de desplazamiento, movimiento panorámico y zoom XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: ejemplo de entrada de lápiz simplificada](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Entrada: muestra de gestos de Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Entrada: muestra de manipulaciones y gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [Muestra de entrada táctil de DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 




