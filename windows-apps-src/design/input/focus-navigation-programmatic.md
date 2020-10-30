---
description: Obtenga información sobre cómo administrar mediante programación la navegación con el foco con el teclado, el controlador de juegos y las herramientas de accesibilidad en una aplicación de Windows.
title: Navegación con foco mediante programación con teclado, controlador para juegos y herramientas de accesibilidad
label: Programmatic focus navigation
keywords: teclado, dispositivo de juego, control remoto, navegación, estrategia de navegación, entrada, interacción del usuario, accesibilidad, facilidad de uso
ms.date: 09/24/2020
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b64eeafda6abe146d05e29d491850da3b36a1999
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032178"
---
# <a name="programmatic-focus-navigation"></a>Navegación con foco mediante programación

![Teclado, remoto y PAD D](images/dpad-remote/dpad-remote-keyboard.png)

Para desplazar el foco mediante programación en la aplicación Windows, puede usar el método [FocusManager. TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) o el método [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) .

[TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) intenta cambiar el foco del elemento que tiene el foco al siguiente elemento enfocable en la dirección especificada, mientras que [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) recupera el elemento (como [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject)) que recibirá el foco en función de la dirección de navegación especificada (solo navegación direccional, no se puede usar para emular la navegación por tabulación).

> [!NOTE]
> Se recomienda usar el método [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) en lugar de [FindNextFocusableElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextFocusableElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) porque FindNextFocusableElement recupera un objeto UIElement, que devuelve NULL si el siguiente elemento enfocable no es un elemento UIElement (como un objeto Hyperlink). 

## <a name="find-a-focus-candidate-within-a-scope"></a>Buscar un candidato de foco dentro de un ámbito

Puede personalizar el comportamiento de navegación de foco para [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) y [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_), lo que incluye la búsqueda del siguiente candidato de enfoque en un árbol de interfaz de usuario específico o la exclusión de elementos específicos de la consideración.

En este ejemplo se usa un juego de TicTacToe para mostrar los métodos [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) y [FindNextElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindNextElement_Windows_UI_Xaml_Input_FocusNavigationDirection_) .

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
        XYFocusNavigationStrategyOverride = XYFocusNavigationStrategyOverride.Projection
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

Use [FindNextElementOptions](/uwp/api/windows.ui.xaml.input.findnextelementoptions) para personalizar aún más el modo en que se identifican los candidatos. Este objeto proporciona las siguientes propiedades:

- [SearchRoot](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot) : el ámbito de la búsqueda de candidatos para la navegación del foco a los elementos secundarios de este DependencyObject. Null indica que se va a iniciar la búsqueda desde la raíz del árbol visual.

> [!Important] 
> Si se aplican una o varias transformaciones a los descendientes de **SearchRoot** que las colocan fuera del área direccional, estos elementos se consideran candidatos.

- Los candidatos de navegación [ExclusionRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) se identifican mediante un rectángulo delimitador "ficticio" donde todos los objetos superpuestos se excluyen del foco de navegación. Este rectángulo solo se usa para los cálculos y nunca se agrega al árbol visual.
- Los candidatos de navegación [HintRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) se identifican mediante un rectángulo delimitador "ficticio" que identifica los elementos que es más probable que reciban el foco. Este rectángulo solo se usa para los cálculos y nunca se agrega al árbol visual.
- [XYFocusNavigationStrategyOverride](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_XYFocusNavigationStrategyOverride) : estrategia de navegación de foco que se usa para identificar el mejor elemento candidato para recibir el foco.

En la imagen siguiente se muestran algunos de estos conceptos. 

