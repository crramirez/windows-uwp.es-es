---
Description: Si tu aplicación no proporciona un buen acceso de teclado, los usuarios invidentes o con problemas de motricidad pueden llegar a tener dificultades para usar tu aplicación o, probablemente, no puedan usarla.
title: Accesibilidad de teclado
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
label: Keyboard accessibility
template: detail.hbs
---

Accesibilidad de teclado
=================================================================================

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Si tu aplicación no proporciona un buen acceso de teclado, los usuarios invidentes o con problemas de motricidad pueden llegar a tener dificultades para usar tu aplicación o, probablemente, no puedan usarla.

<span id="keyboard_navigation_among_UI_elements"></span><span id="keyboard_navigation_among_ui_elements"></span><span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"></span>Navegación de teclado entre elementos de la interfaz de usuario
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Para usar el teclado con un control, el control debe tener el foco y, para recibirlo (sin usar un puntero), debe ser accesible en un diseño de interfaz de usuario a través de navegación mediante tabulación. De manera predeterminada, el orden de tabulación de los controles es el mismo que el orden en el que se agregaron a una superficie de diseño, enumerada en XAML, o que se agregó mediante programación a un contenedor.

En la mayoría de los casos, el orden predeterminado basado en cómo definiste los controles en XAML es el mejor orden, especialmente porque es el orden en el que los lectores de pantalla leen los controles. No obstante, el orden predeterminado no necesariamente corresponde al orden visual. La posición de visualización real probablemente dependa del contenedor de diseño primario y ciertas propiedades que puedes establecer en los elementos secundarios para influir en el diseño. Para asegurarte de que la aplicación tiene un buen orden de tabulación, prueba este comportamiento tú mismo. Especialmente si tienes una metáfora de cuadrícula o de tabla para el diseño, el orden de la tabla podría ser distinto al orden en que leerían los usuarios. Esto no siempre es un problema. Pero asegúrate de probar la funcionalidad de tu aplicación como una interfaz de usuario táctil y como una interfaz de usuario con teclado, y comprueba que la interfaz de usuario tiene sentido con ambos métodos.

Puedes hacer que el orden de tabulación coincida con el orden visual ajustando el código XAML. O bien, puedes invalidar el orden de tabulación predeterminado configurando la propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461), como se muestra en el siguiente ejemplo de un diseño [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) que usa la navegación mediante tabulación por columnas primero.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;!--Custom tab order.--&gt; 
&lt;Grid&gt;
  &lt;Grid.RowDefinitions&gt;...&lt;/Grid.RowDefinitions&gt;
  &lt;Grid.ColumnDefinitions&gt;...&lt;/Grid.ColumnDefinitions&gt;

  &lt;TextBlock Grid.Column=&quot;1&quot; HorizontalAlignment=&quot;Center&quot;&gt;Groom&lt;/TextBlock&gt;
  &lt;TextBlock Grid.Column=&quot;2&quot; HorizontalAlignment=&quot;Center&quot;&gt;Bride&lt;/TextBlock&gt;

  &lt;TextBlock Grid.Row=&quot;1&quot;&gt;First name&lt;/TextBlock&gt;
  &lt;TextBox x:Name=&quot;GroomFirstName&quot; Grid.Row=&quot;1&quot; Grid.Column=&quot;1&quot; TabIndex=&quot;1&quot;/&gt;
  &lt;TextBox x:Name=&quot;BrideFirstName&quot; Grid.Row=&quot;1&quot; Grid.Column=&quot;2&quot; TabIndex=&quot;3&quot;/&gt;

  &lt;TextBlock Grid.Row=&quot;2&quot;&gt;Last name&lt;/TextBlock&gt;
  &lt;TextBox x:Name=&quot;GroomLastName&quot; Grid.Row=&quot;2&quot; Grid.Column=&quot;1&quot; TabIndex=&quot;2&quot;/&gt;
  &lt;TextBox x:Name=&quot;BrideLastName&quot; Grid.Row=&quot;2&quot; Grid.Column=&quot;2&quot; TabIndex=&quot;4&quot;/&gt;
