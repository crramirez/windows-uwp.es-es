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
ms.openlocfilehash: 8b33fe420e93c9ce78c625ad365ec8dc10e343ad
ms.sourcegitcommit: 789bfe3756c5c47f7324b96f482af636d12c0ed3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68867456"
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
![imagen de botón](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>Botones</b>

Los <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">botones</a> desencadenan una acción inmediata. Algunos ejemplos son enviar un correo electrónico, enviar datos de un formulario o confirmar una acción en un cuadro de diálogo.
:::row-end:::

:::row:::
    :::column:::
![imagen de lista](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>Listas</b>

Las <a href="../controls-and-patterns/lists.md" style="text-decoration:none">listas</a> presentan los elementos en una cuadrícula o lista interactiva. Por lo general, se usa para muchas opciones o para mostrar elementos. Algunos ejemplos son: lista desplegable, cuadro de lista, vista de lista y vista de cuadrícula.
:::row-end:::

:::row:::
    :::column:::
![imagen de control de selección](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>Controles de selección</b>

Permite a los usuarios elegir entre varias opciones, como al completar una encuesta o al configurar los parámetros de una aplicación. Entre los ejemplos se incluyen la <a href="../controls-and-patterns/checkbox.md">casilla</a>, el <a href="../controls-and-patterns/radio-button.md">botón de radio</a> y el <a href="../controls-and-patterns/toggles.md">conmutador de alternancia</a>.
:::row-end:::

:::row:::
    :::column:::
![imagen de calendario](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>Calendario, selectores de fecha y hora</b>

El <a href="../controls-and-patterns/date-and-time.md">calendario y los selectores de fecha y hora</a> permiten a los usuarios ver y modificar la información de fecha y hora, por ejemplo, al crear un evento o al establecer una alarma. Algunos ejemplos son: selector de fecha del calendario, vista de calendario, selector de fecha, selector de hora.
:::row-end:::

:::row:::
    :::column:::
![imagen de entrada de texto predictivo](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>Entrada de texto predictivo</b>

Proporciona a los usuarios sugerencias a medida que escriben, como al escribir datos o realizar consultas. Un ejemplo es el <a href="../controls-and-patterns/auto-suggest-box.md">cuadro sugerencia automática</a>.<br>
:::row-end:::

Para obtener una lista completa, consulta [Controles y elementos de la interfaz de usuario](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>Colocar los comandos en la superficie correcta

Puedes colocar elementos de comandos en diversas superficies de tu aplicación, como el lienzo de la aplicación o contenedores de comandos especiales, como barras de comandos, elementos flotantes de barra de comandos, barra de menús o cuadros de diálogo.

Procura siempre que los usuarios puedan manipular el contenido directamente en lugar de usar comandos que actúan en el contenido, como arrastrar y colocar para reorganizar los elementos de una lista en lugar de botones de comando para subir y bajar. 

Sin embargo, tal vez esto no sea posible con determinados dispositivos de entrada o al acomodar preferencias y habilidades específicas del usuario. En estos casos, ofrece tantas prestaciones como sea posible y coloca estos elementos de comando en una superficie de comandos en la aplicación.

A continuación se incluye una lista de algunas de las superficies de comandos más habituales.

:::row:::
    :::column:::
![imagen de lienzo de la aplicación](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>Lienzo de la aplicación (área de contenido)</b>

Si un comando se necesita constantemente para que los usuarios realicen acciones en los escenarios principales, ponlo en el lienzo. Debido a que se pueden colocar comandos cerca (o encima) de los objetos sobre los que actúan, colocarlos en el lienzo hace que sean más evidentes y fáciles de usar. Sin embargo, elige los comandos que colocas en el lienzo con cuidado. Si se colocan demasiados comandos en el lienzo de la aplicación, estos ocupan un valioso espacio en pantalla y pueden saturar al usuario. Si el comando no se va a usar con frecuencia, considera la posibilidad de colocarlo en otra superficie de comandos.
:::row-end:::

:::row:::
    :::column:::
![imagen de barra de comandos](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>Barras de comandos y barras de menús</b>

Las <a href="../controls-and-patterns/app-bars.md">barras de comandos</a> ayudan a organizar los comandos para que sea más fácil acceder a ellos. Las barras de comandos se pueden colocar en la parte superior de la pantalla, en la parte inferior de la pantalla o en la parte superior e inferior de la pantalla (también se puede usar una <a href="../controls-and-patterns/menus.md#create-a-menu-bar">barra de menús</a> cuando la funcionalidad de la aplicación es demasiado compleja para una barra de comandos).
:::row-end:::

:::row:::
    :::column:::
![imagen de menú contextual](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>Menús y menús contextuales</b>

<p>Los menús y los menús contextuales ahorran espacio al organizar los comandos y ocultarlos hasta que el usuario los necesite. Para acceder a un menú o menú contextual, los usuarios suelen hacer clic en un botón o clic con el botón derecho en un control.</p> 

<p>El <a href="../controls-and-patterns/command-bar-flyout.md">control flotante de barra de comandos</a> es un tipo de menú contextual que combina las ventajas de una barra de comandos y un menú contextual en un único control. Pueden proporcionar accesos directos a las acciones más comunes y ofrecer acceso a los comandos secundarios que solo son relevantes en contextos determinados, como el Portapapeles o los comandos personalizados.</p>

<p>UWP también proporciona un conjunto de menús tradicionales y menús contextuales. Para más información, consulta <a href="../controls-and-patterns/menus.md">Introducción a los menús y menús contextuales</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>Proporcionar comentarios sobre los comandos 

Los comentarios sobre los comandos comunican a los usuarios que se ha detectado un comando o interacción, cómo se interpretó y controló el comando, y si el comando se ejecutó correctamente o no. Esto ayuda a los usuarios a entender lo que han hecho y qué pueden hacer a continuación. Lo ideal es que los comentarios se integren de forma natural en la interfaz de usuario, para que los usuarios no sufran interrupciones ni tengan que realizar acciones adicionales a menos que sea absolutamente necesario.

> [!NOTE]
> Proporciona comentarios solo cuando sea necesario y solo la información no está disponible en otro lugar. Mantén la interfaz de usuario de la aplicación limpia y despejada, a menos que vayas a agregar un valor.

Estas son algunas maneras de proporcionar comentarios en tu aplicación.

:::row:::
    :::column:::
![imagen del área de contenido de la barra de comandos](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>Barra de comandos</b>

El área de contenido de la <a href="../controls-and-patterns/app-bars.md">barra de comandos</a> es un lugar intuitivo para comunicar el estado a los usuarios si desean ver comentarios.
:::row-end:::

:::row:::
    :::column:::
![Imagen de control flotante](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>controles flotantes</b>

       Los <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">controles flotantes</a> son elementos emergentes contextuales ligeros que se puede cerrar pulsando o haciendo clic en algún lugar fuera del control flotante.
:::row-end:::

:::row:::
    :::column:::
![Imagen de cuadro de diálogo](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>Controles de cuadro de diálogo</b>

Los <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">controles de cuadro de diálogo</a> son superposiciones modales en la interfaz de usuario que proporcionan información contextual sobre la aplicación. En la mayoría de casos, los cuadros de diálogo bloquean las interacciones con la ventana de la aplicación hasta que se descartan de forma explícita y a menudo solicitan algún tipo de acción por parte del usuario. Los cuadros de diálogo pueden ser molestos y solo deben usarse en ciertas situaciones. Para obtener más información, consulta la sección [Cuándo se deben confirmar o deshacer acciones](#when-to-confirm-or-undo-actions).
    :::column-end:::
:::row-end:::

> [!TIP]
> Ten cuidado con la cantidad de cuadros de diálogo de confirmación que usa la aplicación; pueden ser muy útiles cuando el usuario comete un error, pero son un estorbo cuando el usuario intenta realizar una acción de forma intencionada.

### <a name="when-to-confirm-or-undo-actions"></a>Cuándo se deben confirmar o deshacer acciones

Da igual lo bien diseñada que esté la interfaz de usuario de tu aplicación, todos los usuarios realizan una acción que desearían no haber hecho. La aplicación puede ayudar en este tipo de situaciones, solicitando al usuario que confirme una acción o proporcionando una manera de deshacer las acciones recientes.

:::row:::
    :::column:::
![imagen correcto](images/do.svg)

Para las acciones que no se pueden deshacer y tienen consecuencias mayores, te recomendamos que uses un cuadro de diálogo de confirmación. Algunos ejemplos de estas acciones son:
-   sobrescribir un archivo
-   no guardar un archivo antes de cerrar
-   confirmar la eliminación permanente de un archivo o datos
-   realizar una compra (a menos que el usuario rechace exigir una confirmación)
-   enviar un formulario, como cuando se iniciar sesión para algo.
    :::column-end:::
    :::column:::
![imagen correcto](images/do.svg)

Para las acciones que se pueden deshacer, ofrecer un comando Deshacer simple suele bastar. Algunos ejemplos de estas acciones son:
-   eliminar un archivo
-   eliminar un correo electrónico (no permanentemente)
-   modificar contenido o editar texto
-   cambiar el nombre de un archivo
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>Optimización para determinados tipos de entradas

Consulta la [Información básica de interacción](../input/index.md) para obtener más información sobre la optimización de las experiencias de usuario en torno a un dispositivo o tipo de entrada.

