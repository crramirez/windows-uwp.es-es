---
description: Cree aplicaciones de Windows con experiencias de interacción de usuario intuitivas y distintivas que estén optimizadas para touchpad, pero que sean funcionalmente coherentes en todos los dispositivos de entrada.
title: Interacciones del panel táctil
ms.assetid: CEDEA30A-FE94-4553-A7FB-6C1FA44F06AB
label: Touchpad interactions
template: detail.hbs
keywords: panel táctil,PTP,táctil,función táctil,puntero,entrada,interacción de usuario
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6b0e0a7e45ee63d845a1d5b0057d00da11e01c18
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035098"
---
# <a name="touchpad-design-guidelines"></a>Directrices para el diseño de panel táctil


Diseña tu aplicación de modo que los usuarios puedan interactuar con ella a través de un panel táctil. Un panel táctil combina la entrada multitáctil indirecta con la entrada precisa de un dispositivo señalador, como un mouse. Esta combinación hace que el panel táctil sea ideal tanto para una interfaz de usuario optimizada para entrada táctil como para los destinos de menor tamaño de las aplicaciones de productividad.

 

![panel táctil](images/input-patterns/input-touchpad.jpg)


Las interacciones del panel táctil requieren tres cosas:

-   Un panel táctil estándar o un panel táctil de precisión de Windows.

    Los paneles táctiles de precisión están optimizados para dispositivos de aplicaciones Windows. Permiten al sistema controlar ciertos aspectos de la experiencia del panel táctil de forma nativa, como el seguimiento de dedo y la detección de palma para ofrecer una experiencia más coherente en todos los dispositivos.

-   El contacto directo de uno o más dedos en el panel táctil.
-   Movimiento de los contactos táctiles (o la falta de movimiento, según un umbral de tiempo).

Los datos de entrada proporcionados por el panel táctil pueden ser:

-   Interpretados como un gesto físico para la manipulación directa de uno o más elementos de la interfaz de usuario (como el movimiento panorámico, girar, cambiar el tamaño o mover). Por el contrario, interactuar con un elemento a través de su ventana de propiedades u otro cuadro de diálogo se considera una manipulación indirecta.
-   Reconocidos como un método de entrada alternativo, como un mouse o un lápiz.
-   Usados para complementar o modificar aspectos de otros métodos de entrada, como difuminar un trazo de lápiz dibujado con un lápiz.

Un panel táctil combina la entrada multitáctil indirecta con la entrada precisa de un dispositivo señalador, como por ejemplo un mouse. Esta combinación hace que el panel táctil sea ideal tanto para la interfaz de usuario optimizada para entrada táctil como para los destinos típicamente más pequeños de las aplicaciones de productividad y el entorno de escritorio. Optimizar el diseño de la aplicación Windows para la entrada táctil y obtener compatibilidad con Touchpad de forma predeterminada.

Los paneles táctiles integran diversas experiencias de interacción, por lo que te recomendamos usar el evento [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered) para proporcionar comandos de interfaz de usuario de estilo mouse además de la compatibilidad integrada con la entrada táctil. Por ejemplo, usa botones Anterior y Siguiente para que los usuarios puedan pasar de una página de contenido a otra además de realizar movimientos panorámicos por el contenido.

Los gestos y las directrices que se explican en este tema te pueden ayudar a asegurarte de que tu aplicación admite la entrada del panel táctil sin problemas y con muy poco código.

## <a name="the-touchpad-language"></a>El idioma del panel táctil


Un escueto conjunto de interacciones del panel táctil se usan de forma coherente en todo el sistema. Optimiza tu aplicación para la entrada táctil y del mouse, y este idioma hará que los usuarios se sientan cómodos al instante, lo que aumentará su confianza y les permitirá aprender a usar tu aplicación con mayor facilidad.

Los usuarios pueden establecer muchos más comportamientos de interacción y gestos de panel táctil de precisión que puede hacer con un panel táctil estándar. Estas dos imágenes muestran las distintas páginas de configuración (Configuración &gt; Dispositivos &gt; Mouse y panel táctil) para un panel táctil estándar y un panel táctil de precisión, respectivamente.

![configuración del panel táctil estándar](images/mouse-touchpad-settings-standard.png)

<sup>Configuración estándar de \\ touchpad \\</sup>

![configuración del panel táctil de precisión de Windows](images/mouse-touchpad-settings-ptp.png)

<sup>Configuración de Touchpad de Windows \\ Precision \\ \\</sup>