&lt;/Grid&gt;</code></pre></td>
</tr>
</tbody>
</table>

Es posible que quieras excluir un control del orden de tabulación. Por lo general, esto se logra convirtiendo el control a no interactivo, por ejemplo, configurando la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419) en **false** Un control deshabilitado se excluye automáticamente del orden de tabulación. Pero, en ocasiones, quizá quieras excluir un control del orden de tabulación incluso si no está deshabilitado. En este caso, puedes establecer la propiedad [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) **false**.

Todos los elementos que pueden tener foco suelen estar en el orden de tabulación de manera predeterminada. La excepción es que es posible que ciertos tipos de presentación de texto como [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) tengan foco con el fin de que el portapapeles pueda acceder a ellos para seleccionar texto; sin embargo, no se encuentran en el orden de tabulación porque no se espera que los elementos de texto estático estén en el orden de tabulación. No suelen ser interactivos (pueden invocarse y no requieren entrada de texto, pero no admiten el [patrón de control de texto](https://msdn.microsoft.com/library/windows/desktop/Ee671194) que admite la búsqueda y el ajuste de puntos de selección en el texto). El texto no debe dar a entender que, si se coloca el foco en él, pueda existir la posibilidad de realizar alguna actividad. Las tecnologías de asistencia seguirán detectando los elementos de texto y los lectores de pantalla los leerán en voz alta, pero esto dependerá de otro tipo de técnicas que no tienen que ver con encontrar estos elementos en el orden de tabulación práctico.

Tanto si ajustas los valores [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) como si usas el orden predeterminado, se aplicarán las siguientes reglas:

-   Los elementos de la interfaz de usuario con [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) igual a 0 se añaden al orden de tabulación según el orden de declaración en XAML o colecciones secundarias.
-   Los elementos de la interfaz de usuario con una propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) mayor que 0 se añaden al orden de tabulación según el valor **TabIndex**.
-   Los elementos de la interfaz de usuario con una propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) menor que 0 se añaden al orden de tabulación y aparecen antes de cualquier valor cero. Esto difiere potencialmente de la administración del atributo **tabindex** de HTML (y el **tabindex** negativo no se admitía en especificaciones HTML más antiguas).

<span id="keyboard_navigation_within_a_UI_element"></span><span id="keyboard_navigation_within_a_ui_element"></span><span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"></span>Navegación por teclado dentro de un elemento de interfaz de usuario
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Para los elementos compuestos, es importante asegurar una correcta navegación interna entre los elementos contenidos. Un elemento compuesto puede administrar su elemento secundario activo actual para reducir la sobrecarga que implica hacer que todos los elementos secundarios tengan foco. Un elemento compuesto de este tipo se incluye en el orden de tabulación y controla los eventos de navegación por teclado. Muchos de los controles compuestos ya tienen una lógica de navegación interna integrada para controlar la administración de eventos. Por ejemplo, el recorrido mediante teclas de dirección de los elementos está habilitado de manera predeterminada en los controles [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878), [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242704view), [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) y [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678) .

<span id="keyboard_activation"></span><span id="KEYBOARD_ACTIVATION"></span>Alternativas de teclado para eventos y acciones de puntero para elementos de control específicos
-------------------------------------------------------------------------------------------------------------------------------------------------------------

Asegúrate de que los elementos de la interfaz de usuario en los que pueda hacerse clic también puedan invocarse con el teclado. Para usar el teclado con un elemento de la interfaz de usuario, el elemento debe tener foco. Solo las clases que derivan de [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) admiten navegación mediante tabulación y foco.

Para aquellos elementos de la interfaz de usuario que puedan invocarse, implementa controladores de eventos de teclado para las tecla ENTRAR y la barra espaciadora. Esto hace que la compatibilidad de accesibilidad de teclado básica sea completa y permite a los usuarios lograr escenarios de aplicación básicos mediante el uso del teclado solamente, es decir, los usuarios pueden alcanzar todos los elementos de la interfaz de usuario interactivos y activar la funcionalidad predeterminada.

