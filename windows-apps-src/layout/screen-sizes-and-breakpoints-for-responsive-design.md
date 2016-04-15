---
title: 'Tamaños de pantalla y puntos de interrupción de diseño adaptativo'
description: .
ms.assetid: BF42E810-CDC8-47D2-9C30-BAA19DCBE2DA
label: Screen sizes and break points
template: detail.hbs
---

#  Tamaños de pantalla y puntos de interrupción de diseño adaptativo


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]



El número de destinos de dispositivo y tamaños de pantalla en el ecosistema de Windows 10 es demasiado grande como para preocuparse de optimizar la interfaz de usuario para cada uno de ellos. En su lugar, se recomienda el diseño de unos anchos claves (también denominados "puntos de interrupción"): 360, 640, 1024 y 1366 epx.

**Sugerencia**  Al diseñar para puntos de interrupción específicos, diseña la cantidad de espacio en pantalla disponible para la aplicación (ventana de la aplicación). Cuando la aplicación se ejecuta en pantalla completa, la ventana de la aplicación tiene el mismo tamaño que la pantalla, pero, en otros casos, es más pequeña.
 

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
<td align="left">Tamaño de pantalla habitual (diagonal)</td>
<td align="left">De 4 a 6 pulgadas</td>
<td align="left">De 7 a 12 pulgadas o televisores</td>
<td align="left">13 pulgadas y más</td>
</tr>
<tr class="even">
<td align="left">Dispositivos habituales</td>
<td align="left">Teléfonos</td>
<td align="left">Tabléfono, tabletas, televisores</td>
<td align="left">PC, portátiles, Surface Hub</td>
</tr>
<tr class="odd">
<td align="left">Tamaños de ventana comunes en píxeles efectivos</td>
<td align="left">320x569, 360x640, 480x854</td>
<td align="left">960x540, 1024x640</td>
<td align="left">1366x768, 1920x1080</td>
</tr>
<tr class="even">
<td align="left">Puntos de interrupción de ancho de ventana en píxeles efectivos</td>
<td align="left">640 píxeles o menos</td>
<td align="left">De 641 a 1007 píxeles</td>
<td align="left">1008 píxeles o más</td>
</tr>
<tr class="odd">
<td align="left" valign="top">Recomendaciones generales</td>
<td align="left" valign="top"><ul>
<li>Centra los elementos de la pestaña.</li>
<li>Establece los márgenes de la ventana de la izquierda y derecha en 12 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.</li>
<li>Acopla [app bars](../controls-and-patterns/app-bars.md) a la parte inferior de la ventana para una mejor accesibilidad.</li>
<li>Usa una columna/región a la vez.</li>
<li>Usa un icono para representar la búsqueda (no mostrar un cuadro de búsqueda).</li>
<li>Poner el [navigation pane](../controls-and-patterns/nav-pane.md) en modo de superposición para ahorrar espacio en pantalla.</li>
<li>Si estás usando el [master details pattern](../controls-and-patterns/master-details.md), usa el modo de presentación apilada para ahorrar espacio en pantalla.</li>
</ul></td>
<td align="left" valign="top"><ul>
<li>Crea elementos de pestaña alineados a la izquierda.</li>
<li>Establece los márgenes de la ventana de la izquierda y derecha en 24 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.</li>
<li>Sitúa los elementos de comando, como [app bars](../controls-and-patterns/app-bars.md), en la parte superior de la ventana de la aplicación.</li>
<li>Hasta dos columnas o regiones.</li>
<li>Muestra el cuadro de búsqueda.</li>
<li>Coloca el [navigation pane](../controls-and-patterns/nav-pane.md) en modo de plateado para que siempre se muestre en una tira estrecha de iconos.</li>

</ul></td>
<td align="left" valign="top"><ul>
<li>Crea elementos de pestaña alineados a la izquierda.</li>
<li>Establece los márgenes de la ventana de la izquierda y derecha en 24 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.</li>
<li>Sitúa los elementos de comando, como [app bars](../controls-and-patterns/app-bars.md), en la parte superior de la ventana de la aplicación.</li>
<li>Hasta tres columnas o regiones.</li>
<li>Muestra el cuadro de búsqueda.</li>
<li>Coloca el [navigation pane](../controls-and-patterns/nav-pane.md) en modo acoplado para que siempre se muestre.</li>
</ul></td>
</tr>
</tbody>
</table>

Con [**Continuum para teléfonos**](http://go.microsoft.com/fwlink/p/?LinkID=699431), una nueva experiencia de dispositivos móviles compatibles con Windows 10, los usuarios pueden conectar sus teléfonos a un monitor, un mouse y un teclado para que los teléfonos funcionen como un portátil. Ten en cuenta esta nueva funcionalidad al diseñar para puntos de interrupción específicos, un teléfono móvil no se mantendrá siempre en la clase de tamaño pequeño.
 


<!--HONumber=Mar16_HO4-->