Estos son algunos ejemplos de gestos optimizados para panel táctil para realizar tareas comunes.

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
<td align="left"><p>Pulsación de tres dedos</p></td>
<td align="left"><p>Preferencias de usuario para realizar búsquedas con <strong>Cortana</strong> o mostrar el <strong>Centro de actividades</strong>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Deslizamiento de tres dedos</p></td>
<td align="left"><p>Preferencia del usuario para abrir la vista de tarea de escritorio virtual, mostrar el Escritorio o cambiar entre aplicaciones abiertas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Pulsar con un dedo para la acción principal</p></td>
<td align="left"><p>Usa un solo dedo para pulsar en un elemento e invocar su acción principal (como iniciar una aplicación o ejecutar un comando).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Pulsar con dos dedos para hacer clic con el botón derecho</p></td>
<td align="left"><p>Pulsa con dos dedos al mismo tiempo en un elemento para seleccionarlo y mostrar los comandos contextuales.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Deslizar con dos dedos para realizar un movimiento panorámico</p></td>
<td align="left"><p>El deslizamiento se usa principalmente para interacciones de movimiento panorámico, pero también se puede usar para mover, dibujar o escribir.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Reducir y ampliar para hacer zoom</p></td>
<td align="left"><p>Los gestos de reducir y ampliar se usan comúnmente para el cambio de tamaño y el zoom semántico.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Presionar con un dedo y deslizar para reordenar</p></td>
<td align="left"><p>Arrastra un elemento.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Presionar con un dedo y deslizar para seleccionar texto</p></td>
<td align="left"><p>Presiona dentro de texto seleccionable y deslízalo para seleccionarlo. Pulsa dos veces para seleccionar una palabra.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Zona de clic con los botones primario y secundario</p></td>
<td align="left"><p>Simula las funciones de los botones primario y secundario de un dispositivo de mouse.</p></td>
</tr>
</tbody>
</table>

 

## <a name="hardware"></a>Hardware


Consulta las funciones del dispositivo de mouse ( [**MouseCapabilities**](/uwp/api/Windows.Devices.Input.MouseCapabilities)) para identificar cuáles son los aspectos de la interfaz de usuario de tu aplicación a los que el hardware del panel táctil puede acceder directamente. Te recomendamos que proporciones interfaces de usuario tanto para entrada táctil como para entrada del mouse.

Para obtener más información sobre cómo consultar las funciones de dispositivos, consulta [Identificar los dispositivos de entrada](identify-input-devices.md).

## <a name="visual-feedback"></a>Información visual


-   Cuando se detecte un cursor del panel táctil (a través de eventos de movimiento o mantenimiento del mouse), muestra la interfaz de usuario específica del mouse para indicar las funciones expuestas por el elemento. Si el cursor del panel táctil no se mueve por un determinado período o el usuario inicia una interacción táctil, haz que la interfaz de usuario del panel táctil vaya desapareciendo gradualmente. Esto mantiene la interfaz de usuario ordenada y organizada.
-   No use el cursor para los comentarios de mantener el mouse; los comentarios proporcionados por el elemento son suficientes (consulte la sección de cursores a continuación).
-   No muestres información visual si un elemento no admite interacción (por ejemplo, texto estático).
-   No uses rectángulos de foco con las interacciones del panel táctil. Resérvalos para las interacciones del teclado.
-   Muestra información visual simultáneamente para todos los elementos que representan el mismo destino de entrada.

Para obtener instrucciones más generales sobre la información visual, consulta el tema sobre las [directrices para información visual](./guidelines-for-visualfeedback.md).

## <a name="cursors"></a>Cursores


Hay disponible un conjunto de cursores estándar para un puntero del panel táctil. Estos se usan para indicar la acción principal de un elemento.

Cada cursor estándar tiene asociada una imagen predeterminada correspondiente. El usuario o una aplicación pueden reemplazar la imagen predeterminada asociada con cualquier cursor estándar en cualquier momento. Las aplicaciones para UWP especifican una imagen de cursor a través de la función [**PointerCursor**](/uwp/api/windows.ui.core.corewindow.pointercursor) .

Si necesitas personalizar el cursor del mouse:

-   Usa siempre el cursor de flecha (![cursor de flecha](images/cursor-arrow.png)) para los elementos clicables. no uses el cursor de mano que señala (![cursor de mano que señala](images/cursor-pointinghand.png)) para vínculos u otros elementos interactivos. En su lugar, usa efectos de mantenimiento del mouse (descritos anteriormente).
-   Usa el cuadro de texto (![cursor de texto](images/cursor-text.png)) para el texto seleccionable.
-   No uses el cursor de movimiento (![cursor de movimiento](images/cursor-move.png)) cuando la acción principal es un movimiento (por ejemplo, al arrastrar o recortar). No uses el cursor de movimiento para elementos en los que la acción principal es la navegación (por ejemplo, los iconos del Inicio).
-   Usa los cursores de ajuste horizontal, vertical y diagonal (![cursor de ajuste vertical](images/cursor-vertical.png), ![cursor de ajuste horizontal](images/cursor-horizontal.png), ![cursor de ajuste diagonal (inferior izquierda, superior derecha)](images/cursor-diagonal2.png), ![cursor de ajuste diagonal (superior izquierda, inferior derecha)](images/cursor-diagonal1.png)cuando se puede ajustar el tamaño de un objeto.
-   Usa los cursores de mano que sujeta (![cursor de mano que sujeta (abierta)](images/cursor-pan1.png), ![cursor de mano que sujeta (cerrada)](images/cursor-pan2.png)) al desplazar contenido lateralmente dentro de un lienzo fijo (como un mapa).

## <a name="related-articles"></a>Artículos relacionados

- [Control de la entrada con puntero](handle-pointer-input.md)
- [Identificación de dispositivos de entrada](identify-input-devices.md)

### <a name="samples"></a>Ejemplos

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Muestras de archivo

- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: muestra de eventos de entrada de usuario de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: gestos y manipulaciones con GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
