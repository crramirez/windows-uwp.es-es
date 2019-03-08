---
Description: Un botón ofrece al usuario una forma de desencadenar una acción inmediata.
title: Botones
label: Buttons
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f585d278d9420865c895d4e20fa1730196d9f0cd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593030"
---
# <a name="buttons"></a>Botones

Un botón ofrece al usuario una forma de desencadenar una acción inmediata. Algunos de los botones están especializadas en determinadas tareas, como navegación, acciones que se repiten o presentar los menús.

![Ejemplo de botones](images/controls/button.png)

El marco XAML proporciona un control de botón estándar, así como varios controles de botón especializado.

Control | Descripción
------- | -----------
[Button](/uwp/api/windows.ui.xaml.controls.button) | Inicia una acción inmediata. Puede utilizarse con un enlace de comando o evento de clic.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Un botón que provoca un evento de clic continuamente mientras se presiona.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Un botón que haya un estilo como un hipervínculo, usado para la navegación. Para obtener más información, consulta [Hipervínculos](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | Botón con un botón de contenido adicional para abrir un ventana flotante adjunto.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | Botón con los dos lados. Uno de los lados inicia una acción y el otro lado, abre un menú.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | Un botón de alternancia con dos lados. Alterna un lado activado/desactivado y el otro lado, abre un menú.

| **Obtención de la biblioteca de interfaz de usuario de Windows** |
| - |
| DropDownButton SplitButton y ToggleSplitButton se incluyen como parte de la biblioteca de interfaz de usuario de Windows, un paquete de NuGet que contiene los nuevos controles y características de interfaz de usuario para aplicaciones UWP. Para obtener más información, incluidas las instrucciones de instalación, consulte el [Introducción a la biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de plataforma** | **API de biblioteca de interfaz de usuario de Windows** |
| - | - |
| [Haga clic en el evento](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), [propiedad comando](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Clase DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton), [SplitButton clase](/uwp/api/microsoft.ui.xaml.controls.splitbutton), [ToggleSplitButton clase](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Use un **botón** para permitir al usuario iniciar una acción inmediata, como enviar un formulario.

No use un botón cuando la acción es navegar hasta otra página; usar un [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) en su lugar. Para obtener más información, consulta [Hipervínculos](hyperlinks.md).
> Excepción: Para la navegación del asistente, use los botones con la etiqueta "Atrás" y "Siguiente". Para otros tipos de hacia atrás exploración o navegación a un nivel superior, use un [botón Atrás](../basics/navigation-history-and-backwards-navigation.md).

Use un **RepeatButton** cuando el usuario podría desear desencadenar una acción varias veces. Por ejemplo, utilice un RepeatButton para incrementar o disminuir un valor en un contador.

Use un **DropDownButton** cuando el botón tiene un control flotante que contiene las opciones más. El contenido adicional de forma predeterminada, proporciona una indicación visual de que el botón incluye un control flotante.

Use un **SplitButton** cuando desee que el usuario sea capaz de iniciar una acción inmediata o elegir opciones adicionales por separado.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Button">abrir la aplicación y ver el botón en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación de galería de controles de XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Este ejemplo usa dos botones, Permitir y Bloquear, en un cuadro de diálogo en el que se solicita acceso a la ubicación.

![Ejemplo de botones que se usan en un cuadro de diálogo](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Crear un botón

Este ejemplo muestra un botón que responde a un clic.

Crear el botón en XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

O crear el botón en el código.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Controlar el evento Click.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>Interacción del botón

Cuando se pulsa un botón con un dedo o lápiz o se presiona el botón izquierdo del mouse mientras el puntero está sobre él, botón genera el evento [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Si un botón tiene el foco del teclado, al presionar la tecla ENTRAR o la barra espaciadora también se genera el evento Click.

Por lo general, no se pueden manipular eventos [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) de bajo nivel en un Button porque tiene el comportamiento Click en su lugar. Para obtener más información, consulta el tema de [introducción a los eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx).

Puedes cambiar la forma en que un botón genera el evento Click cambiando la propiedad [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). El valor predeterminado de ClickMode es **Release**, pero también puedes configurar el ClickMode de un botón como **Hover** o **Press**. Si ClickMode es **Hover**, no se genera el evento Click con el teclado o de forma táctil.


### <a name="button-content"></a>Contenido de Button

Button es una clase [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Su propiedad de contenido XAML es [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), que habilita una sintaxis así para XAML: `<Button>A button's content</Button>`. Puedes establecer cualquier objeto como contenido del botón. Si el contenido es una clase [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), se representa en el botón. Si el contenido es otro tipo de objeto, su representación de cadena se muestra en el botón.

El contenido de un botón suele ser texto. Estas son las recomendaciones de diseño para los botones con contenido de texto:
-   Usa un texto conciso, específico y descriptivo que explique claramente la acción que realiza el botón. Por lo general, el contenido del texto del botón es una única palabra, un verbo.
-   Use la fuente predeterminada, a menos que las directrices de marca le indiquen lo contrario.
-   Para un texto más corto, evita los botones de comando estrechos usando un ancho de botón mínimo de 120 píxeles.
- Para un texto más largo, evita los botones de comando anchos limitando el texto a una longitud máxima de 26 caracteres.
-   Si el contenido del texto del botón es dinámico, (por ejemplo, [localizado](../globalizing/globalizing-portal.md)), decide cómo cambiará de tamaño el botón y qué ocurrirá con los controles que lo rodean.

<table>
<tr>
<td> <b>Debe corregir:</b><br> Botones con texto que se desborda. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Opción 1:</b><br> Aumenta el ancho de botón, los botones de la pila y el ajuste si la longitud del texto es mayor de 26 caracteres. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Opción 2:</b><br> Aumenta la altura del botón y ajusta el texto. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

También puedes personalizar los elementos visuales que conforman la apariencia del botón. Por ejemplo, podrías reemplazar el texto con un icono o usar un icono con texto.

Aquí se ha establecido como contenido de un botón un elemento **StackPanel** que contiene una imagen y texto.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

El botón tendrá el siguiente aspecto:

![Botón con contenido de texto e imagen](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Crear un botón Repetir

Un [RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) es un botón que genera eventos [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) repetidamente desde que se presiona hasta que se suelta. Establece la propiedad [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) para especificar el tiempo que RepeatButton espera después de presionarse antes de que empiece a repetir la acción de clic. Establece la propiedad [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) para especificar el tiempo entre las repeticiones de la acción de clic. Los tiempos de ambas propiedades se especifican en milisegundos.

El siguiente ejemplo muestra dos controles de RepeatButton cuyos respectivos eventos Click se usan para aumentar o disminuir el valor mostrado en un bloque de texto.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="create-a-drop-down-button"></a>Crear un botón desplegable

> DropDownButton requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) es un botón que muestra un botón de contenido adicional como un indicador visual que tiene un control flotante adjunto que contiene las opciones más. Tiene el mismo comportamiento que un botón estándar con un control flotante; solo la apariencia es diferente.

El botón desplegable hereda el evento Click, pero normalmente no se usa. En su lugar, se use la propiedad de control flotante para adjuntar un flotante e invocar acciones mediante las opciones de menú en el control flotante. El control flotante se abre automáticamente cuando se hace clic en el botón.

> [!TIP]
> Para obtener más información sobre los controles flotantes, vea [menús y menús contextuales](menus.md).

### <a name="example---drop-down-button"></a>Ejemplo: botón desplegable

En este ejemplo se muestra cómo crear un botón desplegable con un control flotante que contiene comandos para la alineación del párrafo en un RichEditBox. (Para obtener más información y código, vea [cuadro de edición enriquecida](rich-edit-box.md)).

![Un botón de lista desplegable con los comandos de alineación](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8E4;"/>
    <DropDownButton.Flyout>
        <MenuFlyout Placement="BottomEdgeAlignedLeft">
            <MenuFlyoutItem Text="Left" Icon="AlignLeft" Tag="left"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Center" Icon="AlignCenter" Tag="center"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Right" Icon="AlignRight" Tag="right"
                            Click="AlignmentMenuFlyoutItem_Click"/>
        </MenuFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

```csharp
private void AlignmentMenuFlyoutItem_Click(object sender, RoutedEventArgs e)
{
    var option = ((MenuFlyoutItem)sender).Tag.ToString();

    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the alignment to the selected paragraphs.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (option == "left")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Left;
        }
        else if (option == "center")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Center;
        }
        else if (option == "right")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Right;
        }
    }
}
```

## <a name="create-a-split-button"></a>Crear un botón de expansión

> SplitButton requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón estándar e invoca una acción inmediata. La otra parte, invoca un control flotante que contiene opciones adicionales que puede elegir el usuario.

> [!NOTE]
> Cuando se invoca con toque, el botón de expansión se comporta como un botón desplegable; ambas mitades del botón de invocan la ventana flotante. Con otros métodos de entrada, un usuario puede invocar cualquier mitad del botón por separado.

El comportamiento típico de un botón de expansión es:

- Cuando el usuario hace clic en la parte de botón, controle el evento Click para invocar la opción que está seleccionada actualmente en la lista desplegable.
- Cuando se abre la lista desplegable, identificador de invocación de los elementos de la lista desplegable para ambos cambiar qué opción está seleccionada y, a continuación, invocarlo. Es importante invocar el elemento flotante dado que el botón, haga clic en eventos no se producen al usar toque.

> [!TIP]
> Hay muchas maneras de colocar los elementos en la lista y controlar su invocación. Si usas un ListView o GridView, es una manera de controlar el evento SelectionChanged. Si hace esto, establezca [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) a **false**. Esto permite a los usuarios navegar por las opciones mediante un teclado sin invocar el elemento en cada cambio.

### <a name="example---split-button"></a>Ejemplo: botón de expansión

En este ejemplo se muestra cómo crear un botón de expansión que se usa para cambiar el color de primer plano del texto seleccionado en un RichEditBox. (Para obtener más información y código, vea [cuadro de edición enriquecida](rich-edit-box.md)).

![Un botón de expansión para seleccionar el color de primer plano](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout" Placement="BottomEdgeAlignedLeft">
            <!-- Set SingleSelectionFollowsFocus="False"
                 so that keyboard navigation works correctly. -->
            <GridView ItemsSource="{x:Bind ColorOptions}" 
                      SelectionChanged="BrushSelectionChanged"
                      SingleSelectionFollowsFocus="False"
                      SelectedIndex="0" Padding="0">
                <GridView.ItemTemplate>
                    <DataTemplate>
                        <Rectangle Fill="{Binding}" Width="20" Height="20"/>
                    </DataTemplate>
                </GridView.ItemTemplate>
                <GridView.ItemContainerStyle>
                    <Style TargetType="GridViewItem">
                        <Setter Property="Margin" Value="2"/>
                        <Setter Property="MinWidth" Value="0"/>
                        <Setter Property="MinHeight" Value="0"/>
                    </Style>
                </GridView.ItemContainerStyle>
            </GridView>
        </Flyout>
    </SplitButton.Flyout>
</SplitButton>
```

```csharp
public sealed partial class MainPage : Page
{
    // Color options that are bound to the grid in the split button flyout.
    private List<SolidColorBrush> ColorOptions = new List<SolidColorBrush>();
    private SolidColorBrush CurrentColorBrush = null;

    public MainPage()
    {
        this.InitializeComponent();

        // Add color brushes to the collection.
        ColorOptions.Add(new SolidColorBrush(Colors.Black));
        ColorOptions.Add(new SolidColorBrush(Colors.Red));
        ColorOptions.Add(new SolidColorBrush(Colors.Orange));
        ColorOptions.Add(new SolidColorBrush(Colors.Yellow));
        ColorOptions.Add(new SolidColorBrush(Colors.Green));
        ColorOptions.Add(new SolidColorBrush(Colors.Blue));
        ColorOptions.Add(new SolidColorBrush(Colors.Indigo));
        ColorOptions.Add(new SolidColorBrush(Colors.Violet));
        ColorOptions.Add(new SolidColorBrush(Colors.White));
    }

    private void BrushButtonClick(object sender, object e)
    {
        // When the button part of the split button is clicked,
        // apply the selected color.
        ChangeColor();
    }

    private void BrushSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // When the flyout part of the split button is opened and the user selects
        // an option, set their choice as the current color, apply it, then close the flyout.
        CurrentColorBrush = (SolidColorBrush)e.AddedItems[0];
        SelectedColorBorder.Background = CurrentColorBrush;
        ChangeColor();
        BrushFlyout.Hide();
    }

    private void ChangeColor()
    {
        // Apply the color to the selected text in a RichEditBox.
        Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
        if (selectedText != null)
        {
            Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
            charFormatting.ForegroundColor = CurrentColorBrush.Color;
            selectedText.CharacterFormat = charFormatting;
        }
    }
}
```

## <a name="create-a-toggle-split-button"></a>Crear un botón de alternancia de expansión

> ToggleSplitButton requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o el [biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón de alternancia que se puede activar o desactivar. La otra parte, invoca un control flotante que contiene opciones adicionales que puede elegir el usuario.

Un botón de alternancia de expansión se utiliza normalmente para habilitar o deshabilitar una característica cuando la característica tiene varias opciones que puede elegir el usuario. Por ejemplo, en un editor de documentos, se podría usar para activar las listas de activado o desactivado, mientras se usa la lista desplegable para elegir el estilo de la lista.

> [!NOTE]
> Cuando se invoca con toque, el botón de expansión se comporta como un botón desplegable. Con otros métodos de entrada, un usuario puede invocar cualquier mitad del botón por separado. Con tecnología táctil, ambas mitades del botón invocan la ventana flotante. Por lo tanto, debe incluir una opción en el contenido de la ventana flotante para activar o desactivar el botón Activar o desactivar.

### <a name="differences-with-togglebutton"></a>Diferencias con ToggleButton

A diferencia de [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), ToggleSplitButton no tiene un estado indeterminado. Como resultado, debe tener en cuenta estas diferencias:

- ToggleSplitButton no tiene un **IsThreeState** propiedad o **indeterminado** eventos.
- El [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) propiedad es simplemente un **bool**, no un **bool que acepta valores NULL**.
- ToggleSplitButton sólo tiene la [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) evento; no tiene independiente **Checked** y **Unchecked** eventos.

### <a name="example---toggle-split-button"></a>Ejemplo: alternar el botón de expansión

En el ejemplo siguiente se muestra cómo una botón de expansión de alternancia podría usarse para activar o desactivar el formato de lista y cambiar el estilo de la lista, en un RichEditBox. (Para obtener más información y código, vea [cuadro de edición enriquecida](rich-edit-box.md)).

![Un botón de expansión de alternancia para seleccionar los estilos de lista](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout Placement="BottomEdgeAlignedLeft">
            <ListView x:Name="ListStylesListView"
                      SelectionChanged="ListStylesListView_SelectionChanged" 
                      SingleSelectionFollowsFocus="False">
                <StackPanel Tag="bullet" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7C8;"/>
                    <TextBlock Text="Bullet" Margin="8,0"/>
                </StackPanel>
                <StackPanel Tag="alpha" Orientation="Horizontal">
                    <TextBlock Text="A" FontSize="24" Margin="2,0"/>
                    <TextBlock Text="Alpha" Margin="8"/>
                </StackPanel>
                <StackPanel Tag="numeric" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xF146;"/>
                    <TextBlock Text="Numeric" Margin="8,0"/>
                </StackPanel>
                <TextBlock Tag="none" Text="None" Margin="28,0"/>
            </ListView>
        </Flyout>
    </ToggleSplitButton.Flyout>
</ToggleSplitButton>
```

```csharp
private void ListStyleButton_IsCheckedChanged(ToggleSplitButton sender, ToggleSplitButtonIsCheckedChangedEventArgs args)
{
    // Use the toggle button to turn the selected list style on or off.
    if (((ToggleSplitButton)sender).IsChecked == true)
    {
        // On. Apply the list style selected in the drop down to the selected text.
        var listStyle = ((FrameworkElement)(ListStylesListView.SelectedItem)).Tag.ToString();
        ApplyListStyle(listStyle);
    }
    else
    {
        // Off. Make the selected text not a list,
        // but don't change the list style selected in the drop down.
        ApplyListStyle("none");
    }
}

private void ListStylesListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var listStyle = ((FrameworkElement)(e.AddedItems[0])).Tag.ToString();

    if (ListButton.IsChecked == true)
    {
        // Toggle button is on. Turn it off...
        if (listStyle == "none")
        {
            ListButton.IsChecked = false;
        }
        else
        {
            // or apply the new selection.
            ApplyListStyle(listStyle);
        }
    }
    else
    {
        // Toggle button is off. Turn it on, which will apply the selection
        // in the IsCheckedChanged event handler.
        ListButton.IsChecked = true;
    }
}

private void ApplyListStyle(string listStyle)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the list style to the selected text.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (listStyle == "none")
        {  
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.None;
        }
        else if (listStyle == "bullet")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Bullet;
        }
        else if (listStyle == "numeric")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Arabic;
        }
        else if (listStyle == "alpha")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.UppercaseEnglishLetter;
        }
        selectedText.ParagraphFormat = paragraphFormatting;
    }
}
```

## <a name="recommendations"></a>Recomendaciones

- Asegúrate de que el objetivo y el estado del botón sean claros para el usuario.
- Cuando hay varios botones para la misma decisión (como en un cuadro de diálogo de confirmación), presenta los botones de confirmación en este orden, donde [Hacerlo] y [No hacerlo] son respuestas específicas a la instrucción principal:
    - Aceptar/[Hacerlo]/Sí
    - [No hacerlo]/No
    - Cancelar
- Expón solo uno de los dos botones al usuario a la vez. Por ejemplo, Aceptar y Cancelar. Si necesitas exponer más acciones al usuario, considera la posibilidad de usar [casillas](checkbox.md) o [botones de radio](radio-button.md) con los que el usuario pueda seleccionar acciones, con un único botón de comando para desencadenar estas acciones.
- En el caso de las acciones que deban estar disponibles en varias páginas de la aplicación, en vez de duplicar el mismo botón en varias páginas, piensa en utilizar una [barra de la aplicación inferior](app-bars.md).

### <a name="recommended-single-button-layout"></a>Diseño de botón único recomendado

Si el diseño requiere un único botón, debe estar alineado a la izquierda o a la derecha en función de su contexto de contenedor.

- Los cuadros de diálogo con un único botón deben **alinear a la derecha** el botón. Si el cuadro de diálogo contiene solo un botón, asegúrate de que el botón realizar la acción segura y no destructiva. Si usas [ContentDialog](dialogs.md) y especificas un único botón, se alineará automáticamente a la derecha.

![Botón dentro de un cuadro de diálogo](images/pushbutton_doc_dialog.png)

- Si el botón aparece dentro de una interfaz de usuario de contenedor (por ejemplo, dentro de una notificación del sistema, un control flotante o un elemento de vista de lista), debes **alinear a la derecha** el botón dentro del contenedor.

![Botón dentro de un contenedor](images/pushbutton_doc_container.png)

- En las páginas que contengan un solo botón (por ejemplo, un botón "Aplicar" en la parte inferior de una página de configuración), debes **alinear a la izquierda** el botón. Así se garantiza que el botón se alinea con el resto del contenido de la página.

![Botón de una página](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Botones Atrás

El botón Atrás es un elemento de la interfaz de usuario proporcionado por el sistema que permite la navegación hacia atrás a través de la pila de retroceso o el historial de navegación del usuario. No es necesario que crees tu propio botón Atrás, pero es posible que debas realizar algunas acciones para permitir que la experiencia de navegación hacia atrás resulte adecuada. Para obtener más información, consulta [Historial y navegación hacia atrás](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase de botón](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Botones de radio](radio-button.md)
- [Casillas de verificación](checkbox.md)
- [Modificadores de alternancia](toggles.md)
