---
author: Karl-Bridge-Microsoft
Description: Respond to mouse input in your apps by handling the same basic pointer events that you use for touch and pen input.
title: Interacciones de mouse
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b79edc5499343498801081dd00554128c3b57eae
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5546247"
---
# <a name="mouse-interactions"></a>Interacciones de mouse


Optimiza el diseño de tu aplicación de la Plataforma universal de Windows (UWP) para la entrada táctil y obtén compatibilidad básica con mouse de manera predeterminada.

 

![mouse](images/input-patterns/input-mouse.jpg)



La entrada de mouse es ideal para las interacciones del usuario que requieren precisión al apuntar y hacer clic. Desde luego que la interfaz de usuario de Windows admite esta precisión inherente, ya que se ha optimizado para la imprecisa naturaleza de la entrada táctil.

Donde el mouse y la entrada táctil difieren es en la posibilidad de esta última de emular con más precisión la manipulación directa de los elementos de interfaz de usuario a través de gestos físicos realizados directamente sobre esos objetos (como deslizar rápidamente, deslizar, arrastrar, girar, etc.). Por lo general, las manipulaciones con un mouse requieren alguna otra prestación de la interfaz de usuario, como el uso de controladores de cambiar el tamaño o girar un objeto.

En este tema se describen las consideraciones de diseño para interacciones de mouse.

## <a name="the-uwp-app-mouse-language"></a>Lenguaje del mouse de la aplicación para UWP


Un escueto conjunto de interacciones del mouse se usan de forma coherente en todo el sistema.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Término</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Mantener el mouse para obtener información</p></td>
<td align="left"><p>Mantén el mouse sobre un elemento para mostrar información más detallada o elementos visuales didácticos (como información sobre herramientas) sin confirmar una acción.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Clic con el botón primario para acción principal</p></td>
<td align="left"><p>Haz clic con el botón primario sobre un elemento para invocar su acción principal (como iniciar una aplicación o ejecutar un comando).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Desplazar para cambiar de vista</p></td>
<td align="left"><p>Muestra barras de desplazamiento para moverte hacia arriba, hacia abajo, a la izquierda y a la derecha dentro de un área de contenido. Los usuarios pueden desplazarse haciendo clic en las barras de desplazamiento o girando la rueda del mouse. Las barras de desplazamiento pueden indicar la ubicación de la vista actual dentro del área de contenido (al desplazar lateralmente con entrada táctil se muestra una interfaz de usuario similar).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Clic con el botón secundario para seleccionar y ordenar</p></td>
<td align="left"><p>Haz clic con el botón secundario para mostrar la barra de navegación (si está disponible) y la barra de la aplicación con comandos globales. Haz clic con el botón secundario en un elemento para seleccionarlo y mostrar la barra de la aplicación con comandos contextuales para el elemento seleccionado.</p>
<div class="alert">
<strong>Nota</strong>con el botón secundario para mostrar un menú contextual si la aplicación o selección de barra de comandos no es comportamientos de la interfaz de usuario adecuados. Pero te recomendamos encarecidamente que uses la barra de la aplicación para todos los comportamientos de comandos.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>Comandos de la interfaz de usuario para hacer zoom</p></td>
<td align="left"><p>Muestra los comandos de la interfaz de usuario en la barra de la aplicación (como + y -), o bien presiona Ctrl y gira la rueda del mouse, para emular los gestos de reducir y ampliar.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Comandos de la interfaz de usuario para girar</p></td>
<td align="left"><p>Muestra los comandos de la interfaz de usuario en la barra de la aplicación o bien presiona Ctrl+Mayús y gira la rueda del mouse, para emular el gesto de girar. Gira el dispositivo propiamente dicho para girar toda la pantalla.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Clic con el botón primario y arrastrar para reorganizar</p></td>
<td align="left"><p>Haz clic con el botón primario en un elemento y arrástralo para moverlo.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Clic con el botón primario y arrastrar para seleccionar texto</p></td>
<td align="left"><p>Haz clic con el botón primario dentro de texto seleccionable y arrástralo para seleccionarlo. Haz doble clic para seleccionar una palabra.</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-events"></a>Eventos de mouse

Responde a las entradas de mouse de tus aplicaciones controlando los mismos eventos de puntero básicos que usas para las entradas táctiles y de pluma.

