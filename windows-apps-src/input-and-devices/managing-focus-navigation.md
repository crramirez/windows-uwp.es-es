---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 8389d1f089d507a087693879a793b95572d7860b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2017
---
# <a name="managing-focus-navigation"></a>Administrar la navegación del foco

Muchas entradas, es decir, el teclado, las herramientas de accesibilidad, como el Narrador de Windows, el controlador para juegos y el control remoto, comparten un mecanismo subyacente común para mover los elementos visuales de foco por la interfaz de usuario de las aplicaciones. Lee más sobre los elementos visuales de foco y la navegación en los documentos [Interacción de teclado](keyboard-interactions.md) y [Diseño para Xbox y televisión](designing-for-tv.md#xy-focus-navigation-and-interaction).

A continuación se ofrece una manera independiente de entrada para que la aplicación mueva el foco por la interfaz de usuario de la aplicación, lo que te permite escribir un único código que haga que tu aplicación funcione a la perfección con varios tipos de entrada.

## <a name="navigation-strategies-properties-to-fine-tune-focus-movements"></a>Propiedades de estrategias de navegación para ajustar los movimientos del foco

Usa las propiedades de estrategias de navegación XYFocus para especificar qué control debe recibir el foco en función de la tecla de dirección que se presiona. Estas propiedades son:

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

Estas propiedades tienen un valor del tipo **XYFocusNavigationStrategy**. Si se establece en **Auto**, el comportamiento del elemento se basa en los antecesores del elemento. Si todos los elementos se establecen en **Auto**, se usa **Projection**.

Estas estrategias de navegación se aplican al teclado, el controlador para juegos y el control remoto.

### <a name="projection"></a>Proyección

La estrategia de proyección mueve el foco al primer elemento encontrado al proyectar el borde del elemento enfocado actualmente en la dirección de navegación.

> [!NOTE]
> Otros factores, como el elemento enfocado anteriormente y la proximidad al eje de la dirección de navegación, pueden influir en el resultado.

![proyección](images/keyboard/projection.png)

*El foco se mueve desde A hasta D en navegación hacia abajo, en función de la proyección de la parte inferior de A*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

La estrategia NavigationDirectionDistance mueve el foco al elemento más cercano al eje de la dirección de navegación.

El borde del rectángulo delimitador correspondiente a la dirección de navegación se extiende y proyecta para identificar los objetivos candidatos. El primer elemento encontrado se identifica como el objetivo. En el caso de varios candidatos, el elemento más próximo se identifica como el objetivo. Si sigue habiendo varios candidatos, el elemento más hacia arriba/más a la izquierda se identifica como el candidato.

![navegación dirección distancia](images/keyboard/navigation-direction-distance.png)

*El foco se mueve de A a C y después de C a B en navegación hacia abajo*


### <a name="rectilineardistance"></a>RectilinearDistance

La estrategia RectilinearDistance mueve el foco al elemento más cercano según la distancia 2D más corta (longitud Manhattan). Esta distancia se calcula sumando la distancia principal y la distancia secundaria de cada posible candidato. En caso de empate, se selecciona el primer elemento a la izquierda si la dirección es hacia arriba o hacia abajo, o se selecciona el primer elemento de la parte superior si la dirección es izquierda o derecha.

En la siguiente imagen, se muestra que cuando A tiene el foco, este se mueve a B porque:

-   Distancia (A, B, abajo) = 10 + 0 = 10
-   Distancia (A, C, abajo) = 30 + 0 = 30
-   Distancia (A, D, abajo) 30 + 0 = 30

![Distancia rectilínea](images/keyboard/rectilinear-distance.png)

*Cuando A tiene el foco, este se mueve a B al usar la estrategia RectilinearDistance*

En la imagen siguiente se muestra cómo se aplica la estrategia de navegación al elemento mismo, no a los elementos secundarios (a diferencia de XYFocusKeyboardNavigation).

Por ejemplo, cuando E tiene el foco, presionar la flecha derecha mueve el foco a F usando la estrategia RectilinearDistance, pero cuando D tiene el foco, presionar la flecha derecha mueve el foco a H usando la estrategia Projection.

Ten en cuenta que no se aplica la estrategia de contenedor principal (distancia de la dirección de navegación). Solo se usa cuando el foco está en H, F o G.

![distintas estrategias de navegación](images/keyboard/different-navigation-strategies.png)

*Distintos comportamientos de foco en función de la estrategia de navegación aplicada*

En el siguiente ejemplo se simula el comportamiento del panel Iconos en el menú Inicio de Windows10. En este caso, presionar las flechas arriba y abajo usa la estrategia NavigationDirectionDistance, y presionar las flechas izquierda y derecha usa la estrategia Projection.

```XAML
<start:TileGridView
        XYFocusKeyboardNavigation ="Default"
        XYFocusUpNavigationStrategy=" NavigationDirectionDistance "
        XYFocusDownNavigationStrategy=" NavigationDirectionDistance "
        XYFocusLeftNavigationStrategy="Projection"
        XYFocusRightNavigationStrategy="Projection"
/>
```

## <a name="move-focus-programmatically"></a>Mover el foco mediante programación

Usa el método [FocusManager.TryMoveFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.TryMoveFocus) o el método [FocusManager.FindNextElement](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Input.FocusManager.FindNextFocusableElement) para mover el foco mediante programación.

TryMoveFocus establece el foco como si se presionara una tecla de navegación.
FindNextElement devuelve el elemento (una clase DependencyObject) al que se movería TryMoveFocus.

**NOTA** Se recomienda usar el método **FindNextElement** en lugar de **FindNextFocusableElement**. FindNextFocusableElement intenta devolver una clase UIElement, pero si el siguiente elemento activable es Hyperlink, devuelve null porque Hyperlink no es una clase UIElement. El método FindNextElement devuelve una clase DependencyObject en su lugar.

### <a name="find-a-focus-candidate-in-a-scope"></a>Buscar un candidato de foco en un ámbito

Tanto FocusManager.FindNextElement como FocusManager.TryMoveFocus te permiten personalizar el comportamiento de búsqueda del siguiente elemento activable. Puedes buscar dentro de un árbol específico de la interfaz de usuario o excluir elementos en un área de navegación específica.

Este ejemplo se toma de una implementación de un juego TicTacToe y demuestra cómo usar los métodos FindNextElement y TryMoveFocus:

```XAML
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
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="0" x:Name="Cell00" />
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="0" x:Name="Cell10"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="0" x:Name="Cell20"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="1" x:Name="Cell01"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="1" x:Name="Cell11"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="1" x:Name="Cell21"/>
            <myControls:TicTacToeCell Grid.Column="0" Grid.Row="2" x:Name="Cell02"/>
            <myControls:TicTacToeCell Grid.Column="1" Grid.Row="2" x:Name="Cell22"/>
            <myControls:TicTacToeCell Grid.Column="2" Grid.Row="2" x:Name="Cell32"/>
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
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Up, options);
                    break;
                case Windows.System.VirtualKey.Down:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Down, options);
                    break;
                case Windows.System.VirtualKey.Left:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Left, options);
                    break;
                case Windows.System.VirtualKey.Right:
                    candidate = FocusManager.FindNextElement(FocusNavigationDirection.Right, options);
                    break;
            }
         //Note you should also consider whether is a Hyperlink, WebView, or TextBlock.
            if (candidate != null && candidate is Control)
            {
                (candidate as Control).Focus(FocusState.Keyboard);
            }
        }
```

**NOTA** FocusNavigationDirection.Left y FocusNavigationDirection.Right son lógicos (cerca y lejos) para admitir escenarios RTL (de derecha a izquierda). (Esto coincide con el resto del código Xaml).

La búsqueda de candidatos de foco puede ajustarse mediante la clase FindNextElementOptions. Este objeto tiene las siguientes propiedades:

-   **SearchRoot** DependencyObject: Para definir el ámbito de la búsqueda de candidatos para los elementos secundarios de esta clase DependencyObject. Un valor nulo indica el árbol visual completo de la aplicación.
-   **ExclusionRect** Rect: Para excluir de la búsqueda los elementos que, cuando se representan, se superponen al menos un píxel con este rectángulo.
-   **HintRect** Rect: Los posibles candidatos se calculan con el elemento enfocado como referencia. Este rectángulo permite a los desarrolladores especificar otra referencia en lugar del elemento enfocado. Este es un rectángulo "ficticio" usado para cálculos únicamente. Nunca se convierte en un rectángulo real ni se agrega al árbol visual.
-   **XYFocusNavigationStrategyOverride** XYFocusNavigationStrategyOverride: La estrategia de navegación que se usa para mover el foco. El tipo es una enumeración con los valores None (que es el valor de cero), Auto, RectilinearDistance, NavigationDirectionDistance y Projection.
    **Importante** **SearchRoot** no se usa para calcular el área representada (geométrica) ni para obtener los candidatos dentro del área. Como consecuencia, si se aplican transformaciones a los descendientes de la clase DependencyObject que los ubican fuera del área direccional, estos elementos aún se consideran candidatos.

La sobrecarga de opciones de FindNextElement no puede usarse con la navegación mediante tabulaciones, solo admite la navegación direccional.

La siguiente imagen ilustra estas opciones.

Por ejemplo, cuando B tiene el foco, llamar a FindNextElement (con varias opciones establecidas para indicar que el foco se mueve a la derecha) establece el foco en I. Las razones para esto son:

1.  La referencia no es B, es A debido a la propiedad HintRect
2.  C se excluye porque el candidato posible debe estar en MyPanel (la propiedad SearchRoot)
3.  D se excluye porque se superpone con el rectángulo de exclusiones

![sugerencias de navegación](images/keyboard/navigation-hints.png)

*Comportamiento de foco basado en sugerencias de navegación*

### <a name="no-focus-candidate-available"></a>No hay candidatos de foco disponibles

El evento UIElement.NoFocusCandidateFound se desencadena cuando el usuario intenta mover el foco con las teclas Tab o de dirección, pero no hay ningún candidato posible en la dirección especificada. Este evento no se desencadena para FocusManager.TryMoveFocus().

Dado que este es un evento enrutado, se propaga del elemento enfocado a través de los objetos primarios sucesivos a la raíz del árbol de objetos. De este modo, puedes controlar el evento siempre que sea apropiado.

<a name="split-view-code-sample"></a> En el siguiente ejemplo se muestra una clase Grid que abre una vista dividida cuando el usuario intenta mover el foco a la izquierda del control activable más a la izquierda, como se describe en la documentación de [Diseño para televisión y Xbox](designing-for-tv.md#navigation-pane).

```XAML
<Grid NoFocusCandidateFound="OnNoFocusCandidateFound">  ...</Grid>
```

```csharp
private void OnNoFocusCandidateFound (UIElement sender, NoFocusCandidateFoundEventArgs args)
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

## <a name="focus-events"></a>Eventos de foco

Los eventos [UIElement.GotFocus](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.GotFocus) y UIElement.LostFocus se desencadenan cuando un control obtiene el foco o pierde el foco, respectivamente. Este evento no se desencadena para FocusManager.TryMoveFocus().

Dado que este es un evento enrutado, se propaga del elemento enfocado a través de los objetos primarios sucesivos a la raíz del árbol de objetos. De este modo, puedes controlar el evento siempre que sea apropiado.

Los eventos **UIElement.GettingFocus** y **UIElement.LosingFocus** también se propagan desde el control, pero *antes* de que se produzca el cambio de foco. Esto da a la aplicación una oportunidad para redirigir o cancelar el cambio de foco.

Ten en cuenta que los eventos GettingFocus y LosingFocus son sincrónicos, mientras que los eventos GotFocus y LostFocus son asincrónicos. Por ejemplo, si el foco se mueve porque la aplicación llama al método Control.Focus(), GettingFocus se genera durante la llamada, pero GotFocus se genera en algún momento después de la llamada.

UIElements puede enlazarse a los eventos GettingFocus y LosingFocus y cambiar el destino (con la propiedad NewFocusedElement) antes de que se mueva el foco. Incluso si ha cambiado el destino, el evento aún se propaga y otro elemento primario puede cambiar el destino de nuevo.

Para evitar problemas de reentrada, se lanzan excepciones si se intenta mover el foco (con FocusManager.TryMoveFocus o Control.Focus) mientras se propagan estos eventos.

Estos eventos se desencadenan independientemente del motivo para el desplazamiento del foco (por ejemplo, navegación mediante tabulación, navegación XYFocus o mediante programación).

La siguiente imagen muestra cómo, al pasar de A a la derecha, XYFocus elige LVI4 como candidato. Luego LVI4 desencadena el evento GettingFocus, donde ListView tiene la oportunidad de volver a asignar el foco a LVI3.

![eventos de foco](images/keyboard/focus-events.png)

*Eventos de foco*

En este ejemplo de código se muestra cómo controlar el evento OnGettingFocus y redirigir el foco basándose en dónde estaba establecido el foco anteriormente.

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
            if (MyListView.SelectedIndex != -1 && IsNotAChildOf(MyListView, args.OldFocusedElement))
            {
                var selectedContainer = MyListView.ContainerFromItem(MyListView.SelectedItem);
                if (args.FocusState == FocusState.Keyboard && args.NewFocusedElement != selectedContainer)
                {
                    args.NewFocusedElement = MyListView.ContainerFromItem(MyListView.SelectedItem);
                    args.Handled = true;
                }
            }        
  }
```

En este ejemplo de código se muestra cómo controlar el evento OnLosingFocus de un objeto CommandBar y esperar para establecer el foco hasta que se cierre el menú DropDown.

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
          if (MyCommandBar.IsOpen == true && IsNotAChildOf(MyCommandBar, args.NewFocusedElement))
          {
              args.Cancel = true;
              args.Handled = true;
          }
}
```

### <a name="losingfocus-and-gettingfocus-event-order"></a>Orden de los eventos LosingFocus y GettingFocus

LosingFocus y GettingFocus son eventos sincrónicos, por lo que el foco no se moverá mientras se propaguen estos eventos. Sin embargo, dado que LostFocus y GotFocus eventos asincrónicos, no hay ninguna garantía de que el foco no se volverá a mover antes de que se ejecute el controlador. La única garantía es que el controlador de LostFocus se ejecuta antes del controlador de GotFocus.

Este es orden de ejecución:

1.  Sync LosingFocus: Si LosingFocus restablece el foco nuevamente en el elemento que perdió el foco o se establece Cancel=true, no hay eventos adicionales.
2.  Sync GettingFocus: Si GettingFocus restablece el foco nuevamente en el elemento que perdió el foco o se establece Cancel=true, no hay eventos adicionales.
3.  Async LostFocus
4.  Async GotFocus

## <a name="find-the-first-and-last-focusable-element-a-namefindfirstfocusableelement"></a>Buscar el primer y el último elemento activable <a name="findfirstfocusableelement">

Los métodos **FocusManager.FindFirstFocusableElement** y **FocusManager.FindLastFocusableElement** te permiten mover el foco al primer o último elemento activable dentro del ámbito de un objeto (el árbol de elementos de UIElement o el árbol de texto de TextElement). Especificas el ámbito cuando llamas a estos métodos. Si el argumento es null, el ámbito será la ventana actual.

**Nota** Estos métodos pueden devolver null si no hay ningún elemento activable en el ámbito.

<a name="popup-ui-code-sample"></a> En el siguiente ejemplo de código se muestra cómo especificar que los botones de un control CommandBar tienen comportamiento direccional de ajuste automático que se describe en el artículo [Interacciones de teclado](keyboard-interactions.md#popup-ui).

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
