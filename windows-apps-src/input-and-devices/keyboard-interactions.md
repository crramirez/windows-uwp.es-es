---
author: Karl-Bridge-Microsoft
Description: "Aprende a diseñar y optimizar tus aplicaciones para UWP para que proporcionen la mejor experiencia posible para los usuarios avanzados de teclado y para aquellos que tienen discapacidades y otros requisitos de accesibilidad."
title: Interacciones de teclado
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: "teclado, accesibilidad, navegación, foco, texto, entrada, interacciones del usuario, controlador para juegos, remoto"
ms.author: kbridge
ms.date: 03/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 22a95cfb77740d7521c62f0b7153130ccdc1b552
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2017
---
# <a name="keyboard-interactions"></a>Interacciones de teclado
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

![Imagen principal de teclado](images/keyboard/keyboard-hero.jpg)

Aprende a diseñar y optimizar tus aplicaciones para UWP para que proporcionen la mejor experiencia posible para los usuarios avanzados de teclado y para aquellos que tienen discapacidades y otros requisitos de accesibilidad.

En todos los dispositivos, la entrada de teclado es una parte importante de la experiencia de interacción global de la Plataforma universal de Windows(UWP). Una experiencia de teclado bien diseñada permite a los usuarios navegar por la interfaz de usuario de la aplicación de manera eficaz y acceder a su funcionalidad total sin tener que levantar las manos del teclado.

![Imagen del teclado y el controlador para juegos](images/keyboard/keyboard-gamepad.jpg)

***El teclado y el controlador para juegos comparten patrones comunes de interacción***

