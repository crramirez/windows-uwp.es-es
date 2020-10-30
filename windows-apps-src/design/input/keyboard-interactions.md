---
description: Obtenga información sobre cómo diseñar y optimizar las aplicaciones de Windows para que proporcionen la mejor experiencia posible para los usuarios avanzados de teclado y los con discapacidades y otros requisitos de accesibilidad.
title: Interacciones de teclado
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
keywords: teclado, accesibilidad, navegación, foco, texto, entrada, interacciones del usuario, controlador para juegos, remoto
ms.date: 09/24/2020
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 6fba6654913f481faba98c598e4c683d62b09adf
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034808"
---
# <a name="keyboard-interactions"></a>Interacciones de teclado

![Imagen principal de teclado](images/keyboard/keyboard-hero.jpg)

Obtenga información sobre cómo diseñar y optimizar las aplicaciones de Windows para que proporcionen la mejor experiencia posible para los usuarios avanzados de teclado y los con discapacidades y otros requisitos de accesibilidad.

En todos los dispositivos, la entrada de teclado es una parte importante de la experiencia de interacción de aplicaciones de Windows general. Una experiencia de teclado bien diseñada permite a los usuarios navegar de forma eficaz por la interfaz de usuario de la aplicación y acceder a toda su funcionalidad sin levantar las manos del teclado.

![imagen de teclado y controlador para juegos](images/keyboard/keyboard-gamepad.jpg)

*Los **patrones de interacción comunes se comparten entre el teclado y el controlador de juegos** _

