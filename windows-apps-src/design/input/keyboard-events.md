---
description: Responde a las acciones de pulsación de tecla desde teclados de hardware o de software en tus aplicaciones, usando para ello controladores de eventos tanto de clase como de teclado.
title: Eventos de teclado
ms.assetid: ac500772-d6ed-4a3a-825b-210a9c3c8f59
label: Keyboard events
template: detail.hbs
keywords: teclado, controlador para juegos, remoto, accesibilidad, navegación, enfoque, texto, entrada, interacciones del usuario, tecla arriba, tecla abajo
ms.date: 03/29/2017
ms.topic: article
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: efd8a2bb205974efdcf13d614cb6fa7848f96dc7
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033698"
---
# <a name="keyboard-events"></a>Eventos de teclado

## <a name="keyboard-events-and-focus"></a>Eventos de teclado y foco

Los siguientes eventos de teclado se pueden producir en teclados tanto de hardware como táctiles.

| Evento                                      | Descripción                    |
|--------------------------------------------|--------------------------------|
| [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keydown) | Se produce cuando se presiona una tecla.  |
| [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)     | Se produce cuando se suelta una tecla. |

> [!IMPORTANT]
> Algunos controles de Windows Runtime controlan los eventos de entrada de manera interna. en cuyo caso puede dar la impresión de que un evento de entrada no se produce, dado que la escucha de eventos no invoca al controlador asociado correspondiente. Normalmente, este subconjunto de teclas se procesa mediante el controlador de eventos de clase para aportar compatibilidad integrada de accesibilidad de teclado básico. Por ejemplo, la clase [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) invalida los eventos [**OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown) tanto de la barra espaciadora como de la tecla ENTRAR (así como [**OnPointerPressed**](/uwp/api/windows.ui.xaml.controls.control.onpointerpressed)) y los enruta al evento [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) del control. Cuando una presión de tecla se controla mediante la clase de control, no se generan los eventos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup).  
> Esto proporciona un teclado integrado que equivale a invocar el botón, como si se presionara con un dedo o se hiciera clic en él con un mouse. Las teclas que no son la barra espaciadora ni ENTRAR sí siguen desencadenando eventos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup). Para obtener más información sobre cómo funciona el control basado en clases de los eventos (concretamente, la sección "controladores de eventos de entrada en controles"), consulte [Introducción a eventos y eventos enrutados](../../xaml-platform/events-and-routed-events-overview.md).


Los controles de la interfaz de usuario generan eventos de teclado solo cuando tienen el foco de entrada. Un control individual obtiene el foco cuando el usuario hace clic o presiona directamente sobre dicho control en el diseño o bien usa la tecla TAB para entrar en una secuencia de tabulación dentro del área de contenido.

También puedes llamar al método [**Focus**](/uwp/api/windows.ui.xaml.controls.control.focus) de un control para forzar el foco. Esto solo es necesario si implementas teclas de método abreviado, porque el foco del teclado no se establece de manera predeterminada cuando se carga la interfaz de usuario. Si deseas obtener más información, consulta el **ejemplo de teclas de método abreviado** más adelante en este tema.

Para que un control reciba el foco de entrada, debe estar habilitado y sus propiedades [**IsTabStop**](/uwp/api/windows.ui.xaml.controls.control.istabstop) y [**HitTestVisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible) deben tener un valor **true** . Este es el estado predeterminado para la mayoría de los controles. Cuando un control tiene el foco de entrada, puede generar eventos de entrada del teclado y responder a ellos, tal y como se describe más adelante en este capítulo. También puedes responder a un control que recibe o pierde el enfoque al controlar los eventos [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) y [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus).

De manera predeterminada, la secuencia de tabulación de los controles es el orden con que aparecen en el lenguaje XAML. Sin embargo, puedes modificar este orden mediante la propiedad [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex). Para obtener más información, consulte [implementar la accesibilidad del teclado](/previous-versions/windows/apps/hh868161(v=win.10)).

## <a name="keyboard-event-handlers"></a>Controladores de eventos de teclado


Un controlador de eventos de entrada implementa un delegado que proporciona la siguiente información:

