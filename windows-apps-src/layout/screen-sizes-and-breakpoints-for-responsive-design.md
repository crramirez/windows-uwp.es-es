---
author: mijacobs
title: "Tamaños de pantalla y puntos de interrupción para el diseño adaptativo"
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7c992aa651069f6876aa920da88ada659480132e
ms.lasthandoff: 02/07/2017

---

#  <a name="screen-sizes-and-break-points-for-responsive-design"></a>Tamaños de pantalla y puntos de interrupción para el diseño adaptativo

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

El número de destinos de dispositivo y tamaños de pantalla en el ecosistema de Windows 10 es demasiado grande como para preocuparse de optimizar la interfaz de usuario para cada uno de ellos. En su lugar, se recomienda el diseño de unos anchos claves (también denominados "puntos de interrupción"): 360, 640, 1024 y 1366 epx.

> [!TIP]
> Al diseñar para puntos de interrupción específicos, diseña la cantidad de espacio en pantalla disponible para la aplicación (ventana de la aplicación). Cuando la aplicación se ejecuta en pantalla completa, la ventana de la aplicación tiene el mismo tamaño que la pantalla, pero, en otros casos, es más pequeña.
 

En esta tabla se describen las diferentes clases de tamaño y se proporcionan recomendaciones generales para personalizar esas clases de tamaño.

![Puntos de interrupción con diseño adaptativo](images/rsp-design/rspd-breakpoints.png)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Clase de tamaño</th>
<th align="left">pequeño</th>
<th align="left">mediano</th>
<th align="left">grande</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;">Tamaño de pantalla habitual (diagonal)</td>
<td style="vertical-align:top;">De 4 a 6 pulgadas&quot;</td>
<td style="vertical-align:top;">De 7 a 12 pulgadas o televisores</td>
<td style="vertical-align:top;">13 pulgadas o más</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Dispositivos habituales</td>
<td style="vertical-align:top;">Teléfonos</td>
<td style="vertical-align:top;">Tabléfono, tabletas, televisores</td>
<td style="vertical-align:top;">PC, portátiles, Surface Hub</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Tamaños de ventana comunes en píxeles efectivos</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
<td style="vertical-align:top;">960x540, 1024x640</td>
<td style="vertical-align:top;">1366x768, 1920x1080</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Puntos de interrupción de ancho de ventana en píxeles efectivos</td>
<td style="vertical-align:top;">640 píxeles o menos</td>
<td style="vertical-align:top;">De 641 a 1007 píxeles</td>
<td style="vertical-align:top;">1008 píxeles o más</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Recomendaciones generales</td>
<td style="vertical-align:top;"><ul>
<li>Centra los elementos de la pestaña.</li>
<li>Establece los márgenes de la ventana de la izquierda y derecha en 12 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.</li>
<li>Acopla las [barras de la aplicación](../controls-and-patterns/app-bars.md) a la parte inferior de la ventana para una mejor accesibilidad.</li>
<li>Usa una columna/región a la vez.</li>
<li>Usa un icono para representar la búsqueda (no mostrar un cuadro de búsqueda).</li>
<li>Coloca el [panel de navegación](../controls-and-patterns/nav-pane.md) en modo de superposición para ahorrar espacio en pantalla.</li>
<li>Si usas el [patrón de maestro y detalles](../controls-and-patterns/master-details.md), elige el modo de presentación apilada para ahorrar espacio en pantalla.</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Crea elementos de pestaña alineados a la izquierda.</li>
<li>Establece los márgenes izquierdo y derecho de la ventana en 24 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.</li>
<li>Sitúa los elementos de comando, como las [barras de la aplicación](../controls-and-patterns/app-bars.md) en la parte superior de la ventana de la aplicación.</li>
<li>Hasta dos columnas o regiones.</li>
<li>Muestra el cuadro de búsqueda.</li>
<li>Coloca el [panel de navegación](../controls-and-patterns/nav-pane.md) en modo de franja para que siempre se muestre en una tira estrecha de iconos.</li>
<li>Considera la posibilidad de realizar más adaptaciones para las [experiencias con televisores](http://go.microsoft.com/fwlink/?LinkId=760736).</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Crea elementos de pestaña alineados a la izquierda.</li>
<li>Establece los márgenes izquierdo y derecho de la ventana en 24 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.</li>
<li>Sitúa los elementos de comando, como las [barras de la aplicación](../controls-and-patterns/app-bars.md) en la parte superior de la ventana de la aplicación.</li>
<li>Hasta tres columnas o regiones.</li>
<li>Muestra el cuadro de búsqueda.</li>
<li>Coloca el [panel de navegación](../controls-and-patterns/nav-pane.md) en modo acoplado para que siempre se muestre.</li>
</ul></td>
</tr>
</tbody>
</table>

Con [**Continuum para teléfonos**](http://go.microsoft.com/fwlink/p/?LinkID=699431), una nueva experiencia de dispositivos móviles compatibles con Windows 10, los usuarios pueden conectar sus teléfonos a un monitor, un mouse y un teclado para que los teléfonos funcionen como un portátil. Ten en cuenta esta nueva funcionalidad al diseñar para puntos de interrupción específicos, un teléfono móvil no se mantendrá siempre en la clase de tamaño pequeño.
 

