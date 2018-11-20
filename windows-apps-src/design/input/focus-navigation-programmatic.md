---
author: Karl-Bridge-Microsoft
Description: Learn how to programmatically manage focus navigation with keyboard, gamepad, and accessibility tools in a UWP app.
title: Navegación con foco mediante programación con teclado, controlador para juegos y herramientas de accesibilidad
label: Programmatic focus navigation
keywords: teclado, controlador para juegos, control remoto, navegación, estrategia de navegación, entrada, interacción del usuario, accesibilidad, facilidad de uso
ms.author: kbridge
ms.date: 03/19/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d2317b419a2679d13e846690bbaca0eb212a245e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7432021"
---
# <a name="programmatic-focus-navigation"></a>Navegación con foco mediante programación

![Teclado, control remoto y pad-D](images/dpad-remote/dpad-remote-keyboard.png)

Para mover el foco mediante programación en tu aplicación para UWP, puedes usar el método [FocusManager.TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) o el método [FocusManager.FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_).

[TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) intenta cambiar el foco del elemento con foco al siguiente elemento activable en la dirección especificada, mientras que [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) recupera el elemento (como [DependencyObject](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject)) que recibirá el foco basado en la dirección especificada de navegación (solo la navegación direccional, no se puede usar para emular la navegación mediante tabulación).

> [!NOTE]
> Recomendamos usar el método [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) en lugar de [FindNextFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) porque FindNextFocusableElement recupera un UIElement, que devuelve null si el siguiente elemento activable no es un UIElement (como un objeto Hyperlink). 

## <a name="find-a-focus-candidate-within-a-scope"></a>Buscar un candidato de foco en un ámbito

Puedes personalizar el comportamiento de navegación con foco para [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) y [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_), incluyendo la búsqueda del siguiente candidato de foco dentro de un árbol específico de la interfaz de usuario o no teniendo en cuenta elementos específicos.

En este ejemplo se usa un juego TicTacToe para demostrar los métodos [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) y [FindNextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_).

```xaml
<StackPanel Orientation="Horizontal"
                VerticalAlignment="Center"
                HorizontalAlignment="Center" >
    <Button Content="Start Game" />
    <Button Content="Undo Movement" />
    <Grid x:Name="TicTacToeGrid" KeyDown="OnKeyDown">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
            <ColumnDefinition Width="50" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="0" 
            x:Name="Cell00" />
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="0" 
            x:Name="Cell10"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="0" 
            x:Name="Cell20"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="1" 
            x:Name="Cell01"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="1" 
            x:Name="Cell11"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="1" 
            x:Name="Cell21"/>
        <myControls:TicTacToeCell 
            Grid.Column="0" Grid.Row="2" 
            x:Name="Cell02"/>
        <myControls:TicTacToeCell 
            Grid.Column="1" Grid.Row="2" 
            x:Name="Cell22"/>
        <myControls:TicTacToeCell 
            Grid.Column="2" Grid.Row="2" 
            x:Name="Cell32"/>
    </Grid>
</StackPanel>
```

```csharp
private void OnKeyDown(object sender, KeyRoutedEventArgs e)
{
    DependencyObject candidate = null;

    var options = new FindNextElementOptions ()
    {
        SearchRoot = TicTacToeGrid,
        NavigationStrategy = NavigationStrategyMode.Heuristic
    };

    switch (e.Key)
    {
        case Windows.System.VirtualKey.Up:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Up, options);
            break;
        case Windows.System.VirtualKey.Down:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Down, options);
            break;
        case Windows.System.VirtualKey.Left:
            candidate = FocusManager.FindNextElement(
                FocusNavigationDirection.Left, options);
            break;
        case Windows.System.VirtualKey.Right:
            candidate = 
                FocusManager.FindNextElement(
                    FocusNavigationDirection.Right, options);
            break;
    }
    // Also consider whether candidate is a Hyperlink, WebView, or TextBlock.
    if (candidate != null && candidate is Control)
    {
        (candidate as Control).Focus(FocusState.Keyboard);
    }
}
```

Usa [FindNextElementOptions](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.findnextelementoptions) para personalizar más el modo en que se identifican los candidatos de foco. Este objeto proporciona las siguientes propiedades:

- [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot): define el ámbito de la búsqueda de candidatos de navegación con foco para los elementos secundarios de esta clase DependencyObject. Null indica que se inicie la búsqueda desde la raíz del árbol visual.

> [!Important] 
> Si se aplican una o varias transformaciones a los descendientes de la clase **SearchRoot** que los colocan fuera del área direccional, estos elementos aún se consideran candidatos.