-   Remitente del evento. El remitente notifica el objeto al que se adjunta el controlador de eventos.
-   Datos de evento Para los eventos de teclado, esos datos serán una instancia de [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs). El delegado de los controladores es [**KeyEventHandler**](/uwp/api/windows.ui.xaml.input.keyeventhandler). Las propiedades más importantes de **KeyRoutedEventArgs** para la mayoría de los escenarios de controladores son [**Key**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) y posiblemente [**KeyStatus**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus).
-   [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource). Dado que los eventos de teclado están enrutados, los datos del evento proporcionan el objeto **OriginalSource** . Si estás permitiendo deliberadamente que los eventos se propaguen por un árbol de objetos, a veces **OriginalSource** es el objeto de interés en lugar del remitente. Sin embargo, eso depende de tu diseño. Si deseas obtener más información sobre cómo podrías usar **OriginalSource** en lugar del remitente, consulta la sección "Eventos de teclado enrutados" de este tema o bien el tema [Introducción a eventos y eventos enrutados](../../xaml-platform/events-and-routed-events-overview.md).

### <a name="attaching-a-keyboard-event-handler"></a>Adjuntar un controlador de eventos de teclado

Puedes adjuntar funciones de controlador de eventos de teclado a cualquier objeto que incluya el evento como miembro. Esto engloba cualquier clase derivada de [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement). El siguiente ejemplo en XAML muestra cómo adjuntar controladores para el evento [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) de una clase [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid).

```xaml
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

También puedes adjuntar un controlador de eventos mediante código. Para obtener más información, consulta [Introducción a eventos y eventos enrutados](../../xaml-platform/events-and-routed-events-overview.md).

### <a name="defining-a-keyboard-event-handler"></a>Definición de un controlador de eventos de teclado

El siguiente ejemplo muestra la definición incompleta de un controlador de eventos para el controlador de eventos [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) que se adjuntó en el ejemplo anterior.

```csharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```vb
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    ' handling code here
End Sub
```

```c++
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
  {
      //handling code here
  }
```

### <a name="using-keyroutedeventargs"></a>Uso de KeyRoutedEventArgs

Todos los eventos de teclado usan [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) para datos de eventos. **KeyRoutedEventArgs** contiene las siguientes propiedades:

-   [**Clave**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key)
-   [**KeyStatus**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.keystatus)
-   [**Controlado**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)
-   [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) (heredado de [**RoutedEventArgs**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs))

### <a name="key"></a>Clave

El evento [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) se genera si se presiona una tecla. De igual modo, [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) se genera si se libera una tecla. Por lo general, los eventos se escuchan para procesar un valor de tecla específico. Para determinar qué tecla se presiona o se libera, comprueba el valor [**Key**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.key) en los datos del evento. La propiedad **Key** devuelve un valor [**VirtualKey**](/uwp/api/Windows.System.VirtualKey). La enumeración **VirtualKey** incluye todas las teclas compatibles.

### <a name="modifier-keys"></a>Teclas modificadoras

Las teclas modificadoras son teclas como Ctrl o Mayús que los usuarios suelen presionar en combinación con otras teclas. Tu aplicación puede usar estas combinaciones como métodos abreviados de teclado para invocar comandos de la aplicación.

Puede detectar combinaciones de teclas de método abreviado en los controladores de eventos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) . Cuando se produce un evento de teclado para una tecla no modificadora, puede comprobar si una tecla modificadora está en estado presionado.

Como alternativa, la función [**GetKeyState ()**](/uwp/api/windows.ui.core.corewindow.getkeystate) de [**corewindow**](/uwp/api/windows.ui.core.corewindow) (obtenida a través de [**corewindow. GetForCurrentThread ()**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)) también se puede usar para comprobar el estado del modificador cuando se presiona una tecla no modificadora.

En los siguientes ejemplos se implementa este segundo método, mientras que también se incluye código auxiliar para la primera implementación.

> [!NOTE]
> La tecla Alt está representada por el valor **VirtualKey.Menu** .

### <a name="shortcut-keys-example"></a>Ejemplo de teclas de método abreviado

En el siguiente ejemplo se muestra cómo implementar teclas de método abreviado. En este ejemplo, los usuarios pueden controlar la reproducción multimedia con los botones Reproducir, Pausa y Detener, o bien con las teclas de método abreviado Ctrl+P, Ctrl+A y Ctrl+S. El XAML de los botones muestra los métodos abreviados mediante las propiedades [**AutomationProperties**](/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) e información sobre herramientas en las etiquetas de los botones. Esta información automática es importante para mejorar la facilidad de uso y de acceso de tu aplicación. Para obtener más información, vea [accesibilidad del teclado](../accessibility/keyboard-accessibility.md).

