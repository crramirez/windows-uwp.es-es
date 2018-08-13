---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: Conceptos básicos sobre el diseño de comandos para las aplicaciones para la Plataforma universal de Windows (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 09f775ad0ba596379b6d3ddf158285849520111f
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842572"
---
#  <a name="command-design-basics-for-uwp-apps"></a>Conceptos básicos de diseño de los comandos para las aplicaciones para UWP

En una aplicación para la Plataforma universal de Windows (UWP), los *elementos de comandos* son los elementos interactivos de la interfaz de usuario que permiten a los usuarios realizar acciones como enviar un correo electrónico, eliminar un elemento o enviar un formulario. En este artículo se describen los elementos de comandos comunes, las interacciones que admiten y las superficies de comando para alojarlos.

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

:::row::: :::column::: ![imagen de botón](images/commanding/thumbnail-button.svg) :::column-end::: :::column span="2"::: <b>Botones</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row::: :::column::: ![imagen de lista](images/commanding/thumbnail-list.svg) :::column-end::: :::column span="2"::: <b>Listas</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row::: :::column::: ![imagen de control, de selección](images/commanding/thumbnail-selection.svg) :::column-end::: :::column span="2"::: <b>Controles de selección</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row::: :::column::: ![Imagen de calendario](images/commanding/thumbnail-calendar.svg) :::column-end::: :::column span="2"::: <b>Selectores de calendario, fecha y hora</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row::: :::column::: ![Imagen de entrada de texto predictivo](images/commanding/thumbnail-autosuggest.svg) :::column-end::: :::column span="2"::: <b>Entrada de texto predictivo</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

Para obtener una lista completa, consulta [Controles y elementos de la interfaz de usuario](../controls-and-patterns/index.md).

##  <a name="place-commands-on-the-right-surface"></a>Colocar los comandos en la superficie correcta
Puedes colocar elementos de comandos en diversas superficies de tu aplicación, como el lienzo de la aplicación o contenedores de comandos especiales, como barras de comandos, menús, cuadros de diálogo y controles flotantes.

Ten en cuenta que, siempre que sea posible, deberías permitir que los usuarios manipulen directamente el contenido, en lugar de utilizar comandos para actuar sobre él. Por ejemplo, permite a los usuarios reorganicen listas arrastrando y colocando sus elementos, en lugar de con botones de comando para subir y bajar.

En caso de que los usuarios no puedan manipular directamente el contenido, coloca los elementos de comandos en una superficie de comando de la aplicación. A continuación se incluye una lista de algunas de las superficies de comandos más habituales.

:::row::: :::column::: ![imagen de lienzo de aplicación](images/commanding/thumbnail-canvas.svg) :::column-end::: :::column span="2"::: <b>Lienzo de aplicación (área de contenido)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row::: :::column::: ![imagen de barra de comandos](images/commanding/thumbnail-commandbar.svg) :::column-end::: :::column span="2"::: <b>Barras de comandos</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen.
:::row-end:::

:::row::: :::column::: ![imagen de menú contextual](images/commanding/thumbnail-contextmenu.svg) :::column-end::: :::column span="2"::: <b>Menús y menús contextuales</b>

        Sometimes it is more efficient to group multiple commands into a command menu to save space. <a href="../controls-and-patterns/menus.md">Menus and context menus</a> display a list of commands or options when the user requests them. Context menus can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands. Context menus are usually prompted by a user right-clicking.
:::row-end:::

## <a name="provide-feedback-for-interactions"></a>Proporcionar comentarios sobre las interacciones

Los comentarios comunican los resultados de los comandos y permiten a los usuarios comprender qué han hecho y qué pueden hacer a continuación. Lo ideal es que los comentarios se integren de forma natural en la interfaz de usuario, para que los usuarios no sufran interrupciones ni tengan que realizar acciones adicionales a menos que sea absolutamente necesario. 

Estas son algunas maneras de proporcionar comentarios en tu aplicación.

:::row::: :::column::: ![imagen de área de contenido de barra de comandos](images/commanding/thumbnail-commandbar2.svg) :::column-end::: :::column span="2"::: <b>Barra de comandos</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row::: :::column::: ![Imagen de control flotante](images/commanding/thumbnail-flyout.svg) :::column-end::: :::column span="2"::: <b>Controles flotantes</b>

       <a href="../controls-and-patterns/dialogs.md">Flyouts</a> are lightweight contextual popups that can be dismissed by tapping or clicking somewhere outside the flyout.
:::row-end:::

:::row::: :::column::: ![Imagen de diálogo](images/commanding/thumbnail-dialog.svg) :::column-end::: :::column span="2"::: <b>Controles de diálogo</b>

        <a href="../controls-and-patterns/dialogs.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> Ten cuidado con la cantidad de cuadros de diálogo de confirmación que usa la aplicación; pueden ser muy útiles cuando el usuario comete un error, pero son un estorbo cuando el usuario intenta realizar una acción de forma intencionada.

### <a name="when-to-confirm-or-undo-actions"></a>Cuándo se deben confirmar o deshacer acciones

Independientemente de lo bien diseñada que esté la interfaz de usuario y de lo cuidadoso que sea el usuario, en algún momento todos los usuarios llevarán a cabo una acción que desearían no haber realizado. La aplicación puede ayudar en este tipo de situaciones, solicitando al usuario que confirme una acción o proporcionando una manera de deshacer las acciones recientes.

:::row::: :::column::: ![hacer imagen](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>optimizar determinados tipos de entradas

Consulta la [Información básica de interacción](../input/index.md) para obtener más información sobre la optimización de las experiencias de usuario en torno a un dispositivo o tipo de entrada.