Cuando el elemento B tiene el foco, FindNextElement identifica I como candidato de foco al navegar a la derecha. Las razones para esto son las siguientes:
- Debido a la [HintRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_HintRect) en un, la referencia inicial es A, no a B
- C no es un candidato porque se ha especificado mi panel como [SearchRoot](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_SearchRoot)
- F no es un candidato porque la [ExclusionRect](/uwp/api/windows.ui.xaml.input.findnextelementoptions#Windows_UI_Xaml_Input_FindNextElementOptions_ExclusionRect) se superpone

![Comportamiento de navegación del foco personalizado con sugerencias de navegación](images/keyboard/navigation-hints.png)

*Comportamiento de navegación del foco personalizado con sugerencias de navegación*

## <a name="navigation-focus-events"></a>Eventos de foco de navegación

### <a name="nofocuscandidatefound-event"></a>Evento NoFocusCandidateFound

El evento [UIElement. NoFocusCandidateFound](/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_NoFocusCandidateFound) se desencadena cuando se presionan las teclas de tabulación o de flecha y no hay ningún candidato de foco en la dirección especificada. Este evento no se desencadena para [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Dado que se trata de un evento enrutado, se propaga desde el elemento enfocado hacia arriba a través de los objetos primarios sucesivos hasta la raíz del árbol de objetos. Esto le permite controlar el evento siempre que sea apropiado.

<a name="split-view-code-sample"></a>

En este caso, se muestra cómo una cuadrícula abre un [SplitView](/uwp/api/windows.ui.xaml.controls.splitview) cuando el usuario intenta cambiar el foco a la izquierda del control que tiene el foco más a la izquierda (consulte [diseño de Xbox y TV](../devices/designing-for-tv.md#navigation-pane)).

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
Los eventos [UIElement. GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) y [UIElement. LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) se desencadenan cuando un elemento recibe el foco o pierde el foco, respectivamente. Este evento no se desencadena para [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_).

Dado que se trata de eventos enrutados, se propagan desde el elemento enfocado hasta los objetos primarios sucesivos hasta la raíz del árbol de objetos. Esto le permite controlar el evento siempre que sea apropiado.

### <a name="gettingfocus-and-losingfocus-events"></a>Eventos GettingFocus y LosingFocus

Los [eventos UIElement. GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y [UIElement. LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) se activan antes que los eventos [UIElement. GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) y [UIElement. LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) respectivos. 

Dado que se trata de eventos enrutados, se propagan desde el elemento enfocado hasta los objetos primarios sucesivos hasta la raíz del árbol de objetos. Como sucede antes de que se produzca un cambio de foco, puede redirigir o cancelar el cambio de foco.

[GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) son eventos sincrónicos, por lo que el foco no se mueve mientras se propaguen estos eventos. Sin embargo, [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) y [LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus) son eventos asincrónicos, lo que significa que no hay ninguna garantía de que el foco no se mueva de nuevo antes de que se ejecute el controlador.

Si el foco se mueve a través de una llamada a [control. Focus](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_), [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) se genera durante la llamada, mientras que [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus) se genera después de la llamada.

El destino de navegación de foco se puede cambiar durante los eventos [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) (antes de que el foco se mueva) a través de la propiedad [GettingFocusEventArgs. NewFocusedElement](/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_NewFocusedElement) . Incluso si se cambia el destino, el evento todavía se propaga y se puede cambiar de nuevo el destino.

Para evitar problemas de reentrada, se produce una excepción si intenta cambiar el foco (mediante [TryMoveFocus](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_TryMoveFocus_Windows_UI_Xaml_Input_FocusNavigationDirection_) o [control. Focus](/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Focus_Windows_UI_Xaml_FocusState_)) mientras se ejecutan estos eventos.

Estos eventos se desencadenan independientemente de la razón por la que se mueve el foco (incluida la navegación por tabulaciones, la navegación direccional y la navegación mediante programación).

Este es el orden de ejecución de los eventos de foco:

1.  [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) Si el foco se restablece de nuevo al elemento que pierde el foco o [TryCancel](/uwp/api/windows.ui.xaml.input.losingfocuseventargs#Windows_UI_Xaml_Input_LosingFocusEventArgs_TryCancel) se realiza correctamente, no se desencadena ningún evento.
2.  [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) Si el foco se restablece de nuevo al elemento que pierde el foco o [TryCancel](/uwp/api/windows.ui.xaml.input.gettingfocuseventargs#Windows_UI_Xaml_Input_GettingFocusEventArgs_TryCancel) se realiza correctamente, no se desencadena ningún evento.
3.  [LostFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LostFocus)
4.  [GotFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GotFocus)

En la imagen siguiente se muestra cómo, cuando se mueve a la derecha desde un, el XYFocus elige B4 como candidato. A continuación, B4 desencadena el evento GettingFocus donde ListView tiene la oportunidad de volver a asignar el foco a B3.

![Cambiar el destino de navegación del foco en el evento GettingFocus](images/keyboard/focus-events.png)

*Cambiar el destino de navegación del foco en el evento GettingFocus*

Aquí se muestra cómo controlar el evento [GettingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_GettingFocus) y redirigir el foco.

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

Aquí se muestra cómo controlar el evento [LosingFocus](/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_LosingFocus) para un control [CommandBar](/uwp/api/windows.ui.xaml.controls.commandbar) y establecer el foco cuando se cierra el menú.

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

## <a name="find-the-first-and-last-focusable-element"></a>Buscar el primer y último elemento enfocable

Los métodos [FocusManager. FindFirstFocusableElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindFirstFocusableElement_Windows_UI_Xaml_DependencyObject_) y [FocusManager. FindLastFocusableElement](/uwp/api/windows.ui.xaml.input.focusmanager#Windows_UI_Xaml_Input_FocusManager_FindLastFocusableElement_Windows_UI_Xaml_DependencyObject_) mueven el foco al primer o último elemento enfocable dentro del ámbito de un objeto (el árbol de elementos de un elemento [UIElement](/uwp/api/windows.ui.xaml.uielement) o el árbol de texto de un elemento [TextElement](/uwp/api/windows.ui.xaml.documents.textelement)). El ámbito se especifica en la llamada (si el argumento es null, el ámbito es la ventana actual).

Si no se puede identificar ningún candidato de foco en el ámbito, se devuelve NULL.

Aquí se muestra cómo especificar que los botones de un CommandBar tienen un comportamiento direccional de encapsulado (consulte [interacciones de teclado](keyboard-interactions.md#popup-ui)).

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

- [Navegación centrada en el teclado, el controlador para juegos, el control remoto y las herramientas de accesibilidad](focus-navigation.md)
- [Interacciones de teclado](keyboard-interactions.md)
- [Accesibilidad de teclado](../accessibility/keyboard-accessibility.md)