En este tema, nos centraremos específicamente en el diseño de aplicaciones de Windows para la entrada de teclado en PC. Sin embargo, una experiencia de teclado bien diseñada es importante para admitir herramientas de accesibilidad como el narrador de Windows, mediante [teclados de software](#software-keyboard) como el teclado táctil y el teclado en pantalla (OSK), y para controlar otros tipos de dispositivos de entrada, como el controlador de juegos de Xbox y el control remoto.

Muchas de las instrucciones y recomendaciones que se describen aquí, incluidos los [objetos visuales de foco](#focus-visuals), [las claves de acceso](#access-keys)y la navegación de la [interfaz de usuario](#navigation), también se aplican a estos otros escenarios.

_ *Nota* * aunque se usan teclados de hardware y software para la entrada de texto, el enfoque de este tema es la navegación y la interacción.

## <a name="built-in-support"></a>Compatibilidad integrada

Junto con el mouse, el teclado es el periférico más usado en los equipos y, como tal, es una parte fundamental de la experiencia del PC. Los usuarios de equipos esperan una experiencia completa y coherente tanto del sistema como de las aplicaciones individuales en respuesta a la entrada de teclado.

Todos los controles de UWP incluyen compatibilidad integrada para experiencias de teclado enriquecidas e interacciones de usuario, mientras que la propia plataforma proporciona una amplia base para crear experiencias de teclado que se sientan más adecuadas para sus controles y aplicaciones personalizados.

![teclado con imagen de teléfono](images/keyboard/keyboard-phone.jpg)

**_UWP admite el teclado con cualquier dispositivo_* .

## <a name="basic-experiences"></a>Experiencias básicas
![Dispositivos basados en el foco](images/keyboard/focus-based-devices.jpg)

Como se mencionó anteriormente, los dispositivos de entrada, como el controlador de juegos de Xbox y el control remoto, y las herramientas de accesibilidad como narrador, comparten gran parte de la experiencia de entrada de teclado para la navegación y los comandos. Esta experiencia común en los tipos de entrada y las herramientas minimiza el trabajo adicional que le proporciona y contribuye al objetivo "crear una vez, ejecutar en cualquier lugar" del Plataforma universal de Windows.

En caso necesario, se identificarán las diferencias clave que se deben tener en cuenta y se debe describir cualquier mitigación que se deba tener en cuenta.

Estos son los dispositivos y las herramientas que se describen en este tema:

| Dispositivo o herramienta                       | Description     |
|-----------------------------------|-----------------|
|Teclado (hardware y software)   |Además del teclado de hardware estándar, las aplicaciones de Windows admiten dos teclados de software: el [teclado táctil (o el software)](#software-keyboard) y el [teclado en pantalla](#on-screen-keyboard).|
|Controlador para juegos y control remoto         |El controlador de juegos de Xbox y el control remoto son dispositivos de entrada fundamentales en la [experiencia de 10 pies](../devices/designing-for-tv.md). Para obtener detalles específicos sobre la compatibilidad de Windows con el controlador para juegos y el control remoto, consulte [interacciones de control remoto y controlador de juegos](gamepad-and-remote-interactions.md).|
|Lectores de pantalla (narrador)          |Narrador es un lector de pantalla integrado para Windows que proporciona características y experiencias de interacción exclusivas, pero que sigue basándose en la entrada y la navegación básicas del teclado. Para obtener detalles sobre el narrador, consulte [Introducción a narrador](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator).|

## <a name="custom-experiences-and-efficient-keyboarding"></a>Experiencias personalizadas y teclado eficaz
Como se mencionó, la compatibilidad con el teclado es fundamental para garantizar que las aplicaciones funcionen bien para los usuarios con diferentes aptitudes, capacidades y expectativas. Se recomienda dar prioridad a lo siguiente.
- Compatibilidad con la navegación y la interacción con el teclado
    - Asegúrese de que los elementos procesables se identifican como delimitadores de tabulación (y los elementos no procesables no) y el orden de navegación es lógico y predecible (consulte [puntos de tabulación](#tab-stops)).
    - Establecer el foco inicial en el elemento lógico más (vea el [foco inicial](#initial-focus))
    - Proporcionar navegación por la tecla de dirección para "navegaciones internas" (vea [navegación](#navigation))
- Compatibilidad con métodos abreviados de teclado
    - Proporcionar teclas de aceleración para acciones rápidas (consulte [aceleradores](#accelerators))
    - Proporcionar claves de acceso para navegar por la interfaz de usuario de la aplicación (consulte [claves de acceso](access-keys.md))

### <a name="focus-visuals"></a>Objetos visuales de foco

UWP admite un diseño visual de enfoque único que funciona bien con todos los tipos de entrada y experiencias.
![Foco visual](images/keyboard/focus-visual.png)

Un visual de foco:

- Se muestra cuando un elemento de la interfaz de usuario recibe el foco de un teclado y/o un control remoto
- Se representa como un borde resaltado alrededor del elemento de la interfaz de usuario para indicar que se puede realizar una acción
- Ayuda a un usuario a navegar por una interfaz de usuario de la aplicación sin perderse
- Se puede personalizar para su aplicación (consulte [objetos visuales de foco de alta visibilidad](guidelines-for-visualfeedback.md#high-visibility-focus-visuals))

_ *Note* * el aspecto visual del foco de UWP no es el mismo que el rectángulo de foco del narrador.

### <a name="tab-stops"></a>Puntos de tabulación

Para usar un control (incluidos los elementos de navegación) con el teclado, el control debe tener un foco. Una manera de que un control reciba el foco de teclado es hacer que sea accesible a través de la navegación por tabulación mediante su identificación como una tabulación en el orden de tabulación de la aplicación.

Para que un control se incluya en el orden de tabulación, la propiedad [IsEnabled](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) debe establecerse en **true** y la propiedad [IsTabStop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) debe establecerse en **true** .

Para excluir específicamente un control del orden de tabulación, establezca la propiedad [IsTabStop](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) en **false** .

De forma predeterminada, el orden de tabulación refleja el orden en que se crean los elementos de la interfaz de usuario. Por ejemplo, si un `StackPanel` contiene un `Button` , un `Checkbox` y un `TextBox` , el orden de tabulación es `Button` , `Checkbox` y `TextBox` .

Puede invalidar el orden de tabulación predeterminado estableciendo la propiedad [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) .

#### <a name="tab-order-should-be-logical-and-predictable"></a>El orden de tabulación debe ser lógico y predecible

Un modelo de navegación de teclado bien diseñado, mediante un orden de tabulación lógico y predecible, hace que la aplicación sea más intuitiva y ayude a los usuarios a explorar, detectar y obtener acceso a la funcionalidad de forma más eficaz y eficaz.

Todos los controles interactivos deben tener tabulaciones (a menos que estén en un [Grupo](#control-group)), mientras que los controles no interactivos, como las etiquetas, no deberían.

Evite un orden de tabulación personalizado que haga que el foco se desplace por la aplicación. Por ejemplo, una lista de controles de un formulario debe tener un orden de tabulación que fluya desde la parte superior a la inferior y de izquierda a derecha (según la configuración regional).

Vea [accesibilidad del teclado](../accessibility/keyboard-accessibility.md) para obtener más detalles sobre cómo personalizar las tabulaciones.

#### <a name="try-to-coordinate-tab-order-and-visual-order"></a>Intente coordinar el orden de tabulación y el orden visual

La coordinación del orden de tabulación y el orden visual (también denominado orden de lectura o orden de presentación) ayuda a reducir la confusión de los usuarios cuando navegan por la interfaz de usuario de la aplicación.

Intente clasificar y presentar los comandos, controles y contenido más importantes en primer lugar en el orden de tabulación y en el orden visual. Sin embargo, la posición de visualización puede depender del contenedor de diseño primario y de ciertas propiedades de los elementos secundarios que influyen en el diseño. En concreto, los diseños que usan una metáfora de cuadrícula o una tabla metáfora pueden tener un orden visual muy diferente del orden de tabulación.

**Nota:** El orden visual también depende de la configuración regional y el idioma.

### <a name="initial-focus"></a>Foco inicial

Foco inicial especifica el elemento de la interfaz de usuario que recibe el foco cuando una aplicación o una página se inicia o activa por primera vez. Al usar un teclado, es de este elemento que un usuario comienza a interactuar con la interfaz de usuario de la aplicación.

En el caso de las aplicaciones UWP, el foco inicial se establece en el elemento con el valor de [TabIndex](/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) más alto que puede recibir el foco. Los elementos secundarios de los controles de contenedor se omiten. En un empate, el primer elemento del árbol visual recibe el foco.

#### <a name="set-initial-focus-on-the-most-logical-element"></a>Establecer el foco inicial en el elemento lógico más

Establezca el foco inicial en el elemento de la interfaz de usuario de la primera acción, o primaria, que los usuarios tienen más probabilidades de realizar al iniciar la aplicación o ir a una página. Estos son algunos ejemplos:
-   Una aplicación fotográfica en la que el foco se establece en el primer elemento de una galería
-   Una aplicación de música en la que el foco se establece en el botón de reproducción

#### <a name="dont-set-initial-focus-on-an-element-that-exposes-a-potentially-negative-or-even-disastrous-outcome"></a>No establezca el foco inicial en un elemento que exponga un resultado potencialmente negativo, o incluso desastroso,.

Este nivel de funcionalidad debe ser la elección de un usuario. Establecer el foco inicial en un elemento con un resultado significativo podría producir la pérdida de datos o el acceso al sistema imprevistos. Por ejemplo, no establezca el foco en el botón eliminar al navegar a un correo electrónico.

Vea [navegación centrada](focus-navigation.md) para obtener más detalles sobre cómo invalidar el orden de tabulación.

### <a name="navigation"></a>Navegación

La navegación con el teclado normalmente se admite a través de las teclas de tabulación y las teclas de dirección.

![Tabulador y teclas de dirección](images/keyboard/tab-and-arrow.png)

De forma predeterminada, los controles de UWP siguen estos comportamientos básicos del teclado:
-   **Las teclas de tabulación** se desplazan entre los controles accionables/activos en el orden de tabulación.
-   **MAYÚS + TAB** desplazar controles en el orden de tabulación inversa. Si el usuario ha navegado dentro del control mediante la tecla de dirección, el foco se establece en el último valor conocido dentro del control.
-   **Las teclas de dirección** exponen "navegación interna" específica del control cuando el usuario escribe "navegación interna". las teclas de dirección no se desplazan fuera de un control. Estos son algunos ejemplos:
    -   La tecla de dirección arriba/abajo mueve el foco dentro `ListView` de y `MenuFlyout`
    -   Modificar los valores actualmente seleccionados para `Slider` y `RatingsControl`
    -   Desplace el símbolo de intercalación dentro `TextBox`
    -   Expandir o contraer elementos dentro de `TreeView`

Use estos comportamientos predeterminados para optimizar la navegación del teclado de la aplicación.

#### <a name="use-inner-navigation-with-sets-of-related-controls"></a>Usar "navegación interna" con conjuntos de controles relacionados

Proporcionar la navegación por la tecla de dirección en un conjunto de controles relacionados refuerza su relación dentro de la organización general de la interfaz de usuario de la aplicación.

Por ejemplo, el `ContentDialog` control que se muestra aquí proporciona la navegación interna de forma predeterminada para una fila horizontal de botones (para controles personalizados, vea la sección [grupo de controles](#control-group) ).

![ejemplo de cuadro de diálogo](images/keyboard/dialog.png)

**_La interacción con una colección de botones relacionados se facilita con la navegación por la tecla de dirección_* .

Si los elementos se muestran en una sola columna, la tecla de dirección arriba/abajo navega por los elementos. Si los elementos se muestran en una sola fila, la tecla de dirección derecha/izquierda navega por los elementos. Si los elementos son varias columnas, se desplazan las cuatro teclas de dirección.

#### <a name="define-a-single-tab-stop-for-a-collection-of-related-controls"></a>Definir una única tabulación para una colección de controles relacionados

Al definir una única posición de tabulación para una colección de controles relacionados, o complementarios, puede minimizar el número de tabulaciones generales en la aplicación.

Por ejemplo, las siguientes imágenes muestran dos controles apilados `ListView` . La imagen de la izquierda muestra la navegación por la tecla de dirección que se usa con una tabulación para desplazarse `ListView` por los controles, mientras que la imagen de la derecha muestra cómo la navegación entre los elementos secundarios podría ser más fácil y eficaz eliminando la necesidad de para atravesar los controles primarios con una tecla de tabulación.


<table>
  <td><img src="images/keyboard/arrow-and-tab.png" alt="arrow and tab" /></td>
  <td><img src="images/keyboard/arrow-only.png" alt="arrow only" /></td>
</table>

_*_La interacción con dos controles ListView apilados puede ser más fácil y eficaz, ya que se elimina la tabulación y se navega por las teclas de flecha._*_

Visite la sección del [grupo de control](#control-group) para obtener información sobre cómo aplicar los ejemplos de optimización a la interfaz de usuario de la aplicación.

### <a name="interaction-and-commanding"></a>Interacción y comandos

Una vez que un control tiene el foco, un usuario puede interactuar con él e invocar cualquier funcionalidad asociada mediante la entrada de teclado específica.

#### <a name="text-entry"></a>Entrada de texto

En el caso de los controles diseñados específicamente para entradas de texto como `TextBox` y `RichEditBox` , todas las entradas de teclado se usan para escribir o navegar por el texto, lo que tiene prioridad sobre otros comandos de teclado. Por ejemplo, el menú desplegable de un `AutoSuggestBox` control no reconoce la clave _ *Space* * como un comando de selección.

![entrada de texto](images/keyboard/text-entry.png)

#### <a name="space-key"></a>Clave de espacio

Cuando no está en modo de entrada de texto, la tecla **barra espaciadora** invoca la acción o el comando asociado al control que tiene el foco (como una pulsación con el toque o un clic con el mouse).

![clave de espacio](images/keyboard/space-key.png)

#### <a name="enter-key"></a>Entrar (tecla)

La tecla **entrar** puede realizar una serie de interacciones de usuario comunes, dependiendo del control con el foco:
-   Activa controles de comando como `Button` o `Hyperlink` . Para evitar la confusión del usuario final, la tecla **entrar** también activa controles que se parecen a controles de comando como `ToggleButton` o `AppBarToggleButton` .
-   Muestra la interfaz de usuario del selector para controles como `ComboBox` y `DatePicker` . La tecla **entrar** también confirma y cierra la interfaz de usuario del selector.
-   Activa controles de lista como `ListView` , `GridView` y `ComboBox` .
    -   La tecla **entrar** realiza la acción de selección como la clave de **espacio** para los elementos de lista y cuadrícula, a menos que haya una acción adicional asociada a estos elementos (abrir una nueva ventana).
    -   Si hay una acción adicional asociada al control, la tecla **entrar** realiza la acción adicional y la clave de **espacio** realiza la acción de selección.

**Nota:** La tecla **entrar** y la clave de **espacio** no siempre realizan la misma acción, pero a menudo lo hacen.

![tecla entrar](images/keyboard/enter-key.png)

La tecla ESC permite al usuario cancelar la interfaz de usuario transitoria (junto con cualquier acción en curso en esa interfaz de usuario).

Estos son algunos ejemplos de esta experiencia:
-   El usuario abre un `ComboBox` con un valor seleccionado y usa las teclas de dirección para desplazar la selección del foco a un nuevo valor. Al presionar la tecla Esc `ComboBox` , se cierra y se restablece el valor seleccionado al valor original.
-   El usuario invoca una acción de eliminación permanente para un correo electrónico y se le solicita `ContentDialog` que confirme la acción. El usuario decide que esta no es la acción deseada y presiona la tecla **ESC** para cerrar el cuadro de diálogo. Como la tecla **ESC** está asociada al botón **Cancelar** , el cuadro de diálogo se cierra y se cancela la acción. La tecla **ESC** solo afecta a la interfaz de usuario transitoria, no se cierra o se vuelve a navegar a través de la interfaz de usuario de la aplicación.

![Tecla ESC](images/keyboard/esc-key.png)

#### <a name="home-and-end-keys"></a>Teclas de inicio y fin

Las teclas **Inicio** y **fin** permiten a los usuarios desplazarse al principio o al final de una región de la interfaz de usuario.

Estos son algunos ejemplos de esta experiencia:
-   En el caso de los `ListView` `GridView` controles y, la tecla **Inicio** mueve el foco al primer elemento y lo desplaza a la vista, mientras que la tecla **fin** mueve el foco al último elemento y lo desplaza a la vista.
-   Para un `ScrollView` control, la tecla **Inicio** se desplaza hasta la parte superior de la región, mientras que la tecla **fin** se desplaza hasta la parte inferior de la región (el foco no cambia).

![teclas Inicio y fin](images/keyboard/home-and-end.png)

#### <a name="page-up-and-page-down-keys"></a>Teclas Re Pág y Av Pág

Las claves de **Página** permiten a un usuario desplazarse por una región de la interfaz de usuario en incrementos discretos.

Por ejemplo, para `ListView` los `GridView` controles y, la tecla **Re Pág** desplaza la región hacia arriba por una "página" (normalmente el alto de la ventanilla) y mueve el foco a la parte superior de la región. Como alternativa, la tecla **Av Pág** desplaza la región hacia abajo por una página y mueve el foco a la parte inferior de la región.

![tecla RE PÁG y Av Pág](images/keyboard/page-up-and-down.png)

### <a name="keyboard-shortcuts"></a>Accesos directos del teclado

Los métodos abreviados de teclado pueden facilitar el uso de la aplicación al proporcionar compatibilidad mejorada para la accesibilidad y una mayor eficiencia para los usuarios del teclado.

Además de admitir la navegación y la activación del teclado en la aplicación, también se recomienda proporcionar accesos directos para la funcionalidad de la aplicación. La navegación por tabulación proporciona un buen nivel básico de compatibilidad con el teclado, pero con una interfaz de usuario más compleja podría querer agregar también compatibilidad con las teclas de método abreviado. 

Un método abreviado es una combinación de teclas que mejora la productividad al proporcionar al usuario una forma eficaz de acceder a las funciones de la aplicación. Existen dos tipos de métodos abreviados:
-   Los [aceleradores](#accelerators) son accesos directos que invocan un comando de aplicación. La aplicación puede proporcionar o no una interfaz de usuario específica que se corresponda con el comando. Normalmente, los aceleradores se componen de la tecla Ctrl más una tecla de letra.
-   [Las claves de acceso](#access-keys) son accesos directos que establecen el foco en la interfaz de usuario específica de la aplicación. Las claves de acceso suelen estar formadas por la tecla Alt y por una tecla de letra.

Proporcionar métodos abreviados de teclado coherentes que admitan tareas similares entre aplicaciones hace que sean mucho más útiles y eficaces y ayuda a los usuarios a recordarlos.

#### <a name="accelerators"></a>Aceleradores

Los aceleradores ayudan a los usuarios a realizar acciones comunes en una aplicación de forma mucho más rápida y eficaz. 

Ejemplos de aceleradores:
-   Al presionar la tecla Ctrl + N en cualquier parte de la aplicación de **correo** , se inicia un nuevo elemento de correo.
-   Al presionar la tecla Ctrl + E en cualquier parte de Microsoft Edge (y muchas aplicaciones Microsoft Store), se inicia la búsqueda.

Los aceleradores tienen las siguientes características:
-   Principalmente utilizan secuencias de teclas de función y Ctrl (las teclas de método abreviado del sistema de Windows también usan Alt + claves no alfanuméricas y la tecla del logotipo de Windows).
-   Se asignan únicamente a los comandos que más se usan.
-   Se espera que se memoricen y solo se documentan en los menús, en la información sobre herramientas y en la Ayuda.
-   Tienen efecto en toda la aplicación, cuando se admiten.
-   Deben asignarse de forma coherente a medida que se memorizan y no se documentan directamente.

#### <a name="access-keys"></a>Claves de acceso

Vea la página [claves de acceso](access-keys.md) para obtener información más detallada sobre la compatibilidad con las claves de acceso con UWP.

Las claves de acceso ayudan a los usuarios con las discapacidades de las funciones del motor a presionar una tecla cada vez para actuar sobre un elemento específico en la interfaz de usuario. Además, las claves de acceso se pueden usar para comunicar teclas de método abreviado adicionales para ayudar a los usuarios avanzados a realizar acciones rápidamente.

Las teclas de acceso tienen las siguientes características:
-   Usan la tecla Alt y una tecla alfanumérica.
-   Son principalmente para la accesibilidad.
-   Están documentados directamente en la interfaz de usuario, adyacentes al control, a través de las [sugerencias de teclas](access-keys.md).
-   Tienen efecto únicamente en la ventana actual y desplazan al control o elemento de menú correspondiente.
-   Las claves de acceso deben asignarse de forma coherente a los comandos de uso frecuente (especialmente los botones de confirmación), siempre que sea posible.
-   Están traducidas.

#### <a name="common-keyboard-shortcuts"></a>Métodos abreviados de teclado comunes

La tabla siguiente es una pequeña muestra de los métodos abreviados de teclado que se usan con frecuencia. 

| Acción                               | Comando de teclas                                      |
|--------------------------------------|--------------------------------------------------|
| Seleccionar todo                           | Ctrl+A                                           |
| Seleccionar continuamente                  | Mayús+Tecla de flecha                                  |
| Guardar                                 | Ctrl+S                                           |
| Buscar                                 | Ctrl+F                                           |
| Imprimir                                | Ctrl+P                                           |
| Copiar                                 | Ctrl+C                                           |
| Cortar                                  | Ctrl+X                                           |
| Pegar                                | Ctrl+V                                           |
| Deshacer                                 | Ctrl+Z                                           |
| Pestaña siguiente                             | Ctrl+Tab                                         |
| Cerrar pestaña                            | Ctrl + F4 o Ctrl + W                                |
| Zoom semántico                        | Ctrl++ o Ctrl+-                                 |

Para obtener una lista completa de los métodos abreviados del sistema de Windows, consulte [métodos abreviados de teclado para Windows](https://support.microsoft.com/help/12445/windows-keyboard-shortcuts). Para obtener acceso a los métodos abreviados de aplicaciones comunes, consulte [métodos abreviados de teclado para aplicaciones de Microsoft](https://support.microsoft.com/help/13805/windows-keyboard-shortcuts-in-apps).

## <a name="advanced-experiences"></a>Experiencias avanzadas

En esta sección, se describen algunas de las experiencias de interacción de teclado más complejas que admiten las aplicaciones UWP, junto con algunos de los comportamientos que debe tener en cuenta cuando se usa la aplicación en diferentes dispositivos y con herramientas diferentes.

### <a name="control-group"></a>Grupo de control

Puede agrupar un conjunto de controles relacionados, o complementarios, en un "grupo de control" (o área direccional), que permite "navegación interna" mediante las teclas de dirección. El grupo de control puede ser una sola posición de tabulación, o puede especificar varias tabulaciones en el grupo de control.

#### <a name="arrow-key-navigation"></a>Navegación por la tecla de dirección

Los usuarios esperan compatibilidad con la navegación por la tecla de dirección cuando hay un grupo de controles similares relacionados en una región de la interfaz de usuario:
-   `AppBarButtons` en un `CommandBar`
-   `ListItems` o `GridItems` dentro `ListView` o `GridView`
-   `Buttons` énfasis `ContentDialog`

Los controles de UWP admiten la navegación por teclas de flecha de forma predeterminada. En el caso de los diseños personalizados y los grupos de control, use `XYFocusKeyboardNavigation="Enabled"` para proporcionar un comportamiento similar.

Considere la posibilidad de agregar compatibilidad con la navegación por teclas de dirección al usar los siguientes controles:

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/dialog.png" alt="Dialog buttons"/></p>
      <p><sup>Botones de cuadro de diálogo</sup></p>
      <p><img src="images/keyboard/radiobutton.png" alt="Radio buttons"/></p>
      <p><sup>RadioButtons</sup></p>     
    </td>
    <td>
      <p><img src="images/keyboard/appbar.png" alt="AppBar buttons"/></p>
      <p><sup>AppBarButtons</sup></p>
      <p><img src="images/keyboard/list-and-grid-items.png" alt="List and Grid items"/></p>
      <p><sup>ListItems y GridItems</sup></p>
    </td>    
  </tr>
</table>

#### <a name="tab-stops"></a>Puntos de tabulación

Dependiendo de la funcionalidad y el diseño de la aplicación, la mejor opción de navegación de un grupo de control puede ser una única posición de tabulación con navegación por las flechas a los elementos secundarios, varias tabulaciones o alguna combinación.

##### <a name="use-multiple-tab-stops-and-arrow-keys-for-buttons"></a>Usar varias tabulaciones y teclas de dirección para botones

Los usuarios de accesibilidad se basan en reglas de navegación de teclado bien establecidas, que normalmente no usan teclas de dirección para navegar por una colección de botones. Sin embargo, los usuarios sin discapacidades visuales podrían creer que el comportamiento es natural.

Un ejemplo de comportamiento predeterminado de UWP en este caso es `ContentDialog` . Aunque las teclas de dirección se pueden usar para navegar entre los botones, cada botón también es una tabulación.

##### <a name="assign-single-tab-stop-to-familiar-ui-patterns"></a>Asignación de una sola tabulación a patrones familiares de la interfaz de usuario

En los casos en los que el diseño sigue un patrón de interfaz de usuario conocido para los grupos de control, la asignación de una única posición de tabulación al grupo puede mejorar la eficacia de la navegación para los usuarios.

Algunos ejemplos son:
-   `RadioButtons`
-   Múltiplos `ListViews` que tienen el mismo aspecto y se comportan como un solo `ListView`
-   Cualquier interfaz de usuario para buscar y comportarse como una cuadrícula de mosaicos (como los mosaicos del menú Inicio)

#### <a name="specifying-control-group-behavior"></a>Especificar el comportamiento del grupo de control

Use las siguientes API para admitir el comportamiento del grupo de control personalizado (todos se describen con más detalle más adelante en este tema):

-   [XYFocusKeyboardNavigation](focus-navigation.md#2d-directional-navigation-for-keyboard) habilita la navegación por la tecla de dirección entre controles
-   [TabFocusNavigation](focus-navigation.md#tab-navigation) indica si hay varias tabulaciones o una sola tabulación
-   [FindFirstFocusableElement y FindLastFocusableElement](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) establecen el foco en el primer elemento con la tecla **Inicio** y el último elemento con la tecla **fin**

La imagen siguiente muestra un comportamiento intuitivo de navegación por el teclado para un grupo de controles de botones de radio asociados. En este caso, se recomienda una única tabulación para el grupo de control, la navegación interna entre los botones de radio mediante las teclas de dirección, la tecla **Inicio** enlazada al primer botón de radio y la tecla **fin** enlazada al último botón de radio.

![reunir todo](images/keyboard/putting-it-all-together.png)

### <a name="keyboard-and-narrator"></a>Teclado y narrador

Narrador es una herramienta de accesibilidad de interfaz de usuario orientada hacia los usuarios del teclado (también se admiten otros tipos de entrada). Sin embargo, la funcionalidad del narrador va más allá de las interacciones de teclado admitidas por las aplicaciones UWP y se requiere atención adicional al diseñar la aplicación para UWP para el narrador. (La [Página de conceptos básicos del narrador](https://support.microsoft.com/help/22808/windows-10-narrator-basics) le guía a través de la experiencia del usuario de narrador).

Algunas de las diferencias entre los comportamientos de teclado de UWP y los admitidos por el narrador incluyen:
-   Combinaciones clave adicionales para la navegación a los elementos de la interfaz de usuario que no se exponen a través de la navegación de teclado estándar, como Bloq Mayús + teclas de dirección para leer las etiquetas de control.
-   Navegación a los elementos deshabilitados. De forma predeterminada, los elementos deshabilitados no se exponen a través de la navegación de teclado estándar.
-   Controle "views" para una navegación más rápida basada en la granularidad de la interfaz de usuario. Los usuarios pueden navegar a elementos, caracteres, palabras, líneas, párrafos, vínculos, encabezados, tablas, puntos de referencia y sugerencias. La navegación de teclado estándar expone estos objetos como una lista plana, lo que puede hacer que la navegación sea tediosa a menos que proporcione teclas de método abreviado.

#### <a name="case-study--autosuggestbox-control"></a>Caso práctico: control AutoSuggestBox

El botón buscar de `AutoSuggestBox` no es accesible para la navegación de teclado estándar mediante las teclas de tabulación y de flecha porque el usuario puede presionar la tecla **entrar** para enviar la consulta de búsqueda. Sin embargo, es accesible a través del narrador cuando el usuario presiona Bloq Mayús + una tecla de dirección.

![enfoque de teclado de sugerencia automática](images/keyboard/auto-suggest-keyboard.png)

*Con el teclado, los usuarios presionan el*  * **Escriba** _ _Key para enviar una consulta de búsqueda *

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-1.png" alt="autosuggest narrator focus"/></p>
      <p><em>Con narrador, los usuarios presionan la tecla <strong>entrar</strong> para enviar una consulta de búsqueda</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/auto-suggest-narrator-2.png" alt="autosuggest narrator focus on search"/></p>
      <p><em>Con el narrador, los usuarios también pueden tener acceso al botón buscar mediante la <strong>tecla Bloq Mayús + Flecha derecha</strong>y, después, presionar la tecla <strong>barra espaciadora</strong> .</em></p>
    </td>
  </tr>
</table>

### <a name="keyboard-and-the-xbox-gamepad-and-remote-control"></a>Teclado y el mando a distancia y control remoto de Xbox

Los controladores de juegos y controles remotos de Xbox admiten muchos comportamientos y experiencias de teclado de UWP. Sin embargo, debido a la falta de varias opciones de clave disponibles en un teclado, el controlador para juegos y el control remoto carecen de varias optimizaciones de teclado (el control remoto es aún más limitado que el controlador de juegos).

Consulte interacciones del controlador para juegos [y control remoto](gamepad-and-remote-interactions.md) para obtener más información sobre la compatibilidad de UWP con el controlador de juegos y la entrada de control remoto.

A continuación se muestran algunas asignaciones de teclas entre el teclado, el controlador de juegos y el control remoto.

| **Teclado**  | **Controlador para juegos**                         | **Control remoto**  |
|---------------|-------------------------------------|---------------------|
| Space         | Botón A                            | Botón seleccionar       |
| Escriba         | Botón A                            | Botón seleccionar       |
| Escape        | Botón B                            | Botón Atrás         |
| Inicio o fin      | N/D                                 | N/D                 |
| RE PÁG/AV pág  | Botón desencadenador para desplazamiento vertical, botón de golpe para desplazamiento horizontal   | N/D                 |

Algunas de las principales diferencias que debe tener en cuenta al diseñar la aplicación para UWP para su uso con el controlador de juegos y el uso del control remoto son:
-   La entrada de texto requiere que el usuario presione para activar un control de texto.
-   La navegación de foco no se limita a los grupos de control; los usuarios pueden navegar libremente a cualquier elemento de la interfaz de usuario que tenga el foco en la aplicación.

    **Nota:** El foco puede moverse a cualquier elemento de la interfaz de usuario que tenga el foco en la dirección de la pulsación de teclas, a menos que esté en una interfaz de usuario de superposición o se especifique el [foco](gamepad-and-remote-interactions.md#focus-engagement) , lo que evita que el foco entre o salga de una región hasta que se haya comprometido/desactivado con el botón a. Para obtener más información, vea la sección [navegación direccional](#directional-navigation) .
-   Los botones del panel D y del stick izquierdo se usan para cambiar el foco entre los controles y la navegación interna.

    **Nota:** El controlador de juegos y el control remoto solo navegan a los elementos que se encuentran en el mismo orden visual que la tecla direccional presionada. La navegación se deshabilita en esa dirección cuando no hay ningún elemento posterior que pueda recibir el foco. Dependiendo de la situación, los usuarios del teclado no siempre tienen esa restricción. Para obtener más información, consulte la sección sobre la [optimización de teclado integrada](#built-in-keyboard-optimization) .

#### <a name="directional-navigation"></a>Navegación direccional

La navegación direccional se administra mediante una clase auxiliar de [Administrador de foco](/uwp/api/Windows.UI.Xaml.Input.FocusManager) de UWP, que toma la tecla direccional presionada (tecla de dirección, almohadilla D) e intenta cambiar el foco en la dirección visual correspondiente.

A diferencia del teclado, cuando una aplicación opta por el [modo de mouse](gamepad-and-remote-interactions.md#mouse-mode), la navegación direccional se aplica en toda la aplicación para el controlador de juegos y el control remoto. Consulte [interacciones de control remoto y controlador para juegos](gamepad-and-remote-interactions.md) para más información sobre la optimización de navegación direccional.

**Nota:** La navegación con la tecla de tabulación del teclado no se considera una navegación direccional. Para obtener más información, vea la sección de [tabulaciones](#tab-stops) .

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/directional-navigation.png" alt="directional navigation"/></p>
      <p><em><strong>Navegación direccional compatible</strong></br>Con las teclas de dirección (flechas de teclado, controlador para juegos y panel de control remoto), el usuario puede navegar entre distintos controles.</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/no-directional-navigation.png" alt="no directional navigation"/></p>
      <p><em><strong>No se admite la navegación direccional</strong> </br>El usuario no puede navegar entre distintos controles mediante claves direccionales. Otros métodos de navegación entre los controles (tecla TAB) no se ven afectados.</em></p>
    </td>
  </tr>
</table>

### <a name="built-in-keyboard-optimization"></a>Optimización de teclado integrada

En función del diseño y los controles usados, las aplicaciones UWP se pueden optimizar específicamente para la entrada de teclado.

En el ejemplo siguiente se muestra un grupo de elementos de lista, elementos de cuadrícula y elementos de menú que se han asignado a una sola tabulación ( [vea la sección de tabulaciones](#tab-stops) ). Cuando el grupo tiene el foco, la navegación interna se realiza con las teclas de dirección en el orden visual correspondiente (vea la sección [navegación](#navigation) ).

![navegación por la tecla de flecha de una sola columna](images/keyboard/single-column-arrow.png)

**_Navegación por la tecla de dirección de una sola columna_*

![navegación por la tecla de flecha de una sola fila](images/keyboard/single-row-arrow.png)

_*_Navegación por la tecla de flecha de una sola fila_*_

![navegación por varias claves de flecha de filas y columnas](images/keyboard/multiple-column-and-row-navigation.png)

_*_Navegación por varias teclas de flecha de columna o fila_*_

#### <a name="wrapping-homogeneous-list-and-grid-view-items"></a>Ajustar elementos de vista de cuadrícula y lista homogénea

La navegación direccional no es siempre la manera más eficaz de navegar por varias filas y columnas de elementos de lista y de GridView.

_ *Nota* * los elementos de menú suelen ser listas de una sola columna, pero se pueden aplicar reglas de foco especiales en algunos casos (vea la [interfaz de usuario emergente](#popup-ui)).

Los objetos de lista y cuadrícula se pueden crear con varias filas y columnas. Normalmente se encuentran en la fila principal (donde los elementos rellenan primero la fila antes de rellenar la fila siguiente) o la columna principal (donde los elementos rellenan la columna por completo antes de rellenar el orden de la columna siguiente). El orden principal de fila o columna depende de la dirección de desplazamiento y debe asegurarse de que el orden de los elementos no entra en conflicto con esta dirección.

En el orden principal de las filas (en el que los elementos se rellenan de izquierda a derecha, de arriba abajo), cuando el foco está en el último elemento de una fila y se presiona la tecla de dirección derecha, el foco se mueve al primer elemento de la fila siguiente. Este mismo comportamiento se produce en orden inverso: cuando el foco se establece en el primer elemento de una fila y se presiona la tecla de dirección izquierda, el foco se mueve al último elemento de la fila anterior.

En el orden principal de las columnas (donde los elementos se rellenan de arriba abajo, de izquierda a derecha), cuando el foco está en el último elemento de una columna y el usuario presiona la tecla de dirección abajo, el foco se mueve al primer elemento de la columna siguiente. Este mismo comportamiento se produce en orden inverso: cuando el foco se establece en el primer elemento de una columna y se presiona la tecla de dirección arriba, el foco se mueve al último elemento de la columna anterior.

<table>
  <tr>
    <td>
      <p><img src="images/keyboard/row-major-keyboard.png" alt="row major keyboard navigation"/></p>
      <p><em>Navegación de teclado principal de filas</em></p>
    </td>
    <td>
      <p><img src="images/keyboard/column-major-keyboard.png" alt="column major keyboard navigation"/></p>
      <p><em>Navegación de teclado principal de columna</em></p>
    </td>
  </tr>
</table>

#### <a name="popup-ui"></a>Interfaz de usuario emergente

Como se mencionó, debe intentar asegurarse de que la navegación direccional se corresponda con el orden visual de los controles en la interfaz de usuario de la aplicación.

Algunos controles (por ejemplo, el menú contextual, el menú de desbordamiento de CommandBar y el menú de sugerencia automática) muestran un menú emergente en una ubicación y dirección (hacia abajo de forma predeterminada) con respecto al control principal y al espacio disponible de la pantalla. Tenga en cuenta que la dirección de apertura puede verse afectada por diversos factores en tiempo de ejecución.

<table>
  <td><img src="images/keyboard/command-bar-open-down.png" alt="command bar opens down with down arrow key" /></td>
  <td><img src="images/keyboard/command-bar-open-up.png" alt="command bar opens up with down arrow key" /></td>
</table>

Para estos controles, cuando se abre el menú por primera vez (y el usuario no ha seleccionado ningún elemento), la tecla de flecha abajo establece siempre el foco en el primer elemento, mientras que la tecla de dirección arriba establece siempre el foco en el último elemento del menú. 

Si el último elemento tiene el foco y se presiona la tecla de dirección abajo, el foco se desplaza al primer elemento del menú. Del mismo modo, si el primer elemento tiene el foco y se presiona la tecla de dirección arriba, el foco se desplaza al último elemento del menú. Este comportamiento se conoce como *ciclo* y es útil para navegar por los menús emergentes que se pueden abrir en direcciones imprevisibles.

> [!NOTE]
> Los ciclos deben evitarse en las interfaces de usuario no emergentes en las que los usuarios puedan estar atraídos en un bucle sin fin. 

Se recomienda emular estos mismos comportamientos en los controles personalizados. El ejemplo de código sobre cómo implementar este comportamiento se puede encontrar en la documentación de [navegación del foco mediante programación](focus-navigation-programmatic.md#find-the-first-and-last-focusable-element) .


## <a name="test-your-app"></a>Prueba de la aplicación

Pruebe la aplicación con todos los dispositivos de entrada admitidos para asegurarse de que los elementos de la interfaz de usuario se pueden navegar de forma coherente e intuitiva y de que ningún elemento inesperado interfiere con el orden de tabulación deseado.

## <a name="related-articles"></a>Artículos relacionados

* [Eventos de teclado](keyboard-events.md)
* [Identificación de dispositivos de entrada](identify-input-devices.md)
* [Responder a la presencia del teclado táctil](respond-to-the-presence-of-the-touch-keyboard.md)
* [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
* [Detalles del teclado del control NavigationView](../controls-and-patterns/navigationview.md#hierarchical-navigation)
* [Accesibilidad de teclado](../accessibility/keyboard-accessibility.md)

## <a name="appendix"></a>Apéndice

### <a name="software-keyboard"></a>Teclado de software

Teclado de software es un teclado que se muestra en pantalla y que el usuario puede usar en lugar del teclado físico para escribir y escribir datos mediante la función táctil, el mouse, el lápiz o el lápiz, o cualquier otro dispositivo señalador (no se requiere una pantalla táctil). En la pantalla táctil, estos teclados se pueden tocar directamente para escribir texto. En dispositivos de Xbox One, es necesario seleccionar claves individuales moviendo el foco visual o usando teclas de método abreviado con el control remoto o el controlador de juegos.

![Teclado táctil de Windows 10](images/keyboard/default.png)

***Teclado táctil de Windows 10**

![Teclado Xbox One en pantalla](images/keyboard/xbox-onscreen-keyboard.png)

_*_Teclado Xbox One en pantalla_*_

Dependiendo del dispositivo, el teclado del software aparece cuando un campo de texto u otro control de texto modificable recibe el foco, o cuando el usuario lo habilita manualmente a través del centro de notificaciones * *:

![Icono del teclado táctil en el centro de notificaciones](images/keyboard/touch-keyboard-notificationcenter.png)

Si la aplicación establece el foco mediante programación en un control de entrada de texto, no se invoca el teclado táctil. Esto elimina comportamientos inesperados no originados directamente por el usuario. Sin embargo, el teclado se oculta automáticamente cuando el foco se mueve mediante programación a un control de entrada que no es de texto.

El teclado táctil normalmente permanece visible mientras el usuario navega por los controles de un formulario. Este comportamiento puede variar en función de los otros tipos de control dentro del formulario.

La siguiente es una lista de controles de no edición que pueden recibir el foco durante una sesión de entrada de texto con el teclado táctil sin por ello descartar el teclado. En lugar de renovar innecesariamente la interfaz de usuario y, posiblemente, desorientar al usuario, el teclado táctil permanece visible porque es probable que el usuario alterne entre estos controles y la entrada de texto con el teclado táctil.

-   Casilla de verificación
-   Cuadro combinado
-   Botón de selección
-   Barra de desplazamiento
-   Árbol
-   Elemento de árbol
-   Menú
-   Barra de menús
-   Elemento de menú
-   Barra de herramientas
-   List
-   Elemento de lista

Estos son algunos ejemplos de los diferentes modos del teclado táctil. La primera imagen es el diseño predeterminado, la segunda es el diseño para pulgares (puede que no esté disponible para todos los idiomas).

![teclado táctil en el modo de diseño predeterminado](images/keyboard/default.png)

**_Teclado táctil en el modo de diseño predeterminado_* _

![teclado táctil en el modo de diseño expandido](images/keyboard/extendedview.png)

_*_Teclado táctil en el modo de diseño expandido_*_

La correcta interacción con el teclado permite a los usuarios emplear escenarios de aplicación básicos mediante el uso exclusivo del teclado. Es decir, que los usuarios pueden alcanzar todos los elementos interactivos de la interfaz de usuario y activar funciones predeterminadas. Hay una serie de factores que pueden afectar el grado de éxito, como la navegación por el teclado, las teclas de acceso para accesibilidad y las teclas de aceleración (o de método abreviado) para usuarios avanzados.

_ *Note* * el teclado táctil no admite la alternancia y la mayoría de los comandos del sistema.

#### <a name="on-screen-keyboard"></a>Teclado en pantalla
Al igual que el teclado de software, el teclado en pantalla es un visual, teclado de software que puede usar en lugar del teclado físico para escribir y escribir datos mediante la función táctil, Mouse, pluma/lápiz u otro dispositivo señalador (no se requiere una pantalla táctil). El teclado en pantalla se proporciona para sistemas que no incluyen un teclado físico, o para usuarios cuyos problemas de movilidad les impidan usar los dispositivos de entrada físicos tradicionales. El teclado en pantalla simula la mayoría de las funciones, si no todas, de un teclado de hardware.

Puede activarse desde la página Teclado, que se encuentra en Configuración &gt; Accesibilidad.

**Nota:** El teclado en pantalla tiene prioridad sobre el teclado táctil, que no se mostrará si el teclado en pantalla está presente.

![Teclado en pantalla](images/keyboard/osk.png)

**_Teclado en pantalla_**

Visite [la página teclado en pantalla](https://support.microsoft.com/help/10762/windows-use-on-screen-keyboard) para obtener más detalles sobre el teclado en pantalla.
