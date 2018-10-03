---
author: QuinnRadich
Description: A button gives the user a way to trigger an immediate action.
title: Botones
label: Buttons
template: detail.hbs
ms.author: quradic
ms.date: 10/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: badaefc406daa5f4500c76262d916f47d82e7a52
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "4263508"
---
# <a name="buttons"></a>Botones

Un botón ofrece al usuario una forma de desencadenar una acción inmediata. Algunos botones están especializados para tareas particulares, como la navegación, acciones que se repiten o presentar los menús.

![Ejemplo de botones](images/controls/button.png)

El marco XAML proporciona un control button estándar, así como varios controles de botón especializados.

Control | Descripción
------- | -----------
[Botón](/uwp/api/windows.ui.xaml.controls.button) | Inicia una acción inmediata. Puede usarse con un enlace de comandos o el evento Click.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Un botón que genera un evento de clic continuamente mientras presionado.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | Un botón que haya un estilo como un hipervínculo, que se usan para la navegación. Para obtener más información, consulta [Hipervínculos](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | (Versión preliminar) Un botón con comillas angulares para abrir un control flotante adjunto.
[Botón de división](/uwp/api/windows.ui.xaml.controls.splitbutton) | (Versión preliminar) Un botón con dos caras. Uno de los lados inicia una acción y el otro lado, abre un menú.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | (Versión preliminar) Un botón de alternancia con dos caras. Activa o desactiva uno de los lados activar/desactivar y el otro lado, abre un menú.

| **Obtén la biblioteca de la interfaz de usuario de Windows** |
| - |
| DropDownButton, botón de división y ToggleSplitButton se incluye como parte de la biblioteca de la interfaz de usuario de Windows, un paquete de NuGet que contiene los nuevos controles y funciones de la interfaz de usuario para aplicaciones para UWP. Para obtener más información, incluidas las instrucciones de instalación, vea la [información general de la biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de la plataforma** | **API de la biblioteca de la interfaz de usuario de Windows** |
| - | - |
| [Evento click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), [propiedad de comando](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Clase DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton), la [clase de botón de división](/uwp/api/microsoft.ui.xaml.controls.splitbutton), [clase ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usar un **botón** para permitir al usuario iniciar una acción inmediata, como enviar un formulario.

No uses un botón cuando la acción sea navegar a otra página; usa un [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) en su lugar. Para obtener más información, consulta [Hipervínculos](hyperlinks.md).
> Excepción: para la navegación por asistentes, usa los botones llamados "Atrás" y "Siguiente". Para otros tipos de exploración hacia atrás o a un nivel superior, usa un [botón Atrás](../basics/navigation-history-and-backwards-navigation.md).

Usa un **RepeatButton** cuando el usuario quizá quiera desencadenar una acción repetidamente. Por ejemplo, usa un RepeatButton para aumentar o disminuir un valor en un contador.

Usa un **DropDownButton** cuando el botón tiene un control flotante que contiene más opciones. Las comillas angulares de forma predeterminada, proporcionan una indicación visual de que el botón incluye un control flotante.

Usa un **botón de división** cuando quieres que el usuario pueda iniciar una acción inmediata o elegir entre las opciones adicionales de forma independiente.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Button">abrir la aplicación y ver el botón en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
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
<td> <b>Tienes que arreglar:</b><br> Botones con texto que se desborda. </td>
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

## <a name="create-a-drop-down-button"></a>Crear un botón de lista desplegable

> **Vista previa**: DropDownButton requiere la [última compilación de Windows 10 Insider Preview y SDK](https://insider.windows.com/for-developers/) o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) es un botón que muestra comillas angulares como un indicador visual que tiene un control flotante adjunto que contiene más opciones. Tiene el mismo comportamiento que un botón estándar con un control flotante; solo el aspecto es diferente.

El botón de lista desplegable hereda el evento Click, pero normalmente no se usa. En su lugar, usa la propiedad de control flotante para asociar un control flotante e invocar acciones con opciones de menú en el control flotante. El control flotante se abrirá automáticamente cuando se hace clic en el botón.

> [!TIP]
> Para obtener más información sobre los controles flotantes, consulta [los menús y menús contextuales](menus.md).

### <a name="example---drop-down-button"></a>Por ejemplo, botón de lista desplegable

Este ejemplo muestra cómo crear un botón de lista desplegable con un control flotante que contiene los comandos para la alineación de párrafo en un RichEditBox. (Para obtener más información y código, consulta [cuadro de texto enriquecido](rich-edit-box.md)).

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

## <a name="create-a-split-button"></a>Crear un botón en dos paneles

> **Vista previa**: botón de división requiere la [última compilación de Windows 10 Insider Preview y SDK](https://insider.windows.com/for-developers/) o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [botón de división](/uwp/api/windows.ui.xaml.controls.splitbutton) tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón estándar e invoca una acción inmediata. La otra parte, invoca un control flotante que contiene las opciones adicionales que el usuario puede elegir.

> [!NOTE]
> Cuando se invoca con la entrada táctil, el botón de división se comporta como un botón de lista desplegable; dos mitades del botón invocan el control flotante. Con otros métodos de entrada, un usuario puede invocar cualquier mitad del botón por separado.

Es el comportamiento típico de un botón de división:

- Cuando el usuario hace clic en la parte de botón, controlar el evento Click para invocar la opción seleccionada actualmente en la lista desplegable.
- Cuando se abre el menú desplegable, invocación de identificador de los elementos de la lista desplegable para ambos cambio qué opción está activada y, a continuación, invocarlo. Es importante invocar el elemento de control flotante porque el botón no se producirá evento Click cuando con la entrada táctil.

> [!TIP]
> Existen muchas formas de colocar elementos en la lista desplegable hacia abajo y controlar su invocación. Si usas un control ListView o GridView, es una manera de controlar el evento SelectionChanged. Si haces esto, establece [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) en **false**. Esto permite a los usuarios navegar por las opciones con un teclado sin invocar el elemento en cada cambio.

### <a name="example---split-button"></a>Por ejemplo, el botón de en dos paneles

Este ejemplo muestra cómo crear un botón de división que se usa para cambiar el color de primer plano del texto seleccionado en un RichEditBox. (Para obtener más información y código, consulta [cuadro de texto enriquecido](rich-edit-box.md)).

![Un botón en dos paneles para seleccionar el color de primer plano](images/split-button-rtb.png)

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

## <a name="create-a-toggle-split-button"></a>Crear un botón de alternancia en dos paneles

> **Vista previa**: ToggleSplitButton requiere la [última compilación de Windows 10 Insider Preview y SDK](https://insider.windows.com/for-developers/) o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Un [ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) tiene dos partes que se pueden invocar por separado. Una parte se comporta como un botón de alternancia que puede estar activada o desactivada. La otra parte, invoca un control flotante que contiene las opciones adicionales que el usuario puede elegir.

Por lo general, se usa un botón de alternancia en dos paneles para habilitar o deshabilitar una característica cuando la característica tiene varias opciones que el usuario puede elegir. Por ejemplo, en un editor de documento, se podría usar para activar las listas o desactivar, mientras se usa la lista desplegable para elegir el estilo de la lista.

> [!NOTE]
> Cuando se invoca con la entrada táctil, el botón de división se comporta como un botón de lista desplegable. Con otros métodos de entrada, un usuario puede invocar cualquier mitad del botón por separado. Con la entrada táctil, dos mitades del botón invocan el control flotante. Por lo tanto, debes incluir una opción en el contenido del control flotante para activar o desactivar el botón activado o desactivado.

### <a name="differences-with-togglebutton"></a>Diferencias con ToggleButton

A diferencia de [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton), ToggleSplitButton no tiene un estado indeterminado. Como resultado, debes tener en cuenta estas diferencias:

- ToggleSplitButton no tiene una propiedad **IsThreeState definida como** o un evento **indeterminado** .
- La propiedad [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) es simplemente un **bool**, no un **bool acepta valores NULL**.
- ToggleSplitButton tiene solo el evento [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) ; no tiene eventos **Checked** y **Unchecked** separados.

### <a name="example---toggle-split-button"></a>Por ejemplo, botón de alternancia de división

El siguiente ejemplo muestra cómo se pueden usar para activar o desactivar el formato de lista un botón de división de botón de alternancia y cambiar el estilo de la lista, en un RichEditBox. (Para obtener más información y código, consulta [cuadro de texto enriquecido](rich-edit-box.md)).

![Un botón de alternancia en dos paneles para seleccionar los estilos de lista](images/toggle-split-button-open.png)

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

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Botones de radio](radio-button.md)
- [Casillas](checkbox.md)
- [Modificadores para alternar](toggles.md)
