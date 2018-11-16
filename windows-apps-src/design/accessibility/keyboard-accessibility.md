---
author: Xansky
Description: If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: Accesibilidad de teclado
label: Keyboard accessibility
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 41c5e018ee56b6a0d26bf2159f62801aa4ab5c3c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6984659"
---
# <a name="keyboard-accessibility"></a>Accesibilidad de teclado  



Si tu aplicación no proporciona un buen acceso de teclado, los usuarios invidentes o con problemas de motricidad pueden llegar a tener dificultades para usar tu aplicación o, probablemente, no puedan usarla.

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>

## <a name="keyboard-navigation-among-ui-elements"></a>Navegación de teclado entre elementos de la interfaz de usuario  
Para usar el teclado con un control, el control debe tener el foco y, para recibirlo (sin usar un puntero), debe ser accesible en un diseño de interfaz de usuario a través de navegación mediante tabulación. De manera predeterminada, el orden de tabulación de los controles es el mismo que el orden en el que se agregaron a una superficie de diseño, enumerada en XAML, o que se agregó mediante programación a un contenedor.

En la mayoría de los casos, el orden predeterminado basado en cómo definiste los controles en XAML es el mejor orden, especialmente porque es el orden en el que los lectores de pantalla leen los controles. No obstante, el orden predeterminado no necesariamente corresponde al orden visual. La posición de visualización real probablemente dependa del contenedor de diseño primario y ciertas propiedades que puedes establecer en los elementos secundarios para influir en el diseño. Para asegurarte de que la aplicación tiene un buen orden de tabulación, prueba este comportamiento tú mismo. Especialmente si tienes una metáfora de cuadrícula o de tabla para el diseño, el orden de la tabla podría ser distinto al orden en que leerían los usuarios. Esto no siempre es un problema. Pero asegúrate de probar la funcionalidad de tu aplicación como una interfaz de usuario táctil y como una interfaz de usuario con teclado, y comprueba que la interfaz de usuario tiene sentido con ambos métodos.

Puedes hacer que el orden de tabulación coincida con el orden visual ajustando el código XAML. O bien, puedes invalidar el orden de tabulación predeterminado configurando la propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461), como se muestra en el siguiente ejemplo de un diseño [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) que usa la navegación mediante tabulación por columnas primero.

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