En los casos en los que un elemento que quieres usar en la interfaz de usuario no pueda tener foco, podrías crear tu propio control personalizado. Debes establecer la propiedad [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) en **true** para habilitar el foco y debes proporcionar una indicación visual del estado enfocado creando un estado visual que decore la interfaz de usuario con un indicador de foco. Sin embargo, suele ser más fácil usar la composición de controles para que el soporte de los modelos de automatización de la interfaz de usuario de Microsoft y automatización del mismo nivel, foco y detenciones de tabulación sea administrado por el control dentro del cual elijas que se componga tu contenido.

Por ejemplo, en lugar de administrar un evento de presión de puntero en una clase [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752), podrías encapsularlo en una clase [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) para obtener compatibilidad para foco, teclado y puntero.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;!--Don&#39;t do this.--&gt;
&lt;Image Source=&quot;sample.jpg&quot; PointerPressed=&quot;Image_PointerPressed&quot;/&gt;

&lt;!--Do this instead.--&gt;
&lt;Button Click=&quot;Button_Click&quot;&gt;&lt;Image Source=&quot;sample.jpg&quot;/&gt;&lt;/Button&gt;</code></pre></td>
</tr>
</tbody>
</table>

<span id="keyboard_shortcuts"></span><span id="KEYBOARD_SHORTCUTS"></span>Métodos abreviados de teclado
--------------------------------------------------------------------------------------------

Además de implementar la navegación por teclado y la activación para tu aplicación, una práctica recomendada es implementar métodos abreviados para la funcionalidad de la aplicación. La navegación mediante tabulación proporciona un buen nivel básico de compatibilidad con teclado, pero con formas complejas, probablemente también quieras agregar compatibilidad para teclas de método abreviado. Esto puede hacer que el uso de tu aplicación sea más eficiente, incluso para aquellos que usan tanto dispositivos de puntero como un teclado.

Un *método abreviado* es una combinación de teclas que mejora la productividad al proporcionar al usuario una forma eficaz de acceder a las funciones de la aplicación. Existen dos tipos de métodos abreviados:

-   Una *tecla de acceso* es un método abreviado a un elemento de la interfaz de usuario de la aplicación. Las teclas de acceso consisten en la tecla Alt y una tecla de letra.
-   Una *tecla de aceleración* es un método abreviado a un comando de aplicación. Tu aplicación podría tener una interfaz de usuario que corresponda exactamente al comando. Las teclas de aceleración consisten en la tecla Ctrl y una tecla de letra.

Es fundamental que proporciones a los usuarios que dependen de los lectores de pantalla y otra tecnología de asistencia un método sencillo para descubrir las teclas de los métodos abreviados de la aplicación. Comunica las teclas de método abreviado mediante la información sobre herramientas, los nombres accesibles, las descripciones accesibles o alguna otra forma de comunicación en pantalla. Como mínimo, las teclas de método abreviado deben estar correctamente documentadas en el contenido de la ayuda de la aplicación.

Puedes documentar las teclas de acceso mediante lectores de pantalla configurando la propiedad adjunta [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) como una cadena que describe la tecla de método abreviado. También existe una propiedad adjunta [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) para documentar las teclas de método abreviado no mnemotécnicas, aunque los lectores de pantalla generalmente tratan ambas propiedades de la misma manera. Intenta documentar las teclas de método abreviado de diferentes maneras, mediante documentación de ayuda escrita, propiedades de automatización e información sobre herramientas.