Usa los eventos de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) para implementar la funcionalidad básica de entrada sin tener que escribir un código para cada dispositivo de entrada. Sin embargo, todavía puedes sacar provecho de las funcionalidades especiales de cada dispositivo (por ejemplo, los eventos de rueda del mouse) con el puntero, con gestos y con los eventos de manipulación de este objeto.

**Muestras:** Consulta esta funcionalidad en acción en nuestras [muestras de aplicaciones](http://go.microsoft.com/fwlink/p/?LinkID=264996).


- [Entrada: muestra de funcionalidades del dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)

- [Muestra de entrada](http://go.microsoft.com/fwlink/p/?linkid=226855)

- [Entrada: gestos y manipulaciones con GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)

## <a name="guidelines-for-visual-feedback"></a>Instrucciones para la información visual


-   Cuando se detecta un mouse (a través de eventos de movimiento o mantenimiento del mouse), muestra la interfaz de usuario específica del mouse para indicar las funciones expuestas por el elemento. Si el mouse no se mueve por un determinado período o el usuario inicia una interacción táctil, haz que la interfaz de usuario del mouse vaya desapareciendo gradualmente. Esto mantiene la interfaz de usuario ordenada y organizada.
-   No uses el cursor para obtener información al mantener el mouse. La información proporcionada por el elemento es suficiente (consulta la sección Cursores más abajo).
-   No muestres información visual si un elemento no admite interacción (por ejemplo, texto estático).
-   No uses rectángulos de foco con las interacciones del mouse. Resérvalos para las interacciones del teclado.
-   Muestra información visual simultáneamente para todos los elementos que representan el mismo destino de entrada.
-   Proporciona botones (como + y -) para emular las manipulaciones táctiles, como desplazar lateralmente, girar, hacer zoom, etc.

Para obtener instrucciones más generales sobre la información visual, consulta el tema sobre las [directrices para información visual](guidelines-for-visualfeedback.md).


## <a name="cursors"></a>Cursores


Dispones de un conjunto de cursores estándar para un puntero del mouse. Estos se usan para indicar la acción principal de un elemento.

Cada cursor estándar tiene asociada una imagen predeterminada correspondiente. El usuario o una aplicación pueden reemplazar la imagen predeterminada asociada con cualquier cursor estándar en cualquier momento. Especifica una imagen de cursor mediante la función [**PointerCursor**](https://msdn.microsoft.com/library/windows/apps/br208273).

Si necesitas personalizar el cursor del mouse:

-   Usa siempre el cursor de flecha (![cursor de flecha](images/cursor-arrow.png)) para los elementos clicables. no uses el cursor de mano que señala (![cursor de mano que señala](images/cursor-pointinghand.png)) para vínculos u otros elementos interactivos. En su lugar, usa efectos de mantenimiento del mouse (descritos anteriormente).
-   Usa el cuadro de texto (![cursor de texto](images/cursor-text.png)) para el texto seleccionable.
-   No uses el cursor de movimiento (![cursor de movimiento](images/cursor-move.png)) cuando la acción principal es un movimiento (por ejemplo, al arrastrar o recortar). No uses el cursor de movimiento para elementos en los que la acción principal es la navegación (por ejemplo, los iconos del Inicio).
-   Usa los cursores de ajuste horizontal, vertical y diagonal (![cursor de ajuste vertical](images/cursor-vertical.png), ![cursor de ajuste horizontal](images/cursor-horizontal.png), ![cursor de ajuste diagonal (inferior izquierda, superior derecha)](images/cursor-diagonal2.png), ![cursor de ajuste diagonal (superior izquierda, inferior derecha)](images/cursor-diagonal1.png)cuando se puede ajustar el tamaño de un objeto.
-   Usa los cursores de mano que sujeta (![cursor de mano que sujeta (abierta)](images/cursor-pan1.png), ![cursor de mano que sujeta (cerrada)](images/cursor-pan2.png)) al desplazar contenido lateralmente dentro de un lienzo fijo (como un mapa).

## <a name="related-articles"></a>Artículos relacionados

* [Controlar la entrada de puntero](handle-pointer-input.md)
* [Identificar dispositivos de entrada](identify-input-devices.md)

**Muestras**
* [Ejemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Muestra de entrada de latencia baja](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Muestra de modo de interacción del usuario](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Muestras de archivo**
* [Entrada: muestra de funcionalidades del dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: muestra de eventos de entrada de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Muestra de desplazamiento, movimiento panorámico y zoom XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: gestos y manipulaciones con GestureRecognizer](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 
 

 




