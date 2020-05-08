---
Description: Responde a las entradas de mouse de tus aplicaciones controlando los mismos eventos de puntero básicos que usas para las entradas táctiles y de pluma.
title: Interacciones de mouse
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e0591a62134f09c1b3a9d115d038020e95f0c139
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970720"
---
# <a name="mouse-interactions"></a>Interacciones de mouse

Optimizar el diseño de la aplicación de aplicación de Windows para la entrada táctil y obtener compatibilidad básica con el mouse de forma predeterminada. 

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
<strong>Nota</strong>  : haga clic con el botón derecho para mostrar un menú contextual si la selección o los comandos de la barra de la aplicación no son comportamientos de interfaz de usuario apropiados. Pero te recomendamos encarecidamente que uses la barra de la aplicación para todos los comportamientos de comandos.
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

## <a name="mouse-input-events"></a>Eventos de entrada del mouse

La mayoría de la entrada del mouse se puede controlar a través de los eventos de entrada enrutados comunes admitidos por todos los objetos [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) . Entre ellas se incluyen las siguientes:

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Omisiones**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefound)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

Sin embargo, puede aprovechar las capacidades específicas de cada dispositivo (como los eventos de la rueda del mouse) mediante el puntero, el gesto y los eventos de manipulación de [Windows. UI. Input](https://docs.microsoft.com/uwp/api/windows.ui.input).

**Ejemplos:** Vea nuestro [ejemplo de BasicInput](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput), para.

## <a name="guidelines-for-visual-feedback"></a>Directrices para información visual

- Cuando se detecta un mouse (a través de eventos de movimiento o mantenimiento del mouse), muestra la interfaz de usuario específica del mouse para indicar las funciones expuestas por el elemento. Si el mouse no se mueve por un determinado período o el usuario inicia una interacción táctil, haz que la interfaz de usuario del mouse vaya desapareciendo gradualmente. Esto mantiene la interfaz de usuario ordenada y organizada.
- No uses el cursor para obtener información al mantener el mouse. La información proporcionada por el elemento es suficiente (consulta la sección Cursores más abajo).
- No muestres información visual si un elemento no admite interacción (por ejemplo, texto estático).
- No uses rectángulos de foco con las interacciones del mouse. Resérvalos para las interacciones del teclado.
- Muestra información visual simultáneamente para todos los elementos que representan el mismo destino de entrada.
- Proporciona botones (como + y -) para emular las manipulaciones táctiles, como desplazar lateralmente, girar, hacer zoom, etc.

Para obtener instrucciones más generales sobre la información visual, consulta el tema sobre las [directrices para información visual](guidelines-for-visualfeedback.md).

## <a name="cursors"></a>Cursores

Dispones de un conjunto de cursores estándar para un puntero del mouse. Estos se usan para indicar la acción principal de un elemento.

Cada cursor estándar tiene asociada una imagen predeterminada correspondiente. El usuario o una aplicación pueden reemplazar la imagen predeterminada asociada con cualquier cursor estándar en cualquier momento. Especifica una imagen de cursor mediante la función [**PointerCursor**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointercursor).

Si necesitas personalizar el cursor del mouse:

- Usa siempre el cursor de flecha (![cursor de flecha](images/cursor-arrow.png)) para los elementos clicables. no uses el cursor de mano que señala (![cursor de mano que señala](images/cursor-pointinghand.png)) para vínculos u otros elementos interactivos. En su lugar, usa efectos de mantenimiento del mouse (descritos anteriormente).
- Usa el cuadro de texto (![cursor de texto](images/cursor-text.png)) para el texto seleccionable.
- No uses el cursor de movimiento (![cursor de movimiento](images/cursor-move.png)) cuando la acción principal es un movimiento (por ejemplo, al arrastrar o recortar). No uses el cursor de movimiento para elementos en los que la acción principal es la navegación (por ejemplo, los iconos del Inicio).
- Usa los cursores de ajuste horizontal, vertical y diagonal (![cursor de ajuste vertical](images/cursor-vertical.png), ![cursor de ajuste horizontal](images/cursor-horizontal.png), ![cursor de ajuste diagonal (inferior izquierda, superior derecha)](images/cursor-diagonal2.png), ![cursor de ajuste diagonal (superior izquierda, inferior derecha)](images/cursor-diagonal1.png)cuando se puede ajustar el tamaño de un objeto.
- Usa los cursores de mano que sujeta (![cursor de mano que sujeta (abierta)](images/cursor-pan1.png), ![cursor de mano que sujeta (cerrada)](images/cursor-pan2.png)) al desplazar contenido lateralmente dentro de un lienzo fijo (como un mapa).

## <a name="related-articles"></a>Artículos relacionados

- [Controlar la entrada de puntero](handle-pointer-input.md)
- [Identificación de dispositivos de entrada](identify-input-devices.md)
- [Introducción a eventos y eventos enrutados](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

### <a name="samples"></a>Ejemplos

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