En el siguiente ejemplo se muestra cómo documentar las teclas de método abreviado para botones de detención, pausa y reproducción de medios.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Grid KeyDown=&quot;Grid_KeyDown&quot;&gt;

  &lt;Grid.RowDefinitions&gt;
    &lt;RowDefinition Height=&quot;Auto&quot; /&gt;
    &lt;RowDefinition Height=&quot;Auto&quot; /&gt;
  &lt;/Grid.RowDefinitions&gt;

  &lt;MediaElement x:Name=&quot;DemoMovie&quot; Source=&quot;xbox.wmv&quot; 
    Width=&quot;500&quot; Height=&quot;500&quot; Margin=&quot;20&quot; HorizontalAlignment=&quot;Center&quot; /&gt;

  &lt;StackPanel Grid.Row=&quot;1&quot; Margin=&quot;10&quot;
    Orientation=&quot;Horizontal&quot; HorizontalAlignment=&quot;Center&quot;&gt;

    &lt;Button x:Name=&quot;PlayButton&quot; Click=&quot;MediaButton_Click&quot;
      ToolTipService.ToolTip=&quot;Shortcut key: Ctrl+P&quot;
      AutomationProperties.AcceleratorKey=&quot;Control P&quot;&gt;
      &lt;TextBlock&gt;Play&lt;/TextBlock&gt;
    &lt;/Button&gt;

    &lt;Button x:Name=&quot;PauseButton&quot; Click=&quot;MediaButton_Click&quot;
      ToolTipService.ToolTip=&quot;Shortcut key: Ctrl+A&quot; 
      AutomationProperties.AcceleratorKey=&quot;Control A&quot;&gt;
      &lt;TextBlock&gt;Pause&lt;/TextBlock&gt;
    &lt;/Button&gt;

    &lt;Button x:Name=&quot;StopButton&quot; Click=&quot;MediaButton_Click&quot;
      ToolTipService.ToolTip=&quot;Shortcut key: Ctrl+S&quot; 
      AutomationProperties.AcceleratorKey=&quot;Control S&quot;&gt;
      &lt;TextBlock&gt;Stop&lt;/TextBlock&gt;
    &lt;/Button&gt;

  &lt;/StackPanel&gt;

&lt;/Grid&gt;</code></pre></td>
</tr>
</tbody>
</table>

**Importante**  Establecer [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) o [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) no habilita la funcionalidad de teclado. Únicamente notifica al marco de automatización de la interfaz de usuario qué teclas se deben usar para poder pasar esta información a los usuarios mediante las tecnologías de asistencia. La implementación de la administración de teclas debe hacerse en el código, no en XAML. Deberás adjuntar controladores para los eventos [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941) o [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942) en el control correspondiente para implementar realmente el comportamiento de los métodos abreviados de teclado en tu aplicación. Además, el detalle de texto subrayado en una tecla de acceso no se proporciona de manera automática. Si quieres mostrar texto subrayado en la interfaz de usuario, debes subrayar explícitamente el texto de la tecla de acceso específica como formato [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982) en línea.

 

Por cuestiones de simplicidad, el ejemplo anterior omite el uso de recursos para cadenas como Ctrl+A. No obstante, también debes tener en cuenta las teclas de método abreviado durante la localización. La localización de las teclas de método abreviado es relevante porque la elección de la tecla que se usa como método abreviado suele depender de la etiqueta de texto visible del elemento.