Ten en cuenta que la página establece el foco de entrada en sí misma cuando se carga. Sin este paso, ninguno de los controles tiene el enfoque de entrada inicial y la aplicación no genera eventos de entrada hasta que el usuario establece el enfoque manualmente (por ejemplo, mediante tabulación o haciendo clic en un control).

```xaml
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

```c++
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) 
{
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


bool KeyboardSupport::MainPage::IsCtrlKeyPressed()
{
    auto ctrlState = CoreWindow::GetForCurrentThread()->GetKeyState(VirtualKey::Control);
    return (ctrlState & CoreVirtualKeyStates::Down) == CoreVirtualKeyStates::Down;
}

void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (IsCtrlKeyPressed()) 
    {
        if (e->Key==VirtualKey::P) { DemoMovie->Play(); }
        if (e->Key==VirtualKey::A) { DemoMovie->Pause(); }
        if (e->Key==VirtualKey::S) { DemoMovie->Stop(); }
    }
}
```

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}

private static bool IsCtrlKeyPressed()
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (IsCtrlKeyPressed())
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Function IsCtrlKeyPressed As Boolean
    Dim ctrlState As CoreVirtualKeyStates = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    Return (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down;
End Function

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If IsCtrlKeyPressed() Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

> [!NOTE]
> Establecer [**AutomationProperties.AcceleratorKey**](/dotnet/api/system.windows.automation.automationproperties.acceleratorkey) o [**AutomationProperties.AccessKey**](/dotnet/api/system.windows.automation.automationproperties.accesskey) en XAML proporciona información sobre la cadena, lo que documenta la tecla de método abreviado para invocar esa acción en particular. Los clientes de automatización de la interfaz de usuario de Microsoft (como, por ejemplo, Narrador) capturan esta información que, por lo general, se entrega directamente al usuario.
>
> Establecer **AutomationProperties.AcceleratorKey** o **AutomationProperties.AccessKey** no genera ninguna acción. Deberás adjuntar controladores para los eventos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) o [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) para implementar el comportamiento de método abreviado de teclado en tu aplicación. Además, el detalle de texto subrayado en una tecla de acceso no se proporciona de manera automática. Si quieres mostrar texto subrayado en la interfaz de usuario, debes subrayar explícitamente el texto de la tecla de acceso específica como formato [**Underline**](/uwp/api/Windows.UI.Xaml.Documents.Underline) en línea.

 

## <a name="keyboard-routed-events"></a>Eventos de teclado enrutados


Ciertos eventos son eventos enrutados, entre ellos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup). Los eventos enrutados usan la estrategia de enrutamiento por propagación. La estrategia de enrutamiento por propagación significa que un evento se origina en un objeto secundario y después se enruta hacia los objetos primarios sucesivos del árbol de objetos. Esto da otra oportunidad para controlar el mismo evento e interactuar con los mismos datos de evento.

Observa el siguiente ejemplo de XAML, que controla eventos [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) para una clase [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) y dos objetos [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button). En este caso, si liberas una tecla mientras el foco está en cualquiera de los objetos **Button** , se genera el evento **KeyUp** . Después, el evento se propaga hasta el **lienzo** primario.

```xaml
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

El siguiente ejemplo muestra cómo implementar el controlador de eventos [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) para el contenido en XAML correspondiente al ejemplo anterior.

```csharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

Observa el uso de la propiedad [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource) en el controlador precedente. Aquí **OriginalSource** notifica el objeto que generó el evento. El objeto no pudo ser [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) porque **StackPanel** no es un control y no puede tener el foco. Solo uno de los dos botones dentro de **StackPanel** podría haber generado el evento, pero ¿cuál? Usa **OriginalSource** para distinguir el objeto origen real del evento, si estás controlando el evento en un objeto primario.

### <a name="the-handled-property-in-event-data"></a>La propiedad Handled en los datos de evento

En función de la estrategia de control de eventos que implementes, puede ser que desees que solo un controlador de eventos reaccione ante un evento de propagación. Por ejemplo, si tienes un controlador [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) específico adjunto a uno de los controles [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button), tendría la primera oportunidad de controlar dicho evento. En este caso, quizás no quieras que el panel primario controle también el evento. Para este escenario, puedes usar la propiedad [**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) en los datos del evento.

El propósito de la propiedad [**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) en una clase de datos de evento enrutado es notificar que otro controlador que registraste antes en la ruta del evento ya ha actuado. Esto influye en el comportamiento del sistema de eventos enrutados. Cuando estableces **Handled** en **true** en un controlador de eventos, ese evento deja de enrutarse y no se envía a los elementos primarios sucesivos.

### <a name="addhandler-and-already-handled-keyboard-events"></a>AddHandler y eventos de teclado ya controlados

Puedes usar una técnica especial para adjuntar controladores que pueden actuar sobre eventos que ya están marcados como controlados. Esta técnica usa el método [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) para registrar un controlador, en lugar de usar atributos XAML o la sintaxis específica del lenguaje para agregar controladores, como + = en C \# .

Una limitación general de esta técnica es que la API **AddHandler** toma un parámetro de tipo [**RoutedEvent**](/uwp/api/Windows.UI.Xaml.RoutedEvent) que identifica el evento enrutado en cuestión. No todos los eventos enrutados proporcionan un identificador **RoutedEvent** . Por lo tanto, esta consideración, influye en qué eventos enrutados es posible controlar en el caso [**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled). Los eventos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) tienen identificadores de evento enrutado ( [**KeyDownEvent**](/uwp/api/windows.ui.xaml.uielement.keydownevent) y [**KeyUpEvent**](/uwp/api/windows.ui.xaml.uielement.keyupevent)) en [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement). Sin embargo, otros eventos como [**TextBox.TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) no tienen identificadores de evento enrutado y, por lo tanto, no pueden usarse con la técnica **AddHandler** .

### <a name="overriding-keyboard-events-and-behavior"></a>Invalidar comportamientos y eventos de teclado

Puedes invalidar eventos de tecla para controles específicos (como [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView)) para proporcionar navegación con foco homogéneo para diversos dispositivos de entrada, como el teclado y el controlador para juegos.

En el ejemplo siguiente, se crea una subclase del control y se invalida el comportamiento de KeyDown para cambiar el foco al contenido de GridView cuando se presiona cualquier tecla de dirección.

```csharp
  public class CustomGridView : GridView
  {
    protected override void OnKeyDown(KeyRoutedEventArgs e)
    {
      // Override arrow key behaviors.
      if (e.Key != Windows.System.VirtualKey.Left && e.Key !=
        Windows.System.VirtualKey.Right && e.Key !=
          Windows.System.VirtualKey.Down && e.Key !=
            Windows.System.VirtualKey.Up)
              base.OnKeyDown(e);
      else
        FocusManager.TryMoveFocus(FocusNavigationDirection.Down);
    }
  }