- [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect): los candidatos de navegación con foco se identifican con un rectángulo delimitador "ficticio" en el que todos los objetos superpuestos se excluyen del foco de navegación. Este rectángulo se usa solo para cálculos y nunca se agrega al árbol visual.
- [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect): los candidatos de navegación con foco se identifican con un rectángulo delimitador "ficticio" que identifica los elementos con más probabilidades de recibir el foco. Este rectángulo se usa solo para cálculos y nunca se agrega al árbol visual.
- [XYFocusNavigationStrategyOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride): la estrategia de navegación con foco empleada a fin de identificar el mejor elemento candidato para recibir el foco.

En la siguiente imagen se ilustran algunos de estos conceptos. 

Cuando el elemento B tiene el foco, FindNextElement identifica I como el candidato de foco al ir a la derecha. A continuación se muestran los motivos de esto:
- Debido a la propiedad [HintRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) en A, la referencia de inicio es A, no B
- C no es candidato porque MyPanel se ha especificado como [SearchRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)
- F no es candidato porque [ExclusionRect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) se solapa con él

![Personaliza el comportamiento de navegación con foco mediante sugerencias de navegación](images/keyboard/navigation-hints.png)

*Personaliza el comportamiento de navegación con foco mediante sugerencias de navegación*

## <a name="navigation-focus-events"></a>Eventos de foco de navegación

### <a name="nofocuscandidatefound-event"></a>Evento NoFocusCandidateFound