Para obtener más información sobre la implementación de las teclas de método abreviado, consulta [Teclas de método abreviado](http://go.microsoft.com/fwlink/p/?linkid=221825) en las Directrices para la interacción de la experiencia de usuarios de Windows.

### <span id="Implementing_a_key_event_handler"></span><span id="implementing_a_key_event_handler"></span><span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"></span>Implementar un controlador de eventos de tecla

Los eventos de entrada como los eventos de tecla usan un concepto de evento denominado *eventos enrutados*. Un evento enrutado se puede propagar por los elementos secundarios de un control compuesto, lo que permite a un control primario controlar los eventos de varios elementos secundarios. Este modelo de evento es conveniente para definir acciones de teclas de método abreviado en un control que contenga varias partes compuestas que no estén diseñadas para admitir foco ni para formar parte del orden de tabulación.

Para obtener código de ejemplo que muestra cómo escribir un controlador de eventos de tecla que incluya la comprobación de modificadores como la tecla Ctrl, consulta [Interacciones de teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607).

<span id="Keyboard_navigation_for_custom_controls"></span><span id="keyboard_navigation_for_custom_controls"></span><span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"></span>Navegación mediante teclado para controles personalizados
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Se recomienda el uso de teclas de dirección como métodos abreviados de teclado para navegar entre elementos secundarios, en los casos en que dichos elementos mantengan una relación espacial entre sí. Si los nodos de la vista de árbol tienen sub-elementos secundarios separados para administrar la activación de nodos y las acciones de expandir/contraer, usa las teclas de flecha izquierda y derecha para proporcionar la funcionalidad de expandir/contraer del teclado. Si tienes un control orientado que admite el recorrido direccional dentro del contenido del control, usa las teclas de dirección adecuadas.

Por lo general, para implementar un control de teclas personalizado para controles personalizados se incluye una invalidación de los métodos [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/BR209390_onkeydown) y [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/BR209390_onkeyup) como parte de la lógica de la clase.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"></span><span id="an_example_of_a_visual_state_for_a_focus_indicator"></span><span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"></span>Ejemplo de un estado visual para un indicador de foco
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Ya hemos mencionado que los controles personalizados que el usuario habilite para que tengan el foco deben tener un indicador de foco visual. Normalmente, ese indicador de foco es tan simple como dibujar un rectángulo justo alrededor del rectángulo de límite normal del control. La clase [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) del foco visual es un elemento del mismo nivel que el resto de la composición del control en una plantilla de control, pero se establece inicialmente con un valor [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) en **Collapsed** porque el control aún no tiene el foco. Después, cuando el control obtiene el foco, se invoca un estado visual que establece específicamente el valor de **Visibility** en **Visible**. Cuando el foco se mueve a otro lugar, se llama a otro estado visual y **Visibility** pasa a ser **Collapsed**.

Todos los controles XAML predeterminados mostrarán un indicador de foco visual adecuado cuando reciban el foco (si es que pueden recibirlo). También hay distintos aspectos potenciales según el tema seleccionado por el usuario (en especial si el usuario usa un modo de contraste alto). Si usas los controles XAML en la interfaz de usuario y no reemplazas las plantillas de control, no tienes que hacer nada más para obtener indicadores de foco visual en los controles que se comportan y se muestran correctamente. Si lo que intentas es volver a crear la plantilla de un control, o si tienes curiosidad sobre cómo los controles XAML proporcionan sus indicadores de foco visual, en el resto de esta sección se explica cómo se logra en XAML y la lógica de control.

Aquí incluimos un código XAML de ejemplo que procede de la plantilla XAML predeterminada de una clase [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) .

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;ControlTemplate TargetType=&quot;Button&quot;&gt;
... 
    &lt;Rectangle 
      x:Name=&quot;FocusVisualWhite&quot; 
      IsHitTestVisible=&quot;False&quot; 
      Stroke=&quot;{ThemeResource FocusVisualWhiteStrokeThemeBrush}&quot; 
      StrokeEndLineCap=&quot;Square&quot; 
      StrokeDashArray=&quot;1,1&quot; 
      Opacity=&quot;0&quot; 
      StrokeDashOffset=&quot;1.5&quot;/&gt;
    &lt;Rectangle 
      x:Name=&quot;FocusVisualBlack&quot; 
      IsHitTestVisible=&quot;False&quot; 
      Stroke=&quot;{ThemeResource FocusVisualBlackStrokeThemeBrush}&quot; 
      StrokeEndLineCap=&quot;Square&quot; 
      StrokeDashArray=&quot;1,1&quot; 
      Opacity=&quot;0&quot; 
      StrokeDashOffset=&quot;0.5&quot;/&gt;
...
&lt;/ControlTemplate&gt;</code></pre></td>
</tr>
</tbody>
</table>

De momento, esto es solo la composición. Para controlar la visibilidad del indicador de foco, tienes que definir los estados visuales que alternan la propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992). Para hacerlo, usa la propiedad [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505) adjunta, tal y como se aplica al elemento raíz que define la composición

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;ControlTemplate TargetType=&quot;Button&quot;&gt;
  &lt;Grid&gt;
    &lt;VisualStateManager.VisualStateGroups&gt; 
       &lt;!--other visual state groups here--&gt;
       &lt;VisualStateGroup x:Name=&quot;FocusStates&quot;&gt; 
         &lt;VisualState x:Name=&quot;Focused&quot;&gt; 
           &lt;Storyboard&gt; 
             &lt;DoubleAnimation 
               Storyboard.TargetName=&quot;FocusVisualWhite&quot; 
               Storyboard.TargetProperty=&quot;Opacity&quot; 
               To=&quot;1&quot; Duration=&quot;0&quot;/&gt;
             &lt;DoubleAnimation 
               Storyboard.TargetName=&quot;FocusVisualBlack&quot; 
               Storyboard.TargetProperty=&quot;Opacity&quot; 
               To=&quot;1&quot; Duration=&quot;0&quot;/&gt;
         &lt;/VisualState&gt; 
         &lt;VisualState x:Name=&quot;Unfocused&quot; /&gt; 
         &lt;VisualState x:Name=&quot;PointerFocused&quot; /&gt; 
       &lt;/VisualStateGroup&gt;
     &lt;VisualStateManager.VisualStateGroups&gt;