```

> [!NOTE]
> Si utilizas GridView solo para diseño, considera la posibilidad de usar otros controles, como [**ItemsControl**](/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) con [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid).

## <a name="commanding"></a>Comandos

Algunos elementos de la interfaz de usuario admiten comandos. Los comandos usan eventos enrutados relacionados con la entrada en su implementación subyacente. Permiten procesar la entrada de la interfaz de usuario relacionada (como una acción de puntero determinada o una tecla aceleradora específica) mediante la invocación de un único controlador de comandos.

Si hay comandos disponibles para un elemento de interfaz de usuario, te recomendamos que uses las API de comandos en lugar de eventos de entrada discretos. Para obtener más información, consulta [**ButtonBase.Command**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

También puedes implementar [**ICommand**](/uwp/api/Windows.UI.Xaml.Input.ICommand) para encapsular la funcionalidad de los comandos que invocas desde controladores de eventos comunes. Esto permite usar los comandos aunque no haya ninguna propiedad **Command** disponible.

## <a name="text-input-and-controls"></a>Controles y entrada de texto

Ciertos controles reaccionan ante los eventos de teclado con su propio control. Por ejemplo, [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) es un control diseñado para capturar y representar visualmente el texto que se especificó con el teclado. Usa [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) y [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) en su propia lógica para capturar las pulsaciones y después también genera su propio evento [**TextChanged**](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) si el texto ha cambiado.

Por lo general, puedes agregar controladores para [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) y [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) a una clase [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) o cualquier control relacionado que tenga por finalidad procesar entrada de texto. Sin embargo, puede ocurrir que, como parte de su diseño intencional, un control no responda a todos los valores de teclas que se dirijan a él a través de eventos de tecla. El comportamiento es específico de cada control.

A modo de ejemplo, [**ButtonBase**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) (la clase base para [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)) procesa [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup) para poder buscar la barra espaciadora o la tecla ENTRAR. La clase **ButtonBase** considera que el evento **KeyUp** equivale al botón primario del mouse presionado con el propósito de generar un evento [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Este procesamiento del evento se logra cuando **ButtonBase** invalida el método virtual [**OnKeyUp**](/uwp/api/windows.ui.xaml.controls.control.onkeyup). En su implementación, establece [**Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) en **true** . El resultado es que cualquier elemento primario de un botón que escucha un evento de tecla, en el caso de una barra espaciadora, no recibiría el evento ya controlado para sus propios controladores.

Otro ejemplo es [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox). Algunas claves, como las teclas de dirección, no se consideran texto por **cuadro** de texto y se consideran específicas del comportamiento de la interfaz de usuario del control. **TextBox** marca estos casos de eventos como controlados.

Los controles personalizados pueden implementar su propio comportamiento de invalidación similar para eventos clave mediante la invalidación de [**onkeydown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown)  /  [**onkeyup**](/uwp/api/windows.ui.xaml.controls.control.onkeyup). Si tu control personalizado procesa teclas aceleradoras específicas o tiene un comportamiento de control o foco similar al escenario que describimos en el caso de la clase [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox), puedes incluir esta lógica en tus propias invalidaciones de **OnKeyDown** / **OnKeyUp** .

## <a name="the-touch-keyboard"></a>Teclado táctil

Los controles de entrada de texto proporcionan compatibilidad automática para el teclado táctil. Cuando el usuario establece el enfoque de entrada en un control de texto mediante entrada táctil, el teclado táctil aparece automáticamente. Cuando el enfoque de entrada no está en un control de texto, el teclado táctil se oculta.

Cuando el teclado táctil aparece, recoloca automáticamente la interfaz de usuario para asegurar que el elemento con foco permanezca visible. Esto puede hacer que otras áreas importantes de la interfaz de usuario queden fuera de la pantalla. Sin embargo, puedes deshabilitar el comportamiento predeterminado y realizar tus propios ajustes en la interfaz de usuario cuando el teclado táctil aparezca. Para obtener más información, consulte el [ejemplo de teclado táctil](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

Si creas un control personalizado que necesite una entrada de texto, pero que no derive de un control de entrada de texto estándar, puedes agregar la compatibilidad con el teclado táctil si implementas los modelos de control adecuados para la automatización de la interfaz de usuario. Para obtener más información, consulte el [ejemplo de teclado táctil](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard).

Al presionar las teclas en el teclado táctil se generan los eventos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup), igual que cuando se presionan las teclas de un teclado de hardware. Sin embargo, el teclado táctil no genera eventos de entrada para Ctrl+A, Ctrl+Z, Ctrl+X, Ctrl+C y Ctrl+V, que están reservadas para la manipulación de texto en el control de entrada.

Para que los usuarios puedan escribir datos en la aplicación de forma mucho más rápida y sencilla, establece el ámbito de entrada del control de texto para que coincida con el tipo de datos que esperas que escriba el usuario. El ámbito de entrada proporciona una sugerencia sobre el tipo de entrada de texto que espera el control para que el sistema pueda proporcionar una distribución del teclado táctil especializada para el tipo de entrada. Por ejemplo, si un cuadro de texto solo se usa para escribir un PIN de 4 dígitos, establezca la propiedad [**InputScope**](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) en [**Number**](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue). Esto indica al sistema que debe mostrar el diseño de teclado numérico, lo cual facilita al usuario la inserción del PIN. Para obtener más información, consulta [Usar el ámbito de entrada para cambiar el teclado táctil](./use-input-scope-to-change-the-touch-keyboard.md).

## <a name="related-articles"></a>Artículos relacionados

### <a name="developers"></a>Desarrolladores

- [Interacciones de teclado](keyboard-interactions.md)
- [Identificación de dispositivos de entrada](identify-input-devices.md)
- [Responder a la presencia del teclado táctil](respond-to-the-presence-of-the-touch-keyboard.md)

### <a name="designers"></a>Diseñadores

- [Directrices para el diseño de teclado](./keyboard-interactions.md)

### <a name="samples"></a>Ejemplos

- [Ejemplo de teclado táctil](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Muestras de archivo

- [Ejemplo de entrada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: muestra de teclado táctil](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Touch%20keyboard%20sample%20(Windows%208))
- [Muestra de respuesta a la apariencia del teclado en pantalla](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [Ejemplo de edición de texto XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