En este tema, nos centramos específicamente en el diseño de aplicaciones para UWP para la entrada de teclado en equipos. No obstante, es importante una experiencia de teclado bien diseñada para admitir las herramientas de accesibilidad, como el Narrador de Windows, mediante los teclados de software, como el [teclado táctil](#touch-keyboard) y el [teclado en pantalla (OSK)](#osk), y para controlar otros tipos de dispositivos, como el controlador para juegos y el control remoto de Xbox.

Muchas de las directrices y recomendaciones que se describe aquí, incluidos los [elementos visuales de foco](#focus-visual), las [teclas de acceso](#access-keys) y la [navegación de la interfaz de usuario](#navigation), también se aplican a estos otros escenarios.

**NOTA**  Si bien se usan teclados hardware y software para la entrada de texto, este tema se centra en la navegación y la interacción.

## <a name="built-in-support"></a>Compatibilidad integrada

Junto con el mouse, el teclado es el periférico más ampliamente usado en los equipos y, como tal, es una parte fundamental de la experiencia del equipo. Los usuarios de equipos esperan una experiencia integral y coherente tanto del sistema como de las aplicaciones individuales en la respuesta a la entrada de teclado.

Todos los controles de UWP incluyen compatibilidad integrada para experiencias de teclado e interacciones de usuario enriquecidas, mientras que la plataforma misma proporciona una amplia base para crear experiencias de teclado que creas que se adecuan mejor a tus aplicaciones y controles personalizados.

![Imagen de teclado con teléfono](images/keyboard/keyboard-phone.jpg)

***UWP admite teclados con cualquier dispositivo***

## <a name="basic-experiences"></a>Experiencias básicas
![Dispositivos basados en el foco](images/keyboard/focus-based-devices.jpg)

Como se mencionó anteriormente, los dispositivos de entrada, como el controlador para juegos y el control remoto de Xbox, y las herramientas de accesibilidad, como el Narrador, comparten gran parte de la experiencia de entrada de teclado en cuanto a la navegación y los comandos. Esta experiencia en común entre los distintos tipos y herramientas de entrada minimiza el trabajo adicional para ti y contribuye al objetivo de "compilar una vez, ejecutar en cualquier parte" de la Plataforma universal de Windows.

En caso necesario, se identificarán las diferencias principales que debes tener en cuenta y describirán las mitigaciones que debes tener en cuenta.

Estos son los dispositivos y las herramientas que se describen en este tema:

| Dispositivo/herramienta                       | Descripción     |
|-----------------------------------|-----------------|
|Teclado (hardware y software)   |Además del teclado de hardware estándar, las aplicaciones para UWP admiten dos teclados de software: el [táctil (o teclado de software)](#touch-keyboard) y el [teclado en pantalla](#osk).|
|Controlador para juegos y control remoto         |El controlador para juegos y el control remoto de Xbox son dispositivos de entrada fundamentales en la [experiencia de 10 pies](designing-for-tv.md).
Para obtener información específica sobre la compatibilidad de UWP para el controlador para juegos y el control remoto, consulta [Interacciones con el controlador para juegos y el control remoto](gamepad-and-remote-interactions.md).|
|Lectores de pantalla (Narrador)          |El Narrador es un lector de pantalla integrado para Windows, que proporciona una funcionalidad y unas experiencias de interacción únicas, pero que sigue basándose en la navegación y entrada básicas por teclado.
Para más información sobre el Narrador, consulta [Introducción al Narrador](https://support.microsoft.com/help/22798/windows-10-narrator-get-started).|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Experiencias personalizadas y uso eficiente del teclado
Como se mencionó, la compatibilidad del teclado es integral, para garantizar que las aplicaciones funcionen bien para usuarios con distintas habilidades, capacidades y expectativas. Te recomendamos que des prioridad a lo siguiente.
- Admitir la navegación e interacción por teclado
    - Asegurar que los elementos accionables estén identificados como paradas de tabulación (y no lo estén los no accionables) y que el orden de la navegación sea lógico y predecible (consulta [Paradas de tabulación](#tab-stops))
    - Establece el foco inicial en el elemento más lógico (véase [Foco inicial](#initial-focus))
    - Proporcionar navegación con las teclas de flecha para las "navegaciones internas" (véase [Navegación](#navigation))
- Admitir métodos abreviados de teclado
    - Proporcionar las teclas de aceleración para acciones rápidas (véase [Aceleradores](#accelerators))
    - Proporcionar teclas de acceso para navegar por la interfaz de usuario de la aplicación (véase [Teclas de acceso](access-keys.md))

### <a name="focus-visuals-a-namefocus-visual"></a>Elementos visuales de foco <a name="focus-visual">

La UWP admite un único diseño visual de foco que funciona bien para todos los tipos y experiencias de entrada.
![Elemento visual de foco](images/keyboard/focus-visual.png)

Un elemento visual de foco:
-   Aparece cuando un elemento de la interfaz de usuario recibe el foco desde un teclado, controlador para juegos o control remoto.
-   Se representa como un borde resaltado alrededor del elemento de la interfaz de usuario para indicar que se puede realizar una acción.
-   Ayuda al usuario a navegar por la interfaz de usuario de una aplicación sin perderse.
-   Puede personalizarse para tu aplicación (consulta [Elementos visuales de foco de alta visibilidad](guidelines-for-visualfeedback.md#high-visibility-focus-visuals)).

**NOTA** El elemento visual de foco de la UWP no es el mismo que el rectángulo de foco del Narrador.

### <a name="tab-stops-a-nametab-stops"></a>Puntos de tabulación <a name="tab-stops">

Para usar un control (incluidos los elementos de navegación) con el teclado, el control debe tener el foco. Una forma de que un control reciba el foco del teclado es que sea accesible a través de la navegación mediante tabulación al identificarlo como un punto de tabulación en el orden de tabulación de la aplicación.

Para que un control se incluya en el orden de tabulación, la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) debe establecerse en **true** y la propiedad [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) debe establecerse en **true.**

Para excluir específicamente un control del orden de tabulación, establece la propiedad [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) en **false.**

De manera predeterminada, el orden de tabulación refleja el orden en que se crean los elementos de la interfaz de usuario. Por ejemplo, si `StackPanel`contiene `Button`, `Checkbox`y `TextBox`, el orden de tabulación es `Button`, `Checkbox` y `TextBox`.

Para invalidar el orden de tabulación predeterminada, puedes configurar la propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461).

#### <a name="tab-order-should-be-logical-and-predictable"></a>El orden de tabulación debe ser predecible y lógico

Un modelo bien diseñada de navegación con el teclado, con un orden de tabulación predecible y lógico, hace que la aplicación sea más intuitiva y ayuda a los usuarios a explorar, descubrir y acceder a la funcionalidad de manera más eficaz y eficiente.

Todos los controles interactivos deberían tener puntos de tabulación (a menos que estén en un [grupo](#control-group)), todo lo contrario de los controles no interactivos, como `labels`.

Evita un orden de tabulación que haga que el foco salte sin sentido en la aplicación. Por ejemplo, una lista de controles en una interfaz de usuario de formulario debe tener un orden de tabulación que fluya de arriba abajo y de izquierda a derecha.

Consulta la página [Accesibilidad de teclado](../accessibility/keyboard-accessibility.md) para obtener más información sobre cómo personalizar los puntos de tabulación.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Intenta coordinar el orden de tabulación y el orden visual

Coordinar el orden de tabulación y el orden visual (también llamado el orden de lectura u orden de visualización) ayuda a reducir la confusión a los usuarios mientras navegan a través de la interfaz de usuario de la aplicación.

Prueba a clasificar y presentar los comandos, controles y contenido más importantes en primer lugar, tanto en el orden de tabulación como en el orden visual. Sin embargo, la posición de visualización puede depender del contenedor de diseño primario y de ciertas propiedades de los elementos secundarios que influyen en el diseño. En concreto, los diseños que usan una metáfora de cuadrícula o una tabla pueden tener un orden visual muy diferente del orden de tabulación.

**NOTA** El orden visual depende también de la configuración regional y el idioma.

### <a name="initial-focus-a-nameinitial-focus"></a>Foco inicial <a name="initial-focus">

El foco inicial especifica el elemento de la interfaz de usuario que recibe el foco cuando se inicia o activa una aplicación o una página por primera vez. Al usar un teclado, es a partir de este elemento que un usuario comienza a interactuar con la interfaz de usuario de la aplicación.

Para las aplicaciones para UWP, el foco inicial se establece en el elemento con el nivel más alto de [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) que puede recibir el foco. Se omiten los elementos secundarios de los controles de contenedor. En caso de empate, el primer elemento en el árbol visual recibe el foco.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Establece el foco inicial en el elemento más lógico

Establece el foco inicial en el elemento de la interfaz de usuario para la acción primera, o principal, que los usuarios más probablemente realicen al iniciar la aplicación o navegar a una página. Entre algunos ejemplos se incluyen:
-   Una aplicación de fotos, donde el foco se establece en el primer elemento de una galería
-   Una aplicación de música, donde el foco se establece en el botón de reproducción

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>No establezcas el foco inicial en un elemento que presente un resultado potencialmente negativo o, incluso, desastroso.

Este nivel de funcionalidad debe quedar a elección del usuario. Establecer el foco inicial en un elemento con un resultado significativo podría provocar la pérdida accidental de datos o acceso al sistema. Por ejemplo, no establezcas el foco en el botón de eliminar al navegar a un correo electrónico.

Consulta la página [Accesibilidad de teclado](../accessibility/keyboard-accessibility.md) para obtener más información sobre cómo invalidar el orden de tabulación.

### <a name="navigation-a-namenavigation"></a>Navegación <a name="navigation">

Por lo general, se admite la navegación por teclado a través de la tecla TAB y las teclas de dirección.

![Teclas TAB y de dirección](images/keyboard/tab-and-arrow.png)

De manera predeterminada, los controles de UWP siguen estos comportamientos de teclado básicos:
-   La **tecla TAB** navega entre los controles que se pueda accionar/activar en el orden de tabulación.
-   **Mayús+TAB** navega por los controles en el orden de tabulación inverso. Si el usuario ha navegado dentro del control con las teclas de dirección, el foco se establece en el último valor conocido dentro del control.
-   Las **teclas de dirección** exponen la "navegación interna" específica del control. Cuando el usuario entra en la "navegación interna", las teclas de dirección no navegan fuera de un control. Entre algunos ejemplos se incluyen:
    -   Las teclas de dirección arriba/abajo mueven el foco dentro de `ListView` y `MenuFlyout`
    -   Modificar los valores seleccionados actualmente para `Slider` y `RatingsControl`
    -   Mover el cursor de inserción dentro de `TextBox`
    -   Expandir o contraer elementos dentro de `TreeView`

Usa estos comportamientos predeterminados para optimizar la navegación por teclado de la aplicación.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Usa la "navegación interna" con conjuntos de controles relacionados

Proporcionar navegación mediante teclas de dirección en un conjunto de controles relacionados refuerza su relación dentro de la organización general de la interfaz de usuario de la aplicación.

Por ejemplo, el control `ContentDialog` que se muestra aquí proporciona navegación interna de manera predeterminada para una fila horizontal de botones (para controles personalizados, consulta la sección [Grupo de controles](#control-group)).

![ejemplo de diálogo](images/keyboard/dialog.png)

***La interacción con una colección de botones relacionados es más fácil con la navegación mediante teclas de dirección***

Si se muestran los elementos en una sola columna, las teclas de dirección arriba o abajo navegan por los elementos. Si se muestran los elementos en una sola fila, las teclas de dirección derecha o izquierda navegan por los elementos. Si los elementos se encuentran en varias columnas, se usan las cuatro teclas de dirección para navegar.

#### <a name="make-a-set-of-related-controls-a-single-tab-stop"></a>Convertir un conjunto de controles relacionados en un solo punto de tabulación

Al convertir un conjunto de controles relacionados o complementarios en un solo punto de tabulación, puedes minimizar el número total de puntos de tabulación en la aplicación.

Por ejemplo, las imágenes siguientes muestran dos controles `ListView` apilados. La imagen de la izquierda muestra la navegación mediante teclas de dirección usada con un punto de tabulación para navegar entre controles `ListView`; mientras que la imagen de la derecha muestra de qué manera la navegación entre elementos secundarios puede hacerse más fácil y eficaz al eliminar la necesidad de atravesar los controles principales con una tecla TAB.


<table>
  <td>![flecha y tabulación](images/keyboard/arrow-and-tab.png)</td>
  <td>![solo flecha](images/keyboard/arrow-only.png)</td>
</table>

***La interacción con dos controles ListView apilados puede hacerse de manera más fácil y eficaz al eliminar el punto de tabulación y navegar solo con las teclas de dirección.***

Visita la sección [Grupo de controles](#control-group) para aprender cómo aplicar los ejemplos de optimización en la interfaz de usuario de tu aplicación.

### <a name="interaction-and-commanding"></a>Interacción y comandos

Una vez que un control tiene el foco, un usuario puede interactuar con él e invocar cualquier funcionalidad asociada con la entrada de teclado específica.

#### <a name="text-entry"></a>Entrada de texto

Para los controles diseñados específicamente para la entrada de texto, como `TextBox` y `RichEditBox`, toda la entrada de teclado se usa para escribir texto o navegar por él, lo que tiene prioridad sobre los otros comandos de teclado. Por ejemplo, el menú desplegable para un control `AutoSuggestBox` no reconoce la **barra espaciadora** como comando de selección.

![entrada de texto](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Barra espaciadora

Cuando la **barra espaciadora** no está en modo de entrada de texto, invoca la acción o el comando asociados con el control del foco (igual que una pulsación con la entrada táctil o un clic con el mouse).

![barra espaciadora](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Tecla ENTRAR

La tecla **ENTRAR** puede realizar una variedad de interacciones comunes del usuario, según el control que tenga el foco:
-   Activa los controles de comando, como `Button` o `Hyperlink`. Para evitar la confusión del usuario final, la tecla **ENTRAR** también activa los controles que parecen controle de comando, como `ToggleButton` o `AppBarToggleButton`.
-   Muestra la interfaz de usuario del selector para los controles, como `ComboBox` y `DatePicker`. La tecla **ENTRAR** también confirma y cierra la interfaz de usuario del selector.
-   Activa los controles de lista, como `ListView`, `GridView` y `ComboBox`.
    -   La tecla **ENTRAR** realiza la acción de selección al igual que la **barra espaciadora** para los elementos de listas y cuadrículas, a menos que haya una acción adicional asociada a estos elementos (abrir una nueva ventana).
    -   Si hay una acción adicional asociada con el control, la tecla **ENTRAR** realiza la acción adicional y la **barra espaciadora** realiza la acción de selección.

**NOTA** La tecla **ENTRAR** y la **barra espaciadora** no siempre realizan la misma acción, pero a menudo sí.

![tecla ENTRAR](images/keyboard/enter-key.png)

La tecla Esc permite al usuario cancelar la interfaz de usuario transitoria (junto con las acciones en curso en esa interfaz de usuario).

Entre los ejemplos de esta experiencia se incluyen:
-   El usuario abre un `ComboBox` con un valor seleccionado y usa las teclas de dirección para mover la selección del foco a un nuevo valor. Al presionar la tecla Esc se cierra el `ComboBox`, y el valor seleccionado se restablece al valor original.
-   El usuario invoca una acción de eliminación permanente de un correo electrónico y aparece un `ContentDialog` para confirmar la acción. El usuario decide que esta no es la acción deseada y presiona la tecla **Esc** para cerrar el cuadro de diálogo. Como la tecla **Esc** está asociada al botón **Cancelar**, se cierra el cuadro de diálogo y se cancela la acción. La tecla **Esc** solo afecta a la interfaz de usuario transitoria, no cierra la interfaz de usuario de la aplicación ni vuelve en la navegación.

![tecla ESC](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Teclas Inicio y Fin

Las teclas **Inicio** y **Fin** permiten al usuario desplazarse al principio o al final de una región de la interfaz de usuario.

Entre los ejemplos de esta experiencia se incluyen:
-   Para los controles `ListView` y `GridView`, la tecla **Inicio** mueve el foco al primer elemento y desplaza la vista hasta él, mientras que la tecla **Fin** mueve el foco al último elemento y desplaza la vista hasta él.
-   Para un control `ScrollView`, la tecla **Inicio** permite desplazarse a la parte superior de la región, mientras que la tecla **Fin** permite desplazarse a la parte inferior de la región (no se cambia el foco).

![teclas Inicio y Fin](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Teclas Retroceder Página (RePág) y Avanzar Página (AvPág)

Las teclas **Página** permiten al usuario desplazarse a una región de la interfaz de usuario en incrementos discretos.

Por ejemplo, para controles `ListView` y `GridView`, la tecla **AvPág** desplaza la región hacia arriba una "página" (normalmente la altura de la ventanilla) y mueve el foco a la parte superior de la región. Por el contrario, la tecla **AvPág** desplaza la región hacia abajo una página y mueve el foco a la parte inferior de la región.

![teclas RePág y AvPág](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Métodos abreviados de teclado

Los métodos abreviados de teclado hacen que tu aplicación sea más fácil y eficaz de usar.

Además de implementar la navegación por teclado y la activación para tu aplicación, una práctica recomendada es implementar métodos abreviados para la funcionalidad de la aplicación. La navegación mediante tabulación proporciona un buen nivel básico de compatibilidad con teclado, pero con formas complejas, probablemente también quieras agregar compatibilidad para teclas de método abreviado. Esto puede hacer que el uso de tu aplicación sea más eficiente, incluso para aquellos que usan tanto dispositivos de puntero como un teclado.

Un método abreviado es una combinación de teclas que mejora la productividad al proporcionar al usuario una forma eficaz de acceder a las funciones de la aplicación. Existen dos tipos de métodos abreviados:
-   Una [tecla de aceleración](#accelerators) es un método abreviado a un comando de aplicación. Tu aplicación podría tener una interfaz de usuario que corresponda exactamente al comando. Las teclas de aceleración consisten en la tecla Ctrl y una tecla de letra.
-   Una [tecla de acceso](#access-keys) es un método abreviado a un elemento de la interfaz de usuario de la aplicación. Las teclas de acceso consisten en la tecla Alt y una tecla de letra.

Visita esta página para ver una lista exhaustiva de [métodos abreviados de teclado para Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts), así como [métodos abreviados de teclado específicos de aplicación](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps) usados por aplicaciones desarrolladas por Microsoft.

#### <a name="accelerators-a-nameaccelerators"></a>Aceleradores <a name="accelerators">

Los aceleradores ayudan a los usuarios a completar acciones comunes que existen en la aplicación rápidamente. Proporcionar teclas de aceleración coherentes que los usuarios puedan recordar y usar fácilmente en las aplicaciones que ofrecen tareas similares es muy importante para que el acelerador resulte útil, además de eficaz.

Ejemplos de aceleradores:
-   Al presionar Ctrl+N en cualquier lugar de la aplicación Correo inicia un nuevo elemento de correo.
-   Al presionar Ctrl+E en cualquier lugar de Edge y aplicación de la Tienda, el usuario puede escribir texto en el cuadro de búsqueda rápidamente.

Los aceleradores tienen las siguientes características:
-   Usan principalmente secuencias de Ctrl y teclas de función (las teclas de método abreviado del sistema de Windows usan también Alt+teclas no alfanuméricas y la tecla del logotipo de Windows).
-   Su fin principal es aumentar la eficiencia de los usuarios avanzados.
-   Se asignan únicamente a los comandos que más se usan.
-   Se espera que se memoricen y solo se documentan en los menús, en la información sobre herramientas y en la Ayuda.
-   Tienen efecto en todo el programa, pero no tienen efecto si no son de aplicación.
-   Se deben asignar de forma coherente porque se memorizan y no se documentan de forma directa.

#### <a name="access-keys-a-nameaccess-keys"></a>Teclas de acceso <a name="access-keys">

Las teclas de acceso brindan a los usuarios que tienen requisitos de accesibilidad y a los usuarios avanzados de teclado una manera eficiente y eficaz de navegar por la interfaz de usuario de la aplicación.

Para obtener información más detallada para admitir las teclas de acceso con UWP, consulta la página [Teclas de acceso](access-keys.md).

Las teclas de acceso ayudan a los usuarios que tienen discapacidades motrices la posibilidad de presionar una tecla a la vez para activar un elemento específico de la interfaz de usuario. Además, las teclas de acceso pueden usarse para comunicar teclas de método abreviado adicionales para ayudar a los usuarios avanzados a realizar acciones rápidamente.

Las teclas de acceso tienen las siguientes características:
-   Usan la tecla Alt y una tecla alfanumérica.
-   Son principalmente para la accesibilidad.
-   Se documentan directamente en la interfaz de usuario adyacente al control mediante el uso de [sugerencias de teclas](access-keys.md).
-   Tienen efecto únicamente en la ventana actual y desplazan al control o elemento de menú correspondiente.
-   No se asignan de forma coherente porque no siempre es posible hacerlo. Sin embargo, las teclas de acceso deben asignarse de forma coherente para los comandos usados con frecuencia, especialmente los botones de confirmación.
-   Están traducidas.

#### <a name="common-keyboard-shortcuts"></a>Métodos abreviados de teclado habituales

La siguiente tabla es una pequeña muestra de comandos de teclado usados con frecuencia. Para obtener una lista completa de comandos de teclado, consulta el tema sobre las [teclas de métodos abreviados de teclado de Windows](https://support.microsoft.com/kb/126449).

| Acción                               | Comando de tecla                                      |
|--------------------------------------|--------------------------------------------------|
| Seleccionar todo                           | Ctrl+E                                           |
| Seleccionar continuamente                  | Mayús+tecla de dirección                                  |
| Guardar                                 | Ctrl+S                                           |
| Buscar                                 | Ctrl+F                                           |
| Imprimir                                | Ctrl+P                                           |
| Copiar                                 | Ctrl+C                                           |
| Cortar                                  | Ctrl+X                                           |
| Pegar                                | Ctrl+V                                           |
| Deshacer                                 | Ctrl+Z                                           |
| Pestaña siguiente                             | Ctrl+TAB                                         |
| Cerrar pestaña                            | Ctrl+F4 o Ctrl+W                                |
| Zoom semántico                        | Ctrl++ o Ctrl+-                                 |

## <a name="advanced-experiences"></a>Experiencias avanzadas

En esta sección, veremos algunas de las experiencias de interacción de teclado más complejas compatibles con las aplicaciones para UWP, junto con algunos de los comportamientos que debes tener en cuenta cuando tu aplicación se use en diferentes dispositivos y con diferentes herramientas.

### <a name="control-group-a-namecontrol-group"></a>Grupo de controles <a name="control-group">

Puedes agrupar un conjunto de controles relacionados o complementarios, en un "grupo de controles" (o área direccional), lo que permite la "navegación interna" con las teclas de dirección. El grupo de controles puede ser un único punto de tabulación, o puedes especificar varios puntos de tabulación dentro del grupo de controles.

#### <a name="arrow-key-navigation"></a>Navegación mediante teclas de dirección

Los usuarios esperan que se admita la navegación mediante teclas de dirección cuando hay un grupo de controles similares y relacionados en una región de la interfaz de usuario:
-   `AppBarButtons` en `CommandBar`
-   `ListItems` o `GridItems` dentro de `ListView` o `GridView`
-   `Buttons` dentro de `ContentDialog`

Los controles para UWP admiten la navegación mediante teclas de dirección de manera predeterminada. Para los diseños personalizados y grupos de controles, usa `XYFocusKeyboardNavigation="Enabled"` para proporcionar un comportamiento similar.

Considera la posibilidad de agregar compatibilidad para la navegación mediante teclas de dirección cuando tengas los controles siguientes:

<table>
  <tr>
    <td>
      <p>![dialog](images/keyboard/dialog.png)</p>
      <p>**Buttons**</p>
      <p>![radiobutton](images/keyboard/radiobutton.png)</p>
      <p>**RadioButtons**</p>     
    </td>
    <td>
      <p>![appbar](images/keyboard/appbar.png)</p>
      <p>**AppBarButtons**</p>
      <p>![elementos de lista y cuadrícula](images/keyboard/list-and-grid-items.png)</p>
      <p>**ListItems y GridItems**</p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Puntos de tabulación

Según la funcionalidad y el diseño de tu aplicación, la mejor opción de navegación para un grupo de controles podría ser un único punto de tabulación con la navegación mediante teclas de dirección a los elementos secundarios, varios puntos de tabulación o alguna combinación.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Usa varios puntos de tabulación y las teclas de dirección para los botones

Los usuarios de accesibilidad dependen de reglas de navegación mediante teclado bien definidas, que normalmente no usan las teclas de dirección para desplazarse por una colección de botones. Sin embargo, los usuarios sin dificultades visuales podrían considerar que el comportamiento es natural.

En este caso, un ejemplo de comportamiento predeterminado de UWP es `ContentDialog`. Si bien las teclas de dirección pueden usarse para desplazarse entre los botones, cada botón es también un punto de tabulación.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Asigna un único punto de tabulación a los patrones de interfaz de usuario familiares

En los casos donde el diseño siga un patrón de interfaz de usuario conocido para los grupos de controles, la asignación de un único punto de tabulación al grupo puede mejorar la eficacia de navegación para los usuarios.

Algunos ejemplos son los siguientes:
-   `RadioButtons`
-   Varios `ListViews` que se ven y se comportan como una única `ListView`
-   Cualquier interfaz de usuario hecha para verse y comportarse como una cuadrícula de iconos (por ejemplo, los iconos del menú Inicio)

#### <a name="specifying-control-group-behavior"></a>Especificar el comportamiento del grupo de controles

Usa las siguientes API para admitir el comportamiento personalizado de grupos de controles (todos se explican con más detalle más adelante en este tema):

-   [XYFocusKeyboardNavigation](custom-keyboard-interactions.md#xyfocuskeyboardnavigation) permite la navegación mediante de teclas de dirección entre los controles.
-   [TabFocusNavigation](custom-keyboard-interactions.md#tab-navigation) indica si existen varios puntos de tabulación o solo un punto de tabulación.
-   [FindFirstFocusableElement y FindLastFocusableElement](managing-focus-navigation.md#findfirstfocusableelement) establece el foco en el primer elemento con la tecla **Inicio** y el último elemento con la tecla **Fin**.

En la siguiente imagen se muestra un comportamiento intuitivo de navegación mediante el teclado para un grupo de controles de botones de radio asociados. En este caso, te recomendamos un único punto de tabulación para el grupo de controles, navegación interna entre los botones de radio con las teclas de dirección, la tecla **Inicio** enlazada al primer botón de radio, y la tecla **Fin** enlazada al último botón de radio.

![implementación](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>El teclado y el Narrador

El Narrador es una herramienta de accesibilidad de la interfaz de usuario dirigida a los usuarios del teclado (también se admiten otros tipos de entrada). Sin embargo, la funcionalidad del Narrador va más allá de las interacciones de teclado admitidas por las aplicaciones para UWP y es necesario prestar especial atención al diseñar tu aplicación para UWP para el Narrador. (La [página de conceptos básicos del Narrador](https://support.microsoft.com/help/22808/windows-10-narrator-learning-basics) te guía a través de la experiencia del usuario con el Narrador).

Algunas de las diferencias entre los comportamientos del teclado para UWP y los admitidos por el Narrador incluyen:
-   Combinaciones de teclas adicionales para la navegación a elementos de la interfaz de usuario que no están expuestos a través de la navegación habitual mediante el teclado, como Bloq Mayús+teclas de dirección para leer las etiquetas de los controles.
-   Navegación a los elementos deshabilitados. De manera predeterminada, los elementos deshabilitados no se exponen a través de la navegación habitual mediante el teclado.
    -   "Vistas" de controles para la navegación más rápida en función de la granularidad de la interfaz de usuario. Los usuarios pueden navegar a los elementos, caracteres, palabras, líneas, párrafos, vínculos, encabezados, tablas, puntos de referencia y sugerencias. La navegación habitual mediante el teclado expone estos objetos como una lista plana, lo que podría hacer que la navegación fuese engorrosa, a menos que proporciones teclas de método abreviado.

#### <a name="case-study--autosuggestbox-control"></a>Caso práctico: control AutoSuggestBox

El botón de búsqueda para `AutoSuggestBox` no es accesible para la navegación habitual mediante el teclado con las teclas TAB y de dirección porque el usuario puede presionar la tecla **ENTRAR** para enviar la consulta de búsqueda. Sin embargo, es accesible mediante el Narrador cuando el usuario presiona Bloq Mayús+una tecla de dirección.

![sugerencia automática de foco del teclado](images/keyboard/auto-suggest-keyboard.png)

***Con el teclado***, *los usuarios usan la tecla* ***ENTRAR*** *para enviar la consulta de búsqueda*

<table>
  <tr>
    <td>
      <p>![sugerencia automática de foco del Narrador](images/keyboard/auto-suggest-narrator-1.png)</p>
      <p>**Con el Narrador,** *los usuarios pueden usar la tecla ENTRAR para enviar la consulta de búsqueda*</P>
    </td>
    <td>
      <p>![sugerencia automática de foco del Narrador en búsquedas](images/keyboard/auto-suggest-narrator-2.png)</p>
      <p>*El usuario también es capaz de acceder al botón de búsqueda mediante Bloq Mayús+tecla de dirección derecha y luego presionar la barra espaciadora*</p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>El teclado y el controlador para juegos y el control remoto de Xbox

Los controles para juegos y controles remotos de Xbox admiten muchos comportamientos y experiencias del teclado para UWP. Sin embargo, debido a la falta de varias opciones de teclas que sí están disponibles en un teclado, el controlador para juegos y el control remoto no tienen muchas optimizaciones de teclado (el control remoto es incluso más limitado que el controlador para juegos).

Consulta [Diseño para Xbox y televisión](designing-for-tv.md#gamepad-and-remote-control) para obtener más información sobre la compatibilidad de UWP con la entrada del controlador para juegos y el control remoto.

A continuación se muestran algunas asignaciones clave entre el teclado, el controlador para juegos y el control remoto.

| **Teclado**  | **Controlador para juegos**                         | **Control remoto**  |
|---------------|-------------------------------------|---------------------|
| Barra espaciadora         | Botón A                            | Botón de selección       |
| ENTRAR         | Botón A                            | Botón de selección       |
| Escape        | Botón B                            | Botón Atrás         |
| Inicio/Fin      | N/D                                 | N/D                 |
| RePág/AvPág  | Botón de gatillo para desplazamiento vertical, botón superior para desplazamiento horizontal   | N/D                 |

Algunas diferencias clave que debes tener en cuenta al diseñar la aplicación para UWP para su uso con el controlador para juegos y el control remoto incluyen:
-   La entrada de texto requiere que el usuario presione A activar un control de texto.
-   La navegación con foco no se limita a los grupos de controles; los usuarios pueden navegar libremente a cualquier elemento activable de la interfaz de usuario en la aplicación.

    **NOTA** El foco puede moverse a cualquier elemento activable de la interfaz de usuario en la dirección de presión de las teclas, a menos que sea una superposición de la interfaz de usuario o se especifique una [participación del foco](designing-for-tv.md#focus-engagement), lo que impide que el foco entre o salga de una región hasta que se active o desactive con el botónA. Consulta la sección de [navegación direccional](#directional-navigation) para obtener más información.
-   La cruceta y los botones de la palanca izquierda se usan para mover el foco entre los controles y para la navegación interna.

    **NOTA** El controlador para juegos y el control remoto solo se desplazan a los elementos que están en el mismo orden visual que la tecla direccional presionada. La navegación está deshabilitada en esa dirección cuando no hay ningún elemento posterior que pueda recibir el foco. Según la situación, los usuarios del teclado no siempre tienen esa restricción. Para obtener más información, consulta la sección [Optimización integrada del teclado](#built-in-keyboard-optimization).

#### <a name="directional-navigation-a-namedirectional-navigation"></a>Navegación direccional <a name="directional-navigation">

La navegación direccional se administra mediante una clase auxiliar [FocusManager](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.FocusManager) de UWP, que toma la tecla direccional presionada (tecla de dirección, cruceta) e intenta mover el foco en la dirección visual correspondiente.

A diferencia del teclado, cuando una aplicación descarta el [modo de mouse](designing-for-tv.md#mouse-mode), la navegación direccional se aplica en toda la aplicación para el controlador para juegos y el control remoto. Para obtener más información sobre las optimizaciones de navegación direccional para el controlador para juegos y el control remoto, visita la sección [Interacción y navegación con foco XY](designing-for-tv.md#xy-focus-navigation-and-interaction).

**NOTA** La navegación mediante la tecla TAB del teclado no se considera como navegación direccional. Para obtener más información, consulta la sección [Puntos de tabulación](#tab-stops).

<table>
  <tr>
    <td>
      <p>![navegación direccional](images/keyboard/directional-navigation.png)</p>
      <p>***Navegación direccional admitida*** </br>*Con las teclas direccionales (flechas del teclado, cruceta del controlador para juegos y control remoto), el usuario puede desplazarse entre distintos controles.*</p>
    </td>
    <td>
      <p>![sin navegación direccional](images/keyboard/no-directional-navigation.png)</p>
      <p>***Navegación direccional no admitida*** </br>*El usuario no puede desplazarse entre los distintos controles mediante las teclas direccionales. Los otros métodos de navegación entre los controles (tecla TAB) no se ven afectados.*</p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization-a-namebuilt-in-keyboard-optimization"></a>Optimización integrada del teclado <a name="built-in-keyboard-optimization">

Según el diseño y los controles que se usen, las aplicaciones para UWP pueden optimizarse específicamente para la entrada de teclado.

En el siguiente ejemplo se muestra un grupo de elementos de lista, elementos de cuadrícula y elementos de menú a los que se les han asignado un único punto de tabulación (consulta la sección [Puntos de tabulación](#tab-stops)). Cuando el grupo tiene el foco, se realiza la navegación interna con las teclas direccionales en el orden visual correspondiente (consulta la sección [Navegación](#navigation)).

![navegación con teclas de dirección por una sola columna](images/keyboard/single-column-arrow.png)

***Navegación con teclas de dirección por una sola columna***

![navegación con teclas de dirección por una sola fila](images/keyboard/single-row-arrow.png)

***Navegación con teclas de dirección por una sola fila***

![navegación con teclas de dirección por varias filas y columnas](images/keyboard/multiple-column-and-row-navigation.png)

***Navegación con teclas de dirección por varias filas y columnas***

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Ajuste de elementos homogéneos de la vista de lista y de cuadrícula

La navegación direccional no siempre es la manera más eficaz de desplazarse por varias filas y columnas de elementos List y GridView.

**NOTA** En general, los elementos de menú son listas de una sola columna, pero en algunos casos es posible que se apliquen reglas de enfoque especiales (consulta [Interfaz de usuario emergente](#popup-ui)).

Los objetos List y Grid pueden crearse con varias filas y columnas. Suelen ordenarse en row-major (donde los elementos llenan toda la fila antes de rellenar la fila siguiente) o column-major (donde los elementos rellenan toda la columna antes de rellenar la siguiente columna). El orden de row-major o column-major depende de la dirección de desplazamiento, y debes asegurarte de que el orden de los elementos no entre en conflicto con esta dirección.

En el orden row-major (donde los elementos se rellenan de izquierda a derecha y de arriba abajo), cuando el foco está en el último elemento de una fila y se presiona la tecla de flecha derecha, el foco se mueve al primer elemento de la fila siguiente. Este mismo comportamiento se produce a la inversa: cuando el foco se encuentra en el primer elemento en una fila y se presiona la tecla de flecha izquierda, el foco se mueve al último elemento de la fila anterior.

En el orden column-major (donde los elementos se rellenan de arriba abajo y de izquierda a derecha), cuando el foco está en el último elemento de una columna y el usuario presiona la tecla de flecha abajo, el foco se mueve al primer elemento de la columna siguiente. Este mismo comportamiento se produce a la inversa: cuando el foco se encuentra en el primer elemento en una columna y se presiona la tecla de flecha arriba, el foco se mueve al último elemento de la columna anterior.

<table>
  <tr>
    <td>
      <p>![navegación mediante teclado con orden por filas](images/keyboard/row-major-keyboard.png)</p>
      <p>***Navegación mediante teclado con orden por filas***</p>
    </td>
    <td>
      <p>![navegación mediante teclado con orden por columnas](images/keyboard/column-major-keyboard.png)</p>
      <p>***Navegación mediante teclado con orden por columnas***</p>
    </td>
  </tr>
</table>

#### <a name="popup-ui-a-namepopup-ui"></a>Interfaz de usuario emergente <a name="popup-ui">

Como se mencionó, deberías intentar asegurarte de que la navegación direccional se corresponda con el orden visual de los controles de la interfaz de usuario de la aplicación.

Algunos controles, como `ContextMenu`, `AppBarOverflowMenu` y `AutoSuggest`, incluyen un menú emergente que aparece en una ubicación y dirección relativas al control principal (basado en el espacio disponible en pantalla). Por ejemplo, cuando no hay espacio suficiente para que el menú se abra hacia abajo (la dirección predeterminada), se abre hacia arriba. No hay ninguna garantía de que el menú se abra en la misma dirección cada vez.

<table>
  <td>![la barra de comandos se abre hacia abajo con la tecla de flecha abajo](images/keyboard/command-bar-open-down.png)</td>
  <td>![la barra de comandos se abre hacia arriba con la tecla de flecha abajo](images/keyboard/command-bar-open-up.png)</td>
</table>

Para estos controles, cuando se abre el menú por primera vez (y no hay ningún elemento seleccionado por el usuario), la tecla de flecha abajo siempre establece el foco en el primer elemento y la tecla de flecha arriba siempre establece el foco en el último elemento del menú. Del mismo modo, cuando se selecciona el último elemento y se presiona la tecla de flecha abajo, el foco se mueve al primer elemento del menú y, cuando se selecciona el primer elemento y se presiona la tecla de flecha arriba, el foco se mueve al último elemento del menú.

Deberías intentar emular estos mismos comportamientos en tus controles personalizados. Para obtener un código de muestra sobre cómo implementar este comportamiento, consulta el documento [Managing focus navigation](managing-focus-navigation.md#popup-ui-code-sample) (Administrar la navegación del foco).

## <a name="test-your-app"></a>Probar la aplicación

Prueba tu aplicación con todos los dispositivos de entrada admitidos para garantizar que puedas desplazarte a los elementos de la interfaz de usuario forma coherente e intuitiva y de que ningún elemento inesperado interfiera con el orden de tabulación deseado.

## <a name="related-articles"></a>Artículos relacionados
* [Eventos de teclado](keyboard-events.md)
* [Identificar dispositivos de entrada](identify-input-devices.md)
* [Responder a la presencia del teclado táctil](respond-to-the-presence-of-the-touch-keyboard.md)
* [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

## <a name="appendix"></a>Apéndice

### <a name="software-keyboard-a-nametouch-keyboard"></a>Teclado de software <a name="touch-keyboard">

El teclado de software es un teclado que se muestra en la pantalla y que el usuario puede usar en lugar del teclado físico para escribir datos con entrada táctil, el mouse, el lápiz o la pluma u otro dispositivo señalador (no es necesaria una pantalla táctil). En la pantalla táctil, estos teclados también pueden tocarse directamente para escribir texto. En los dispositivos XboxOne, las teclas individuales deben seleccionarse moviendo el elemento visual de foco o usando métodos abreviados con el controlador para juegos o el control remoto.

![Teclado táctil de Windows10](images/keyboard/kbdpcdefault.png)

***Teclado táctil de Windows10***

![Teclado táctil de Windows 10 Phone](images/keyboard/kbdwpdefault.png)

***Teclado táctil de Windows Phone 10***

![Teclado en pantalla de Xbox One](images/keyboard/xbox-onscreen-keyboard.png)

***Teclado en pantalla de Xbox One***

Según el dispositivo, el teclado de software aparece cuando un campo de texto u otro control de texto editable obtiene el foco o cuando el usuario manualmente lo habilita a través del **centro de notificaciones**:

![Icono del teclado táctil en el centro de notificaciones](images/keyboard/touch-keyboard-notificationcenter.png)

Si la aplicación establece el foco mediante programación en un control de entrada de texto, no se invoca el teclado táctil. Esto elimina comportamientos inesperados no originados directamente por el usuario. Sin embargo, el teclado se oculta automáticamente cuando el foco se mueve mediante programación a un control de entrada que no es de texto.

El teclado táctil normalmente permanece visible mientras el usuario navega por los controles de un formulario. Este comportamiento puede variar en función de los otros tipos de control dentro del formulario.

La siguiente es una lista de controles de no edición que pueden recibir el foco durante una sesión de entrada de texto con el teclado táctil sin por ello descartar el teclado. En lugar de renovar innecesariamente la interfaz de usuario y, posiblemente, desorientar al usuario, el teclado táctil permanece visible porque es probable que el usuario alterne entre estos controles y la entrada de texto con el teclado táctil.

-   Casilla
-   Cuadro combinado
-   Botón de radio
-   Barra de desplazamiento
-   Árbol
-   Elemento del árbol
-   Menú
-   Barra de menús
-   Elemento de menú
-   Barra de herramientas
-   Lista
-   Elemento de lista

Estos son algunos ejemplos de los diferentes modos del teclado táctil. La primera imagen es el diseño predeterminado, la segunda es el diseño para pulgares (puede que no esté disponible para todos los idiomas).

![teclado táctil en el modo de diseño predeterminado](images/keyboard/touchkeyboard-standard.png)

***El teclado táctil en el modo de diseño predeterminado***

![teclado táctil en el modo de diseño expandido](images/keyboard/touchkeyboard-expanded.png)

***El teclado táctil en el modo de diseño predeterminado expandido***

![teclado táctil en el modo de diseño para pulgares](images/keyboard/touchkeyboard-thumb.png)

***El teclado táctil en el modo de diseño para pulgares predeterminado***

![teclado táctil en el modo de diseño para pulgares numérico](images/keyboard/touchkeyboard-numeric-thumb.png)

***El teclado táctil en el modo de diseño para pulgares numérico***

La correcta interacción con el teclado permite a los usuarios emplear escenarios de aplicación básicos mediante el uso exclusivo del teclado. Es decir, que los usuarios pueden alcanzar todos los elementos interactivos de la interfaz de usuario y activar funciones predeterminadas. Hay una serie de factores que pueden afectar el grado de éxito, como la navegación por el teclado, las teclas de acceso para accesibilidad y las teclas de aceleración (o de método abreviado) para usuarios avanzados.

**NOTA**  El teclado táctil no admite la alternancia ni la mayoría de los comandos del sistema.

#### <a name="on-screen-keyboard-a-nameosk"></a>Teclado en pantalla <a name="osk">
Al igual que el teclado de software, el teclado en pantalla es un teclado software visual que puedes usar en lugar del teclado físico para escribir datos con entrada táctil, el mouse, el lápiz o la pluma u otro dispositivo señalador (no es necesaria una pantalla táctil). El teclado en pantalla se proporciona para sistemas que no incluyen un teclado físico, o para usuarios cuyos problemas de movilidad les impidan usar los dispositivos de entrada físicos tradicionales. El teclado en pantalla simula la mayoría de las funciones, si no todas, de un teclado de hardware.

Puede activarse desde la página Teclado, que se encuentra en Configuración &gt; Accesibilidad.

**NOTA** El teclado en pantalla tiene prioridad sobre el teclado táctil, que no se mostrará si el primero está presente.

![teclado en pantalla](images/keyboard/osk.png)

***Teclado en pantalla***

Para obtener más información sobre el teclado en pantalla, visita la página [Teclado en pantalla](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard).
