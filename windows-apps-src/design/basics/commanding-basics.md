---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Conceptos básicos sobre el diseño de comandos para las aplicaciones para la Plataforma universal de Windows (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07b9ce7b5a57f6dc1ba202ed57e8b2d4d93e583f
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654394"
---
#  <a name="command-design-basics-for-uwp-apps"></a>Conceptos básicos de diseño de los comandos para las aplicaciones para UWP

En una aplicación para la Plataforma universal de Windows (UWP), los *elementos de comandos* son los elementos interactivos de la interfaz de usuario que permiten a los usuarios realizar acciones como enviar un correo electrónico, eliminar un elemento o enviar un formulario. 

En este artículo se describen los elementos de comandos comunes, las interacciones que admiten y las superficies de comando para alojarlos.

![elementos de comandos en la aplicación Mapas](images/maps.png)

Anteriormente se muestran ejemplos de elementos de comando en la aplicación Mapas.

## <a name="provide-the-right-type-of-interactions"></a>Proporcionar el tipo correcto de interacciones

Al diseñar una interfaz de comandos, la decisión más importante es elegir lo que los usuarios deberían poder hacer. Para planear el tipo correcto de interacciones, céntrate en la aplicación: ten en cuenta las experiencias de usuario que deseas habilitar y qué pasos tendrán que dar los usuarios. Cuando hayas decidido lo que quieres que logren los usuarios, puedes proporcionarles las herramientas para que lo hagan.

Algunas interacciones que podrías querer que tu aplicación proporcione incluyen:

- Enviar información 
- Seleccionar la configuración y las opciones
- Buscar contenido y filtrarlo
- Abrir, guardar y eliminar archivos
- Editar y crear contenido

## <a name="use-the-right-command-element-for-the-interaction"></a>Usa el elemento de comando adecuado para la interacción

Si se usan los elementos adecuados para habilitar las interacciones de comandos, se puede marcar la diferencia entre una aplicación intuitiva y fácil de usar y otra que parezca difícil o confusa. La Plataforma universal de Windows (UWP) ofrece un gran conjunto de elementos de comandos que puedes usar en tu aplicación. Esta es una lista de algunos de los controles más habituales y un resumen de las interacciones que pueden habilitar.

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Categoría</th>
<th align="left">Elementos</th>
<th align="left">Interacción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Botones</b><br/><br/>
    <img src="../controls-and-patterns/images/controls/button.png" alt="button" /></td>
<td align="left"><a href="../controls-and-patterns/buttons.md">Botón</a></td>
<td align="left">Desencadena una acción inmediata. Los ejemplos incluyen enviar un correo electrónico, enviar datos de un formulario o confirmar una acción en un cuadro de diálogo.</td>
</tr>
<tr class="even">
<td align="left">Listas<br/><br/>
    <img src="../controls-and-patterns/images/controls/combo-box-open.png" alt="drop down list" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">lista desplegable, cuadro de lista, vista de lista y vista de cuadrícula</a></td>
<td align="left">Presenta los elementos en una lista interactiva o una cuadrícula. Por lo general, se usa para muchas opciones o para mostrar elementos.</td>
</tr>
<tr class="odd">
<td align="left">Controles de selección<br/><br/>
    <img src="../controls-and-patterns/images/controls/radio-button.png" alt="radio button" /></td>
<td align="left"><a href="../controls-and-patterns/checkbox.md">casilla</a>, <a href="../controls-and-patterns/radio-button.md">botón de radio</a>, <a href="../controls-and-patterns/toggles.md">modificador para alternar</a></td>
<td align="left">Permite a los usuarios elegir entre varias opciones, como al completar una encuesta o al configurar los parámetros de una aplicación.</td>
</tr>
<tr class="even">
<td align="left">Selectores de fecha y hora<br/><br/>
    <img src="../controls-and-patterns/images/controls/calendar-date-picker-open.png" alt="date picker" /></td>
<td align="left"><a href="../controls-and-patterns/date-and-time.md">selector de fecha del calendario, vista de calendario, selector de fecha, selector de hora</a></td>
<td align="left">Permite a los usuarios ver y modificar la información de fecha y hora, como al crear un evento o al establecer una alarma.</td>
</tr>
<tr class="odd">
<td align="left">Entrada de texto predictivo<br/><br/>
    <img src="../controls-and-patterns/images/controls/auto-suggest-box.png" alt="autosuggest box" /></td>
<td align="left"><a href="../controls-and-patterns/auto-suggest-box.md">Cuadro de sugerencias automáticas</a></td>
<td align="left">Proporcionar a los usuarios sugerencias a medida que escriben, como al escribir datos o realizar consultas.</td>
</tr>
</tbody>
</table>
</div>

Para obtener una lista completa, consulta [Controls and UI elements](https://dev.windows.com/design/controls-patterns) (Controles y elementos de la interfaz de usuario).

##  <a name="place-commands-on-the-right-surface"></a>Colocar los comandos en la superficie correcta
Puedes colocar elementos de comandos en diversas superficies de tu aplicación, como el lienzo de la aplicación o contenedores de comandos especiales, como barras de comandos, menús, cuadros de diálogo y controles flotantes.

Ten en cuenta que, siempre que sea posible, deberías permitir que los usuarios manipulen directamente el contenido, en lugar de utilizar comandos para actuar sobre él. Por ejemplo, permite a los usuarios reorganicen listas arrastrando y colocando sus elementos, en lugar de con botones de comando para subir y bajar.
  
En caso de que los usuarios no puedan manipular directamente el contenido, coloca los elementos de comandos en una superficie de comando de la aplicación:

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">Superficie</th>
<th align="left">Descripción</th>
<th align="left">Ejemplo</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top">Lienzo de la aplicación (área de contenido)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>

<td align="left" style="vertical-align: top;">Si un comando se necesita constantemente para que los usuarios completen los escenarios principales, ponlo en el lienzo. Debido a que se pueden colocar comandos cerca (o encima) de los objetos sobre los que actúan, colocarlos en el lienzo hace que sean más evidentes y fáciles de usar.
<p>Sin embargo, elige los comandos que colocas en el lienzo con cuidado. Si se colocan demasiados comandos en el lienzo de la aplicación, estos ocupan un valioso espacio en pantalla y pueden saturar al usuario. Si el comando no se va a usar con frecuencia, considera la posibilidad de colocarlo en otra superficie de comandos.</p> 
</td><td>
Un cuadro de sugerencias automáticas en el lienzo de la aplicación Mapas.
<br></br>
  <img src="images/maps-canvas.png" alt="autosuggest box on Maps app canvas"/>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/app-bars.md">Barra de comandos</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p></td>
<td align="left" style="vertical-align: top;"> Las barras de comandos ayudan a organizar los comandos y a que sean de más fácil acceso. Las barras de comandos pueden colocarse en la parte superior de la pantalla, en la parte inferior o en ambas partes a la vez. 
</td>
<td>
Una barra de comandos en la parte superior de la aplicación Mapas.
<br></br>
<img src="images/maps-commandbar.png" alt="command bar in Maps app"/>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/menus.md">Menús y menús contextuales</a>
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left" style="vertical-align: top;">A veces resulta más eficaz agrupar varios comandos en un menú de comandos para ahorrar espacio. Los menús y los menús contextuales muestran una lista de opciones o comandos cuando el usuario los solicita.
<p>Los menús contextuales pueden proporcionar accesos directos a las acciones más comunes y ofrecer acceso a los comandos secundarios que solo son relevantes en contextos determinados, como el Portapapeles o los comandos personalizados. Normalmente los menús contextuales se solicitan cuando el usuario hace clic con el botón derecho.</p>
</td><td>
Aparece un menú contextual cuando los usuarios hacen clic con el botón derecho en la aplicación Mapas.
<br></br>
  <img src="images/maps-contextmenu.png" alt="context menu in Maps app"/>
</td>
</tr>
</table>
</div>

## <a name="provide-feedback-for-interactions"></a>Proporcionar comentarios sobre las interacciones

Los comentarios comunican los resultados de los comandos y permiten a los usuarios comprender qué han hecho y qué pueden hacer a continuación. Lo ideal es que los comentarios se integren de forma natural en la interfaz de usuario, para que los usuarios no sufran interrupciones ni tengan que realizar acciones adicionales a menos que sea absolutamente necesario. 

Estas son algunas maneras de proporcionar comentarios en tu aplicación.

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">Superficie</th>
<th align="left">Descripción</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"> <a href="../controls-and-patterns/app-bars.md">Barra de comandos</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p>
</td>
<td align="left" style="vertical-align: top;"> El área de contenido de la barra de comandos es un lugar intuitivo para comunicar el estado a los usuarios si desean ver comentarios.
<p>
  <img src="images/commandbar_anatomy.png" alt="Command bar content area for feedback"/>
  </p>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">Control flotante</a>
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left" style="vertical-align: top;">
Un elemento emergente contextual ligero que se puede cerrar tocando o haciendo clic en algún lugar fuera del control flotante.
<p>
  <img src="../controls-and-patterns/images/controls/flyout.png" alt="Flyout above button"/>
  </p>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">Controles de cuadro de diálogo</a>
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left" style="vertical-align: top;">Los cuadros de diálogo son superposiciones modales en la interfaz de usuario que proporcionan información contextual sobre la aplicación. En la mayoría de casos, los cuadros de diálogo bloquean las interacciones con la ventana de la aplicación hasta que se descartan de forma explícita y a menudo solicitan algún tipo de acción por parte del usuario.
<p>Los cuadros de diálogo pueden ser molestos y solo deben usarse en ciertas situaciones. Para obtener más información, consulta la sección [Cuándo se deben confirmar o deshacer acciones](#when-to-confirm-or-undo-actions).</p>
<p>
  <img src="../controls-and-patterns/images/dialogs/dialog_RS2_delete_file.png" alt="dialog delete file"/></p>
</td>
</tr>

</table>
</div>

> [!TIP]
> Ten cuidado con la cantidad de cuadros de diálogo de confirmación que usa la aplicación; pueden ser muy útiles cuando el usuario comete un error, pero son un estorbo cuando el usuario intenta realizar una acción de forma intencionada.

### <a name="when-to-confirm-or-undo-actions"></a>Cuándo se deben confirmar o deshacer acciones

Independientemente de lo bien diseñada que esté la interfaz de usuario y de lo cuidadoso que sea el usuario, en algún momento todos los usuarios llevarán a cabo una acción que desearían no haber realizado. La aplicación puede ayudar en este tipo de situaciones, solicitando al usuario que confirme una acción o proporcionando una manera de deshacer las acciones recientes.

-   Para las acciones que no se pueden deshacer y tienen consecuencias mayores, te recomendamos que uses un cuadro de diálogo de confirmación. Algunos ejemplos de estas acciones son:
    -   sobrescribir un archivo
    -   no guardar un archivo antes de cerrar
    -   confirmar la eliminación permanente de un archivo o datos
    -   realizar una compra (a menos que el usuario rechace exigir una confirmación)
    -   enviar un formulario, como cuando se iniciar sesión para algo.
-   Para las acciones que se pueden deshacer, ofrecer un comando Deshacer simple suele bastar. Algunos ejemplos de estas acciones son:
    -   eliminar un archivo
    -   eliminar un correo electrónico (no permanentemente)
    -   modificar contenido o editar texto
    -   cambiar el nombre de un archivo

##  <a name="optimize-for-specific-input-types"></a>optimización para determinados tipos de entradas

Consulta la [Información básica de interacción](../input/index.md) para obtener más información sobre la optimización de las experiencias de usuario en torno a un dispositivo o tipo de entrada.