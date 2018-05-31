---
author: mijacobs
title: Tamaños de pantalla y puntos de interrupción de diseño adaptativo
description: En lugar de optimizar tu interfaz de usuario de los muchos dispositivos en el ecosistema de Windows 10, te recomendamos diseñar para unas cuantas categorías de ancho clave denominadas puntos de interrupción.
ms.author: mijacobs
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0d84530c1a7c3795c566495c1eae121691b0766a
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
ms.locfileid: "1688941"
---
#  <a name="screen-sizes-and-breakpoints"></a>Tamaños de pantalla y puntos de interrupción

Las aplicaciones para UWP pueden ejecutarse en cualquier dispositivo que ejecute Windows 10, esto es, teléfonos, tabletas, equipos de escritorio, televisores y mucho más. Con un elevado número de destinos de dispositivo y tamaños de pantalla en el ecosistema de Windows 10, en lugar de optimizar tu interfaz de usuario para cada dispositivo, te recomendamos diseñar unas cuantas categorías de ancho clave (también denominadas "puntos de interrupción"): 
- Pequeño (menos de 640 píxeles)
- Medio (de 641 a 1007 píxeles)
- Grande (1008 píxeles o más)

> [!TIP]
> Cuando diseñes puntos de interrupción específicos, diseña la cantidad de espacio en pantalla disponible para tu aplicación (ventana de la aplicación), no el tamaño de la pantalla. Cuando la aplicación se ejecuta a pantalla completa, la ventana de la aplicación tiene el mismo tamaño que la pantalla, pero cuando la aplicación no está a pantalla completa, la ventana es menor que la pantalla.

## <a name="breakpoints"></a>Puntos de interrupción
Esta tabla describe las distintas clases de tamaño y los puntos de interrupción.

![Puntos de interrupción con diseño adaptativo](images/rsp-design/rspd-breakpoints.png)

<table>
<thead>
<tr class="header">
<th align="left">Clase de tamaño</th>
<th align="left">Puntos de interrupción</th>
<th align="left">Tamaño de pantalla (diagonal)</th>
<th align="left">Dispositivos</th>
<th align="left">Tamaños de la ventana</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td style="vertical-align:top;">Pequeño</td>
<td style="vertical-align:top;">640 píxeles o menos</td>
<td style="vertical-align:top;">De 4 a 6 pulgadas</td>
<td style="vertical-align:top;">Teléfonos</td>
<td style="vertical-align:top;">320x569, 360x640, 480x854</td>
</tr>
<tr class="odd">
<td style="vertical-align:top;">Medio</td>
<td style="vertical-align:top;">De 641 a 1007 píxeles</td>
<td style="vertical-align:top;">De 7 a 12 pulgadas</td>
<td style="vertical-align:top;">Tabléfono, tabletas, televisores</td>
<td style="vertical-align:top;">960x540</td>
</tr>
<tr class="even">
<td style="vertical-align:top;">Grande</td>
<td style="vertical-align:top;">1008 píxeles o más</td>
<td style="vertical-align:top;">13 pulgadas o más</td>
<td style="vertical-align:top;">PC, portátiles, Surface Hub</td>
<td style="vertical-align:top;">1024x640, 1366x768, 1920x1080</td>
</tr>
</tbody>
</table>

## <a name="general-recommendations"></a>Recomendaciones generales

### <a name="small"></a>Pequeño
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
- Considera la posibilidad de realizar más adaptaciones para las [experiencias con televisores](http://go.microsoft.com/fwlink/?LinkId=760736).

### <a name="large"></a>Grande
- Establece los márgenes izquierdo y derecho de la ventana en 24 píxeles para crear una separación visual entre los bordes izquierdo y derecho de la ventana de la aplicación.
- Sitúa los elementos de comando, como las [barras de la aplicación](../controls-and-patterns/app-bars.md) en la parte superior de la ventana de la aplicación.
- Usa hasta tres columnas o regiones.
- Muestra el cuadro de búsqueda.
- Coloca el [panel de navegación](../controls-and-patterns/navigationview.md) en modo acoplado para que siempre se muestre.

>[!TIP] 
> Con [**Continuum para teléfonos**](http://go.microsoft.com/fwlink/p/?LinkID=699431), los usuarios pueden conectar dispositivos móviles compatibles con Windows 10 a un monitor, un ratón y un teclado para que los teléfonos funcionen como un portátil. Ten en cuenta esta nueva funcionalidad al diseñar para puntos de interrupción específicos, un teléfono móvil no se mantendrá siempre en la clase de tamaño pequeño.

## <a name="effective-pixels-and-scale-factor"></a>Píxeles efectivos y factor de escala

Las aplicaciones para UWP escalan automáticamente tu interfaz de usuario para garantizar que tu aplicación sea legible en todos los dispositivos de Windows 10. Windows escala automáticamente cada pantalla en función de su valor de PPP (puntos por pulgada) y la distancia de visualización del dispositivo. Los usuarios pueden anular el valor predeterminado si se dirigen a la página **Configuración** > **Pantalla** > **Escala y distribución**. 
