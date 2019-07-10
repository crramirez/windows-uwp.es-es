---
Description: En una aplicación de la Plataforma universal de Windows (UWP), los elementos de comandos son los elementos interactivos de la interfaz de usuario que permiten al usuario realizar acciones como enviar un correo electrónico, eliminar un elemento o enviar un formulario.
title: Conceptos básicos sobre el diseño de comandos de aplicaciones de la Plataforma universal de Windows (UWP)
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ac2bd55d1cea25359c3c609148c7098532d76c46
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "63796390"
---
# <a name="command-design-basics-for-uwp-apps"></a>Conceptos básicos de diseño de los comandos para las aplicaciones para UWP

En una aplicación de la Plataforma universal de Windows (UWP), los *elementos de comando* son los elementos interactivos de la interfaz de usuario que permiten a los usuarios realizar acciones como enviar un correo electrónico, eliminar un elemento o enviar un formulario. Las *interfaces de comandos* están formadas por elementos de comando comunes, las superficies de comandos que las hospedan, las interacciones que admiten y las experiencias que proporcionan.

## <a name="provide-the-best-command-experience"></a>Proporcionar la mejor experiencia de comandos

El aspecto más importante de una interfaz de comandos es lo que quieres permitir hacer a los usuarios. A la hora de planear la funcionalidad de la aplicación, ten en cuenta los pasos necesarios para realizar esas tareas y las experiencias de usuario que quieres habilitar. Una vez completado el borrador inicial de estas experiencias, puedes tomar decisiones sobre las herramientas y las interacciones para implementarlas.

Estas son algunas experiencias de comando comunes:

- Enviar información
- Seleccionar la configuración y las opciones
- Buscar contenido y filtrarlo
- Abrir, guardar y eliminar archivos
- Editar y crear contenido

Se creativo a la hora de diseñar las experiencias de comandos. Elige qué dispositivos de entrada admite la aplicación y cómo responde la aplicación a cada dispositivo. Una aplicación que incorpora la gama más amplia de funcionalidades y preferencias hace que sea utilizable, portátil y accesible. Consulta más información en [Diseño de comandos para aplicaciones de la Plataforma universal de Windows (UWP)](../controls-and-patterns/commanding.md).



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>Elige los elementos de comando adecuados

Usar los elementos adecuados en una interfaz de usuario puede marcar la diferencia entre una aplicación intuitiva y fácil de usar y otra que parezca difícil o confusa. La Plataforma universal de Windows (UWP) incluye un completo conjunto de elementos de comando. Esta es una lista de algunos de los elementos de comandos para UWP más habituales.

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

Para obtener una lista completa, consulta [Controles y elementos de la interfaz de usuario](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>Colocar los comandos en la superficie correcta

Puedes colocar elementos de comandos en diversas superficies de tu aplicación, como el lienzo de la aplicación o contenedores de comandos especiales, como barras de comandos, elementos flotantes de barra de comandos, barra de menús o cuadros de diálogo.

Procura siempre que los usuarios puedan manipular el contenido directamente en lugar de usar comandos que actúan en el contenido, como arrastrar y colocar para reorganizar los elementos de una lista en lugar de botones de comando para subir y bajar. 

Sin embargo, tal vez esto no sea posible con determinados dispositivos de entrada o al acomodar preferencias y habilidades específicas del usuario. En estos casos, ofrece tantas prestaciones como sea posible y coloca estos elementos de comando en una superficie de comandos en la aplicación.

A continuación se incluye una lista de algunas de las superficies de comandos más habituales.

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Proporcionar comentarios sobre los comandos 

Los comentarios sobre los comandos comunican a los usuarios que se ha detectado un comando o interacción, cómo se interpretó y controló el comando, y si el comando se ejecutó correctamente o no. Esto ayuda a los usuarios a entender lo que han hecho y qué pueden hacer a continuación. Lo ideal es que los comentarios se integren de forma natural en la interfaz de usuario, para que los usuarios no sufran interrupciones ni tengan que realizar acciones adicionales a menos que sea absolutamente necesario.

> [!NOTE]
> Proporciona comentarios solo cuando sea necesario y solo la información no está disponible en otro lugar. Mantén la interfaz de usuario de la aplicación limpia y despejada, a menos que vayas a agregar un valor.

Estas son algunas maneras de proporcionar comentarios en tu aplicación.

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       Los <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">controles flotantes</a> son elementos emergentes contextuales ligeros que se puede cerrar pulsando o haciendo clic en algún lugar fuera del control flotante.
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> Ten cuidado con la cantidad de cuadros de diálogo de confirmación que usa la aplicación; pueden ser muy útiles cuando el usuario comete un error, pero son un estorbo cuando el usuario intenta realizar una acción de forma intencionada.

### <a name="when-to-confirm-or-undo-actions"></a>Cuándo se deben confirmar o deshacer acciones

Da igual lo bien diseñada que esté la interfaz de usuario de tu aplicación, todos los usuarios realizan una acción que desearían no haber hecho. La aplicación puede ayudar en este tipo de situaciones, solicitando al usuario que confirme una acción o proporcionando una manera de deshacer las acciones recientes.

:::row:::
    :::column:::
        ![do image](images/do.svg)

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

##  <a name="optimize-for-specific-input-types"></a>Optimización para determinados tipos de entradas

Consulta la [Información básica de interacción](../input/index.md) para obtener más información sobre la optimización de las experiencias de usuario en torno a un dispositivo o tipo de entrada.