Es posible que quieras excluir un control del orden de tabulación. Por lo general, esto se logra convirtiendo el control a no interactivo, por ejemplo, configurando la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/BR209419) en **false** Un control deshabilitado se excluye automáticamente del orden de tabulación. Pero, en ocasiones, quizá quieras excluir un control del orden de tabulación incluso si no está deshabilitado. En este caso, puedes establecer la propiedad [**IsTabStop **](https://msdn.microsoft.com/library/windows/apps/BR209422)**false**.

Todos los elementos que pueden tener foco suelen estar en el orden de tabulación de manera predeterminada. La excepción es que es posible que ciertos tipos de presentación de texto como [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) tengan foco con el fin de que el portapapeles pueda acceder a ellos para seleccionar texto; sin embargo, no se encuentran en el orden de tabulación porque no se espera que los elementos de texto estático estén en el orden de tabulación. No suelen ser interactivos (pueden invocarse y no requieren entrada de texto, pero no admiten el [patrón de control de texto](https://msdn.microsoft.com/library/windows/desktop/Ee671194) que admite la búsqueda y el ajuste de puntos de selección en el texto). El texto no debe dar a entender que, si se coloca el foco en él, pueda existir la posibilidad de realizar alguna actividad. Las tecnologías de asistencia seguirán detectando los elementos de texto y los lectores de pantalla los leerán en voz alta, pero esto dependerá de otro tipo de técnicas que no tienen que ver con encontrar estos elementos en el orden de tabulación práctico.

Tanto si ajustas los valores [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) como si usas el orden predeterminado, se aplicarán las siguientes reglas:

* Los elementos de la interfaz de usuario con [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) igual a 0 se añaden al orden de tabulación según el orden de declaración en XAML o colecciones secundarias.
* Los elementos de la interfaz de usuario con una propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) mayor que 0 se añaden al orden de tabulación según el valor **TabIndex**.
* Los elementos de la interfaz de usuario con una propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) menor que 0 se añaden al orden de tabulación y aparecen antes de cualquier valor cero. Esto difiere potencialmente de la administración del atributo **tabindex** de HTML (y el **tabindex** negativo no se admitía en especificaciones HTML más antiguas).

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>

## <a name="keyboard-navigation-within-a-ui-element"></a>Navegación por teclado dentro de un elemento de interfaz de usuario  
Para los elementos compuestos, es importante asegurar una correcta navegación interna entre los elementos contenidos. Un elemento compuesto puede administrar su elemento secundario activo actual para reducir la sobrecarga que implica hacer que todos los elementos secundarios tengan foco. Un elemento compuesto de este tipo se incluye en el orden de tabulación y controla los eventos de navegación por teclado. Muchos de los controles compuestos ya tienen una lógica de navegación interna integrada para controlar la administración de eventos. Por ejemplo, el recorrido mediante teclas de dirección de los elementos está habilitado de manera predeterminada en los controles [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878), [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview), [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) y [**FlipView**](https://msdn.microsoft.com/library/windows/apps/BR242678) .

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>

## <a name="keyboard-alternatives-to-pointer-actions-and-events-for-specific-control-elements"></a>Alternativas de teclado para eventos y acciones de puntero para elementos de control específicos  
Asegúrate de que los elementos de la interfaz de usuario en los que pueda hacerse clic también puedan invocarse con el teclado. Para usar el teclado con un elemento de la interfaz de usuario, el elemento debe tener foco. Solo las clases que derivan de [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) admiten navegación mediante tabulación y foco.

Para aquellos elementos de la interfaz de usuario que puedan invocarse, implementa controladores de eventos de teclado para las tecla ENTRAR y la barra espaciadora. Esto hace que la compatibilidad de accesibilidad de teclado básica sea completa y permite a los usuarios lograr escenarios de aplicación básicos mediante el uso del teclado solamente, es decir, los usuarios pueden alcanzar todos los elementos de la interfaz de usuario interactivos y activar la funcionalidad predeterminada.

En los casos en los que un elemento que quieres usar en la interfaz de usuario no pueda tener foco, podrías crear tu propio control personalizado. Debes establecer la propiedad [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/BR209422) en **true** para habilitar el foco y debes proporcionar una indicación visual del estado enfocado creando un estado visual que decore la interfaz de usuario con un indicador de foco. Sin embargo, suele ser más fácil usar la composición de controles para que el soporte de los modelos de automatización de la interfaz de usuario de Microsoft y automatización del mismo nivel, foco y detenciones de tabulación sea administrado por el control dentro del cual elijas que se componga tu contenido.

Por ejemplo, en lugar de administrar un evento de presión de puntero en una clase [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752), podrías encapsularlo en una clase [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) para obtener compatibilidad para foco, teclado y puntero.

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>

## <a name="keyboard-shortcuts"></a>Métodos abreviados de teclado  
Además de implementar la navegación por teclado y la activación para tu aplicación, una práctica recomendada es implementar métodos abreviados para la funcionalidad de la aplicación. La navegación mediante tabulación proporciona un buen nivel básico de compatibilidad con teclado, pero con formas complejas, probablemente también quieras agregar compatibilidad para teclas de método abreviado. Esto puede hacer que el uso de tu aplicación sea más eficiente, incluso para aquellos que usan tanto dispositivos de puntero como un teclado.

Un *método abreviado* es una combinación de teclas que mejora la productividad al proporcionar al usuario una forma eficaz de acceder a las funciones de la aplicación. Existen dos tipos de métodos abreviados:

* Una *tecla de acceso* es un método abreviado a un elemento de la interfaz de usuario de la aplicación. Las teclas de acceso consisten en la tecla Alt y una tecla de letra.
* Una *tecla de aceleración* es un método abreviado a un comando de aplicación. Tu aplicación podría tener una interfaz de usuario que corresponda exactamente al comando. Las teclas de aceleración consisten en la tecla Ctrl y una tecla de letra.

Es fundamental que proporciones a los usuarios que dependen de los lectores de pantalla y otra tecnología de asistencia un método sencillo para descubrir las teclas de los métodos abreviados de la aplicación. Comunica las teclas de método abreviado mediante la información sobre herramientas, los nombres accesibles, las descripciones accesibles o alguna otra forma de comunicación en pantalla. Como mínimo, las teclas de método abreviado deben estar correctamente documentadas en el contenido de la ayuda de la aplicación.

Puedes documentar las teclas de acceso mediante lectores de pantalla configurando la propiedad adjunta [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) como una cadena que describe la tecla de método abreviado. También existe una propiedad adjunta [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) para documentar las teclas de método abreviado no mnemotécnicas, aunque los lectores de pantalla generalmente tratan ambas propiedades de la misma manera. Intenta documentar las teclas de método abreviado de diferentes maneras, mediante documentación de ayuda escrita, propiedades de automatización e información sobre herramientas.

En el siguiente ejemplo se muestra cómo documentar las teclas de método abreviado para botones de detención, pausa y reproducción de medios.

XAML
```xml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>
  </StackPanel>
</Grid>
```

> [!IMPORTANT]
> Establecer [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/Hh759762) o [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/Hh759763) no habilita la funcionalidad de teclado. Únicamente notifica al marco de trabajo de automatización de la interfaz de usuario qué teclas se deben usar para poder pasar esta información a los usuarios mediante las tecnologías de asistencia. La implementación de la administración de teclas debe hacerse en el código, no en XAML. Deberás adjuntar controladores para los eventos [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/BR208941) o [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/BR208942) en el control correspondiente para implementar realmente el comportamiento de los métodos abreviados de teclado en tu aplicación. Además, el detalle de texto subrayado en una tecla de acceso no se proporciona de manera automática. Si quieres mostrar texto subrayado en la interfaz de usuario, debes subrayar explícitamente el texto de la tecla de acceso específica como formato [**Underline**](https://msdn.microsoft.com/library/windows/apps/BR209982) en línea.

Por cuestiones de simplicidad, el ejemplo anterior omite el uso de recursos para cadenas como Ctrl+A. No obstante, también debes tener en cuenta las teclas de método abreviado durante la localización. La localización de las teclas de método abreviado es relevante porque la elección de la tecla que se usa como método abreviado suele depender de la etiqueta de texto visible del elemento.

Para obtener más información sobre la implementación de las teclas de método abreviado, consulta [Teclas de método abreviado](http://go.microsoft.com/fwlink/p/?linkid=221825) en las Directrices para la interacción de la experiencia de usuarios de Windows.

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>

### <a name="implementing-a-key-event-handler"></a>Implementar un controlador de eventos de tecla  
Los eventos de entrada como los eventos de tecla usan un concepto de evento denominado *eventos enrutados*. Un evento enrutado se puede propagar por los elementos secundarios de un control compuesto, lo que permite a un control primario controlar los eventos de varios elementos secundarios. Este modelo de evento es conveniente para definir acciones de teclas de método abreviado en un control que contenga varias partes compuestas que no estén diseñadas para admitir foco ni para formar parte del orden de tabulación.

Para obtener código de ejemplo que muestra cómo escribir un controlador de eventos de tecla que incluya la comprobación de modificadores como la tecla Ctrl, consulta [Interacciones de teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607).

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>

## <a name="keyboard-navigation-for-custom-controls"></a>Navegación mediante teclado para controles personalizados  
Se recomienda el uso de teclas de dirección como métodos abreviados de teclado para navegar entre elementos secundarios, en los casos en que dichos elementos mantengan una relación espacial entre sí. Si los nodos de la vista de árbol tienen sub-elementos secundarios separados para administrar la activación de nodos y las acciones de expandir/contraer, usa las teclas de flecha izquierda y derecha para proporcionar la funcionalidad de expandir/contraer del teclado. Si tienes un control orientado que admite el recorrido direccional dentro del contenido del control, usa las teclas de dirección adecuadas.

Por lo general, para implementar un control de teclas personalizado para controles personalizados se incluye una invalidación de los métodos [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982.aspx) y [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983.aspx) como parte de la lógica de la clase.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>

## <a name="an-example-of-a-visual-state-for-a-focus-indicator"></a>Un ejemplo de un estado visual para un indicador de foco  
Ya hemos mencionado que los controles personalizados que el usuario habilite para que tengan el foco deben tener un indicador de foco visual. Normalmente, ese indicador de foco es tan simple como dibujar un rectángulo justo alrededor del rectángulo de límite normal del control. La clase [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) del foco visual es un elemento del mismo nivel que el resto de la composición del control en una plantilla de control, pero se establece inicialmente con un valor [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) en **Collapsed** porque el control aún no tiene el foco. Después, cuando el control obtiene el foco, se invoca un estado visual que establece específicamente el valor de **Visibility** en **Visible**. Cuando el foco se mueve a otro lugar, se llama a otro estado visual y **Visibility** pasa a ser **Collapsed**.

Todos los controles XAML predeterminados mostrarán un indicador de foco visual adecuado cuando reciban el foco (si es que pueden recibirlo). También hay distintos aspectos potenciales según el tema seleccionado por el usuario (en especial si el usuario usa un modo de contraste alto). Si usas los controles XAML en la interfaz de usuario y no reemplazas las plantillas de control, no tienes que hacer nada más para obtener indicadores de foco visual en los controles que se comportan y se muestran correctamente. Si lo que intentas es volver a crear la plantilla de un control, o si tienes curiosidad sobre cómo los controles XAML proporcionan sus indicadores de foco visual, en el resto de esta sección se explica cómo se logra en XAML y la lógica de control.

Aquí incluimos un código XAML de ejemplo que procede de la plantilla XAML predeterminada de una clase [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) .

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

De momento, esto es solo la composición. Para controlar la visibilidad del indicador de foco, tienes que definir los estados visuales que alternan la propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992). Para hacerlo, usa la propiedad [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/Hh738505) adjunta, tal y como se aplica al elemento raíz que define la composición

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

Ten en cuenta que solo uno de los estados con nombre ajusta la propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) directamente mientras que los demás están aparentemente vacíos. El funcionamiento de los estados visuales es que en cuanto el control usa otro estado del mismo [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/BR209014), se cancelan inmediatamente las animaciones aplicadas por el estado anterior. Debido a que el valor predeterminado de **Visibility** de la composición es **Collapsed**, el rectángulo no aparecerá. Esto se controla con la lógica de control, mediante la escucha de eventos de foco como [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/BR208927) y la modificación de los estados a [**GoToState**](https://msdn.microsoft.com/library/windows/apps/BR209025). A menudo esto se realiza automáticamente si usas un control predeterminado o personalizado, basado en un control que ya tenga ese comportamiento.

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>

## <a name="keyboard-accessibility-and-windows-phone"></a>Accesibilidad de teclado y WindowsPhone
Por lo general, un dispositivo de WindowsPhone carece de un teclado de hardware dedicado. Sin embargo, un panel de entrada de software (SIP) puede dar cabida a diversos escenarios de accesibilidad de teclado. Los lectores de pantalla pueden leer entradas de texto desde el SIP de **Texto** e incluso advertir de posibles eliminaciones. Los usuarios podrán saber dónde están sus dedos porque el lector de pantalla es capaz de detectar que el usuario está examinando las teclas, de modo que lee en voz alta el nombre de la tecla examinada. De igual modo, algunos de los conceptos de accesibilidad relativos al teclado se pueden asignar a determinados comportamientos de tecnología de asistencia en los que no se usa el teclado en absoluto. Por ejemplo, incluso si un SIP no incluye una tecla TAB, el Narrador admite un gesto táctil que equivale a presionar dicha tecla, de modo que disponer de un orden de tabulación útil de los controles de una interfaz de usuario sigue siendo un principio de accesibilidad de gran importancia. Las teclas de dirección para navegar por los elementos de controles complejos también se pueden usar como gestos táctiles en el Narrador. Cuando el foco llega a un control que no está destinado a la entrada de texto, Narrador admite un gesto con el que se invoca la acción de dicho control.

Los métodos abreviados de teclado no suelen ser relevantes para las aplicaciones Windows Phone, puesto que un SIP carece de teclas Control o ALT.

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  
* [Accesibilidad](accessibility.md)
* [Interacciones de teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607)
* [Muestra de teclado táctil](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [Muestra de accesibilidad en XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)