El evento [UIElement.NoFocusCandidateFound](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) se desencadena cuando se presionan las teclas Tabulador o de dirección y no hay ningún candidato de foco en la dirección especificada. Este evento no se desencadena para [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Dado que este es un evento enrutado, se propaga del elemento enfocado a través de los objetos primarios sucesivos a la raíz del árbol de objetos. Esto te permite controlar el evento siempre que sea apropiado.

<a name="split-view-code-sample"></a>

Aquí te mostramos cómo una cuadrícula abre [SplitView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.splitview) cuando el usuario intenta mover el foco a la izquierda del control activable situado más a la izquierda (consulta [Diseño para Xbox y televisión](../devices/designing-for-tv.md#navigation-pane)).

```xaml
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">
...
</Grid>
```

```csharp
private void OnNoFocusCandidateFound (
    UIElement sender, NoFocusCandidateFoundEventArgs args)
{
    if(args.NavigationDirection == FocusNavigationDirection.Left)
    {
        if(args.InputDevice == FocusInputDeviceKind.Keyboard ||
        args.InputDevice == FocusInputDeviceKind.GameController )
            {
                OpenSplitPaneView();
            }
        args.Handled = true;
    }
}
```

### <a name="gotfocus-and-lostfocus-events"></a>Eventos GotFocus y LostFocus
Los eventos [UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) y [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) se desencadenan cuando un elemento obtiene el foco o pierde el foco, respectivamente. Este evento no se desencadena para [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Dado que estos son eventos enrutados, se propagan del elemento enfocado a través de los objetos primarios sucesivos a la raíz del árbol de objetos. Esto te permite controlar el evento siempre que sea apropiado.

### <a name="gettingfocus-and-losingfocus-events"></a>Eventos GettingFocus y LosingFocus

Los eventos [UIElement.GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y [UIElement.LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) se activan antes de invocar a los eventos [UIElement.GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) y [UIElement.LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus). 

Dado que estos son eventos enrutados, se propagan del elemento enfocado a través de los objetos primarios sucesivos a la raíz del árbol de objetos. Como esto sucede antes de producirse un cambio de foco, puedes redirigir o cancelar el cambio de foco.

[GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) son eventos sincrónicos, por lo que el foco no se moverá mientras se propaguen estos eventos. Sin embargo, [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) y [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) son eventos asincrónicos, lo que significa que no hay ninguna garantía de que el foco no se vuelva a mover antes de que se ejecute el controlador.

Si el foco de desplaza por una llamada a [Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_), [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) se genera durante la llamada, mientras que [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) se genera después de la llamada.

El destino de navegación con foco se puede cambiar durante los eventos [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) (antes de que el foco se desplace) por la propiedad [GettingFocusEventArgs.NewFocusedElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement). Incluso si cambia el destino, el evento aún se propaga y el destino puede cambiar de nuevo.

Para evitar problemas de reentrada, se lanza una excepción si se intenta mover el foco (con [TryMoveFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) o [Control.Focus](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)) mientras se propagan estos eventos.

Estos eventos se desencadenan independientemente del motivo del desplazamiento del foco (incluidas la navegación mediante tabulación, la navegación direccional y la navegación mediante programación).

Este es el orden de ejecución de los eventos de foco:

1.  [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) Si el foco se restablece nuevamente en el elemento que perdió el foco o [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) es correcto, no se desencadenarán más eventos.
2.  [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) Si el foco se restablece nuevamente en el elemento que perdió el foco o [TryCancel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) es correcto, no se desencadenarán más eventos.
3.  [LostFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

La siguiente imagen muestra cómo, al pasar de A a la derecha, XYFocus elige B4 como candidato. Luego B4 desencadena el evento GettingFocus, donde ListView tiene la oportunidad de volver a asignar el foco a B3.

![Cambio del destino de navegación con foco en el evento GettingFocus](images/keyboard/focus-events.png)

*Cambio del destino de navegación con foco en el evento GettingFocus*

Aquí te mostraremos cómo controlar el evento [GettingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y redirigir el foco.

```XAML
<StackPanel Orientation="Horizontal">
    <Button Content="A" />
    <ListView x:Name="MyListView" SelectedIndex="2" GettingFocus="OnGettingFocus">
        <ListViewItem>LV1</ListViewItem>
        <ListViewItem>LV2</ListViewItem>
        <ListViewItem>LV3</ListViewItem>
        <ListViewItem>LV4</ListViewItem>
        <ListViewItem>LV5</ListViewItem>
    </ListView>
</StackPanel>
```

```csharp
private void OnGettingFocus(UIElement sender, GettingFocusEventArgs args)
{
    //Redirect the focus only when the focus comes from outside of the ListView.
    // move the focus to the selected item
    if (MyListView.SelectedIndex != -1 && 
        IsNotAChildOf(MyListView, args.OldFocusedElement))
    {
        var selectedContainer = 
            MyListView.ContainerFromItem(MyListView.SelectedItem);
        if (args.FocusState == 
            FocusState.Keyboard && 
            args.NewFocusedElement != selectedContainer)
        {
            args.TryRedirect(
                MyListView.ContainerFromItem(MyListView.SelectedItem));
            args.Handled = true;
        }
    }
}
```

Aquí te mostraremos cómo controlar el evento [LosingFocus](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) para [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) y establecer el foco al cerrarse el menú.

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
     <AppBarButton Icon="Back" Label="Back" />
     <AppBarButton Icon="Stop" Label="Stop" />
     <AppBarButton Icon="Play" Label="Play" />
     <AppBarButton Icon="Forward" Label="Forward" />

     <CommandBar.SecondaryCommands>
         <AppBarButton Icon="Like" Label="Like" />
         <AppBarButton Icon="Share" Label="Share" />
     </CommandBar.SecondaryCommands>
 </CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (MyCommandBar.IsOpen == true && 
        IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
    {
        if (args.TryCancel())
        {
            args.Handled = true;
        }
    }
}
```

## <a name="find-the-first-and-last-focusable-element"></a>Buscar el primer y el último elemento activable

Los métodos [FocusManager.FindFirstFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) y [FocusManager.FindLastFocusableElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) mueven el foco al primer o último elemento activable dentro del ámbito de un objeto (el árbol de elementos de [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) o el árbol de texto de [TextElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.documents.textelement)). El ámbito se especifica en la llamada (si el argumento es null, el ámbito es la ventana actual)

Si no puede identificarse ningún candidato de foco en el ámbito, se devolverá null.

Aquí mostramos cómo especificar que los botones de un control CommandBar tienen comportamiento direccional de ajuste automático (consulta [Interacciones de teclado](keyboard-interactions.md#popup-ui)).

```XAML
<CommandBar x:Name="MyCommandBar" LosingFocus="OnLosingFocus">
    <AppBarButton Icon="Back" Label="Back" />
    <AppBarButton Icon="Stop" Label="Stop" />
    <AppBarButton Icon="Play" Label="Play" />
    <AppBarButton Icon="Forward" Label="Forward" />

    <CommandBar.SecondaryCommands>
        <AppBarButton Icon="Like" Label="Like" />
        <AppBarButton Icon="ReShare" Label="Share" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

```csharp
private void OnLosingFocus(UIElement sender, LosingFocusEventArgs args)
{
    if (IsNotAChildOf(MyCommandBar, args.NewFocussedElement))
    {
        DependencyObject candidate = null;
        if (args.Direction == FocusNavigationDirection.Left)
        {
            candidate = FocusManager.FindLastFocusableElement(MyCommandBar);
        }
        else if (args.Direction == FocusNavigationDirection.Right)
        {
            candidate = FocusManager.FindFirstFocusableElement(MyCommandBar);
        }
        if (candidate != null)
        {
            args.NewFocusedElement = candidate;
            args.Handled = true;
        }
    }
}
```

## <a name="related-articles"></a>Artículos relacionados

- [Navegación con foco para teclado, controlador para juegos, control remoto y herramientas de accesibilidad](focus-navigation.md)
- [Interacciones de teclado](keyboard-interactions.md)
- [Accesibilidad de teclado](../accessibility/keyboard-accessibility.md)