&lt;!--composition is here--&gt;
   &lt;/Grid&gt;
&lt;/ControlTemplate&gt;</code></pre></td>
</tr>
</tbody>
</table>

Ten en cuenta que solo uno de los estados con nombre ajusta la propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) directamente mientras que los demás están aparentemente vacíos. El funcionamiento de los estados visuales es que en cuanto el control usa otro estado del mismo [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014), se cancelan inmediatamente las animaciones aplicadas por el estado anterior. Debido a que el valor predeterminado de **Visibility** de la composición es **Collapsed**, el rectángulo no aparecerá. Esto se controla con la lógica de control, mediante la escucha de eventos de foco como [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927) y la modificación de los estados a [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025). A menudo esto se realiza automáticamente si usas un control predeterminado o personalizado, basado en un control que ya tenga ese comportamiento.

<span id="_Keyboard_accessibility_and_Windows_Phone"></span><span id="_keyboard_accessibility_and_windows_phone"></span><span id="_KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"></span> Accesibilidad de teclado y Windows Phone
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Por lo general, un dispositivo de Windows Phone carece de un teclado de hardware dedicado. Sin embargo, un panel de entrada de software (SIP) puede dar cabida a diversos escenarios de accesibilidad de teclado. Los lectores de pantalla pueden leer entradas de texto desde el SIP de **Texto** e incluso advertir de posibles eliminaciones. Los usuarios podrán saber dónde están sus dedos porque el lector de pantalla es capaz de detectar que el usuario está examinando las teclas, de modo que lee en voz alta el nombre de la tecla examinada. De igual modo, algunos de los conceptos de accesibilidad relativos al teclado se pueden asignar a determinados comportamientos de tecnología de asistencia en los que no se usa el teclado en absoluto. Por ejemplo, incluso si un SIP no incluye una tecla TAB, el Narrador admite un gesto táctil que equivale a presionar dicha tecla, de modo que disponer de un orden de tabulación útil de los controles de una interfaz de usuario sigue siendo un principio de accesibilidad de gran importancia. Las teclas de dirección para navegar por los elementos de controles complejos también se pueden usar como gestos táctiles en el Narrador. Cuando el foco llega a un control que no está destinado a la entrada de texto, Narrador admite un gesto con el que se invoca la acción de dicho control.

Los métodos abreviados de teclado no suelen ser relevantes para las aplicaciones Windows Phone, puesto que un SIP carece de teclas Control o ALT.

<span id="related_topics"></span>Temas relacionados
-----------------------------------------------

* [Accesibilidad](accessibility.md)
* [Interacciones de teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [Entrada: muestra de teclado táctil](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Muestra de respuesta a la apariencia del teclado en pantalla](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [Muestra de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 





<!--HONumber=Mar16_HO3-->


